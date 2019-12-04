---
title: Dostęp do obiektu HttpContext w ASP.NET Core
author: coderandhiker
description: Dowiedz się, jak uzyskać dostęp do obiektu HttpContext w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: fundamentals/httpcontext
ms.openlocfilehash: 8a7ee180380c42ea745c91b8e6a18c1baa820220
ms.sourcegitcommit: 5974e3e66dab3398ecf2324fbb82a9c5636f70de
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74778742"
---
# <a name="access-httpcontext-in-aspnet-core"></a>Dostęp do obiektu HttpContext w ASP.NET Core

ASP.NET Core dostęp do aplikacji `HttpContext` za pomocą interfejsu <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> i jego domyślnej implementacji <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>. Należy używać `IHttpContextAccessor` tylko wtedy, gdy potrzebujesz dostępu do `HttpContext` wewnątrz usługi.

## <a name="use-httpcontext-from-razor-pages"></a>Użyj kontekstu HttpContext z Razor Pages

<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages uwidacznia Właściwość <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext>:

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

## <a name="use-httpcontext-from-a-razor-view"></a>Używanie elementu HttpContext z widoku Razor

Widoki Razor uwidaczniają `HttpContext` bezpośrednio przez właściwość [RazorPage. Context](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context) w widoku. Poniższy przykład pobiera bieżącą nazwę użytkownika w aplikacji intranetowej przy użyciu uwierzytelniania systemu Windows:

```cshtml
@{
    var username = Context.User.Identity.Name;
    
    ...
}
```

## <a name="use-httpcontext-from-a-controller"></a>Używanie elementu HttpContext z poziomu kontrolera

Kontrolery uwidaczniają Właściwość [ControllerBase. HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) :

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;

        ...

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
        ...
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>Używanie obiektu HttpContext ze składników niestandardowych

W przypadku innych platform i składników niestandardowych, które wymagają dostępu do `HttpContext`, zalecana metoda polega na zarejestrowaniu zależności przy użyciu wbudowanego kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) . Kontener iniekcji zależności dostarcza `IHttpContextAccessor` do wszelkich klas, które deklarują ją jako zależność w konstruktorach:

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddControllersWithViews();
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

W poniższym przykładzie:

* `UserRepository` deklaruje swoją zależność od `IHttpContextAccessor`.
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

`HttpContext` nie jest bezpieczny wątkowo. Odczytanie lub zapisanie właściwości `HttpContext` poza przetwarzaniem żądania może spowodować <xref:System.NullReferenceException>.

> [!NOTE]
> Jeśli aplikacja generuje sporadyczne błędy `NullReferenceException`, przejrzyj fragmenty kodu, które rozpoczynają przetwarzanie w tle lub kontynuują przetwarzanie po zakończeniu żądania. Poszukaj błędów, takich jak Definiowanie metody kontrolera jako `async void`.

Aby bezpiecznie wykonywać prace w tle za pomocą `HttpContext` danych:

* Skopiuj wymagane dane podczas przetwarzania żądania.
* Przekaż skopiowane dane do zadania w tle.

Aby uniknąć niebezpiecznego kodu, nigdy nie przekazuj `HttpContext` do metody, która wykonuje działania w tle. Zamiast tego Przekaż wymagane dane. W poniższym przykładzie `SendEmailCore` jest wywoływana w celu rozpoczęcia wysyłania wiadomości e-mail. `correlationId` jest przenoszona do `SendEmailCore`, a nie do `HttpContext`. Wykonanie kodu nie oczekuje na ukończenie `SendEmailCore`:

```csharp
public class EmailController : Controller
{
    public IActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        _ = SendEmailCore(correlationId);

        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        ...
    }
}
