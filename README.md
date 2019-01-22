# Embedded Devices "IoT Gateway"

## Aufgabenstellung
Die detaillierte [Aufgabenstellung](TASK.md) beschreibt die notwendigen Schritte zur Realisierung.

## Verwendung
Wenn man das Template von Michael Borko [Github](https://github.com/mborko/stm32f4-template)
verwendet.
Einfach das Projekt clonen und flashen.
```sh
git clone https://github.com/dlangheiter-tgm/syt4-ek712-iot-gateway-dlangheiter-tgm src/iot-gateway
make clean flash PROJ=src/iot-gateway
```

## Implementierung
Am anfang habe ich den Code vom der Aufgabe [GK7.1.1](https://elearning.tgm.ac.at/mod/assign/view.php?id=47943)
copiert weil ich da die Statemachine schon geschrieben hatte.
Dies habe ich dann so verändert das es nicht `HAL_Delay();` funcktioniert
sondern den Systick verwendet. Danach habe ich ein Array erstellt
wo ich alle Statuscodes speichert. Dann habe ich den Outputpin
konfiguriert und versucht eine Signal auszugeben. Dies hat eine lange
Zeit nicht funktioniert weil ich vergessen habe die Clock für die
GPIO Bank nicht aktiviert. Also vergessen `__HAL_RCC_GPIOC_CLK_ENABLE();`
aufzurufen. Danach hat das ausgeben eines Status coded funktionert.
Dies habe ich mithilfe eines LogikAnalyzer überprüft.

Um nicht in einem Sendens eines Codes plöztlich auf einen anderen zu
wechseln gitbt es zwei Variablen `state` und `wantState`. Von der
Statemachine wird der wantState gesetzt damit wenn der Code fertig
übermittelt wurde der neue Status gesendet werden kann.

Das senden funktioniert ganz einfach und basiert darauf das Systick jede
Millisekunde aufgerufen wird. Das heißt er zählt bis 5 und ändert dann
den Status des ausganges. Jedes mal wenn er 3 mal den Ausgang gesetzt
hat holt er sich den Status von `wantState`.

Zur leichteren verwenden von `state` und `setState` habe ich ein Enum
definiert in denen die Stati mit Namen drinnen stehen.

## Quellen
Enums: https://www.geeksforgeeks.org/enumeration-enum-c/