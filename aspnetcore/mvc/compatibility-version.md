---
title: Zgodność wersji dla platformy ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak klasa startowa. w programie ASP.NET Core umożliwia skonfigurowanie usług i potok żądań aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2019
uid: mvc/compatibility-version
ms.openlocfilehash: 7c4189db435088e0803b35add82fa0eb9372e664
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410148"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>Zgodność wersji dla platformy ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Metoda <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> umożliwia aplikacji włączenie wykorzystania lub rezygnację ze zmian zachowania wprowadzanych w programie ASP.NET Core MVC w wersji 2.1 lub nowszej, które potencjalnie mogą prowadzić do awarii. Istotne zmiany zachowania w potencjalnie są zazwyczaj w jak działa w podsystemie MVC i jak **kodu** jest wywoływana w czasie wykonywania. Przez zgodzie na rozwiązanie, możesz korzystać z najnowszych zachowanie i długoterminowe zachowanie platformy ASP.NET Core.

Poniższy kod ustawia tryb zgodności do programu ASP.NET Core w wersji 2.2:

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

Zaleca się przetestowanie aplikacji przy użyciu najnowszej wersji (`CompatibilityVersion.Version_2_2`). Przewidujemy, że większość aplikacji nie będziesz mieć istotne zmiany zachowania przy użyciu najnowszej wersji.

Aplikacje, które wywołują `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` są chronione przed potencjalnie przełomowe zmiany zachowania wprowadzone w programie ASP.NET Core 2.1 MVC i nowszych wersji 2.x. Ta ochrona:

* Nie ma zastosowania do wszystkich zmian 2.1 i nowsze, jest on skierowany do potencjalnie przełomowe zmiany zachowania środowiska uruchomieniowego platformy ASP.NET Core w podsystemie MVC.
* Nie jest rozszerzana następnej wersji głównej.

Zgodność domyślny dla platformy ASP.NET Core 2.1 i nowsze aplikacje 2.x, które obsługują **nie** wywołania `SetCompatibilityVersion` jest zgodność 2.0. Oznacza to, nie wywołuje metody `SetCompatibilityVersion` jest taka sama jak wywołania `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Poniższy kod ustawia tryb zgodności ASP.NET Core 2.2, z wyjątkiem następujących problemów:

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

W przypadku aplikacji, wystąpić przełomowe zmiany zachowania, za pomocą przełączników odpowiednie zgodności:

* Umożliwia użyj najnowszej wersji i zrezygnować z specyficzne przełomowe zmiany zachowania.
* Umożliwia aktualizacji aplikacji, dzięki czemu działa z najnowszymi zmianami.

[MvcOptions](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Mvc/Mvc.Core/src/MvcOptions.cs) komentarze źródła klasy mają dobrej wyjaśnienie, co zmieniło i dlaczego zmiany są poprawę dla większości użytkowników.

W przyszłości, będzie istnieć [wersji platformy ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap). Starego zachowania obsługiwany przez przełączniki zgodności zostaną usunięte w wersji 3.0. Uważamy, że są to dodatnia zmian niemal wszystkich użytkowników korzystających. Wprowadzenie do tych zmian, większość aplikacji mogą teraz korzystać i innych będzie miał czas na aktualizację aplikacji.
