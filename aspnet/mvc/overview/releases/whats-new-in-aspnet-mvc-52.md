---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: Co to jest nowe w programie ASP.NET MVC 5.2 | Dokumentacja firmy Microsoft
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-52"></a>Co to jest nowe w programie ASP.NET MVC 5.2
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano nowe dla platformy ASP.NET MVC 5.2, [Microsoft.AspNet.MVC 5.2.2](#52) i [ASP.NET MVC 5.2.3 w wersji Beta](#mvc523Beta)

- [Wymagania dotyczące oprogramowania](#softRequire)
- [Pobieranie](#download)
- [Dokumentacja](#documentation)
- [Nowe funkcje w programie ASP.NET MVC 5.2](#new-features)

    - [Atrybut ulepszenia routingu](#attributerouting)
- [Znane problemy i fundamentalne zmiany](#knownbreakingchanges)
- [Poprawki błędów](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Wymagania programowe

- Program Visual Studio 2012: Pobierz [ASP.NET i sieć Web narzędzi 2013.1 dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Program Visual Studio 2013: Pobierz [programu Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) lub nowszej. Ta aktualizacja jest potrzebne do edycji widoki ASP.NET MVC 5.2 Razor.

<a id="download"></a>
## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakietów NuGet w galerii NuGet. Wykonaj wszystkie pakiety środowiska uruchomieniowego [Wersjonowania semantycznego](http://semver.org/) specyfikacji. Najnowszy pakiet ASP.NET MVC 5.2 ma następującą wersję: "5.2.0". Można zainstalować lub zaktualizować te pakiety za pośrednictwem [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Wydanie obejmuje również odpowiedniego zlokalizowane pakiety na NuGet.

Można zainstalować lub zaktualizować do pakietów NuGet wydanych przy użyciu konsoli Menedżera pakietów NuGet:

Instalacja pakietu Microsoft.AspNet.Mvc — wersja 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje na temat platformy ASP.NET MVC 5.2 są dostępne w witrynie sieci web platformy ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Nowe funkcje w programie ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Atrybut ulepszenia routingu

Atrybut routingu teraz udostępnia punkt rozszerzalności o nazwie IDirectRouteProvider, który umożliwia pełną kontrolę nad jak tras atrybutów są odnajdywane i skonfigurowane. IDirectRouteProvider jest odpowiedzialny za zapewnienie listę akcji i kontrolerów wraz z informacjami skojarzone trasy do określenia dokładnie konfiguracji routingu jest pożądany dla tych czynności. Implementacja IDirectRouteProvider można określić podczas wywoływania metody MapAttributes/MapHttpAttributeRoutes.

Dostosowywanie IDirectRouteProvider będzie najprostszym rozszerzając naszych Domyślna implementacja DefaultDirectRouteProvider. Ta klasa zawiera osobne wirtualnego metod, które można zmienić logikę wykrywania atrybuty, tworzenie wejścia dla trasy i odnajdywanie prefiks trasy i prefiks obszaru.

Z nowy atrybut routingu możliwość rozszerzania IDirectRouteProvider użytkownik może wykonać następujące czynności:

1. Obsługuje dziedziczenia tras atrybutów. Na przykład w poniższym scenariuszu kontrolery blogu i magazynu są przy użyciu konwencji tras atrybutu, która jest definiowana za pomocą BaseController. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Automatyczne generowanie nazw trasy dla tras atrybutów. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Zmodyfikuj prefiksy trasy w jednym centralnym miejscu przed trasy zostaną dodane do tabeli tras.
4. Odfiltrować kontrolery, które mają atrybut routingu do wyszukania. Mamy nadzieję, blogu na 3 i 4 wkrótce.

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook poprawki dla zmienione powierzchni interfejsu API

Pakiet MVC Facebook [zostało przerwane](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) z powodu zmiany kilka interfejsu API w serwisie Facebook. Również udostępnimy nowy pakiet Facebook (Microsoft.AspNet.Facebook 1.0.0) Aby rozwiązać te problemy.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Funkcja szkieletów MVC/interfejs API sieci Web do projektu z 5.2.0 pakietów pakiety powoduje 5.1.2 dla tych, które już nie istnieje w projekcie

Aktualizowanie pakietów NuGet dla platformy ASP.NET MVC 5.2.0 nie powoduje aktualizacji narzędzi programu Visual Studio, takich jak ASP.NET rusztowania lub szablonu projektu aplikacji sieci Web ASP.NET. Korzystają z poprzedniej wersji pakietów środowiska uruchomieniowego ASP.NET (np. 5.1.2 w wersji Update 2). W związku z tym szkieletów ASP.NET zainstaluje poprzedniej wersji wymaganych pakietów (np. 5.1.2 w wersji Update 2) Jeśli nie są one już dostępne w projektach. Jednak funkcja szkieletów ASP.NET w programie Visual Studio 2013 RTM lub Update 1 nie powoduje zastąpienia najnowszych pakietów w projektach. Jeśli używasz szkieletów ASP.NET po zaktualizowaniu pakietów projektów sieci Web interfejsu API w wersji 2.2 lub ASP.NET MVC 5.2, upewnij się, że wersje interfejsu API sieci Web i ASP.NET MVC są spójne.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Instalacja pakietu Microsoft.jQuery.Unobtrusive.Validation NuGet nie powiedzie się, ponieważ nie można odnaleźć wersji elementu Microsoft.jQuery.Unobtrusive.Validation zgodny z jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation wymaga jQuery &gt;= 1.8 i jQuery.Validation &gt;= 1.8. But,jQuery.Validation (1.8) wymaga jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). W związku z tym NuGet instaluje JQuery 1.8 i jQuery.Validation 1.8 w tym samym czasie jej nie powiedzie się. Gdy pojawi się ten problem, należy po prostu zaktualizuj wersję jQuery.Validation do &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) mającego zakończenia jQuery stałej, powinno być możliwe do zainstalowania Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery. Wersja pakietu nuget weryfikacji 1.13.0 nie może rozpoznać niektórych adresów e-mail międzynarodowych

Wersja pakietu nuget jQuery.Validation 1.11.1 jest ostatnią znaną wersję, która rozpoznaje następujących prawidłowych adresów e-mail. Wszystkie nowsze wersje nie można rozpoznać ich. Na przykład:

Standard adres przystosowywanie do warunków międzynarodowych (EAI) wiadomości e-Mail (np. [&#29992; &#25143;@domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + Internationalized identyfikatory zasobów (IRIs) (np., [&#29992; &#25143; @&#1076; &#1086; &#1084; &#1077; &#1085;. &#1088; &#1092; ](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Problem został zgłoszony w [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Wyróżnianie składni Razor widoków w programie Visual Studio 2013

Po aktualizacji platformy ASP.NET MVC 5.2 bez aktualizacji programu Visual Studio 2013, nie zapewni obsługę edytora programu Visual Studio dla wyróżnianie składni podczas edycji widokami Razor. Należy zaktualizować programu Visual Studio 2013, aby uzyskać tę obsługę.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Poprawki i aktualizacje funkcji pomocniczych

Ta wersja zawiera również kilka poprawek usterek i funkcji pomocniczych aktualizacji. Pełną listę można znaleźć:

- [5.2 pakietu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

Ta wersja nie ma żadnych nowych funkcji lub poprawki na platformie MVC. Wprowadziliśmy [zmiany na stronach sieci Web](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) poprawy wydajności znaczące, a następnie zostały zaktualizowane innych pakietów zależnych możemy właścicielem do zależą od nowa wersja stron sieci Web.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 w wersji Beta

Informacje o wersji [tutaj](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Ta wersja zawiera tylko poprawki błędów. Można użyć [to zapytanie](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) lista problemy rozwiązane w tej wersji.
