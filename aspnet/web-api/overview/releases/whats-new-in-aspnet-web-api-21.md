---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Co nowego we wzorcu ASP.NET Web API 2.1 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 8e0501570e6dc6a9a6f69a642f9ab031c5497b5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385703"
---
<a name="whats-new-in-aspnet-web-api-21"></a>Co nowego we wzorcu ASP.NET Web API 2.1
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano what's new for ASP.NET Web API 2.1.

- [Pobieranie](#download)
- [Dokumentacja](#documentation)
- [Nowe funkcje we wzorcu ASP.NET Web API 2.1](#new-features)

    - [Obsługa błędów globalne](#global-error)
    - [Atrybut ulepszenia routingu](#attribute-routing)
    - [Ulepszenia strony pomocy](#help-page)
    - [Obsługa IgnoreRoute](#ignoreroute)
    - [Element formatujący typu nośnika BSON](#bson)
    - [Lepsza obsługa Async filtry](#async-filters)
    - [Zapytanie analizy dla klienta, formatowanie biblioteki](#query-parsing)
- [Znane problemy i fundamentalne zmiany](#known-issues)
- [Poprawki błędów](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakiety NuGet w galerii NuGet. Wykonaj wszystkie pakiety środowiska uruchomieniowego [Semantic Versioning](http://semver.org/) specyfikacji. Najnowszy pakiet ASP.NET Web API 2.1 RTM ma następującą wersję: "5.1.2". Możesz zainstalować lub zaktualizować te pakiety przy użyciu [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Wersja ta oferuje również odpowiedni zlokalizowanych pakietów dla narzędzia NuGet.

Możesz zainstalować lub zaktualizować do wydanych pakietów NuGet za pomocą konsoli Menedżera pakietów NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

W samouczkach i innych informacji na temat platformy ASP.NET Web API 2.1 RTM są dostępne w witrynie sieci web platformy ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Nowe funkcje we wzorcu ASP.NET Web API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Obsługa błędów globalne

Wszystkie nieobsłużone wyjątki, teraz mogą być rejestrowane za pośrednictwem jednego mechanizmu centralnej i można dostosować zachowanie dla nieobsłużonych wyjątków.

Platforma obsługuje wiele rejestratorów wyjątków, które Zobacz nieobsługiwany wyjątek i informacje o kontekście, w którym się pojawił, takie jak żądanie przetwarzane, w tym czasie.

Na przykład w poniższym kodzie użyto System.Diagnostics.TraceSource, aby rejestrować wszystkie nieobsłużone wyjątki:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Może również zastąpić domyślny program obsługi wyjątków, tak aby było w pełni dostosować komunikat odpowiedzi HTTP, która jest wysyłana, gdy wystąpił nieobsługiwany wyjątek występuje.

Udostępniliśmy [przykładowe](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) która rejestruje wszystkie nieobsłużone wyjątki przy użyciu popularnego środowiska ELMAH.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Atrybut ulepszenia routingu

Teraz trasowanie atrybutów obsługuje ograniczenia, umożliwiające wybór opartej na nagłówkach trasy i przechowywania wersji. Ponadto wiele aspektów tras atrybutów są teraz można dostosowywać za pomocą **IDirectRouteFactory** interfejsu i **RouteFactoryAttribute** klasy. Prefiks trasy jest teraz rozszerzalny za pośrednictwem **IRoutePrefix** interfejsu i **RoutePrefixAttribute** klasy.

Udostępniliśmy [przykładowe](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) używającej ograniczenia filtrującą dane dynamiczne kontrolerów według nagłówek HTTP "api-version".

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Ulepszenia strony pomocy

Składnik Web API 2.1 zawiera następujące rozszerzenia [strony pomocy dla interfejsu API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Dokumentacja poszczególnych właściwości parametry lub zwracane typy akcji.
- Dokumentacja adnotacje modelu danych.

Projekt interfejsu użytkownika strony pomocy również został zaktualizowany, aby uwzględnić te zmiany.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Obsługa IgnoreRoute

Web API 2.1 obsługuje ignorowanie wzorce adresów URL w routingu internetowego interfejsu API, za pomocą zestawu **IgnoreRoute** metod rozszerzenia **HttpRouteCollection**. Te metody powodują interfejsu API sieci Web ignorować wszystkie adresy URL, które pasują do określonego szablonu i zezwala na hostów w celu zastosowania dodatkowego przetwarzania, jeśli to stosowne.

Poniższy przykład powoduje ignorowanie identyfikatory URI, które zaczynają się &quot;zawartości&quot; segmencie:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Element formatujący typu nośnika BSON

Sieci Web interfejs API obsługuje teraz [BSON](http://bsonspec.org/) formatu, zarówno na kliencie, jak i na serwerze.

Aby włączyć BSON po stronie serwera, Dodaj **BsonMediaTypeFormatter** do kolekcji elementów formatujących:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Poniżej przedstawiono, jak klienta platformy .NET mogą używać formatu BSON:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Udostępniliśmy [przykładowe](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) pokazujący po stronie klienta i serwera.

Aby uzyskać więcej informacji, zobacz [Obsługa formatu BSON w sieci Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Lepsza obsługa Async filtry

Internetowy interfejs API obsługuje teraz łatwy sposób tworzenia filtrów, które są wykonywane asynchronicznie. Ta funkcja jest przydatna jest filtr potrzebuje do wykonywania akcji asynchronicznych, takich jak dostęp w bazie danych. Wcześniej Aby utworzyć filtr async, trzeba było implementacji interfejsu filtru, samodzielnie, ponieważ klas bazowych filtr dostępne tylko synchroniczne metody. Teraz możesz zastąpić wirtualnego `On*Async` metody filtr klasy bazowej.

Na przykład:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**, **ActionFilterAttribute**, i **ExceptionFilterAttribute** wszystkie klasy obsługuje asynchroniczne w sieci Web API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Zapytanie analizy dla klienta, formatowanie biblioteki

Wcześniej **System.Net.Http.Formatting** obsługiwane podczas analizowania i aktualizowanie zapytania identyfikatora URI dla kodu po stronie serwera, ale równoważne biblioteki przenośnej brakowało tej funkcji. W sieci Web API 2.1 aplikacja kliencka można teraz łatwo analizować i aktualizować ciągu zapytania.

Poniższe przykłady pokazują, jak analizować, modyfikować i wygenerować zapytania identyfikatora URI. (W przykładach przedstawiono aplikację konsolową w języku dla uproszczenia).

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

W tej sekcji opisano znane problemy i przełomowe zmiany w ASP.NET Web API 2.1 RTM.

### <a name="attribute-routing"></a>Routing atrybutów

Niejasności w atrybucie dopasowania routingu teraz zgłosić błąd, a nie wybierając pierwsze dopasowanie.

Tras atrybutów mogą używać tychże *{controller}* parametru i przy użyciu *{action}* parametru na trasach umieścić w akcji. Te parametry bardzo prawdopodobne spowodowałby niejednoznaczności.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Tworzenie szkieletów MVC/interfejsu API sieci Web do projektu przy użyciu 5.1 pakietów skutkuje 5.0 pakietów dla tych, które jeszcze nie istnieją w projekcie

Trwa aktualizowanie pakietów NuGet dla platformy ASP.NET Web API 2.1 RTM nie powoduje aktualizacji narzędzi programu Visual Studio, takich jak funkcja tworzenia szkieletu ASP.NET lub szablon projektu aplikacji sieci Web ASP.NET. Używają poprzedniej wersji pakiety środowiska uruchomieniowego platformy ASP.NET (5.0.0.0). Co w efekcie funkcja tworzenia szkieletu ASP.NET zainstaluje starszą wersję (5.0.0.0) wymagane pakiety, jeśli nie są jeszcze dostępne w projektach. Funkcja tworzenia szkieletu ASP.NET w Visual Studio 2013 RTM i Update 1 nie zastąpi jednak najnowszych pakietów w swoich projektach.

Jeśli używasz funkcja tworzenia szkieletu ASP.NET po zaktualizowaniu pakietów do sieci Web API 2.1 lub platformy ASP.NET MVC 5.1, upewnij się, że wersje interfejsu API sieci Web i MVC są spójne.

### <a name="type-renames"></a>Zmienia nazwę typu

Niektóre typy używane do rozszerzalności routingu atrybutu nazwy zostały zmienione z wersji RC do 2.1 RTM.

| Stara nazwa typu (2.1 RC) | Nowy typ nazwy (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Filtry wyjątków nie Odkodowywanie agregacji wyjątki zgłaszane w działaniach asynchronicznych

Wcześniej Jeśli akcję async spowodowała **aggregateexception —**, filtra wyjątku czy Odkodowywanie wyjątek, i **OnException** otrzymamy wyjątek podstawowy. 2.1, filtra wyjątku być nie Odkodowywanie, oraz **OnException** pobiera oryginalny **aggregateexception —**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Poprawki błędów

Ta wersja zawiera także kilka poprawek usterek. Pełną listę można znaleźć:

- [5.1.0 pakiet](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pakiet](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 pakiet zawiera aktualizacje funkcji IntelliSense, ale nie poprawki.
