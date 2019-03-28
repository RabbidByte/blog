---
layout: splash
title: "Transfer Any Binary into a Protected Network"
date: 2013-06-24 00:00:00 -0700
categories: Blog Misc
permalink: /:title.html
---
## Summary

Well this is another old trick that still works today.  It still gets past all edge security and antivirus … well that is until you pull the executable out again.  I find myself using this trick when I need to bring penetration testing tools into a network.  Most of the time antivirus or edge security devices will stop the transfer of these executables.  So what can you do to get them in?

The first is very obvious, just zip them up and encrypt them.  This will get the package into some networks but not all.  Many networks now configure edge devices and email security systems to block and/or alert on encrypted attachments.  This would probably blow your cover and game over.

Another way to get executables into networks is by using a system called [TransHex](https://github.com/RabbidByte/TransHex "TransHex Download") written while I was running essentialexploit.com.  As of now this has not been released but it will be very soon (*update* TransHex is now released).  This application requires a web server on the internet that dynamically converts binary to hex and then gives you a text file that you can download.  Once you have the text file the TransHex client will put that back into a binary for you.

The simplest way to get an executable into a network is to email it or just download it.  However as we mentioned earlier most edge devices will stop the download and alert the admins.  Not something that you want to have happen, however with a little prep you can hide any binary into another file and just bring it on into a network.  Here is how it works.

## How To Do It

You must know beforehand which executibles you want send.  This is the downside.  If you forget about a program that you need you will have to have access to the outside system again to send it.  Sometimes this could delay your penetration testing by quite a few hours, and sometimes it is not a problem at all.  So here’s what you do.

1.  On an external windows system take a harmless picture file and your executable and drop them in the same folder so that it’s easy to work with them
2.  Compress your executable (no encryption needed) into a zip file
3.  Open up a windows command prompt
4.  Navigate into the folder where your files are located
5.  Run – copy /B \[image file\] + \[zip file\] \[new image file\]
6.  Now there is a new file in your folder that looks exactly like your original image
7.  This new file is the image file with the zip in binary appended to the end of the file
8.  Now send this file into the protected network either by email, http … whatever
9.  On the internal windows system open up 7zip
10.  Under “File” select “open” and find your image file
11.  Once 7zip opens the image … you see your package!  Extract it and away you go, but beware of local antivirus because once the exe is extracted it will most likely be scanned.

## Images From Testing
The command

![alt text](/assets/images/transhex/command.png "The Command")

The files

![alt text](/assets/images/transhex/file_listing.png "The Files")

Scan the attachment (just to show that it was malicious)

![alt text](/assets/images/transhex/scan-1.png "Scan 1")

Scan the malicious image

![alt text](/assets/images/transhex/scan-2.png "Scan 2")

Extracting the package from the image

![alt text](/assets/images/transhex/zip.png "Zip")
