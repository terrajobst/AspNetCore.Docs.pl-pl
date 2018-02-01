---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: What's New in ASP.NET Web API 2.1 | Dokumentacja firmy Microsoft
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a>What's New in ASP.NET Web API 2.1
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano, jakie nowości 2.1 interfejsu API sieci Web ASP.NET.

- [Pobieranie](#download)
- [Dokumentacja](#documentation)
- [Nowe funkcje w składniku ASP.NET Web API 2.1](#new-features)

    - [Obsługa błędów globalne](#global-error)
    - [Atrybut ulepszenia routingu](#attribute-routing)
    - [Ulepszenia strona pomocy](#help-page)
    - [Obsługa IgnoreRoute](#ignoreroute)
    - [Program formatujący typ nośnika BSON](#bson)
    - [Lepszą obsługę filtry Async](#async-filters)
    - [Analiza kodu dla klienta formatowania biblioteki zapytania](#query-parsing)
- [Znane problemy i fundamentalne zmiany](#known-issues)
- [Poprawki błędów](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakietów NuGet w galerii NuGet. Wykonaj wszystkie pakiety środowiska uruchomieniowego [Wersjonowania semantycznego](http://semver.org/) specyfikacji. Najnowszy pakiet ASP.NET Web API 2.1 RTM ma następującą wersję: "5.1.2". Można zainstalować lub zaktualizować te pakiety za pośrednictwem [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Wydanie obejmuje również odpowiedniego zlokalizowane pakiety na NuGet.

Można zainstalować lub zaktualizować do pakietów NuGet wydanych przy użyciu konsoli Menedżera pakietów NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje na temat programu ASP.NET Web API 2.1 RTM są dostępne w witrynie sieci web platformy ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Nowe funkcje w składniku ASP.NET Web API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Obsługa błędów globalne

Wszystkie nieobsługiwane wyjątki teraz mogą być rejestrowane za pośrednictwem jednego mechanizmu centralnej i można dostosować zachowanie nieobsługiwanych wyjątków.

Platforma obsługuje wiele rejestratorów wyjątków, które można znaleźć w nieobsługiwany wyjątek i informacje o kontekście, w którym wystąpił, takich jak żądanie przetwarzane, w tym czasie.

Na przykład w poniższym kodzie użyto System.Diagnostics.TraceSource rejestrowania wszystkich nieobsługiwanych wyjątków:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Można również zastąpić domyślny program obsługi wyjątku, dzięki czemu można całkowicie dostosować komunikat odpowiedzi HTTP, który jest wysyłany, gdy wystąpił nieobsługiwany wyjątek występuje.

Firma Microsoft umieściła [próbki](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) który rejestruje wszystkie nieobsługiwane wyjątki za pomocą popularnych framework ELMAH.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Atrybut ulepszenia routingu

Atrybut routingu teraz obsługuje ograniczenia, włączanie wersji i wybór tras oparte na protokole nagłówka. Ponadto wiele aspektów tras atrybutów są teraz można dostosować za pomocą **IDirectRouteFactory** interfejsu i **RouteFactoryAttribute** klasy. Prefiks trasy jest teraz rozszerzalny za pośrednictwem **IRoutePrefix** interfejsu i **RoutePrefixAttribute** klasy.

Firma Microsoft umieściła [próbki](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) używającą ograniczenia dynamicznie filtrowania kontrolerów nagłówka HTTP "api-version".

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Ulepszenia strona pomocy

Składnik Web API 2.1 zawiera następujące rozszerzenia [strony pomocy interfejsu API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Dokumentacja poszczególnych właściwości parametrów lub zwracanych typów akcji.
- Dokumentacja adnotacji modelu danych.

Projekt interfejsu użytkownika strony pomocy Zaktualizowano również, aby uwzględnić te zmiany.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Obsługa IgnoreRoute

Sieci Web obsługuje interfejs API 2.1 ignorowanie wzorców adresów URL w składniku Web API routingu, za pomocą zestawu **IgnoreRoute** metody rozszerzenia na **HttpRouteCollection**. Te metody spowodować interfejsu API sieci Web zignorować wszelkie adresy URL, które pasują do określonego szablonu i umożliwić hosta do zastosowania w razie potrzeby dodatkowego przetwarzania.

Poniższy przykład powoduje ignorowanie identyfikatory URI, które zaczynają się &quot;zawartości&quot; segmentu:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Program formatujący typ nośnika BSON

Sieci Web obsługuje teraz interfejsu API [BSON](http://bsonspec.org/) format danych przesyłanych w sieci, zarówno na kliencie, jak i na serwerze.

Aby włączyć BSON po stronie serwera, Dodaj **BsonMediaTypeFormatter** do kolekcji elementów formatujących:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Oto, jak klienta .NET mogą korzystać z formatu BSON:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Firma Microsoft umieściła [próbki](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) którym wyświetlana po stronie klienta i serwera.

Aby uzyskać więcej informacji, zobacz [wsparcia formatu BSON 2.1 interfejsu API sieci Web](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Lepszą obsługę filtry Async

Interfejs API sieci Web obsługuje teraz łatwo tworzyć filtry, które są wykonywane asynchronicznie. Ta funkcja jest przydatna jest filtr potrzebnych do wykonania akcji asynchronicznej, takich jak dostęp w bazie danych. Wcześniej Aby utworzyć filtr asynchroniczne, trzeba było implementować interfejs filtru samodzielnie, ponieważ klas podstawowych filtru dostępne tylko metod synchronicznych. Teraz można zastąpić wirtualnego `On*Async` klasy podstawowej metody filtru.

Na przykład:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**, **ActionFilterAttribute**, i **ExceptionFilterAttribute** wszystkie klasy obsługuje asynchroniczne 2.1 interfejsu API sieci Web.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Analiza kodu dla klienta formatowania biblioteki zapytania

Wcześniej **System.Net.Http.Formatting** obsługiwane podczas analizowania i aktualizowanie zapytania identyfikatora URI dla kodu po stronie serwera, ale równoważne przenośnej biblioteki brakowało tej funkcji. W sieci Web interfejsu API 2.1 aplikacja kliencka można teraz łatwo przeanalizować i aktualizować ciąg zapytania.

Poniższe przykłady przedstawiają sposób przeanalizować, modyfikowania i generowania zapytania identyfikatora URI. (W przykładach przedstawiono aplikację konsolową dla uproszczenia).

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

W tej sekcji opisano znane problemy i fundamentalne zmiany w RTM 2.1 interfejsu API sieci Web ASP.NET.

### <a name="attribute-routing"></a>Atrybut routingu

Niejednoznaczności w dopasowań routingu atrybutu teraz raport błędu zamiast wybranie pierwszego dopasowania.

Tras atrybutów mogą używać tychże *{controller}* parametru i korzystania z *{action}* parametru na trasach dotyczącymi akcji. Te parametry najprawdopodobniej spowoduje, że niejednoznaczności.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Funkcja szkieletów MVC/interfejs API sieci Web do projektu z 5.1 wynikiem pakiety w wersji 5.0 pakiety dla tych, które już nie istnieje w projekcie

Aktualizowanie pakietów NuGet dla programu ASP.NET Web API 2.1 RTM nie powoduje aktualizacji Visual Studio tools, takich jak ASP.NET rusztowania lub szablonu projektu aplikacji sieci Web ASP.NET. Korzystają z poprzedniej wersji pakietów środowiska uruchomieniowego ASP.NET (5.0.0.0). W związku z tym szkieletów ASP.NET zainstaluje poprzedniej wersji wymaganych pakietów (5.0.0.0), jeśli nie są one już dostępne w projektach. Jednak funkcja szkieletów ASP.NET w programie Visual Studio 2013 RTM lub Update 1 nie powoduje zastąpienia najnowszych pakietów w projektach.

Użycie programu ASP.NET szkieletów po zaktualizowaniu pakietów 2.1 interfejsu API sieci Web lub ASP.NET MVC w wersji 5.1, upewnij się, że wersje interfejsu API sieci Web i MVC są spójne.

### <a name="type-renames"></a>Zmienia nazwę typu

Niektóre typy używane dla routingu rozszerzalności atrybutu nazwy zostały zmienione z wersji RC do wersji 2.1 RTM.

| Stara nazwa typu (2.1 RC) | Nowy typ Name (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Filtry wyjątków nie dekodowania agregacji wyjątki zgłaszane w asynchronicznych akcji

Wcześniej Jeśli operacji asynchronicznej zwrócił **AggregateException**, filtra wyjątku czy dekodowania wyjątku, i **OnException** otrzyma podstawowego wyjątku. 2.1, filtru wyjątków nie Cofnij zawijanie, i **OnException** pobiera oryginalny **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Poprawki błędów

Ta wersja zawiera również kilka poprawek. Pełną listę można znaleźć:

- [5.1.0 pakiet](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pakietu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 pakiet zawiera aktualizacje funkcji IntelliSense, ale nie poprawki.
