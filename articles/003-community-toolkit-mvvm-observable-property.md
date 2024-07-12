---
title: "WinUI x CommunityToolkit.Mvvm [ObservableObject・ObservableProperty編]"
emoji: "🛠️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["WinUI", "WinUI3", "WindowsAppSDK", "XAML", "MVVM"]
published: true
---

[`CommunityToolkit.Mvvm`](https://learn.microsoft.com/ja-jp/dotnet/communitytoolkit/mvvm/?wt.mc_id=MVP_356303)NuGetパッケージを使った実装サンプルコードを紹介します。

# UI
![](/images/003/firstname-lastname-fullname.gif)

# サンプルコード

```cs:SomePageViewModel.cs
using CommunityToolkit.Mvvm.ComponentModel;

namespace CommunityToolkitMvvmDemo;

public partial class SomePageViewModel : ObservableObject
{
    [ObservableProperty]
    [NotifyPropertyChangedFor(nameof(FullName))]
    private string _firstName = string.Empty;

    [ObservableProperty]
    [NotifyPropertyChangedFor(nameof(FullName))]
    private string _lastName = string.Empty;

    public string FullName => $"{FirstName} {LastName}";
}
```

```cs:SomePage.xaml.cs
using Microsoft.UI.Xaml.Controls;

namespace CommunityToolkitMvvmDemo;

public sealed partial class SomePage : Page
{
    public SomePage()
    {
        InitializeComponent();
    }

    public SomePageViewModel ViewModel { get; } = new();
}
```

```xml:SomePage.xaml
<?xml version="1.0" encoding="utf-8" ?>
<Page
    x:Class="CommunityToolkitMvvmDemo.SomePage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:local="using:CommunityToolkitMvvmDemo"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    mc:Ignorable="d">

    <Page.Resources>
        <x:Double x:Key="Spacing">12</x:Double>
        <Style
            BasedOn="{StaticResource DefaultTextBoxStyle}"
            TargetType="TextBox">
            <Setter Property="Width" Value="200" />
        </Style>
    </Page.Resources>

    <StackPanel
        HorizontalAlignment="Center"
        VerticalAlignment="Center"
        Spacing="{StaticResource Spacing}">
        <StackPanel
            Orientation="Horizontal"
            Spacing="{StaticResource Spacing}">
            <TextBox
                Header="First Name"
                Text="{x:Bind ViewModel.FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />
            <TextBox
                Header="Last Name"
                Text="{x:Bind ViewModel.LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />
        </StackPanel>
        <StackPanel>
            <TextBlock Text="Full Name" />
            <TextBlock Text="{x:Bind ViewModel.FullName, Mode=OneWay}" />
        </StackPanel>
    </StackPanel>

</Page>
```
# 解説

## `ObservableObject`の継承

`ObservableObject`は抽象クラスであり、

- `INotifyPropertyChanging`
- `INotifyPropertyChanged`

が既に実装されています。

どうしても他のクラスを継承する必要がある場合、`ObservableObject`属性を使うことで、`CommunityToolkit.Mvvm`のソースジェネレーターに同等のコードを生成させることができます。

```cs:SomePageViewModel.cs
[ObservableObject]
public partial class SomePageViewModel : SomeBaseClass
{
    [ObservableProperty]
    [NotifyPropertyChangedFor(nameof(FullName))]
    private string _firstName = string.Empty;

    [ObservableProperty]
    [NotifyPropertyChangedFor(nameof(FullName))]
    private string _lastName = string.Empty;

    public string FullName { get => $"{FirstName} {LastName}"; }
}
```

ソースジェネレーターが生成したコードを確認することができます。

![](/images/003/observable-object-attribute.png)
*CommunityToolkitMvvmDemo.SomePageViewModel.g.cs*

:::message alert
基本的には、`ObservableObject`属性よりも`ObservableObject`の継承を優先してください。`ObservableObject`属性は、使用されたクラス毎に`INotifyPropertyChanging`、`INotifyPropertyChanged`を実装したクラスのコードが自動生成されますので、バイナリサイズが大きくなっていきます。
:::

## `partial`キーワード

`CommunityToolkit.Mvvm`のソースジェネレーターにコードを生成してもらうため、対象クラスをpartialクラスにする必要があります。

## `ObservableProperty`属性

`private`フィールドに`ObservableProperty`属性を付けることで、`CommunityToolkit.Mvvm`のソースジェネレーターがObservableなプロパティのコードを生成してくれます。

```cs
[ObservableProperty]
private string _firstName = string.Empty;
```

上記のコードでフィールド名_firstName(`firstName`、`m_firstName`のいずれも可)に従って、下記のコード通り、`FirstName`プロパティが自動生成されます。

```cs
/// <inheritdoc cref="_firstName"/>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.2.0.0")]
[global::System.Diagnostics.CodeAnalysis.ExcludeFromCodeCoverage]
public string FirstName
{
    get => _firstName;
    [global::System.Diagnostics.CodeAnalysis.MemberNotNull("_firstName")]
    set
    {
        if (!global::System.Collections.Generic.EqualityComparer<string>.Default.Equals(_firstName, value))
        {
            OnFirstNameChanging(value);
            OnFirstNameChanging(default, value);
            OnPropertyChanging(global::CommunityToolkit.Mvvm.ComponentModel.__Internals.__KnownINotifyPropertyChangingArgs.FirstName);
            _firstName = value;
            OnFirstNameChanged(value);
            OnFirstNameChanged(default, value);
            OnPropertyChanged(global::CommunityToolkit.Mvvm.ComponentModel.__Internals.__KnownINotifyPropertyChangedArgs.FirstName);
        }
    }
}

/// <summary>Executes the logic for when <see cref="FirstName"/> is changing.</summary>
/// <param name="value">The new property value being set.</param>
/// <remarks>This method is invoked right before the value of <see cref="FirstName"/> is changed.</remarks>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.2.0.0")]
partial void OnFirstNameChanging(string value);
/// <summary>Executes the logic for when <see cref="FirstName"/> is changing.</summary>
/// <param name="oldValue">The previous property value that is being replaced.</param>
/// <param name="newValue">The new property value being set.</param>
/// <remarks>This method is invoked right before the value of <see cref="FirstName"/> is changed.</remarks>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.2.0.0")]
partial void OnFirstNameChanging(string? oldValue, string newValue);
/// <summary>Executes the logic for when <see cref="FirstName"/> just changed.</summary>
/// <param name="value">The new property value that was set.</param>
/// <remarks>This method is invoked right after the value of <see cref="FirstName"/> is changed.</remarks>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.2.0.0")]
partial void OnFirstNameChanged(string value);
/// <summary>Executes the logic for when <see cref="FirstName"/> just changed.</summary>
/// <param name="oldValue">The previous property value that was replaced.</param>
/// <param name="newValue">The new property value that was set.</param>
/// <remarks>This method is invoked right after the value of <see cref="FirstName"/> is changed.</remarks>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.2.0.0")]
partial void OnFirstNameChanged(string? oldValue, string newValue);
```

また、見ての通り、

```cs
/// <inheritdoc cref="_firstName"/>
```

が定義されていますので、

```cs
/// <summary>
/// 名
/// </summary>
[ObservableProperty]
private string _firstName = string.Empty;
```

と記述すれば、`FirstName`プロパティにXMLドキュメントが継承されます。

さらに、
- OnFirstNameChanging
- OnFirstNameChanging
- OnFirstNameChanged
- OnFirstNameChanged

がpartialメソッドとして用意されていますので、必要であれば次のように使うことができます。

```cs
[ObservableProperty]
private string _firstName = string.Empty;

partial void OnFirstNameChanged(string? oldValue, string newValue)
{
    System.Diagnostics.Debug.WriteLine($"FirstName changed from {oldValue} to {newValue}");
}
```

## `NotifyPropertyChangedFor`属性

`NotifyPropertyChangedFor`属性も使えば、他のプロパティの変更もUIに通知するコードが自動生成されます。

```cs
[ObservableProperty]
[NotifyPropertyChangedFor(nameof(FullName))]
private string _firstName = string.Empty;
```

```cs
/// <inheritdoc cref="_firstName"/>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.2.0.0")]
[global::System.Diagnostics.CodeAnalysis.ExcludeFromCodeCoverage]
public string FirstName
{
    get => _firstName;
    [global::System.Diagnostics.CodeAnalysis.MemberNotNull("_firstName")]
    set
    {
        if (!global::System.Collections.Generic.EqualityComparer<string>.Default.Equals(_firstName, value))
        {
            OnFirstNameChanging(value);
            OnFirstNameChanging(default, value);
            OnPropertyChanging(global::CommunityToolkit.Mvvm.ComponentModel.__Internals.__KnownINotifyPropertyChangingArgs.FirstName);
            _firstName = value;
            OnFirstNameChanged(value);
            OnFirstNameChanged(default, value);
            OnPropertyChanged(global::CommunityToolkit.Mvvm.ComponentModel.__Internals.__KnownINotifyPropertyChangedArgs.FirstName);

            // ↓このコードが追加されます。
            OnPropertyChanged(global::CommunityToolkit.Mvvm.ComponentModel.__Internals.__KnownINotifyPropertyChangedArgs.FullName);
        }
    }
}
```

いかがでしたでしょうか？
`CommunityToolkit.Mvvm`NuGetパッケージを使えば、超強力なソースジェネレーターの力によって
- ボイラープレートコードの削減
- MVVMに最適化されたコードの適用

などのメリットがあります。

`CommunityToolkit.Mvvm`NuGetパッケージにはまだまだ魅力的な機能がありますので、
次回以降も紹介する予定です。

https://zenn.dev/andrewkeepcodin/articles/004-community-toolkit-mvvm-relay-command


Happy Coding!😊