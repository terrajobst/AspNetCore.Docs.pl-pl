---
title: "Aktywacji opartej na fabryki oprogramowania pośredniczącego z kontenerem innych firm w ASP.NET Core"
author: guardrex
description: "Dowiedz się, jak używać jednoznacznie oprogramowania pośredniczącego z aktywacją opartą na fabryki i kontener innych firm w ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 02/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: bb318747e254ac244facc1fe1ff08a1f5c4727f2
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="factory-based-middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="1d86c-103">Aktywacji opartej na fabryki oprogramowania pośredniczącego z kontenerem innych firm w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d86c-103">Factory-based middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="1d86c-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1d86c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1d86c-105">W tym artykule przedstawiono sposób użycia [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) i [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) jako punkt rozszerzeń dla [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) aktywacji z kontenerem innych firm.</span><span class="sxs-lookup"><span data-stu-id="1d86c-105">This article demonstrates how to use [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) and [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="1d86c-106">Podstawowe informacje na `IMiddlewareFactory` i `IMiddleware`, zobacz [aktywacji opartej na fabryki oprogramowanie pośredniczące](xref:fundamentals/middleware/extensibility) tematu.</span><span class="sxs-lookup"><span data-stu-id="1d86c-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see the [Factory-based middleware activation](xref:fundamentals/middleware/extensibility) topic.</span></span>

<span data-ttu-id="1d86c-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1d86c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1d86c-108">Oprogramowanie pośredniczące aktywacji przez przedstawiono przykładową aplikację `IMiddlewareFactory` implementacji `SimpleInjectorMiddlewareFactory`.</span><span class="sxs-lookup"><span data-stu-id="1d86c-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="1d86c-109">W przykładzie użyto [proste iniektor](https://simpleinjector.org) kontenera (Podpisane) iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="1d86c-109">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="1d86c-110">Podana wartość parametru ciągu zapytania rejestruje implementacji oprogramowania pośredniczącego w próbce (`key`).</span><span class="sxs-lookup"><span data-stu-id="1d86c-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="1d86c-111">Oprogramowanie pośredniczące używa kontekstu wprowadzony bazy danych (usługa zakresie) do rejestrowania wartość ciągu kwerendy w bazie danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="1d86c-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="1d86c-112">Przykładowe zastosowania aplikacji [proste iniektor](https://github.com/simpleinjector/SimpleInjector) wyłącznie w celach demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="1d86c-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="1d86c-113">Użyj prostego iniektor nie potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="1d86c-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="1d86c-114">Oprogramowanie pośredniczące metod aktywacji opisanej w dokumentacji iniektor proste i GitHub problemów są zalecane przez maintainers z prostego iniektor.</span><span class="sxs-lookup"><span data-stu-id="1d86c-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="1d86c-115">Aby uzyskać więcej informacji, zobacz [dokumentacji proste iniektor](https://simpleinjector.readthedocs.io/en/latest/index.html) i [repozytorium GitHub iniektor proste](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="1d86c-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="1d86c-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="1d86c-116">IMiddlewareFactory</span></span>

<span data-ttu-id="1d86c-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) udostępnia metody do tworzenia oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="1d86c-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span>

<span data-ttu-id="1d86c-118">W przykładowej aplikacji, fabryki oprogramowanie pośredniczące zaimplementowano utworzyć `SimpleInjectorActivatedMiddleware` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1d86c-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="1d86c-119">Fabryka oprogramowanie pośredniczące używa kontenera proste iniektor rozwiązać oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="1d86c-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="1d86c-120">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="1d86c-120">IMiddleware</span></span>

<span data-ttu-id="1d86c-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definiuje oprogramowanie pośredniczące do potoku żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1d86c-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="1d86c-122">Oprogramowanie pośredniczące aktywowany przez `IMiddlewareFactory` implementacji (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span><span class="sxs-lookup"><span data-stu-id="1d86c-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="1d86c-123">Rozszerzenie jest tworzona dla oprogramowania pośredniczącego (*Middleware/MiddlewareExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1d86c-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="1d86c-124">`Startup.ConfigureServices` trzeba wykonać kilka zadań:</span><span class="sxs-lookup"><span data-stu-id="1d86c-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="1d86c-125">Skonfiguruj kontener iniektor proste.</span><span class="sxs-lookup"><span data-stu-id="1d86c-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="1d86c-126">Zarejestruj fabrykę i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="1d86c-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="1d86c-127">Udostępnia kontekst bazy danych aplikacji z kontenera proste iniektor dla stron Razor.</span><span class="sxs-lookup"><span data-stu-id="1d86c-127">Make the app's database context available from the Simple Injector container for a Razor Page.</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="1d86c-128">Oprogramowanie pośredniczące jest zarejestrowany w potoku przetwarzania żądań w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1d86c-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=12)]

## <a name="additional-resources"></a><span data-ttu-id="1d86c-129">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1d86c-129">Additional resources</span></span>

* [<span data-ttu-id="1d86c-130">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="1d86c-130">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="1d86c-131">Aktywacji opartej na fabryki oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="1d86c-131">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="1d86c-132">Proste repozytorium GitHub iniektor</span><span class="sxs-lookup"><span data-stu-id="1d86c-132">Simple Injector GitHub repository</span></span>](https://github.com/simpleinjector/SimpleInjector)
* [<span data-ttu-id="1d86c-133">Proste iniektor dokumentacji</span><span class="sxs-lookup"><span data-stu-id="1d86c-133">Simple Injector documentation</span></span>](https://simpleinjector.readthedocs.io/en/latest/index.html)
