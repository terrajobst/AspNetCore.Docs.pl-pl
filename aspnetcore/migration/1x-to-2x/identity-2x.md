---
title: "Migrowanie uwierzytelnianie i tożsamość platformy ASP.NET Core 2.0"
author: scottaddie
description: "W tym artykule opisano najbardziej typowe kroki dotyczące migrowania platformy ASP.NET Core 1.x uwierzytelnianie i tożsamość platformy ASP.NET Core 2.0."
keywords: "Uwierzytelnianie ASP.NET Core tożsamości,"
ms.author: scaddie
manager: wpickett
ms.date: 10/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 1d8c75a21cd7110b3e414f0c600e9f05cbaeff45
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a>Migrowanie uwierzytelnianie i tożsamość platformy ASP.NET Core 2.0

Przez [Scott Addie](https://github.com/scottaddie) i [Hao Kung](https://github.com/HaoK)

Platformy ASP.NET Core 2.0 ma nowy model do uwierzytelniania i [tożsamości](xref:security/authentication/identity) który upraszcza konfigurację za pomocą usług. Może zostać zaktualizowana platformy ASP.NET Core 1.x aplikacji, które używają uwierzytelniania lub tożsamości do użycia w nowym modelu w sposób opisany poniżej.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Oprogramowanie pośredniczące uwierzytelniania i usługi
W projektach 1.x skonfigurowano uwierzytelnianie za pomocą oprogramowania pośredniczącego. Metoda oprogramowanie pośredniczące jest wywoływane dla każdego schematu uwierzytelniania, które mają być obsługiwane.

W poniższym przykładzie 1.x konfiguruje uwierzytelniania serwisu Facebook z tożsamością w *Startup.cs*:

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

W projektach 2.0 skonfigurowano uwierzytelnianie za pośrednictwem usługi. Zarejestrowanie każdego schematu uwierzytelniania `ConfigureServices` metody *Startup.cs*. `UseIdentity` Metody jest zastępowany `UseAuthentication`.

Poniższy przykład 2.0 umożliwia skonfigurowanie uwierzytelniania serwisu Facebook tożsamość w *Startup.cs*:

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

`UseAuthentication` Metoda dodaje składnik oprogramowania pośredniczącego uwierzytelniania pojedynczego jest odpowiedzialny za automatyczne uwierzytelnianie i obsługę żądań uwierzytelniania zdalnego. Go zastępuje wszystkie składniki oprogramowania pośredniczącego poszczególnych składników oprogramowania pośredniczącego jednej, wspólnej.

Poniżej przedstawiono 2.0 instrukcje migracji dla każdego schematu uwierzytelniania głównych.

### <a name="cookie-based-authentication"></a>Uwierzytelnianie na podstawie plików cookie
Wybierz jedną z dwóch poniższych opcji, a następnie wprowadź niezbędne zmiany w *Startup.cs*:

1. Używanie plików cookie z tożsamością
    - Zastąp `UseIdentity` z `UseAuthentication` w `Configure` metody:

        ```csharp
        app.UseAuthentication();
        ```

    - Wywołanie `AddIdentity` metoda `ConfigureServices` metody w celu dodania usługi uwierzytelniania pliku cookie.
    - Opcjonalnie można wywołać `ConfigureApplicationCookie` lub `ConfigureExternalCookie` metoda `ConfigureServices` metodę, aby dostosować ustawienia plików cookie tożsamości.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Używanie plików cookie bez tożsamości
    - Zastąp `UseCookieAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - Wywołanie `AddAuthentication` i `AddCookie` metod w `ConfigureServices` metody:

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
- Zastąp `UseJwtBearerAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Wywołanie `AddJwtBearer` metoda `ConfigureServices` metody:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Następujący fragment kodu nie są używane tożsamości, więc domyślny schemat powinien być ustawiony przez przekazanie `JwtBearerDefaults.AuthenticationScheme` do `AddAuthentication` metody.

### <a name="openid-connect-oidc-authentication"></a>OpenID Connect uwierzytelniania (OIDC)
Wprowadź następujące zmiany w *Startup.cs*:

- Zastąp `UseOpenIdConnectAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywołanie `AddOpenIdConnect` metoda `ConfigureServices` metody:

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

### <a name="facebook-authentication"></a>Uwierzytelniania serwisu Facebook
Wprowadź następujące zmiany w *Startup.cs*:
- Zastąp `UseFacebookAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Wywołanie `AddFacebook` metoda `ConfigureServices` metody:
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Uwierzytelniania serwisu Google
Wprowadź następujące zmiany w *Startup.cs*:
- Zastąp `UseGoogleAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Wywołanie `AddGoogle` metoda `ConfigureServices` metody:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a>Uwierzytelnianie konta Microsoft
Wprowadź następujące zmiany w *Startup.cs*:
- Zastąp `UseMicrosoftAccountAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywołanie `AddMicrosoftAccount` metoda `ConfigureServices` metody:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a>Uwierzytelnianie w usłudze Twitter
Wprowadź następujące zmiany w *Startup.cs*:
- Zastąp `UseTwitterAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Wywołanie `AddTwitter` metoda `ConfigureServices` metody:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Ustawienie domyślne schematy uwierzytelniania
W 1.x `AutomaticAuthenticate` i `AutomaticChallenge` właściwości [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) miały klasy podstawowej można ustawić na jeden schemat uwierzytelniania. Nie można było dobrym sposobem wyegzekwować to.

W 2.0, te dwie właściwości zostały usunięte jako właściwości na poszczególnych `AuthenticationOptions` wystąpienia. Można je skonfigurować w `AddAuthentication` wywołania metody w ramach `ConfigureServices` metody *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

W poprzednim fragmencie kodu, domyślny schemat ma ustawioną wartość `CookieAuthenticationDefaults.AuthenticationScheme` ("plików cookie").

Można również użyć zastąpionej wersji `AddAuthentication` metodę, aby ustawić więcej niż jedną właściwość. W poniższym przykładzie przeciążonej metody domyślny schemat ma ustawioną wartość `CookieAuthenticationDefaults.AuthenticationScheme`. Alternatywnie można określić schemat uwierzytelniania w ramach poszczególnych sieci `[Authorize]` atrybuty lub zasad autoryzacji.

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Zdefiniuj domyślny schemat w 2.0, jeśli jest spełniony jeden z następujących warunków:
- Użytkownik, który ma być automatycznie zalogowany
- Możesz użyć `[Authorize]` atrybutu lub autoryzacji zasady bez określenia schematów

Wyjątkiem od tej reguły jest `AddIdentity` metody. Ta metoda dodaje pliki cookie i ustawia domyślne uwierzytelniania i żądanie schematy do pliku cookie aplikacji `IdentityConstants.ApplicationScheme`. Ponadto ustawia domyślny schemat logowania zewnętrznego pliku cookie `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Element HttpContext uwierzytelniania rozszerzenia
`IAuthenticationManager` Interfejs jest główny punkt wejścia do systemu 1.x uwierzytelniania. Go zostało zastąpione przez nowy zestaw `HttpContext` metody rozszerzenia w `Microsoft.AspNetCore.Authentication` przestrzeni nazw.

Na przykład 1.x projektów odwołanie `Authentication` właściwości:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

W projektach 2.0, należy zaimportować `Microsoft.AspNetCore.Authentication` przestrzeni nazw i Usuń `Authentication` odwołań do właściwości:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Uwierzytelnianie systemu Windows (plik HTTP.sys / IISIntegration)
Istnieją dwie odmiany uwierzytelniania systemu Windows:
1. Host zezwala tylko uwierzytelnionych użytkowników
2. Host umożliwia zarówno anonimowego i użytkownicy uwierzytelnieni

Pierwsza opisane powyżej nie mają wpływu zmiany 2.0.

Druga opisany powyżej jest wpływ zmiany 2.0. Na przykład może być stosowanie anonimowych użytkowników do aplikacji w usługach IIS lub [HTTP.sys](xref:fundamentals/servers/weblistener) warstwy ale autoryzowanie użytkowników na poziomie kontrolera. W tym scenariuszu Ustaw domyślny schemat `IISDefaults.AuthenticationScheme` w `ConfigureServices` metody *Startup.cs*:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

Nie można ustawić domyślnego schematu zapobiega odpowiednio żądania autoryzacji zażąda działać.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions wystąpień
Efektem ubocznym zmiany 2.0 jest przełącznika tak, aby przy użyciu o nazwie Opcje zamiast wystąpień opcje pliku cookie. Możliwość dostosowania nazwy schematu pliku cookie tożsamości jest usuwany.

Na przykład 1.x projektów użyj [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) do przekazania `IdentityCookieOptions` parametru do *AccountController.cs*. Schemat uwierzytelniania zewnętrzny plik cookie jest dostępny z udostępnionego wystąpienia:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

Iniekcji wyżej wymienione konstruktora staje się niepotrzebne w projektach 2.0 i `_externalCookieScheme` można usunąć pola:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

`IdentityConstants.ExternalScheme` Stała mogą być używane bezpośrednio:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Dodaj właściwości nawigacji POCO IdentityUser
Właściwości nawigacji Core Entity Framework (EF) podstawowego `IdentityUser` POCO (zwykły stary obiekt CLR) zostały usunięte. Jeśli projekt 1.x używane te właściwości, ręcznie dodać je do projektu 2.0:

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

Aby uniknąć zduplikowanych kluczy obcych podczas uruchamiania migracji Core EF, Dodaj następujący kod do Twojej `IdentityDbContext` klasy `OnModelCreating` — metoda (po `base.OnModelCreating();` wywołać):

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
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
Metoda synchroniczna `GetExternalAuthenticationSchemes` usunięto na rzecz asynchroniczną wersję. projekty 1.x mają następujący kod *ManageController.cs*:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Ta metoda jest wyświetlana w *Login.cshtml* za:

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

W projektach 2.0, należy użyć `GetExternalAuthenticationSchemesAsync` metody:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

W *Login.cshtml*, `AuthenticationScheme` dostęp do właściwości `foreach` pętli zmienia się na `Name`:

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Zmiana właściwości ManageLoginsViewModel
A `ManageLoginsViewModel` obiekt jest używany w `ManageLogins` akcji *ManageController.cs*. W 1.x projektów, obiekt w `OtherLogins` właściwości zwracany typ jest `IList<AuthenticationDescription>`. Ten typ zwracany wymaga importowania `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

W projektach 2.0, zwracany typ zmiany `IList<AuthenticationScheme>`. Ten nowy typ zwracany wymaga zastępowanie `Microsoft.AspNetCore.Http.Authentication` importowania `Microsoft.AspNetCore.Authentication` zaimportować.

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Dodatkowe zasoby
Dodatkowe szczegóły oraz omówienie, zobacz [dyskusji 2.0 uwierzytelniania](https://github.com/aspnet/Security/issues/1338) problem w witrynie GitHub.
