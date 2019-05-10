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
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="494a9-103">Autoryzuj przy użyciu określonego schematu w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="494a9-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="494a9-104">W niektórych scenariuszach, takich jak aplikacje jednostronicowe (źródła) jest często używa wielu metod uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="494a9-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="494a9-105">Na przykład aplikacja może używać na podstawie plików cookie uwierzytelniania do logowania i uwierzytelniania elementu nośnego tokenu JWT dla żądań JavaScript.</span><span class="sxs-lookup"><span data-stu-id="494a9-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="494a9-106">W niektórych przypadkach aplikacja może mieć wiele wystąpień do obsługi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="494a9-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="494a9-107">Na przykład dwa obsługi plików cookie, w których jedna zawiera podstawowe tożsamości, a drugi jest tworzony podczas została wyzwolona usługi Multi-Factor authentication (MFA).</span><span class="sxs-lookup"><span data-stu-id="494a9-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="494a9-108">Może zostać wyzwolone MFA, ponieważ użytkownik zażądał operacji, która wymaga zapewnienia dodatkowego bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="494a9-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="494a9-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="494a9-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="494a9-110">Schemat uwierzytelniania nosi nazwę po skonfigurowaniu usługi uwierzytelniania podczas uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="494a9-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="494a9-111">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="494a9-111">For example:</span></span>

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

<span data-ttu-id="494a9-112">W poprzednim kodzie zostały dodane dwa obsługi uwierzytelniania: jeden dla plików cookie i jeden dla elementu nośnego.</span><span class="sxs-lookup"><span data-stu-id="494a9-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="494a9-113">Określanie domyślnego schematu powoduje `HttpContext.User` ustawioną na tej tożsamości.</span><span class="sxs-lookup"><span data-stu-id="494a9-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="494a9-114">Jeśli to zachowanie nie jest konieczne, ją wyłączyć, wywołując bez parametrów formie `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="494a9-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="494a9-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="494a9-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="494a9-116">Schematy uwierzytelniania są nazywane po skonfigurowaniu uwierzytelniania middlewares podczas uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="494a9-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="494a9-117">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="494a9-117">For example:</span></span>

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

<span data-ttu-id="494a9-118">W poprzednim kodzie zostały dodane dwa middlewares uwierzytelniania: jeden dla plików cookie i jeden dla elementu nośnego.</span><span class="sxs-lookup"><span data-stu-id="494a9-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="494a9-119">Określanie domyślnego schematu powoduje `HttpContext.User` ustawioną na tej tożsamości.</span><span class="sxs-lookup"><span data-stu-id="494a9-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="494a9-120">Jeśli to zachowanie nie jest konieczne, ją wyłączyć, ustawiając `AuthenticationOptions.AutomaticAuthenticate` właściwość `false`.</span><span class="sxs-lookup"><span data-stu-id="494a9-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="494a9-121">Wybieranie schematu z atrybutem autoryzacji</span><span class="sxs-lookup"><span data-stu-id="494a9-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="494a9-122">Punkcie autoryzacji aplikacja wskazuje obsługi, który ma być używany.</span><span class="sxs-lookup"><span data-stu-id="494a9-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="494a9-123">Wybierz program obsługi, za pomocą którego aplikacja będzie autoryzować przez przekazanie rozdzielana przecinkami lista schematów uwierzytelniania na `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="494a9-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="494a9-124">`[Authorize]` Atrybut Określa schemat uwierzytelniania lub schematów niezależnie od tego, czy domyślny jest skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="494a9-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="494a9-125">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="494a9-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="494a9-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="494a9-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="494a9-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="494a9-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="494a9-128">W powyższym przykładzie obsługi plików cookie i elementu nośnego Uruchom i masz szansę, aby utworzyć i dołączyć tożsamości dla bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="494a9-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="494a9-129">Określając tylko jednego schematu, odpowiedni program obsługi jest uruchamiany.</span><span class="sxs-lookup"><span data-stu-id="494a9-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="494a9-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="494a9-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="494a9-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="494a9-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="494a9-132">W poprzednim kodzie obsługi ze schematem "Bearer" działa.</span><span class="sxs-lookup"><span data-stu-id="494a9-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="494a9-133">Wszystkie tożsamości oparte na pliku cookie są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="494a9-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="494a9-134">Wybieranie schematu przy użyciu zasad</span><span class="sxs-lookup"><span data-stu-id="494a9-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="494a9-135">Jeśli chcesz określić żądany schemat w [zasad](xref:security/authorization/policies), można ustawić `AuthenticationSchemes` kolekcji podczas dodawania zasad:</span><span class="sxs-lookup"><span data-stu-id="494a9-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="494a9-136">W powyższym przykładzie zasady "Over18" działa tylko względem utworzonej przez procedurę obsługi "Bearer" tożsamości.</span><span class="sxs-lookup"><span data-stu-id="494a9-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="494a9-137">Użyj zasad, ustawiając `[Authorize]` atrybutu `Policy` właściwości:</span><span class="sxs-lookup"><span data-stu-id="494a9-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="494a9-138">Użyj wielu schematów uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="494a9-138">Use multiple authentication schemes</span></span>

<span data-ttu-id="494a9-139">Niektóre aplikacje może być konieczne obsługuje wiele typów uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="494a9-139">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="494a9-140">Na przykład aplikacja może uwierzytelniać użytkowników z usługi Azure Active Directory i z bazy danych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="494a9-140">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="494a9-141">Innym przykładem jest aplikacja, która uwierzytelnia użytkowników z usług federacyjnych Active Directory i Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="494a9-141">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="494a9-142">W takim przypadku aplikacja powinna obsługiwać tokenu elementu nośnego JWT z kilku wystawców.</span><span class="sxs-lookup"><span data-stu-id="494a9-142">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="494a9-143">Dodaj wszystkie schematy uwierzytelniania, który chcesz zaakceptować.</span><span class="sxs-lookup"><span data-stu-id="494a9-143">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="494a9-144">Na przykład, poniższy kod w `Startup.ConfigureServices` dodaje dwa schematy uwierzytelniania elementu nośnego tokenu JWT z różnych wydawców:</span><span class="sxs-lookup"><span data-stu-id="494a9-144">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

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
> <span data-ttu-id="494a9-145">Tylko jeden uwierzytelniania elementu nośnego tokenu JWT jest zarejestrowana przy użyciu domyślnego schematu uwierzytelniania `JwtBearerDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="494a9-145">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="494a9-146">Dodatkowe uwierzytelnianie musi być zarejestrowane przy użyciu schematu; unikatowe uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="494a9-146">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="494a9-147">Następnym krokiem jest zaktualizuj domyślne zasady autoryzacji do akceptowania zarówno schematów uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="494a9-147">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="494a9-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="494a9-148">For example:</span></span>

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

<span data-ttu-id="494a9-149">Jako domyślne zasady autoryzacji jest zastępowany, jest możliwe użycie `[Authorize]` atrybutu w kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="494a9-149">As the default authorization policy is overridden, it's possible to use the `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="494a9-150">Kontroler akceptuje żądania następnie, przy użyciu tokenu JWT wystawione przez wystawcę pierwszej lub drugiej.</span><span class="sxs-lookup"><span data-stu-id="494a9-150">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
