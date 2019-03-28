---
layout: splash
title: "AWS S3: Misconfiguration, Discovery, and Abuse"
date: 2017-06-20 00:00:00 -0700
categories: Blog Misc
permalink: /:title.html
---
## Summary

Recent news of the Verizon data leak [http://www.darkreading.com/cloud/verizon-suffers-cloud-data-leak-exposing-data-on-millions-of-customers/](http://www.darkreading.com/cloud/verizon-suffers-cloud-data-leak-exposing-data-on-millions-of-customers/) and a similar scenario concerning Dow Jones [http://www.darkreading.com/cloud/dow-jones-data-leak-results-from-amazon-aws-configuration-error/](http://www.darkreading.com/cloud/dow-jones-data-leak-results-from-amazon-aws-configuration-error/) prompted me to do a little “poking around”. The basic misconfiguration in these cases was that the administrators of the AWS S3 buckets permitted read access to “Everyone” from anywhere. So really all someone had to do was find the S3 bucket and take whatever they wanted. The interesting thing is that AWS has changed the ACL configuration on S3 buckets pretty much right after the news of Dow Jones hit the main stream media.

I started to think a little bit about this situation. If administrators make the mistake to leave sensitive information out on S3 buckets with anonymous read access could they make bigger mistakes? What if an administrator left an S3 bucket open with anonymous read/write access? All that someone would need to do is find the bucket, see if they could write to that bucket, figure out what it is used for, and then manipulate content for fun/profit.

FYI – I will not post my code for bots/spiders

## Find S3 Buckets (one way out of many)

Going through the net finding S3 buckets and testing them is painfully tedious if done manually and you would probably never find anything of use. You have to automate this process and the way I would go about doing this would be by creating a Spider.

Create a simple spider that wonders through the web looking at the source code of webpages that have the following ‘src=’ tags.
*.s3.amazonaws.com
*.s3-[aws regions].amazonaws.com

Once the spider finds these URLs in the SRC tags have it dump the value, along with the page’s URL to a log file after checking for duplicate entries. That’s it … you found an S3 bucket … whoopee, note sarcasm

If you need help building a bot or spider pick up a book and learn. I recommend this one [https://www.amazon.com/Webbots-Spiders-Screen-Scrapers-Developing/dp/1593273975/](https://www.amazon.com/Webbots-Spiders-Screen-Scrapers-Developing/dp/1593273975/)

I will use CNN.com as an example. I found on the main page the S3 bucket http://s3.amazonaws.com/cnn-sponsored-content

## Check for Write Permissions (2 different ways)

### Get the ACL
```
curl [s3 bucket url]/?acl
```

Probably the easiest and most detailed way to see if you can get anonymous write access is by reading the ACL. This has its drawbacks though as administrators could misconfigure the ACL for the bucket objects and have no access to read or write the ACL. In this case the bot would show that access is denied.

![alt text](/blog/assets/images/aws2017/s3-00.png "s3-00")

The example from CNN.com gives us the ACL because “Everyone” has access to read the ACL (READ_ACP). You can see this listed at the bottom of the server’s response.

![alt text](/assets/images/aws2017/s3-01.png "s3-01")

```
<URI>http://acs.amazonaws.com/groups/global/AllUsers</URI></Grantee><Permission> WRITE</Permission>
```

Again we don’t want to have to go through all of this manually … you got a puter … use it. Write a simple script to monitor the log file for new entries. When a new S3 bucket URL is written to the log file the script just needs to request the ACL and check for write access. This can all be done with a combination of (you don’t need them all really) curl, grep, awk, and xmllint. Don’t know how to do it? Try google, I really do not want to provide all information for someone to launch a potentially malicious bot/spider.

### Just Try to Write to the Bucket

Another way to test to see if you have write access to an S3 bucket is just to write to it. Create a simple txt file with a couple characters in it. Then use curl to upload it to the S3 bucket. If the connection ENDS with an HTTP/1.1 100 Continue then you have write access to the bucket.

![alt text](/assets/images/aws2017/s3-02.png "s3-02")

```
curl -v -H acl=public-read -H key=test.txt -T test.txt http://s3.amazonaws.com/cnn-sponsored-content
```

![alt text](/assets/images/aws2017/s3-03.png "s3-03")

### Abuse: What Does the Bucket Contain

This is where it gets a tad fun. So lets say that CNN had all their videos for their website hosted in an S3 bucket called cnn_video and we successfully wrote to http://s3.amazonaws.com/cnn_video Well if you have write access in S3 you also have delete access (at least it seems that way in my testing).

So if the video http://s3.amazonaws.com/cnn_video/main.mp4 was linked in www.cnn.com/index.htm then all an attacker would have to do is use the AWS REST API to delete (http://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectDELETE.html) the video in the bucket and replace it with whatever other video they wanted (just named the same). The attacker now has their video posted on CNN … hypothetically of course.

### Note:

* All CNN links with the exception of the one S3 bucket are fictional<br />
* Yes, this was written quickly<br />
* Yes, this is incomplete<br />
* No, I will not give you the code for my spider<br />