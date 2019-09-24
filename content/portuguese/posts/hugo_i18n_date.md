---
title: "Localização de datas com Hugo"
date: 2019-09-16T14:58:39+01:00
---

O Hugo ainda não dá suporte completo a i18n no momento de escrita desta página. Um dos itens faltantes é a formatação de datas, o que resulta em todos os posts em todas as línguas sempre apresentarem datas com formatação inglesa. Isto é um tanto quanto inconveniente, mas pelo menos é possível contornar esse problema.

Dada a situação acima vou apresentar o contorno, que já é, inclusive, documentado pelos projetos do Hugo.

Os dados mais importantes serão adicionados na pasta "Data". O problema, porém, é que este diretório não é criado automaticamente pelo Hugo. Para tanto basta criar a pasta ``data`` na raiz do projeto em questão. Dentro desta pasta serão criados um arquivo para cada língua, conforme o exemplo abaixo:

```
/data
|-- months_en.json
|-- months_de.json
|-- months_pt.json
|-- ...
```

Devidos aos requisitos do da framework os arquivos dentro da pasta podem ser apenas dos formatos YAML, JSON, ou TOML. Neste caso escolhi JSON, mas cada um é livre para escolher o formato que melhor lhe convier.

Dentro dos arquivos são escritas as traduções de cada mês:

```
{
    "1" : "Jan",
    "2" : "Fev",
    "3" : "Mar",
    "4" : "Abr",
    "5" : "Mai",
    "6" : "Jun",
    "7" : "Jul",
    "8" : "Ago",
    "9" : "Set",
    "10" : "Out",
    "11" : "Nov",
    "12" : "Dez"
}
```

Utilizaremos os índices para determinar a tradução adequada de cada mês. Agora resta definir a maneira como as datas serão formatadas dentro dos textos:

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

Vejamos parter por parte.

Inicialmente, a cadeia de condicionais determina a língua atual, dada por ``.Site.Language.Lang``, que no meu caso serão ``de`` (alemão), ``jp`` (japonês), ``pt`` (português) ou a língua padrão ``en`` (inglês).

Em seguinda os campos de dia , dado por ``{{ .Date.Day }}``, e ano, ``{{ .Date.Year }}``, não têm muito segredo. O principal foco desta apresentação são os campos de meses:

```
{{ index $.Site.Data.months_de (printf "%d" .Date.Month) }}
{{ index $.Site.Data.months_pt (printf "%d" .Date.Month) }}
{{ index $.Site.Data.months_en (printf "%d" .Date.Month) }}
```

Cada arquivo criado na pasta ``data`` ficará disponível sob o parâmetro ``.Site.Data``, e as linhas acima permitirão obter os meses corretos pelos índices.