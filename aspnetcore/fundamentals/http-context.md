---
title: Dostęp do obiektu HttpContext w ASP.NET Core
author: coderandhiker
description: Dowiedz się, jak uzyskać dostęp do obiektu HttpContext w ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 0bf40f9cd2554f5ba01ccc06001fa4f1940d51a5
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289049"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="4318a-103">Dostęp do obiektu HttpContext w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4318a-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="4318a-104">ASP.NET Core aplikacje uzyskują dostęp do `HttpContext` za pomocą interfejsu [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) oraz jego domyślnej implementacji [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="4318a-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="4318a-105">Należy używać `IHttpContextAccessor` tylko wtedy, gdy potrzebujesz dostępu do `HttpContext` wewnątrz usługi.</span><span class="sxs-lookup"><span data-stu-id="4318a-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="4318a-106">Użyj kontekstu HttpContext z Razor Pages</span><span class="sxs-lookup"><span data-stu-id="4318a-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="4318a-107">[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) Razor Pages uwidacznia Właściwość [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) :</span><span class="sxs-lookup"><span data-stu-id="4318a-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="4318a-108">Używanie elementu HttpContext z widoku Razor</span><span class="sxs-lookup"><span data-stu-id="4318a-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="4318a-109">Widoki Razor uwidaczniają `HttpContext` bezpośrednio przez właściwość [RazorPage. Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) w widoku.</span><span class="sxs-lookup"><span data-stu-id="4318a-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="4318a-110">Poniższy przykład pobiera bieżącą nazwę użytkownika w aplikacji intranetowej przy użyciu uwierzytelniania systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="4318a-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="4318a-111">Używanie elementu HttpContext z poziomu kontrolera</span><span class="sxs-lookup"><span data-stu-id="4318a-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="4318a-112">Kontrolery uwidaczniają Właściwość [ControllerBase. HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) :</span><span class="sxs-lookup"><span data-stu-id="4318a-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="4318a-113">Używanie obiektu HttpContext w oprogramowaniu pośredniczącym</span><span class="sxs-lookup"><span data-stu-id="4318a-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="4318a-114">Podczas pracy z niestandardowymi składnikami oprogramowania pośredniczącego `HttpContext` jest przenoszona do metody `Invoke` lub `InvokeAsync` i można uzyskać do nich dostęp po skonfigurowaniu oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="4318a-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="4318a-115">Używanie obiektu HttpContext ze składników niestandardowych</span><span class="sxs-lookup"><span data-stu-id="4318a-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="4318a-116">W przypadku innych platform i składników niestandardowych, które wymagają dostępu do `HttpContext`, zalecana metoda polega na zarejestrowaniu zależności przy użyciu wbudowanego kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="4318a-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="4318a-117">Kontener iniekcji zależności dostarcza `IHttpContextAccessor` do wszystkich klas, które deklarują ją jako zależność w konstruktorach.</span><span class="sxs-lookup"><span data-stu-id="4318a-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

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

<span data-ttu-id="4318a-118">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="4318a-118">In the following example:</span></span>

* <span data-ttu-id="4318a-119">`UserRepository` deklaruje swoją zależność od `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="4318a-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="4318a-120">Zależność jest dostarczana, gdy iniekcja zależności rozpoznaje łańcuch zależności i tworzy wystąpienie `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="4318a-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="4318a-121">Dostęp do obiektu HttpContext z wątku w tle</span><span class="sxs-lookup"><span data-stu-id="4318a-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="4318a-122">`HttpContext` nie jest bezpieczna wątkowo.</span><span class="sxs-lookup"><span data-stu-id="4318a-122">`HttpContext` is not thread-safe.</span></span> <span data-ttu-id="4318a-123">Odczytanie lub zapisanie właściwości `HttpContext` poza przetwarzaniem żądania może spowodować `NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="4318a-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a `NullReferenceException`.</span></span>

> [!NOTE]
> <span data-ttu-id="4318a-124">Użycie `HttpContext` poza przetwarzaniem żądania często powoduje `NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="4318a-124">Using `HttpContext` outside of processing a request often results in a `NullReferenceException`.</span></span> <span data-ttu-id="4318a-125">Jeśli aplikacja generuje sporadyczne `NullReferenceException`s, zapoznaj się z częścią kodu, który rozpoczyna przetwarzanie w tle, lub Kontynuuj przetwarzanie po zakończeniu żądania.</span><span class="sxs-lookup"><span data-stu-id="4318a-125">If your app generates sporadic `NullReferenceException`s , review parts of the code that start background processing, or that continue processing after a request completes.</span></span> <span data-ttu-id="4318a-126">Poszukaj błędów, takich jak Definiowanie metody kontrolera jako `async void`.</span><span class="sxs-lookup"><span data-stu-id="4318a-126">Look for mistakes, such as defining a controller method as `async void`.</span></span>

<span data-ttu-id="4318a-127">Aby bezpiecznie wykonywać prace w tle za pomocą `HttpContext` danych:</span><span class="sxs-lookup"><span data-stu-id="4318a-127">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="4318a-128">Skopiuj wymagane dane podczas przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="4318a-128">Copy the required data during request processing.</span></span>
* <span data-ttu-id="4318a-129">Przekaż skopiowane dane do zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="4318a-129">Pass the copied data to a background task.</span></span>

<span data-ttu-id="4318a-130">Aby uniknąć niebezpiecznego kodu, nigdy nie przekazuj `HttpContext` do metody, która działa w tle — Przekaż wymagane dane.</span><span class="sxs-lookup"><span data-stu-id="4318a-130">To avoid unsafe code, never pass the `HttpContext` into a method that does background work - pass the data you need instead.</span></span>

```csharp
public class EmailController : Controller
{
    public IActionResult SendEmail(string email)
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
