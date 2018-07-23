---
title: HttpContext dostępu w programie ASP.NET Core
author: coderandhiker
description: Dowiedz się, jak dostęp do obiektu HttpContext w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: b1ff80943db1788b465accd51c70a3c3a3462d5c
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202721"
---
# <a name="access-httpcontext-in-aspnet-core"></a>HttpContext dostępu w programie ASP.NET Core

Dostęp do aplikacji platformy ASP.NET Core `HttpContext` za pośrednictwem [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interfejsu i jego domyślna implementacja [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a>HttpContext korzystanie ze stron Razor

Strony Razor [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) udostępnia [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) właściwości:

```csharp
public class AboutModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = HttpContext.Request.PathBase;
    }
}
```

::: moniker-end

## <a name="use-httpcontext-from-a-controller"></a>Użyj obiektu HttpContext za pomocą kontrolera

Uwidacznianie kontrolerów [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) właściwości:

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;
        // Do something with the PathBase.

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a>HttpContext korzystanie z oprogramowania pośredniczącego

Podczas pracy ze składnikami niestandardowe oprogramowanie pośredniczące `HttpContext` jest przekazywana do `Invoke` lub `InvokeAsync` metody i jest dostępny w przypadku skonfigurowania oprogramowania pośredniczącego:

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>Użyj obiektu HttpContext ze składnikami niestandardowymi

Dla innych framework i składników niestandardowych, które wymagają dostępu do `HttpContext`, zalecanym podejściem jest zarejestrować zależności za pomocą wbudowanych [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera. Dostarcza kontener iniekcji zależności `IHttpContextAccessor` do dowolnej klasy, które Zadeklaruj go jako zależności w ich konstruktory.

::: moniker range=">= aspnetcore-2.1"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc();
     services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

W poprzednim przykładzie:

* `UserRepository` deklaruje jego zależności na `IHttpContextAccessor`.
* Zależność zostanie dostarczona podczas wstrzykiwanie zależności jest rozpoznawana jako łańcuch zależności i tworzy wystąpienie `UserRepository`.

```csharp
public class UserRepository : IUserRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public UserRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public void LogCurrentUser()
    {
        var username = _httpContextAccessor.HttpContext.User.Identity.Name;
        service.LogAccessRequest(username);
    }
}
```
