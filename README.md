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
```
 sudo apt-get update
```

Then install the pre-built emulator

```
sudo apt-get install ubuntu-emulator
```

Then create an inistance (virtual phone)

```
sudo ubuntu-emulator create --arch=i386 x86-sample --password=0000
```
If you want to install it on ARM architecture, then change the `i386` to `armhf`, but be warned, the emulator will be *painfully slow*. 

We have also set the default sudo password on the inistance as `0000`, if you do not provide it with a password it will return some error.

Running the Emulator
------
In order to run the emulator, the following command need to be executed.
```
ubuntu-emulator run  --scale 0.7 --memory=720 arm-sample
```
The `scale` of the phone will be at 70%, I found the scale to be too large for my laptop, but you can change it as you desire. We also instructed the emulator to use the maximum allowed memory by this command `--memory=720`.

This will take a long time if you decided to go with ARM, be patient.
 
Then open a new terminal window/tab and connect to the inistance/phone:
```
adb shell
```
If you get any problems with the device being offline, then you will need to turn on the developer mode on the virtualised Ubuntu Phone.
```
Systmes and Setting > About this phone > Developer Mode
```  

Again, before installing anything, it is `important` that you update the system. Otherwise, you won't be able to install all the prerequisites that are needed for OpenTEE.
```
sudo apt-get update
```
If you have followed this tutorial to the letter, then the password is: 
```
0000
```
If you decided to upgrade as well, then you must tell apt to hold the `bluez` package since it will halt the upgrade for reasons unknown to me. 
```
sudo apt-mark hold bluz
```
Installing OpenTEE
------
The instructions here on how to install OpenTEE were taken from the official <a href="https://github.com/Open-TEE/project" target="_blank">OpenTEE Project Documentation </a>.

Then install the **Prerequisites** for the OpenTEE:
``` 
sudo apt-get install git curl pkg-config build-essential uuid-dev libssl-dev libglu1-mesa-dev libelfg0-dev mesa-common-dev libfuse-dev
```
Set up git global configuration:
```
git config --global user.name "Firstname Lastname"
git config --global user.email "name@example.com"
```
Create a directory for OpenTEE files:
```
mkdir Open-TEE
cd Open-TEE
```
Install **repo**
```
sudo add-apt-repository ppa:phablet-team/tools
sudo apt-get update
sudo apt-get install phablet-tools android-tools-adb android-tools-fastboot
sudo rm -r /home/phablet/.repo
```

Get repo to fetch the OpenTEE manifest
```
mkdir ~/bin
curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod +x ~/bin/repo
~/bin/repo init -u https://github.com/Open-TEE/manifest.git
~/bin/repo sync -j10
```
Install **autotools**
```
sudo apt-get install autoconf automake libtool
```
Compile using **autotools**
```
mkdir build
cd build
../autogen.sh
make
sudo make install
```
Configure OpenTEE
For reasons unknown to me, you will have to use vim in order to creat/edit the following file:
```
sudo apt-get install vim
sudo vim /etc/opentee.conf
```
Not sure why, but the `-` is missing in the `Open-TEE`, so we have to set the paths as follows using vim. First press the key `i` in order to insert/add to the file. Then copy and paste the following: 
```
[PATHS]
ta_dir_path = /opt/OpenTEE/lib/TAs
core_lib_path = /opt/OpenTEE/lib
subprocess_manager = libManagerApi.so
subprocess_launcher = libLauncherApi.so
```
Now press `escape` to finish inserting, then `write` the changes to the file and `quit` vim editor by typing the following: 
```
:wq
```

Install **qbs**
```
sudo add-apt-repository ppa:qutim/qutim
sudo apt-get update
sudo apt-get install qbs
```
Please note, if you simply do `sudo apt-get install qbs`, it will install QBS version 1.3.3+dfsg-4 which will not work. 

Configure **qbs**
```
qbs detect-toolchains
qbs config --list profiles
qbs config defaultProfile gcc
```
Compile uisng **qbs**
```
qbs debug
```