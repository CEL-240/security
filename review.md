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











## steps 
to exploit a webpage: 
  nmap --script=http-enum
  after nmap you type in the directorie found at the search bar
  then you replace the ssh keys with your own so you dont need a password to get on the box and make sure to create the authorized_keys 
  directory 







  
