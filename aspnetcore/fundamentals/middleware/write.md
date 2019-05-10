---
title: Pisanie niestandardowych platformy ASP.NET Core oprogramowania pośredniczącego.
author: rick-anderson
description: Dowiedz się, jak napisać niestandardowy platformy ASP.NET Core oprogramowania pośredniczącego.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 2c5577394a10370d92c8a83f9d806b63f3245c8b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901684"
---
# <a name="write-custom-aspnet-core-middleware"></a><span data-ttu-id="5f58e-103">Pisanie niestandardowych platformy ASP.NET Core oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="5f58e-103">Write custom ASP.NET Core middleware</span></span>

<span data-ttu-id="5f58e-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="5f58e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5f58e-105">Oprogramowanie pośredniczące to oprogramowanie, które jest umieszczone w potoku aplikacji do obsługi żądań i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5f58e-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="5f58e-106">Platforma ASP.NET Core oferuje rozbudowanego zestawu składników oprogramowania pośredniczącego wbudowanych, ale w niektórych scenariuszach możesz chcieć napisać niestandardowego oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="5f58e-106">ASP.NET Core provides a rich set of built-in middleware components, but in some scenarios you might want to write a custom middleware.</span></span>

## <a name="middleware-class"></a><span data-ttu-id="5f58e-107">Klasa oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="5f58e-107">Middleware class</span></span>

<span data-ttu-id="5f58e-108">Oprogramowanie pośredniczące jest zazwyczaj hermetyzowane w klasie i widoczne z metodą rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="5f58e-108">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="5f58e-109">Należy wziąć pod uwagę następujące oprogramowanie pośredniczące, które ustawia kulturę bieżącego żądania z ciągu zapytania:</span><span class="sxs-lookup"><span data-stu-id="5f58e-109">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="5f58e-110">Poprzedni przykładowy kod jest używany do zademonstrowania tworzenia składników oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="5f58e-110">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="5f58e-111">Obsługę wbudowanych lokalizacja platformy ASP.NET Core, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="5f58e-111">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="5f58e-112">Możesz przetestować oprogramowanie pośredniczące, przekazując w kulturze.</span><span class="sxs-lookup"><span data-stu-id="5f58e-112">You can test the middleware by passing in the culture.</span></span> <span data-ttu-id="5f58e-113">Na przykład `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="5f58e-113">For example, `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="5f58e-114">Poniższy kod powoduje delegata oprogramowania pośredniczącego do klasy:</span><span class="sxs-lookup"><span data-stu-id="5f58e-114">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5f58e-115">Oprogramowanie pośredniczące `Task` nazwę metody musi być `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="5f58e-115">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="5f58e-116">W programie ASP.NET Core 2.0 lub nowszej, może to być albo `Invoke` lub `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="5f58e-116">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

## <a name="middleware-extension-method"></a><span data-ttu-id="5f58e-117">Metoda rozszerzenia oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="5f58e-117">Middleware extension method</span></span>

<span data-ttu-id="5f58e-118">Następujące metody rozszerzenia udostępnia oprogramowanie pośredniczące za pośrednictwem <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="5f58e-118">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="5f58e-119">Poniższy kod wywołuje oprogramowanie pośredniczące z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="5f58e-119">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="5f58e-120">Oprogramowanie pośredniczące powinno wykonać [jawne zależności zasady](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) uwidaczniając jego zależności w jego konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="5f58e-120">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="5f58e-121">Oprogramowanie pośredniczące jest tworzona raz na *okres istnienia aplikacji*.</span><span class="sxs-lookup"><span data-stu-id="5f58e-121">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="5f58e-122">Zobacz [zależności żądania](#per-request-dependencies) sekcji, jeśli zachodzi potrzeba udostępniania usług za pomocą oprogramowania pośredniczącego w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="5f58e-122">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="5f58e-123">Składniki oprogramowania pośredniczącego może rozpoznać ich zależności z [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) za pośrednictwem parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="5f58e-123">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="5f58e-124">[UseMiddleware&lt;T&gt; ](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) mogą również akceptować dodatkowe parametry bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="5f58e-124">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

## <a name="per-request-dependencies"></a><span data-ttu-id="5f58e-125">Zależności żądania</span><span class="sxs-lookup"><span data-stu-id="5f58e-125">Per-request dependencies</span></span>

<span data-ttu-id="5f58e-126">Ponieważ oprogramowanie pośredniczące jest konstruowany przy uruchamianiu aplikacji, nie dla poszczególnych żądań, *zakresie* okres istnienia, obejmujący usługi używane przez oprogramowanie pośredniczące konstruktory nie są udostępniane przy użyciu innych typów zależności, wprowadzony podczas każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="5f58e-126">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="5f58e-127">Jeśli musisz udostępnić *zakresie* usługi między oprogramowania pośredniczącego a innymi typami danych, należy dodać tych usług `Invoke` podpis metody.</span><span class="sxs-lookup"><span data-stu-id="5f58e-127">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="5f58e-128">`Invoke` Metoda może obsługiwać dodatkowe parametry, które są wypełniane przez DI:</span><span class="sxs-lookup"><span data-stu-id="5f58e-128">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="5f58e-129">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5f58e-129">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
