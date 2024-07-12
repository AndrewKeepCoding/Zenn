---
title: "WinUIとは何か"
emoji: "🪟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["WinUI", "WinUI3", "WinAppSDK", "WindowsAppSDK", "XAML"]
published: true
---

# WinUIとはなにか

## 公式には
https://learn.microsoft.com/ja-jp/windows/apps/winui/

> Windows UI ライブラリ (WinUI) は、Windows デスクトップと UWP の両方のアプリケーションに対応したネイティブ ユーザー エクスペリエンス (UX) フレームワークです。

と書かれていますが、基本的にはWindowsアプリに使えるUIライブラリです。

尚、WinUIとして表現されることが多く、Windows UIはほとんど見かけません。もしかしたら、WinUI 3の方が聞いたことがあるかと思います。

## WinUI 3
WinUI 3は、WinUIの第3世代で、2021年にProject ReunionのUIとしてリリースされました。

:::message
Project Reunionは後にWindows App SDK (WinAppSDK) に名前が変更されます。
:::

WinUI 3は、v3.xxで単体リリースされず、WinAppSDKの一部としてリリースされ、現時点ではv1.5.xがリリースされています。

:::message
### 各WinUI世代
- 最初のWinUIは、2018年にPreview版のみがリリースされました。
- WinUI 2は、最初のWinUIリリースの数か月後、WinUIとして初めて製品版としてリリースされました。WinUI 2はUWP向けであり、現時点(2024/05)ではv2.8までリリースされています。
- WinUI 3は、2021年にProject ReunionのUIとしてリリースされます。Project Reunionは後にWindows App SDK (WinAppSDK) に名前が変更されます。
:::

### WinUI 3 Gallery
とりあえずWinUI 3がどんな感じなのかを試したい場合には、WinUI 3 Galleryで遊んでみてください。Microsoft Storeからダウンロードできます。
https://apps.microsoft.com/detail/9p3jfpwwdzrc?launch=true&mode=full&hl=en-us&gl=us&ocid=bingwebsearch

## WinUI 3 → WinUI?
Microsoft Build 2024では、WinUI 3ではなく、WinUIとして表現されることが多かったことから、UWPのWinUI 2と区別する必要がある場合のみWinUI 3と呼び、基本的にはWinUIに呼び方を統一していくと思われます。

## WinUI と WPF
またMicrosoft Build 2024の話になりますが、Windowsのネイティブアプリには、

- WinUI
- WPF

の2本立てを推していく方針が発表されました。

![](/images/001/winui-vs-wpf.png)

基本的には、

- WinUI: 最新のエクスペリエンスやテクノロジーを優先する場合
- WPF: 17年間の蓄積のある安定感やサードパーティの充実を優先する場合

のような住み分けになるかと思います。

尚、Microsoft Build 2024の動画はYouTubeで公開されています。
https://youtu.be/ZjaL3pL-OuM?si=BvIqghlFO5jruNWb

### 個人的な意見
Windowsネイティブアプリを新規開発する場合には、まずWinUIをお勧めします。WPFはWinUIはリリース以降、安定性・開発の透明性などで厳しい状況が続いていましたが、最近は非常に改善された印象があります。WinUIを試してみて、どうしても困難であると感じた場合にWPFを検討すれば良いかと考えています。

Happy Coding!😊



