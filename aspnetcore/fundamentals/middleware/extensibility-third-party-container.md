---
title: Oprogramowanie pośredniczące aktywacji z kontenerem innych firm w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać silnie typizowane oprogramowanie pośredniczące oparte na fabryce aktywacji i kontenerem innych firm w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2018
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: 9e4d4c6c0232ebc51ad08923e10164262b652280
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901183"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="e2c4e-103">Oprogramowanie pośredniczące aktywacji z kontenerem innych firm w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2c4e-103">Middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="e2c4e-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e2c4e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e2c4e-105">W tym artykule przedstawiono sposób użycia [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) i [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) jako punkt rozszerzalności dla [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) aktywacji z kontenerem innej firmy.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-105">This article demonstrates how to use [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) and [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="e2c4e-106">Aby uzyskać wprowadzające informacje na `IMiddlewareFactory` i `IMiddleware`, zobacz [aktywacji oprogramowanie pośredniczące oparte na fabryce](xref:fundamentals/middleware/extensibility) tematu.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see the [Factory-based middleware activation](xref:fundamentals/middleware/extensibility) topic.</span></span>

<span data-ttu-id="e2c4e-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e2c4e-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e2c4e-108">Przykładowa aplikacja pokazuje, oprogramowanie pośredniczące aktywacji przez `IMiddlewareFactory` implementacji `SimpleInjectorMiddlewareFactory`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="e2c4e-109">W przykładzie użyto [proste iniektor](https://simpleinjector.org) kontenera (DI) iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-109">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="e2c4e-110">Przykład oprogramowania pośredniczącego implementacji rejestruje wartość dostarczona przez parametr ciągu zapytania (`key`).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="e2c4e-111">Oprogramowanie pośredniczące używa kontekstu wprowadzonego bazy danych (usługi o określonym zakresie) do rejestrowania wartości ciągu zapytania w bazie danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="e2c4e-112">Ta aplikacja używa przykładowych [proste iniektor](https://github.com/simpleinjector/SimpleInjector) wyłącznie w celach demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="e2c4e-113">Korzystanie z prostych iniektor nie potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="e2c4e-114">Metody aktywacji oprogramowania pośredniczącego opisanej w dokumentacji iniektor proste i problemy usługi GitHub są zalecane przez maintainers z prostego iniektor.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="e2c4e-115">Aby uzyskać więcej informacji, zobacz [dokumentacji proste iniektor](https://simpleinjector.readthedocs.io/en/latest/index.html) i [repozytorium GitHub usługi Simple iniektor](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="e2c4e-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="e2c4e-116">IMiddlewareFactory</span></span>

<span data-ttu-id="e2c4e-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) udostępnia metody do tworzenia oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span>

<span data-ttu-id="e2c4e-118">W przykładowej aplikacji fabrykę oprogramowanie pośredniczące jest implementowane w celu tworzenia `SimpleInjectorActivatedMiddleware` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="e2c4e-119">Fabryka oprogramowania pośredniczącego używa iniektor prosty kontener, aby rozwiązać oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="e2c4e-120">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="e2c4e-120">IMiddleware</span></span>

<span data-ttu-id="e2c4e-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definiuje oprogramowanie pośredniczące do potoku żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="e2c4e-122">Oprogramowanie pośredniczące aktywowany przez `IMiddlewareFactory` implementacji (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span><span class="sxs-lookup"><span data-stu-id="e2c4e-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="e2c4e-123">Rozszerzenie jest tworzona dla oprogramowania pośredniczącego (*Middleware/MiddlewareExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="e2c4e-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="e2c4e-124">`Startup.ConfigureServices` należy wykonać kilka zadań:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="e2c4e-125">Skonfiguruj kontener iniektor proste.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="e2c4e-126">Zarejestruj fabryki i oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="e2c4e-127">Udostępnia kontekst bazy danych aplikacji z kontenera proste iniektor strony Razor.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-127">Make the app's database context available from the Simple Injector container for a Razor Page.</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="e2c4e-128">Oprogramowanie pośredniczące jest zarejestrowany w potoku przetwarzania żądań w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a><span data-ttu-id="e2c4e-129">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e2c4e-129">Additional resources</span></span>

* [<span data-ttu-id="e2c4e-130">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="e2c4e-130">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="e2c4e-131">Oprogramowanie pośredniczące oparte na fabryce aktywacji</span><span class="sxs-lookup"><span data-stu-id="e2c4e-131">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="e2c4e-132">Proste repozytorium GitHub iniektor</span><span class="sxs-lookup"><span data-stu-id="e2c4e-132">Simple Injector GitHub repository</span></span>](https://github.com/simpleinjector/SimpleInjector)
* [<span data-ttu-id="e2c4e-133">Proste iniektor dokumentacji</span><span class="sxs-lookup"><span data-stu-id="e2c4e-133">Simple Injector documentation</span></span>](https://simpleinjector.readthedocs.io/en/latest/index.html)
