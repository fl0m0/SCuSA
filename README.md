# SCuSA
 **S**cope **Cu**rrent **S**ense **A**mplifier, work in progress

<img src="doc/img/SCuSa002.jpg" width="35%">


# Design goals

The main reason I designed this current sense amplifier was the need to verify power supplys on PCBs:

* Turn on behavior
* Inrush current
* Short circuit current
* Ripple current

My main requirements were:

* High side measurement > 60 V
* Common mode input range down to 0 V
* Burden voltage below 150 mV
* No galvanic isolation
* Current range up to approx ~3 A
* Accuracy better than 2%
* Bandwith > 150 kHz
* Bonus: Measurement of negative current (for battery charging applications)

I had already built a simple current sense amplifier based on the [INA169](https://www.ti.com/product/INA169) on a breadboard. It fullfilled most of my requirements except the common mode voltage range: 2.7 V made it impossible to measure 1.8 V or 1.0 V voltage supplies which are quite common these days.

I ended up with a design way to complicated and expensive :-) But I really wanted to try out a few interesting ICs:

* [LM27762](https://www.ti.com/product/LM27762): "Low-Noise Positive and Negative Output Integrated Charge Pump Plus LDO"
* [AD8210A](https://www.analog.com/en/products/ad8210.html) or [AD8418A](https://www.analog.com/en/products/ad8418a.html): Very fancy looking "High Voltage, Bidirectional Current Shunt Monitors"

# Current status

* RevA built and partially tested
* Documentation not finished
* RevB work in progress (fixed errors, improvements)
