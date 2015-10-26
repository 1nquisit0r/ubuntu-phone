Installing OpenTEE on Ubuntu-Emulator
======

<a href="https://scan.coverity.com/projects/1nquisit0r-ubuntu-phone">
  <img alt="Coverity Scan Build Status"
       src="https://scan.coverity.com/projects/6776/badge.svg"/>
</a>

The instructions here on how to install Ubuntu-Emulator were taken from the official <a href="https://wiki.ubuntu.com/Touch/Emulator" target="_blank">Ubuntu Wiki page </a>. 

Installing the pre-built emulator
------

First update your system:
		sudo apt-get update
Then install the pre-built emulator
		sudo apt-get install ubuntu-emulator
Then create an inistance (virtual phone)
		ubuntu-emulator create MyPhone --arch=i386 myinstance
The "--arch=i386 myinstance" will create a X86 inistance. The default **ARM** inistance is just **too slow** to boot and work with.
Then run the inistance
		ubuntu-emulator run  --scale 0.7 --memory=720 MyPhone
The "scale" of the phone will be at 0.7, I found the scale to be too large for my laptop, but you can change it as you desire. We also instructed the emulator to use the maximum allowed memory by this command "--memory=720". 

 
