# change Windows sticky notes Font

The standard font for sticky notes on Windows is,
to be polite, really ugly. Here's how to change it.

**DISCLAIMER: I only tested Windows 8.1!**

**DISCLAIMER: the font is changed systemwide, so it will
be changed in every intsance it is used (should not be a problem, 
its ugly in other apps, too)**

**DISCLAIMER: installation and usage of fonttools is untested! (Tested on Ubuntu)**

## Preview on content
1. install required tools
2. backup and delete old font
3. choose and copy new font
4. transform both old an d new font in a changeable format
5. transfer naming parameters
6. retransform new fotn to TrueType
7. install the font created

change_windows_sticky_notes_font

## install required tools

1. To convert the TrueType fonts in a read- and changeable format, w1e use fonttools.
For version 4.x, Pyhton 3.6 or newer is mandatory.
2. To install the fonttools type (after Win+r -> 'cmd' eingeben -> OK

~~~
pip3 install fonttools
~~~

## backup and delete old font

1. create working directory
2. open 'Run..' Windows with Win+r
3. enter fonts in test area and confirm ith OK
4. search for "*Segoe Print Standard*", copy, insert in working directory,
delete from fonts folder (admin privilieges required!)
5. while copying, multiple files with similar names may appear, e.g.
the bold and italic variant of the font (`segoepr.ttf segoeprb.ttf`);
all steps described below have to be performed for every single file!

## choose and copy new font

1. choose a TrueType color from any source (I will use `consolas.ttf` and
 `consolasb.ttf` in the following example) and copy it to the working directory.
(You CAN use fonts from the standard Windows ones, but ALWAYS copy (never delete)
from their original location to your working directory)
2. convert old and new font in changeable format:

On the commandline, convert original font to readable format:
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

3. Now rename the original font, so the newly created ones can be saved under their
names. (commands for Windows cmd.exe):

~~~
  ren segoepr.ttf __segoepr.ttf
  ren segoeprb.ttf __segoeprb.ttf
~~~

## transfer font name

The generated \*.ttx files are in XML format, and therefore easily read- and searchable.
To trick Windows to use the new font for the sticky notes, the area surrounded by
<name> and </name> in the new font has to be replaced with the ones from the old one
(both for regular and bold).
After saving, the modified new font can be reconverted to TrueType. We have to use
the name of the old font. 
~~~
  ttx -o segoepr.ttf consolas.ttx
  ttx -o segoeprb.ttf consolasb.ttx
~~~

## install new font

1. to be able to restore to the original fonts, you should keep them,
 e.g. as `__segoepr.ttf` und `__segoeprb.ttf` in yout working directory
2. drag the newly created fonts with the name of the old fonts in the
fonts directory (admin privileges required!)
4. the new fonts will be used after reboot
