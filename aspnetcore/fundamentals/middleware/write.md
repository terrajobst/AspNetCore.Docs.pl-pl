---
title: Pisanie niestandardowych platformy ASP.NET Core oprogramowania pośredniczącego.
author: rick-anderson
description: Dowiedz się, jak napisać niestandardowy platformy ASP.NET Core oprogramowania pośredniczącego.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/17/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 352db93dd7061070c76e34f6c03883f68e2041ee
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67167106"
---
# <a name="write-custom-aspnet-core-middleware"></a><span data-ttu-id="fe9a6-103">Pisanie niestandardowych platformy ASP.NET Core oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-103">Write custom ASP.NET Core middleware</span></span>

<span data-ttu-id="fe9a6-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="fe9a6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="fe9a6-105">Oprogramowanie pośredniczące to oprogramowanie, które jest umieszczone w potoku aplikacji do obsługi żądań i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="fe9a6-106">Platforma ASP.NET Core oferuje rozbudowanego zestawu składników oprogramowania pośredniczącego wbudowanych, ale w niektórych scenariuszach możesz chcieć napisać niestandardowego oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-106">ASP.NET Core provides a rich set of built-in middleware components, but in some scenarios you might want to write a custom middleware.</span></span>

## <a name="middleware-class"></a><span data-ttu-id="fe9a6-107">Klasa oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="fe9a6-107">Middleware class</span></span>

<span data-ttu-id="fe9a6-108">Oprogramowanie pośredniczące jest zazwyczaj hermetyzowane w klasie i widoczne z metodą rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-108">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="fe9a6-109">Należy wziąć pod uwagę następujące oprogramowanie pośredniczące, które ustawia kulturę bieżącego żądania z ciągu zapytania:</span><span class="sxs-lookup"><span data-stu-id="fe9a6-109">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="fe9a6-110">Poprzedni przykładowy kod jest używany do zademonstrowania tworzenia składników oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-110">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="fe9a6-111">Obsługę wbudowanych lokalizacja platformy ASP.NET Core, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-111">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="fe9a6-112">Testowanie oprogramowania pośredniczącego, przekazując w kulturze.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-112">Test the middleware by passing in the culture.</span></span> <span data-ttu-id="fe9a6-113">Na przykład żądania `https://localhost:5001/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-113">For example, request `https://localhost:5001/?culture=no`.</span></span>

<span data-ttu-id="fe9a6-114">Poniższy kod powoduje delegata oprogramowania pośredniczącego do klasy:</span><span class="sxs-lookup"><span data-stu-id="fe9a6-114">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="fe9a6-115">Klasa oprogramowania pośredniczącego musi zawierać:</span><span class="sxs-lookup"><span data-stu-id="fe9a6-115">The middleware class must include:</span></span>

* <span data-ttu-id="fe9a6-116">Konstruktor publiczny z parametrem typu <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-116">A public constructor with a parameter of type <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span>
* <span data-ttu-id="fe9a6-117">Publiczną metodę o nazwie `Invoke` lub `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-117">A public method named `Invoke` or `InvokeAsync`.</span></span> <span data-ttu-id="fe9a6-118">Ta metoda musi:</span><span class="sxs-lookup"><span data-stu-id="fe9a6-118">This method must:</span></span>
  * <span data-ttu-id="fe9a6-119">Zwróć `Task`.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-119">Return a `Task`.</span></span>
  * <span data-ttu-id="fe9a6-120">Zaakceptuj pierwszy parametr typu <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-120">Accept a first parameter of type <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>
  
<span data-ttu-id="fe9a6-121">Dodatkowe parametry dla konstruktora i `Invoke` / `InvokeAsync` są wypełniane przez [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fe9a6-121">Additional parameters for the constructor and `Invoke`/`InvokeAsync` are populated by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware-dependencies"></a><span data-ttu-id="fe9a6-122">Zależności oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="fe9a6-122">Middleware dependencies</span></span>

<span data-ttu-id="fe9a6-123">Oprogramowanie pośredniczące powinno wykonać [jawne zależności zasady](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) uwidaczniając jego zależności w jego konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-123">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="fe9a6-124">Oprogramowanie pośredniczące jest tworzona raz na *okres istnienia aplikacji*.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-124">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="fe9a6-125">Zobacz [zależności oprogramowania pośredniczącego żądania](#per-request-middleware-dependencies) sekcji, jeśli zachodzi potrzeba udostępniania usług za pomocą oprogramowania pośredniczącego w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-125">See the [Per-request middleware dependencies](#per-request-middleware-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="fe9a6-126">Składniki oprogramowania pośredniczącego może rozpoznać ich zależności z [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) za pośrednictwem parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-126">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="fe9a6-127">[UseMiddleware&lt;T&gt; ](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) mogą również akceptować dodatkowe parametry bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-127">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

## <a name="per-request-middleware-dependencies"></a><span data-ttu-id="fe9a6-128">Zależności oprogramowania pośredniczącego żądania</span><span class="sxs-lookup"><span data-stu-id="fe9a6-128">Per-request middleware dependencies</span></span>

<span data-ttu-id="fe9a6-129">Ponieważ oprogramowanie pośredniczące jest konstruowany przy uruchamianiu aplikacji, nie dla poszczególnych żądań, *zakresie* okres istnienia, obejmujący usługi używane przez oprogramowanie pośredniczące konstruktory nie są udostępniane przy użyciu innych typów zależności, wprowadzony podczas każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-129">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="fe9a6-130">Jeśli musisz udostępnić *zakresie* usługi między oprogramowania pośredniczącego a innymi typami danych, należy dodać tych usług `Invoke` podpis metody.</span><span class="sxs-lookup"><span data-stu-id="fe9a6-130">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="fe9a6-131">`Invoke` Metoda może obsługiwać dodatkowe parametry, które są wypełniane przez DI:</span><span class="sxs-lookup"><span data-stu-id="fe9a6-131">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

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

## <a name="middleware-extension-method"></a><span data-ttu-id="fe9a6-132">Metoda rozszerzenia oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="fe9a6-132">Middleware extension method</span></span>

<span data-ttu-id="fe9a6-133">Następujące metody rozszerzenia udostępnia oprogramowanie pośredniczące za pośrednictwem <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="fe9a6-133">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="fe9a6-134">Poniższy kod wywołuje oprogramowanie pośredniczące z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="fe9a6-134">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

## <a name="additional-resources"></a><span data-ttu-id="fe9a6-135">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fe9a6-135">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
