---
title: Autoryzacja określonego schematu w ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono, jak ograniczyć tożsamości do określonego schematu podczas pracy z wielu metod uwierzytelniania.
manager: wpickett
ms.author: riande
ms.date: 10/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 81a01d7de8221fcb3bf90a108d9df6633ca2b696
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Autoryzacja określonego schematu w ASP.NET Core

W niektórych scenariuszach, takich jak aplikacje jednostronicowe (źródła) jest często wiele metod uwierzytelniania. Na przykład aplikacji mogą używać uwierzytelniania opartego na pliku cookie do logowania i uwierzytelniania elementu nośnego JWT dla żądań JavaScript. W niektórych przypadkach aplikacja może mieć wiele wystąpień program obsługi uwierzytelniania. Na przykład dwa obsługi pliku cookie, których jedna zawiera podstawowej tożsamości, a drugi jest tworzony podczas zostało wyzwolone uwierzytelnianie wieloskładnikowe (MFA). Mogą być wyzwalane MFA, ponieważ użytkownik zażądał operacji, która wymaga dodatkowych zabezpieczeń.

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

W powyższym kodzie dwóch metod obsługi uwierzytelniania zostały dodane: jeden dla plików cookie i jeden dla elementu nośnego.

>[!NOTE]
>Określanie domyślnego schematu powoduje `HttpContext.User` ustawiona do tej tożsamości. W razie potrzeby nie jest to zachowanie można ją wyłączyć, wywołując bez parametrów formę `AddAuthentication`.

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

W powyższym kodzie dodano dwa middlewares uwierzytelniania: jeden dla plików cookie i jeden dla elementu nośnego.

>[!NOTE]
>Określanie domyślnego schematu powoduje `HttpContext.User` ustawiona do tej tożsamości. W razie potrzeby nie jest to zachowanie można ją wyłączyć, ustawiając `AuthenticationOptions.AutomaticAuthenticate` właściwości `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Wybieranie schemat w atrybucie autoryzacji

W punkcie autoryzacji aplikacja wskazuje obsługi, który ma być używane. Wybierz program obsługi, z którym autoryzacji przez przekazanie rozdzielana przecinkami lista schematy uwierzytelniania do aplikacji `[Authorize]`. `[Authorize]` Atrybut Określa schemat uwierzytelniania lub systemów do użycia niezależnie od tego, czy domyślny został skonfigurowany. Na przykład:

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

W powyższym przykładzie zarówno pliku cookie i elementu nośnego Uruchom i mieć możliwość Utwórz i Dołącz tożsamości dla bieżącego użytkownika. Umożliwia określenie tylko jednego schematu, odpowiedni obsługi jest uruchamiany.

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

W powyższym kodzie tylko obsługi ze schematem "Bearer" działa. Wszystkie tożsamości oparte na pliku cookie są ignorowane.

## <a name="selecting-the-scheme-with-policies"></a>Wybieranie schematu przy użyciu zasad

Jeśli wolisz określić żądany schemat w [zasad](xref:security/authorization/policies), można ustawić `AuthenticationSchemes` kolekcji podczas dodawania zasad:

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

W powyższym przykładzie zasad "Over18" jest uruchamiana tylko dla tożsamości utworzony przez program obsługi "Bearer". Użyj zasad przez ustawienie `[Authorize]` atrybutu `Policy` właściwości:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
