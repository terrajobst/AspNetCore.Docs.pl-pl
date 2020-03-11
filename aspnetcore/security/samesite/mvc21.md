---
title: Przykładowy plik cookie SameSite 2,1 ASP.NET Core MVC
author: rick-anderson
description: Przykładowy plik cookie SameSite 2,1 ASP.NET Core MVC
monikerRange: '>= aspnetcore-2.1 < aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: security/samesite/mvc21
ms.openlocfilehash: 14f3d3d27d5740519e1ba57529d5694b4cdeb9ec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667749"
---
# <a name="aspnet-core-21-mvc-samesite-cookie-sample"></a><span data-ttu-id="ae4f5-103">Przykładowy plik cookie SameSite 2,1 ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="ae4f5-103">ASP.NET Core 2.1 MVC SameSite cookie sample</span></span>

<span data-ttu-id="ae4f5-104">ASP.NET Core 2,1 ma wbudowaną obsługę atrybutu [SameSite](https://www.owasp.org/index.php/SameSite) , ale został on zapisany w oryginalnym standardzie.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-104">ASP.NET Core 2.1 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it was written to the original standard.</span></span> <span data-ttu-id="ae4f5-105">[Zachowanie poprawek](https://github.com/dotnet/aspnetcore/issues/8212) zostało zmienione znaczenie `SameSite.None`, aby wyemitować atrybut sameSite o wartości `None`, zamiast wyemitować wartość wcale.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-105">The [patched behavior](https://github.com/dotnet/aspnetcore/issues/8212) changed the meaning of `SameSite.None` to emit the sameSite attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="ae4f5-106">Jeśli nie chcesz emitować wartości, możesz ustawić właściwość `SameSite` w pliku cookie na wartość-1.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="ae4f5-107">Pisanie atrybutu SameSite</span><span class="sxs-lookup"><span data-stu-id="ae4f5-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="ae4f5-108">Poniżej przedstawiono przykład sposobu pisania atrybutu SameSite w pliku cookie:</span><span class="sxs-lookup"><span data-stu-id="ae4f5-108">Following is an example of how to write a SameSite attribute on a cookie:</span></span>

```c#
var cookieOptions = new CookieOptions
{
    // Set the secure flag, which Chrome's changes will require for SameSite none.
    // Note this will also require you to be running on HTTPS
    Secure = true,

    // Set the cookie to HTTP only which is good practice unless you really do need
    // to access it client side in scripts.
    HttpOnly = true,

    // Add the SameSite attribute, this will emit the attribute with a value of none.
    // To not emit the attribute at all set the SameSite property to (SameSiteMode)(-1).
    SameSite = SameSiteMode.None
};

// Add the cookie to the response cookie collection
Response.Cookies.Append(CookieName, "cookieValue", cookieOptions);
```

## <a name="setting-cookie-authentication-and-session-state-cookies"></a><span data-ttu-id="ae4f5-109">Ustawianie uwierzytelniania plików cookie i plików cookie stanu sesji</span><span class="sxs-lookup"><span data-stu-id="ae4f5-109">Setting Cookie Authentication and Session State cookies</span></span>

<span data-ttu-id="ae4f5-110">Uwierzytelnianie plików cookie, stan sesji i [różne inne składniki](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1) ustawiają opcje sameSite za pomocą opcji plików cookie, na przykład</span><span class="sxs-lookup"><span data-stu-id="ae4f5-110">Cookie authentication, session state and [various other components](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1) set their sameSite options via Cookie options, for example</span></span>

```c#
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.Cookie.SameSite = SameSiteMode.None;
        options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        options.Cookie.IsEssential = true;
    });

services.AddSession(options =>
{
    options.Cookie.SameSite = SameSiteMode.None;
    options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
    options.Cookie.IsEssential = true;
});
```

<span data-ttu-id="ae4f5-111">W powyższym kodzie zarówno uwierzytelnianie plików cookie, jak i stan sesji ustawiają swój atrybut sameSite na `None`, emitując atrybut z wartością `None`, a także ustawić atrybut Secure na true.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-111">In the preceding code, both cookie authentication and session state set their sameSite attribute to `None`, emitting the attribute with a `None` value, and also set the Secure attribute to true.</span></span>

### <a name="run-the-sample"></a><span data-ttu-id="ae4f5-112">Uruchamianie aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="ae4f5-112">Run the sample</span></span>

<span data-ttu-id="ae4f5-113">W przypadku uruchomienia [przykładowego projektu](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)Załaduj debuger przeglądarki na stronie początkowej i użyj go do wyświetlenia kolekcji plików cookie dla tej witryny.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-113">If you run the [sample project](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC), load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span> <span data-ttu-id="ae4f5-114">Aby to zrobić, w przeglądarce Edge i Chrome naciśnij `F12` następnie wybierz kartę `Application` i kliknij adres URL witryny poniżej opcji `Cookies` w sekcji `Storage`.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-114">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Lista plików cookie debugera przeglądarki](BrowserDebugger.png)

<span data-ttu-id="ae4f5-116">Po kliknięciu przycisku "Utwórz SameSite cookie" można zobaczyć z obrazu powyżej, że plik cookie utworzony przez próbkę ma wartość `Lax`, dopasowując wartość ustawioną w [przykładowym kodzie](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="ae4f5-116">You can see from the image above that the cookie created by the sample when you click the "Create SameSite Cookie" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="ae4f5-117">Przechwytywanie plików cookie</span><span class="sxs-lookup"><span data-stu-id="ae4f5-117">Intercepting cookies</span></span>

<span data-ttu-id="ae4f5-118">W celu przechwycenia plików cookie w celu dostosowania wartości brak zgodnie z jej obsługą w agencie przeglądarki użytkownika należy użyć `CookiePolicy` oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-118">In order to intercept cookies, to adjust the none value according to its support in the user's browser agent you must use the `CookiePolicy` middleware.</span></span> <span data-ttu-id="ae4f5-119">Ta wartość musi być umieszczona w potoku żądania HTTP **przed** wszelkimi składnikami, które zapisują pliki cookie i są skonfigurowane w `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-119">This must be placed into the http request pipeline **before** any components that write cookies and configured within `ConfigureServices()`.</span></span>

<span data-ttu-id="ae4f5-120">Aby wstawić go do potoku, użyj `app.UseCookiePolicy()` w metodzie `Configure(IApplicationBuilder, IHostingEnvironment)` w [Startup.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="ae4f5-120">To insert it into the pipeline use `app.UseCookiePolicy()` in the `Configure(IApplicationBuilder, IHostingEnvironment)` method in [Startup.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs).</span></span> <span data-ttu-id="ae4f5-121">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ae4f5-121">For example:</span></span>

```c#
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
       app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseAuthentication();
    app.UseSession();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="ae4f5-122">Następnie w `ConfigureServices(IServiceCollection services)` skonfigurować zasady plików cookie, aby wywoływać klasy pomocnika, gdy pliki cookie są dołączane lub usuwane.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-122">Then in the `ConfigureServices(IServiceCollection services)` configure the cookie policy to call out to a helper class when cookies are appended or deleted.</span></span> <span data-ttu-id="ae4f5-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ae4f5-123">For example:</span></span>

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<CookiePolicyOptions>(options =>
    {
        options.CheckConsentNeeded = context => true;
        options.MinimumSameSitePolicy = SameSiteMode.None;
        options.OnAppendCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
        options.OnDeleteCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
    });
}

private void CheckSameSite(HttpContext httpContext, CookieOptions options)
{
    if (options.SameSite == SameSiteMode.None)
    {
        var userAgent = httpContext.Request.Headers["User-Agent"].ToString();
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            options.SameSite = (SameSiteMode)(-1);
        }
    }
}
```

<span data-ttu-id="ae4f5-124">`CheckSameSite(HttpContext, CookieOptions)`funkcji pomocnika:</span><span class="sxs-lookup"><span data-stu-id="ae4f5-124">The helper function `CheckSameSite(HttpContext, CookieOptions)`:</span></span>

* <span data-ttu-id="ae4f5-125">Jest wywoływana, gdy pliki cookie są dołączane do żądania lub usuwane z żądania.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-125">Is called when cookies are appended to the request or deleted from the request.</span></span>
* <span data-ttu-id="ae4f5-126">Sprawdza, czy właściwość `SameSite` jest ustawiona na `None`.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-126">Checks to see if the `SameSite` property is set to `None`.</span></span>
* <span data-ttu-id="ae4f5-127">Jeśli `SameSite` jest ustawiona na `None`, a agent bieżącego użytkownika jest znany jako nieobsługujący wartości atrybutu none.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-127">If `SameSite` is set to `None` and the current user agent is known to not support the none attribute value.</span></span> <span data-ttu-id="ae4f5-128">Sprawdzanie jest wykonywane przy użyciu klasy [SameSiteSupport](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs) :</span><span class="sxs-lookup"><span data-stu-id="ae4f5-128">The check is done using the [SameSiteSupport](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs) class:</span></span>
  * <span data-ttu-id="ae4f5-129">Ustawia `SameSite`, aby nie emitować wartości przez ustawienie właściwości na `(SameSiteMode)(-1)`</span><span class="sxs-lookup"><span data-stu-id="ae4f5-129">Sets `SameSite` to not emit the value by setting the property to `(SameSiteMode)(-1)`</span></span>

## <a name="targeting-net-framework"></a><span data-ttu-id="ae4f5-130">.NET Framework określania wartości docelowej</span><span class="sxs-lookup"><span data-stu-id="ae4f5-130">Targeting .NET Framework</span></span>

<span data-ttu-id="ae4f5-131">ASP.NET Core i system. Web (ASP.NET Classic) mają niezależne implementacje SameSite.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-131">ASP.NET Core and System.Web (ASP.NET Classic) have independent implementations of SameSite.</span></span> <span data-ttu-id="ae4f5-132">Poprawki SameSite KB dla .NET Framework nie są wymagane w przypadku używania ASP.NET Core, ani do programu System. Web SameSite minimalne wymagania wersji platformy (.NET 4.7.2) mają zastosowanie do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-132">The SameSite KB patches for .NET Framework are not required if using ASP.NET Core nor does the System.Web SameSite minimum framework version requirement (.NET 4.7.2) apply to ASP.NET Core.</span></span>

<span data-ttu-id="ae4f5-133">ASP.NET Core na platformie .NET wymaga aktualizacji zależności pakietów NuGet w celu uzyskania odpowiednich poprawek.</span><span class="sxs-lookup"><span data-stu-id="ae4f5-133">ASP.NET Core on .NET requires updating nuget package dependencies to get the appropriate fixes.</span></span>

<span data-ttu-id="ae4f5-134">Aby uzyskać ASP.NET Core zmiany .NET Framework upewnij się, że masz bezpośrednie odwołanie do pakietów i wersji z poprawkami (2.1.14 lub nowszymi wersjami 2,1).</span><span class="sxs-lookup"><span data-stu-id="ae4f5-134">To get the ASP.NET Core changes for .NET Framework ensure that you have a direct reference to the patched packages and versions (2.1.14 or later 2.1 versions).</span></span>

```xml
<PackageReference Include="Microsoft.Net.Http.Headers" Version="2.1.14" />
<PackageReference Include="Microsoft.AspNetCore.CookiePolicy" Version="2.1.14" />
```

### <a name="more-information"></a><span data-ttu-id="ae4f5-135">Więcej informacji</span><span class="sxs-lookup"><span data-stu-id="ae4f5-135">More Information</span></span>
 
<span data-ttu-id="ae4f5-136">[Aktualizacje programu Chrome](https://www.chromium.org/updates/same-site)
[ASP.NET Core dokumentacja SameSite](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)
[ASP.NET Core 2,1 SameSite zmiany anonsu](https://github.com/dotnet/aspnetcore/issues/8212)</span><span class="sxs-lookup"><span data-stu-id="ae4f5-136">[Chrome Updates](https://www.chromium.org/updates/same-site)
[ASP.NET Core SameSite Documentation](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)
[ASP.NET Core 2.1 SameSite Change Announcement](https://github.com/dotnet/aspnetcore/issues/8212)</span></span>