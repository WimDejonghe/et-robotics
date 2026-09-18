---
mathjax:
  presets: '\def\lr#1#2#3{\left#1#2\right#3}'
---

# GPIO: Output

De esp32 bezit dus een aantal GPIO (General Purpose Input Output) pinnen. Deze pinnen kunnen gebruikt worden als digitale in- of output. Bij een input kan een digitale toestand (0 of 1) worden gelezen door de microcontroller. 


> - Een ingang zal gebruikt worden om door de microcontroller te worden gelezen, hierop zal dus één of andere vorm van sensor of detector worden aangesloten. Meest eenvoudige vorm van zoiets is een drukknop.
> - Een uitgang zal gebruikt worden om door de microcontroller te worden aangestuurd, hierop zal dus één of andere vorm van actuator worden aangesloten. Meest eenvoudige vorm van zoiets is een LED.

## Digitale uitgangen

Een kan dus als een uitgang (output) worden ingesteld in de software. Hierdoor kan de software dan die pin logisch 0 of logisch 1 aansturen. 

> - Wanneer een ouput pin logisch 0 is, dan zal de spanning die op die pin aanwezig is, 0V zijn.
> - Wanneer een ouput pin logisch 1 is, dan zal de spanning die op die pin aanwezig is, 3,3V zijn (= voedingsspanning van de microcontroller).

In digitale systemen worden een aantal termen door elkaar gebruikt om te zeggen dat een toestel **aan** of **uit** staat. Deze verschillende termen worden weergegeven in de volgende tabel.


| 0V | 3,3V |
| ----------- |:------------:|
| Open        | Closed    | 
| Off    | On           | 
| Low  | High   |
| Clear  | Set   |
| Logic 0  | Logic 1   |
| False  | True  |


De termen ‘logisch 0’ en ‘logisch 1’ worden meestal afgekort naar ‘0’ en ‘1’.

## Digitale uitgangspinnen van de EPS32 feather van Adafruit

Wij gebruiken de huzzah32 feather van Adafruit zoals in volgende figuur is weergegeven.

![example image](./images/vsc_28.png "De digitale IO-pinnen van de Adafruit Huzzah ESP32 feather")

![example image](./images/feather_pinouttop.jpg "De digitale IO-pinnen van de Adafruit Huzzah ESP32 feather")

![example image](./images/esp32_2.jpg "De digitale IO-pinnen van de Adafruit Huzzah ESP32 feather")


Enkel de pinnen met de gele labels zoals in bovenste figuur kunnen als digitale uitgangen gebruikt worden, behalve pin 34, 39 en 36 niet. Het maximum aantal is dus 18.

***

Het ontwikkelbord heeft 2 leds aanwezig waarbij de led boven de USB-connecter en naast de aansluiting van de battreij verbonden is met IO-pin 13. Deze kunnen we aansturen van uit de code en kan ingesteld worden als uitgang (1).
De led onder de USB-connector (2) knippert vanaf dat er voedingsspanning aanwezig is.

![example image](./images/onboardled.png "De Leds op de ESP32 Huzzah")

::: warning 
Zorg er altijd voor dat je geen actoren rechtstreeks op een IO-pin van de ESP32 aansluit die een stroom moet sourcen (leveren) die hoger is dan 40mA of een stroom moet sinken (afleiden naar massa) die hoger is dan 20mA.
:::

## pinMode

Als men een IO-pin als uitgang wil gebruiken moet men de pinMode van de IO-pin instellen als uitgang zoals in volgende figuur. Het is het gemakkelijkst om hier de gele pinbenaming te gebruiken zoals in bovenste figuur.
De pinMode van de IO-pin stel je in bij opstart van de controller en dit gebeurt in de setup-methode (instructie moet slechts één keer worden uitgevoerd, vanaf dan weet de µC dat deze pin een OUTPUT is).
Aan de functie *Pin* worden er twee parameters meegegeven tussen haakjes. De eerste parameter is de IO-pin waarover het gaat en de tweede parameter is hoe deze ingesteld moet worden, hier is dit als uitgang (Pin.OUT)

```python
from machine import Pin
led_onboard = Pin(13, Pin.OUT)
#led_onboard is een variabele van het type Pin en krijgt twee parameters
#Er is een onboard led aangesloten op pin13 bij de ESP32 feather
```

## Schrijven naar een digitale output

Een digitale uitgang kan twee waarden aannemen. In de volgende figuur zijn een aantal mogelijkheden om een digitale uitgang een waarde de te geven.
Als de uitgang laag, 0 of als false wordt ingesteld, dan wordt er een 0V spanning op de desbetreffende pin geplaatst (door de inwendige elektronica van de µC).
Als de uitgang hoog, 1 of als true wordt ingesteld dan wordt er een spanning gelijk aan de voedingsspanning op de pin geplaatst. Bij de EPS32 is dit een spanning van 3,3V.
De methode Pin.value(param) heeft één parameter die moeten worden meegegeven tussen haakjes. Die parameter heeft de toestand van die pin aan (hoog of laag, 1 of 0, True of False)is de IO-pin waarover het gaat. Hier is dit *LED* die aangesloten is op IO-pin 13. 

```python
from machine import Pin
from time import sleep
led_onboard = Pin(13, Pin.OUT)
while True:
  led_onboard.value(False)
  sleep(0.5)
  led_onboard.value(True)
  sleep(0.5)
```

::: details
Het commando "Pin.value()", wat echter ook gebruikt om een digtale in- en zelfs een uitgang te lezen, echter dan zonder parameter van True of False.
:::

***

De meest eenvoudige actuator die kan aangestuurd worden met een digitale ouput pin is een LED.

Een LED is een diode met twee aansluitklemmen (Anode en Kathode). Een  LED zal licht afgeven bij volgende voorwaarden:
> - Anode staat op een positief niveau tov de anode (ongeveer 1,5V) 
> - De stroom die van Anode naar Kathode vloeit is ongeveer 20mA.  

Voor detail bij een specifieke LED, zoek in datasheet van die LED naar Uf en If.

Door deze twee waarden is het noodzakelijk om een weerstand in serie te plaatsen met de LED.

We kunnen een LED extern plaatsen en aansturen door gebruik te maken van een breadboard.

Op een breadboard kan dan via draadjes verbindingen en schakelingen worden gebouwd. 

![example image](./images/vtbb3.jpg "Breadboard")

De doorverbindingen van het breadboard ziet er als volgt uit:

![example image](./images/bb1.png "Doorverbindingen breadboard")

## Aansluiten van een led (hardware)

De meeste standaard leds hebben een werkspanning lager dan 3,3V. Als we de led rechtstreeks aansluiten op de microcontroller zal door de te hoge spanning de stroom door de led te groot worden en zal dit de led beschadigen. In het slechtste geval is de led direct defect.
Als de stroom groter zal zijn dan 40mA bij de ESP32, dan zal de uitgang van de controller ook beschadigd worden.

Het schema is te zien in de volgende figuur. In het schema wordt er een weerstand gebruikt van 220Ω.
Gemiddeld staat er over een rode led een spanning van ongeveer 1,6V. Dits wil zeggen dat er over de weerstand een spanning staat van:

$U_R=Voedingsspanning-U_{LED}=3,3V-1,6V=1,7V$

De stroom door de weerstand is dan:
$I = \frac{U_R} {R} = \frac{1,7V} {220\Omega}=7,72mA$

De stroom is kleiner dan de 40mA en geeft voldoende licht als de stroom 7,72mA is

![example image](./images/led1.png "Schema van de rode led verbonden op IO-21")

In de vorige figuur is een led verbonden met het ontwikkelbord door gebruik te maken van een breadbord, en een serieweerstand van 220Ω en enkele draadjes.
De voedingsspanning en de GND heeft men bij vele schema’s veel nodig en daarom gaan we de voedingsspanning 3,3V verbinden met de rode draad met het breadbord dat gebruikt wordt voor de voedingsspanning. De GND wordt met de zwarte draad verbonden met het breadbord met het deel dat gebruikt wordt voor de GND.
IO-pin 21 wordt verbonden met de weerstand van 220Ω. Aan de andere aansluiting van de weerstand wordt de Anode van de led verbonden. De anode is de langste aansluiting. De kathode wordt verbonden met de GND.

:::tip
Er bestaan ook nog andere manieren om een digitale output aan te sturen in MicroPython:
```python
p0 = Pin(0, Pin.OUT)    # create output pin on GPIO0
p0.on()                 # set pin to "on" (high) level
p0.off()                # set pin to "off" (low) level
p0.value(1)             # set pin to on/high
```
:::

## Opdrachten

<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht: Maak een programma om een rode en groene led extern aan te sturen. Sluit de rode led aan op IO16 en de groene led op IO17. De rode led heeft een voedingsspanning nodig van ongeveer 1,6V en de groene led een spanning van 2V. Beide verbruiken een stroom van 10mA. Bereken de twee Rv's. <br>
Beide leds moeten alternerend knipperen waarbij de periode gelijk is aan 1 seconde. Dit wil zeggen als de rode led brandt, dat de groene led niet brandt en omgekeerd. De leds zijn in de periode van 1 seconde even lang aan als uit (= duty cycle = 50%).<br>
Teken het schema, schrijf het programma (met commentaar), test en toon aan de docent.<br>
</p>
</div>

***

<div style="background-color:darkred; text-align:left; vertical-align:left; padding:15px;">
<p style="color:white; margin:10px">
Opdracht: Maak een verkeerslicht.
</p>
</div>

***


