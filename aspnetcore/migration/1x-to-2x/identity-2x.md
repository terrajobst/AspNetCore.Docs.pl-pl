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
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="663cc-104">Migrowanie uwierzytelnianie i tożsamość platformy ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="663cc-104">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="663cc-105">Przez [Scott Addie](https://github.com/scottaddie) i [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="663cc-105">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="663cc-106">Platformy ASP.NET Core 2.0 ma nowy model do uwierzytelniania i [tożsamości](xref:security/authentication/identity) który upraszcza konfigurację za pomocą usług.</span><span class="sxs-lookup"><span data-stu-id="663cc-106">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="663cc-107">Może zostać zaktualizowana platformy ASP.NET Core 1.x aplikacji, które używają uwierzytelniania lub tożsamości do użycia w nowym modelu w sposób opisany poniżej.</span><span class="sxs-lookup"><span data-stu-id="663cc-107">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="663cc-108">Oprogramowanie pośredniczące uwierzytelniania i usługi</span><span class="sxs-lookup"><span data-stu-id="663cc-108">Authentication Middleware and Services</span></span>
<span data-ttu-id="663cc-109">W projektach 1.x skonfigurowano uwierzytelnianie za pomocą oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="663cc-109">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="663cc-110">Metoda oprogramowanie pośredniczące jest wywoływane dla każdego schematu uwierzytelniania, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="663cc-110">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="663cc-111">W poniższym przykładzie 1.x konfiguruje uwierzytelniania serwisu Facebook z tożsamością w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-111">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="663cc-112">W projektach 2.0 skonfigurowano uwierzytelnianie za pośrednictwem usługi.</span><span class="sxs-lookup"><span data-stu-id="663cc-112">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="663cc-113">Zarejestrowanie każdego schematu uwierzytelniania `ConfigureServices` metody *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="663cc-113">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="663cc-114">`UseIdentity` Metody jest zastępowany `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="663cc-114">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="663cc-115">Poniższy przykład 2.0 umożliwia skonfigurowanie uwierzytelniania serwisu Facebook tożsamość w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-115">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="663cc-116">`UseAuthentication` Metoda dodaje składnik oprogramowania pośredniczącego uwierzytelniania pojedynczego jest odpowiedzialny za automatyczne uwierzytelnianie i obsługę żądań uwierzytelniania zdalnego.</span><span class="sxs-lookup"><span data-stu-id="663cc-116">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="663cc-117">Go zastępuje wszystkie składniki oprogramowania pośredniczącego poszczególnych składników oprogramowania pośredniczącego jednej, wspólnej.</span><span class="sxs-lookup"><span data-stu-id="663cc-117">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="663cc-118">Poniżej przedstawiono 2.0 instrukcje migracji dla każdego schematu uwierzytelniania głównych.</span><span class="sxs-lookup"><span data-stu-id="663cc-118">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="663cc-119">Uwierzytelnianie na podstawie plików cookie</span><span class="sxs-lookup"><span data-stu-id="663cc-119">Cookie-based Authentication</span></span>
<span data-ttu-id="663cc-120">Wybierz jedną z dwóch poniższych opcji, a następnie wprowadź niezbędne zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-120">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="663cc-121">Używanie plików cookie z tożsamością</span><span class="sxs-lookup"><span data-stu-id="663cc-121">Use cookies with Identity</span></span>
    - <span data-ttu-id="663cc-122">Zastąp `UseIdentity` z `UseAuthentication` w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="663cc-122">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="663cc-123">Wywołanie `AddIdentity` metoda `ConfigureServices` metody w celu dodania usługi uwierzytelniania pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="663cc-123">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="663cc-124">Opcjonalnie można wywołać `ConfigureApplicationCookie` lub `ConfigureExternalCookie` metoda `ConfigureServices` metodę, aby dostosować ustawienia plików cookie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="663cc-124">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="663cc-125">Używanie plików cookie bez tożsamości</span><span class="sxs-lookup"><span data-stu-id="663cc-125">Use cookies without Identity</span></span>
    - <span data-ttu-id="663cc-126">Zastąp `UseCookieAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="663cc-126">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="663cc-127">Wywołanie `AddAuthentication` i `AddCookie` metod w `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="663cc-127">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="663cc-128">Uwierzytelniania elementu nośnego JWT</span><span class="sxs-lookup"><span data-stu-id="663cc-128">JWT Bearer Authentication</span></span>
<span data-ttu-id="663cc-129">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-129">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="663cc-130">Zastąp `UseJwtBearerAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="663cc-130">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="663cc-131">Wywołanie `AddJwtBearer` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="663cc-131">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="663cc-132">Następujący fragment kodu nie są używane tożsamości, więc domyślny schemat powinien być ustawiony przez przekazanie `JwtBearerDefaults.AuthenticationScheme` do `AddAuthentication` metody.</span><span class="sxs-lookup"><span data-stu-id="663cc-132">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="663cc-133">OpenID Connect uwierzytelniania (OIDC)</span><span class="sxs-lookup"><span data-stu-id="663cc-133">OpenID Connect (OIDC) Authentication</span></span>
<span data-ttu-id="663cc-134">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-134">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="663cc-135">Zastąp `UseOpenIdConnectAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="663cc-135">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="663cc-136">Wywołanie `AddOpenIdConnect` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="663cc-136">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

### <a name="facebook-authentication"></a><span data-ttu-id="663cc-137">Uwierzytelniania serwisu Facebook</span><span class="sxs-lookup"><span data-stu-id="663cc-137">Facebook Authentication</span></span>
<span data-ttu-id="663cc-138">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="663cc-139">Zastąp `UseFacebookAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="663cc-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="663cc-140">Wywołanie `AddFacebook` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="663cc-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="663cc-141">Uwierzytelniania serwisu Google</span><span class="sxs-lookup"><span data-stu-id="663cc-141">Google Authentication</span></span>
<span data-ttu-id="663cc-142">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="663cc-143">Zastąp `UseGoogleAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="663cc-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="663cc-144">Wywołanie `AddGoogle` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="663cc-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="663cc-145">Uwierzytelnianie konta Microsoft</span><span class="sxs-lookup"><span data-stu-id="663cc-145">Microsoft Account Authentication</span></span>
<span data-ttu-id="663cc-146">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="663cc-147">Zastąp `UseMicrosoftAccountAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="663cc-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="663cc-148">Wywołanie `AddMicrosoftAccount` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="663cc-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="663cc-149">Uwierzytelnianie w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="663cc-149">Twitter Authentication</span></span>
<span data-ttu-id="663cc-150">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="663cc-151">Zastąp `UseTwitterAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="663cc-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="663cc-152">Wywołanie `AddTwitter` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="663cc-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="663cc-153">Ustawienie domyślne schematy uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="663cc-153">Setting Default Authentication Schemes</span></span>
<span data-ttu-id="663cc-154">W 1.x `AutomaticAuthenticate` i `AutomaticChallenge` właściwości [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) miały klasy podstawowej można ustawić na jeden schemat uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="663cc-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="663cc-155">Nie można było dobrym sposobem wyegzekwować to.</span><span class="sxs-lookup"><span data-stu-id="663cc-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="663cc-156">W 2.0, te dwie właściwości zostały usunięte jako właściwości na poszczególnych `AuthenticationOptions` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="663cc-156">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="663cc-157">Można je skonfigurować w `AddAuthentication` wywołania metody w ramach `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-157">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="663cc-158">W poprzednim fragmencie kodu, domyślny schemat ma ustawioną wartość `CookieAuthenticationDefaults.AuthenticationScheme` ("plików cookie").</span><span class="sxs-lookup"><span data-stu-id="663cc-158">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="663cc-159">Można również użyć zastąpionej wersji `AddAuthentication` metodę, aby ustawić więcej niż jedną właściwość.</span><span class="sxs-lookup"><span data-stu-id="663cc-159">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="663cc-160">W poniższym przykładzie przeciążonej metody domyślny schemat ma ustawioną wartość `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="663cc-160">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="663cc-161">Alternatywnie można określić schemat uwierzytelniania w ramach poszczególnych sieci `[Authorize]` atrybuty lub zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="663cc-161">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="663cc-162">Zdefiniuj domyślny schemat w 2.0, jeśli jest spełniony jeden z następujących warunków:</span><span class="sxs-lookup"><span data-stu-id="663cc-162">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="663cc-163">Użytkownik, który ma być automatycznie zalogowany</span><span class="sxs-lookup"><span data-stu-id="663cc-163">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="663cc-164">Możesz użyć `[Authorize]` atrybutu lub autoryzacji zasady bez określenia schematów</span><span class="sxs-lookup"><span data-stu-id="663cc-164">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="663cc-165">Wyjątkiem od tej reguły jest `AddIdentity` metody.</span><span class="sxs-lookup"><span data-stu-id="663cc-165">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="663cc-166">Ta metoda dodaje pliki cookie i ustawia domyślne uwierzytelniania i żądanie schematy do pliku cookie aplikacji `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="663cc-166">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="663cc-167">Ponadto ustawia domyślny schemat logowania zewnętrznego pliku cookie `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="663cc-167">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="663cc-168">Element HttpContext uwierzytelniania rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="663cc-168">Use HttpContext Authentication Extensions</span></span>
<span data-ttu-id="663cc-169">`IAuthenticationManager` Interfejs jest główny punkt wejścia do systemu 1.x uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="663cc-169">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="663cc-170">Go zostało zastąpione przez nowy zestaw `HttpContext` metody rozszerzenia w `Microsoft.AspNetCore.Authentication` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="663cc-170">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="663cc-171">Na przykład 1.x projektów odwołanie `Authentication` właściwości:</span><span class="sxs-lookup"><span data-stu-id="663cc-171">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="663cc-172">W projektach 2.0, należy zaimportować `Microsoft.AspNetCore.Authentication` przestrzeni nazw i Usuń `Authentication` odwołań do właściwości:</span><span class="sxs-lookup"><span data-stu-id="663cc-172">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="663cc-173">Uwierzytelnianie systemu Windows (plik HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="663cc-173">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="663cc-174">Istnieją dwie odmiany uwierzytelniania systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="663cc-174">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="663cc-175">Host zezwala tylko uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="663cc-175">The host only allows authenticated users</span></span>
2. <span data-ttu-id="663cc-176">Host umożliwia zarówno anonimowego i użytkownicy uwierzytelnieni</span><span class="sxs-lookup"><span data-stu-id="663cc-176">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="663cc-177">Pierwsza opisane powyżej nie mają wpływu zmiany 2.0.</span><span class="sxs-lookup"><span data-stu-id="663cc-177">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="663cc-178">Druga opisany powyżej jest wpływ zmiany 2.0.</span><span class="sxs-lookup"><span data-stu-id="663cc-178">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="663cc-179">Na przykład może być stosowanie anonimowych użytkowników do aplikacji w usługach IIS lub [HTTP.sys](xref:fundamentals/servers/weblistener) warstwy ale autoryzowanie użytkowników na poziomie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="663cc-179">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="663cc-180">W tym scenariuszu Ustaw domyślny schemat `IISDefaults.AuthenticationScheme` w `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-180">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="663cc-181">Nie można ustawić domyślnego schematu zapobiega odpowiednio żądania autoryzacji zażąda działać.</span><span class="sxs-lookup"><span data-stu-id="663cc-181">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="663cc-182">IdentityCookieOptions wystąpień</span><span class="sxs-lookup"><span data-stu-id="663cc-182">IdentityCookieOptions Instances</span></span>
<span data-ttu-id="663cc-183">Efektem ubocznym zmiany 2.0 jest przełącznika tak, aby przy użyciu o nazwie Opcje zamiast wystąpień opcje pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="663cc-183">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="663cc-184">Możliwość dostosowania nazwy schematu pliku cookie tożsamości jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="663cc-184">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="663cc-185">Na przykład 1.x projektów użyj [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) do przekazania `IdentityCookieOptions` parametru do *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="663cc-185">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="663cc-186">Schemat uwierzytelniania zewnętrzny plik cookie jest dostępny z udostępnionego wystąpienia:</span><span class="sxs-lookup"><span data-stu-id="663cc-186">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="663cc-187">Iniekcji wyżej wymienione konstruktora staje się niepotrzebne w projektach 2.0 i `_externalCookieScheme` można usunąć pola:</span><span class="sxs-lookup"><span data-stu-id="663cc-187">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="663cc-188">`IdentityConstants.ExternalScheme` Stała mogą być używane bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="663cc-188">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="663cc-189">Dodaj właściwości nawigacji POCO IdentityUser</span><span class="sxs-lookup"><span data-stu-id="663cc-189">Add IdentityUser POCO Navigation Properties</span></span>
<span data-ttu-id="663cc-190">Właściwości nawigacji Core Entity Framework (EF) podstawowego `IdentityUser` POCO (zwykły stary obiekt CLR) zostały usunięte.</span><span class="sxs-lookup"><span data-stu-id="663cc-190">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="663cc-191">Jeśli projekt 1.x używane te właściwości, ręcznie dodać je do projektu 2.0:</span><span class="sxs-lookup"><span data-stu-id="663cc-191">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="663cc-192">Aby uniknąć zduplikowanych kluczy obcych podczas uruchamiania migracji Core EF, Dodaj następujący kod do Twojej `IdentityDbContext` klasy `OnModelCreating` — metoda (po `base.OnModelCreating();` wywołać):</span><span class="sxs-lookup"><span data-stu-id="663cc-192">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="663cc-193">Zastąp GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="663cc-193">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="663cc-194">Metoda synchroniczna `GetExternalAuthenticationSchemes` usunięto na rzecz asynchroniczną wersję.</span><span class="sxs-lookup"><span data-stu-id="663cc-194">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="663cc-195">projekty 1.x mają następujący kod *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="663cc-195">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="663cc-196">Ta metoda jest wyświetlana w *Login.cshtml* za:</span><span class="sxs-lookup"><span data-stu-id="663cc-196">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="663cc-197">W projektach 2.0, należy użyć `GetExternalAuthenticationSchemesAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="663cc-197">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="663cc-198">W *Login.cshtml*, `AuthenticationScheme` dostęp do właściwości `foreach` pętli zmienia się na `Name`:</span><span class="sxs-lookup"><span data-stu-id="663cc-198">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="663cc-199">Zmiana właściwości ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="663cc-199">ManageLoginsViewModel Property Change</span></span>
<span data-ttu-id="663cc-200">A `ManageLoginsViewModel` obiekt jest używany w `ManageLogins` akcji *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="663cc-200">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="663cc-201">W 1.x projektów, obiekt w `OtherLogins` właściwości zwracany typ jest `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="663cc-201">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="663cc-202">Ten typ zwracany wymaga importowania `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="663cc-202">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="663cc-203">W projektach 2.0, zwracany typ zmiany `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="663cc-203">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="663cc-204">Ten nowy typ zwracany wymaga zastępowanie `Microsoft.AspNetCore.Http.Authentication` importowania `Microsoft.AspNetCore.Authentication` zaimportować.</span><span class="sxs-lookup"><span data-stu-id="663cc-204">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="663cc-205">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="663cc-205">Additional Resources</span></span>
<span data-ttu-id="663cc-206">Dodatkowe szczegóły oraz omówienie, zobacz [dyskusji 2.0 uwierzytelniania](https://github.com/aspnet/Security/issues/1338) problem w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="663cc-206">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
