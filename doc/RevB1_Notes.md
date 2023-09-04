# Modifications

1. Change F1 to 0.75 A polyfuse SMD1206B075TF/16

2. Bodge wire to U111 charge pump VIN pin. Othervise the output voltage stayed at 1.6 V. This is due to an layout error: EN was mistaken with VIN. The resulting impedance of the long, narrow track to VIN was obviously to high (the switching frequency is 2 MHz!).

   ![](img/U111_fix.JPG)

# Power supply

Current consumption is 20 mA. Iq of [LM2775](https://www.ti.com/lit/ds/symlink/lm2775.pdf?ts=1693816107344&ref_url=https%253A%252F%252Fwww.google.com%252F) is 5 mA typ. with forced PWM mode (PFM = 0). Doesn't vary with change of input voltage.

![image-20230904195308126](img/image-20230904195308126.png) ![image-20230904195611369](img/image-20230904195611369.png)

In RevA1 current consumption was 7 mA with Vx- = GND (-2V unused but present). The "old" LM27762 had an Iq of 390 µA.

| Component | Quiescent Current (mA) |
| --------- | ---------------------- |
| AD8418A   | 4.1                    |
| AD8655    | 3.7                    |
| LM2775    | 5.0                    |
| **Total** | **12.8**               |

Efficency is around 45%:

![image-20230904200355752](img/image-20230904200355752.png)

$\frac{1}{45\ \%} \cdot (4.1\ +\ 3.7)\ mA =\ 17.3 mA$ so the measurements seem plausible. PFM mode shouldn't improve the results with load current > 7 mA:

![image-20230904200846734](img/image-20230904200846734.png)



## +5V voltage ripple

![](scope/SDS2504X_Plus_PNG_23.png)

## Line regulation

## Load regulation



# Output measurements

## Output voltage at 0 A

![](scope/SDS2504X_Plus_PNG_24.png)

# Thermal Images

## Battery Charger

![](img/IRI_20230831_150342.jpg)

## Sense Resistor

@ 3 A, 5 A, 6 A (6 A not in steady state, temp. still slowly rising). Max. continuous current should be limited to 5 A.

![](img/IRI_20230831_151020.jpg) ![](img/IRI_20230901_131541.jpg) ![](img/IRI_20230901_131411.jpg)

## Charge Pump

No Load:

![](img/IRI_20230831_151045.jpg)

Load 50R max. Vout (4.6 V):

![](img/IRI_20230831_155653.jpg)

## 50R Output Buffer

With 50R load at max. Vout = 4.6 V. The output voltage swing doesn't reach the full 5 V supply voltage. Without 50R load the output voltage is very close to the supply voltage.

![](img/IRI_20230831_155546.jpg)

Whole PCB:

![](img/IRI_20230831_155638.jpg)

# Changes for RevC1

1. Increase trace width to sense resistor (main current path)
2. Change distance of binding posts to 3/4 inch to support double banana plugs.
