---
title: "Date Internationalization in Hugo"
date: 2019-09-16T14:58:39+01:00
draft: true
---

At the moment of writing of this page, Hugo still does not support full i18n. One of the things still left is date representation, which means every post in every language will probably end up presenting English formatted dates. It is unfortunate, but I found a way to work around it.

In this post I will show how I made a quick workaround for such problem in this project. A lot of what I made is already presented officially by Hugo in their documentation.

The most important information will be added to the so called "Data Folder." The problem is, this folder is not created automatically by Hugo when it generates a new project. Simply enough all you have to do is create a new folder, called ``data`` at the root of the project. Inside we will add one file for each language you support, like this:

```
/data
|-- months_en.json
|-- months_de.json
|-- months_pt.json
|-- ...
```

As per Hugo's requirements, files inside the data folder must be in YAML, JSON, or TOML format. I chose JSON in this case, you are free to choose whichever you are more comfortable with.

Inside those files write the translated month names as follows:

```
{
    "1" : "Jan",
    "2" : "Feb",
    "3" : "Mar",
    "4" : "Apr",
    "5" : "May",
    "6" : "Jun",
    "7" : "Jul",
    "8" : "Aug",
    "9" : "Sep",
    "10" : "Oct",
    "11" : "Nov",
    "12" : "Dec"
}
```

We will use the index in order to determine which month will be written out. Now we will need to use them, be it inside posts or (more likely) layout files.

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

Lets see it step by step.

First, we have a stream of conditionals which will determine the current localization in use, given by ``.Site.Language.Lang``, which in my case will be either ``de`` (German), ``jp`` (Japanese), ``pt`` (Portuguese) or default to ``en`` (English.)

Second, there isn't much secret around ``{{ .Date.Day }}`` (which gives us the current day) or ``{{ .Date.Year }}`` (the current year.) The biggest point here is defined by the month field:

```
{{ index $.Site.Data.months_de (printf "%d" .Date.Month) }}
{{ index $.Site.Data.months_pt (printf "%d" .Date.Month) }}
{{ index $.Site.Data.months_en (printf "%d" .Date.Month) }}
```

Each file created in the data folder will be available under ``.Site.Data``, and using the lines above you will be able to retrieve the translations written in each file.