# Errors

1. C101, C102: wrong footprint, 0805 instead of 0603
2. FB100, FB101: wrong footprint, 1206 instead of 0805 but 742792620 should be 0603
3. DNP parts are shown on ibom
4. U103: wrong pinout (should be non-R type), GND and RESET flipped.

# Modifications

1. FB100, FB101: used 742792118 (had the right footprint and was available as leftover from previous projects)
1. R104 changed to 1.2k to adjust charging indicator LED brightness
1. R103 changed to 4.7k to increase charge current to ~ 210 mA
1. Replace U103 with [BD4836G](https://fscdn.rohm.com/en/products/databook/datasheet/ic/power/voltage_detector/bd48xxg-e.pdf), 3.6 V Supervisor in SOT-23-5 package. Solder bodge wire on pad 3 to pin 2.

![](img/U103_fix.JPG)

# To Dos

1. Full charging cycle
2. Use a real USB-Power Supply
3. Negative measure current
4. Solder voltage supervisor
5. Measure current consumption

# Battery

![Zertifizierte EREMIT LiPo LiIon Akku Batterie 1S JST-PH 2.0 Stecker GPS Arduino - Bild 6 von 16](img/s-l500.jpg)

* From ebay "Zertifizierte EREMIT LiPo LiIon Akku Batterie 1S JST-PH 2.0 Stecker GPS Arduino"
* Integrated protection circutry: Overchare, overdischarge, overcurrent

| Parameter                | Symbol  | Value           |
| ------------------------ | ------- | --------------- |
| Technology               |         | Lithium Polymer |
| Dimensions               |         | 9x 20x 30mm     |
| Capacity                 | C       | 500 mAh         |
| Nominal charing current  | I_c,nom | 0,5 C           |
| Maximum charging current | I_c,max | 1 C             |
| Nominal voltage          | V_nom   | 3.7 V           |
| Maximum voltage          | V_max   | 4.2 V           |
| Internal resistance      | R_ESR   | 90 mR - 120 mR  |

# Photos

![](img/SCuSa000.jpg) 

![](img/SCuSa001.jpg) 

![](img/SCuSa002.jpg) 

# Measurements

* `PGOOD` LED turns off below 3.14 V

* `Low_Bat` Turns on at 3 V. Expected but to low. Should be 3.5 V or 3.6 V.
* Charging current = 230 mA
* Current consumption = 15,9 mA (without 50R termination connected to the SMA connector)

## Supply rail voltage ripple

+3V3

V_PP = 4,9 mV

V_RMS = 855 µV

![](scope/SDS2504X_Plus_PNG_3.png)

+3V3 after ferrite bead:

V_PP = 4,5 mV

V_RMS = 814 µV

![](scope/SDS2504X_Plus_PNG_4.png)

-2V:

V_PP = 5,3 mV

V_RMS = 1,1 mV

![](scope/SDS2504X_Plus_PNG_5.png)

-2V after ferrite bead:

V_PP = 4,13 mV

V_RMS = 800 µV

![](scope/SDS2504X_Plus_PNG_6.png)

# Output signal noise

5mR sense resistor. CH2 set to high impedance input

CH1: Current sense amplifier output

CH2: 50R buffer output (actually 47R, no termination resistor in scope)

V_PP = 12,5 mV

V_RMS = 1,43 mV

![](scope/SDS2504X_Plus_PNG_7.png)

## Correlation with power supply ripple

No obvious correlation. Signal noise seems to be inherent to signal chain, not supply voltage.

F1 = FFT(C1), F2 = FFT(C2)

![](scope/SDS2504X_Plus_PNG_8.png)

![](scope/SDS2504X_Plus_PNG_10.png)

# Accuracy

**Measurement setup:**

* PSU GPP-4323 as power source. Current readings taken from PSU. Accuracy above 50 mA is approx. 1%, below more like 5%.
* DUT directly connected to PSU (constant current, almost short circuit operation)

**Results:**

* 1,5% accuracy approx. from 50 mA to 3 A.

![image-20230804195229701](img/image-20230804195229701.png)

# Frequency Response

## Bode plot

**Measurement setup:**

* Siglent SDS2504X Plus
  * Generator level 3 Vpp (50R)
  * CH1 = DUT voltage output
  * CH2 = Measured generator level
* DUT with 50R in series connected to signal generator (resulting peak to peak current = 3 Vpp / (50R + 50R) = 30 mA)

**Results:**

* 3dB bandwidth = 232 kHz

* Output voltage amplitude in Vpp:

![](scope/SDS2504X_Plus_PNG_17.png) 

* CH1 / CH2 in dB (why -34dB? Current sense amplifier is scaling 1 A / 1 V. With 100 R the current is 100 times smaller than the generator voltage. 1/100 = -40dB. The difference of 6dB = 0,5 comes from the voltage divider: generator output resistance and the resistance added to the current path. Both are 50R)

![](scope/SDS2504X_Plus_PNG_16.png) 

## Time domain (square wave)

Same measurement setup as in [Bode plot](#Bode plot). The noise is relatively high because of the limited current capability of the signal generator (this applies for the bode plot, too).

![](scope/SDS2504X_Plus_PNG_19.png) 
![](scope/SDS2504X_Plus_PNG_20.png) 
![](scope/SDS2504X_Plus_PNG_21.png) 

# Change to single supply

With JP103 in setting 2-3 the negative supply voltage is set to GND. I implemented this option to because I wasn't sure if a dual supply is such a good idea. **It was not!**

![image-20230816185456309](img/image-20230816185456309.png)

V_PP drops from 12.5 mV to 2.5 mV, V_RMS drops from 1.43 mV to 180 µV at the current sense amplifier output. The noise at the 50R buffer (CH4) is even lower.

I guess this is where the Power Supply Rejection Ratio (PSRR) of the amplifier comes into play. With a dual supply and the scope measurement referenced to ground the PSRR doesn't do anything. For a redesign I will use a step up converter and a single supply. Possible parts:

* [LM2750](https://www.ti.com/lit/ds/symlink/lm2750.pdf) or [LM2775](https://www.ti.com/lit/ds/symlink/lm2775.pdf)
* [NCP163](https://www.onsemi.com/download/data-sheet/pdf/ncp163-d.pdf) as post regulator, fixed voltage of 4.5V
* [TPS7A49](https://www.ti.com/lit/ds/symlink/tps7a49.pdf) as post regulator, adjustable voltage, ~300 mV dropout, high PSRR > 1 MHz

![](scope/SDS2504X_Plus_PNG_22.png)

# Notes

* Trimming potentiometer can be used to increase the positive (or negative) measurement range. Turing it fully left offsets the output voltage to -2 V. This increases the positive current measurement range to 5.3 A (3.3 V - (-2 V)).
