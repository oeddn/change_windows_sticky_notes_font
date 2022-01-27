# Schriftart der Windows Kurznotizen ändern

Die Standardschriftart der Windows Kurznotizen ist mit einfachen Bordmitteln nicht einstellbar und optisch zumindest, sagen wir, Geschmackssache. Hier beschreibe ich wie man diese ändern kann. Der Modus ist dabei eigentlich ganz einfach: Da Windows nur eine bestimmte Schriftart für die Kurznotizen vorsieht, verändern wir unsere Wunschschriftart so dass sie von Windows als eben diese Standardschriftart akzeptiert wird. Dazu passen wir die Namensparameter innerhalb des TrueTypeFonts an. Eine einfache Umbenennung der Fontdateien reicht dazu nämlich leider nicht aus.

**DISCLAIMER: ausschließlich getestet unter Windows 8.1!**

**DISCLAIMER: die Schriftart wird systemweit geändert, falls also genau diese Schriftart an anderer Stelle weiterbenutzt werden soll ist dieses Verfahren ungeeignet.**

**DISCLAIMER: die fonttools wurden nicht direkt unter Windows genutzt, sondern auf Ubuntu Linux. Die Installation von Python und den fonttools unter Windows ist also ungetestet!**

Insgesamt müssen wir die folgenden Schritte ausführen:

- [benötigte Tools installieren](#benötigte-tools-installieren)
- [alte Schriftart sichern und löschen](#alte-schriftart-sichern-und-löschen)
- [gewünschte Schriftart auswählen und kopieren](#gewünschte-schriftart-auswählen-und-kopieren)
- [beide Schriftarten in ein änderbares Format übertragen](#beide-schriftarten-in-ein-änderbares-format-übertragen)
- [Fontnamen übertragen](#fontnamen-übertragen)
- [neue Schriftart zurück nach TrueType wandeln](#neue-schriftart-zurück-nach-truetype-wandeln)
- [erstellte Schriftart installieren](#erstellte-schriftart-installieren)

Das ist ein ganzes Stück Arbeit nur zum Ändern einer Schriftart, für mich hat sich dieser jedoch auf jeden Fall gelohnt.

## benötigte Tools installieren
Um die TrueType Schriftarten in ein les- und änderbares Format zu konvertieren werden die `fonttools` genutzt. Für die Version 4.x wird Python 3.6 oder neuer vorausgesetzt. Die aktuelle Version von Python kann [hier](https://www.python.org/downloads/windows/) heruntergeladen und mit den enthaltenen Anweisungen installiert werden. Die `fonttools` installiert man dann via `pip3`, dem Python Paketmanager für die Python Version 3, auf der Windows Kommandozeile. Die Kommandozeile öffnet man mit

`Win`+`r` -> `cmd.exe` eingeben -> mit `Enter` bestätigen

Dann mit folgendem Befehl die Installation der `fonttools` starten:
```
pip3 install fonttools
```

## alte Schriftart sichern und löschen
- zunächst ein Arbeitsverzeichnis erstellen, zum Beispiel `tmpfont` in den Eigenen Dateien des Benutzers
- dann den Windows Schriftartenordner aufrufen, dazu
    - die Tastenkombination `Win`+`r` drücken, es öffnet sich das Fenster `Ausführen`
    - in das Textfeld `fonts` eingeben und mit `OK` bestätigen
- nach Schriftart "*Segoe Print Standard*" suchen, diese dann
    - mit Rechtsklick kopieren
    - in das Arbeitsverzeichnis (`tmpfont`) ebenfalls mit Rechtsklick einfügen
    - aus dem Schriftartenordner löschen
      >Hierzu sind Adminrechte erforderlich, den entsprechenden Dialog mit `OK` bestätigen

>beim Kopieren erscheinen mehrere Dateien ähnlichen Namens, z.B. für die regular und bold Variante der Schriftart (`segoepr.ttf` und `segoeprb.ttf`); die unten genannten Schritte müssen für jede einzelne der Dateien angewendet werden!

## gewünschte Schriftart auswählen und kopieren
Jetzt kann aus beliebiger Quelle eine für den eigenen Bedarf geeignete TrueType Schriftart ausgewählt (für den Rest der der Beschreibung verwende ich `consolas.ttf` und `consolasb.ttf`) und in das Arbeitsverzeichnis (`tmpfont`) kopiert werden. Diese kann ebenfalls aus dem Windows Schriftartenordner ausgewählt und kopiert werden, darf dann aber nicht gelöscht werden! Wichtig ist darauf zu achten, dass für die **regular** und **bold** Variante der Originalschriftart eine entsprechende Variante der Wunschschriftart existiert. Grundsätzlich ist die Vorgehensweise aber für alle TrueType Schriftarten geeignet.

## beide Schriftarten in ein änderbares Format übertragen
Um die Informationen aus der Originalschriftart auslesen und in unserer Wunschschriftart anpassen zu können, müssen wir zunächst die TrueType Dateien in ein les- und änderbares Format konvertieren (die Befehle in diesem Abschnitt werden alle auf der Windows Kommandozeile eingegeben).

Die Originalschriftart in ein lesbares Format umwandeln (erzeugt `segoepr.ttx` und `segoeprb.ttx`):
```
ttx segoepr.ttf
ttx segoeprb.ttf
```
Gleiches mit der gewünschten Schriftart (erzeugt `consolas.ttx` und `consolasb.ttx`):
```
ttx consolas.ttf
ttx consolasb.ttf
```
Jetzt die Originalschriftarten umbenennen, damit die neu erstellten Schriftarten unter dem gleichen Namen abgespeichert werden können:
```
ren segoepr.ttf __segoepr.ttf
ren segoeprb.ttf __segoeprb.ttf
```

## Fontnamen übertragen
Die erzeugten `.ttx` Dateien sind im XML-Format, und somit les- und durchsuchbar. Um Windows die ausgewählte Schrift als Standard für die Kurznotizen unterzuschieben, muss der von `<name>` und `</name>` eingeschlossene Bereich in der XML-Datei der Wunschschriftart durch den der jeweiligen Originalschriftart ersetzt werden, und zwar jeweils für **regular** und **bold**. Also:
- ttx-Datei der **Wunsch**schriftart (`consolas.ttx`) mit beliebigem Texteditor (z.B. `Notepad.exe`) öffnen
- nach `<name>` suchen (`Notepad.exe`: `Strg`+`F` drücken, Suchtext eingeben, `Enter` drücken)
- den kompletten Bereich zwischen `<name>` und `</name>` löschen
- in einem zweiten Texteditorfenster die ttx-Datei der **Original**schriftart (`segoepr.ttx`) öffnen
- nach `<name>` suchen
- den kompletten Bereich zwischen `<name>` und `</name>` markieren und in die Zwischenablage kopieren (`Notepad.exe`: `Strg`+`c` drücken)
- den kopierten Text in der Zwischenablage in den Bereich zwischen `<name>` und `</name>` im Texteditor der **Wunsch**schriftart (`consolas.ttx`) einfügen (`Notepad.exe`: den Cursor an das Ende von `<name>` positionieren, dann `Strg`+`c` drücken)
- die geänderte Datei der **Wunsch**schriftart (`consolas.ttx`) speichern (`Notepad.exe`: `Strg`+`s` drücken)
Die beiden Fenster der Texteditoren können jetzt geschlossen werden. Das ganze muss jetzt noch für die **bold** Variante der Schriftart (`segoeprb.ttf` und `consolasb.ttf`) durchgeführt werden

## neue Schriftart zurück nach TrueType wandeln
Damit unsere neue Schriftart mit alter Kennung wieder in das System installiert werden kann muss diese wieder in einen TrueTypeFont umgewandelt werden. Auf der Kommandozeile geschieht das mit den folgenden Befehlen:
```
ttx -o segoepr.ttf consolas.ttx
ttx -o segoeprb.ttf consolasb.ttx
```
Die Dateien `segoepr.ttf` und `segoeprb.ttf` beinhalten jetzt unsere angepasste Wunschschriftart!

## erstellte Schriftart installieren
Die geänderten Dateien werden jetzt einfach aus unserem Arbeitsverzeichnis in den Windows Schriftartenordner verschoben:
- im Arbeitsverzeichnis (`tmpfont`) die Dateien `segoepr.ttf` und `segoeprb.ttf` markieren, dann `Strg`+`x` drücken
- in den Windows Schritfartenordner wechseln und `Strg`+`v` drücken
  >Hierzu sind Adminrechte erforderlich, den entsprechenden Dialog mit `OK` bestätigen
- nach einem Neustart des Computers wird die neue Schriftart für die Kurznotizen verwendet

>um zu einem späteren Zeitpunkt die Originalschriftart wiederherstellen zu können, empfiehlt es sich die beiden gesicherten Dateien `__segoepr.ttf` und `__segoeprb.ttf` zu sichern bzw. im Arbeitsverzeichnis zu lassen!

