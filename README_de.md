# Schriftart der Windows Kurznotizen ändern

Die Standardschriftart der Windows Kurznotizen ist mit einfachen Bordmitteln nicht einstellbar und zumindest, sagen wir, Geschmackssache. Hier beschreiben ich kurz wie man diese ändern kann.

**DISCLAIMER: ausschließlich getestet unter Windows 8.1!**

**DISCLAIMER: die Schriftart wird systemweit geändert, falls also genau diese Schriftart an anderer Stelle weiterbenutzt werden soll ist dieses Verfahren ungeeignet.**

**DISCLAIMER: die fonttools wurden nicht direkt unter Windows genutzt, sondern auf Ubuntu Linux. Die Installation von Python und den fonttools unter Windows ist also ungetestet!**

Der Modus ist dabei eigentlich ganz einfach: Da Windows nur eine bestimmst Schriftart für die Kurznotizen akzeptiert, verändern wir unsere Wunschschriftart so dass sie von Windows als eben diese Standardschriftart akzeptiert wird. Dazu passen wir die Namensparameter innerhalb des TrueTypeFonts an. Eine einfache Umbenennung der Fontdateien reicht dazu nämlich leider nicht aus.

Und das geht so:

    - benötigte Tools installieren
    - alte Schriftart sichern und löschen
    - gewünschte Schriftart auswählen und kopieren
    - beide Schriftarten in ein änderbares Format übertragen
    - Namensparameter übertragen
    - neue Schriftart zurück nach TrueType wandeln
    - erstellte Schriftart installieren

## benötigte Tools installieren

Um die TrueType Schriftarten in ein les- und änderbares Format zu konvertieren werden die fonttools genutzt. Für die Version 4.x wird Python 3.6 oder neuer gebraucht. Diese kann hier heruntergeladen werden.

Die fonttools installiert man dann via pip auf der Kommandozeile (Win+r -> cmd eingeben -> OK):
~~~
  pip3 install fonttools
~~~
## alte Schriftart sichern und löschen

1. Arbeitsverzeichnis erstellen, zum Beispiel C:\Users\benutzer\tmp\fonttest
2. die Tastenkombination Win+r öffnet das Fenster ‚Ausführen‘
3. in das Textfeld `fonts` eingeben und mit OK bestätigen
4. nach Schriftart „*Segoe Print Standard*“ suchen, kopieren, in das Arbeitsverzeichnis einfügen und aus dem Schriftartenordner löschen (Adminrechte erforderlich!)
5. beim Kopieren erscheinen u.U. mehrere Dateien ähnlichen Namens, z.B. für die regular und bold Variante der Schriftart (`segoepr.ttf segoeprb.ttf`); die unten genannten Schritte sind dann für jede einzelne der Dateien anzuwenden

## gewünschte Schriftart auswählen und kopieren

1. aus beliebiger Quelle eine für den eigenen Bedarf geeignete TrueType Schriftart auswählen (im folgenden Beispiel mit `consolas.ttf` bzw. `consolasb.ttf`) und in das Arbeitsverzeichnis kopieren (z.B. auch einfach aus den Windowsschriftarten wie oben beschrieben, allerdings diese Schriftart nicht löschen!). Darauf achten dass für jeweils die regular und die bold Variante der Originalschriftart eine entsprechende Variante der Wunschschriftart existiert.
2. beide Schriftarten in ein änderbares Format übertragen:

Auf der Kommandozeile die Originalschriftart in ein lesbares Format umwandeln:
~~~
  ttx segoepr.ttf
  ttx segoeprb.ttf
~~~
(erzeugt `segoepr.ttx` und `segoeprb.ttx`). Gleiches mit der gewünschten Schriftart:
~~~
  ttx consolas.ttf
  ttx consolasb.ttf
~~~
(erzeugt `consolas.ttx` und `consolasb.ttx`).

3. Jetzt die Originalschriftarten umbenennen, damit die neu erstellten Schriftarten unter dem gleichen Namen abgespeichert werden können (Befehle für Windows cmd):

~~~
  ren segoepr.ttf __segoepr.ttf
  ren segoeprb.ttf __segoeprb.ttf
~~~

## Fontnamen übertragen

Die erzeugten \*.ttx Dateien sind im XML-Format, und somit les- und durchsuchbar. Um Windows die ausgewählte Schrift als Standard für die Kurznotizen unterzuschieben, muss der von <name> und </name> eingeschlossene Bereich in der Wunschschriftart durch den der jeweiligen Originalschriftart (je einmal für regular und bold) ersetzt werden. Danach wird die geänderte \*.ttx Datei der Wunschschriftart gespeichert.
neue Schriftart zurück nach TrueType wandeln

Damit unsere neue Schriftart mit alter Kennung wieder in das System installiert werden kann muss diese wieder in einen TrueTypeFont umgewandelt werden:
~~~
  ttx -o segoepr.ttf consolas.ttx
  ttx -o segoeprb.ttf consolasb.ttx
~~~
## erstellte Schriftart installieren

1. um zu einem späteren Zeitpunkt die Originalschriftart wiederherstellen zu können, empfiehlt es sich die beiden gesicherten Dateien `__segoepr.ttf` und `__segoeprb.ttf` zu sichern bzw. im Arbeitsverzeichnis zu lassen
2. die umbenannten Dateien in das Schriftartenfenster ziehen (Adminrechte erforderlich!)
3. nachdem Neustart der Anwendung wird der neue Font verwendet
