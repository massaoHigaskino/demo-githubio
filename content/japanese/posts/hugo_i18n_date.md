---
title: "Ｈｕｇｏで日付の形式の国際化"
date: 2019-09-16T14:58:39+01:00
---

現在Ｈｕｇｏが国際化をまだ完全にできていません。この中に日付の形式があります、そしてそのわけで全員の言語で英語の形式が現れています。これは残念ですが、自分でほかの形式を作っていることができます。そしてこの作り方が教えてあげます。ちなみに多くの情報がＨｕｇｏのマニュアルから取り出しました。

結局一番重要なファイルが「データフォルダー」に起きられています。問題はこのフォルダーが自動で作成されていません。さって、``ｄata``　フォルダーをプロジェクトのルートディレクトリの中に作成しましょう。

なお、``ｄata``フォルダーの中に各自の言語でファイルを作成して下さい。例えば月訳書がこんなにになります。

```
/data
|-- months_en.json
|-- months_de.json
|-- months_pt.json
|-- ...
```

Ｈｕｇｏの要求は、``ｄata``フォルダーの中にＹＡＭＬとＪＳＯＮとＴＯＭＬのフォーマットだけ起きることができます。ＪＳＯＮが選ばれましたが、もっとふさわしいフォーマットを自由で選択して下さい。

次の例えを見て、ファイルの中に月を通訳して下さい。

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

ちなみに、日本語では月を通訳ことが必要ありません。なぜなら、言葉じゃなくて、番号が使われているからです。

今は月のインデックスを使って、正しいの言葉が出されています。そしてブログの中に使うことができます。

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

分析しましょう。

まず、条件付きブロックが正しい言語を``.Site.Language.Lang``で決定します。この場合には``.Site.Language.Lang``が``de``（ドイツ語）と``jp``（日本語）と``pt``（ポルトガル語）と``en``（英語）をリターンします。

そして、``{{ .Date.Day }}``（日）と``{{ .Date.Year }}``（年）は直接なフィルドですが、問題は月を定義するフィルドです。

```
{{ index $.Site.Data.months_de (printf "%d" .Date.Month) }}
{{ index $.Site.Data.months_pt (printf "%d" .Date.Month) }}
{{ index $.Site.Data.months_en (printf "%d" .Date.Month) }}
```

各ファイルが``.Site.Data``で公開されます。上のラインがファイルから通訳された言葉を取り出します。お疲れ。