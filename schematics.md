---
layout: page
title: Schematics
linkdesc: The GNUVario wiring diagram
linkmsg: Get it !
linktarget: "/assets/schematic.pdf"
---

This page explain how to assemble the GNUVario components on you own. If you use the GNUVario's PCB or the pre-build kits you can go directly to the [Hardware]({{ site.baseurl }}{% link hardware.md %}) page.

What you need :
--------------

Almost all components are optional unless the Arduino and the ms5611 boards. You can adjust to you needs. You just have to edit the *libraries/VarioSettings/VarioSettings.h* file and disable the components you don't have.

For example if you only have the speaker, the screen and the voltage divisor, change the file like this :

{% highlight c %}
/* Comment or uncomment according to  */
/* what you embed in the variometer   */ 
#define HAVE_SPEAKER
//#define HAVE_ACCELEROMETER
#define HAVE_SCREEN
//#define HAVE_GPS
//#define HAVE_SDCARD
//#define HAVE_BLUETOOTH
#define HAVE_VOLTAGE_DIVISOR
{% endhighlight %}

If you change the pins used to connect the components, don't forget to change the value also in *VarioSettings.h*.

So you need or you can get :
* An Arduino board (preferably 3.3v, see below)
* A ms5611 board ( can be combined with the MPU9250 accelerometer )
* A MPU9250 board ( can be combined with the ms5611 barometer )
* A Speaker or Buzzer ( with a 120 Ohms resistor if you don't have the L9110 chip ) 
* A L9110 amplifier
* A Nokia 5110 display
* A GPS module
* A SD card reader module
* A Bluetooth module
* A 270k and 1M resistors for the voltage divisor
* A slide switch, a Lipo Battery and a LiPO charging board.


Power design :
-------------

Choosing a proper power design for your GNUVario is a important part of the job. As there exist two types of Arduino boards (5v and 3.3v) and many types of batteries, there is many possible combinations. Each setup influence the overall current consumption and measure stability.

The main thing you need to understand is that all the voltage regulators have some drop-off voltage. So if you get a 5v Arduino board you need more than 5v for your power source. If your GPS module have a 3.3v regulator, you need to power it with more than 3.3v.

As nearly all the electronic components are now based on the 3.3v voltage, **I advice you use a 3.3v Arduino board**. This will reduce the power consumption and as single cell LiPo batteries can deliver up to 4.2v you can use them directly.

Except the Nokia 5110 display that need a regulated 3.3v source, **all the component boards need a more than 3.3v power source**. This power source is labeled **RAW_V** on the schematics.


### If you choose a 5v Arduino

The single cell batteries deliver a maximum of 4.2v voltage. So you have three possibilities :
* You buy a step-up 5v voltage regulator for your battery and you connect the output to the Arduino's 5v pin.
* You buy a more than 5v step-up voltage regulator and you connect the output to the Arduino's RAW pin.
* You put two LiPo batteries in parallel. But you need a special LiPo Charger.

So for each setup you have now a 5v regulated power source on the Arduino's 5v pin. You can directly use this power source as the **RAW_V** power source to feed all the component boards.

**!! Warning !!** If you power the Arduino board with the RAW pin, the power outputted by the 5v pin pass through the Arduino's regulator. This regulator can't feed directly the buzzer as it draw too much current. So **don't connect the buzzer directly to the 5v pin or to some other Arduino pin**. You need to add a 120 ohms resistor or you need to power the buzzer with the battery using the L9110 amplifier (see below).


Also The 5110 display need a 3.3v power source. So connect it to the Arduino's 3.3v pin.


### If you choose a 3.3v Arduino

You can connect directly the LiPo battery to the RAW pin as it deliver up to 4.2v. But you can't power the component boards with the regulated 3.3v output because each boards already have a 3.3v regulator that need a more than 3.3v power source.

So you need to power all the boards, except the 5110 display, directly with the battery output. This is your **RAW_V** power source. Connect all the component boards to the **RAW** pin.


Variometer's wiring :
---------------------

You have now your **RAW_V** power source that deliver more than 3.3v and a regulated **3.3v** power source. You can now connect all the components following this [schematic]( {{"/assets/schematic.pdf" | absolute_url }} ) or the tables below. **Be careful** the pin numbers are only valid for the Arduino boards based on the Atmega328(P) chip. 


**The ms5611 and MPU9250 board**

|    ms5611 board  |     Arduino    |  
| :--------------: | :------------: |
|       SDA        |     SDA (A4)   |
|       SCL        |     SCL (A5)   |
|       VCC        |       RAW_V    |
|       GND        |       GND      |

**Speaker or buzzer** (Without amplifier)

|     Buzzer           |     Arduino    |  
| :------------------: | :------------: |
|  + -> 120k resistor  |       D9       |
|       -              |       D10      |

**Speaker or buzzer** (With L9110 amplifier, see [datasheet](https://www.elecrow.com/download/datasheet-l9110.pdf) )

|      Buzzer      |      L9110     |  
| :--------------: | :------------: |
|        +         |       OA       |
|        -         |       OB       |

**L9110 amplifier** (see [datasheet](https://www.elecrow.com/download/datasheet-l9110.pdf) )

|      L9110       |      Arduino   |  
| :--------------: | :------------: |
|        IA        |       D9       |
|        IB        |       D10      |
|        VCC       |       RAW_V    |
|        GND       |       GND      |


**Nokia 5110 display**

|    5110 display  |     Arduino                           |  
| :--------------: | :-----------------------------------: |
|       SCK        |    SCK (D13)                          |
|     DIN/MOSI     |    MOSI (D11)                         |
|       DC         | Set in VarioSettings.h ( default D6 ) |
|       CS         | Set in VarioSettings.h ( default D7 ) |
|       RST        | Set in VarioSettings.h ( default D8 ) |
|       VCC        |    Regulated 3.3v                     |
|       GND        |      GND                              |

**SD card reader**

|    SD card reader  |     Arduino                               |  
| :----------------: | :---------------------------------------: |
|       CS           |  Set in VarioSettings.h ( default A0 )    |
|       MOSI         |      MOSI (D11)                           |
|       MISO         |      MISO (D12)                           |
|       SCLK         |      SCK (D13)                            |
|     5v or 3.3v     |    RAW_V or regulated 3.3v                |
|       GND          |         GND                               |

**GPS board**

|    GPS board     |     Arduino                  |  
| :--------------: | :--------------------------: |
|       TX         |        RX                    |
|       RX         |    Any (not used actually)   |
|       VCC        |       RAW_V                  |
|       GND        |       GND                    |

**Bluetooth module**

|    Bluetooth     |     Arduino                                     |  
| :--------------: | :---------------------------------------------: |
|       RX         |       TX                                        |
|       TX         |     Any pin with interrupts (not used actually) |
|       VCC        |       RAW_V                                     |
|       GND        |       GND                                       |

**Voltage divisor** (270k and 1M resistors inline)

|    Voltage divisor           |     Arduino                           |  
| :--------------------------: | :-----------------------------------: |
|       270k side              |       Battery + (RAW_V)               |
|       Between the resistors  | Set in VarioSettings.h ( default A2 ) |
|       1M side                |       GND                             |
                      

















 

