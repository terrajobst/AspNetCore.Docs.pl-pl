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
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658747"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="8c434-103">Dostęp do obiektu HttpContext w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c434-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="8c434-104">ASP.NET Core dostęp do aplikacji `HttpContext` za pomocą interfejsu <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> i jego domyślnej implementacji <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="8c434-104">ASP.NET Core apps access `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span> <span data-ttu-id="8c434-105">Należy używać `IHttpContextAccessor` tylko wtedy, gdy potrzebujesz dostępu do `HttpContext` wewnątrz usługi.</span><span class="sxs-lookup"><span data-stu-id="8c434-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="8c434-106">Użyj kontekstu HttpContext z Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8c434-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="8c434-107"><xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages uwidacznia Właściwość <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext>:</span><span class="sxs-lookup"><span data-stu-id="8c434-107">The Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> exposes the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="8c434-108">Używanie elementu HttpContext z widoku Razor</span><span class="sxs-lookup"><span data-stu-id="8c434-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="8c434-109">Widoki Razor uwidaczniają `HttpContext` bezpośrednio przez właściwość [RazorPage. Context](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context) w widoku.</span><span class="sxs-lookup"><span data-stu-id="8c434-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context) property on the view.</span></span> <span data-ttu-id="8c434-110">Poniższy przykład pobiera bieżącą nazwę użytkownika w aplikacji intranetowej przy użyciu uwierzytelniania systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="8c434-110">The following example retrieves the current username in an intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
    
    ...
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="8c434-111">Używanie elementu HttpContext z poziomu kontrolera</span><span class="sxs-lookup"><span data-stu-id="8c434-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="8c434-112">Kontrolery uwidaczniają Właściwość [ControllerBase. HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) :</span><span class="sxs-lookup"><span data-stu-id="8c434-112">Controllers expose the [ControllerBase.HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="8c434-113">Używanie obiektu HttpContext w oprogramowaniu pośredniczącym</span><span class="sxs-lookup"><span data-stu-id="8c434-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="8c434-114">Podczas pracy z niestandardowymi składnikami oprogramowania pośredniczącego `HttpContext` jest przenoszona do metody `Invoke` lub `InvokeAsync` i można uzyskać do nich dostęp po skonfigurowaniu oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="8c434-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        ...
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="8c434-115">Używanie obiektu HttpContext ze składników niestandardowych</span><span class="sxs-lookup"><span data-stu-id="8c434-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="8c434-116">W przypadku innych platform i składników niestandardowych, które wymagają dostępu do `HttpContext`, zalecana metoda polega na zarejestrowaniu zależności przy użyciu wbudowanego kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="8c434-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="8c434-117">Kontener iniekcji zależności dostarcza `IHttpContextAccessor` do wszelkich klas, które deklarują ją jako zależność w konstruktorach:</span><span class="sxs-lookup"><span data-stu-id="8c434-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors:</span></span>

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

<span data-ttu-id="8c434-118">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="8c434-118">In the following example:</span></span>

* <span data-ttu-id="8c434-119">`UserRepository` deklaruje swoją zależność od `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="8c434-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="8c434-120">Zależność jest dostarczana, gdy iniekcja zależności rozpoznaje łańcuch zależności i tworzy wystąpienie `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="8c434-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="8c434-121">Dostęp do obiektu HttpContext z wątku w tle</span><span class="sxs-lookup"><span data-stu-id="8c434-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="8c434-122">`HttpContext` nie jest bezpieczny wątkowo.</span><span class="sxs-lookup"><span data-stu-id="8c434-122">`HttpContext` isn't thread-safe.</span></span> <span data-ttu-id="8c434-123">Odczytanie lub zapisanie właściwości `HttpContext` poza przetwarzaniem żądania może spowodować <xref:System.NullReferenceException>.</span><span class="sxs-lookup"><span data-stu-id="8c434-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a <xref:System.NullReferenceException>.</span></span>

> [!NOTE]
> <span data-ttu-id="8c434-124">Jeśli aplikacja generuje sporadyczne błędy `NullReferenceException`, przejrzyj fragmenty kodu, które rozpoczynają przetwarzanie w tle lub kontynuują przetwarzanie po zakończeniu żądania.</span><span class="sxs-lookup"><span data-stu-id="8c434-124">If your app generates sporadic `NullReferenceException` errors, review parts of the code that start background processing or that continue processing after a request completes.</span></span> <span data-ttu-id="8c434-125">Poszukaj błędów, takich jak Definiowanie metody kontrolera jako `async void`.</span><span class="sxs-lookup"><span data-stu-id="8c434-125">Look for mistakes, such as defining a controller method as `async void`.</span></span>

<span data-ttu-id="8c434-126">Aby bezpiecznie wykonywać prace w tle za pomocą `HttpContext` danych:</span><span class="sxs-lookup"><span data-stu-id="8c434-126">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="8c434-127">Skopiuj wymagane dane podczas przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="8c434-127">Copy the required data during request processing.</span></span>
* <span data-ttu-id="8c434-128">Przekaż skopiowane dane do zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="8c434-128">Pass the copied data to a background task.</span></span>

<span data-ttu-id="8c434-129">Aby uniknąć niebezpiecznego kodu, nigdy nie przekazuj `HttpContext` do metody, która wykonuje działania w tle.</span><span class="sxs-lookup"><span data-stu-id="8c434-129">To avoid unsafe code, never pass the `HttpContext` into a method that performs background work.</span></span> <span data-ttu-id="8c434-130">Zamiast tego Przekaż wymagane dane.</span><span class="sxs-lookup"><span data-stu-id="8c434-130">Pass the required data instead.</span></span> <span data-ttu-id="8c434-131">W poniższym przykładzie `SendEmailCore` jest wywoływana w celu rozpoczęcia wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="8c434-131">In the following example, `SendEmailCore` is called to start sending an email.</span></span> <span data-ttu-id="8c434-132">`correlationId` jest przenoszona do `SendEmailCore`, a nie do `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="8c434-132">The `correlationId` is passed to `SendEmailCore`, not the `HttpContext`.</span></span> <span data-ttu-id="8c434-133">Wykonanie kodu nie oczekuje na ukończenie `SendEmailCore`:</span><span class="sxs-lookup"><span data-stu-id="8c434-133">Code execution doesn't wait for `SendEmailCore` to complete:</span></span>

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
