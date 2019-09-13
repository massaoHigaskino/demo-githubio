---
title: "Wie schreibt man auf Portugiesisch in Linux Mint mit Deutsch Macintosh Tastatur"
date: 2019-09-12T11:23:06+01:00
---

Ich stelle ein sehr spezifische Situation vor, in der mit einen Deutschen MacBook Pro auf Portugiesisch oft schreibe. Es geht ganz gut in macOS, aber Linux Mint's voreingestellt Tastaturbelegung ist nicht ganz änhlich (es heißt ``Deutsch (Macintosh)``). Es macht sehr schwierig (bzw. umöglich) um Tilde (z.B. ã oder õ) und Cedille (z.B. ç) einzutippen. Durch diese Eintrag vorstelle ich wie kann man die eingestellt ``Deutsch (Macintosh)`` Tastaturbelegung veränder um diesen Buchstaben einzutippen.

Linux Mint's Tastaturbelegungen sind durch Textdateien eingestellt, die in ``/usr/share/X11/xkb/symbols`` gefunden werden, und jeden Deutschebelegungen sind in die ``de`` Datei geschrieben. Wählen Sie deine Lieblingstexteditor um diese Datei zu öffnen (es fordert ``root`` Berechtigungen zu bearbeiten).

Wenn Sie Einstellungdateien veränder möchten, es währe besser ein Kopie zu sichern.

In diese Datei wird die folgendes Stück gefunden:

```
xkb_symbols "mac" {
    ...
};
```

Zuerst, finden Sie die folgende Linie:

```
key <AB06> { [ n, N, asciitilde] };
```

Diese Linie eingestellt, dass ``alt+n`` ein Tilde (~) eintippen, aber wir benötige es als ein Tottaste, weil es um andere Buchstaben zu modifizieren erlauben. Tauschen Sie mit der folgenden Linie:

```
key <AB06> { [ n, N, dead_tilde] };
```

Jetzt kann man ``alt+n`` benutzen um andere Buchstaben zu verändern.

Dannach, brauchen wir irgendwie ç und Ç einzutippen. Das ist sehr einfach, hinzufügen Sie die folgende Linie:

```
key <AB03> { [ c, C, ccedilla, Ccedilla] };
```

Fertig! Speichern Sie, ausloggen und wieder einloggen, dann Testen Sie das Tastatur.