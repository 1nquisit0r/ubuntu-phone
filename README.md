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

 ```sudo apt-get update

Then install the pre-built emulator

```sudo apt-get install ubuntu-emulator

Then create an inistance (virtual phone)

```sudo ubuntu-emulator create --arch=i386 x86-sample --password=0000

The "--arch=i386 myinstance" will create a X86 inistance. The default **ARM** inistance is just **too slow** to boot and work with. We have also set the default password as 0000.

Running the Emulator
------
Then run the inistance:

```ubuntu-emulator run  --scale 0.7 --memory=720 x86-sample

The "scale" of the phone will be at 0.7, I found the scale to be too large for my laptop, but you can change it as you desire. We also instructed the emulator to use the maximum allowed memory by this command "--memory=720".
First of all, in order to connect using ADB, you need to turn on the developer mode. 

```Systmes and Setting > About this phone > Developer Mode   

Then open a new terminal window/tab and connect to the phone:

```adb shell

Installing OpenTEE
------
The instructions here on how to install Ubuntu-Emulator were taken from the official <a href="https://wiki.ubuntu.com/Touch/Emulator" target="_blank">Ubuntu Wiki page </a>.




 
