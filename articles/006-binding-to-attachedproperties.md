---
title: "AttachedProperty(添付プロパティ)にバインディングする方法"
emoji: "📎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["WinUI", "WinUI3", "WindowsAppSDK", "XAML"]
published: true
---

https://youtu.be/QX8-V-PcHA4

[AttachedProperty(添付プロパティ)](https://learn.microsoft.com/ja-jp/windows/uwp/xaml-platform/attached-properties-overview?wt.mc_id=MVP_356303)には、

- Grid.Row
- Grid.Column
- Grid.RowSpan
- Grid.ColumnSpan
- Canvas.Top
- Canvas.Left
- Canvas.ZIndex

などがあり、自分で定義するカスタムなものを含め、とてもよく使いますが、
これらにバインディングする方法は以外と知られていません。
(「添付プロパティ」よりも「後付けプロパティ」という名称の方が理解し易いと感じるのは私だけでしょうか？🤔)

では、早速AttachedPropertyにバインディングする方法を紹介します。

例えば、`TextBlock`の`Text`プロパティに
`SomeButton`というコントロールの`Grid.Row`や`Grid.Column`
をバインディングするには次のように記述します。

```xml
<TextBlock>
    <Run Text="Row: " />
    <Run Text="{x:Bind SomeButton.(Grid.Row)}" />
</TextBlock>
<TextBlock>
    <Run Text="Column: " />
    <Run Text="{x:Bind SomeButton.(Grid.Column)}" />
</TextBlock>
```

つまり、

```cs
{x:Bind [対象コントロール名].([AttachedPropertyクラス名].[AttachedProperty名])}
```

となります。
今回は以上です。

Happy Coding!😊
