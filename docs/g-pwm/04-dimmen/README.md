---
mathjax:
  presets: '\def\lr#1#2#3{\left#1#2\right#3}'
---

# PWM voorbeeld dimmen van een LED

Het voorbeeldprogramma laat de helderheid van een led regelen door middel van een potentiometer. 

```python
from machine import Pin, ADC, PWM
from time import sleep

led1_pwm = PWM(Pin(21), freq=200)
potentiometer = ADC(Pin(36), atten=ADC.ATTN_11DB)

while True:
    potWaarde = potentiometer.read() #leest 0-4095
    print ('De analoge waarde =', potWaarde)
    led1_pwm.duty(int(potWaarde/4)) #duty 0-1024
```

Een ander voorbeeld laat de LED in helderheid toenemen tot maximum en terug afnemen tot nul, en dit repetitief.

```python
from machine import Pin, ADC, PWM
from time import sleep

led1_pwm = PWM(Pin(21), freq=200)

while True:
    for x in range(1023):
        led1_pwm.duty(x)
        sleep(0.001)
    for y in range(1023):
        led1_pwm.duty(1023-y)
        sleep(0.001)

```