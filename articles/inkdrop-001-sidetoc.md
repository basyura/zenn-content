---
title: "Inkdrop - sidetoc v1.16.0 release"
emoji: "📝"
type: "idea"
topics: ["Inkdrop"]
published: true
---

# Inkdrop

https://inkdrop.app

> Work with developer-focused features
> With its high customizability, extensibility and 100+ plugins, it will stick with your workflow and improve your productivity.

ノートアプリ(メモ管理)を転々としてきましたが、Inkdrop にようやく落ち着けたように思います。開発者向けであり、electron 上で js と style を使って細かいところまで自分の思うようにカスタマイズできるところがとても魅力的です。

公式とコミュニティの [plugin](https://my.inkdrop.app/plugins) が沢山あり、ダウンロード数一位になっている Vim Plugin が特に魅力的ですね。

使い始めは Windows のレンダリングが受け入れられず継続するか迷いましたが、使用するフォントとスタイルの調整で納得できる状態になったのを期に課金を始めて丸二年経ちました。

# sidetoc

Inkdrop に限らず各種ノートアプリに前々から欲しいと思っていた機能がありました。Web サイトではよく見るサイドバーの見出しです(このページの右にも出ているはず)。議事録(メモ)を取っているケースが顕著で、議題単位にヘッダー (`#`) を付けたとしても行数が膨らんだときに全体のどこに何が書いてあるのかを把握するのが辛くなります。書いているときは記憶に残っているのでまだよいのですが、後から見返すケースなどではとくに上から下まで目を通さないと何を書いているかを把握できないことがよくありました (記憶力が悪いので)。

冒頭に記載したとおり Inkdrop はカスタマイズ可能な開発者向けツールです。足りないものは自分で作って足すことができるのでトライしてみたのが 2 年前です。

@[card](https://blog.basyura.org/entry/2020/03/12/210001)

古き良き js を沢山書いてきた経験があったものの、近年の移り変わりが早すぎる状況に距離を置いていたこともあって react や webpack などはふんわり知っていたものの具体的には使えませんでした。そこから右往左往しながらリリースしたのが「[sidetoc plugin - Inkdrop](https://my.inkdrop.app/plugins/sidetoc)」です。

@[card](https://github.com/basyura/inkdrop-sidetoc)

![](/images/sidetoc.png)

実装内容は GitHub にある通りで、js 本業の人からすると色々思うこともあると思いますが、初めてづくしの中でなんとか形にしてリリースしました。それになりに使ってくださっているユーザーがいらっしゃるようでダウンロード数だけで見ると 8 番目に位置しています。リリースする回数が多ければダウンロード数がその分伸びるようになっていますが、本家の toc (プレビュー時にノート丈夫にヘッダを表示する) の次にランクインしているのが感慨深いですね。


前置きが長くなりましたが、地道に機能追加や不具合修正を重ねて v1.16.0 をリリースしました。

今回の修正は、ヘッダ部の折返しをしない機能を追加しています。具体的には下記のスタイルを当てる/当てないの設定とコマンドを追加しています。

```css
.sidetoc-pane li {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

plugin 設定ページで ON/OFF の設定があり、keymap.cson に下記のように定義して随時切り替えることができます。


```
'ctrl-x ctrl-e': 'sidetoc:wraptext-toggle'
```

# まとめ

Inkdrop は良いですね。開発者のライフスタイルやスタンスも参考になります。

[sidetoc plugin - Inkdrop](https://my.inkdrop.app/plugins/sidetoc) 以外にもいくつかリリースしているものがあります。自分だけで使っているものもあります。plugin 化しなくても init.js に記述することで UI を伴わないものはだいたい実現できるのではないかと思います。スタイルも UI テーマを自分で作らなくても styles.less に定義することで変えられます。便利ですね。
 
※ zenn.dev に興味が出たので初めて(お試し)の投稿になります。


