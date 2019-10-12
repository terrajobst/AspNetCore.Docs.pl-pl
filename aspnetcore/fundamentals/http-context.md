---
title: Dostęp do obiektu HttpContext w ASP.NET Core
author: coderandhiker
description: Dowiedz się, jak uzyskać dostęp do obiektu HttpContext w ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 888adf6d61e6968127385952e65f942e86b7eb63
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288968"
---
# <a name="access-httpcontext-in-aspnet-core"></a>Dostęp do obiektu HttpContext w ASP.NET Core

ASP.NET Core aplikacje uzyskują dostęp do `HttpContext` za pomocą interfejsu [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) i jego domyślnej implementacji [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor). Jest to konieczne tylko w przypadku korzystania z `IHttpContextAccessor`, gdy potrzebny jest dostęp do `HttpContext` w ramach usługi.

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a>Użyj kontekstu HttpContext z Razor Pages

[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) Razor Pages uwidacznia Właściwość [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) :

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

## <a name="use-httpcontext-from-a-razor-view"></a>Używanie elementu HttpContext z widoku Razor

Widoki Razor uwidaczniają `HttpContext` bezpośrednio za pośrednictwem właściwości [RazorPage. Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) w widoku. Poniższy przykład pobiera bieżącą nazwę użytkownika w aplikacji intranetowej przy użyciu uwierzytelniania systemu Windows:

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a>Używanie elementu HttpContext z poziomu kontrolera

Kontrolery uwidaczniają Właściwość [ControllerBase. HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) :

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

## <a name="use-httpcontext-from-middleware"></a>Używanie obiektu HttpContext w oprogramowaniu pośredniczącym

Podczas pracy z niestandardowymi składnikami oprogramowania pośredniczącego `HttpContext` jest przenoszona do metody `Invoke` lub `InvokeAsync` i można uzyskać do nich dostęp po skonfigurowaniu oprogramowania pośredniczącego:

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>Używanie obiektu HttpContext ze składników niestandardowych

W przypadku innych platform i składników niestandardowych, które wymagają dostępu do `HttpContext` Zalecanym podejściem jest zarejestrowanie zależności przy użyciu wbudowanego kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) . Kontener iniekcji zależności dostarcza `IHttpContextAccessor` do dowolnych klas, które deklarują ją jako zależność w konstruktorach.

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

* `UserRepository` deklaruje zależność od `IHttpContextAccessor`.
* Zależność jest dostarczana, gdy iniekcja zależności rozpoznaje łańcuch zależności i tworzy wystąpienie `UserRepository`.

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

## <a name="httpcontext-access-from-a-background-thread"></a>Dostęp do obiektu HttpContext z wątku w tle

`HttpContext` nie jest bezpieczny wątkowo. Odczyt lub zapis właściwości `HttpContext` poza przetwarzaniem żądania może spowodować `NullReferenceException`.

> [!NOTE]
> Użycie `HttpContext` poza przetwarzaniem żądania często powoduje `NullReferenceException`. Jeśli aplikacja generuje sporadyczne @no__t 0s, przejrzyj fragmenty kodu, które rozpoczynają przetwarzanie w tle, lub Kontynuuj przetwarzanie po zakończeniu żądania. Poszukaj błędów, takich jak Definiowanie metody kontrolera jako `async void`.

Aby bezpiecznie wykonać prace w tle z `HttpContext` danych:

* Skopiuj wymagane dane podczas przetwarzania żądania.
* Przekaż skopiowane dane do zadania w tle.

Aby uniknąć niebezpiecznego kodu, nigdy nie przekazuj `HttpContext` do metody, która działa w tle — Przekaż wymagane dane.

```csharp
public class EmailController
{
    public ActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        // Starts sending an email, but doesn't wait for it to complete
        _ = SendEmailCore(correlationId);
        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        // send the email
    }
}
