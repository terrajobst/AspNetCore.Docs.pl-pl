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
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="e6e4e-103">Migrowanie uwierzytelniania i tożsamości do ASP.NET Core 2,0</span><span class="sxs-lookup"><span data-stu-id="e6e4e-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="e6e4e-104">Przez [Scott Addie](https://github.com/scottaddie) i [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="e6e4e-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="e6e4e-105">ASP.NET Core 2,0 ma nowy model uwierzytelniania i [tożsamości](xref:security/authentication/identity) , który upraszcza konfigurację przy użyciu usług.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) that simplifies configuration by using services.</span></span> <span data-ttu-id="e6e4e-106">ASP.NET Core 1. x aplikacje używające uwierzytelniania lub tożsamości można zaktualizować, aby użyć nowego modelu, jak opisano poniżej.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

## <a name="update-namespaces"></a><span data-ttu-id="e6e4e-107">Aktualizowanie przestrzeni nazw</span><span class="sxs-lookup"><span data-stu-id="e6e4e-107">Update namespaces</span></span>

<span data-ttu-id="e6e4e-108">W 1. x klasy takie `IdentityRole` i `IdentityUser` zostały znalezione w przestrzeni nazw `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-108">In 1.x, classes such `IdentityRole` and `IdentityUser` were found in the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` namespace.</span></span>

<span data-ttu-id="e6e4e-109">W 2,0 obszar nazw <xref:Microsoft.AspNetCore.Identity> stał się nowym domem dla kilku takich klas.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-109">In 2.0, the <xref:Microsoft.AspNetCore.Identity> namespace became the new home for several of such classes.</span></span> <span data-ttu-id="e6e4e-110">W przypadku domyślnego kodu tożsamości klasy, których to dotyczy, obejmują `ApplicationUser` i `Startup`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-110">With the default Identity code, affected classes include `ApplicationUser` and `Startup`.</span></span> <span data-ttu-id="e6e4e-111">Dostosuj instrukcje `using`, aby rozwiązać odnośne odwołania.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-111">Adjust your `using` statements to resolve the affected references.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="e6e4e-112">Uwierzytelnianie i oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="e6e4e-112">Authentication Middleware and services</span></span>

<span data-ttu-id="e6e4e-113">W projektach 1. x uwierzytelnianie jest konfigurowane za pośrednictwem oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-113">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="e6e4e-114">Metoda pośrednicząca jest wywoływana dla każdego schematu uwierzytelniania, który ma być obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-114">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="e6e4e-115">Poniższy przykład 1. x konfiguruje uwierzytelnianie w serwisie Facebook przy użyciu tożsamości w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-115">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="e6e4e-116">W przypadku projektów 2,0 uwierzytelnianie jest konfigurowane za pośrednictwem usług.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-116">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="e6e4e-117">Każdy schemat uwierzytelniania jest rejestrowany w metodzie `ConfigureServices` *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-117">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="e6e4e-118">Metoda `UseIdentity` jest zastępowana `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-118">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="e6e4e-119">Poniższy przykład 2,0 konfiguruje uwierzytelnianie w serwisie Facebook o tożsamości w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-119">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="e6e4e-120">Metoda `UseAuthentication` dodaje pojedynczy składnik pośredniczący uwierzytelniania, który jest odpowiedzialny za automatyczne uwierzytelnianie i obsługę żądań uwierzytelniania zdalnego.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-120">The `UseAuthentication` method adds a single authentication middleware component, which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="e6e4e-121">Zastępuje wszystkie poszczególne składniki pośredniczące pojedynczym, wspólnym składnikiem pośredniczącym.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-121">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="e6e4e-122">Poniżej przedstawiono 2,0 instrukcje dotyczące migracji dla każdego głównego schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-122">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="e6e4e-123">Uwierzytelnianie na podstawie plików cookie</span><span class="sxs-lookup"><span data-stu-id="e6e4e-123">Cookie-based authentication</span></span>

<span data-ttu-id="e6e4e-124">Wybierz jedną z dwóch opcji poniżej i wprowadź niezbędne zmiany w programie *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-124">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="e6e4e-125">Używanie plików cookie z tożsamością</span><span class="sxs-lookup"><span data-stu-id="e6e4e-125">Use cookies with Identity</span></span>
    - <span data-ttu-id="e6e4e-126">Zastąp `UseIdentity` `UseAuthentication` w metodzie `Configure`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-126">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="e6e4e-127">Wywołaj metodę `AddIdentity` w metodzie `ConfigureServices`, aby dodać usługi uwierzytelniania plików cookie.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-127">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="e6e4e-128">Opcjonalnie Wywołaj metodę `ConfigureApplicationCookie` lub `ConfigureExternalCookie` w metodzie `ConfigureServices`, aby dostosować ustawienia plików cookie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-128">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="e6e4e-129">Używanie plików cookie bez tożsamości</span><span class="sxs-lookup"><span data-stu-id="e6e4e-129">Use cookies without Identity</span></span>
    - <span data-ttu-id="e6e4e-130">Zastąp wywołanie metody `UseCookieAuthentication` w metodzie `Configure` `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-130">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="e6e4e-131">Wywołaj metody `AddAuthentication` i `AddCookie` w metodzie `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-131">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="e6e4e-132">Uwierzytelnianie okaziciela JWT</span><span class="sxs-lookup"><span data-stu-id="e6e4e-132">JWT Bearer Authentication</span></span>

<span data-ttu-id="e6e4e-133">Wprowadź następujące zmiany w programie *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-133">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="e6e4e-134">Zastąp wywołanie metody `UseJwtBearerAuthentication` w metodzie `Configure` `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-134">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e6e4e-135">Wywołaj metodę `AddJwtBearer` w metodzie `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-135">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="e6e4e-136">Ten fragment kodu nie używa tożsamości, dlatego należy ustawić domyślny schemat, przekazując `JwtBearerDefaults.AuthenticationScheme` do metody `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-136">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="e6e4e-137">Uwierzytelnianie OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="e6e4e-137">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="e6e4e-138">Wprowadź następujące zmiany w programie *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-138">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="e6e4e-139">Zastąp wywołanie metody `UseOpenIdConnectAuthentication` w metodzie `Configure` `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-139">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e6e4e-140">Wywołaj metodę `AddOpenIdConnect` w metodzie `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-140">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

- <span data-ttu-id="e6e4e-141">Zastąp Właściwość `PostLogoutRedirectUri` w akcji `OpenIdConnectOptions` `SignedOutRedirectUri`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-141">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="e6e4e-142">Uwierzytelnianie za pomocą konta Facebook</span><span class="sxs-lookup"><span data-stu-id="e6e4e-142">Facebook authentication</span></span>

<span data-ttu-id="e6e4e-143">Wprowadź następujące zmiany w programie *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-143">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="e6e4e-144">Zastąp wywołanie metody `UseFacebookAuthentication` w metodzie `Configure` `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-144">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e6e4e-145">Wywołaj metodę `AddFacebook` w metodzie `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-145">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="e6e4e-146">Uwierzytelnianie za pomocą konta Google</span><span class="sxs-lookup"><span data-stu-id="e6e4e-146">Google authentication</span></span>

<span data-ttu-id="e6e4e-147">Wprowadź następujące zmiany w programie *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-147">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="e6e4e-148">Zastąp wywołanie metody `UseGoogleAuthentication` w metodzie `Configure` `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-148">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e6e4e-149">Wywołaj metodę `AddGoogle` w metodzie `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-149">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="e6e4e-150">Uwierzytelnianie za pomocą konta Microsoft</span><span class="sxs-lookup"><span data-stu-id="e6e4e-150">Microsoft Account authentication</span></span>

<span data-ttu-id="e6e4e-151">Aby uzyskać więcej informacji na temat uwierzytelniania konto Microsoft, zobacz [ten problem](https://github.com/dotnet/AspNetCore.Docs/issues/14455)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-151">For more information on Microsoft account authentication, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/14455).</span></span>

<span data-ttu-id="e6e4e-152">Wprowadź następujące zmiany w programie *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-152">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="e6e4e-153">Zastąp wywołanie metody `UseMicrosoftAccountAuthentication` w metodzie `Configure` `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-153">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e6e4e-154">Wywołaj metodę `AddMicrosoftAccount` w metodzie `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-154">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="e6e4e-155">Uwierzytelnianie za pomocą konta Twitter</span><span class="sxs-lookup"><span data-stu-id="e6e4e-155">Twitter authentication</span></span>

<span data-ttu-id="e6e4e-156">Wprowadź następujące zmiany w programie *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-156">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="e6e4e-157">Zastąp wywołanie metody `UseTwitterAuthentication` w metodzie `Configure` `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-157">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e6e4e-158">Wywołaj metodę `AddTwitter` w metodzie `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-158">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="e6e4e-159">Ustawianie domyślnych schematów uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="e6e4e-159">Setting default authentication schemes</span></span>

<span data-ttu-id="e6e4e-160">W 1. x właściwości `AutomaticAuthenticate` i `AutomaticChallenge` klasy bazowej [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) zostały przeznaczone do ustawienia w jednym schemacie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-160">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="e6e4e-161">Nie było dobrym sposobem wymuszenia tego.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-161">There was no good way to enforce this.</span></span>

<span data-ttu-id="e6e4e-162">W 2,0 te dwie właściwości zostały usunięte jako właściwości dla poszczególnych wystąpień `AuthenticationOptions`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-162">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="e6e4e-163">Można je skonfigurować w wywołaniu metody `AddAuthentication` w ramach metody `ConfigureServices` *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-163">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="e6e4e-164">W poprzednim fragmencie kodu domyślny schemat jest ustawiany na `CookieAuthenticationDefaults.AuthenticationScheme` ("pliki cookie").</span><span class="sxs-lookup"><span data-stu-id="e6e4e-164">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="e6e4e-165">Alternatywnie możesz użyć przeciążonej wersji metody `AddAuthentication`, aby ustawić więcej niż jedną właściwość.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-165">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="e6e4e-166">W poniższym przykładzie przeciążonej metody domyślny schemat jest ustawiany na `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-166">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="e6e4e-167">Schemat uwierzytelniania można także określić w ramach poszczególnych atrybutów `[Authorize]` lub zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-167">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="e6e4e-168">Zdefiniuj domyślny schemat w 2,0, jeśli spełniony jest jeden z następujących warunków:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-168">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="e6e4e-169">Chcesz, aby użytkownik był zalogowany automatycznie</span><span class="sxs-lookup"><span data-stu-id="e6e4e-169">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="e6e4e-170">Używasz atrybutu `[Authorize]` lub zasad autoryzacji bez określania schematów</span><span class="sxs-lookup"><span data-stu-id="e6e4e-170">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="e6e4e-171">Wyjątkiem od tej reguły jest metoda `AddIdentity`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-171">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="e6e4e-172">Ta metoda umożliwia dodanie plików cookie i ustawienie domyślnych schematów uwierzytelniania i wyzwania do `IdentityConstants.ApplicationScheme`plików cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-172">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="e6e4e-173">Ponadto ustawia domyślny schemat logowania na zewnętrzny plik cookie `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-173">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="e6e4e-174">Korzystanie z rozszerzeń kontekstu uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="e6e4e-174">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="e6e4e-175">Interfejs `IAuthenticationManager` jest głównym punktem wejścia do systemu uwierzytelniania 1. x.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-175">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="e6e4e-176">Został on zastąpiony nowym zestawem metod rozszerzenia `HttpContext` w przestrzeni nazw `Microsoft.AspNetCore.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-176">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="e6e4e-177">Na przykład projekty 1. x odwołują się do właściwości `Authentication`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-177">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="e6e4e-178">W projektach 2,0 zaimportuj przestrzeń nazw `Microsoft.AspNetCore.Authentication` i Usuń `Authentication` odwołania do właściwości:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-178">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="e6e4e-179">Uwierzytelnianie systemu Windows (HTTP. sys/IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="e6e4e-179">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="e6e4e-180">Istnieją dwie różne warianty uwierzytelniania systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-180">There are two variations of Windows authentication:</span></span>

* <span data-ttu-id="e6e4e-181">Host zezwala tylko uwierzytelnionym użytkownikom.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-181">The host only allows authenticated users.</span></span> <span data-ttu-id="e6e4e-182">Ta zmiana nie ma wpływ na 2,0 zmian.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-182">This variation isn't affected by the 2.0 changes.</span></span>
* <span data-ttu-id="e6e4e-183">Host umożliwia zarówno anonimowych, jak i uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-183">The host allows both anonymous and authenticated users.</span></span> <span data-ttu-id="e6e4e-184">Ta zmiana ma wpływ na zmiany 2,0.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-184">This variation is affected by the 2.0 changes.</span></span> <span data-ttu-id="e6e4e-185">Na przykład aplikacja powinna zezwalać anonimowym użytkownikom na warstwy [usług IIS](xref:host-and-deploy/iis/index) lub [http. sys](xref:fundamentals/servers/httpsys) , ale autoryzuje użytkowników na poziomie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-185">For example, the app should allow anonymous users at the [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorize users at the controller level.</span></span> <span data-ttu-id="e6e4e-186">W tym scenariuszu Ustaw domyślny schemat w metodzie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-186">In this scenario, set the default scheme in the `Startup.ConfigureServices` method.</span></span>

  <span data-ttu-id="e6e4e-187">Dla elementu [Microsoft. AspNetCore. Server. IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)Ustaw domyślny schemat na `IISDefaults.AuthenticationScheme`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-187">For [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/), set the default scheme to `IISDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="e6e4e-188">Dla elementu [Microsoft. AspNetCore. Server. HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)Ustaw domyślny schemat na `HttpSysDefaults.AuthenticationScheme`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-188">For [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/), set the default scheme to `HttpSysDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="e6e4e-189">Nie można ustawić domyślnego schematu, aby zapobiec działaniu żądania Autoryzuj (wyzwanie) z powodu następującego wyjątku:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-189">Failure to set the default scheme prevents the authorize (challenge) request from working with the following exception:</span></span>

  > <span data-ttu-id="e6e4e-190">`System.InvalidOperationException`: nie określono authenticationScheme i nie znaleziono DefaultChallengeScheme.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-190">`System.InvalidOperationException`: No authenticationScheme was specified, and there was no DefaultChallengeScheme found.</span></span>

<span data-ttu-id="e6e4e-191">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/windowsauth>.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-191">For more information, see <xref:security/authentication/windowsauth>.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="e6e4e-192">IdentityCookieOptions instances</span><span class="sxs-lookup"><span data-stu-id="e6e4e-192">IdentityCookieOptions instances</span></span>

<span data-ttu-id="e6e4e-193">Efektem ubocznym zmian 2,0 jest przełączenie do użycia nazwanych opcji zamiast wystąpień opcji plików cookie.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-193">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="e6e4e-194">Możliwość dostosowywania nazw schematu plików cookie tożsamości jest usuwana.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-194">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="e6e4e-195">Na przykład projekty 1. x wykorzystują [iniekcję konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) do przekazania parametru `IdentityCookieOptions` do *AccountController.cs* i *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-195">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs* and *ManageController.cs*.</span></span> <span data-ttu-id="e6e4e-196">Do schematu uwierzytelniania zewnętrznego pliku cookie jest uzyskiwany dostęp z podanego wystąpienia:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-196">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="e6e4e-197">Wspomniane wyżej iniekcja konstruktora nie jest konieczna w projektach 2,0 i można usunąć pole `_externalCookieScheme`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-197">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="e6e4e-198">1. x projekty używały pola `_externalCookieScheme` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-198">1.x projects used the `_externalCookieScheme` field as follows:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="e6e4e-199">W projektach 2,0 Zastąp poprzedni kod poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-199">In 2.0 projects, replace the preceding code with the following.</span></span> <span data-ttu-id="e6e4e-200">Stałej `IdentityConstants.ExternalScheme` można używać bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-200">The `IdentityConstants.ExternalScheme` constant can be used directly.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="e6e4e-201">Usuń nowo dodane wywołanie `SignOutAsync` przez zaimportowanie następującej przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-201">Resolve the newly added `SignOutAsync` call by importing the following namespace:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="e6e4e-202">Dodawanie właściwości nawigacji IdentityUser POCO</span><span class="sxs-lookup"><span data-stu-id="e6e4e-202">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="e6e4e-203">Wszystkie podstawowe właściwości nawigacji Entity Framework (EF) podstawowej `IdentityUser` POCO (zwykły obiekt CLR) zostały usunięte.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-203">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="e6e4e-204">Jeśli projekt 1. x użył tych właściwości, ręcznie dodaj je z powrotem do projektu 2,0:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-204">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="e6e4e-205">Aby zapobiec duplikowaniu kluczy obcych podczas uruchamiania EF Core migracji, Dodaj następujące elementy do metody `IdentityDbContext` klasy "`OnModelCreating` (po wywołaniu `base.OnModelCreating();`):</span><span class="sxs-lookup"><span data-stu-id="e6e4e-205">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="e6e4e-206">Zastąp GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="e6e4e-206">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="e6e4e-207">Metoda synchroniczna `GetExternalAuthenticationSchemes` została usunięta na rzecz asynchronicznej wersji.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-207">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="e6e4e-208">1. x projekty mają następujący kod w obszarze *controllers/ManageController. cs*:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-208">1.x projects have the following code in *Controllers/ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="e6e4e-209">Ta metoda pojawia się w obszarze *widoki/Account/Login. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e6e4e-209">This method appears in *Views/Account/Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

<span data-ttu-id="e6e4e-210">W przypadku projektów 2,0 użyj metody <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*>.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-210">In 2.0 projects, use the <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> method.</span></span> <span data-ttu-id="e6e4e-211">Zmiana w *ManageController.cs* przypomina następujący kod:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-211">The change in *ManageController.cs* resembles the following code:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="e6e4e-212">W polu *login. cshtml*Właściwość `AuthenticationScheme`, do której uzyskano dostęp w pętli `foreach`, zmieni się na `Name`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-212">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="e6e4e-213">Zmiana właściwości ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="e6e4e-213">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="e6e4e-214">Obiekt `ManageLoginsViewModel` jest używany w akcji `ManageLogins` *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-214">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="e6e4e-215">W projektach 1. x typ zwracany właściwości `OtherLogins` obiektu jest `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-215">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="e6e4e-216">Ten typ zwracany wymaga importu `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="e6e4e-216">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="e6e4e-217">W projektach 2,0 typ zwracany zmieni się na `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-217">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="e6e4e-218">Ten nowy typ zwracany wymaga zastąpienia importu `Microsoft.AspNetCore.Http.Authentication` przy importowaniu `Microsoft.AspNetCore.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-218">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="e6e4e-219">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e6e4e-219">Additional resources</span></span>

<span data-ttu-id="e6e4e-220">Aby uzyskać więcej informacji, zobacz [Omówienie problemu z uwierzytelnianiem 2,0](https://github.com/aspnet/Security/issues/1338) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="e6e4e-220">For more information, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
