---
title: Oprogramowanie pośredniczące oparte na fabryce aktywacji w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać silnie typizowane oprogramowania pośredniczącego z implementacją oparte na fabryce aktywacji w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 9305616ce3f2ef49cf9dfcab719f673c0fb4b51e
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "58809169"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="8869f-103">Oprogramowanie pośredniczące oparte na fabryce aktywacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8869f-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="8869f-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8869f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8869f-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> jest punktem rozszerzalności dla [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) aktywacji.</span><span class="sxs-lookup"><span data-stu-id="8869f-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="8869f-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> metody rozszerzenia, sprawdź, jeśli oprogramowanie pośredniczące, zarejestrowany typ implementuje <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span><span class="sxs-lookup"><span data-stu-id="8869f-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="8869f-107">Jeśli tak jest, <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> wystąpieniu zarejestrowana w kontenerze jest używany do rozpoznawania <xref:Microsoft.AspNetCore.Http.IMiddleware> implementacji zamiast przy użyciu logiki aktywacji oprogramowanie pośredniczące oparte na Konwencji.</span><span class="sxs-lookup"><span data-stu-id="8869f-107">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="8869f-108">Oprogramowanie pośredniczące jest zarejestrowany jako [usługi o określonym zakresie lub przejściowy](xref:fundamentals/dependency-injection#service-lifetimes) w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8869f-108">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="8869f-109">Korzyści:</span><span class="sxs-lookup"><span data-stu-id="8869f-109">Benefits:</span></span>

* <span data-ttu-id="8869f-110">Aktywacji dla każdego żądania klienta (iniekcji usługi o określonym zakresie)</span><span class="sxs-lookup"><span data-stu-id="8869f-110">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="8869f-111">Silne wpisywanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="8869f-111">Strong typing of middleware</span></span>

<span data-ttu-id="8869f-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> została aktywowana dla każdego żądania klienta (połączenie), usługi o określonym zakresie może wprowadzone do konstruktora oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="8869f-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="8869f-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8869f-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="8869f-114">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="8869f-114">IMiddleware</span></span>

<span data-ttu-id="8869f-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> Definiuje oprogramowanie pośredniczące do potoku żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8869f-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="8869f-116">[InvokeAsync (HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) metoda obsługuje żądania i zwraca <xref:System.Threading.Tasks.Task> reprezentujący wykonywania oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="8869f-116">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="8869f-117">Oprogramowanie pośredniczące aktywowane zgodnie z Konwencją:</span><span class="sxs-lookup"><span data-stu-id="8869f-117">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="8869f-118">Oprogramowanie pośredniczące aktywowany przez <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span><span class="sxs-lookup"><span data-stu-id="8869f-118">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="8869f-119">Rozszerzenia są tworzone dla middlewares:</span><span class="sxs-lookup"><span data-stu-id="8869f-119">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="8869f-120">Nie można przekazać obiekty do aktywacji fabryki oprogramowania pośredniczącego z <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span><span class="sxs-lookup"><span data-stu-id="8869f-120">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="8869f-121">Oprogramowanie pośredniczące aktywacji fabryki jest dodawany do kontenerze wbudowane w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8869f-121">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="8869f-122">Zarówno middlewares są rejestrowane w potoku przetwarzania żądań w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="8869f-122">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="8869f-123">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="8869f-123">IMiddlewareFactory</span></span>

<span data-ttu-id="8869f-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> udostępnia metody do tworzenia oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="8869f-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="8869f-125">Wdrożenie fabryki oprogramowanie pośredniczące jest zarejestrowany w kontenerze w zakresie usługi.</span><span class="sxs-lookup"><span data-stu-id="8869f-125">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="8869f-126">Wartość domyślna <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementacji <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, znajduje się w [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="8869f-126">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8869f-127">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8869f-127">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
