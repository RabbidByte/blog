---
layout: splash
title: "The New Phish"
date: 2019-06-06 00:00:00 -0700
categories: Blog
permalink: /:title.html
---
## Microsoft platform for Phishing ... Microsoft

Scams and phishing are continually getting more creative, and today I came across one phishing attack that really kinda amazed me.  The attack uses a legitimate Microsoft login page that will POST credentials to a malicious server.  I have not seen anything like this in the past so I thought that it would definitely be worth looking into.

The attack starts as most do ... an email.  The subject line is "Unblock Messages for [email address]" and the email claims that the portal (labeled by the email's domain) has blocked some emails and that the user must release them.  The phishing email is not very convincing as it starts with Dear [username] and finishes with Regards, [domain] among other red flags.  But the URL is where things get quite interesting.

![alt text](/assets/images/phish/phish-01.PNG "phish-01")

The initial hyperlink went to https://[redacted]/some/directory/redirect.aspx?url=https://[redacted].z6.web.core.windows.net/?username=anemailaddress  The attack initially uses an open redirect vulnerability on a website to push the user to a Microsoft login page.  This bypasses most if not all URL rewriting controls on emails and gets the user to the "payload" page.

![alt text](/assets/images/phish/phish-02.PNG "phish-02")

![alt text](/assets/images/phish/phish-03.PNG "phish-03")

The login page is crazy neat!  It is a real Microsoft login page with an additional gotcha.  During the login process it adds an extra POST that sends the entered username and password to the attacker's server.  

![alt text](/assets/images/phish/phish-04.PNG "phish-04")

![alt text](/assets/images/phish/phish-05.PNG "phish-05")

![alt text](/assets/images/phish/phish-06.PNG "phish-06")
