---
title: HttpContext dostępu w programie ASP.NET Core
author: coderandhiker
description: Dowiedz się, jak dostęp do obiektu HttpContext w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 6b932d40aa501e7f046c67dc8f2edaae78531213
ms.sourcegitcommit: 516d0645c35ea784a3ae807be087ae70446a46ee
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320674"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="43e2c-103">HttpContext dostępu w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43e2c-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="43e2c-104">Dostęp do aplikacji platformy ASP.NET Core `HttpContext` za pośrednictwem [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interfejsu i jego domyślna implementacja [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="43e2c-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="43e2c-105">Jest tylko do użycia `IHttpContextAccessor` gdy będziesz potrzebować dostępu do `HttpContext` wewnątrz usługi.</span><span class="sxs-lookup"><span data-stu-id="43e2c-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="43e2c-106">HttpContext korzystanie ze stron Razor</span><span class="sxs-lookup"><span data-stu-id="43e2c-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="43e2c-107">Strony Razor [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) udostępnia [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) właściwości:</span><span class="sxs-lookup"><span data-stu-id="43e2c-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="43e2c-108">Użyj obiektu HttpContext z widoku Razor</span><span class="sxs-lookup"><span data-stu-id="43e2c-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="43e2c-109">Udostępnianie widokami razor `HttpContext` bezpośrednio za pośrednictwem [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) właściwości w widoku.</span><span class="sxs-lookup"><span data-stu-id="43e2c-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="43e2c-110">Poniższy przykład pobiera bieżącej nazwy użytkownika w aplikacji sieci Intranet przy użyciu uwierzytelniania Windows:</span><span class="sxs-lookup"><span data-stu-id="43e2c-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml

@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="43e2c-111">Użyj obiektu HttpContext za pomocą kontrolera</span><span class="sxs-lookup"><span data-stu-id="43e2c-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="43e2c-112">Uwidacznianie kontrolerów [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) właściwości:</span><span class="sxs-lookup"><span data-stu-id="43e2c-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="43e2c-113">HttpContext korzystanie z oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="43e2c-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="43e2c-114">Podczas pracy ze składnikami niestandardowe oprogramowanie pośredniczące `HttpContext` jest przekazywana do `Invoke` lub `InvokeAsync` metody i jest dostępny w przypadku skonfigurowania oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="43e2c-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="43e2c-115">Użyj obiektu HttpContext ze składnikami niestandardowymi</span><span class="sxs-lookup"><span data-stu-id="43e2c-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="43e2c-116">Dla innych framework i składników niestandardowych, które wymagają dostępu do `HttpContext`, zalecanym podejściem jest zarejestrować zależności za pomocą wbudowanych [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="43e2c-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="43e2c-117">Dostarcza kontener iniekcji zależności `IHttpContextAccessor` do dowolnej klasy, które Zadeklaruj go jako zależności w ich konstruktory.</span><span class="sxs-lookup"><span data-stu-id="43e2c-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

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

<span data-ttu-id="43e2c-118">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="43e2c-118">In the preceding example:</span></span>

* <span data-ttu-id="43e2c-119">`UserRepository` deklaruje jego zależności na `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="43e2c-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="43e2c-120">Zależność zostanie dostarczona podczas wstrzykiwanie zależności jest rozpoznawana jako łańcuch zależności i tworzy wystąpienie `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="43e2c-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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
