---
layout: splash
title: "HackRF: The easy way"
date: 2016-04-01 00:00:00 -0700
categories: Blog Misc
permalink: /:title.html
---
<br />
![alt text](/assets/images/HRF/HRF-768x280.jpg "HackRF Title")
## The Story
Recently many different radio hacks (mousejack, drone hijacking) have hit the internet. This has spurred my interest in analogoue and digital radio. After some bindge shopping on the internet and a few days wait I have had some very nice toys show up. Once the new electronics smell and novelty of a new toy had worn off the frustration kicked in.

RTL-SDR is a dirt cheap way to start playing with radio using the RTL2832U chipset. I like to do things right when I start a project so I decided to purchase a nice SDR hardware transciever that would provide all the functionality needed without breaking the bank. Enter the HackRF One from [Great Scott Gadgets](https://greatscottgadgets.com/hackrf/).

Now wanting to get up and running with this device ASAP I followed the recommendation from Michael Ossmann [https://greatscottgadgets.com/sdr/1/](https://greatscottgadgets.com/sdr/1/) and downloaded Pentoo Linux. This distro with the disposable laptop that I had dedicated to radio caused ALL my frustration. First off new inexpensive laptops now all come with Windows 10 … I like to keep that around for convenience. So I shrunk the partition and started the install of Pentoo. Now this HP laptop has the WORST BIOS I have ever come across. The ability to enable legacy booting is there but it lacks the option to set priority between UEFI and Legacy. Solution: Troubleshooting reboot to UEFI device from Windows EVERY TIME, then crash the boot process with USB drive, then the BIOS will let you select an OS from the MBR … so bloody painful!

Finally I am logged into Pentoo … no network … and video keeps crapping out. I felt like I was working on a linux system back in 1999 hunting for drivers. Let’s make this part of the story short shall we? After about 5 hours of messing with this crap Gentoo distro and not being able to get openGL running right I almost chucked the laptop across the room. Why spend all this time trying to make a piece of junk like Gentoo working when the time could be spent building a proper box with support for HackRF?

ENTER Fedora 23, yes I would much rather run RedHat however I am too cheap to pay for a subscription. I could not believe how streamlined it is to get HackRF going in Fedora so I decided to write this to save others time and pain with a crappy ass Gentoo distro. Oh and did I mention that Fedora 23 has UEFI support? Makes everything so much nicer.

This is not the officially recommended way to install gnuradio. I encountered issues with PyBOMBS and version numbers. This supposedly was fixed [https://github.com/gnuradio/pybombs/pull/246](https://github.com/gnuradio/pybombs/pull/246) although it didn’t work for me. I decided to go with dnf and see what I could do with the repositories and was pleasantly surprised.

My recommendations for HackRF One:
..* DON’T use Pentoo
..* Screen shots and build done on a VM, don’t do this. Load onto your hardware so that you can take advantage of the high USB speeds of the HackRF One
..* Install gqrx with Fedora. Not needed but very useful.

## BUILD

1. Perform a normal installation of Fedora 23 Workstation
2. Open the CLI and run the following:
```
sudo dnf update
```
![alt text](/assets/images/HRF/hrf-01.png "hrf-01")
```
sudo dnf install kernel-devel-4.2.3-300.fc23.x86_64
```
(Most likely not needed but I had to install to compile the VM additions)
![alt text](/assets/images/HRF/hrf-03.png "hrf-03")
```
sudo dnf install gnuradio
```
![alt text](/assets/images/HRF/hrf-04.png "hrf-04")
```
sudo dnf install gr-osmosdr
```
![alt text](/assets/images/HRF/hrf-05.png "hrf-05")
```
sudo dnf install gqrx
```
![alt text](/assets/images/HRF/hrf-06.png "hrf-06")
```
hackrf_info
```
(Used to check if all is good)<br />
![alt text](/assets/images/HRF/hrf-07.png "hrf-07")
3. You are done … yup that’s it. Way easier than Pentoo.
![alt text](/assets/images/HRF/hrf-08.png "hrf-08")

## Additional Reading

[The Hobbyist’s Guide to the RTL-SDR: Really Cheap Software Defined Radio](http://www.amazon.com/The-Hobbyists-Guide-RTL-SDR-Software-ebook/dp/B00KCDF1QI)

[A Software-Defined Radio for the Masses, Part 1](http://www.arrl.org/files/file/Technology/tis/info/pdf/020708qex013.pdf)

[ECE 4670 Spring 2014 Lab 6 Software Defined Radio and the RTL-SDR USB Dongle](http://www.eas.uccs.edu/~mwickert/ece4670/lecture_notes/Lab6.pdf)