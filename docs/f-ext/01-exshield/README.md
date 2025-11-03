---
mathjax:
  presets: '\def\lr#1#2#3{\left#1#2\right#3}'
---

# De ESP32 Extension shield

Om het programmeren wat eenvoudiger te maken gaan we gebruik maken van een ESP32 shield.

![example image](./images/shield1.png "De ESP32 shield.")

De shield heeft 4 drukknoppen, 8 leds, een trimmer en een connector om SPI- en I²C componenten aan te sluiten. In de volgende tabel is weergegeven wat de naam en op welke IO-pin deze zijn aangesloten. De connector met de SPI en I²C aansluitingen zijn niet weergegeven maar deze kan een goede technicus uit het schema van de volgende figuur halen.


| Naam | IO-pin |
| ----------- |:------------:|
| SW1| 39 | 
| SW2| 34| 
| SW3| 25 | 
| SW4| 26 | 
| LED1| 21 | 
| LED2| 14 | 
| LED3| 32 | 
| LED4| 15 | 
| LED5| 33 | 
| LED6| 27 | 
| LED7| 12 | 
| LED8| 13 | 
| POT| A4 | 

![example image](./images/schema.png "De ESP32 shield.")

## Testen van de ESP32 shield voor de Adafruit Huzzah32 feather

Onderstaande code kan men gebruiken om de shield te testen.

```python
from machine import Pin, ADC
from time import sleep
led1 = Pin(21, Pin.OUT)
led2 = Pin(14, Pin.OUT)
led3 = Pin(32, Pin.OUT)
led4 = Pin(15, Pin.OUT)
led5 = Pin(33, Pin.OUT)
led6 = Pin(27, Pin.OUT)
led7 = Pin(12, Pin.OUT)
led8 = Pin(13, Pin.OUT)

sw1 = Pin(39, Pin.IN)
sw2 = Pin(34, Pin.IN)
sw3 = Pin(25, Pin.IN)
sw4 = Pin(26, Pin.IN)

potentiometer = ADC(36)
potWaarde = 0

while True:
  #put your main code here, to run repeatedly:
  led1.value( not sw1.value())
  led2.value( not sw2.value())
  led3.value( not sw3.value())
  led4.value( not sw4.value())
  led5.value(sw1.value())
  led6.value(sw2.value())
  led7.value(sw3.value())
  led8.value(sw4.value())
  

  potWaarde = potentiometer.read()
  print("De potentiometer waarde = ", potWaarde)
 

```

<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht1: Maak een programma waarbij de spanning op de analoge ingang A2 wordt gelezen en doorgestuurd wordt naar een seriële monitor (kan je gewoon uitvoeren door gebruik te maken van het "print"-commando) op de PC. Door te printen kan je de waarde zien in Thonny. <br>
Maak gebruik van de ESP32-shield voor de potentiometer.
</p>
</div>

***

<html>
<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht2: Maak een programma waarbij de spanning op de analoge ingang A4 (is dit hetzelfde als pin36?, zoek dit op) wordt gelezen en 4 leds stuurt afhankelijk van de gemeten waarde volgens onderstaande tabel<br>
Maak gebruik van de ESP32-shield.
</p>
<table>
  <tr bgcolor="Gray">
    <th>waarde A4</th>
    <th>LED1</th>
    <th>LED2</th>
    <th>LED3</th>
    <th>LED4</th>
  </tr>
  <tr bgcolor="DimGray">
    <td>X&lt=819 (&lt=20%)</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr bgcolor="Gray">
    <td>819 &lt X &lt= 1638 <br> 20% &lt X &lt= 40%</td>
    <td>1</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr bgcolor="DimGray">
    <td>1638 &lt X &lt= 2457 <br> 40% &lt X &lt= 60%</td>
    <td>1</td>
    <td>1</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr bgcolor="Gray">
    <td>2457 &lt X &lt= 3276 <br> 60% &lt X &lt= 80%</td>
    <td>1</td>
    <td>1</td>
    <td>1</td>
    <td>0</td>
  </tr>
  <tr bgcolor="DimGray">
    <td>3276 &lt X &lt= 4095 <br> 80% &lt X &lt= 100%</td>
    <td>1</td>
    <td>1</td>
    <td>1</td>
    <td>1</td>
  </tr>
</table>
</div>
</html>

***

::: tip
Bij bepaalde opdrachten kan het interessant zijn dat een unsigned integer 0-255 (een char type dus), data, moet omgezet worden naar 8bits. Ziehier een methode die dit voor u kan doen.
:::

```python
def ToonLEDS (data):
    #print(bin(data)) zou een andere manier zijn
    #is een uitdaging voor de student
    if (int(data/128)%2 != 0): led8.value(1) #Set LED8   
    else: led8.value(0) #Reset LED8   
    if (int(data/64)%2 != 0): led7.value(1) #Set LED7   
    else: led7.value(0) #Reset LED7   
    if (int(data/32)%2 != 0): led6.value(1) #Set LED6   
    else: led6.value(0) #Reset LED6   
    if (int(data/16)%2 != 0): led5.value(1) #Set LED5   
    else: led5.value(0) #Reset LED5   
    if (int(data/8)%2 != 0): led4.value(1) #Set LED4   
    else: led4.value(0) #Reset LED4   
    if (int(data/4)%2 != 0): led3.value(1) #Set LED3   
    else: led3.value(0) #Reset LED3   
    if (int(data/2)%2 != 0): led2.value(1) #Set LED2   
    else: led2.value(0) #Reset LED2   
    if (data%2 != 0): led1.value(1) #Set LED1   
    else: led1.value(0) #Reset LED1

```

<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht3: Zorg dat je met voorgaande methode een analoge ingang inleest, en dat die waarde binair wordt weergegeven op de 8 LEDS (256 combinaties).
</p>
</div>

***

<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht4: Knipperen van een LED vlug - traag op basis van toestand van een digitale input.
</p>
</div>

***

<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht5: Laat een LED bij de start 10 keer snel knipperen en daarna oneindig keer op een lage snelheid knipperen
</p>
</div>

***

<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht6: Laat een LED knipperen met een oplopende snelheid (repetitief). Als dit werkt, bij max snelheid terug aflopende snelheid en dit ook repetitief. 
</p>
</div>

***

<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht7: Wat is de minimum delay tijd bij het knipperen van een LED zodat we het net niet meer kunnen waarnemen. Wat is dan de knipperfrequentie en wat is de knipperperiode? Meet met de oscilloscoop. Bereken en bepaal het verband met uw delay tijden. 
</p>
</div>

***

<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht8: Wijzig bij het onzichtbaar knipperen (door de hoge knipperfrequentie) de AAN-tijd tot 10% van de knipperperiode. Wat is dit in seconden? wat is dan de UIT-tijd in seconden? Let wel de knipperfrequentie mag niet wijzigen. Meet met de oscilloscoop. Bereken en bepaal het verband met uw delay tijden. 
</p>
</div>

***