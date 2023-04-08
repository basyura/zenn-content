---
title: "Inkdrop - 検索で Enter したらエディタにフォーカスして Vim の検索キーワードにセットして ime を off"
emoji: "🌊"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Inkdrop]
published: true
---

# やりたいこと

* vim plugin を入れている前提。
* 検索のテキストボックスに日本語入力して記事を絞ると一つ目の記事が表示されるので Enter キーでエディタにフォーカスを移したい。
* ついでに vim の検索ワードにセットしてすぐ検索するようにしたい。
* ついでに ime を off にしてすぐ vim のキーバインドでカーソル移動したい (ひとまず Mac で)。

# ime を off にする

Mac で ime を off にするには ime-select を使えばよいので brew でインストール。
```sh
$ brew install im-select
```

Inkdrop で platform を判定しつつ、execSync で im-select を叩く。

```js
if (process.platform == "darwin") {
    const { execSync } = require("child_process");
    execSync("/usr/local/bin/im-select com.google.inputmethod.Japanese.Roman");
}
```

# init.js

検索のテキストボックスを取得する方法が適当なので、起動時にレイアウトによって無い場合はスルーしておく。

いつもの init.js に記述。

```js
// 検索テキストボックスで Enter したらエディタにフォーカスして Vim の検索キーワードにセットする
inkdrop.onEditorLoad(() => {
  const ele = document.querySelector(".note-list-search-bar input");
  // 起動時に非表示になっている場合は何もしない (非同期だった場合の考慮が必要)
  if (ele == null) {
    return;
  }
  ele.addEventListener("keydown", (e) => {
    // 実行トリガーキー
    if (e.key != "Enter") {
      return;
    }

    // 変換確定時は何もしない
    if (e.isComposing) {
      return;
    }

    // ime off
    imeoff();

    // vim の検索ワードにセットする
    setTimeout(() => {
      const vim = inkdrop.packages.activePackages.vim.mainModule.vim;
      const cm = inkdrop.getActiveEditor().cm;
      vim.exCommandDispatcher.processCommand(cm, "nohlsearch");
      vim.getVimGlobalState().query = new RegExp(ele.value, "i");
      inkdrop.commands.dispatch(document.body, "editor:focus");
    }, 100);
  });
});

const imeoff = () => {
  if (process.platform == "darwin") {
    const { execSync } = require("child_process");
    execSync("/usr/local/bin/im-select com.google.inputmethod.Japanese.Roman");
  }
};
```

# まとめ

また便利になった。
