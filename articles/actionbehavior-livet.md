---
title: "ActionBehavior.Livet で Action を簡単に呼ぶ"
emoji: "🦔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["WPF", "MVVM", "Livet"]
published: true
---

## 何かしらのちょっとした GUI アプリケーションを作るとき

さぁ、つくるぞー。

* 気持ち的に Windows Form は嫌なので WPF を選ぶ。
* WPF で作り始めるけど、"チョットシタものだから" とコードビハインドでゴリゴリ書き始める。
* ちょっとしたを超えて機能を充実させていくと、コードビハインドのゴリゴリ感が気になって MVVM にしたくなる。
* 本気の MVVM をしたいわけではないけどロジックとビューを分離したい。

となる(定期)。

MVVM したくなると ViewModel が必要になるけど、INotifyPropertyChanged の実装を毎回持ってくるのが面倒なので三周回って Livet を使うのに落ち着く。Livet の全機能 ([Cask](https://www.nuget.org/packages/LivetCask/)) は要らないので [Livet.MVVM](https://www.nuget.org/packages/LivetCask.Mvvm/) があれば、ほぼほぼ大丈夫。Livet.MVVM の依存関係で Livet 機能の幾つかと、イベント呼び出しの核となる [Microsoft.Xaml.Behaviors.Wpf](https://www.nuget.org/packages/Microsoft.Xaml.Behaviors.Wpf/) も付いてくる。

で、結局書き直す。

## コマンドをもっと分離したい

MVVM に書き直すのはいいとして

* ViewModel にコマンドを定義しまくると ViewModel が厚くなりすぎるのでコマンドを別クラスに整理して分離したくなる。
* コマンドを簡単に呼び出したい。
* 基本的に MVVM で作るけど、状況によっては View に直接アクセスしたい (MVVM 原理主義ではない)。

ので、このあたりをライブラリとして作成 (細かいところは雑)。

https://github.com/basyura/ActionBehavior.Livet


https://www.nuget.org/packages/ActionBehavior.Livet/


## Install

```
PM> NuGet\Install-Package ActionBehavior.Livet -Version 0.2.0
```

## ViewModel

Livet.ViewModel を継承した ViewModel を作成。

```cs
using Livet;

namespace Hoge
{
    public class MainWIndowViewModel : ViewModel
    {
        private string _Message;
        public string Message
        {
            get => _Message;
            set => RaisePropertyChangedIfSet(ref _Message, value);
        }
    }
}

```

## Action クラス

クリックなどの各イベントに対応した処理を実行するクラスを定義。
ActionBehavior.Livet.ActionCommand を継承し、Execute メソッドを実装する。`Task<bool>` を返す OK しか定義していないけど、汎用性を持たせるためにどうするか悩み中。false を返したときの挙動をどうするのか、その他のステータスを細かく返す必要があるのかとか。

```cs
using System;
using System.Threading.Tasks;
using Livet;

namespace Hoge
{
    public class Hello : ActionCommand<MainWindowViewModel>
    {
        public override Task<bool> Execute(object sender, EventArgs evnt, object parameter)
        {
            ViewModel.Message = "Hello";

            return OK;
        }
    }
}
```


## Xaml からの呼び出し

Microsoft.Xaml.Behaviors を使ってイベントに応じた Action 呼び出しを記述。Action の呼び出しには Execute タグを使う。

```xml
<Window 
        ・・・(略)・・・
        xmlns:i="http://schemas.microsoft.com/xaml/behaviors"
        xmlns:ab="clr-namespace:ActionBehavior.Livet;assembly=ActionBehavior.Livet"
        >
    <Window.DataContext>
        <vm:MainWindowViewModel/>
    </Window.DataContext>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <TextBlock Text="{Binding Message}" />
        <Button Grid.Row="1" Content="Hello">
            <i:Interactions.Triggers>
              <i:EventTrigger EventName="Click">
                  <ab:Execute Action="Hello" />
              </i:EventTrigger>
            </i:Interactions.Triggers>
        </Button>
    </Grid>
</Window>
```

## Resolver の定義

ViewModel (の型) と action 名から呼び出すクラスの FullName を返す Resolver を定義。ライブラリ側で MEF を使って Resolver を取得するので、IActionResolver を指定した Export 定義が必要。
画面 (Window) を複数作る場合などは、それぞれに ViewModel を定義して namespace に ViewModel 名を入れて分割することを想定。

```cs
using System.ComponentModel.Composition;
using ActionBehavior.Livet;
using Livet;

namespace Hoge
{
    [Export(typeof(IActionResolver))]
    public class ActionResolver : IActionResolver
    {
        public string Resolve(ViewModel vm, string action);
        {
            return $"Hoge.Actions.{action}"
        }
    }
}
```

これで、

```
View → Execute → Resolver → Action
```

の順に実行される。

## View にアクセスしたい場合

Window の ContentRendered イベントで Initialize を呼んで、Initialize の中で View を ViewModel に保持する。後は `Window#FindName` を使うなど好きにする。

```xaml
<Window>
    <i:Interaction.Triggers>
        <i:EventTrigger EventName="ContentRendered">
            <ab:Execute Action="Initialize" />
        </i:EventTrigger>
    </i:Interaction.Triggers>
</Window>
```

```cs
public class Initialize : ActionCommand<MainWIndowViewModel>
{
    public override Task<bool> Execute(object sender, EventArgs evnt, object parameter)
    {
        ViewModel.View = sender as Window;

        return OK;
    }
}
```



## まとめ

Livet を使わずに、もろもろを含んだ Framework を作ってみたりしたけど ([Eleve](https://www.nuget.org/packages/Eleve/))、テンプレートや拡張を作るのがクソめんどくさすぎるし、機能として信頼できる Livet を使う方が楽なのもあって欲しいところだけ組み込む形をとってみた。実装が雑なところは否めないけど。

GUI アプリケーションを作るタイミングはそんなに無いので、今後機能追加していくかは謎だけど作ろうと思ったときにどうするかの自分の型ができたので細々とやっていくつもり。

※ 名前がいろいろと大丈夫なのかがあるけど気にしない。
