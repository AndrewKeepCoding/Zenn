---
title: "WinUI (C#) アプリ開発環境を整えよう"
emoji: "🔰"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["WinUI", "WinUI3", "WinAppSDK", "WindowsAppSDK", "XAML"]
published: true
---

WinUI 3リリース直後は、
「このワークロードをインストールして」
「このコンポーネントも忘れないで」
という感じでとても分かりづらかったのですが、
今ではとても簡単になり、基本的には以下の公式手順に従えばOKです。

https://learn.microsoft.com/ja-jp/windows/apps/get-started/start-here?tabs=vs-2022-17-10

ここでは、公式手順をさらに簡略した手順を載せておきます。

# 1. Visual Studio 2022

https://visualstudio.microsoft.com/downloads/

# 2. ワークロードとコンポーネント

- Visual Studioインストーラーを起動します。
- [ワークロード]タブで[Windowsアプリケーション開発]にチェックを入れます。

![](/images/002/windows-application-development-workload.png)

# 3. 開発者モード

‐ Windowsの[設定]を開きます。
- [システム] > [開発者用]ページに移動します。
- [開発者モード]を[オン]に設定します。

https://learn.microsoft.com/ja-jp/windows/apps/get-started/enable-your-device-for-development


これでWinUIプロジェクトを作成できるようになります✨

Happy Coding!😊