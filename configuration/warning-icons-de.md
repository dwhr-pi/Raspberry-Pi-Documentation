# Firmware-Warnsymbole

Unter bestimmten Umständen zeigt die Raspberry Pi-Firmware ein Warnsymbol auf dem Display an, um auf ein Problem hinzuweisen.

Derzeit können drei Symbole angezeigt werden.

## Unterspannungswarnung

Sinkt die Stromversorgung des Raspberry Pi unter 4,63V (+/-5%), wird folgendes Symbol angezeigt.

![Unterspannung](images/under_volt.png)

## Übertemperaturwarnung (80-85C)

Wenn die Temperatur des SoC zwischen 80 °C und 85 °C liegt, wird das folgende Symbol angezeigt. Der/die ARM-Kerne werden gedrosselt, um die Kerntemperatur zu senken.

![Übertemperatur (80-85C)](images/over_temperature_80_85.png)

## Übertemperaturwarnung (über 85 ° C)

Wenn die Temperatur des SoC über 85 °C liegt, wird das folgende Symbol angezeigt. Die ARM-Kerne und die GPU werden gedrosselt, um die Kerntemperatur zu senken.

![Übertemperatur (85C+)](images/over_temperature_85.png)