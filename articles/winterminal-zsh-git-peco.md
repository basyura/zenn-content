---
title: "Windows Terminal + zsh + git で peco れないとき"
emoji: "🔨"
type: "tech"
topics: ["windowsterminal", "zsh", "git", "peco", "go"]
published: true
---

Windows Terminal + Git for Windows SDK + zsh で git コマンドの結果を peco りたいけど、画面が崩れてキー入力も受けなくなって Terminal を閉じるしかない事が発生するようにいつのまにかなってしまって困る。ブランチが大量にあることに起因しているのかと思いきやそうでもなく、sh で返す量を調整してから peco に渡してもダメだったので間に挟むプログラムを作ってみた。

@[card](https://github.com/basyura/pecogit)

# .zshrc

```sh
function peco_insert_selected_git_files(){
    BUFFER="  $(pecogit status --porcelain | peco | awk -F ' ' '{print $NF}' | tr '\n' ' ')"
    CURSOR=0
    zle reset-prompt
}
zle -N peco_insert_selected_git_files
bindkey "^x^l" peco_insert_selected_git_files

function peco-select-local-branch() {
    BUFFER="  $(pecogit branch | peco)"
    CURSOR=0
    zle reset-prompt
}
zle -N peco-select-local-branch
bindkey '^x^b' peco-select-local-branch

peco-select-branch() {
    BUFFER="  $(pecogit branch -a --sort=-authordate -n 1000 | peco)"
    CURSOR=0
    zle reset-prompt
}
zle -N peco-select-branch
bindkey '^x^a' peco-select-branch
```

# git branch

* ブランチ名が英数のみのものだけを対象とする (全角文字は除去)
* ブランチ数を制限するためのオプション -n を独自追加

```
$ pecogit branch -a --sort=-authordate -n 1000
```

# config

`~/.config/pecogit/config.json`

git branch で無視したいもの設定

```
{
    "branch_ignores":["cherry-pick", "revert-"]
}
```

# まとめ

画面が崩れないようになったけど何故なのかがよく分からないまま動いてて今に至る。
