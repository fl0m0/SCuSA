# Changes

1. Change F1 to 0.75 A polyfuse SMD1206B075TF/16
2. Bodge wire to U111 charge pump VIN pin. Othervise the output voltage stayed at 1.6 V. This is due to an layout error: EN was mistaken with VIN. The resulting impedance of the long, narrow track to VIN was obviously to high (the switching frequency is 2 MHz!).
3. ![](img/U111_fix.JPG)

## +5V voltage ripple

![](scope/SDS2504X_Plus_PNG_23.png)

## Output voltage at 0 A

![](scope/SDS2504X_Plus_PNG_24.png)

# Thermal Images

## Battery Charger

![](img/IRI_20230831_150342.jpg)

## Sense Resistor

Tested with 3 A

![](img/IRI_20230831_151020.jpg)

## Charge Pump

No Load:

![](img/IRI_20230831_151045.jpg)

Load 50R max. Vout (4.6 V):

![](img/IRI_20230831_155653.jpg)

## 50R Output Buffer

With 50R load at max. Vout (4.6 V)

![](img/IRI_20230831_155546.jpg)

Whole PCB

![](img/IRI_20230831_155638.jpg)

