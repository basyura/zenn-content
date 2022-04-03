---
title: "Inkdrop で未完了のタスクにジャンプする"
emoji: "💡"
type: "tech"
topics: ["inkdrop"]
published: false
---

@[card](https://www.inkdrop.app)

@[card](https://my.inkdrop.app/plugins/vim)


Vim plugin を入れている状態で、タスク `* [ ]` にジャンプする。plugin にしなくても [init.js に記述する](https://docs.inkdrop.app/manual/the-init-file) ことでコマンド呼び出しができるようになる。

```js:init.js
inkdrop.commands.add(document.body, "mycmd:find-task", () => {
  const vim = inkdrop.packages.activePackages.vim.mainModule.vim;
  vim.getVimGlobalState().query = /\[ \]/;

  const el = inkdrop.getActiveEditor().cm.getWrapperElement();
  inkdrop.commands.dispatch(el, "vim:repeat-search");
});
```

```json:keymap.cson
'.CodeMirror.vim-mode:not(.insert-mode):not(.key-buffering) textarea':
    'ctrl-x ctrl-c': 'mycmd:find-task'
```

地味に便利。
もっといい方法がありそうだけど。
