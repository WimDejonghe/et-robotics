---
mathjax:
  presets: '\def\lr#1#2#3{\left#1#2\right#3}'
---

# Digitale Logische Poorten

Logische poorten zijn de bouwstenen van digitale elektronica. Elke logische poort voert een eenvoudige logische digitale bewerking uit op een of meer invoersignalen en produceert een enkele uitvoer.

## 0. YES Poort

De **YES**-poort neemt één invoer en stuurt deze naar de uitgang. Als de invoer `1` is, is de uitvoer `1` en omgekeerd.

**Waarheidstabel:**

| Invoer (A) | Uitvoer (X) |
|------------|-------------|
| 0          | 0           |
| 1          | 1           |

**Symbol:**

![example image](./images/yes.png "Het symbool van een YES-poort.")



## 1. NOT Poort (Inverter)

De **NOT**-poort (ook wel een **inverter** genoemd) neemt één invoer en keert deze om. Als de invoer `1` is, is de uitvoer `0` en omgekeerd.

**Waarheidstabel:**

| Invoer (A) | Uitvoer (X) |
|------------|-------------|
| 0          | 1           |
| 1          | 0           |

**Symbol:**

![example image](./images/not.png "Het symbool van een NOT-poort.")


## 2. AND Poort

De **AND**-poort geeft alleen een `1` als ALLE ingangen `1` zijn.

**Waarheidstabel:**

| Invoer (A) | Invoer (B) | Uitvoer (X) |
|------------|------------|-------------|
| 0          | 0          | 0           |
| 0          | 1          | 0           |
| 1          | 0          | 0           |
| 1          | 1          | 1           |

**Symbol:**

![example image](./images/and.png "Het symbool van een AND-poort.")

## 3. OR Poort

De **OR**-poort geeft een `1` als ten minste één invoer `1` is.

**Waarheidstabel:**

| Invoer (A) | Invoer (B) | Uitvoer (X) |
|------------|------------|-------------|
| 0          | 0          | 0           |
| 0          | 1          | 1           |
| 1          | 0          | 1           |
| 1          | 1          | 1           |

**Symbol:**

![example image](./images/or.png "Het symbool van een OR-poort.")


## 4. NAND Poort

De **NAND**-poort is het tegenovergestelde van de AND-poort. De uitvoer is `0` alleen als ALLE ingangen `1` zijn.

**Waarheidstabel:**

| Invoer (A) | Invoer (B) | Uitvoer (X) |
|------------|------------|-------------|
| 0          | 0          | 1           |
| 0          | 1          | 1           |
| 1          | 0          | 1           |
| 1          | 1          | 0           |

**Symbol:**

![example image](./images/nand.png "Het symbool van een NAND-poort.")


## 5. NOR Poort

De **NOR**-poort is het tegenovergestelde van de OR-poort. De uitvoer is `1` alleen als ALLE ingangen `0` zijn.

**Waarheidstabel:**

| Invoer (A) | Invoer (B) | Uitvoer (X) |
|------------|------------|-------------|
| 0          | 0          | 1           |
| 0          | 1          | 0           |
| 1          | 0          | 0           |
| 1          | 1          | 0           |

**Symbol:**

![example image](./images/nor.png "Het symbool van een NOR-poort.")


## 6. XOR Poort

De **XOR**-poort (Exclusieve OF) geeft een `1` wanneer de ingangen verschillend zijn.

**Waarheidstabel:**

| Invoer (A) | Invoer (B) | Uitvoer (X) |
|------------|------------|-------------|
| 0          | 0          | 0           |
| 0          | 1          | 1           |
| 1          | 0          | 1           |
| 1          | 1          | 0           |

**Symbol:**

![example image](./images/xor.png "Het symbool van een XOR-poort.")


## 7. XNOR Poort

De **XNOR**-poort (Exclusieve-NOR) is het tegenovergestelde van de XOR-poort. De uitvoer is `1` wanneer de ingangen gelijk zijn.

**Waarheidstabel:**

| Invoer (A) | Invoer (B) | Uitvoer (X) |
|------------|------------|-------------|
| 0          | 0          | 1           |
| 0          | 1          | 0           |
| 1          | 0          | 0           |
| 1          | 1          | 1           |

**Symbol:**

![example image](./images/xnor.png "Het symbool van een XNOR-poort.")

---

Dit zijn de basislogische poorten die worden gebruikt in digitale schakelingen. Elke poort kan worden gecombineerd om complexe logische functies te creëren.

***
<div style="background-color:darkgreen; text-align:left; vertical-align:left; padding:15px;">
<p style="color:lightgreen; margin:10px">
Opdracht: Maak van iedere logische poort functie een werkend programma op de microcontroller zodat drukknoppen gebruikt worden als input voor een logische poort en een LED de uitgang. Combineer alle poorten in één en hetzelfde programma. Met twee drukknoppen die alle inputs van alle poorten bedienen en er zijn LED's op de shield voldoende om voor iedere output een LED aan te sturen. Je mag hier geen gebruik maken van IF-statements.

</p>
<p style="color:white; margin:10px">Je moet ervan uitgaan dat de drukknoppen zo worden geinterpreteerd dat een druk op een knop een 1 veroorzaakt in de verwerking naar een uitgang van een poort!!!</p>
</div>

***