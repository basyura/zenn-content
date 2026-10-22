---
title: "Inkdrop style 変更"
emoji: "🎨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Inkdrop]
published: true
---

## はじめに

Markdown ノートアプリの Inkdrop を Win/Mac 共に**常用して気がつけばもうすぐ丸七年**。メモ系はしっくりこなくてあれこれ移るのを繰り返していたので、ようやく**安住の地を得た**と言える。

![](/images/inkdrop-006-style.png)

僕の Inkdrop の画面。画面構成はシンプルになるようにして、エディタ部はなるべくプレビューと同じように見えるようにしている。本来の画面は本家の [inkdrop.app](https://www.inkdrop.app) を参照。web 上でデモが触れる (すごい)。



plugin が多くあり、満足度が一番高いのは vim。vim があるから Inkdrop を使っていると言っても過言ではない。

plugin をインストールする以外で挙動を変えたい所があれば、js/css を使ってだいたいのことができるのもとても良い。

下記に自作の plugin と css/js を使ってデフォルトから style を変えた点を記載する。

## テーマ

Default Dark テーマを使っているけど [material-dark-mod-syntax](https://my.inkdrop.app/plugins/material-dark-mod-syntax) の見た目が気に入っていたので、近くなるように styles.css に定義して調整している。

## ノートリストとサイドコンテンツ

ノートの一覧に本文は無くていいので [simple-note-list](https://my.inkdrop.app/plugins/simple-note-list) をインストールしてスッキリ。ノートの一覧がたくさん見えるのも良い。

最右列にあるノートのヘッダー一覧を表示しているのが自作の [sidetoc plugin](https://my.inkdrop.app/plugins/sidetoc)。各ヘッダーへのジャンプもキーボードで楽に操作できる。自分的に必須の plugin (なので作った)。

## エディターヘッダー部のレイアウト

タイトル部の `.editor-title-bar-layout` と book、status や tag を表示する `.editor-meta-layout` の領域がある。これに ToolBar が加わってデフォルトでは縦に並んでいる。

エディタはスッキリさせたいけど、設定で変えられるのは Toolbar を非表示にするのみ。

ユーザー用のスタイルシートで `.editor-title-bar-layout` と `.editor-meta-layout` を横に並べるように変更。

自分だけで css を書くのが限界に来て ChatGPT.app で Inkdrop をデバッグモードで開いて実際の DOM を確認してもらいながら概ね生成してもらった後に微調整した。前々からやりたいと思っていたのでようやく実現して嬉しい。

## リストの表示

`*` のままでもいいのだけど、`・` で html のリストのように表示される方が好みなので css で変更。番号付きリストも `・` で表示されてしまうのが難点と言えるが、使うことはほとんど無いので許容範囲。単純に付与されてる class で判断できないし、仕方がない (未調査)。結構、気に入っている。これによってプレビューと見た目がだいぶ近く見える印象。

```css
.md-list-mark {
  color: transparent !important;
}
.md-list-mark::before {
  content: "•";
  margin-right: -7px;
  color: #fac4fe !important;
}
```

## リンクをコンパクトに表示

リンク記法は url が長くなるほどに冗長で、一気に文が読み辛くなる。

url 部分が気にならないよう記号 (または絵文字) に置き換える shortlink plugin があったものの、メンテされておらず挙動が期待しているのと違う事もあって [link-compact plugin](https://my.inkdrop.app/plugins/link-compact) としてリリースした。

shortlink plugin では [、]、(、) といった記号は、スタイルシートで見えない色にした上でフォントサイズを小さくして見えないようにしていた。ほぼほぼ問題ないけど、カーソル移動でひっかかる感じになるし、見た目に違和感があるのは否めなかった。

記号を完全に非表示にしつつカーソル移動に違和感がないように取り組んだ。ChatGPT.app の Codex が、Computer Use で。便利。

## 記号を非表示

Inkdrop で使える [hide-markup plugin](https://my.inkdrop.app/plugins/hide-markup) は、**マークアップ用記号を非表示状態**にする。記号の存在を気にせずノートが書けるようになる。

現時点では**強調表示**の `**` と~~取り消し~~ の `~~` をサポート。記号を**一時的に表示**したい場合、該当箇所で `<Enter>` キーを押すと `**解除**` のようになる。カーソルが範囲外に出ると、記号が非表示状態に**戻る**。

link-compact plugin で url や記号が見えないようにしつつカーソル移動の考慮を追加したところで他の記号も対応したくなってしまったので作成した。最終的には link-compact も取り込んだほうが分かりやすそうには思うけど、まずは別 plugin とした。

文に `**` のような記号がある場合はノイズになってしまうので**なるべく使わないようにしていた**けど気にしなくて良くなった。むしろ積極的に使いそう。なんだかんだ不要な情報で読みづらさがあったと再認識した。

他の記号 (記法) も対応していきたいとは思うが、`インラインコード` や引用 `>` のように対応しない方が見やすそうなケースもあるのでおいおい (こういうのを考えている時間は楽しい)。

## まとめ

具体的な css は [inkdrop-config](https://github.com/basyura/inkdrop-config) で管理している。複数端末で共有していることもあって OS やモニタによる微妙な違いが出るケースがあるので、固有の設定もできるように init.js で styles.local.css が存在していたら読み込むようにしている。

とてもよい。
