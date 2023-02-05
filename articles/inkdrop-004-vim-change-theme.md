---
title: "Inkdrop - vim plugin でテーマを切り替える"
emoji: "🔥"
type: "tech"
topics: ["Inkdrop"]
published: true
---

# テーマを変えるときに設定画面を開きたくない

テーマを変えることはあまりないのだけど、環境によって (周りが明るいとか暗いとか目の調子とか) で明るめのテーマが良いときと暗めのテーマが良いときがある。その場合に設定画面を開いてリストボックスを選ぶのがめんどい。
テーマをプログラムで変える方法があるはずだと既存の plugin を調べてみるとあった。

@[card](https://my.inkdrop.app/plugins/auto-switch-themes)

この plugin を参考に init.js に組み込んでみる。

# Vim plugin のコマンドから変える


```js:init.js
inkdrop.onEditorLoad(() => {
  var CodeMirror = require("codemirror");
  // vim plugin not loaded
  if (CodeMirror.Vim == null) {
    return;
  }

  CodeMirror.Vim.defineEx("theme", "theme", (cm, param) => {
    if (param.args == null || param.args.length == 0) {
      showConfirm(cm, "args : theme");
      return;
    }

    const themes = {
      light: ["github-preview", "default-light-ui", "default-light-syntax"],
      dark: ["github-preview", "default-dark-ui", "default-dark-syntax"],
    };

    const theme = themes[param.args[0]];
    if (theme == null) {
      showConfirm(cm, "no theme");
      return;
    }

    inkdrop.config.set("core.themes", theme);
  });
```

使い方

```sh
:theme light
```

```sh
:theme dark
```


# まとめ

UI と Syntax の両方のテーマをサクッと切り替えられて便利。
コマンドと引数を `Tab` で補完できると便利なのだけど、今はコマンドバーが謎に残ってしまうので後で見てみる。
