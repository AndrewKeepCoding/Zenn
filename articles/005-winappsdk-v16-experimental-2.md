---
title: "WinAppSDK v1.6 Experimental-2"
emoji: "⚗️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["WinUI", "WinUI3", "WindowsAppSDK", "XAML"]
published: true
---

https://youtu.be/_Fgikgr7sno?si=Xyn_0N7ksWPnplbr

WinAppSKDのNuGetパッケージは、

- [Stable](https://learn.microsoft.com/ja-jp/windows/apps/windows-app-sdk/stable-channel)
- [Preview](https://learn.microsoft.com/ja-jp/windows/apps/windows-app-sdk/preview-channel)
- [Experimental](https://learn.microsoft.com/ja-jp/windows/apps/windows-app-sdk/experimental-channel)

の3つのリリースチャンネルで展開されていますが、今回は**Experimental**を試す方法を紹介します。

:::message alert
Experimentalは、運用環境での使用ではサポートされていません。また、Experimentalを使用するアプリを Microsoft Store に公開することはできません。
:::

# WinAppSDK v1.6 Experimental-2 NuGetパッケージのインストール
1. [NuGetパッケージマネージャー]を開きます。
1. [プレリリースを含める]にチェックを入れます。
2. [更新プログラム]に移動します。
3. [Microsoft.WindowsAppSDK Prerelease]にチェックを入れます。
4. [更新]をクリックし、要求に応じて表示されるダイアログで[OK]または[同意する]をクリックします。

    ![](/images/005/nuget-package-manager.png)

# Microsoft.Windows.SDKエラーの対応
*WinAppSDK v1.6 Experimental-2*をインストールすると以下のエラーメッセージが出力されるはずです。

```
Severity Code Description Project File Line Suppression State Details Error (active)
This version of the Windows App SDK requires 
Microsoft.Windows.SDK.NET.Ref 10.0.19041.35-preview;10.0.19041.35-preview or later, 
which can be added with:
<PropertyGroup>
    <WindowsSdkPackageVersion>10.0.19041.35-preview;10.0.19041.35-preview</WindowsSdkPackageVersion>
</PropertyGroup>
```

アプリのプロジェクトファイル(*.csproj)を開き、以下のブロックを追加します。

```xml
<PropertyGroup>
    <WindowsSdkPackageVersion>10.0.19041.35-preview</WindowsSdkPackageVersion>
</PropertyGroup>
```

:::message
エラーメッセージではバージョン情報"10.0.19041.35-preview"が2回繰り返されていますが、プロジェクトファイルには1回のみにします。これは既知のバグで、次のバージョンで修正される予定です。
:::

# WinAppSDK v1.6 Experimental-2の注目点
せっかくなのでこのバージョンの個人的な注目点に触れます。尚、あくまでも*Experimental*チャンネルなので、*Stable*チャンネルでは内容が変更される可能性があります。

### Unpackagedアプリの多言語対応
これまでは*Packaged*アプリのみが多言語に対応していて、

```cs
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "ja";
```

で設定します。また、この設定は保持され、次回のアプリ起動時は設定不要です。

このリリースで*Unpackaged* アプリも多言語に対応しています。

具体的には、

```cs
Microsoft.Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "ja";
```

のように設定します。とてもややこしいですが、名前空間の先頭に"Microsoft"が追加されています。

**特記事項**

- *Microsoft.Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride*は*Packaged*でも*Unpackaged*アプリでも使うことができます。
- *Pacakged*アプリでは設定が保持されますが、*Unpackaged*アプリでは保持されません。

**GitHub**
https://github.com/AndrewKeepCoding/WinAppSDKExperimentalChannelDemo

### TitleBarコントロールバグ修正
*WinAppSDK v1.6 Experimental1*で新規追加された`TitleBar`コントロールには、とても簡単に`AutoSuggestBox`のようなコントロールを並べることができるようになりましたが、それらのコントロールがフォーカスされない(入力できない)というバグがありました。今回のリリースではこのバグも修正されています。

2024年6月に実施された**WinUI Community Call**でとても分かりやすいデモが紹介されいました。

https://www.youtube.com/watch?v=pExPo1VJ8Ks&t=1340s

Happy Coding!😊