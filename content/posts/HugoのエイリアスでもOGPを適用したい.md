---
title: "HugoのエイリアスでもOGPを適用したい"
date: 2021-12-21T22:56:24-08:00
tags:
    - hugo
---

最近、ブログのジェネレーターをHugoに乗り換えて色々機能を見ていく中で特に便利だと思ったのがエイリアスの機能でした。一つ残念だったのがデフォルトの状態だとエイリアスのURLにはOGPのメタデータが付与されないため、例えばTwitterで投稿されたリンクがTwitterカードとして展開されないという点でした。（自分が利用している[Anubis](https://github.com/Mitrichius/hugo-theme-anubis)テーマだと通常のページにはOGPメタデータはデフォルトで挿入されるようでした。）

# 解決策
エイリアスのテンプレートに独自の変更を入れることでメタデータを伝播させることができました。

```html {linenos=table}
<!DOCTYPE html>
<html>

<head>
    {{ if .Page }}
        {{ template "_internal/opengraph.html" . }}
        {{ template "_internal/twitter_cards.html" . }}
        <title>{{ .Page.Title }}</title>
    {{ else }}
        <title>{{ .Permalink }}</title>
    {{ end }}
    <link rel="canonical" href="{{ .Permalink }}" />
    <meta name="robots" content="noindex">
    <meta charset="utf-8" />
    <meta http-equiv="refresh" content="0; url={{ .Permalink }}" />
</head>

</html>
```
上記のHTMLを`layouts/`以下に`alias.html`として保存することでエイリアスのURLをポストした際もメタデータをうまく伝えてくれます。[Card Validator | Twitter Developers](https://cards-dev.twitter.com/validator) でしっかりメタデータが渡せているか確認できます。

# 参考
- https://gohugo.io/content-management/urls/#customize
- https://gohugo.io/templates/internal/#use-the-open-graph-template
- https://gohugo.io/templates/internal/#use-the-twitter-cards-template
