---
title: HttpContext dostępu w programie ASP.NET Core
author: coderandhiker
description: Dowiedz się, jak dostęp do obiektu HttpContext w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: babc637cdec8590ac14f7924c17e862e5b2f6a81
ms.sourcegitcommit: d22b3c23c45a076c4f394a70b1c8df2fbcdf656d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55428489"
---
# <a name="access-httpcontext-in-aspnet-core"></a>HttpContext dostępu w programie ASP.NET Core

Dostęp do aplikacji platformy ASP.NET Core `HttpContext` za pośrednictwem [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interfejsu i jego domyślna implementacja [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor). Jest tylko do użycia `IHttpContextAccessor` gdy będziesz potrzebować dostępu do `HttpContext` wewnątrz usługi.

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

## <a name="use-httpcontext-from-a-razor-view"></a>Użyj obiektu HttpContext z widoku Razor

Udostępnianie widokami razor `HttpContext` bezpośrednio za pośrednictwem [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) właściwości w widoku. Poniższy przykład pobiera bieżącej nazwy użytkownika w aplikacji sieci Intranet przy użyciu uwierzytelniania Windows:

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

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

W poniższym przykładzie:

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
