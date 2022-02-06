---
title: "Inkdrop にタブ機能をつけてみる"
emoji: "💡"
type: "tech"
topics: ["inkdrop"]
published: false
---

# タブ機能リクエスト

フォーラムに寄せられた機能追加リクエスト。

@[card](https://forum.inkdrop.app/t/tabs-support/2497)

ブラウザのようなタブ機能が Inkdrop にあると便利だよね。
  * 既にノートの一覧があるし、タブ機能を組み込むと複雑になるし人によって欲しいものが異なるしシンプルに保ちたい。

ノートブックをいろいろまたがるし一つのウインドウ内で見れると便利だよね
  * 重くなるし複雑になるしやっぱりシンプルに保ちたい。


どっちもわかる。

# vtab plugin

タブ機能があったら便利そうだけど便利に使えるのかがよくわからないので作ってみた。

@[card](https://github.com/basyura/inkdrop-vtab)


vtab の v は virtual の意 (てきとう)。イメージは↓でウインドウの右下(ノートの下部)にタブを付けてみた。

![](/images/vtab.png)

タブの情報は設定にあるノート ID (例: note:opQ-gZXdC) に指定された先に json で保存する。

```json:note
[
  {
    "noteId": "note:pfvAtIgbD",
    "title": "⚙ PC 設定"
  },
  {
    "noteId": "note:AqcErCsc",
    "title": "🚀 Go"
  },
  {
    "noteId": "note:yuxRt3Dw",
    "title": "📈 plantuml"
  },
  {
    "noteId": "note:sBg2KwElb",
    "title": "UML"
  }
]
```

タブをどう開くかは悩ましい。ノートリストをダブルクリックしたときや、なにかしらで表示したら(開いたら)とりあえず tab に追加するとしてもいいけど、意図しないものが大量に追加されそうなのでコマンド発行で登録するようにしてみた。タブに追加したノートの切り替えもコマンドで行う (タブクリックで切り替えも可)。

```json:keymap.cson
'.CodeMirror.vim-mode:not(.insert-mode):not(.key-buffering) textarea':
    'ctrl-t': 'vtab:add'
    'ctrl-f': 'vtab:next'
    'ctrl-b': 'vtab:prev'
```

僕は vim plugin を愛用しているけど、そうでない場合は `body` や `.mde-preview` に指定する。


# 使ってみた感想

関連するノートが複数あってすぐに行き来したい場合や、並行して複数のことをしている場合や、 todo として pin の代わりに使う場合に便利なこともある。けど、そういうケースはあまり多くないので、[switch-note](https://my.inkdrop.app/plugins/switch-note) plugin (実際には fork した自分用) を使ってビシビシ切り替えてしまえばいいし、別ウインドウで表示しておくことでも対応できる。ノートの領域が使っている事の方が気になることが多くなって使うのをやめてしまった。
もうちょっとヒネればイイ感じになりそうな気はする。

