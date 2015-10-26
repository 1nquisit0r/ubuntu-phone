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
sudo ubuntu-emulator create --arch=armhf arm-sample --password=0000
```
The default architecture is ARM, but if you are interested in installing the emulator on X86 architecture, then you can change `armhf` to `i386`. 
It is worth noting, that the ARM architecture is *painfully slow* for the emulator to boot and load. From what I have read online, most people go for `--arch=i386` because it is much faster.

However, the whole purpose of this installation for me was to see if we can run OpenTEE on the Ubuntu Phone which has ARM `Quad Core Cortex A7`.
The phones can be found here: 
- <a href="http://www.store.bq.com/gl/ubuntu-edition-e5" target="_blank">Aquaris E5 HD Ubuntu Edition</a>
- <a href="http://www.store.bq.com/gl/ubuntu-edition-e-4-5" target="_blank">Aquaris E4.5 Ubuntu Edition</a>

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
If you get any problems with the device being offline, then you will need to turn on the developer mode.
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
Install `repo`
```
mkdir -p ~/bin
curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod +x ~/bin/repo
```
Install `qbs`
```
sudo apt-get install qbs
```