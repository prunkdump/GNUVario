---
step: 2
description: Setting power configuration
---

The GNUVario come with two possible power modes **STANDARD** and **EXPERT**.

In the **STANDARD** mode all the components boards can be soldered directly. They are powered with the 4.2v power coming from the LiPo battery. As each board have an 3.3v regulator this power source can be used directly.

In the **EXPERT** mode the components boards are powered with the Arduino's regulator output. And it's **not** fine to power a 3.3v regulators with a 3.3v source as the regulator have some drop-out voltage. So in this mode you need to bypass all the board's regulators. And you can also change the Arduino's regulator with a good quality one. This manner you can greatly improve the variometer power consumption.

So prepare a wire for the power mode you want :
{% include tutoimg.md name="IMG_6319.JPG" %}
{% include tutoimg.md name="IMG_6321.JPG" %}

Cut the wire's tips very close of the PCB. Keeping just enough to touch the tips with the soldering iron.
{% include tutoimg.md name="IMG_6322.JPG" %}

Next solder one tip and check the wire placement before soldering the other :
{% include tutoimg.md name="IMG_6323.JPG" %}
{% include tutoimg.md name="IMG_6324.JPG" %}
{% include tutoimg.md name="IMG_6326.JPG" %}


