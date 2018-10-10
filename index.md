---
layout: page
title: GNUVario
linkdesc: View on GitHub
linkmsg: Go !
linktarget: "https://github.com/prunkdump/arduino-variometer"
---
The GNUVario project is a collaborative effort to built an open source and open hardware variometer.

If you need help, register on [GitHub](https://github.com/prunkdump/arduino-variometer) and use the email address given on the profile page or add an issue on the project.

Features :
---------
* High precision altitude with improved ms5611 code
* High precision vertical velocity with InvenSense accelerometer (optionnal)
* Screen display with Nokia 5110 : altitude, vertical speed, ground speed, glide ratio and more (optionnal)
* Ground speed and glide ratio with GPS device (optionnal)
* Flight tracking with SD card reader and GPS (optionnal)
* Bluetooth communication with external device (optionnal)
* Battery level (optionnal)

<iframe width="560" height="315" src="https://www.youtube.com/embed/60fqfbTenkc" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Where to start :
----------------

If you don't know anything about [Arduino](https://www.arduino.cc/) start by getting :
* An easy to use Arduino board (Uno or Nano).
* A breadboard and the connecting cables.
* A ms5611 + MPU9250 board.
* An 5110 LCD display.

With this, you can start becoming familiar with the Arduino tools. Connect the components following the [schematics]({{ site.baseurl }}{% link schematics.md %}). And try compiling the source [code]({{ site.baseurl }}{% link code.md %}).

You can get the variometer working on the breadboard and start testing some different [settings]({{ site.baseurl }}{% link configuration.md %}).

Next, you can add some other components (GPS, buzzer, SD card .... ). And if everything works, you can try to put all your components in a case following yout own design.

Or if you want something small and optimized you can make the GNUVario [PCB]({{ site.baseurl }}{% link PCB.md %}) and follow the [hardware]({{ site.baseurl }}{% link hardware.md %}) tutorial.





