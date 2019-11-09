---
title: Autoryzuj z określonym schematem w ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono, jak ograniczyć tożsamość do określonego schematu podczas pracy z wieloma metodami uwierzytelniania.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 38da80519b9d5d097c24d38b5a37503174629fc4
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896970"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="1fffb-103">Autoryzuj z określonym schematem w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1fffb-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="1fffb-104">W niektórych scenariuszach, takich jak aplikacje jednostronicowe (aplikacji jednostronicowych), często używane są wiele metod uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1fffb-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="1fffb-105">Na przykład aplikacja może używać uwierzytelniania opartego na plikach cookie do logowania się i uwierzytelniania JWT dla żądań języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1fffb-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="1fffb-106">W niektórych przypadkach aplikacja może mieć wiele wystąpień programu obsługi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1fffb-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="1fffb-107">Na przykład dwa programy obsługi plików cookie, w których jeden zawiera tożsamość podstawową, a jedna jest tworzona podczas wyzwalania uwierzytelniania wieloskładnikowego (MFA).</span><span class="sxs-lookup"><span data-stu-id="1fffb-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="1fffb-108">Uwierzytelnianie wieloskładnikowe może być wyzwalane, ponieważ użytkownik zażądał operacji wymagającej dodatkowych zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="1fffb-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

<span data-ttu-id="1fffb-109">Schemat uwierzytelniania ma nazwę, gdy usługa uwierzytelniania jest konfigurowana podczas uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1fffb-109">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="1fffb-110">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1fffb-110">For example:</span></span>

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

<span data-ttu-id="1fffb-111">W poprzednim kodzie dodano dwa programy obsługi uwierzytelniania: jeden dla plików cookie i jeden dla okaziciela.</span><span class="sxs-lookup"><span data-stu-id="1fffb-111">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="1fffb-112">Określanie schematu domyślnego powoduje, że właściwość `HttpContext.User` jest ustawiana na tę tożsamość.</span><span class="sxs-lookup"><span data-stu-id="1fffb-112">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="1fffb-113">Jeśli takie zachowanie nie jest wymagane, należy je wyłączyć, wywołując formularz bez parametrów `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="1fffb-113">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="1fffb-114">Wybieranie schematu z atrybutem Autoryzuj</span><span class="sxs-lookup"><span data-stu-id="1fffb-114">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="1fffb-115">W punkcie autoryzacji Aplikacja wskazuje program obsługi, który ma być używany.</span><span class="sxs-lookup"><span data-stu-id="1fffb-115">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="1fffb-116">Wybierz program obsługi, za pomocą którego aplikacja będzie autoryzować, przekazując rozdzieloną przecinkami listę schematów uwierzytelniania do `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="1fffb-116">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="1fffb-117">Atrybut `[Authorize]` określa schemat lub schematy uwierzytelniania, które mają być używane niezależnie od tego, czy skonfigurowano wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="1fffb-117">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="1fffb-118">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1fffb-118">For example:</span></span>

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

<span data-ttu-id="1fffb-119">W poprzednim przykładzie uruchomiono zarówno programy obsługi plików cookie, jak i okaziciela oraz możliwość tworzenia i dołączania tożsamości bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1fffb-119">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="1fffb-120">Określając tylko jeden schemat, zostanie uruchomiony odpowiedni program obsługi.</span><span class="sxs-lookup"><span data-stu-id="1fffb-120">By specifying a single scheme only, the corresponding handler runs.</span></span>

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

<span data-ttu-id="1fffb-121">W poprzednim kodzie jest tylko procedura obsługi ze schematem "Bearer".</span><span class="sxs-lookup"><span data-stu-id="1fffb-121">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="1fffb-122">Wszystkie tożsamości oparte na plikach cookie są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="1fffb-122">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="1fffb-123">Wybieranie schematu z zasadami</span><span class="sxs-lookup"><span data-stu-id="1fffb-123">Selecting the scheme with policies</span></span>

<span data-ttu-id="1fffb-124">Jeśli wolisz określić żądane schematy w [zasadach](xref:security/authorization/policies), możesz ustawić kolekcje `AuthenticationSchemes` podczas dodawania zasad:</span><span class="sxs-lookup"><span data-stu-id="1fffb-124">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="1fffb-125">W poprzednim przykładzie zasada "Over18" działa tylko w odniesieniu do tożsamości utworzonej przez procedurę obsługi "Bearer".</span><span class="sxs-lookup"><span data-stu-id="1fffb-125">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="1fffb-126">Użyj zasad, ustawiając właściwość `Policy` `[Authorize]` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="1fffb-126">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="1fffb-127">Używanie wielu schematów uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="1fffb-127">Use multiple authentication schemes</span></span>

<span data-ttu-id="1fffb-128">Niektóre aplikacje mogą wymagać obsługi wielu typów uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1fffb-128">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="1fffb-129">Na przykład aplikacja może uwierzytelniać użytkowników z Azure Active Directory i z bazy danych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1fffb-129">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="1fffb-130">Innym przykładem jest aplikacja, która uwierzytelnia użytkowników zarówno z Active Directory Federation Services, jak i Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="1fffb-130">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="1fffb-131">W takim przypadku aplikacja powinna akceptować token okaziciela JWT z kilku wystawców.</span><span class="sxs-lookup"><span data-stu-id="1fffb-131">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="1fffb-132">Dodaj wszystkie schematy uwierzytelniania, które chcesz zaakceptować.</span><span class="sxs-lookup"><span data-stu-id="1fffb-132">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="1fffb-133">Na przykład poniższy kod w `Startup.ConfigureServices` dodaje dwa systemy uwierzytelniania okaziciela JWT z różnymi wystawcami:</span><span class="sxs-lookup"><span data-stu-id="1fffb-133">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

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
> <span data-ttu-id="1fffb-134">Zarejestrowano tylko jedno uwierzytelnianie okaziciela JWT z domyślnym schematem uwierzytelniania `JwtBearerDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="1fffb-134">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="1fffb-135">Dodatkowe uwierzytelnianie musi być zarejestrowane przy użyciu unikatowego schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1fffb-135">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="1fffb-136">Następnym krokiem jest zaktualizowanie domyślnych zasad autoryzacji w celu zaakceptowania obu schematów uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1fffb-136">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="1fffb-137">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1fffb-137">For example:</span></span>

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

<span data-ttu-id="1fffb-138">Ponieważ domyślne zasady autoryzacji są zastępowane, można użyć atrybutu `[Authorize]` w kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="1fffb-138">As the default authorization policy is overridden, it's possible to use the `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="1fffb-139">Kontroler akceptuje żądania z tokenem JWT wystawionym przez pierwszego lub drugiego wystawcy.</span><span class="sxs-lookup"><span data-stu-id="1fffb-139">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
