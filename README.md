Installing OpenTEE on Ubuntu-Emulator
======

#Installing the pre-built emulator

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
ubuntu-emulator run  --scale 0.7 --memory=720 x86-sample
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
#Installing OpenTEE
The instructions here on how to install OpenTEE were taken from the official <a href="https://github.com/Open-TEE/project" target="_blank">OpenTEE Project Documentation </a>.

Installing prerequisites for the OpenTEE
---
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
## Install repo
```
sudo add-apt-repository ppa:phablet-team/tools
sudo apt-get update
sudo apt-get install phablet-tools android-tools-adb android-tools-fastboot
```

Get repo to fetch the OpenTEE manifest
```
mkdir ~/bin
curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod +x ~/bin/repo
~/bin/repo init -u https://github.com/Open-TEE/manifest.git
~/bin/repo sync -j10
```
## Install autotools & compile OpenTEE
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
### Configure OpenTEE

For reasons unknown to me, `nano` won't do and you will have to use `vim` in order to creat/edit the following file:
```
sudo apt-get install vim
sudo vim /etc/opentee.conf
```
First if you are not familiar with vim, then start by pressing the key `i` in order to insert/add to the file. Then copy and paste the following: 
```
[PATHS]
ta_dir_path = /home/phablet/Open-TEE/gcc-debug/TAs
core_lib_path = /home/phablet/Open-TEE/gcc-debug
subprocess_manager = libManagerApi.so
subprocess_launcher = libLauncherApi.so
```
Now press `escape` to finish inserting, then `write` the changes to the file and `quit` vim editor by typing `:wq` and press entre.

## Install qbs & compile OpenTEE

First install qt-sdk and qbs:
```
sudo apt-get install qt-sdk
sudo add-apt-repository ppa:qutim/qutim
sudo apt-get update
sudo apt-get install qbs
qbs setup-toolchains --detect
qbs config defaultProfile gcc
cd ..
qbs debug
```
Please note, if you simply do `sudo apt-get install qbs`, it will install QBS version 1.3.3+dfsg-4 which will give you errors (not that I got anywhere with this). 

Now test if everything went smoothly:
```
cd gcc-debug
./open-tee
```

See if the processes are running: 
```
ps waux | grep tee
```
You should see something like this: 
```
phablet   9251  0.0  0.0  10904   892 ?        Sl   04:49   0:00 tee_manager                    
phablet   9252  0.0  0.1   4632  1292 ?        S    04:49   0:00 tee_launcher                   
phablet  10682  0.0  0.0   5744   868 pts/16   S+   04:51   0:00 grep --color=auto tee
```
Then you test it by running an app :)
```
cd gcc-debug
./conn_test_app
```
