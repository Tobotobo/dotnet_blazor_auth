# dotnet_blazor_auth

## 途中

## 概要
以下の記事を参考に `StaticSSR` `InteractiveServer` `InteractiveWebAssmelby` のそれぞれで認証を実装してみる。 

.NET 8 の Blazor にオレオレ ログイン機能を付けよう  
https://zenn.dev/microsoft/articles/aspnetcore-blazor-dotnet8-tryaddauth  

## 詳細

## プロジェクト作成
以下のそれぞれでプロジェクトを作成し認証を実装する。

-int, --interactivity <Auto|None|Server|WebAssembly>  対話型コンポーネントに使用するホスティング プラットフォームを選択します
* None         インタラクティビティなし (静的サーバー レンダリングのみ)
* Server       サーバーで実行
* WebAssembly  WebAssembly を使用してブラウザーで実行します
* Auto         WebAssembly 資産のダウンロード中にサーバーを使用してから、WebAssembly を使用します

```sh
dotnet new razorclasslib -n DotnetBlazorAuth.Shared
# dotnet add DotnetBlazorAuth.Shared package Microsoft.AspNetCore.Authentication ※古い、非推奨

dotnet new blazor --interactivity Auto -n DotnetBlazorAuth.Auto
dotnet add DotnetBlazorAuth.Auto/DotnetBlazorAuth.Auto.Client package Microsoft.AspNetCore.Components.Authorization --version 8.0.11
# dotnet add DotnetBlazorAuth.Auto/DotnetBlazorAuth.Auto reference DotnetBlazorAuth.Shared
# dotnet add DotnetBlazorAuth.Auto/DotnetBlazorAuth.Auto.Client reference DotnetBlazorAuth.Shared

dotnet new blazor --interactivity Server -n DotnetBlazorAuth.Server
```

### 実行
```sh
dotnet run --project DotnetBlazorAuth.Auto/DotnetBlazorAuth.Auto
dotnet watch --project DotnetBlazorAuth.Auto/DotnetBlazorAuth.Auto

dotnet run --project DotnetBlazorAuth.Server
dotnet watch --project DotnetBlazorAuth.Server
```



### Auto

### Shared

