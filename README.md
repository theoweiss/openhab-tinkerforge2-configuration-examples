# openhab-tinkerforge2-configuration-examples

## org.openhab.binding.tinkerforge-2.5.0-11-SNAPSHOT

__Ab org.openhab.binding.tinkerforge-2.5.0-11-SNAPSHOT wird eine openHAB-2.5-Snapshot Installation benötigt__.


### Neue Bricklets

* Bricklet LCD128x64

## Bricklet LCD128x64

### Channels

* Display (textcommand)
* Button 0-11 (system.rawbutton => TriggerChannel)
* Slider 0-5 (DecimalValue 0-42)
* Tab 0-9 (TriggerChannel)

#### Display

Dem Display Channel kann man StringType Commands schicken um Text anzuzeigen, dies funktioniert z.B. auch aus dem PaperUI.

Die Startzeile und die Startspalte wird mit "{Zeilennummer,Spaltenummer}" gesetzt, dann kommt der anzuzeigende Text (siehe auch writeLine in der TF Doku https://www.tinkerforge.com/en/doc/Software/Bricklets/LCD128x64_Bricklet_Java.html#basic-functions). Es gibt die Zeilen 0-7 und die Spalten 0-21.

Ein Beispiel:

```
{0,0}Hallo TF
```

oder 

```
{7,18}Ende
```

#### Button / Slider / Tab

Mit der Action setGUIButton fügt ihr einen Button hinzu. Der Button Index (erstes Argument) ist der Channel Index. Also Index 0 bedeutet Channel Button0, Index 1 bedeutet Channel Button1 usw.. Die Button-Aktionen werden an den jeweiligen Button Channel gesendet.

Für Slider und Tab verhält sich das äquivalent. Die relevanten Actions sind hier: setGUISlider und setGUITabText/setGUITabIcon.

Das grundsätzliche Vorgehen ist: mit einer Action die GUI-Elemente erzeugen und dann die Aktionen an den GUI-Elementen über die Channel-Events verfolgen.

Das klingt kompliziert, ist es aber nicht.
Mit einer Action den Button erzeugen sobald das LCD-Bricklet ONLINE geht. 

```
     actions.setGUIButton(0, 0, 0, 60, 20, "MeinButton0")
```

Jedesmal wenn dann der Button gedrückt wird bekommst du z.B. ein solches Ereignis:

```
tinkerforge:lcd128x64:09797a8d:H8Z:button0 triggered PRESSED
```

Beim Loslassen des Buttons ein Ereignis:

```
tinkerforge:lcd128x64:09797a8d:H8Z:button0 triggered RELEASED
```

Genau so als wäre es ein "echter" Taster.

### Actions

Es gibt folgende Actions:

* writePixels
* clearDisplay
* writeLine
* drawBufferedFrame
* drawLine
* drawBox
* drawText
* setGUIButton
* removeGUIButton
* setGUISlider
* removeGUISlider
* setGUITabText
* setGUITabIcon
* removeGUITab
* setGUIGraphConfiguration
* setGUIGraphData
* removeGUIGraph
* removeAllGUI

Die Methoden Signaturen und Bedeutungen stimmen mit denen des TinkerForge API überein: siehe https://www.tinkerforge.com/de/doc/Software/Bricklets/LCD128x64_Bricklet_Java.html#basic-functions

Ein Beispiel:

```
     actions.setGUIButton(0, 0, 0, 60, 20, "button0")
```

Die Actions können in Rules verwendet werden. Ein Rule-Beispiel findet ihr hier unter lcd128x64.
