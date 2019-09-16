---
title: "Escrevendo em português utilizando um MacBook Pro com Linux Mint"
date: 2019-09-12T11:23:06+01:00
---

Como dito no título, esta postagem engloba um caso extremamente específico. Eu tenho um MacBook Pro alemão e frequentemente escrevo em Português com ele. Funciona bem no macOS. Entretanto, o layout padrão do Linux Mint (chamado ``Deutsch (Macintosh)``) não funciona exatamente da mesma forma. Isto dificulta a entrada de caracteres com til (e.g. ã, õ) e cedilhas (e.g ç). Ao longo deste texto mostro com alterar o layout padrão para permitir a digitação destes caracteres.

Os layouts de teclado no Linux Mint são definidos em arquivos textos do ``/usr/share/X11/xkb/symbols`` e os layouts alemães estão todos inclusos no arquivo ``de``. Escolha seu editor de texto preferido e abra o arquivo (serão necessários os privilégios de ``root`` para editá-lo).

Como vamos alterar um arquivo de configuração recomendo que seja feita uma cópia de segurança do mesmo.

Dentro do arquivo deparamos com o seguinte bloco:

```
xkb_symbols "mac" {
    ...
};
```

Será necessário editar uma linha já existente e adicionar uma outra.

Primeiramente encontre a seguinte linha:

```
key <AB06> { [ n, N, asciitilde] };
```

Esta linha define que a combinação ``alt+n`` resulta num til (~), mas não numa dead key, que permitiria a modificação de outros caracteres com til. Altere a linha para a seguinte forma:

```
key <AB06> { [ n, N, dead_tilde] };
```

Agora seremos capazes de utilizar o ``alt+n`` para adicionar til em outros caracteres.

Por segundo, precisamos adicionar os ç e Ç, os quais são bem simples. Basta adicionar a seguinte linha ao bloco:

```
key <AB03> { [ c, C, ccedilla, Ccedilla] };
```

Pronto. Salve suas alterações, saia e entre novamente a sessão e teste o teclado.