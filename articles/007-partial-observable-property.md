---
title: "Partial Observable Property"
emoji: "🧩"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["WinUI", "WinUI3", "WindowsAppSDK", "XAML", "MVVM"]
published: true
---

https://youtu.be/Tu7asErZc54?si=zdzp2qJMQrceQkV0

.NET 9のリリースに[部分プロパティ](https://learn.microsoft.com/ja-jp/dotnet/csharp/language-reference/keywords/partial-member?wt.mc_id=MVP_356303)が盛り込まれ、早速[CommunityToolkit.Mvvm](https://learn.microsoft.com/ja-jp/dotnet/communitytoolkit/mvvm/?wt.mc_id=MVP_356303)NuGetパッケージのv8.4.0-preview2もリリースされました。

これにより、部分プロパティを活用した[ObservableProperty](https://learn.microsoft.com/en-us/dotnet/communitytoolkit/mvvm/generators/observableproperty?wt.mc_id=MVP_356303)が使えるようになりました。

:::message
**CommunityToolkit.Mvvm関連記事**
- [WinUI x CommunityToolkit.Mvvm [ObservableObject・ObservableProperty編]](https://zenn.dev/andrewkeepcodin/articles/003-community-toolkit-mvvm-observable-property)
- [WinUI x CommunityToolkit.Mvvm [RelayCommand編]](https://zenn.dev/andrewkeepcodin/articles/004-community-toolkit-mvvm-relay-command)
:::

具体的には、
これまでは以下のように自分で`フィールド`を書き、ソースジェネレーターにプロパティを書いてもらってましたが...

```cs
using CommunityToolkit.Mvvm.ComponentModel;
using Microsoft.UI.Xaml.Controls;

namespace CommunityToolkitMvvmPartialProperty;

public partial class SomeViewModel : ObservableObject
{
    [ObservableProperty]
    private string? _someText;
}
```

:::details 生成コード
:::message
```cs
// <auto-generated/>
#pragma warning disable
#nullable enable
namespace CommunityToolkitMvvmPartialProperty
{
    /// <inheritdoc/>
    partial class SomeViewModel
    {
        /// <inheritdoc cref="_someText"/>
        [global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.4.0.0")]
        [global::System.Diagnostics.CodeAnalysis.ExcludeFromCodeCoverage]
        public string? SomeText
        {
            get => _someText;
            set
            {
                if (!global::System.Collections.Generic.EqualityComparer<string?>.Default.Equals(_someText, value))
                {
                    OnSomeTextChanging(value);
                    OnSomeTextChanging(default, value);
                    OnPropertyChanging(global::CommunityToolkit.Mvvm.ComponentModel.__Internals.__KnownINotifyPropertyChangingArgs.SomeText);
                    _someText = value;
                    OnSomeTextChanged(value);
                    OnSomeTextChanged(default, value);
                    OnPropertyChanged(global::CommunityToolkit.Mvvm.ComponentModel.__Internals.__KnownINotifyPropertyChangedArgs.SomeText);
                }
            }
        }

        /// <summary>Executes the logic for when <see cref="SomeText"/> is changing.</summary>
        /// <param name="value">The new property value being set.</param>
        /// <remarks>This method is invoked right before the value of <see cref="SomeText"/> is changed.</remarks>
        [global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.4.0.0")]
        partial void OnSomeTextChanging(string? value);
        /// <summary>Executes the logic for when <see cref="SomeText"/> is changing.</summary>
        /// <param name="oldValue">The previous property value that is being replaced.</param>
        /// <param name="newValue">The new property value being set.</param>
        /// <remarks>This method is invoked right before the value of <see cref="SomeText"/> is changed.</remarks>
        [global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.4.0.0")]
        partial void OnSomeTextChanging(string? oldValue, string? newValue);
        /// <summary>Executes the logic for when <see cref="SomeText"/> just changed.</summary>
        /// <param name="value">The new property value that was set.</param>
        /// <remarks>This method is invoked right after the value of <see cref="SomeText"/> is changed.</remarks>
        [global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.4.0.0")]
        partial void OnSomeTextChanged(string? value);
        /// <summary>Executes the logic for when <see cref="SomeText"/> just changed.</summary>
        /// <param name="oldValue">The previous property value that was replaced.</param>
        /// <param name="newValue">The new property value that was set.</param>
        /// <remarks>This method is invoked right after the value of <see cref="SomeText"/> is changed.</remarks>
        [global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.4.0.0")]
        partial void OnSomeTextChanged(string? oldValue, string? newValue);
    }
}
```
:::

v8.4.0-preview2では、以下のように自分で`部分プロパティ`を書き、ソースジェネレーターに実装を書いてもらえるようになりました。

```cs
using CommunityToolkit.Mvvm.ComponentModel;
using Microsoft.UI.Xaml.Controls;

namespace CommunityToolkitMvvmPartialProperty;

public partial class SomeViewModel : ObservableObject
{
    [ObservableProperty]
    public partial string? SomeText { get; set; }
}
```

:::details 生成コード
:::message
```cs
// <auto-generated/>
#pragma warning disable
#nullable enable
namespace CommunityToolkitMvvmPartialProperty
{
    /// <inheritdoc/>
    partial class SomeViewModel
    {
        /// <inheritdoc/>
        [global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.4.0.0")]
        [global::System.Diagnostics.CodeAnalysis.ExcludeFromCodeCoverage]
        public partial string? SomeText
        {
            get => field;
            set
            {
                if (!global::System.Collections.Generic.EqualityComparer<string?>.Default.Equals(field, value))
                {
                    OnSomeTextChanging(value);
                    OnSomeTextChanging(default, value);
                    OnPropertyChanging(global::CommunityToolkit.Mvvm.ComponentModel.__Internals.__KnownINotifyPropertyChangingArgs.SomeText);
                    field = value;
                    OnSomeTextChanged(value);
                    OnSomeTextChanged(default, value);
                    OnPropertyChanged(global::CommunityToolkit.Mvvm.ComponentModel.__Internals.__KnownINotifyPropertyChangedArgs.SomeText);
                }
            }
        }

        /// <summary>Executes the logic for when <see cref="SomeText"/> is changing.</summary>
        /// <param name="value">The new property value being set.</param>
        /// <remarks>This method is invoked right before the value of <see cref="SomeText"/> is changed.</remarks>
        [global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.4.0.0")]
        partial void OnSomeTextChanging(string? value);
        /// <summary>Executes the logic for when <see cref="SomeText"/> is changing.</summary>
        /// <param name="oldValue">The previous property value that is being replaced.</param>
        /// <param name="newValue">The new property value being set.</param>
        /// <remarks>This method is invoked right before the value of <see cref="SomeText"/> is changed.</remarks>
        [global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.4.0.0")]
        partial void OnSomeTextChanging(string? oldValue, string? newValue);
        /// <summary>Executes the logic for when <see cref="SomeText"/> just changed.</summary>
        /// <param name="value">The new property value that was set.</param>
        /// <remarks>This method is invoked right after the value of <see cref="SomeText"/> is changed.</remarks>
        [global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.4.0.0")]
        partial void OnSomeTextChanged(string? value);
        /// <summary>Executes the logic for when <see cref="SomeText"/> just changed.</summary>
        /// <param name="oldValue">The previous property value that was replaced.</param>
        /// <param name="newValue">The new property value that was set.</param>
        /// <remarks>This method is invoked right after the value of <see cref="SomeText"/> is changed.</remarks>
        [global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.ObservablePropertyGenerator", "8.4.0.0")]
        partial void OnSomeTextChanged(string? oldValue, string? newValue);
    }
}
```
:::

メリットとしては、
- `フィールド`(`_someText`)ではなく、XAMLで実際にバインディングする`プロパティ`(`SomeText`)を書くのでコードがより直観的になった
- プロパティ名で検索しやすくなった
- プロパティのsetterを`private`にできる(`TwoWay`バインディングをできなくする)
  ```cs
  public partial string? SomeText { get; private set; }
  ```

デメリットとしては、
...今のところは思いつきません。

:::message alert
v8.4.0-preview2の生成コードで確認できますが、`field`キーワードが使用されています。
`field`キーワードはまだプレビュー機能であるため、プロジェクトファイルで言語バージョンを`preview`に設定する必要があります。
```xml
<PropertyGroup>
  <LangVersion>preview</LangVersion>
</PropertyGroup>
```
:::

今回は以上です。

Happy Coding!😊