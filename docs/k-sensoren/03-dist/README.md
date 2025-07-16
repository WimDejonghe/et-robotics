# Afstandssensoren

Afstandssensoren meten de afstand tot een object. Veelgebruikte types zijn ultrasone sensoren (zoals de HC-SR04) en infraroodsensoren. Hieronder vind je informatie en voorbeelden voor beide soorten.

## Ultrasone afstandssensor (HC-SR04)

**Werking:**  
De HC-SR04 zendt ultrasone geluidsgolven uit en meet de tijd tot de echo terugkomt. Hiermee wordt de afstand tot een object berekend.

**Aansluitschema:**
- VCC → 5V op ESP32
- GND → GND op ESP32
- Trig → GPIO 5 (voorbeeld)
- Echo → GPIO 18 (voorbeeld, via spanningsdeler naar 3.3V!)

**MicroPython voorbeeld:**
```python
from machine import Pin, time_pulse_us
import time

TRIG_PIN = 5
ECHO_PIN = 18

trig = Pin(TRIG_PIN, Pin.OUT)
echo = Pin(ECHO_PIN, Pin.IN)

def meet_afstand():
    trig.value(0)
    time.sleep_us(2)
    trig.value(1)
    time.sleep_us(10)
    trig.value(0)
    duur = time_pulse_us(echo, 1, 30000)
    afstand = (duur / 2) / 29.1
    return afstand

while True:
    afstand = meet_afstand()
    print("Afstand: {:.2f} cm".format(afstand))
    time.sleep(1)
```
> **Let op:** De echo-pin van de HC-SR04 geeft 5V uit. Gebruik een spanningsdeler naar 3.3V voor de ESP32!

---

## Infrarood afstandssensor (bijv. Sharp GP2Y0A21YK0F)

**Werking:**  
Een infrarood afstandssensor stuurt een IR-signaal uit en meet de reflectie. De uitgangsspanning varieert met de afstand tot een object.

**Aansluitschema:**
- VCC → 5V op ESP32
- GND → GND op ESP32
- OUT → GPIO 34 (voorbeeld, analoge ingang)

**MicroPython voorbeeld:**
```python
from machine import ADC, Pin
import time

IR_PIN = 34  # Analoge ingang
adc = ADC(Pin(IR_PIN))
adc.atten(ADC.ATTN_11DB)  # Meetbereik tot ~3.3V

def meet_ir_afstand():
    waarde = adc.read()
    # Omzetten naar spanning (optioneel)
    spanning = waarde / 4095 * 3.3
    # Kalibratie nodig voor exacte afstand!
    return waarde, spanning

while True:
    waarde, spanning = meet_ir_afstand()
    print("Sensorwaarde:", waarde, "Spanning: {:.2f} V".format(spanning))
    time.sleep(1)
```
> **Let op:** De uitgang van de sensor is analoog. De relatie tussen spanning en afstand is niet lineair; raadpleeg de datasheet voor kalibratie.

---

## Toepassingen

- Robotnavigatie
- Obstakeldetectie
- Automatische deuren
- Afstand meten tot objecten in slimme systemen

## Opdracht

<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht: Meet afstand.
<ul style="color: white;">
<li>Kies een sensor, bestuur deze adhv opzoekingen.</li>
<li>Bouw de schakeling.</li>
<li>Schrijf een programma voor de microcontroller dat de waarde van de sensor opvraagt.</li>
<li>Stuur een LED aan wanneer de afstand te klein is (denk aan automatische achteruitrij sensoren bij een wagen).</li>
<li>Toon de werking aan de docent.</li>
</ul>
</p>
</div>