---
title: Migrowanie uwierzytelniania i tożsamości do ASP.NET Core 2,0
author: scottaddie
description: W tym artykule przedstawiono najczęstsze kroki migracji ASP.NET Core 1. x uwierzytelnianie i tożsamość do ASP.NET Core 2,0.
ms.author: scaddie
ms.date: 06/21/2019
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: af905f1127d504839f66d9e0e1ca1dfc27e32772
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667609"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Migrowanie uwierzytelniania i tożsamości do ASP.NET Core 2,0

Przez [Scott Addie](https://github.com/scottaddie) i [Hao Kung](https://github.com/HaoK)

ASP.NET Core 2,0 ma nowy model uwierzytelniania i [tożsamości](xref:security/authentication/identity) , który upraszcza konfigurację przy użyciu usług. ASP.NET Core 1. x aplikacje używające uwierzytelniania lub tożsamości można zaktualizować, aby użyć nowego modelu, jak opisano poniżej.

## <a name="update-namespaces"></a>Aktualizowanie przestrzeni nazw

W 1. x klasy takie `IdentityRole` i `IdentityUser` zostały znalezione w przestrzeni nazw `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

W 2,0 obszar nazw <xref:Microsoft.AspNetCore.Identity> stał się nowym domem dla kilku takich klas. W przypadku domyślnego kodu tożsamości klasy, których to dotyczy, obejmują `ApplicationUser` i `Startup`. Dostosuj instrukcje `using`, aby rozwiązać odnośne odwołania.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Uwierzytelnianie i oprogramowanie pośredniczące

W projektach 1. x uwierzytelnianie jest konfigurowane za pośrednictwem oprogramowania pośredniczącego. Metoda pośrednicząca jest wywoływana dla każdego schematu uwierzytelniania, który ma być obsługiwany.

Poniższy przykład 1. x konfiguruje uwierzytelnianie w serwisie Facebook przy użyciu tożsamości w *Startup.cs*:

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

W przypadku projektów 2,0 uwierzytelnianie jest konfigurowane za pośrednictwem usług. Każdy schemat uwierzytelniania jest rejestrowany w metodzie `ConfigureServices` *Startup.cs*. Metoda `UseIdentity` jest zastępowana `UseAuthentication`.

Poniższy przykład 2,0 konfiguruje uwierzytelnianie w serwisie Facebook o tożsamości w *Startup.cs*:

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

Metoda `UseAuthentication` dodaje pojedynczy składnik pośredniczący uwierzytelniania, który jest odpowiedzialny za automatyczne uwierzytelnianie i obsługę żądań uwierzytelniania zdalnego. Zastępuje wszystkie poszczególne składniki pośredniczące pojedynczym, wspólnym składnikiem pośredniczącym.

Poniżej przedstawiono 2,0 instrukcje dotyczące migracji dla każdego głównego schematu uwierzytelniania.

### <a name="cookie-based-authentication"></a>Uwierzytelnianie na podstawie plików cookie

Wybierz jedną z dwóch opcji poniżej i wprowadź niezbędne zmiany w programie *Startup.cs*:

1. Używanie plików cookie z tożsamością
    - Zastąp `UseIdentity` `UseAuthentication` w metodzie `Configure`:

        ```csharp
        app.UseAuthentication();
        ```

    - Wywołaj metodę `AddIdentity` w metodzie `ConfigureServices`, aby dodać usługi uwierzytelniania plików cookie.
    - Opcjonalnie Wywołaj metodę `ConfigureApplicationCookie` lub `ConfigureExternalCookie` w metodzie `ConfigureServices`, aby dostosować ustawienia plików cookie tożsamości.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Używanie plików cookie bez tożsamości
    - Zastąp wywołanie metody `UseCookieAuthentication` w metodzie `Configure` `UseAuthentication`:

        ```csharp
        app.UseAuthentication();
        ```

    - Wywołaj metody `AddAuthentication` i `AddCookie` w metodzie `ConfigureServices`:

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

### <a name="jwt-bearer-authentication"></a>Uwierzytelnianie okaziciela JWT

Wprowadź następujące zmiany w programie *Startup.cs*:
- Zastąp wywołanie metody `UseJwtBearerAuthentication` w metodzie `Configure` `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywołaj metodę `AddJwtBearer` w metodzie `ConfigureServices`:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Ten fragment kodu nie używa tożsamości, dlatego należy ustawić domyślny schemat, przekazując `JwtBearerDefaults.AuthenticationScheme` do metody `AddAuthentication`.

### <a name="openid-connect-oidc-authentication"></a>Uwierzytelnianie OpenID Connect (OIDC)

Wprowadź następujące zmiany w programie *Startup.cs*:

- Zastąp wywołanie metody `UseOpenIdConnectAuthentication` w metodzie `Configure` `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywołaj metodę `AddOpenIdConnect` w metodzie `ConfigureServices`:

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

- Zastąp Właściwość `PostLogoutRedirectUri` w akcji `OpenIdConnectOptions` `SignedOutRedirectUri`:

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a>Uwierzytelnianie za pomocą konta Facebook

Wprowadź następujące zmiany w programie *Startup.cs*:
- Zastąp wywołanie metody `UseFacebookAuthentication` w metodzie `Configure` `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywołaj metodę `AddFacebook` w metodzie `ConfigureServices`:

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Uwierzytelnianie za pomocą konta Google

Wprowadź następujące zmiany w programie *Startup.cs*:
- Zastąp wywołanie metody `UseGoogleAuthentication` w metodzie `Configure` `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywołaj metodę `AddGoogle` w metodzie `ConfigureServices`:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a>Uwierzytelnianie za pomocą konta Microsoft

Aby uzyskać więcej informacji na temat uwierzytelniania konto Microsoft, zobacz [ten problem](https://github.com/dotnet/AspNetCore.Docs/issues/14455)w usłudze GitHub.

Wprowadź następujące zmiany w programie *Startup.cs*:
- Zastąp wywołanie metody `UseMicrosoftAccountAuthentication` w metodzie `Configure` `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywołaj metodę `AddMicrosoftAccount` w metodzie `ConfigureServices`:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a>Uwierzytelnianie za pomocą konta Twitter

Wprowadź następujące zmiany w programie *Startup.cs*:
- Zastąp wywołanie metody `UseTwitterAuthentication` w metodzie `Configure` `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Wywołaj metodę `AddTwitter` w metodzie `ConfigureServices`:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Ustawianie domyślnych schematów uwierzytelniania

W 1. x właściwości `AutomaticAuthenticate` i `AutomaticChallenge` klasy bazowej [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) zostały przeznaczone do ustawienia w jednym schemacie uwierzytelniania. Nie było dobrym sposobem wymuszenia tego.

W 2,0 te dwie właściwości zostały usunięte jako właściwości dla poszczególnych wystąpień `AuthenticationOptions`. Można je skonfigurować w wywołaniu metody `AddAuthentication` w ramach metody `ConfigureServices` *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

W poprzednim fragmencie kodu domyślny schemat jest ustawiany na `CookieAuthenticationDefaults.AuthenticationScheme` ("pliki cookie").

Alternatywnie możesz użyć przeciążonej wersji metody `AddAuthentication`, aby ustawić więcej niż jedną właściwość. W poniższym przykładzie przeciążonej metody domyślny schemat jest ustawiany na `CookieAuthenticationDefaults.AuthenticationScheme`. Schemat uwierzytelniania można także określić w ramach poszczególnych atrybutów `[Authorize]` lub zasad autoryzacji.

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Zdefiniuj domyślny schemat w 2,0, jeśli spełniony jest jeden z następujących warunków:
- Chcesz, aby użytkownik był zalogowany automatycznie
- Używasz atrybutu `[Authorize]` lub zasad autoryzacji bez określania schematów

Wyjątkiem od tej reguły jest metoda `AddIdentity`. Ta metoda umożliwia dodanie plików cookie i ustawienie domyślnych schematów uwierzytelniania i wyzwania do `IdentityConstants.ApplicationScheme`plików cookie aplikacji. Ponadto ustawia domyślny schemat logowania na zewnętrzny plik cookie `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Korzystanie z rozszerzeń kontekstu uwierzytelniania

Interfejs `IAuthenticationManager` jest głównym punktem wejścia do systemu uwierzytelniania 1. x. Został on zastąpiony nowym zestawem metod rozszerzenia `HttpContext` w przestrzeni nazw `Microsoft.AspNetCore.Authentication`.

Na przykład projekty 1. x odwołują się do właściwości `Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

W projektach 2,0 zaimportuj przestrzeń nazw `Microsoft.AspNetCore.Authentication` i Usuń `Authentication` odwołania do właściwości:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Uwierzytelnianie systemu Windows (HTTP. sys/IISIntegration)

Istnieją dwie różne warianty uwierzytelniania systemu Windows:

* Host zezwala tylko uwierzytelnionym użytkownikom. Ta zmiana nie ma wpływ na 2,0 zmian.
* Host umożliwia zarówno anonimowych, jak i uwierzytelnionych użytkowników. Ta zmiana ma wpływ na zmiany 2,0. Na przykład aplikacja powinna zezwalać anonimowym użytkownikom na warstwy [usług IIS](xref:host-and-deploy/iis/index) lub [http. sys](xref:fundamentals/servers/httpsys) , ale autoryzuje użytkowników na poziomie kontrolera. W tym scenariuszu Ustaw domyślny schemat w metodzie `Startup.ConfigureServices`.

  Dla elementu [Microsoft. AspNetCore. Server. IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)Ustaw domyślny schemat na `IISDefaults.AuthenticationScheme`:

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  Dla elementu [Microsoft. AspNetCore. Server. HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)Ustaw domyślny schemat na `HttpSysDefaults.AuthenticationScheme`:

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  Nie można ustawić domyślnego schematu, aby zapobiec działaniu żądania Autoryzuj (wyzwanie) z powodu następującego wyjątku:

  > `System.InvalidOperationException`: nie określono authenticationScheme i nie znaleziono DefaultChallengeScheme.

Aby uzyskać więcej informacji, zobacz <xref:security/authentication/windowsauth>.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions instances

Efektem ubocznym zmian 2,0 jest przełączenie do użycia nazwanych opcji zamiast wystąpień opcji plików cookie. Możliwość dostosowywania nazw schematu plików cookie tożsamości jest usuwana.

Na przykład projekty 1. x wykorzystują [iniekcję konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) do przekazania parametru `IdentityCookieOptions` do *AccountController.cs* i *ManageController.cs*. Do schematu uwierzytelniania zewnętrznego pliku cookie jest uzyskiwany dostęp z podanego wystąpienia:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

Wspomniane wyżej iniekcja konstruktora nie jest konieczna w projektach 2,0 i można usunąć pole `_externalCookieScheme`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

1. x projekty używały pola `_externalCookieScheme` w następujący sposób:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

W projektach 2,0 Zastąp poprzedni kod poniższym kodem. Stałej `IdentityConstants.ExternalScheme` można używać bezpośrednio.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Usuń nowo dodane wywołanie `SignOutAsync` przez zaimportowanie następującej przestrzeni nazw:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Dodawanie właściwości nawigacji IdentityUser POCO

Wszystkie podstawowe właściwości nawigacji Entity Framework (EF) podstawowej `IdentityUser` POCO (zwykły obiekt CLR) zostały usunięte. Jeśli projekt 1. x użył tych właściwości, ręcznie dodaj je z powrotem do projektu 2,0:

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

Aby zapobiec duplikowaniu kluczy obcych podczas uruchamiania EF Core migracji, Dodaj następujące elementy do metody `IdentityDbContext` klasy "`OnModelCreating` (po wywołaniu `base.OnModelCreating();`):

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

Metoda synchroniczna `GetExternalAuthenticationSchemes` została usunięta na rzecz asynchronicznej wersji. 1. x projekty mają następujący kod w obszarze *controllers/ManageController. cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Ta metoda pojawia się w obszarze *widoki/Account/Login. cshtml* :

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

W przypadku projektów 2,0 użyj metody <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*>. Zmiana w *ManageController.cs* przypomina następujący kod:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

W polu *login. cshtml*Właściwość `AuthenticationScheme`, do której uzyskano dostęp w pętli `foreach`, zmieni się na `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Zmiana właściwości ManageLoginsViewModel

Obiekt `ManageLoginsViewModel` jest używany w akcji `ManageLogins` *ManageController.cs*. W projektach 1. x typ zwracany właściwości `OtherLogins` obiektu jest `IList<AuthenticationDescription>`. Ten typ zwracany wymaga importu `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

W projektach 2,0 typ zwracany zmieni się na `IList<AuthenticationScheme>`. Ten nowy typ zwracany wymaga zastąpienia importu `Microsoft.AspNetCore.Http.Authentication` przy importowaniu `Microsoft.AspNetCore.Authentication`.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać więcej informacji, zobacz [Omówienie problemu z uwierzytelnianiem 2,0](https://github.com/aspnet/Security/issues/1338) w witrynie GitHub.
