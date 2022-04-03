---
title: "Inkdrop - vim plugin をさらに便利にする"
emoji: "🔥"
type: "tech"
topics: ["Inkdrop"]
published: false
---

# 概要

@[card](https://my.inkdrop.app/plugins/vim)

Inkdrop を使う動機となる要素の一つ、[vim keybindings plugin](https://my.inkdrop.app/plugins/vim) の細かい挙動を自分好みに変えて使うための tips です。スタイルに関するものは styles.less、ロジックに関するものは init.js に記述することを想定しています。

https://docs.inkdrop.app/manual/style-tweaks

https://docs.inkdrop.app/manual/the-init-file

# カーソルの色を変える

インサートモード、ノーマルモードのカーソル色を変えたい場合の設定です。
デフォルトのスタイルはインサートモード時のカーソルが細いので少し太くしています。幅、色は好みですね。

```css:styles.less
.CodeMirror-cursor {
    border-left: 2px solid #ffa87d;
}
.cm-fat-cursor .CodeMirror-cursor {
    background: #ffa87d;
}
```


# カーソルの点滅を止める

点滅が気になるので止めたい場合の設定です。点滅の周期が思考・操作の邪魔になる気がする場合の設定です。

```js:init.js
inkdrop.onEditorLoad((e) => {
  const { cm } = inkdrop.getActiveEditor();
  cm.setOption("cursorBlinkRate", 0);
})
```

# プレビューモード

プレビューモード時も `j` `k` `ctrl+d` `ctrl+u` でスクロールができますが、外部リンク・内部リンクを開く場合にはクリック操作が必要になります。キーボードで済ませたい場合は別途 [hitahint](https://my.inkdrop.app/plugins/hitahint) plugin を入れます。

https://my.inkdrop.app/plugins/hitahint

![](https://raw.githubusercontent.com/basyura/inkdrop-hitahint/master/images/preview.png)

```json:keymap.cson
'.mde-preview':
    'e': 'hitahint:show'
```

# ウインドウサイズを変えるコマンドを追加する

基本的に Inkdrop のウインドウを最大化して使っていても、自分で画面を表示しながらメモを取りたい場合や資料と並べてメモを取りたい場合などがあります。通常はマウスを使ってウインドウサイズを変更しますが、キーボードでサクッと済ませるための設定です。

Inkdrop のコマンドとして定義することもできますが、コマンドを増やしていくとキーの組み合わせが足りなくなるので Vim Plugin のコマンドとして定義します。

```js:init.js
inkdrop.onEditorLoad(() => {
  var CodeMirror = require("codemirror");
  // 横幅半分にリサイズ
  CodeMirror.Vim.defineEx("half", "ha", () => {
    const height = window.screen.height;
    const width = window.screen.width;
    const info = { x: width / 2, y: 0, width: width / 2, height: height };
    inkdrop.window.setBounds(info);
  });
  // 画面いっぱいにリサイズ (≠ Full Screen)
  CodeMirror.Vim.defineEx("full", "fu", () => {
    const height = window.screen.height;
    const width = window.screen.width;
    const info = { x: 0, y: 0, width, height };
    inkdrop.window.setBounds(info);
  });
});
```

# Search Bar で検索したあとのノート切り替え

若干バグ的な動きですが、Search Bar で検索し結果のノート一覧をクリックした後にノート上では Visual Mode に引きづられている？のか `j` `k` などでカーソル移動をすると範囲選択になってしまいます。一回 `Escape` を押せば回避できますが、毎回するのは手間なので回避するための設定です。

検索した後にマウスを使ってノートの一覧をクリックするのもめんどうなので、`ctrl-.` `ctr-,` で選択ノートを変更するようにしています。この際に上記の挙動が発動しないようにします。

```json:keymap.cson
'body':
    'ctrl-.': 'mycmd:open-next-note'
    'ctrl-,': 'mycmd:open-prev-note'
```

```javascript:init.js
inkdrop.commands.add(document.body, {
  "mycmd:open-next-note": () => openNote("next"),
  "mycmd:open-prev-note": () => openNote("prev"),
});

function openNote(mode) {
  console.log("openNote");
  inkdrop.commands.dispatch(document.body, `core:open-${mode}-note`);
  inkdrop.commands.dispatch(document.body, "editor:focus");
  // to avoid visual mode
  setTimeout(() => {
    const vim = inkdrop.packages.activePackages.vim.mainModule.vim;
    const cm = inkdrop.getActiveEditor().cm;
    vim.exCommandDispatcher.processCommand(cm, "nohlsearch");
  }, 100);
}
```

# Normal Mode の Escape 時に検索ワードをクリアする

`/inkdrop` のようにノート内の単語検索をした場合、`:noh` or `:nohlsearch` を実行するまで単語がハイライトされた状態になります。Normal Mode 時の `Esc` と同時に解除 (`:noh`)する設定です。

```json:keymap.cson
'.CodeMirror.vim-mode.normal-mode textarea':
    'escape': 'mycmd:reset-normal-mode'
```

```javascript
inkdrop.commands.add(document.body, "mycmd:reset-normal-mode", () => {
  invoke("vim:reset-normal-mode");
  const vim = inkdrop.packages.activePackages.vim.mainModule.vim;
  const cm = inkdrop.getActiveEditor().cm;
  vim.exCommandDispatcher.processCommand(cm, "nohlsearch");
});
```

# Task (check box) を検索対象にする

開いているノート内の未完了タスク (check box) にジャンプするための設定。地味に便利。

```json:keymap.cson
'.CodeMirror.vim-mode.normal-mode textarea':
    'ctrl-x ctrl-c': 'mycmd:find-task'
```

```javascript:init.js
inkdrop.commands.add(document.body, "mycmd:find-task", () => {
  const vim = inkdrop.packages.activePackages.vim.mainModule.vim;
  vim.getVimGlobalState().query = /\[ \]/;
  const el = inkdrop.getActiveEditor().cm.getWrapperElement();
  inkdrop.commands.dispatch(el, "vim:repeat-search");
});
```

# まとめ

Inkdrop + Vim Plugin を常用しながら更にイイ感じになるように書き溜めた tips をまとめました。やはり、かゆいところがあってもどうにかできるところがいいですね。
