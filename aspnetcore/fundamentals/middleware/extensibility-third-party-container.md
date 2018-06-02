---
title: Oprogramowanie pośredniczące aktywacji z kontenerem innych firm w ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać jednoznacznie oprogramowania pośredniczącego z aktywacją opartą na fabryki i kontener innych firm w ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 02/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: c55075fd3c6fda4073d26925eab823c35d8656f5
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729895"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>Oprogramowanie pośredniczące aktywacji z kontenerem innych firm w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

W tym artykule przedstawiono sposób użycia [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) i [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) jako punkt rozszerzeń dla [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) aktywacji z kontenerem innych firm. Podstawowe informacje na `IMiddlewareFactory` i `IMiddleware`, zobacz [aktywacji opartej na fabryki oprogramowanie pośredniczące](xref:fundamentals/middleware/extensibility) tematu.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Oprogramowanie pośredniczące aktywacji przez przedstawiono przykładową aplikację `IMiddlewareFactory` implementacji `SimpleInjectorMiddlewareFactory`. W przykładzie użyto [proste iniektor](https://simpleinjector.org) kontenera (Podpisane) iniekcji zależności.

Podana wartość parametru ciągu zapytania rejestruje implementacji oprogramowania pośredniczącego w próbce (`key`). Oprogramowanie pośredniczące używa kontekstu wprowadzony bazy danych (usługa zakresie) do rejestrowania wartość ciągu kwerendy w bazie danych w pamięci.

> [!NOTE]
> Przykładowe zastosowania aplikacji [proste iniektor](https://github.com/simpleinjector/SimpleInjector) wyłącznie w celach demonstracyjnych. Użyj prostego iniektor nie potwierdzenia. Oprogramowanie pośredniczące metod aktywacji opisanej w dokumentacji iniektor proste i GitHub problemów są zalecane przez maintainers z prostego iniektor. Aby uzyskać więcej informacji, zobacz [dokumentacji proste iniektor](https://simpleinjector.readthedocs.io/en/latest/index.html) i [repozytorium GitHub iniektor proste](https://github.com/simpleinjector/SimpleInjector).

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) udostępnia metody do tworzenia oprogramowania pośredniczącego.

W przykładowej aplikacji, fabryki oprogramowanie pośredniczące zaimplementowano utworzyć `SimpleInjectorActivatedMiddleware` wystąpienia. Fabryka oprogramowanie pośredniczące używa kontenera proste iniektor rozwiązać oprogramowanie pośredniczące:

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definiuje oprogramowanie pośredniczące do potoku żądania aplikacji.

Oprogramowanie pośredniczące aktywowany przez `IMiddlewareFactory` implementacji (*Middleware/SimpleInjectorActivatedMiddleware.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

Rozszerzenie jest tworzona dla oprogramowania pośredniczącego (*Middleware/MiddlewareExtensions.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` trzeba wykonać kilka zadań:

* Skonfiguruj kontener iniektor proste.
* Zarejestruj fabrykę i oprogramowania pośredniczącego.
* Udostępnia kontekst bazy danych aplikacji z kontenera proste iniektor dla stron Razor.

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

Oprogramowanie pośredniczące jest zarejestrowany w potoku przetwarzania żądań w `Startup.Configure`:

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Aktywacji opartej na fabryki oprogramowania pośredniczącego](xref:fundamentals/middleware/extensibility)
* [Proste repozytorium GitHub iniektor](https://github.com/simpleinjector/SimpleInjector)
* [Proste iniektor dokumentacji](https://simpleinjector.readthedocs.io/en/latest/index.html)
