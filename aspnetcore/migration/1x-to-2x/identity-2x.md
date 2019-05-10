---
title: Migrowanie uwierzytelnianie i tożsamość do ASP.NET Core 2.0
author: scottaddie
description: W tym artykule omówiono najbardziej typowe kroki dla migracji platformy ASP.NET Core 1.x uwierzytelnianie i tożsamość ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 12/18/2018
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: d11d41c82236436096660a24df81a3df4da0fb8e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898732"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Migrowanie uwierzytelnianie i tożsamość do ASP.NET Core 2.0

Przez [Scott Addie](https://github.com/scottaddie) i [Hao Kung](https://github.com/HaoK)

Platforma ASP.NET Core w wersji 2.0 ma nowy model uwierzytelniania i [tożsamości](xref:security/authentication/identity) który upraszcza konfigurację za pomocą usług. Aplikacje platformy ASP.NET Core 1.x, korzystających z uwierzytelniania lub tożsamości można aktualizować do korzystania z nowego modelu, co zostało opisane poniżej.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Oprogramowanie pośredniczące uwierzytelniania i usługi

W projektach 1.x skonfigurowano uwierzytelnianie za pomocą oprogramowania pośredniczącego. Metoda oprogramowanie pośredniczące jest wywoływane dla każdego schematu uwierzytelniania, które mają być obsługiwane.

W poniższym przykładzie 1.x konfiguruje uwierzytelnianie serwisu Facebook przy użyciu tożsamości w *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions {
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
}
```

W projektach 2.0 skonfigurowano uwierzytelnianie za pośrednictwem usług. Zarejestrowanie każdego schematu uwierzytelniania `ConfigureServices` metody *Startup.cs*. `UseIdentity` Metoda zostaje zastąpiona opcją `UseAuthentication`.

Poniższy przykład 2.0 umożliwia skonfigurowanie uwierzytelniania serwisu Facebook przy użyciu tożsamości w *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

`UseAuthentication` Metoda dodaje składnik oprogramowania pośredniczącego uwierzytelniania pojedynczego jest odpowiedzialny za automatyczne uwierzytelnianie i obsługę żądań uwierzytelniania zdalnego. Zastępuje wszystkie składniki oprogramowania pośredniczącego poszczególnych składników oprogramowania pośredniczącego jednej, wspólnej.

Poniżej przedstawiono 2.0 instrukcje migracji dla każdego schematu uwierzytelniania głównych.

### <a name="cookie-based-authentication"></a>Na podstawie plików cookie uwierzytelniania

Wybierz jedną z dwóch poniższych opcji i wprowadź niezbędne zmiany w *Startup.cs*:

1. Używanie plików cookie z tożsamością
    - Zastąp `UseIdentity` z `UseAuthentication` w `Configure` metody:

        ```csharp
        app.UseAuthentication();
        ```

    - Wywoływanie `AddIdentity` method in Class metoda `ConfigureServices` metody w celu dodania usługi uwierzytelniania pliku cookie.
    - Opcjonalnie można wywołać `ConfigureApplicationCookie` lub `ConfigureExternalCookie` method in Class metoda `ConfigureServices` metodę, aby dostosować ustawienia plików cookie tożsamości.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Używanie plików cookie bez użycia produktu Identity
    - Zastąp `UseCookieAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:

        ```csharp
        app.UseAuthentication();
        ```

    - Wywoływanie `AddAuthentication` i `AddCookie` metody `ConfigureServices` metody:

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User,
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options =>
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a>Uwierzytelniania elementu nośnego JWT

Wprowadź następujące zmiany w *Startup.cs*:
- Zastąp `UseJwtBearerAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywoływanie `AddJwtBearer` method in Class metoda `ConfigureServices` metody:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Ten fragment kodu nie używa tożsamości, więc należy ustawić domyślny schemat, przekazując `JwtBearerDefaults.AuthenticationScheme` do `AddAuthentication` metody.

### <a name="openid-connect-oidc-authentication"></a>Uwierzytelnianie OpenID Connect (OIDC)

Wprowadź następujące zmiany w *Startup.cs*:

- Zastąp `UseOpenIdConnectAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywoływanie `AddOpenIdConnect` method in Class metoda `ConfigureServices` metody:

    ```csharp
    services.AddAuthentication(options =>
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a>Uwierzytelnianie przy użyciu usługi Facebook

Wprowadź następujące zmiany w *Startup.cs*:
- Zastąp `UseFacebookAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywoływanie `AddFacebook` method in Class metoda `ConfigureServices` metody:

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Uwierzytelnianie przy użyciu usługi Google

Wprowadź następujące zmiany w *Startup.cs*:
- Zastąp `UseGoogleAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywoływanie `AddGoogle` method in Class metoda `ConfigureServices` metody:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a>Uwierzytelnianie za pomocą Account firmy Microsoft

Wprowadź następujące zmiany w *Startup.cs*:
- Zastąp `UseMicrosoftAccountAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywoływanie `AddMicrosoftAccount` method in Class metoda `ConfigureServices` metody:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a>Uwierzytelnianie przy użyciu usługi Twitter

Wprowadź następujące zmiany w *Startup.cs*:
- Zastąp `UseTwitterAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywoływanie `AddTwitter` method in Class metoda `ConfigureServices` metody:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Ustawienie domyślne schematy uwierzytelniania

W 1.x `AutomaticAuthenticate` i `AutomaticChallenge` właściwości [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) miały klasy bazowej należy ustawić na jeden schemat uwierzytelniania. Nie było dobrym sposobem wymuszania tej reguły.

W wersji 2.0, te dwie właściwości zostały usunięte jako właściwości na poszczególnych `AuthenticationOptions` wystąpienia. Można je skonfigurować w `AddAuthentication` wywołania metody w ramach `ConfigureServices` metody *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

W poprzednim fragmencie kodu ustawiono domyślny schemat `CookieAuthenticationDefaults.AuthenticationScheme` ("plików cookie" ").

Można również użyć przeciążoną wersję `AddAuthentication` metodę, aby ustawić więcej niż jednej właściwości. W poniższym przykładzie użycie metody przeciążonej ustawiono domyślny schemat `CookieAuthenticationDefaults.AuthenticationScheme`. Alternatywnie można określić schemat uwierzytelniania w ramach swojego osobistego `[Authorize]` atrybutów lub zasad autoryzacji.

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Zdefiniuj domyślny schemat w wersji 2.0, jeśli jest spełniony jeden z następujących warunków:
- Użytkownik, który ma być automatycznie zalogowany
- Możesz użyć `[Authorize]` atrybutu lub autoryzacji zasad bez określania schematów

Wyjątkiem od tej reguły jest `AddIdentity` metody. Ta metoda dodaje pliki cookie i ustawia domyślne uwierzytelniania i żądanie schematy do pliku cookie aplikacji `IdentityConstants.ApplicationScheme`. Ponadto ustawia domyślny schemat Zaloguj się do zewnętrznego pliku cookie `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Korzystanie z rozszerzeń uwierzytelniania HttpContext

`IAuthenticationManager` Interfejs jest główny punkt wejścia do 1.x systemu uwierzytelniania. Jest zastępowany przy użyciu nowego zestawu `HttpContext` metody rozszerzające w `Microsoft.AspNetCore.Authentication` przestrzeni nazw.

Na przykład 1.x projektów odwołanie `Authentication` właściwości:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

W projektach 2.0, należy zaimportować `Microsoft.AspNetCore.Authentication` przestrzeni nazw i usuwanie `Authentication` odwołania do właściwości:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Uwierzytelnianie Windows (plik HTTP.sys / IISIntegration)

Istnieją dwie odmiany uwierzytelniania Windows:
1. Host zezwala tylko uwierzytelnieni użytkownicy
2. Host umożliwia zarówno anonimowe i uwierzytelnionych użytkowników

Pierwsza wersja opisanych powyżej jest niezależny od zmiany 2.0.

Druga opisanych powyżej jest wpływ zmiany 2.0. Na przykład może być co anonimowych użytkowników w swojej aplikacji w usługach IIS lub [HTTP.sys](xref:fundamentals/servers/httpsys) warstwy, ale autoryzowanie użytkowników na poziomie kontrolera. W tym scenariuszu ustawiono domyślny schemat `IISDefaults.AuthenticationScheme` w `Startup.ConfigureServices` metody:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

Można ustawić domyślny schemat zapobiega odpowiednio żądania autoryzacji jako wezwania od pracy.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions instances

Efektem zmian 2.0 jest po przełączeniu na użycie za pomocą o nazwie Opcje zamiast wystąpień opcje pliku cookie. Możliwość dostosowania nazw systemu plików cookie tożsamości jest usuwany.

Na przykład 1.x projektów użyj [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) do przekazania `IdentityCookieOptions` parametru do *AccountController.cs*. Schemat uwierzytelniania zewnętrznego pliku cookie jest dostępny z udostępnionego wystąpienia:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

Iniekcji wyżej konstruktora staje się niepotrzebne w projektach 2.0 i `_externalCookieScheme` można usunąć pola:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

`IdentityConstants.ExternalScheme` — Stała, które mogą być używane bezpośrednio:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Dodawanie obiektów POCO IdentityUser właściwości nawigacji

Właściwości nawigacji Core Entity Framework (EF) podstawy `IdentityUser` POCO (zwykłe stare obiektu CLR) zostały usunięte. Jeśli projekt 1.x tych właściwości, ręcznie dodać je do projektu 2.0:

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

Aby zapobiec zduplikowane klucze obce podczas uruchamiania programu EF Core migracji, należy dodać następujące polecenie, aby Twoje `IdentityDbContext` klasy `OnModelCreating` — metoda (po `base.OnModelCreating();` wywołania):

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Core Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Core Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a>Zastąp GetExternalAuthenticationSchemes

Metoda synchroniczna `GetExternalAuthenticationSchemes` został usunięty na rzecz asynchroniczna wersja. projekty 1.x znajduje się następujący kod *ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Metoda ta pojawia się w *Login.cshtml* za:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

W projektach 2.0, należy użyć `GetExternalAuthenticationSchemesAsync` metody:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

W *Login.cshtml*, `AuthenticationScheme` właściwość uzyskuje dostęp w `foreach` pętli zmieni się na `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Zmiana właściwości ManageLoginsViewModel

A `ManageLoginsViewModel` obiekt jest używany w `ManageLogins` akcji *ManageController.cs*. W 1.x projektach obiekt firmy `OtherLogins` właściwości typ zwracany jest `IList<AuthenticationDescription>`. Ten typ zwracany wymaga importowania `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

W projektach 2.0, zwracany typ zmienia się na `IList<AuthenticationScheme>`. Ten nowy typ zwracany wymaga zastąpienia `Microsoft.AspNetCore.Http.Authentication` zaimportuj za pomocą `Microsoft.AspNetCore.Authentication` importowania.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać więcej informacji i dyskusji, zobacz [dyskusję odnośnie do uwierzytelniania w wersji 2.0](https://github.com/aspnet/Security/issues/1338) problemu w usłudze GitHub.
