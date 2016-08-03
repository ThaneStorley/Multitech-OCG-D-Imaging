# Setting Up Your Multitech OCG-D with a Base Image
##### By Thane Storley #####


## Getting Started:

Below is a complete guide to set up a Multitech OCG-D, and install a base image onto it. 
This guide will walk you through the process step-by-step to ensure you 
are successful. I will be using Ubuntu 12.04 and Multitech's most recent version of 
CoreCDP ('CoreCDP-2.3.4') in this guide.



### What You'll Need:

* A computer or virtual machine using Ubuntu 10.4 or 12.04
	* (12.04 is optimal and what I will be using in this guide)
	* If you Don't currently have a computer or virtual machine running one of these:
		* [Ubuntu 12.04 Download](http://releases.ubuntu.com/12.04/)
		* [Virtual-Box Download](https://www.virtualbox.org/wiki/Downloads)
		* [Instructions on how to set up VirtualBox with Ubuntu](http://www.psychocats.net/ubuntu/virtualbox)
* A Multitech OCG-D (With the SD card), the SD card is crucial
* A USB thumb drive (flash drive)
* A 'Serial to USB' converter
* A paperclip
* To be focussed and not mess up.
    * Accidentally missing just *one* line of code or forgetting *one* step can be 
	disastrous

If you have the  **MultiConnect OCG Break-Out Board**, then you will need:
* The **Serial to Serial Cable** that came in the Multitech OCG-D Kit
* The **36 Pin to 36 Pin cable** that came in the Multitech OCG-D Kit

If you Don't have the **MultiConnect OCG Break-Out Board**, then you will need:
* A star-head screwdriver (T7)
* A philips-head screwdriver
* The **3 Pin Debug Cable** that came in the Multitech OCG-D Kit


### Some Things to Know

Most Multitech OCG-D's have set defaults, but depending on the distributer of your 
device, these defaults might be different.  As described by Multitech, these are the 
default-defaults.

IP Adress: 192.168.2.1  
User: root  
Password: root



## Downloading The Image

1. Click [here](https://github.com/ThanerZ/Multitech-OCG-D-Imaging) to download the image files and [here](http://www.multitech.net/developer/wp-content/plugins/download-monitor/download.php?id=11) to download Multitech's software for communicating to the OCG (CoreCDP).

##### The Files you need are:

* CoreCDP-2.3.4.tar.gz (the CoreCDP index)
* rootfs.jffs2 
* uImage.bin

2. Transfer those files onto your thumb (flash) drive.
3. Create a folder on the SD card and title it "flash-upgrade".  (Make sure the spelling 
and it is all lower-case).
4. Transfer 'rootfs.jff2' and 'uImage.bin' onto the SD card, and into the folder you just 
created.
5. Eject the SD card and put it back into the OCG.



## Setting Up Your Environment


This process creates a working space used to comunicate with the Multitech OCG-D.

1. **(A)** If you are using a sperate computer for Ubuntu, take your flash drive and 
insert it into the other computer   
**(B)** If you are using a virtual machine, unmount (eject) the USB from your computer,
but don't take it out.  Then in the bottom left corner of your virtual machine (when it 
is running), click on the USB logo, and then click on the your USB device to add it.
2. Transfer the CoreCDP file 'corecdp-2.3.4.tar.gz' to your Ubuntu machine.
3. Right click on the 'corecdp-2.3.4.tar.gz' that you copied to your Ubuntu machine, and note 
it's file path.  You can copy it, you can write it down, you can remember it, I don't 
care so long as you have it.  Case is important so don't mess it up.
4. Untar the Contents of this folder using this command in the terminal
>`tar xzvf **THAT FILE PATH YOU WROTE DOWN**corecdp-2.3.4.tar.gz`

for example I wrote this:
>`tar xzvf /home/thane/corecdp-2.3.4.tar.gz`

5. Now run this set of commands:
	Note: Make sure you wait until each command finishes before you start a new one

> `cd corecdp-2.3.4`    
>`sudo ./multitech/contrib/install-deps/install-debian-ubuntu-deps.sh`  
>`source env-oe.sh`



## Connecting to Your Device


If you want to comunicate and program your device, it is kind of important to connect to it.

Before you do anything however, It is important we wipe the device of anything it might contain. Power up the Multitech OCG-D, and use the paperclip you were supposed to grab, by holding it down on the reset button for a fair amount of time.  I did it for 30 seconds then repeatedly pressed it for another 10 just to be sure it was reset.

#### Connecting *Physically* to your Device
##### If you are using the **MultiConnect OCG Break-Out Board**: #####
1. Connect the the **36 Pin to 36 Pin cable** that came in the Multitech OCG-D Kit to both the **Multitech OCG-D** and the **MultiConnect OCG Break-Out Board**.
2. Connect the the **Serial to Serial Cable** that came in the Multitech OCG-D Kit to the **MultiConnect OCG Break-Out Board**.
3. Connect the **Serial to USB** cord to the *Serial to Serial Cable** and your computer.   
**Note:** If you are running virtual machine, you need to get the machine to recongnize the USB. You can do the same thing you did with the flash drive to do this (selecting the USB icon at the bottom of your virtual machine), but instead select the cable that you just plugged in.
	If you are confused on which one that is, unplug it from your computer, note the USB 
	devices that are already there, plug it back in, and select the new device.
##### If you are using the **3 Pin Debug Cable**: #####
1. Use the philips head screwdriver to unscrew the screws on the bottom, and the T7 star head screwdriver to unscrew the screws on the back (the side with all of the ports, ***not*** the front side with the Logo/lights). To illustrate, here are some [pictures!](http://www.multitech.net/developer/products/multiconnect-ocg/hardware/how-to-setup-serial-console-cable/) .   
	**For Gods' sake please don't loose the screws**
2. Pull out the OCG form its case and plug in the 3 Pin Debug Cable to the 3 pins sticking out of the OCG (also shown in the [pictures!](http://www.multitech.net/developer/products/multiconnect-ocg/hardware/how-to-setup-serial-console-cable/)).
3. Connect the 3 Pin debug Cable to your **Serial to USB** converter and plug that into your 
Computer.   
**Note:** If you are running virtual machine, you need to get the machine to recongnize the USB. You can do the same thing you did with the flash drive to do this (selecting the USB icon at the bottom of your virtual machine), but instead select the cable that you just plugged in.
	If you are confused on which one that is, unplug it from your computer, note the USB 
	devices that are already there, plug it back in, and select the new device.



## Configuring 'Minicom'


Minicom is a software tool used to communicate with other devices, and in this case, 
that is exactly what you need to do.

1. Type this command in your terminal to install Minicom, once you type this, you will 
need to enter your password.  It may look like you aren't typing anything when entering your 
password, but I assure you, you are.
>`sudo apt-get install Minicom`

2. Once that finishes, type in this command to start Minicom.
>`sudo minicom -s`

3. Once Minicom is up and running, use the arrow keys to navigate down to *'Serial Port Setup'*, and make sure the settings are configured to:
	* 'Serial Device' **to** `/dev/ttyUSB0`
	* 'Lockfile location' **to** `/var/lock`
	* Both 'Callin Program' and 'Callout Program' should be blank
	* 'Bps/Par/Bits' **to** `115200 8N1`
	* 'Hardware Flow Control' **to** `NO`
	* 'Software Flow Control' **to** `NO`
4. Exit and navigate down to select *'Save setup as dfl'*.
5. Select *'Exit from Minicom'*



## Putting The Exosite Image Onto Your Multitech OCG


This will complete the process and allow you to start working with your Multitech OCG-D

1. Make sure the SD card is in the OCG and the OCG is connected in the fasion instructed under the 'Connecting *physically* to your Device' section under *'Connecting to Your Device'*, and your Multitech OCG-D is powered on.
2. Type this command into your terminal, and again; you might have to type your password.

>`sudo minicom`

3. Log into the CoreCDP     
**Note:** Log in credientials might varry depending on the distributor you obtained your Multitech OCG-D from, but the base credentials are listed above under the *'Some Things to Know'* section.
4. Type these 2 commands after being logged in

>`touch /var/volatile/do_flash_upgrade`     
>`reboot`


5. Upon reboot, you should see something like this:
~~~~
Broadcast message from root (ttyS0) (Tue Nov 15 05:35:42 2011):
 
The system is going down for reboot NOW!
INIT: Sending processes the TERM signal
Stopping Dropbear SSH server: stopped /usr/sbin/dropbear (pid 303)
dropbear.
Stopping Vixie-cron.
Deconfiguring network interfaces... done.
Stopping Lighttpd Web Server: stopped /usr/sbin/lighttpd (pid 342)
lighttpd.
Stopping syslogd/klogd: stopped syslogd (pid 309)
stopped klogd (pid 311)
done
Sending all processes the TERM signal...
eth0: link down
Sending all processes the KILL signal...
Unmounting remote filesystems...
/var/volatile/flash-upgrade not present, skipping
 
Starting flash upgrade from /media/card/flash-upgrade...
Flashing /dev/mtd5 (uImage) with /media/card/flash-upgrade/uImage.bin...
flash_eraseall has been replaced by `flash_erase <mtddev> 0 0`; please use it
Erasing 128 Kibyte @ 740000 -- 100 % complete 
Writing data to block 0 at offset 0x0
Writing data to block 1 at offset 0x20000
Writing data to block 2 at offset 0x40000
Writing data to block 3 at offset 0x60000
Writing data to block 4 at offset 0x80000
Writing data to block 5 at offset 0xa0000
Writing data to block 6 at offset 0xc0000
Writing data to block 7 at offset 0xe0000
Writing data to block 8 at offset 0x100000
Writing data to block 9 at offset 0x120000
Writing data to block 10 at offset 0x140000
Writing data to block 11 at offset 0x160000
Writing data to block 12 at offset 0x180000
Writing data to block 13 at offset 0x1a0000
Writing data to block 14 at offset 0x1c0000
Flashing /dev/mtd8 (rootfs) with /media/card/flash-upgrade/rootfs.jffs2...
Deactivating swap...
Unmounting local filesystems...
umount2: Device or resource busy
umount: none busy - remounted read-only
umount2: Device or resource busy
umount: devtmpfs busy - remounted read-only
flash_eraseall has been replaced by `flash_erase <mtddev> 0 0`; please use it
Erasing 128 Kibyte @ 0 --  0 % complete flash_erase:  Cleanmarker written at 0
Erasing 128 Kibyte @ 20000 --  0 % complete flash_erase:  Cleanmarker written at 20000
...
Writing data to block 593 at offset 0x4a20000
Writing data to block 594 at offset 0x4a40000
Writing data to block 595 at offset 0x4a60000
Rebooting...
Restarting system.
~~~~
	
If you saw code similar to that, you have successfully imaged your Multitech OCG-D 
with Exosite's Open Gateway Engine

# **CONGRATULATIONS**
Yea you finished, have fun.

                    ._______________.
                   /                 \
                  /      *       *    \
                 |                     |
                  \    \___________/  /
                   \                 /
                      \____________/
