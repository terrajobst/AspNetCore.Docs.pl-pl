---
title: Migrowanie uwierzytelnianie i tożsamość do ASP.NET Core 2.0
author: scottaddie
description: W tym artykule omówiono najbardziej typowe kroki dla migracji platformy ASP.NET Core 1.x uwierzytelnianie i tożsamość ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 12/18/2018
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: d11d41c82236436096660a24df81a3df4da0fb8e
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265356"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="66321-103">Migrowanie uwierzytelnianie i tożsamość do ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="66321-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="66321-104">Przez [Scott Addie](https://github.com/scottaddie) i [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="66321-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="66321-105">Platforma ASP.NET Core w wersji 2.0 ma nowy model uwierzytelniania i [tożsamości](xref:security/authentication/identity) który upraszcza konfigurację za pomocą usług.</span><span class="sxs-lookup"><span data-stu-id="66321-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="66321-106">Aplikacje platformy ASP.NET Core 1.x, korzystających z uwierzytelniania lub tożsamości można aktualizować do korzystania z nowego modelu, co zostało opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="66321-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="66321-107">Oprogramowanie pośredniczące uwierzytelniania i usługi</span><span class="sxs-lookup"><span data-stu-id="66321-107">Authentication Middleware and services</span></span>

<span data-ttu-id="66321-108">W projektach 1.x skonfigurowano uwierzytelnianie za pomocą oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="66321-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="66321-109">Metoda oprogramowanie pośredniczące jest wywoływane dla każdego schematu uwierzytelniania, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="66321-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="66321-110">W poniższym przykładzie 1.x konfiguruje uwierzytelnianie serwisu Facebook przy użyciu tożsamości w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66321-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="66321-111">W projektach 2.0 skonfigurowano uwierzytelnianie za pośrednictwem usług.</span><span class="sxs-lookup"><span data-stu-id="66321-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="66321-112">Zarejestrowanie każdego schematu uwierzytelniania `ConfigureServices` metody *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="66321-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="66321-113">`UseIdentity` Metoda zostaje zastąpiona opcją `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="66321-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="66321-114">Poniższy przykład 2.0 umożliwia skonfigurowanie uwierzytelniania serwisu Facebook przy użyciu tożsamości w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66321-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="66321-115">`UseAuthentication` Metoda dodaje składnik oprogramowania pośredniczącego uwierzytelniania pojedynczego jest odpowiedzialny za automatyczne uwierzytelnianie i obsługę żądań uwierzytelniania zdalnego.</span><span class="sxs-lookup"><span data-stu-id="66321-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="66321-116">Zastępuje wszystkie składniki oprogramowania pośredniczącego poszczególnych składników oprogramowania pośredniczącego jednej, wspólnej.</span><span class="sxs-lookup"><span data-stu-id="66321-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="66321-117">Poniżej przedstawiono 2.0 instrukcje migracji dla każdego schematu uwierzytelniania głównych.</span><span class="sxs-lookup"><span data-stu-id="66321-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="66321-118">Na podstawie plików cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="66321-118">Cookie-based authentication</span></span>

<span data-ttu-id="66321-119">Wybierz jedną z dwóch poniższych opcji i wprowadź niezbędne zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66321-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="66321-120">Używanie plików cookie z tożsamością</span><span class="sxs-lookup"><span data-stu-id="66321-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="66321-121">Zastąp `UseIdentity` z `UseAuthentication` w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="66321-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="66321-122">Wywoływanie `AddIdentity` method in Class metoda `ConfigureServices` metody w celu dodania usługi uwierzytelniania pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="66321-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="66321-123">Opcjonalnie można wywołać `ConfigureApplicationCookie` lub `ConfigureExternalCookie` method in Class metoda `ConfigureServices` metodę, aby dostosować ustawienia plików cookie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="66321-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="66321-124">Używanie plików cookie bez użycia produktu Identity</span><span class="sxs-lookup"><span data-stu-id="66321-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="66321-125">Zastąp `UseCookieAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="66321-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="66321-126">Wywoływanie `AddAuthentication` i `AddCookie` metody `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="66321-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="66321-127">Uwierzytelniania elementu nośnego JWT</span><span class="sxs-lookup"><span data-stu-id="66321-127">JWT Bearer Authentication</span></span>

<span data-ttu-id="66321-128">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66321-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="66321-129">Zastąp `UseJwtBearerAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="66321-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="66321-130">Wywoływanie `AddJwtBearer` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="66321-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="66321-131">Ten fragment kodu nie używa tożsamości, więc należy ustawić domyślny schemat, przekazując `JwtBearerDefaults.AuthenticationScheme` do `AddAuthentication` metody.</span><span class="sxs-lookup"><span data-stu-id="66321-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="66321-132">Uwierzytelnianie OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="66321-132">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="66321-133">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66321-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="66321-134">Zastąp `UseOpenIdConnectAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="66321-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="66321-135">Wywoływanie `AddOpenIdConnect` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="66321-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

### <a name="facebook-authentication"></a><span data-ttu-id="66321-136">Uwierzytelnianie przy użyciu usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="66321-136">Facebook authentication</span></span>

<span data-ttu-id="66321-137">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66321-137">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="66321-138">Zastąp `UseFacebookAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="66321-138">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="66321-139">Wywoływanie `AddFacebook` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="66321-139">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="66321-140">Uwierzytelnianie przy użyciu usługi Google</span><span class="sxs-lookup"><span data-stu-id="66321-140">Google authentication</span></span>

<span data-ttu-id="66321-141">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66321-141">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="66321-142">Zastąp `UseGoogleAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="66321-142">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="66321-143">Wywoływanie `AddGoogle` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="66321-143">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="66321-144">Uwierzytelnianie za pomocą Account firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="66321-144">Microsoft Account authentication</span></span>

<span data-ttu-id="66321-145">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66321-145">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="66321-146">Zastąp `UseMicrosoftAccountAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="66321-146">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="66321-147">Wywoływanie `AddMicrosoftAccount` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="66321-147">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="66321-148">Uwierzytelnianie przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="66321-148">Twitter authentication</span></span>

<span data-ttu-id="66321-149">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66321-149">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="66321-150">Zastąp `UseTwitterAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="66321-150">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="66321-151">Wywoływanie `AddTwitter` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="66321-151">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="66321-152">Ustawienie domyślne schematy uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="66321-152">Setting default authentication schemes</span></span>

<span data-ttu-id="66321-153">W 1.x `AutomaticAuthenticate` i `AutomaticChallenge` właściwości [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) miały klasy bazowej należy ustawić na jeden schemat uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="66321-153">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="66321-154">Nie było dobrym sposobem wymuszania tej reguły.</span><span class="sxs-lookup"><span data-stu-id="66321-154">There was no good way to enforce this.</span></span>

<span data-ttu-id="66321-155">W wersji 2.0, te dwie właściwości zostały usunięte jako właściwości na poszczególnych `AuthenticationOptions` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="66321-155">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="66321-156">Można je skonfigurować w `AddAuthentication` wywołania metody w ramach `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66321-156">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="66321-157">W poprzednim fragmencie kodu ustawiono domyślny schemat `CookieAuthenticationDefaults.AuthenticationScheme` ("plików cookie" ").</span><span class="sxs-lookup"><span data-stu-id="66321-157">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="66321-158">Można również użyć przeciążoną wersję `AddAuthentication` metodę, aby ustawić więcej niż jednej właściwości.</span><span class="sxs-lookup"><span data-stu-id="66321-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="66321-159">W poniższym przykładzie użycie metody przeciążonej ustawiono domyślny schemat `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="66321-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="66321-160">Alternatywnie można określić schemat uwierzytelniania w ramach swojego osobistego `[Authorize]` atrybutów lub zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="66321-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="66321-161">Zdefiniuj domyślny schemat w wersji 2.0, jeśli jest spełniony jeden z następujących warunków:</span><span class="sxs-lookup"><span data-stu-id="66321-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="66321-162">Użytkownik, który ma być automatycznie zalogowany</span><span class="sxs-lookup"><span data-stu-id="66321-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="66321-163">Możesz użyć `[Authorize]` atrybutu lub autoryzacji zasad bez określania schematów</span><span class="sxs-lookup"><span data-stu-id="66321-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="66321-164">Wyjątkiem od tej reguły jest `AddIdentity` metody.</span><span class="sxs-lookup"><span data-stu-id="66321-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="66321-165">Ta metoda dodaje pliki cookie i ustawia domyślne uwierzytelniania i żądanie schematy do pliku cookie aplikacji `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="66321-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="66321-166">Ponadto ustawia domyślny schemat Zaloguj się do zewnętrznego pliku cookie `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="66321-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="66321-167">Korzystanie z rozszerzeń uwierzytelniania HttpContext</span><span class="sxs-lookup"><span data-stu-id="66321-167">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="66321-168">`IAuthenticationManager` Interfejs jest główny punkt wejścia do 1.x systemu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="66321-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="66321-169">Jest zastępowany przy użyciu nowego zestawu `HttpContext` metody rozszerzające w `Microsoft.AspNetCore.Authentication` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="66321-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="66321-170">Na przykład 1.x projektów odwołanie `Authentication` właściwości:</span><span class="sxs-lookup"><span data-stu-id="66321-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="66321-171">W projektach 2.0, należy zaimportować `Microsoft.AspNetCore.Authentication` przestrzeni nazw i usuwanie `Authentication` odwołania do właściwości:</span><span class="sxs-lookup"><span data-stu-id="66321-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="66321-172">Uwierzytelnianie Windows (plik HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="66321-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="66321-173">Istnieją dwie odmiany uwierzytelniania Windows:</span><span class="sxs-lookup"><span data-stu-id="66321-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="66321-174">Host zezwala tylko uwierzytelnieni użytkownicy</span><span class="sxs-lookup"><span data-stu-id="66321-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="66321-175">Host umożliwia zarówno anonimowe i uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="66321-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="66321-176">Pierwsza wersja opisanych powyżej jest niezależny od zmiany 2.0.</span><span class="sxs-lookup"><span data-stu-id="66321-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="66321-177">Druga opisanych powyżej jest wpływ zmiany 2.0.</span><span class="sxs-lookup"><span data-stu-id="66321-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="66321-178">Na przykład może być co anonimowych użytkowników w swojej aplikacji w usługach IIS lub [HTTP.sys](xref:fundamentals/servers/httpsys) warstwy, ale autoryzowanie użytkowników na poziomie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="66321-178">As an example, you may be allowing anonymous users into your app at the IIS or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="66321-179">W tym scenariuszu ustawiono domyślny schemat `IISDefaults.AuthenticationScheme` w `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="66321-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="66321-180">Można ustawić domyślny schemat zapobiega odpowiednio żądania autoryzacji jako wezwania od pracy.</span><span class="sxs-lookup"><span data-stu-id="66321-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="66321-181">IdentityCookieOptions instances</span><span class="sxs-lookup"><span data-stu-id="66321-181">IdentityCookieOptions instances</span></span>

<span data-ttu-id="66321-182">Efektem zmian 2.0 jest po przełączeniu na użycie za pomocą o nazwie Opcje zamiast wystąpień opcje pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="66321-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="66321-183">Możliwość dostosowania nazw systemu plików cookie tożsamości jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="66321-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="66321-184">Na przykład 1.x projektów użyj [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) do przekazania `IdentityCookieOptions` parametru do *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="66321-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="66321-185">Schemat uwierzytelniania zewnętrznego pliku cookie jest dostępny z udostępnionego wystąpienia:</span><span class="sxs-lookup"><span data-stu-id="66321-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="66321-186">Iniekcji wyżej konstruktora staje się niepotrzebne w projektach 2.0 i `_externalCookieScheme` można usunąć pola:</span><span class="sxs-lookup"><span data-stu-id="66321-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="66321-187">`IdentityConstants.ExternalScheme` — Stała, które mogą być używane bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="66321-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="66321-188">Dodawanie obiektów POCO IdentityUser właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="66321-188">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="66321-189">Właściwości nawigacji Core Entity Framework (EF) podstawy `IdentityUser` POCO (zwykłe stare obiektu CLR) zostały usunięte.</span><span class="sxs-lookup"><span data-stu-id="66321-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="66321-190">Jeśli projekt 1.x tych właściwości, ręcznie dodać je do projektu 2.0:</span><span class="sxs-lookup"><span data-stu-id="66321-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="66321-191">Aby zapobiec zduplikowane klucze obce podczas uruchamiania programu EF Core migracji, należy dodać następujące polecenie, aby Twoje `IdentityDbContext` klasy `OnModelCreating` — metoda (po `base.OnModelCreating();` wywołania):</span><span class="sxs-lookup"><span data-stu-id="66321-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="66321-192">Zastąp GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="66321-192">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="66321-193">Metoda synchroniczna `GetExternalAuthenticationSchemes` został usunięty na rzecz asynchroniczna wersja.</span><span class="sxs-lookup"><span data-stu-id="66321-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="66321-194">projekty 1.x znajduje się następujący kod *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="66321-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="66321-195">Metoda ta pojawia się w *Login.cshtml* za:</span><span class="sxs-lookup"><span data-stu-id="66321-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="66321-196">W projektach 2.0, należy użyć `GetExternalAuthenticationSchemesAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="66321-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="66321-197">W *Login.cshtml*, `AuthenticationScheme` właściwość uzyskuje dostęp w `foreach` pętli zmieni się na `Name`:</span><span class="sxs-lookup"><span data-stu-id="66321-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="66321-198">Zmiana właściwości ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="66321-198">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="66321-199">A `ManageLoginsViewModel` obiekt jest używany w `ManageLogins` akcji *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="66321-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="66321-200">W 1.x projektach obiekt firmy `OtherLogins` właściwości typ zwracany jest `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="66321-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="66321-201">Ten typ zwracany wymaga importowania `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="66321-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="66321-202">W projektach 2.0, zwracany typ zmienia się na `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="66321-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="66321-203">Ten nowy typ zwracany wymaga zastąpienia `Microsoft.AspNetCore.Http.Authentication` zaimportuj za pomocą `Microsoft.AspNetCore.Authentication` importowania.</span><span class="sxs-lookup"><span data-stu-id="66321-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="66321-204">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="66321-204">Additional resources</span></span>

<span data-ttu-id="66321-205">Aby uzyskać więcej informacji i dyskusji, zobacz [dyskusję odnośnie do uwierzytelniania w wersji 2.0](https://github.com/aspnet/Security/issues/1338) problemu w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="66321-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
