 GPIO in Python

Die Verwendung der Bibliothek [GPIO Zero](https://gpiozero.readthedocs.io/) erleichtert den Einstieg in die Steuerung von GPIO-Geräten mit Python.

## LED

Um eine an GPIO17 angeschlossene LED zu steuern, können Sie diesen Code verwenden:

```python
from gpiozero import LED
from time import sleep

led = LED(17)

while True:
    led.on()
    sleep(1)
    led.off()
    sleep(1)
```

Führen Sie dies in einer IDE wie Thonny aus, und die LED blinkt wiederholt ein und aus.

Zu den LED-Methoden gehören „on()“, „off()“, „toggle()“ und „blink()“.

## Taste

Um den Zustand einer mit GPIO2 verbundenen Taste zu lesen, können Sie diesen Code verwenden:

```python
from gpiozero import Button
from time import sleep

button = Button(2)

while True:
    if button.is_pressed:
        print("Pressed")
    else:
        print("Released")
    sleep(1)
```

Die Schaltflächenfunktionalität umfasst die Eigenschaften „is_pressed“ und „is_held“; Callbacks `when_pressed`, `when_released` und `when_held`; und Methoden `wait_for_press()` und `wait_for_release`.

## Taste + LED

Um die LED und die Taste miteinander zu verbinden, können Sie diesen Code verwenden:

```python
from gpiozero import LED, Button

led = LED(17)
button = Button(2)

while True:
    if button.is_pressed:
        led.on()
    else:
        led.off()
```

Alternative:

```python
from gpiozero import LED, Button

led = LED(17)
button = Button(2)

while True:
    button.wait_for_press()
    led.on()
    button.wait_for_release()
    led.off()
```

oder:

```python
from gpiozero import LED, Button

led = LED(17)
button = Button(2)

button.when_pressed = led.on
button.when_released = led.off
```

## GPIO Zero-Dokumentation

Viele weitere GPIO-Geräte werden von GPIO Zero unterstützt. Siehe die umfassende Dokumentation der Bibliothek unter [gpiozero.readthedocs.io](https://gpiozero.readthedocs.io/).
