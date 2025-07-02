---
mathjax:
  presets: '\def\lr#1#2#3{\left#1#2\right#3}'
---

# Temperatuur sensoren.

Hieronder een aanzet over het gebruik van **temperatuursensoren** met een **ESP32** geprogrammeerd in **MicroPython**. De focus ligt op het gebruik van een **DS18B20** sensor (digitale temperatuursensor) via de **1-Wire** interface, een veelgebruikte sensor in hobbyprojecten.

---

## Temperatuursensoren gebruiken met de ESP32 en MicroPython

In deze handleiding leer je hoe je een **DS18B20** temperatuursensor gebruikt met een **ESP32** microcontroller, geprogrammeerd in **MicroPython**.

### Benodigdheden

- ESP32 board
- DS18B20 temperatuursensor
- 4.7kΩ pull-up weerstand
- Jumper wires
- Breadboard

### Aansluitschema

| DS18B20 pin | Verbinden met ESP32 |
|-------------|---------------------|
| VDD         | 3.3V of 5V          |
| GND         | GND                 |
| DQ          | GPIO (bijv. GPIO 15) + 4.7kΩ pull-up naar VDD |

### MicroPython installatie op de ESP32

1. Installeer MicroPython firmware op de ESP32 met [esptool](https://github.com/espressif/esptool).
2. Gebruik een tool zoals **Thonny**, **uPyCraft** of **rshell** om scripts te uploaden en uit te voeren.

### Codevoorbeeld: DS18B20 uitlezen

```python
import machine
import onewire
import ds18x20
import time

# Gebruik GPIO15 voor 1-Wire
datapin = machine.Pin(15)

# Initialiseer de bus
ow = onewire.OneWire(datapin)
sensor = ds18x20.DS18X20(ow)

# Zoek naar aangesloten sensoren
roms = sensor.scan()
print("Gevonden sensoren:", roms)

while True:
    sensor.convert_temp()
    time.sleep_ms(750)  # Wacht op conversie
    for rom in roms:
        temp = sensor.read_temp(rom)
        print("Temperatuur: {:.2f}°C".format(temp))
    time.sleep(2)
```

### Uitleg code

* `onewire` en `ds18x20` zijn MicroPython-modules voor communicatie met 1-Wire sensoren zoals de DS18B20.
* `sensor.scan()` zoekt alle sensoren op de bus.
* `convert_temp()` start een temperatuurmeting.
* `read_temp(rom)` leest de temperatuurwaarde.

### Opmerkingen

* De sensor heeft tijd nodig om de temperatuur te meten. Wacht minstens 750ms na `convert_temp()`.
* De DS18B20 kan meerdere sensoren op één pin ondersteunen (uniek ROM-adres per sensor).

### Alternatieve temperatuursensoren

* **DHT11/DHT22** (digitale sensor, lagere nauwkeurigheid)
* **TMP36** (analoog, uitlezen via ADC)
* **BME280/BMP280** (ook luchtdruk en luchtvochtigheid)

## Referenties

* [MicroPython DS18X20 documentatie](https://docs.micropython.org/en/latest/library/ds18x20.html)
* [OneWire documentatie](https://docs.micropython.org/en/latest/library/onewire.html)


## Opdracht

<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht: Meet temperatuur.
<ul style="color: white;">
<li>Kies een sensor, bestuur deze adhv opzoekingen.</li>
<li>Bouw de schakeling.</li>
<li>Schrijf een programma voor de microcontroller dat de waarde van de sensor opvraagt.</li>
<li>Toon de werking aan de docent.</li>
</ul>
</p>
</div>