# DotnetBlazorAuth.Server

## 概要
`blazor --interactivity Server` で作成したプロジェクトに認証を追加する。 

### 参照
.NET 8 の Blazor にオレオレ ログイン機能を付けよう  
https://zenn.dev/microsoft/articles/aspnetcore-blazor-dotnet8-tryaddauth  

## 認証を使うための準備

### Program.cs に認証関係の処理を追加
`Program.cs`

※注意！  
元記事の「認証認可のミドルウェアを追加」の位置では以下のエラーが発生するため注意
* Blazor 認証認可で「A valid antiforgery token was not provided with the request.」エラーが発生した場合の対応 2024-08-08  
  https://hackmd.io/@jkofK/r11pxez90  
  > app.UseAuthorization(); の順番を入れ替えると正常に動作する。
* Blazor antiforgery token issue when posting form (SSR) when user is logged in #50612  
  https://github.com/dotnet/aspnetcore/issues/50612#issuecomment-1722177730  

```cs
using Microsoft.AspNetCore.Authentication.Cookies;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Server;

～～～～～～～～～～～～～～～～～～～～～～～～～～

// 認証系サービスの追加
builder.Services
    .AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie();

// Blazor用の認証情報を提供するためのコンポーネント
builder.Services.AddScoped<AuthenticationStateProvider, ServerAuthenticationStateProvider>();

// 認証情報を CascadingParameter で渡すようにする
builder.Services.AddCascadingAuthenticationState();

～～～～～～～～～～～～～～～～～～～～～～～～～～

// 認証認可のミドルウェアを追加 ※app.UseAntiforgery() より前に追加すること
app.UseAuthentication();
app.UseAuthorization();
```

### _Imports.razor に using を追加
`Components\_Imports.razor`  
```html
@using Microsoft.AspNetCore.Components.Authorization
```

## 認証を使ったページを追加

### ログインページを作成
`Components\Pages\Login.razor`  


### ログアウトページを作成 ※元記事には無いです
`Components\Pages\Logout.razor`  

### Home ページにログインユーザー名表示を追加
`Components\Pages\Home.razor`  

```html
<div>
    <AuthorizeView>
        @context.User.Identity!.Name さん、こんにちは。
    </AuthorizeView>

    <AuthorizeView Roles="Administrator">
        あなたのロールは管理者です。
    </AuthorizeView>

    <AuthorizeView Roles="User">
        あなたのロールは一般ユーザーです。
    </AuthorizeView>
</div>
```

### InteractiveServer ページを追加
`Components\Pages\InteractiveServerPage.razor`  

### 追加したページをメニューに追加
`Components\Layout\NavMenu.razor`   
```html
        <div class="nav-item px-3">
            <NavLink class="nav-link" href="interactiveserver">
                <span class="bi bi-list-nested-nav-menu" aria-hidden="true"></span> InteractiveServer
            </NavLink>
        </div>

        <div class="nav-item px-3">
            <NavLink class="nav-link" href="login">
                <span class="bi bi-list-nested-nav-menu" aria-hidden="true"></span> ログイン
            </NavLink>
        </div>

        <div class="nav-item px-3">
            <NavLink class="nav-link" href="logout">
                <span class="bi bi-list-nested-nav-menu" aria-hidden="true"></span> ログアウト
            </NavLink>
        </div>
```

