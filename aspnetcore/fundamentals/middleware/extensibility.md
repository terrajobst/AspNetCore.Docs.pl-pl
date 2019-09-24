---
title: Aktywacja oprogramowania pośredniczącego oparta na fabryce w ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać silnego typu oprogramowania pośredniczącego z implementacją aktywacji opartej na fabryce w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 17018d2dd20ed7b26bd0aa1095fa720a73f77261
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186945"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="ec14a-103">Aktywacja oprogramowania pośredniczącego oparta na fabryce w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec14a-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="ec14a-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ec14a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ec14a-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware>jest punktem rozszerzalności dla aktywacji [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) .</span><span class="sxs-lookup"><span data-stu-id="ec14a-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="ec14a-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>metody rozszerzające sprawdzają, czy typ zarejestrowanego <xref:Microsoft.AspNetCore.Http.IMiddleware>oprogramowania pośredniczącego jest zaimplementowany.</span><span class="sxs-lookup"><span data-stu-id="ec14a-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="ec14a-107">W takim przypadku <xref:Microsoft.AspNetCore.Http.IMiddleware> wystąpieniezarejestrowanewkontenerzejestużywanedorozpoznawaniaimplementacjizamiastkorzystaniazlogikiaktywacjioprogramowaniapośredniczącegoopartego<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> na Konwencji.</span><span class="sxs-lookup"><span data-stu-id="ec14a-107">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="ec14a-108">Oprogramowanie pośredniczące jest rejestrowane jako [Usługa w zakresie lub przejściowym](xref:fundamentals/dependency-injection#service-lifetimes) w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ec14a-108">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="ec14a-109">Korzysta</span><span class="sxs-lookup"><span data-stu-id="ec14a-109">Benefits:</span></span>

* <span data-ttu-id="ec14a-110">Aktywacja na żądanie klienta (iniekcja usług objętych zakresem)</span><span class="sxs-lookup"><span data-stu-id="ec14a-110">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="ec14a-111">Silne wpisywanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="ec14a-111">Strong typing of middleware</span></span>

<span data-ttu-id="ec14a-112"><xref:Microsoft.AspNetCore.Http.IMiddleware>jest aktywowany dla żądania klienta (połączenie), więc usługi w zakresie mogą być wstrzykiwane do konstruktora oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="ec14a-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="ec14a-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ec14a-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="ec14a-114">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="ec14a-114">IMiddleware</span></span>

<span data-ttu-id="ec14a-115"><xref:Microsoft.AspNetCore.Http.IMiddleware>definiuje oprogramowanie pośredniczące dla potoku żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ec14a-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="ec14a-116">Metoda [InvokeAsync (HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) obsługuje żądania i zwraca <xref:System.Threading.Tasks.Task> wartość reprezentującą wykonywanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="ec14a-116">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="ec14a-117">Oprogramowanie pośredniczące aktywowane zgodnie z konwencją:</span><span class="sxs-lookup"><span data-stu-id="ec14a-117">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="ec14a-118">Oprogramowanie pośredniczące aktywowane <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>przez:</span><span class="sxs-lookup"><span data-stu-id="ec14a-118">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="ec14a-119">Dla middlewares są tworzone rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="ec14a-119">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="ec14a-120">Nie jest możliwe przekazanie obiektów do oprogramowania pośredniczącego aktywowanego przez fabrykę przy użyciu <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span><span class="sxs-lookup"><span data-stu-id="ec14a-120">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="ec14a-121">Oprogramowanie pośredniczące aktywowane fabrycznie jest dodawane do wbudowanego kontenera w programie `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ec14a-121">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="ec14a-122">Oba middlewares są rejestrowane w potoku przetwarzania żądań w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="ec14a-122">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="ec14a-123">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="ec14a-123">IMiddlewareFactory</span></span>

<span data-ttu-id="ec14a-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>zapewnia metody tworzenia oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="ec14a-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="ec14a-125">Implementacja fabryki oprogramowania pośredniczącego jest zarejestrowana w kontenerze jako usługa objęta zakresem.</span><span class="sxs-lookup"><span data-stu-id="ec14a-125">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="ec14a-126">Domyślna <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementacja, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, znajduje się w pakiecie [Microsoft. AspNetCore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) .</span><span class="sxs-lookup"><span data-stu-id="ec14a-126">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ec14a-127"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware>jest punktem rozszerzalności dla aktywacji [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) .</span><span class="sxs-lookup"><span data-stu-id="ec14a-127"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="ec14a-128"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>metody rozszerzające sprawdzają, czy typ zarejestrowanego <xref:Microsoft.AspNetCore.Http.IMiddleware>oprogramowania pośredniczącego jest zaimplementowany.</span><span class="sxs-lookup"><span data-stu-id="ec14a-128"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="ec14a-129">W takim przypadku <xref:Microsoft.AspNetCore.Http.IMiddleware> wystąpieniezarejestrowanewkontenerzejestużywanedorozpoznawaniaimplementacjizamiastkorzystaniazlogikiaktywacjioprogramowaniapośredniczącegoopartego<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> na Konwencji.</span><span class="sxs-lookup"><span data-stu-id="ec14a-129">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="ec14a-130">Oprogramowanie pośredniczące jest rejestrowane jako [Usługa w zakresie lub przejściowym](xref:fundamentals/dependency-injection#service-lifetimes) w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ec14a-130">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="ec14a-131">Korzysta</span><span class="sxs-lookup"><span data-stu-id="ec14a-131">Benefits:</span></span>

* <span data-ttu-id="ec14a-132">Aktywacja na żądanie klienta (iniekcja usług objętych zakresem)</span><span class="sxs-lookup"><span data-stu-id="ec14a-132">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="ec14a-133">Silne wpisywanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="ec14a-133">Strong typing of middleware</span></span>

<span data-ttu-id="ec14a-134"><xref:Microsoft.AspNetCore.Http.IMiddleware>jest aktywowany dla żądania klienta (połączenie), więc usługi w zakresie mogą być wstrzykiwane do konstruktora oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="ec14a-134"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="ec14a-135">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ec14a-135">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="ec14a-136">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="ec14a-136">IMiddleware</span></span>

<span data-ttu-id="ec14a-137"><xref:Microsoft.AspNetCore.Http.IMiddleware>definiuje oprogramowanie pośredniczące dla potoku żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ec14a-137"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="ec14a-138">Metoda [InvokeAsync (HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) obsługuje żądania i zwraca <xref:System.Threading.Tasks.Task> wartość reprezentującą wykonywanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="ec14a-138">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="ec14a-139">Oprogramowanie pośredniczące aktywowane zgodnie z konwencją:</span><span class="sxs-lookup"><span data-stu-id="ec14a-139">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="ec14a-140">Oprogramowanie pośredniczące aktywowane <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>przez:</span><span class="sxs-lookup"><span data-stu-id="ec14a-140">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="ec14a-141">Dla middlewares są tworzone rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="ec14a-141">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="ec14a-142">Nie jest możliwe przekazanie obiektów do oprogramowania pośredniczącego aktywowanego przez fabrykę przy użyciu <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span><span class="sxs-lookup"><span data-stu-id="ec14a-142">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="ec14a-143">Oprogramowanie pośredniczące aktywowane fabrycznie jest dodawane do wbudowanego kontenera w programie `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ec14a-143">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="ec14a-144">Oba middlewares są rejestrowane w potoku przetwarzania żądań w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="ec14a-144">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="ec14a-145">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="ec14a-145">IMiddlewareFactory</span></span>

<span data-ttu-id="ec14a-146"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>zapewnia metody tworzenia oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="ec14a-146"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="ec14a-147">Implementacja fabryki oprogramowania pośredniczącego jest zarejestrowana w kontenerze jako usługa objęta zakresem.</span><span class="sxs-lookup"><span data-stu-id="ec14a-147">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="ec14a-148">Domyślna <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementacja, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, znajduje się w pakiecie [Microsoft. AspNetCore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) .</span><span class="sxs-lookup"><span data-stu-id="ec14a-148">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="ec14a-149">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ec14a-149">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
