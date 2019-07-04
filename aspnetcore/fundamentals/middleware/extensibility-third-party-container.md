---
title: Oprogramowanie pośredniczące aktywacji z kontenerem innych firm w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać silnie typizowane oprogramowanie pośredniczące oparte na fabryce aktywacji i kontenerem innych firm w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/03/2019
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: 4bc99b4c336aba611287c9fbe03d4252f8abee5b
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561648"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="bc1f0-103">Oprogramowanie pośredniczące aktywacji z kontenerem innych firm w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc1f0-103">Middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="bc1f0-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bc1f0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bc1f0-105">W tym artykule przedstawiono sposób użycia <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> i <xref:Microsoft.AspNetCore.Http.IMiddleware> jako punkt rozszerzalności dla [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) aktywacji z kontenerem innej firmy.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-105">This article demonstrates how to use <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> and <xref:Microsoft.AspNetCore.Http.IMiddleware> as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="bc1f0-106">Aby uzyskać wprowadzające informacje na `IMiddlewareFactory` i `IMiddleware`, zobacz <xref:fundamentals/middleware/extensibility>.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see <xref:fundamentals/middleware/extensibility>.</span></span>

<span data-ttu-id="bc1f0-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bc1f0-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bc1f0-108">Przykładowa aplikacja pokazuje, oprogramowanie pośredniczące aktywacji przez `IMiddlewareFactory` implementacji `SimpleInjectorMiddlewareFactory`.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="bc1f0-109">W przykładzie użyto [proste iniektor](https://simpleinjector.org) kontenera (DI) iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-109">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="bc1f0-110">Przykład oprogramowania pośredniczącego implementacji rejestruje wartość dostarczona przez parametr ciągu zapytania (`key`).</span><span class="sxs-lookup"><span data-stu-id="bc1f0-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="bc1f0-111">Oprogramowanie pośredniczące używa kontekstu wprowadzonego bazy danych (usługi o określonym zakresie) do rejestrowania wartości ciągu zapytania w bazie danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="bc1f0-112">Ta aplikacja używa przykładowych [proste iniektor](https://github.com/simpleinjector/SimpleInjector) wyłącznie w celach demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="bc1f0-113">Korzystanie z prostych iniektor nie potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="bc1f0-114">Metody aktywacji oprogramowania pośredniczącego opisanej w dokumentacji iniektor proste i problemy usługi GitHub są zalecane przez maintainers z prostego iniektor.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="bc1f0-115">Aby uzyskać więcej informacji, zobacz [dokumentacji proste iniektor](https://simpleinjector.readthedocs.io/en/latest/index.html) i [repozytorium GitHub usługi Simple iniektor](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="bc1f0-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="bc1f0-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="bc1f0-116">IMiddlewareFactory</span></span>

<span data-ttu-id="bc1f0-117"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> udostępnia metody do tworzenia oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-117"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span>

<span data-ttu-id="bc1f0-118">W przykładowej aplikacji fabrykę oprogramowanie pośredniczące jest implementowane w celu tworzenia `SimpleInjectorActivatedMiddleware` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="bc1f0-119">Fabryka oprogramowania pośredniczącego używa iniektor prosty kontener, aby rozwiązać oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="bc1f0-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="bc1f0-120">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="bc1f0-120">IMiddleware</span></span>

<span data-ttu-id="bc1f0-121"><xref:Microsoft.AspNetCore.Http.IMiddleware> Definiuje oprogramowanie pośredniczące do potoku żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-121"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="bc1f0-122">Oprogramowanie pośredniczące aktywowany przez `IMiddlewareFactory` implementacji (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span><span class="sxs-lookup"><span data-stu-id="bc1f0-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="bc1f0-123">Rozszerzenie jest tworzona dla oprogramowania pośredniczącego (*Middleware/MiddlewareExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="bc1f0-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="bc1f0-124">`Startup.ConfigureServices` należy wykonać kilka zadań:</span><span class="sxs-lookup"><span data-stu-id="bc1f0-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="bc1f0-125">Skonfiguruj kontener iniektor proste.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="bc1f0-126">Zarejestruj fabryki i oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="bc1f0-127">Udostępnia kontekst bazy danych aplikacji z kontenera iniektor proste.</span><span class="sxs-lookup"><span data-stu-id="bc1f0-127">Make the app's database context available from the Simple Injector container.</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="bc1f0-128">Oprogramowanie pośredniczące jest zarejestrowany w potoku przetwarzania żądań w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="bc1f0-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a><span data-ttu-id="bc1f0-129">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bc1f0-129">Additional resources</span></span>

* [<span data-ttu-id="bc1f0-130">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="bc1f0-130">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="bc1f0-131">Oprogramowanie pośredniczące oparte na fabryce aktywacji</span><span class="sxs-lookup"><span data-stu-id="bc1f0-131">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="bc1f0-132">Proste repozytorium GitHub iniektor</span><span class="sxs-lookup"><span data-stu-id="bc1f0-132">Simple Injector GitHub repository</span></span>](https://github.com/simpleinjector/SimpleInjector)
* [<span data-ttu-id="bc1f0-133">Proste iniektor dokumentacji</span><span class="sxs-lookup"><span data-stu-id="bc1f0-133">Simple Injector documentation</span></span>](https://simpleinjector.readthedocs.io/en/latest/index.html)
