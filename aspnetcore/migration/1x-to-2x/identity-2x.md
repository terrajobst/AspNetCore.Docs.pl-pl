---
title: Migrowanie uwierzytelnianie i tożsamość do ASP.NET Core 2.0
author: scottaddie
description: W tym artykule omówiono najbardziej typowe kroki dla migracji platformy ASP.NET Core 1.x uwierzytelnianie i tożsamość ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 06/13/2019
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 3e8bc75b87a85159c9668b52eea32bb7d700be6c
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196381"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="a6caf-103">Migrowanie uwierzytelnianie i tożsamość do ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="a6caf-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="a6caf-104">Przez [Scott Addie](https://github.com/scottaddie) i [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="a6caf-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="a6caf-105">Platforma ASP.NET Core w wersji 2.0 ma nowy model uwierzytelniania i [tożsamości](xref:security/authentication/identity) który upraszcza konfigurację za pomocą usług.</span><span class="sxs-lookup"><span data-stu-id="a6caf-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) that simplifies configuration by using services.</span></span> <span data-ttu-id="a6caf-106">Aplikacje platformy ASP.NET Core 1.x, korzystających z uwierzytelniania lub tożsamości można aktualizować do korzystania z nowego modelu, co zostało opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="a6caf-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

## <a name="update-namespaces"></a><span data-ttu-id="a6caf-107">Aktualizowanie przestrzeni nazw</span><span class="sxs-lookup"><span data-stu-id="a6caf-107">Update namespaces</span></span>

<span data-ttu-id="a6caf-108">W 1.x, klasy takie `IdentityRole` i `IdentityUser` zostały znalezione w `Microsoft.AspNetCore.Identity.EntityFrameworkCore` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="a6caf-108">In 1.x, classes such `IdentityRole` and `IdentityUser` were found in the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` namespace.</span></span>

<span data-ttu-id="a6caf-109">W wersji 2.0 <xref:Microsoft.AspNetCore.Identity> przestrzeni nazw stało się nową stronę główną dla kilku z tych klas.</span><span class="sxs-lookup"><span data-stu-id="a6caf-109">In 2.0, the <xref:Microsoft.AspNetCore.Identity> namespace became the new home for several of such classes.</span></span> <span data-ttu-id="a6caf-110">Kodem tożsamości domyślne, których to dotyczy klasy mają `ApplicationUser` i `Startup`.</span><span class="sxs-lookup"><span data-stu-id="a6caf-110">With the default Identity code, affected classes include `ApplicationUser` and `Startup`.</span></span> <span data-ttu-id="a6caf-111">Dostosuj swoje `using` instrukcje do rozpoznania odwołań, których to dotyczy.</span><span class="sxs-lookup"><span data-stu-id="a6caf-111">Adjust your `using` statements to resolve the affected references.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="a6caf-112">Oprogramowanie pośredniczące uwierzytelniania i usługi</span><span class="sxs-lookup"><span data-stu-id="a6caf-112">Authentication Middleware and services</span></span>

<span data-ttu-id="a6caf-113">W projektach 1.x skonfigurowano uwierzytelnianie za pomocą oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a6caf-113">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="a6caf-114">Metoda oprogramowanie pośredniczące jest wywoływane dla każdego schematu uwierzytelniania, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="a6caf-114">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="a6caf-115">W poniższym przykładzie 1.x konfiguruje uwierzytelnianie serwisu Facebook przy użyciu tożsamości w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6caf-115">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="a6caf-116">W projektach 2.0 skonfigurowano uwierzytelnianie za pośrednictwem usług.</span><span class="sxs-lookup"><span data-stu-id="a6caf-116">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="a6caf-117">Zarejestrowanie każdego schematu uwierzytelniania `ConfigureServices` metody *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a6caf-117">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="a6caf-118">`UseIdentity` Metoda zostaje zastąpiona opcją `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="a6caf-118">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="a6caf-119">Poniższy przykład 2.0 umożliwia skonfigurowanie uwierzytelniania serwisu Facebook przy użyciu tożsamości w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6caf-119">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="a6caf-120">`UseAuthentication` Metoda dodaje składnik oprogramowania pośredniczącego uwierzytelniania jednego, który jest odpowiedzialny za automatyczne uwierzytelnianie i obsługę żądań uwierzytelniania zdalnego.</span><span class="sxs-lookup"><span data-stu-id="a6caf-120">The `UseAuthentication` method adds a single authentication middleware component, which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="a6caf-121">Zastępuje wszystkie składniki oprogramowania pośredniczącego poszczególnych składników oprogramowania pośredniczącego jednej, wspólnej.</span><span class="sxs-lookup"><span data-stu-id="a6caf-121">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="a6caf-122">Poniżej przedstawiono 2.0 instrukcje migracji dla każdego schematu uwierzytelniania głównych.</span><span class="sxs-lookup"><span data-stu-id="a6caf-122">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="a6caf-123">Na podstawie plików cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="a6caf-123">Cookie-based authentication</span></span>

<span data-ttu-id="a6caf-124">Wybierz jedną z dwóch poniższych opcji i wprowadź niezbędne zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6caf-124">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="a6caf-125">Używanie plików cookie z tożsamością</span><span class="sxs-lookup"><span data-stu-id="a6caf-125">Use cookies with Identity</span></span>
    - <span data-ttu-id="a6caf-126">Zastąp `UseIdentity` z `UseAuthentication` w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="a6caf-126">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="a6caf-127">Wywoływanie `AddIdentity` method in Class metoda `ConfigureServices` metody w celu dodania usługi uwierzytelniania pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="a6caf-127">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="a6caf-128">Opcjonalnie można wywołać `ConfigureApplicationCookie` lub `ConfigureExternalCookie` method in Class metoda `ConfigureServices` metodę, aby dostosować ustawienia plików cookie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="a6caf-128">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="a6caf-129">Używanie plików cookie bez użycia produktu Identity</span><span class="sxs-lookup"><span data-stu-id="a6caf-129">Use cookies without Identity</span></span>
    - <span data-ttu-id="a6caf-130">Zastąp `UseCookieAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a6caf-130">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="a6caf-131">Wywoływanie `AddAuthentication` i `AddCookie` metody `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="a6caf-131">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="a6caf-132">Uwierzytelniania elementu nośnego JWT</span><span class="sxs-lookup"><span data-stu-id="a6caf-132">JWT Bearer Authentication</span></span>

<span data-ttu-id="a6caf-133">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6caf-133">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="a6caf-134">Zastąp `UseJwtBearerAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a6caf-134">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="a6caf-135">Wywoływanie `AddJwtBearer` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="a6caf-135">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="a6caf-136">Ten fragment kodu nie używa tożsamości, więc należy ustawić domyślny schemat, przekazując `JwtBearerDefaults.AuthenticationScheme` do `AddAuthentication` metody.</span><span class="sxs-lookup"><span data-stu-id="a6caf-136">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="a6caf-137">Uwierzytelnianie OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="a6caf-137">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="a6caf-138">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6caf-138">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="a6caf-139">Zastąp `UseOpenIdConnectAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a6caf-139">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="a6caf-140">Wywoływanie `AddOpenIdConnect` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="a6caf-140">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

- <span data-ttu-id="a6caf-141">Zastąp `PostLogoutRedirectUri` właściwość `OpenIdConnectOptions` akcji z `SignedOutRedirectUri`:</span><span class="sxs-lookup"><span data-stu-id="a6caf-141">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="a6caf-142">Uwierzytelnianie przy użyciu usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="a6caf-142">Facebook authentication</span></span>

<span data-ttu-id="a6caf-143">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6caf-143">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="a6caf-144">Zastąp `UseFacebookAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a6caf-144">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="a6caf-145">Wywoływanie `AddFacebook` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="a6caf-145">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="a6caf-146">Uwierzytelnianie przy użyciu usługi Google</span><span class="sxs-lookup"><span data-stu-id="a6caf-146">Google authentication</span></span>

<span data-ttu-id="a6caf-147">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6caf-147">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="a6caf-148">Zastąp `UseGoogleAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a6caf-148">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="a6caf-149">Wywoływanie `AddGoogle` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="a6caf-149">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="a6caf-150">Uwierzytelnianie za pomocą Account firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="a6caf-150">Microsoft Account authentication</span></span>

<span data-ttu-id="a6caf-151">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6caf-151">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="a6caf-152">Zastąp `UseMicrosoftAccountAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a6caf-152">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="a6caf-153">Wywoływanie `AddMicrosoftAccount` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="a6caf-153">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="a6caf-154">Uwierzytelnianie przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="a6caf-154">Twitter authentication</span></span>

<span data-ttu-id="a6caf-155">Wprowadź następujące zmiany w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6caf-155">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="a6caf-156">Zastąp `UseTwitterAuthentication` metody wywołania w `Configure` metody z `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a6caf-156">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="a6caf-157">Wywoływanie `AddTwitter` method in Class metoda `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="a6caf-157">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="a6caf-158">Ustawienie domyślne schematy uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="a6caf-158">Setting default authentication schemes</span></span>

<span data-ttu-id="a6caf-159">W 1.x `AutomaticAuthenticate` i `AutomaticChallenge` właściwości [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) miały klasy bazowej należy ustawić na jeden schemat uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="a6caf-159">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="a6caf-160">Nie było dobrym sposobem wymuszania tej reguły.</span><span class="sxs-lookup"><span data-stu-id="a6caf-160">There was no good way to enforce this.</span></span>

<span data-ttu-id="a6caf-161">W wersji 2.0, te dwie właściwości zostały usunięte jako właściwości na poszczególnych `AuthenticationOptions` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a6caf-161">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="a6caf-162">Można je skonfigurować w `AddAuthentication` wywołania metody w ramach `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6caf-162">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="a6caf-163">W poprzednim fragmencie kodu ustawiono domyślny schemat `CookieAuthenticationDefaults.AuthenticationScheme` ("plików cookie" ").</span><span class="sxs-lookup"><span data-stu-id="a6caf-163">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="a6caf-164">Można również użyć przeciążoną wersję `AddAuthentication` metodę, aby ustawić więcej niż jednej właściwości.</span><span class="sxs-lookup"><span data-stu-id="a6caf-164">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="a6caf-165">W poniższym przykładzie użycie metody przeciążonej ustawiono domyślny schemat `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="a6caf-165">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="a6caf-166">Alternatywnie można określić schemat uwierzytelniania w ramach swojego osobistego `[Authorize]` atrybutów lub zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="a6caf-166">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="a6caf-167">Zdefiniuj domyślny schemat w wersji 2.0, jeśli jest spełniony jeden z następujących warunków:</span><span class="sxs-lookup"><span data-stu-id="a6caf-167">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="a6caf-168">Użytkownik, który ma być automatycznie zalogowany</span><span class="sxs-lookup"><span data-stu-id="a6caf-168">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="a6caf-169">Możesz użyć `[Authorize]` atrybutu lub autoryzacji zasad bez określania schematów</span><span class="sxs-lookup"><span data-stu-id="a6caf-169">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="a6caf-170">Wyjątkiem od tej reguły jest `AddIdentity` metody.</span><span class="sxs-lookup"><span data-stu-id="a6caf-170">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="a6caf-171">Ta metoda dodaje pliki cookie i ustawia domyślne uwierzytelniania i żądanie schematy do pliku cookie aplikacji `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="a6caf-171">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="a6caf-172">Ponadto ustawia domyślny schemat Zaloguj się do zewnętrznego pliku cookie `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="a6caf-172">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="a6caf-173">Korzystanie z rozszerzeń uwierzytelniania HttpContext</span><span class="sxs-lookup"><span data-stu-id="a6caf-173">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="a6caf-174">`IAuthenticationManager` Interfejs jest główny punkt wejścia do 1.x systemu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="a6caf-174">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="a6caf-175">Jest zastępowany przy użyciu nowego zestawu `HttpContext` metody rozszerzające w `Microsoft.AspNetCore.Authentication` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="a6caf-175">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="a6caf-176">Na przykład 1.x projektów odwołanie `Authentication` właściwości:</span><span class="sxs-lookup"><span data-stu-id="a6caf-176">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="a6caf-177">W projektach 2.0, należy zaimportować `Microsoft.AspNetCore.Authentication` przestrzeni nazw i usuwanie `Authentication` odwołania do właściwości:</span><span class="sxs-lookup"><span data-stu-id="a6caf-177">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="a6caf-178">Uwierzytelnianie Windows (plik HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="a6caf-178">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="a6caf-179">Istnieją dwie odmiany uwierzytelniania Windows:</span><span class="sxs-lookup"><span data-stu-id="a6caf-179">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="a6caf-180">Host zezwala tylko uwierzytelnieni użytkownicy</span><span class="sxs-lookup"><span data-stu-id="a6caf-180">The host only allows authenticated users</span></span>
2. <span data-ttu-id="a6caf-181">Host umożliwia zarówno anonimowe i uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="a6caf-181">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="a6caf-182">Pierwsza wersja opisanych powyżej jest niezależny od zmiany 2.0.</span><span class="sxs-lookup"><span data-stu-id="a6caf-182">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="a6caf-183">Druga opisanych powyżej jest wpływ zmiany 2.0.</span><span class="sxs-lookup"><span data-stu-id="a6caf-183">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="a6caf-184">Na przykład możesz może być co anonimowych użytkowników w swojej aplikacji w usługach IIS lub [HTTP.sys](xref:fundamentals/servers/httpsys) warstwy, ale autoryzowanie użytkowników na poziomie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a6caf-184">For example, you may be allowing anonymous users into your app at the IIS or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="a6caf-185">W tym scenariuszu ustawiono domyślny schemat `IISDefaults.AuthenticationScheme` w `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="a6caf-185">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="a6caf-186">Można ustawić domyślny schemat zapobiega żądania autoryzacji jako wezwania od pracy.</span><span class="sxs-lookup"><span data-stu-id="a6caf-186">Failure to set the default scheme prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="a6caf-187">IdentityCookieOptions instances</span><span class="sxs-lookup"><span data-stu-id="a6caf-187">IdentityCookieOptions instances</span></span>

<span data-ttu-id="a6caf-188">Efektem zmian 2.0 jest po przełączeniu na użycie za pomocą o nazwie Opcje zamiast wystąpień opcje pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="a6caf-188">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="a6caf-189">Możliwość dostosowania nazw systemu plików cookie tożsamości jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="a6caf-189">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="a6caf-190">Na przykład 1.x projektów użyj [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) do przekazania `IdentityCookieOptions` parametru do *AccountController.cs* i *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="a6caf-190">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs* and *ManageController.cs*.</span></span> <span data-ttu-id="a6caf-191">Schemat uwierzytelniania zewnętrznego pliku cookie jest dostępny z udostępnionego wystąpienia:</span><span class="sxs-lookup"><span data-stu-id="a6caf-191">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="a6caf-192">Iniekcji wyżej konstruktora staje się niepotrzebne w projektach 2.0 i `_externalCookieScheme` można usunąć pola:</span><span class="sxs-lookup"><span data-stu-id="a6caf-192">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="a6caf-193">projekty 1.x użyte `_externalCookieScheme` pola w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="a6caf-193">1.x projects used the `_externalCookieScheme` field as follows:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="a6caf-194">W projektach 2.0 zastąpić poprzedni kod poniżej.</span><span class="sxs-lookup"><span data-stu-id="a6caf-194">In 2.0 projects, replace the preceding code with the following.</span></span> <span data-ttu-id="a6caf-195">`IdentityConstants.ExternalScheme` — Stała, które mogą być używane bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="a6caf-195">The `IdentityConstants.ExternalScheme` constant can be used directly.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="a6caf-196">Rozwiązania w nowo dodanym `SignOutAsync` wywołania, importując następująca przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="a6caf-196">Resolve the newly added `SignOutAsync` call by importing the following namespace:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="a6caf-197">Dodawanie obiektów POCO IdentityUser właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="a6caf-197">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="a6caf-198">Właściwości nawigacji Core Entity Framework (EF) podstawy `IdentityUser` POCO (zwykłe stare obiektu CLR) zostały usunięte.</span><span class="sxs-lookup"><span data-stu-id="a6caf-198">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="a6caf-199">Jeśli projekt 1.x tych właściwości, ręcznie dodać je do projektu 2.0:</span><span class="sxs-lookup"><span data-stu-id="a6caf-199">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="a6caf-200">Aby zapobiec zduplikowane klucze obce podczas uruchamiania programu EF Core migracji, należy dodać następujące polecenie, aby Twoje `IdentityDbContext` klasy `OnModelCreating` — metoda (po `base.OnModelCreating();` wywołania):</span><span class="sxs-lookup"><span data-stu-id="a6caf-200">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="a6caf-201">Zastąp GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="a6caf-201">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="a6caf-202">Metoda synchroniczna `GetExternalAuthenticationSchemes` został usunięty na rzecz asynchroniczna wersja.</span><span class="sxs-lookup"><span data-stu-id="a6caf-202">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="a6caf-203">projekty 1.x znajduje się następujący kod *Controllers/ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6caf-203">1.x projects have the following code in *Controllers/ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="a6caf-204">Metoda ta pojawia się w *Views/Account/Login.cshtml* za:</span><span class="sxs-lookup"><span data-stu-id="a6caf-204">This method appears in *Views/Account/Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

<span data-ttu-id="a6caf-205">W projektach 2.0, należy użyć <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> metody.</span><span class="sxs-lookup"><span data-stu-id="a6caf-205">In 2.0 projects, use the <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> method.</span></span> <span data-ttu-id="a6caf-206">Zmiana *ManageController.cs* przypomina następujący kod:</span><span class="sxs-lookup"><span data-stu-id="a6caf-206">The change in *ManageController.cs* resembles the following code:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="a6caf-207">W *Login.cshtml*, `AuthenticationScheme` właściwość uzyskuje dostęp w `foreach` pętli zmieni się na `Name`:</span><span class="sxs-lookup"><span data-stu-id="a6caf-207">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="a6caf-208">Zmiana właściwości ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="a6caf-208">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="a6caf-209">A `ManageLoginsViewModel` obiekt jest używany w `ManageLogins` akcji *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="a6caf-209">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="a6caf-210">W 1.x projektach obiekt firmy `OtherLogins` właściwości typ zwracany jest `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="a6caf-210">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="a6caf-211">Ten typ zwracany wymaga importowania `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="a6caf-211">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="a6caf-212">W projektach 2.0, zwracany typ zmienia się na `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="a6caf-212">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="a6caf-213">Ten nowy typ zwracany wymaga zastąpienia `Microsoft.AspNetCore.Http.Authentication` zaimportuj za pomocą `Microsoft.AspNetCore.Authentication` importowania.</span><span class="sxs-lookup"><span data-stu-id="a6caf-213">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="a6caf-214">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a6caf-214">Additional resources</span></span>

<span data-ttu-id="a6caf-215">Aby uzyskać więcej informacji, zobacz [dyskusję odnośnie do uwierzytelniania w wersji 2.0](https://github.com/aspnet/Security/issues/1338) problemu w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="a6caf-215">For more information, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
