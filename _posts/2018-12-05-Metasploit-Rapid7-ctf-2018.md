---
layout: splash
title: "Metasploit/Rapid7 CtF: 2018"
date: 2018-12-05 00:00:00 -0700
categories: Blog CtF
permalink: /:title.html
---
## Summary

This was the first CtF that I participated in that I actually kept files for.  This was don back in late 2018 and here I am almost 4 months later trying to write about it ... yeah this will not be pretty.  Either way Metasploit / Rapid7 put on this CtF that you had to register for and basically were let loose trying to find images of playing cards.  Once you found a card the MD5 hash of the image was the flag.  Simple right?  Well I did not have nearly enough time to dedicate to this one.  I spent a whopping 3 days, which were not full days, trying to finish this and didn’t get far at all.  So let me just quickly describe what I did end up finding and my results.

## 3 of Clubs

This one was a “give me”.  Basically, all you had to do was read the rules of entry on the website.  This being my first CtF I read them just to make sure I knew what I was actually supposed to be doing.  At the end of the rules you found the following:

Thanks for actually reading our terms of service.  As a show of our gratitude, please find your splendiferous reward by pointing a web browser to your Linux host on port 31063.

Yeah it was that easy … that’s the 3 of Clubs.

![alt text](/assets/images/msf-2018/3_of_clubs.png "3 of Clubs")

## 3 of Diamonds

What would a CtF be without at least one SQLi vuln right?  Well this was one I found.  It was pretty easy to be honest.  Just sqlmap it and the key was sitting in the table.  Enter the key into the website and you got your flag … at least I think.  Either way that’s how I remember it.

![alt text](/assets/images/msf-2018/3_of_diamonds.png "3 of Diamonds")

## 10 of Hearts

This flag was found by first port scanning the Linux machine.  On TCP:8080 there was a running Apache Struts instance.  A little poking and prodding (remember I am writing this like 4 months later) we find that it is Struts 2.3.  Well Metasploit has an exploit for that … go figure right, who made this CtF?

Using exploit/multi/http/struts2_code_exec_showcase you were able to pop a shell with a reverse bash payload.  Once you were in the system it was time to hunt for the flag.  Eventually I found the flag under /usr/local/tomcat/tmp.

![alt text](/assets/images/msf-2018/10_of_hearts.png "10 of Hearts")

## My Ranking

Pretty lame but expected for the amount of time I put into it.

![alt text](/assets/images/msf-2018/results01.PNG "Results")<br />
![alt text](/assets/images/msf-2018/results02.PNG "Results")<br />
![alt text](/assets/images/msf-2018/results03.PNG "Results")<br />