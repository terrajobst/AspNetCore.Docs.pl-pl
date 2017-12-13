---
title: "Autoryzowanie z określonym schematem - platformy ASP.NET Core"
author: rick-anderson
description: "W tym artykule wyjaśniono, jak ograniczyć tożsamości do określonego schematu podczas pracy z wielu metod uwierzytelniania."
keywords: "Platformy ASP.NET Core tożsamości, schematem uwierzytelniania"
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 8c9d068b88263d0c06b11a6b87416fb02885c475
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="authorize-with-a-specific-scheme"></a><span data-ttu-id="f5d50-104">Autoryzowanie z określonego systemu</span><span class="sxs-lookup"><span data-stu-id="f5d50-104">Authorize with a specific scheme</span></span>

<span data-ttu-id="f5d50-105">W niektórych scenariuszach, takich jak aplikacje jednostronicowe (źródła) jest często wiele metod uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f5d50-105">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="f5d50-106">Na przykład aplikacji mogą używać uwierzytelniania opartego na pliku cookie do logowania i uwierzytelniania elementu nośnego JWT dla żądań JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f5d50-106">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="f5d50-107">W niektórych przypadkach aplikacja może mieć wiele wystąpień program obsługi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f5d50-107">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="f5d50-108">Na przykład dwa obsługi pliku cookie, których jedna zawiera podstawowej tożsamości, a drugi jest tworzony podczas zostało wyzwolone uwierzytelnianie wieloskładnikowe (MFA).</span><span class="sxs-lookup"><span data-stu-id="f5d50-108">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="f5d50-109">Mogą być wyzwalane MFA, ponieważ użytkownik zażądał operacji, która wymaga dodatkowych zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="f5d50-109">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f5d50-110">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f5d50-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f5d50-111">Schemat uwierzytelniania nosi nazwę po skonfigurowaniu usługi uwierzytelniania podczas uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f5d50-111">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="f5d50-112">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f5d50-112">For example:</span></span>

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

<span data-ttu-id="f5d50-113">W powyższym kodzie dwóch metod obsługi uwierzytelniania zostały dodane: jeden dla plików cookie i jeden dla elementu nośnego.</span><span class="sxs-lookup"><span data-stu-id="f5d50-113">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="f5d50-114">Określanie domyślnego schematu powoduje `HttpContext.User` ustawiona do tej tożsamości.</span><span class="sxs-lookup"><span data-stu-id="f5d50-114">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="f5d50-115">W razie potrzeby nie jest to zachowanie można ją wyłączyć, wywołując bez parametrów formę `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="f5d50-115">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f5d50-116">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f5d50-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f5d50-117">Schematy uwierzytelniania są nazywane po skonfigurowaniu uwierzytelniania middlewares podczas uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f5d50-117">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="f5d50-118">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f5d50-118">For example:</span></span>

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

<span data-ttu-id="f5d50-119">W powyższym kodzie dodano dwa middlewares uwierzytelniania: jeden dla plików cookie i jeden dla elementu nośnego.</span><span class="sxs-lookup"><span data-stu-id="f5d50-119">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="f5d50-120">Określanie domyślnego schematu powoduje `HttpContext.User` ustawiona do tej tożsamości.</span><span class="sxs-lookup"><span data-stu-id="f5d50-120">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="f5d50-121">W razie potrzeby nie jest to zachowanie można ją wyłączyć, ustawiając `AuthenticationOptions.AutomaticAuthenticate` właściwości `false`.</span><span class="sxs-lookup"><span data-stu-id="f5d50-121">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="f5d50-122">Wybieranie schemat w atrybucie autoryzacji</span><span class="sxs-lookup"><span data-stu-id="f5d50-122">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="f5d50-123">W punkcie autoryzacji aplikacja wskazuje obsługi, który ma być używane.</span><span class="sxs-lookup"><span data-stu-id="f5d50-123">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="f5d50-124">Wybierz program obsługi, z którym autoryzacji przez przekazanie rozdzielana przecinkami lista schematy uwierzytelniania do aplikacji `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="f5d50-124">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="f5d50-125">`[Authorize]` Atrybut Określa schemat uwierzytelniania lub systemów do użycia niezależnie od tego, czy domyślny został skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="f5d50-125">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="f5d50-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f5d50-126">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f5d50-127">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f5d50-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f5d50-128">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f5d50-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="f5d50-129">W powyższym przykładzie zarówno pliku cookie i elementu nośnego Uruchom i mieć możliwość Utwórz i Dołącz tożsamości dla bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f5d50-129">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="f5d50-130">Umożliwia określenie tylko jednego schematu, odpowiedni obsługi jest uruchamiany.</span><span class="sxs-lookup"><span data-stu-id="f5d50-130">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f5d50-131">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f5d50-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f5d50-132">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f5d50-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="f5d50-133">W powyższym kodzie tylko obsługi ze schematem "Bearer" działa.</span><span class="sxs-lookup"><span data-stu-id="f5d50-133">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="f5d50-134">Wszystkie tożsamości oparte na pliku cookie są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="f5d50-134">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="f5d50-135">Wybieranie schematu przy użyciu zasad</span><span class="sxs-lookup"><span data-stu-id="f5d50-135">Selecting the scheme with policies</span></span>

<span data-ttu-id="f5d50-136">Jeśli wolisz określić żądany schemat w [zasad](xref:security/authorization/policies), można ustawić `AuthenticationSchemes` kolekcji podczas dodawania zasad:</span><span class="sxs-lookup"><span data-stu-id="f5d50-136">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="f5d50-137">W powyższym przykładzie zasad "Over18" jest uruchamiana tylko dla tożsamości utworzony przez program obsługi "Bearer".</span><span class="sxs-lookup"><span data-stu-id="f5d50-137">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="f5d50-138">Użyj zasad przez ustawienie `[Authorize]` atrybutu `Policy` właściwości:</span><span class="sxs-lookup"><span data-stu-id="f5d50-138">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
