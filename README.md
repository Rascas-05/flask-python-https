# flask-python-https
HTTPS &amp; Certificates in Flask Web Applications - NeuralNime - Jan 20 2024 https://www.youtube.com/watch?v=P5mdDTMmfL0 Today we learn how to enable HTTPS and use certificates in Flask. with my build notes using VSCode

================================================================================
The following is a copy of my notes as I built the project on Windows 10 using VSCode and Chrome browser. A beginner with knowledge of Windows and VSCOde should be able to follow my notes and successfully complete this project. 

Python version: Python 3.12.6 (tags/v3.12.6:a4a2d2b, Sep  6 2024, 20:11:23) [MSC v.1940 64 bit (AMD64)] on win32

Windows->Settings->System->About:
Dell Inspiron 5675
Device name	DESKTOP-<deleted>
Processor	AMD Ryzen 7 1700X Eight-Core Processor            3.40 GHz
Installed RAM	32.0 GB (31.9 GB usable)
Device ID	7B071060-A46F-4C6E-B3A2-ED078562432E
Product ID	00325-96183-71089-AAOEM
System type	64-bit operating system, x64-based processor
Pen and touch	No pen or touch input is available for this display


GitHub: https://github.com/NeuralNine 

Create Github repo flask-python-https

VSCode Create .venv

pip install flask

---app.py---
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
	return "<h1>Hello Flask - Python using https!</h1>"

if __name__  == "__main__":
   app.run(debug=True)

------
PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https> cd .venv\scripts
PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https\.venv\scripts> ./activate
(.venv) PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https\.venv\scripts> cd..
(.venv) PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https\.venv> cd..
(.venv) PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https> pip list
Package Version
------- -------
pip     24.3.1 
(.venv) PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https> 
(.venv) PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https> pip list
Package      Version
------------ -------
blinker      1.9.0
click        8.1.7
colorama     0.4.6
Flask        3.1.0
itsdangerous 2.2.0
Jinja2       3.1.4
MarkupSafe   3.0.2
pip          24.3.1
Werkzeug     3.1.3
(.venv) PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https> 
-----------
Chrome browser
http://127.0.0.1:5000/
Hello Flask - Python using https!

try https://127.0.0.1:5000/
This site can’t provide a secure connection
127.0.0.1 sent an invalid response.
Try running Windows Network Diagnostics.
ERR_SSL_PROTOCOL_ERROR
---Terminal---
127.0.0.1 - - [06/Dec/2024 14:37:38] code 400, message Bad request version ('¤\x1aWhÆ')
----
If you don't have a certificate:    app.run(ssl_context='adhoc')
---app.py---
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
	return "<h1>Hello Flask - Python using https!</h1>"

if __name__  == "__main__":
   #app.run(debug=True)
   app.run(ssl_context='adhoc')
---Terminal---
TypeError: Using ad-hoc certificates requires the cryptography library.
-------------
====================Install openssl==========================
Install openssl on Windows 10 64-bit
Shining light productions Win64 OpenSSL

How to install OpenSSL in Windows 10-11 - Check SSL port Status
https://www.youtube.com/watch?v=PmCmVUGaK4Q

OpenSSL is an open-source software library that provides a secure communication layer over the internet using SSL/TLS protocols. It is commonly used to implement secure connections for web servers, email servers, VPNs, and other network services.
I downloaded "Win64OpenSSL-3_3_2" (220,080kb)

Windows search "edit system environment variables"
Add to User path var "C:\Program Files\OpenSSL-Win64\bin"
Restart Windows.

C:\Users\bfvdi>openssl version
OpenSSL 3.3.2 3 Sep 2024 (Library: OpenSSL 3.3.2 3 Sep 2024)

C:\Users\bfvdi>openssl s_client -connect <Windows Device name>:<port number>
Check Windows->settings->System->About-Device name

C:\Users\bfvdi>openssl s_client -connect  <Windows Device name>:80
C:\Users\bfvdi>openssl s_client -connect  <Windows Device name>:80
E8540000:error:8000274D:system library:BIO_connect:Unknown error:crypto\bio\bio_sock2.c:178:calling connect()
E8540000:error:10000067:BIO routines:BIO_connect:connect error:crypto\bio\bio_sock2.c:180:
E8540000:error:8000274D:system library:BIO_connect:Unknown error:crypto\bio\bio_sock2.c:178:calling connect()
E8540000:error:10000067:BIO routines:BIO_connect:connect error:crypto\bio\bio_sock2.c:180:
connect:errno=0

C:\Users\bfvdi>netstat -ano | findstr :135
C:\Users\bfvdi>netstat -ano | findstr :135
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       1320
  TCP    [::]:135               [::]:0                 LISTENING       1320

Try? openssl s_client -connect  <Windows Device name>:1320

https://monovm.com/blog/install-openssl-on-windows/
Some checks:
Path=C:\Program Files\OpenSSL-Win64\bin - okay
Microsoft Visual C++ Redistributable 2015-2022 Redistributable (is installed) - okay 

5. SSL Certificate Issues
Problems with SSL certificates can occur if they are not generated or installed correctly. Here are some common issues and their fixes:

I need to regenerate the certificate cert.pem and key.pem
Read README.md Steps 4 and 5
C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Projects\NodeJS\ssl_certificate\ssl-secured\cert

Certificate Not Trusted: Ensure that your certificate is issued by a trusted Certificate Authority (CA). Self-signed certificates may require additional steps to be trusted.
Incorrect Paths: Verify that the paths to your certificate and key files are correct in your configuration.
Expired Certificates: Check the expiration date of your SSL certificate and renew it if necessary.
Intermediate Certificates: Make sure to include any necessary intermediate certificates in your certificate chain.

==================End Install openssl==================
PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https> openssl -> Prints openssl help menu

-------Back to NeuralNine------
Create our own certificate using openssl 
PS openssl req -x509 -newkey rsa:4896 -nodes -out cert.pem -keyout key.pem -days 365
PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https> openssl req -x509 -newkey rsa:4896 -nodes -out cert.pem -keyout key.pem -days 365
..+.........+...........+.+++++++++++++++++++++++++++++++++++++++++++++*..+..+...+.+...+...+.....+.........+....+.....+......+...+.+++++++++++++++++++++++++++++++++++++++++++++*....+....+............+..+..................+...+.......+........+....+..+...+................+.....+.+.....+..............................+.+..............+++++
.....+.....+....+.....+............+.........+....+.....+......+..........+.........+..............+...+....+........+......+.+.....+.+.........+...+........+....+...+++++++++++++++++++++++++++++++++++++++++++++*..+.........+........+...+++++++++++++++++++++++++++++++++++++++++++++*.............+.....+.......+...............+..............+.+........+....+.....+.......+.....................+.........+...+.....+.+......+........+.......+..............+.+.....+...................+...............+......+.........+...........+...+.........................+........+......................+.....+.+..+....+...........+.......+.....+...+....+...+.....+..................+...+............+...+.+...........+...+.........+..........+................................................+..+......+.+..................+...+.....+...................+...+....................+..........+..+..............................+...+......+..........+..+..........+..+................+...............+.....+....+...........+...................+.....+.......+......+...+.....................+...+..+...+......................+...+......+......+.....+................+..+...+....+.........+.........+..+.................................+...+.........+.+..+.......+..............+...................+..+.............+..+.....................+............................+............+........+....+...+.....+........................+...............+......+.............+.....+.......+..+.+......+.........+.....+......+................+...+..+...+.........+...+....+.....+.........+...............+.......+..............+.+.......................+...................+.....+.....................+.........+.......+.....+.+.............................+.+..............+.+....................+....+..............+.......+...+......+......+.................+.+...........+.+........+.+......+........................+...+......+++++
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:AU
State or Province Name (full name) [Some-State]:Victoria
Locality Name (eg, city) []:Melbourne
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Widget factory Ltd
Organizational Unit Name (eg, section) []:Sausage dept.
Common Name (e.g. server FQDN or YOUR name) []:localhost
Email Address []:sausage@widgetfactory.com
PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https> 
-----------------------------------
Command Prompt =>
cd C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https>tree /f /a > tree.txt

---app.py---
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
	return "<h1>Hello Flask - Python using https!</h1>"

if __name__  == "__main__":
   #app.run(debug=True)
   app.run(ssl_context=('cert.pem', 'key.pem'))
----Terminal---
PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https> ^C
s/.venv/Scripts/python.exe c:/Users/bfvdi/Documents/AppDevelopment/VSCODE/Flask/flask-python-https/app.py
 * Serving Flask app 'app'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on https://127.0.0.1:5000
Press CTRL+C to quit
------Chrome Browser--------
Your connection is not private
Attackers might be trying to steal your information from 127.0.0.1 (for example, passwords, messages or credit cards). Learn more about this warning
net::ERR_CERT_AUTHORITY_INVALID

Accept risk and Click Adbvanced 

https://127.0.0.1:5000/
Hello Flask - Python using https!

Copy of certicate in files.
----Terminal---
Press CTRL+C to quit
127.0.0.1 - - [06/Dec/2024 16:16:05] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [06/Dec/2024 16:16:05] "GET /favicon.ico HTTP/1.1" 404 -
PS C:\Users\bfvdi\Documents\AppDevelopment\VSCODE\Flask\flask-python-https> 
