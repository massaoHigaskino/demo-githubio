---
title: "Datumlokalisation mit Hugo"
date: 2019-09-16T14:58:39+01:00
---

In diesem Moment, Hugo unterstützt nicht voll i18n Funktionalität. Eine die Datumsdarstellung ist, dass heißt jede Eintrag wird mit Englischedatumsformat bearbeitet. Glücklich, es ist möglich ein Workaround nutzen.

Durch diese Eintrag wird ein Workaround präsentiert, das diese Probleme einlösen können. Die größenteiles diese Eintrag wird schon in Hugo's offiziell Dokumentation erscheint.

Die wichtigste Bearbeitungen wird in "Datumordner" hinzugefügt. Sie müssen ``data`` in Stammverzeichnis erstellen, weil es nicht automatische generiert ist. Erstellen Sie drin eine Datei für jede Sprache, dass Sie unterstützen wollen, sowieso:

```
/data
|-- months_en.json
|-- months_de.json
|-- months_pt.json
|-- ...
```

Hugo erfordern, dass die Dateien in YAML, JSON oder TOML Formaten sind. Ich habe JSON benutzt, aber Sie können dein Lieblingsformat auswählen.

Schreiben Sie in die Dateien die Übersetzung der Monaten:

```
{
    "1" : "Jan",
    "2" : "Feb",
    "3" : "Mär",
    "4" : "Apr",
    "5" : "Mai",
    "6" : "Jun",
    "7" : "Jul",
    "8" : "Aug",
    "9" : "Sep",
    "10" : "Okt",
    "11" : "Nov",
    "12" : "Dez"
}
```

Wir nutzen der index um das richtig Monat zu entscheiden. Jetzt nutzen wir diese Einstellungen um Eintragen zu passen.

```
{{ if eq .Site.Language.Lang "de" }}
    {{ .Date.Day }}. {{ index $.Site.Data.months_de (printf "%d" .Date.Month) }}. {{ .Date.Year }}
{{ else if eq .Site.Language.Lang "jp" }}
    {{ .Date.Year }}年{{ index (printf "%d" .Date.Month) }}月{{ .Date.Day }}日
{{ else if eq .Site.Language.Lang "pt" }}
    {{ .Date.Day }} {{ index $.Site.Data.months_pt (printf "%d" .Date.Month) }} {{ .Date.Year }}
{{ else }}
    {{ index $.Site.Data.months_en (printf "%d" .Date.Month) }} {{ .Date.Day }}, {{ .Date.Year }}
{{ end }}
```

Wir analysieren es scrittweise.

Zuerst, die Bedienungsoperatoren entscheiden welche Lokalisierung wird benutzt, durch ``.Site.Language.Lang``, dass in ``de`` (Deutsch), ``jp`` (Japanisch), ``pt`` (Portuguiesisch) oder das voreingestellt ``en`` (Englisch) resultiert.

Dannach, ``{{ .Date.Day }}`` (aktueller Tag) und ``{{ .Date.Year }}`` (aktuelles Jahr) sind einfach. Monaten werden durch die folgende Linien erstellt:

```
{{ index $.Site.Data.months_de (printf "%d" .Date.Month) }}
{{ index $.Site.Data.months_pt (printf "%d" .Date.Month) }}
{{ index $.Site.Data.months_en (printf "%d" .Date.Month) }}
```

Jede erstellt Datei wird unten ``.Site.Data`` verfügbar gemacht, und die obere Linien fragen die richtige Übersetzung ab.