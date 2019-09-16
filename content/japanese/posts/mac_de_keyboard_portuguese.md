---
title: "ドイツMacキーボードを使って、Linux Mintでポルトガル語で書きます"
date: 2019-09-12T11:23:06+01:00
---

タイトル通り、特定の問題を紹介しましょう。MacBook Proを持って、よくポルトガル語で書くことが必要です。macOSなら普通大丈夫ですけど、Linux Mintを使えばキーボードレイアウト（ちなみに``Deutsch (Macintosh)``と申します）はちょっと違うので、ある文字が、例えばチルダとか、セディーユとか入力のことが難しくになります。このエントリで``Deutsch (Macintosh)``のキーボードレイアウトがカスタマイズされると様々なポルトガル語の文字を入力ができています。

Linux Mintのキーボードレイアウトの設定ファイルが``/usr/share/X11/xkb/symbols``のフォルダーであります。そしてドイツのレイアウトが全部``de``ファイルの中にあります。さってお気に入りのテキストエディタを実行しましょう（ファイルを編集をしたら``root``権限が必要です）。

設定ファイルが編集されますのでバックアップしましょう。

ファイルの中に次のようにがあります。

```
xkb_symbols "mac" {
    ...
};
```

この中に一つのラインが編集されます、もう一つは追加されます。

まず、次のラインをさがしましょう。

```
key <AB06> { [ n, N, asciitilde] };
```

このラインの設定の結果は、``alt+n``を押したらチルダの文字が入力されますけど、他の文字の変化のことができません。それをさけるために次に編集しましょう。

```
key <AB06> { [ n, N, dead_tilde] };
```

今``alt+n``を押したらãとか、õとか入力ことができます。

なお、セディーユの入力を追加しましょう。簡単に次のラインを追加。

```
key <AB03> { [ c, C, ccedilla, Ccedilla] };
```

編集をセーブして、ログアウトしてまたログインして、キーボードを試して下さい。お疲れさまでした。