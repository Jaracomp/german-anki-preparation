# Grammar Templates Reference

Full examples for every PoS, loaded when needed for edge cases.

---

## Nomen

Format: `artikel Wort, Genitiv-Endung, Plural-Endung`

```
der Hund, -es, ¨-e          → Gen: des Hundes | Pl: die Hunde (але тут без умлауту!)
der Arzt, -es, ¨-e           → Gen: des Arztes | Pl: die Ärzte
die Frau, -, -en             → Gen: der Frau | Pl: die Frauen
das Auto, -s, -s             → Gen: des Autos | Pl: die Autos
der Vater, -s, ¨             → Gen: des Vaters | Pl: die Väter (тільки умлаут)
das Kind, -(e)s, -er         → Gen: des Kindes | Pl: die Kinder
der Mensch, -en, -en         → n-Deklination: des Menschen | Pl: die Menschen
der Student, -en, -en        → n-Deklination
das Herz, -ens, -en          → Змішана: des Herzens | Pl: die Herzen
die Leute (pl), -, -         → Тільки множина
die Kosten (pl), -, -        → Тільки множина
```

Правила Gen-закінчення:
- `-(e)s` — більшість m/n іменників
- `-` — f іменники та незмінні
- `-en` — n-Deklination (всі відмінки крім Nom Sg = -en)
- `-ens` — das Herz (особливий випадок)

Правила Pl-закінчення:
- `-` = Pl = Nom Sg (без змін)
- `¨` = тільки умлаут (Mutter → Mütter)
- `¨-e` = умлаут + -e (Arzt → Ärzte)
- `-er`, `¨-er`, `-en`, `-n`, `-s` = стандартні закінчення

---

## Verb

Format: `Infinitiv (3sg-якщо-Vokalwechsel, Präteritum, hat/ist Partizip II)`

### Регулярні (слабкі):
```
kaufen (kaufte, hat gekauft)
machen (machte, hat gemacht)
arbeiten (arbeitete, hat gearbeitet)
öffnen (öffnete, hat geöffnet)
```

### Нерегулярні (сильні) без Vokalwechsel у Präsens:
```
fahren (fuhr, ist gefahren)
schreiben (schrieb, hat geschrieben)
bleiben (blieb, ist geblieben)
gehen (ging, ist gegangen)
kommen (kam, ist gekommen)
```

### Нерегулярні з Vokalwechsel у Präsens (3sg):
```
fahren (fährt, fuhr, ist gefahren)       a → ä
laufen (läuft, lief, ist gelaufen)       au → äu
lesen (liest, las, hat gelesen)          e → ie
geben (gibt, gab, hat gegeben)           e → i
nehmen (nimmt, nahm, hat genommen)       e → i (+ подвоєння m)
essen (isst, aß, hat gegessen)           e → i
sehen (sieht, sah, hat gesehen)          e → ie
tragen (trägt, trug, hat getragen)       a → ä
schlafen (schläft, schlief, hat geschlafen)  a → ä
```

3sg додається тільки коли є зміна голосної. `kauft`, `macht` — не додавати.

### Модальні дієслова:
```
können (kann, konnte, hat gekonnt)
müssen (muss, musste, hat gemusst)
dürfen (darf, durfte, hat gedurft)
sollen (soll, sollte, hat gesollt)
wollen (will, wollte, hat gewollt)
mögen (mag, mochte, hat gemocht)
```

### Розділювані (trennbar):
```
an|rufen (rief an, hat angerufen)
ab|fahren (fuhr ab, ist abgefahren)
auf|machen (machte auf, hat aufgemacht)
ein|kaufen (kaufte ein, hat eingekauft)
mit|nehmen (nimmt mit, nahm mit, hat mitgenommen)    ← + Vokalwechsel
```

Präteritum і P.II пишемо в злитій формі (як в словнику).

### Зворотні (reflexiv):
```
sich freuen (freute sich, hat sich gefreut)
sich erinnern (erinnerte sich, hat sich erinnert)
sich waschen (wäscht sich, wusch sich, hat sich gewaschen)   ← + Vokalwechsel
```

### З Rektion:
```
warten (wartete, hat gewartet); auf +Akk
sich freuen (freute sich, hat sich gefreut); auf +Akk / über +Akk
denken (dachte, hat gedacht); an +Akk
sprechen (spricht, sprach, hat gesprochen); über +Akk / von +Dat
helfen (hilft, half, hat geholfen); +Dat
gehören (gehörte, hat gehört); +Dat
```

Rektion через крапку з комою. Якщо кілька варіантів — через ` / `.

---

## Adjektiv

Format: `Adj Rektion-якщо-є (Komparativ, Superlativ)`

### Регулярні:
```
schnell (schneller, am schnellsten)
interessant (interessanter, am interessantesten)
müde (müder, am müdesten)
dunkel (dunkler, am dunkelsten)           ← e випадає
teuer (teurer, am teuersten)              ← e випадає
```

### Нерегулярні:
```
gut (besser, am besten)
viel (mehr, am meisten)
hoch (höher, am höchsten)
nah (näher, am nächsten)
gern → тільки як прислівник (lieber, am liebsten)
```

### З Rektion:
```
stolz auf +Akk (stolzer, am stolzesten)
zufrieden mit +Dat (zufriedener, am zufriedensten)
abhängig von +Dat (abhängiger, am abhängigsten)
interessiert an +Dat (interessierter, am interessiertesten)
```

Переклад для Adj+Rektion включає підказку:
- `stolz auf +Akk` → `гордий (кимось/чимось)`
- `zufrieden mit +Dat` → `задоволений (кимось/чимось)`

---

## Adverb

Зазвичай просто слово. Компаратив тільки якщо нерегулярний або існує:

```
leider
trotzdem
deshalb
gern (lieber, am liebsten)
bald (eher, am ehesten)
oft (öfter/häufiger, am öftesten/häufigsten)
```

Adverb не має Rektion (на відміну від Adjektiv).

---

## Präposition

```
mit +Dat
bei +Dat
von +Dat
zu +Dat
aus +Dat
nach +Dat
seit +Dat        ← час
durch +Akk
für +Akk
gegen +Akk
ohne +Akk
um +Akk
wegen +Gen
trotz +Gen
während +Gen
statt +Gen
```

Wechselpräpositionen (in, an, auf, über, unter, vor, hinter, neben, zwischen):
```
in +Dat/+Akk [wo/wohin]
an +Dat/+Akk [wo/wohin]
auf +Dat/+Akk [wo/wohin]
über +Dat/+Akk [wo/wohin]
unter +Dat/+Akk [wo/wohin]
vor +Dat/+Akk [wo/wohin]
hinter +Dat/+Akk [wo/wohin]
neben +Dat/+Akk [wo/wohin]
zwischen +Dat/+Akk [wo/wohin]
```

---

## Konjunktion

Три типи — впливають по-різному на порядок слів:

### [koord] — координуючий:
Порядок слів не змінюється. Підмет лишається після дієслова якщо вже так стоїть.
```
und [koord]        і
aber [koord]       але
oder [koord]       або
denn [koord]       бо (причина, як пояснення)
sondern [koord]    а (після заперечення: nicht X, sondern Y)
```

### [sub] — підрядний:
Дієслово підрядного речення йде в самий кінець.
```
weil [sub]         тому що
dass [sub]         що
obwohl [sub]       хоча
wenn [sub]         коли / якщо
als [sub]          коли (минуле, одноразово)
ob [sub]           чи (непряме питання)
damit [sub]        щоб (мета)
bevor [sub]        перш ніж
nachdem [sub]      після того як
seitdem [sub]      з тих пір як
```

### [adv] — прислівниковий:
Стоїть на початку речення → дієслово залишається на 2 місці, підмет іде після.
```
deshalb [adv]      тому
trotzdem [adv]     незважаючи на це
dennoch [adv]      однак
außerdem [adv]     крім того
deswegen [adv]     через це
```

Переклад сполучника включає граматичну підказку:
- `weil [sub]` → `тому що [дієслово — в кінець підрядного речення]`
- `deshalb [adv]` → `тому [дієслово залишається другим]`

---

## Partikel (Modalpartikeln)

Модальні частки не мають прямого перекладу. Завжди включати приклад речення.
Переклад = пронумеровані значення з мікроприкладами.

```
doch [partikel]<br>Das ist doch klar!
→ 1) підкреслення очевидного: "Та це ж ясно!" 2) заперечення у відповідь: Doch! = "Та ні!"

mal [partikel]<br>Komm mal her.
→ пом'якшення прохання: "Підійди-но сюди"

ja [partikel]<br>Das ist ja interessant!
→ підкреслення з відтінком здивування: "Це ж бо цікаво!"

schon [partikel]<br>Das wird schon klappen.
→ заспокоєння/впевненість: "Та все вийде"

denn [partikel]<br>Was ist denn los?
→ посилення питання, зацікавленість: "Що ж трапилось?"
```
