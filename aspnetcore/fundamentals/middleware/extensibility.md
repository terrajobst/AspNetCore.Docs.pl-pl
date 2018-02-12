---
title: "Aktywacji opartej na fabryki oprogramowanie pośredniczące w ASP.NET Core"
author: guardrex
description: "Dowiedz się, jak użyć jednoznacznie oprogramowania pośredniczącego z aktywacji opartej na fabryki implementacja platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 5e7e807a0107f1f500d43b5ffd4362b11f53118b
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/11/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="2c0d0-103">Aktywacji opartej na fabryki oprogramowanie pośredniczące w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c0d0-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="2c0d0-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2c0d0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2c0d0-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) punkt rozszerzalności dla [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) aktywacji.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="2c0d0-106">`UseMiddleware`metody rozszerzenia, sprawdź, czy oprogramowanie pośredniczące w zarejestrowany typ implementuje `IMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="2c0d0-107">Jeśli tak, `IMiddlewareFactory` wystąpienia zarejestrowany w kontenerze jest używany do rozpoznawania `IMiddleware` implementacji zamiast logiki aktywacji opartych na konwencjach oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="2c0d0-108">Oprogramowanie pośredniczące jest zarejestrowany jako usługa zakresie lub przejściowej w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="2c0d0-109">Korzyści:</span><span class="sxs-lookup"><span data-stu-id="2c0d0-109">Benefits:</span></span>

* <span data-ttu-id="2c0d0-110">Aktywacja na żądanie (iniekcji usługi w zakresie)</span><span class="sxs-lookup"><span data-stu-id="2c0d0-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="2c0d0-111">Silne wpisywanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="2c0d0-111">Strong typing of middleware</span></span>

<span data-ttu-id="2c0d0-112">`IMiddleware`została aktywowana na żądanie, i usługi w zakresie mogą zostać dodane do konstruktora oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="2c0d0-113">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2c0d0-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2c0d0-114">Przykładowa aplikacja ilustruje aktywowany przez oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="2c0d0-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="2c0d0-115">Konwencja (`ConventionalMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="2c0d0-115">Convention (`ConventionalMiddleware`).</span></span> <span data-ttu-id="2c0d0-116">Aby uzyskać więcej informacji o aktywacji z konwencjonalnej oprogramowanie pośredniczące, zobacz [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) tematu.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="2c0d0-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) implementacji (`IMiddlewareMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="2c0d0-117">An [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) implementation (`IMiddlewareMiddleware`).</span></span> <span data-ttu-id="2c0d0-118">Wartość domyślna [klasy MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) aktywuje oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="2c0d0-119">Implementacje oprogramowania pośredniczącego działają tak samo i rejestrowanie podana wartość parametru ciągu zapytania (`key`).</span><span class="sxs-lookup"><span data-stu-id="2c0d0-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="2c0d0-120">Middlewares Użyj kontekstu wprowadzony bazy danych (zakresie usługa), aby zapisać wartość ciągu kwerendy w bazie danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="2c0d0-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="2c0d0-121">IMiddleware</span></span>

<span data-ttu-id="2c0d0-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definiuje oprogramowanie pośredniczące do potoku żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="2c0d0-123">[InvokeAsync (Element HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) metoda obsługuje żądania i zwraca `Task` reprezentujący wykonywanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="2c0d0-124">Oprogramowanie pośredniczące aktywowany przez Konwencję:</span><span class="sxs-lookup"><span data-stu-id="2c0d0-124">Middleware activated by convention:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="2c0d0-125">Oprogramowanie pośredniczące aktywowany przez `MiddlewareFactory`:</span><span class="sxs-lookup"><span data-stu-id="2c0d0-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

<span data-ttu-id="2c0d0-126">Rozszerzenia są tworzone dla middlewares:</span><span class="sxs-lookup"><span data-stu-id="2c0d0-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="2c0d0-127">Nie można przekazać obiektów do oprogramowania pośredniczącego aktywowany fabryki z `UseMiddleware`:</span><span class="sxs-lookup"><span data-stu-id="2c0d0-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

<span data-ttu-id="2c0d0-128">Oprogramowanie pośredniczące aktywowany fabryki jest dodawany do kontenera wbudowanych w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="2c0d0-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="2c0d0-129">Zarówno middlewares są rejestrowane w potoku przetwarzania żądań w `Configure`:</span><span class="sxs-lookup"><span data-stu-id="2c0d0-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="2c0d0-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="2c0d0-130">IMiddlewareFactory</span></span>

<span data-ttu-id="2c0d0-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) udostępnia metody do tworzenia oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="2c0d0-132">Wdrożenie fabryki oprogramowanie pośredniczące jest zarejestrowany w kontenerze w zakresie usługi.</span><span class="sxs-lookup"><span data-stu-id="2c0d0-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="2c0d0-133">Wartość domyślna `IMiddlewareFactory` implementacji [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), znajduje się w [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu ([źródło odwołania](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span><span class="sxs-lookup"><span data-stu-id="2c0d0-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package ([reference source](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2c0d0-134">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2c0d0-134">Additional resources</span></span>

* [<span data-ttu-id="2c0d0-135">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="2c0d0-135">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="2c0d0-136">Aktywacji opartej na fabryki oprogramowania pośredniczącego z kontenerem innych firm</span><span class="sxs-lookup"><span data-stu-id="2c0d0-136">Factory-based middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
