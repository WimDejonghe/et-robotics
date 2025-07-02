# Gebruik van Lichtsensoren met een ESP32 en MicroPython

Lichtsensoren, zoals de LDR (Light Dependent Resistor) of een digitale lichtsensor (zoals de BH1750), kunnen eenvoudig worden aangesloten op een ESP32 en uitgelezen met MicroPython.

## Benodigdheden

- ESP32 development board
- Lichtsensor (bijv. LDR + weerstand of BH1750)
- Jumper wires
- Breadboard

## Aansluitschema (voorbeeld met LDR)

1. Verbind één kant van de LDR met 3.3V.
2. Verbind de andere kant van de LDR met een analoge pin (bijv. GPIO 34) én met een weerstand naar GND.
3. De spanning over de weerstand geeft een waarde afhankelijk van de lichtsterkte.

## MicroPython Code Voorbeeld (LDR)

```python
from machine import ADC, Pin
import time

adc = ADC(Pin(34))      # Gebruik GPIO 34 (ANALOG IN)
adc.atten(ADC.ATTN_11DB) # Meetbereik tot 3.3V

while True:
    lichtwaarde = adc.read()
    print('Lichtsterkte:', lichtwaarde)
    time.sleep(1)
```

## MicroPython Code Voorbeeld (BH1750)

```python
from machine import I2C, Pin
import bh1750
import time

i2c = I2C(0, scl=Pin(22), sda=Pin(21))
sensor = bh1750.BH1750(i2c)

while True:
    lux = sensor.luminance(bh1750.BH1750.ONCE_HIRES_1)
    print('Lichtsterkte (lux):', lux)
    time.sleep(1)
```

## Toepassingen

- Automatische verlichting
- Lichtgestuurde robots
- Omgevingsmonitoring

## Tips

- Gebruik een analoge pin voor LDR's.
- Voor nauwkeurige metingen is een digitale sensor zoals de BH1750 aan te raden.
- Kalibreer de sensorwaarden voor jouw toepassing.
