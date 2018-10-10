---
layout: page
title: Configuration
---

Before configuring the variometer you need to know how to [compile the code]({{ site.baseurl }}{% link code.md %}). Be sure you are familiar with the Arduino IDE.

All the variometer configuration is done by editing the [libraries/VarioSettings/VarioSetting.h](https://github.com/prunkdump/arduino-variometer/blob/master/libraries/VarioSettings/VarioSettings.h) file inside your Arduino folder. If you don't have an editor adapted to writing code you can download [Notepad++](https://notepad-plus-plus.org/).

Start by checking the file. There is two parts : one part concern the **software** configuration and one part the **hardware** configuration. Each option is documented by some comments directly inside this file, read them carrefully.

To "comments" a line put "//" at the start. This disable the option.


1) Checking the Hardware configuration
--------------------------------------

Before starting to setup the software you need to **make sure that the hardware configuration correspond to your setup**. You can find some tips in the [Schematics]({{ site.baseurl }}{% link schematics.md %}) page.

If you use a prebuild kit, here the parameters you need to change.

**GNUVario V2**

|         Option        |        Value                 |  
| :-------------------: | :--------------------------: |
| VARIOSCREEN_DC_PIN    |           2                  |
| VARIOSCREEN_CS_PIN    |           3                  |
| VARIOSCREEN_RST_PIN   |           4                  |

**GNUVario V3** (current PCB version)

|         Option              |        Value                 |  
| :-------------------------: | :--------------------------: |
| VARIOSCREEN_DC_PIN          |           6                  |
| VARIOSCREEN_CS_PIN          |           7                  |
| VARIOSCREEN_RST_PIN         |           8                  |
| VARIOMETER_POWER_ON_DELAY   |         3000                 |
| VOLTAGE_DIVISOR_REF_VOLTAGE |          3.0                 |


2) If needed prepare the SD card 
---------------------------------

If your GNUVario have an SD card reader you can prepare you SD card now.

**If the GNUVario's bootloader is installed on your variometer you need to complete this step** to be able to flash the firmware with the FIRM.HEX files created following the [compilation]({{ site.baseurl }}{% link code.md %}) page. You also need to know the [flashing procedure]({{ site.baseurl }}{% link bootloader.md %}).

The SD card need to be formated with a FAT16 partition of less than 2Go. Here how to do this.

### On windows or Mac

Download and install [Etcher](https://etcher.io/). With this program upload this [binary image]( {{"/assets/sdcard_fat16.zip" | absolute_url }} ) on you SD card.

You **don't need to extract the zip**. Give the image directly to Etcher.

### On Linux

**Be sure you know what you are doing !** I suppose that if you use Linux you have some skills to understand these steps.

Locate the device corresponding to you SD card inside the "/dev" folder. For example, say "/dev/sdc". You can plug and remove the SD card to be sure.

With *fdisk* as root (use *sudo* if needed) create a 1.5Go partition :

{% highlight shell_session %}
~# fdisk /dev/sdc

Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Command (m for help): d
Selected partition 1
Partition 1 has been deleted.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 
First sector (2048-30277631, default 2048): 
Last sector, +sectors or +size{K,M,G,T,P} (2048-30277631, default 30277631): +1.5G

Created a new partition 1 of type 'Linux' and of size 1.5 GiB.

Command (m for help): t
Selected partition 1
Hex code (type L to list all codes): 6
Changed type of partition 'Linux' to 'FAT16'.

Command (m for help): p
Disk /dev/sdc: 14.4 GiB, 15502147584 bytes, 30277632 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x000ee832

Device     Boot Start     End Sectors  Size Id Type
/dev/sdc1        2048 3123199 3121152  1.5G  6 FAT16

Command (m for help): w
The partition table has been altered.
Syncing disks.
{% endhighlight %}

And format the partition with *mkfs.vfat*
{% highlight shell_session %}
~# mkfs.vfat -F16 /dev/sdc1
{% endhighlight %}

3) Save you personnal informations 
----------------------------------

There is some particular settings that need to stored inside the Arduino memory. These settings are your personnals informations. You need to make this step **just one time**. These settings can't be deleted just by flashing the variometer. You need to use a special sketch.

So in *VarioSettings.h* set your **Pilot name** and **Glider name**.

Open the *SetVarioParameters* sketch. Compile and upload it inside you variometer.

When you power ON the variometer, wait for three high beeps. This signal that you personnal settings are stored in the Arduino memory. You're done for this step.

4) If needed calibrate the accelerometer
----------------------------------------

If you embed an accelerometer you need to calibrate it.

First on Windows or Mac install [Python version 2](https://www.python.org/). On Windows be sure to check the option **add to PATH variable**.

Next compile and upload the **calibration_recorder** sketch. Be sure your SD card is inside the variometer. And follow this procedure :

<iframe width="560" height="315" src="https://www.youtube.com/embed/6yxoZcxxzVY" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

This will create a "RECORD**.IGC" file on the SD card. Copy this file inside the "best-fit-calibration" folder and if needed rename it as "RECORD00.IGC".

On Windows or Mac launch the **Idle** program and open the "best-fit-calibration/calibrate.py" program. Press "F5" to run.

On Linux just run :

{% highlight shell_session %}
~$ cd Arduino/best-fit-calibration
~/best-fit-calibration$ python2 calibrate.py
{% endhighlight %}

This will show the parameters you need to replace in your *VarioSettings.h* file.

5) Upload the variometer code
-----------------------------

You can now set all the parameters you want in *VarioSettings.h*. To apply your configuration you just need to compile and upload the *Variometer* sketch.

**It's NOT needed to run set SetVarioParameters sketch again !**

This sketch is only needed if you change the **Pilot name** or the **Glider name**.

You are done !















