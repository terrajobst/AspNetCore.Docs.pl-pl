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
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>Oprogramowanie pośredniczące aktywacji z kontenerem innych firm w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

W tym artykule przedstawiono sposób użycia <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> i <xref:Microsoft.AspNetCore.Http.IMiddleware> jako punkt rozszerzalności dla [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) aktywacji z kontenerem innej firmy. Aby uzyskać wprowadzające informacje na `IMiddlewareFactory` i `IMiddleware`, zobacz <xref:fundamentals/middleware/extensibility>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykładowa aplikacja pokazuje, oprogramowanie pośredniczące aktywacji przez `IMiddlewareFactory` implementacji `SimpleInjectorMiddlewareFactory`. W przykładzie użyto [proste iniektor](https://simpleinjector.org) kontenera (DI) iniekcji zależności.

Przykład oprogramowania pośredniczącego implementacji rejestruje wartość dostarczona przez parametr ciągu zapytania (`key`). Oprogramowanie pośredniczące używa kontekstu wprowadzonego bazy danych (usługi o określonym zakresie) do rejestrowania wartości ciągu zapytania w bazie danych w pamięci.

> [!NOTE]
> Ta aplikacja używa przykładowych [proste iniektor](https://github.com/simpleinjector/SimpleInjector) wyłącznie w celach demonstracyjnych. Korzystanie z prostych iniektor nie potwierdzenia. Metody aktywacji oprogramowania pośredniczącego opisanej w dokumentacji iniektor proste i problemy usługi GitHub są zalecane przez maintainers z prostego iniektor. Aby uzyskać więcej informacji, zobacz [dokumentacji proste iniektor](https://simpleinjector.readthedocs.io/en/latest/index.html) i [repozytorium GitHub usługi Simple iniektor](https://github.com/simpleinjector/SimpleInjector).

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> udostępnia metody do tworzenia oprogramowania pośredniczącego.

W przykładowej aplikacji fabrykę oprogramowanie pośredniczące jest implementowane w celu tworzenia `SimpleInjectorActivatedMiddleware` wystąpienia. Fabryka oprogramowania pośredniczącego używa iniektor prosty kontener, aby rozwiązać oprogramowania pośredniczącego:

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>IMiddleware

<xref:Microsoft.AspNetCore.Http.IMiddleware> Definiuje oprogramowanie pośredniczące do potoku żądania aplikacji.

Oprogramowanie pośredniczące aktywowany przez `IMiddlewareFactory` implementacji (*Middleware/SimpleInjectorActivatedMiddleware.cs*):

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

Rozszerzenie jest tworzona dla oprogramowania pośredniczącego (*Middleware/MiddlewareExtensions.cs*):

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` należy wykonać kilka zadań:

* Skonfiguruj kontener iniektor proste.
* Zarejestruj fabryki i oprogramowanie pośredniczące.
* Udostępnia kontekst bazy danych aplikacji z kontenera iniektor proste.

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Oprogramowanie pośredniczące jest zarejestrowany w potoku przetwarzania żądań w `Startup.Configure`:

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Oprogramowanie pośredniczące oparte na fabryce aktywacji](xref:fundamentals/middleware/extensibility)
* [Proste repozytorium GitHub iniektor](https://github.com/simpleinjector/SimpleInjector)
* [Proste iniektor dokumentacji](https://simpleinjector.readthedocs.io/en/latest/index.html)
