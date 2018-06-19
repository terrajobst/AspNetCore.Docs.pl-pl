---
title: Migrowanie uwierzytelnianie i tożsamość do platformy ASP.NET Core 2.0
author: scottaddie
description: W tym artykule opisano najbardziej typowe kroki dotyczące migrowania platformy ASP.NET Core 1.x uwierzytelnianie i tożsamość platformy ASP.NET Core 2.0.
manager: wpickett
ms.author: scaddie
ms.date: 10/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 0653906996f9f37d436ebefc6a738d2603788d53
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32741462"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="7ce67-103">Migrowanie uwierzytelnianie i tożsamość do platformy ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="7ce67-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="7ce67-104">Przez [Scott Addie](https://github.com/scottaddie) i [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="7ce67-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="7ce67-105">Platformy ASP.NET Core 2.0 ma nowy model do uwierzytelniania i [tożsamości](xref:security/authentication/identity) który upraszcza konfigurację za pomocą usług.</span><span class="sxs-lookup"><span data-stu-id="7ce67-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="7ce67-106">Może zostać zaktualizowana platformy ASP.NET Core 1.x aplikacji, które używają uwierzytelniania lub tożsamości do użycia w nowym modelu w sposób opisany poniżej.</span><span class="sxs-lookup"><span data-stu-id="7ce67-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="7ce67-107">Oprogramowanie pośredniczące uwierzytelniania i usługi</span><span class="sxs-lookup"><span data-stu-id="7ce67-107">Authentication Middleware and services</span></span>
<span data-ttu-id="7ce67-108">W projektach 1.x skonfigurowano uwierzytelnianie za pomocą oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="7ce67-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="7ce67-109">Metoda oprogramowanie pośredniczące jest wywoływane dla każdego schematu uwierzytelniania, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="7ce67-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="7ce67-110">W poniższym przykładzie 1.x konfiguruje uwierzytelniania serwisu Facebook z tożsamością w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="7ce67-111">W projektach 2.0 skonfigurowano uwierzytelnianie za pośrednictwem usługi.</span><span class="sxs-lookup"><span data-stu-id="7ce67-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="7ce67-112">Zarejestrowanie każdego schematu uwierzytelniania `ConfigureServices` metody *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7ce67-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="7ce67-113">`UseIdentity` Metody jest zastępowany `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="7ce67-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="7ce67-114">Poniższy przykład 2.0 umożliwia skonfigurowanie uwierzytelniania serwisu Facebook tożsamość w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="7ce67-115">`UseAuthentication` Metoda dodaje składnik oprogramowania pośredniczącego uwierzytelniania pojedynczego jest odpowiedzialny za automatyczne uwierzytelnianie i obsługę żądań uwierzytelniania zdalnego.</span><span class="sxs-lookup"><span data-stu-id="7ce67-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="7ce67-116">Go zastępuje wszystkie składniki oprogramowania pośredniczącego poszczególnych składników oprogramowania pośredniczącego jednej, wspólnej.</span><span class="sxs-lookup"><span data-stu-id="7ce67-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="7ce67-117">Poniżej przedstawiono 2.0 instrukcje migracji dla każdego schematu uwierzytelniania głównych.</span><span class="sxs-lookup"><span data-stu-id="7ce67-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="7ce67-118">Uwierzytelnianie na podstawie plików cookie</span><span class="sxs-lookup"><span data-stu-id="7ce67-118">Cookie-based authentication</span></span>
<span data-ttu-id="7ce67-119">Wybierz jedną z dwóch poniższych opcji, a następnie wprowadź niezbędne zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="7ce67-120">Używanie plików cookie z tożsamością</span><span class="sxs-lookup"><span data-stu-id="7ce67-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="7ce67-121">Zastąp `UseIdentity` z `UseAuthentication` w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="7ce67-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="7ce67-122">Wywołanie `AddIdentity` metoda `ConfigureServices` metody w celu dodania usługi uwierzytelniania pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7ce67-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="7ce67-123">Opcjonalnie można wywołać `ConfigureApplicationCookie` lub `ConfigureExternalCookie` metoda `ConfigureServices` metodę, aby dostosować ustawienia plików cookie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="7ce67-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="7ce67-124">Używanie plików cookie bez tożsamości</span><span class="sxs-lookup"><span data-stu-id="7ce67-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="7ce67-125">Zastąp `UseCookieAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="7ce67-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="7ce67-126">Wywołanie `AddAuthentication` i `AddCookie` metod w `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="7ce67-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="7ce67-127">Uwierzytelniania elementu nośnego JWT</span><span class="sxs-lookup"><span data-stu-id="7ce67-127">JWT Bearer Authentication</span></span>
<span data-ttu-id="7ce67-128">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="7ce67-129">Zastąp `UseJwtBearerAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="7ce67-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="7ce67-130">Wywołanie `AddJwtBearer` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="7ce67-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="7ce67-131">Następujący fragment kodu nie są używane tożsamości, więc domyślny schemat powinien być ustawiony przez przekazanie `JwtBearerDefaults.AuthenticationScheme` do `AddAuthentication` metody.</span><span class="sxs-lookup"><span data-stu-id="7ce67-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="7ce67-132">Uwierzytelniania OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="7ce67-132">OpenID Connect (OIDC) authentication</span></span>
<span data-ttu-id="7ce67-133">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="7ce67-134">Zastąp `UseOpenIdConnectAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="7ce67-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="7ce67-135">Wywołanie `AddOpenIdConnect` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="7ce67-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

### <a name="facebook-authentication"></a><span data-ttu-id="7ce67-136">uwierzytelniania serwisu Facebook</span><span class="sxs-lookup"><span data-stu-id="7ce67-136">Facebook authentication</span></span>
<span data-ttu-id="7ce67-137">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-137">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="7ce67-138">Zastąp `UseFacebookAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="7ce67-138">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="7ce67-139">Wywołanie `AddFacebook` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="7ce67-139">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="7ce67-140">Uwierzytelniania serwisu Google</span><span class="sxs-lookup"><span data-stu-id="7ce67-140">Google authentication</span></span>
<span data-ttu-id="7ce67-141">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-141">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="7ce67-142">Zastąp `UseGoogleAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="7ce67-142">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="7ce67-143">Wywołanie `AddGoogle` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="7ce67-143">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="7ce67-144">Uwierzytelnianie Microsoft Account</span><span class="sxs-lookup"><span data-stu-id="7ce67-144">Microsoft Account authentication</span></span>
<span data-ttu-id="7ce67-145">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-145">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="7ce67-146">Zastąp `UseMicrosoftAccountAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="7ce67-146">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="7ce67-147">Wywołanie `AddMicrosoftAccount` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="7ce67-147">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="7ce67-148">Uwierzytelnianie w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="7ce67-148">Twitter authentication</span></span>
<span data-ttu-id="7ce67-149">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-149">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="7ce67-150">Zastąp `UseTwitterAuthentication` wywołanie metody `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="7ce67-150">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="7ce67-151">Wywołanie `AddTwitter` metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="7ce67-151">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="7ce67-152">Ustawienie domyślne schematy uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="7ce67-152">Setting default authentication schemes</span></span>
<span data-ttu-id="7ce67-153">W 1.x `AutomaticAuthenticate` i `AutomaticChallenge` właściwości [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) miały klasy podstawowej można ustawić na jeden schemat uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7ce67-153">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="7ce67-154">Nie można było dobrym sposobem wyegzekwować to.</span><span class="sxs-lookup"><span data-stu-id="7ce67-154">There was no good way to enforce this.</span></span>

<span data-ttu-id="7ce67-155">W 2.0, te dwie właściwości zostały usunięte jako właściwości na poszczególnych `AuthenticationOptions` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="7ce67-155">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="7ce67-156">Można je skonfigurować w `AddAuthentication` wywołania metody w ramach `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-156">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="7ce67-157">W poprzednim fragmencie kodu, domyślny schemat ma ustawioną wartość `CookieAuthenticationDefaults.AuthenticationScheme` ("plików cookie").</span><span class="sxs-lookup"><span data-stu-id="7ce67-157">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="7ce67-158">Można również użyć zastąpionej wersji `AddAuthentication` metodę, aby ustawić więcej niż jedną właściwość.</span><span class="sxs-lookup"><span data-stu-id="7ce67-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="7ce67-159">W poniższym przykładzie przeciążonej metody domyślny schemat ma ustawioną wartość `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="7ce67-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="7ce67-160">Alternatywnie można określić schemat uwierzytelniania w ramach poszczególnych sieci `[Authorize]` atrybuty lub zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="7ce67-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="7ce67-161">Zdefiniuj domyślny schemat w 2.0, jeśli jest spełniony jeden z następujących warunków:</span><span class="sxs-lookup"><span data-stu-id="7ce67-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="7ce67-162">Użytkownik, który ma być automatycznie zalogowany</span><span class="sxs-lookup"><span data-stu-id="7ce67-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="7ce67-163">Możesz użyć `[Authorize]` atrybutu lub autoryzacji zasady bez określenia schematów</span><span class="sxs-lookup"><span data-stu-id="7ce67-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="7ce67-164">Wyjątkiem od tej reguły jest `AddIdentity` metody.</span><span class="sxs-lookup"><span data-stu-id="7ce67-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="7ce67-165">Ta metoda dodaje pliki cookie i ustawia domyślne uwierzytelniania i żądanie schematy do pliku cookie aplikacji `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="7ce67-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="7ce67-166">Ponadto ustawia domyślny schemat logowania zewnętrznego pliku cookie `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="7ce67-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="7ce67-167">Element HttpContext uwierzytelniania rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="7ce67-167">Use HttpContext authentication extensions</span></span>
<span data-ttu-id="7ce67-168">`IAuthenticationManager` Interfejs jest główny punkt wejścia do systemu 1.x uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7ce67-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="7ce67-169">Go zostało zastąpione przez nowy zestaw `HttpContext` metody rozszerzenia w `Microsoft.AspNetCore.Authentication` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="7ce67-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="7ce67-170">Na przykład 1.x projektów odwołanie `Authentication` właściwości:</span><span class="sxs-lookup"><span data-stu-id="7ce67-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="7ce67-171">W projektach 2.0, należy zaimportować `Microsoft.AspNetCore.Authentication` przestrzeni nazw i Usuń `Authentication` odwołań do właściwości:</span><span class="sxs-lookup"><span data-stu-id="7ce67-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="7ce67-172">Uwierzytelnianie systemu Windows (plik HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="7ce67-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="7ce67-173">Istnieją dwie odmiany uwierzytelniania systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="7ce67-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="7ce67-174">Host zezwala tylko uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="7ce67-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="7ce67-175">Host umożliwia zarówno anonimowego i użytkownicy uwierzytelnieni</span><span class="sxs-lookup"><span data-stu-id="7ce67-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="7ce67-176">Pierwsza opisane powyżej nie mają wpływu zmiany 2.0.</span><span class="sxs-lookup"><span data-stu-id="7ce67-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="7ce67-177">Druga opisany powyżej jest wpływ zmiany 2.0.</span><span class="sxs-lookup"><span data-stu-id="7ce67-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="7ce67-178">Na przykład może być stosowanie anonimowych użytkowników do aplikacji w usługach IIS lub [HTTP.sys](xref:fundamentals/servers/weblistener) warstwy ale autoryzowanie użytkowników na poziomie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7ce67-178">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="7ce67-179">W tym scenariuszu Ustaw domyślny schemat `IISDefaults.AuthenticationScheme` w `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="7ce67-180">Nie można ustawić domyślnego schematu zapobiega odpowiednio żądania autoryzacji zażąda działać.</span><span class="sxs-lookup"><span data-stu-id="7ce67-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="7ce67-181">IdentityCookieOptions wystąpień</span><span class="sxs-lookup"><span data-stu-id="7ce67-181">IdentityCookieOptions instances</span></span>
<span data-ttu-id="7ce67-182">Efektem ubocznym zmiany 2.0 jest przełącznika tak, aby przy użyciu o nazwie Opcje zamiast wystąpień opcje pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7ce67-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="7ce67-183">Możliwość dostosowania nazwy schematu pliku cookie tożsamości jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="7ce67-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="7ce67-184">Na przykład 1.x projektów użyj [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) do przekazania `IdentityCookieOptions` parametru do *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="7ce67-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="7ce67-185">Schemat uwierzytelniania zewnętrzny plik cookie jest dostępny z udostępnionego wystąpienia:</span><span class="sxs-lookup"><span data-stu-id="7ce67-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="7ce67-186">Iniekcji wyżej wymienione konstruktora staje się niepotrzebne w projektach 2.0 i `_externalCookieScheme` można usunąć pola:</span><span class="sxs-lookup"><span data-stu-id="7ce67-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="7ce67-187">`IdentityConstants.ExternalScheme` Stała mogą być używane bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="7ce67-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="7ce67-188">Dodawanie obiektów POCO IdentityUser właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="7ce67-188">Add IdentityUser POCO navigation properties</span></span>
<span data-ttu-id="7ce67-189">Właściwości nawigacji Core Entity Framework (EF) podstawowego `IdentityUser` POCO (zwykły stary obiekt CLR) zostały usunięte.</span><span class="sxs-lookup"><span data-stu-id="7ce67-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="7ce67-190">Jeśli projekt 1.x używane te właściwości, ręcznie dodać je do projektu 2.0:</span><span class="sxs-lookup"><span data-stu-id="7ce67-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="7ce67-191">Aby uniknąć zduplikowanych kluczy obcych podczas uruchamiania migracji Core EF, Dodaj następujący kod do Twojej `IdentityDbContext` klasy `OnModelCreating` — metoda (po `base.OnModelCreating();` wywołać):</span><span class="sxs-lookup"><span data-stu-id="7ce67-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="7ce67-192">Zastąp GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="7ce67-192">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="7ce67-193">Metoda synchroniczna `GetExternalAuthenticationSchemes` usunięto na rzecz asynchroniczną wersję.</span><span class="sxs-lookup"><span data-stu-id="7ce67-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="7ce67-194">projekty 1.x mają następujący kod *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ce67-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="7ce67-195">Ta metoda jest wyświetlana w *Login.cshtml* za:</span><span class="sxs-lookup"><span data-stu-id="7ce67-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="7ce67-196">W projektach 2.0, należy użyć `GetExternalAuthenticationSchemesAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="7ce67-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="7ce67-197">W *Login.cshtml*, `AuthenticationScheme` dostęp do właściwości `foreach` pętli zmienia się na `Name`:</span><span class="sxs-lookup"><span data-stu-id="7ce67-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="7ce67-198">Zmiana właściwości ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="7ce67-198">ManageLoginsViewModel property change</span></span>
<span data-ttu-id="7ce67-199">A `ManageLoginsViewModel` obiekt jest używany w `ManageLogins` akcji *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="7ce67-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="7ce67-200">W 1.x projektów, obiekt w `OtherLogins` właściwości zwracany typ jest `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="7ce67-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="7ce67-201">Ten typ zwracany wymaga importowania `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="7ce67-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="7ce67-202">W projektach 2.0, zwracany typ zmiany `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="7ce67-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="7ce67-203">Ten nowy typ zwracany wymaga zastępowanie `Microsoft.AspNetCore.Http.Authentication` importowania `Microsoft.AspNetCore.Authentication` zaimportować.</span><span class="sxs-lookup"><span data-stu-id="7ce67-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="7ce67-204">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7ce67-204">Additional resources</span></span>
<span data-ttu-id="7ce67-205">Dodatkowe szczegóły oraz omówienie, zobacz [dyskusji 2.0 uwierzytelniania](https://github.com/aspnet/Security/issues/1338) problem w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="7ce67-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
