---
title: Oprogramowanie pośredniczące oparte na fabryce aktywacji w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać silnie typizowane oprogramowania pośredniczącego z implementacją oparte na fabryce aktywacji w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2018
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 346f5e7b28a9cd17a03a864772ed8b2e4be9455b
ms.sourcegitcommit: 2c158fcfd325cad97ead608a816e525fe3dcf757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2018
ms.locfileid: "41752664"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="9dd8d-103">Oprogramowanie pośredniczące oparte na fabryce aktywacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9dd8d-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="9dd8d-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9dd8d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9dd8d-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) jest punktem rozszerzalności dla [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) aktywacji.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="9dd8d-106">`UseMiddleware` metody rozszerzenia, sprawdź, jeśli oprogramowanie pośredniczące, zarejestrowany typ implementuje `IMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="9dd8d-107">Jeśli tak jest, `IMiddlewareFactory` wystąpieniu zarejestrowana w kontenerze jest używany do rozpoznawania `IMiddleware` implementacji zamiast przy użyciu logiki aktywacji oprogramowanie pośredniczące oparte na Konwencji.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="9dd8d-108">Oprogramowanie pośredniczące jest zarejestrowany jako usługa o określonym zakresie lub przejściowy w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="9dd8d-109">Korzyści:</span><span class="sxs-lookup"><span data-stu-id="9dd8d-109">Benefits:</span></span>

* <span data-ttu-id="9dd8d-110">Aktywacja na żądanie (iniekcji usługi o określonym zakresie)</span><span class="sxs-lookup"><span data-stu-id="9dd8d-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="9dd8d-111">Silne wpisywanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="9dd8d-111">Strong typing of middleware</span></span>

<span data-ttu-id="9dd8d-112">`IMiddleware` aktywowano na żądanie, dzięki czemu usługi o określonym zakresie można wstrzyknięte do konstruktora oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="9dd8d-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9dd8d-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9dd8d-114">Przykładowa aplikacja pokazuje aktywowany przez oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="9dd8d-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="9dd8d-115">Konwencja.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-115">Convention.</span></span> <span data-ttu-id="9dd8d-116">Aby uzyskać więcej informacji na temat aktywacji są konwencjonalne funkcje oprogramowania pośredniczącego, zobacz [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) tematu.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="9dd8d-117">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementacji.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-117">An [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="9dd8d-118">Wartość domyślna [klasy MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) aktywuje oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="9dd8d-119">Implementacje oprogramowania pośredniczącego działać identycznie i Zapisz wartość dostarczona przez parametr ciągu zapytania (`key`).</span><span class="sxs-lookup"><span data-stu-id="9dd8d-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="9dd8d-120">Middlewares użyć kontekstu wprowadzonego bazy danych (usługi o określonym zakresie) do rejestrowania wartości ciągu zapytania w bazie danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="9dd8d-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="9dd8d-121">IMiddleware</span></span>

<span data-ttu-id="9dd8d-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definiuje oprogramowanie pośredniczące do potoku żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="9dd8d-123">[InvokeAsync (HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) metoda obsługuje żądania i zwraca `Task` reprezentujący wykonywania oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="9dd8d-124">Oprogramowanie pośredniczące aktywowane zgodnie z Konwencją:</span><span class="sxs-lookup"><span data-stu-id="9dd8d-124">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="9dd8d-125">Oprogramowanie pośredniczące aktywowany przez `MiddlewareFactory`:</span><span class="sxs-lookup"><span data-stu-id="9dd8d-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[](extensibility/sample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="9dd8d-126">Rozszerzenia są tworzone dla middlewares:</span><span class="sxs-lookup"><span data-stu-id="9dd8d-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="9dd8d-127">Nie można przekazać obiekty do aktywacji fabryki oprogramowania pośredniczącego z `UseMiddleware`:</span><span class="sxs-lookup"><span data-stu-id="9dd8d-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="9dd8d-128">Oprogramowanie pośredniczące aktywacji fabryki jest dodawany do kontenerze wbudowane w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9dd8d-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

<span data-ttu-id="9dd8d-129">Zarówno middlewares są rejestrowane w potoku przetwarzania żądań w `Configure`:</span><span class="sxs-lookup"><span data-stu-id="9dd8d-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=14-15)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="9dd8d-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="9dd8d-130">IMiddlewareFactory</span></span>

<span data-ttu-id="9dd8d-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) udostępnia metody do tworzenia oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="9dd8d-132">Wdrożenie fabryki oprogramowanie pośredniczące jest zarejestrowany w kontenerze w zakresie usługi.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="9dd8d-133">Wartość domyślna `IMiddlewareFactory` implementacji [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), znajduje się w [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="9dd8d-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9dd8d-134">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9dd8d-134">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
