---
title: Autoryzuj przy użyciu określonego schematu w programie ASP.NET Core
author: rick-anderson
description: W tym artykule opisano sposób ograniczenia tożsamości do określonego schematu podczas pracy z wielu metod uwierzytelniania.
ms.author: riande
ms.date: 10/22/2018
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 778bb61f472ab2e76f85da5999d3c79238188f19
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903004"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Autoryzuj przy użyciu określonego schematu w programie ASP.NET Core

W niektórych scenariuszach, takich jak aplikacje jednostronicowe (źródła) jest często używa wielu metod uwierzytelniania. Na przykład aplikacja może używać na podstawie plików cookie uwierzytelniania do logowania i uwierzytelniania elementu nośnego tokenu JWT dla żądań JavaScript. W niektórych przypadkach aplikacja może mieć wiele wystąpień do obsługi uwierzytelniania. Na przykład dwa obsługi plików cookie, w których jedna zawiera podstawowe tożsamości, a drugi jest tworzony podczas została wyzwolona usługi Multi-Factor authentication (MFA). Może zostać wyzwolone MFA, ponieważ użytkownik zażądał operacji, która wymaga zapewnienia dodatkowego bezpieczeństwa.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Schemat uwierzytelniania nosi nazwę po skonfigurowaniu usługi uwierzytelniania podczas uwierzytelniania. Na przykład:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

W poprzednim kodzie zostały dodane dwa obsługi uwierzytelniania: jeden dla plików cookie i jeden dla elementu nośnego.

>[!NOTE]
>Określanie domyślnego schematu powoduje `HttpContext.User` ustawioną na tej tożsamości. Jeśli to zachowanie nie jest konieczne, ją wyłączyć, wywołując bez parametrów formie `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Schematy uwierzytelniania są nazywane po skonfigurowaniu uwierzytelniania middlewares podczas uwierzytelniania. Na przykład:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

W poprzednim kodzie zostały dodane dwa middlewares uwierzytelniania: jeden dla plików cookie i jeden dla elementu nośnego.

>[!NOTE]
>Określanie domyślnego schematu powoduje `HttpContext.User` ustawioną na tej tożsamości. Jeśli to zachowanie nie jest konieczne, ją wyłączyć, ustawiając `AuthenticationOptions.AutomaticAuthenticate` właściwość `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Wybieranie schematu z atrybutem autoryzacji

Punkcie autoryzacji aplikacja wskazuje obsługi, który ma być używany. Wybierz program obsługi, za pomocą którego aplikacja będzie autoryzować przez przekazanie rozdzielana przecinkami lista schematów uwierzytelniania na `[Authorize]`. `[Authorize]` Atrybut Określa schemat uwierzytelniania lub schematów niezależnie od tego, czy domyślny jest skonfigurowany. Na przykład:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

W powyższym przykładzie obsługi plików cookie i elementu nośnego Uruchom i masz szansę, aby utworzyć i dołączyć tożsamości dla bieżącego użytkownika. Określając tylko jednego schematu, odpowiedni program obsługi jest uruchamiany.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

W poprzednim kodzie obsługi ze schematem "Bearer" działa. Wszystkie tożsamości oparte na pliku cookie są ignorowane.

## <a name="selecting-the-scheme-with-policies"></a>Wybieranie schematu przy użyciu zasad

Jeśli chcesz określić żądany schemat w [zasad](xref:security/authorization/policies), można ustawić `AuthenticationSchemes` kolekcji podczas dodawania zasad:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

W powyższym przykładzie zasady "Over18" działa tylko względem utworzonej przez procedurę obsługi "Bearer" tożsamości. Użyj zasad, ustawiając `[Authorize]` atrybutu `Policy` właściwości:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a>Użyj wielu schematów uwierzytelniania

Niektóre aplikacje może być konieczne obsługuje wiele typów uwierzytelniania. Na przykład aplikacja może uwierzytelniać użytkowników z usługi Azure Active Directory i z bazy danych użytkowników. Innym przykładem jest aplikacja, która uwierzytelnia użytkowników z usług federacyjnych Active Directory i Azure Active Directory B2C. W takim przypadku aplikacja powinna obsługiwać tokenu elementu nośnego JWT z kilku wystawców.

Dodaj wszystkie schematy uwierzytelniania, który chcesz zaakceptować. Na przykład, poniższy kod w `Startup.ConfigureServices` dodaje dwa schematy uwierzytelniania elementu nośnego tokenu JWT z różnych wydawców:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> Tylko jeden uwierzytelniania elementu nośnego tokenu JWT jest zarejestrowana przy użyciu domyślnego schematu uwierzytelniania `JwtBearerDefaults.AuthenticationScheme`. Dodatkowe uwierzytelnianie musi być zarejestrowane przy użyciu schematu; unikatowe uwierzytelnianie.

Następnym krokiem jest zaktualizuj domyślne zasady autoryzacji do akceptowania zarówno schematów uwierzytelniania. Na przykład:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

Jako domyślne zasady autoryzacji jest zastępowany, jest możliwe użycie `[Authorize]` atrybutu w kontrolerach. Kontroler akceptuje żądania następnie, przy użyciu tokenu JWT wystawione przez wystawcę pierwszej lub drugiej.

::: moniker-end
