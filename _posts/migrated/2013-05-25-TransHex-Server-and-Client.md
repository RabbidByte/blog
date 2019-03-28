---
layout: splash
title: "TransHex Server and Client"
date: 2013-05-25 00:00:00 -0700
categories: Blog Tools
permalink: /:title.html
---
## Summary

We have all faced the problem of transferring “malicious” or binary files through corporate firewalls or proxies when doing penetration tests.  Previously the work around was quite easy.  FTP, SSL, and SSH tunneling (the list goes on) provided a quick and easy way to bring whatever tools a penetration tester needed into the environment.  Edge technologies have now matured and these simple techniques are no longer working mainly due to next generation firewalls with application ID.  So how do you get around this?  Go old school to beat new school tech!

The general idea we had before writing the very rough first release of TransHex was AV does not scan hex in text files.  So how to beat the application ID portion of the nexgen firewalls?  Easy, you don’t.  We initially wrote the program completely in python as a client server app and we were quickly blocked by nexgen firewalls.  So if you can’t beat them, join them.  The app was ported to a web server and bingo bango we were transferring files through multiple nexgen firewalls without being detected.

So here is a quick breakdown on how TransHex works.

1. The attacker/pentester goes to the web app that would be loaded on a public server
2. The attacker/pentester requests an exe that is available on a different publicly available server and submits to the application
3. TransHex at this point downloads the exe and saves it to [web path]/hex/[filename]
4. Once the download is complete a python script is ran that converts the binary file to a single hex string
5. The hex string is written to a new text file in [web path]/hex/[filename]
6. The attacker/pentester then goes back to the main page of the application by clicking continue
7. The new text file is now listed on the first page
8. The attacker/pentester then saves the text file to the local machine using the internet browser
9. On the local machine the attacker/pentester converts the text file with the hex string to a binary using the python client script

## Application Notes

This application is provided as is.  NO support is offered for this application and only you are liable for any damage this application may cause.

### Features that need to be added

1. Error handling
2. Help/man pages
3. Request hex conversion of local (server side) executables
4. Hex string obfuscation

### Limitations and Testing

1. Max file size tested with 115KB
2. File types tested Zip and exe, windows files and known malware
3. Tested through multiple nexgen firewalls and proxies
4. Must have python loaded on the client machine or
5. Transfer the client as an exe to the client by other means
6. Tested on Kali Linux (3.7-trunk-686-pae #1 AMP Debian 3.7.0.2-0+kali5 i686 GNU/Linux)
7. The server application is NOT meant to be secure!  You must wrap your own security around the application

## Installation (on Fresh install of Kali Linux)

### SERVER SIDE

1. Download the TransHex package from here (MD5:A38EB881A4708FD474D858680648E611 SHA1:10E0066CEF1B95CF77498347D21DFCBB15CA7D5F)
2. Extract the files from the package
3. Delete /var/www/index.html
4. Copy contents of the Server folder from the package to /var/www/
5. Ensure permissions are correct on the files (again this is NOT a secure app)
```
chmod 755 TransHex_BIN_to_TXT.py
chmod 644 index.php
chmod 644 getfile.php
chmod 777 hex
service apache2 start
```

6. Go to http://127.0.0.1 and the page should now be live

### CLIENT SIDE

1. Somehow get the file TransHex_TXT_to_BIN.py to a client on the inside of the network (needs python installed)
2. OR convert the file TransHex_TXT_to_BIN.py to a PE and get it to a client on the inside of the network

## Usage

1. Locate the URL of the exe that you wish to download (also works with archive files)
2. From the client on the inside of the network open a browser and go to your server, hopefully the URL isn’t blocked
3. Enter the URL of the exe into the text field and click “Go Get It!”
4. Wait for the page to load.  There is no progress bar and with large files or slow connections PHP settings may have to be changed on the server
5. Once the page loads click “continue …”
6. You will now see your text file in the list
7. Right click the hyperlink of the text file and select “Save Link As…”
8. Save the text file to the same location that the TransHex_TXT_to_BIN.py file is
9. Open a command shell on the client
10. Navigate to the path were TransHex_TXT_to_BIN.py is located
11. Run – python TransHex_TXT_to_BIN.py [text file name] [new binary file name]

## Conclusion

Based on a very simple idea TransHex is able to defeat very sophisticated technology.  It is obviously limited to being able to get the client into the internal network and being able to execute it.

This trick has worked since I started using it back about 5 or 6 years ago and continues to work today.  Edge technology may never be able to pick up on this as signatures would have to be made for ASCII text and compared to binary signatures or some other means would have to be created.  So have fun with it and as always don’t be stupid and break the law.

## Images
Installation Permissions

![alt text](/assets/images/transhex/www_perm.png "permissions")

Main Server Page

![alt text](/assets/images/transhex/server.png "server")

Usage Images

![alt text](/assets/images/transhex/usage_01.png "usage 01")

![alt text](/assets/images/transhex/usage_02.png "usage 02")

![alt text](/assets/images/transhex/usage_03.png "usage 03")

![alt text](/assets/images/transhex/usage_04.png "usage 04")

![alt text](/assets/images/transhex/usage_05.png "usage 05")

## Files
[https://github.com/RabbidByte/TransHex](https://github.com/RabbidByte/TransHex)