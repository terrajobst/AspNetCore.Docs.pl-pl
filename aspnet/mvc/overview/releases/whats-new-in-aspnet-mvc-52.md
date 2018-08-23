---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: Co nowego we wzorcu ASP.NET MVC 5.2 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: riande
ms.date: 12/25/2014
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 8c3c5de55396635d2e7f2b7726f54be1c06bb691
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752081"
---
<a name="whats-new-in-aspnet-mvc-52"></a>Co nowego we wzorcu ASP.NET MVC 5.2
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano nowości w programie ASP.NET MVC 5.2 [Microsoft.AspNet.MVC 5.2.2](#52) i [Beta programów ASP.NET MVC 5.2.3](#mvc523Beta)

- [Wymagania dotyczące oprogramowania](#softRequire)
- [Pobieranie](#download)
- [Dokumentacja](#documentation)
- [Nowe funkcje we wzorcu ASP.NET MVC 5.2](#new-features)

    - [Atrybut ulepszenia routingu](#attributerouting)
- [Znane problemy i fundamentalne zmiany](#knownbreakingchanges)
- [Poprawki błędów](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Wymagania programowe

- Program Visual Studio 2012: Pobierz [ASP.NET and Web Tools 2013.1 dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Program Visual Studio 2013: Pobierz [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) lub nowszej. Ta aktualizacja jest potrzebne do edycji widoki ASP.NET MVC 5.2 Razor.

<a id="download"></a>
## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakiety NuGet w galerii NuGet. Wykonaj wszystkie pakiety środowiska uruchomieniowego [Semantic Versioning](http://semver.org/) specyfikacji. Najnowszy pakiet platformy ASP.NET MVC 5.2 ma następującą wersję: "5.2.0". Możesz zainstalować lub zaktualizować te pakiety przy użyciu [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Wersja ta oferuje również odpowiedni zlokalizowanych pakietów dla narzędzia NuGet.

Możesz zainstalować lub zaktualizować do wydanych pakietów NuGet za pomocą konsoli Menedżera pakietów NuGet:

Install-Package Microsoft.AspNet.Mvc — wersja 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

W samouczkach i innych informacji na temat platformy ASP.NET MVC 5.2 są dostępne w witrynie sieci web platformy ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Nowe funkcje we wzorcu ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Atrybut ulepszenia routingu

Teraz trasowanie atrybutów udostępnia punkt rozszerzalności o nazwie IDirectRouteProvider, która umożliwia pełną kontrolę nad jak tras atrybutów są odnajdywane i skonfigurowane. IDirectRouteProvider jest odpowiedzialny za zapewnienie listę akcji i kontrolerów wraz z informacjami skojarzone trasy do określenia dokładnie wymaganego konfigurację routingu dla tych akcji. Implementacja IDirectRouteProvider może być określona podczas wywoływania MapAttributes/MapHttpAttributeRoutes.

Dostosowywanie IDirectRouteProvider będzie najprostszym, rozszerzając nasze Domyślna implementacja DefaultDirectRouteProvider. Ta klasa udostępnia oddzielne możliwym do zastąpienia metod wirtualnych, aby zmienić logikę odnajdywania atrybutów, tworząc wejścia dla trasy i odnajdywanie prefiks trasy i prefiks obszaru.

Przy użyciu nowego atrybutu routingu możliwości rozszerzania usługi IDirectRouteProvider użytkownik może wykonać następujące czynności:

1. Obsługuje dziedziczenia tras atrybutów. Na przykład w poniższym scenariuszu kontrolery blogu i Store korzystają z Konwencji tras atrybut, który jest definiowany przez BaseController. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Automatyczne generowanie nazw trasy dla tras atrybutów. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Zmodyfikuj prefiksy trasy w jednej centralnej lokalizacji, przed trasy poproś o dodanie Cię do tabeli tras.
4. Odfiltruj kontrolery, które mają atrybut routingu do wyszukania. Mamy nadzieję, że blogu na 3 i 4 wkrótce.

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook poprawki dotyczące zmienione powierzchni interfejsu API

Pakiet usługi Facebook MVC [został przerwany](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) z powodu kilka zmian interfejsu API przez usługi Facebook. Udostępniamy nowego pakietu usługi Facebook (Microsoft.AspNet.Facebook 1.0.0) Aby rozwiązać te problemy.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Tworzenie szkieletów MVC/interfejsu API sieci Web do projektu przy użyciu 5.2.0 pakietów skutkuje 5.1.2 pakietów dla tych, które jeszcze nie istnieją w projekcie

Trwa aktualizowanie pakietów NuGet dla platformy ASP.NET MVC 5.2.0 nie powoduje aktualizacji narzędzi programu Visual Studio, takich jak funkcja tworzenia szkieletu ASP.NET lub szablon projektu aplikacji sieci Web ASP.NET. Używają poprzedniej wersji pakiety środowiska uruchomieniowego platformy ASP.NET (np. 5.1.2 w wersji Update 2). Co w efekcie funkcja tworzenia szkieletu ASP.NET zainstaluje poprzedniej wersji (np. 5.1.2 w wersji Update 2) wymagane pakiety, jeśli nie są jeszcze dostępne w projektach. Funkcja tworzenia szkieletu ASP.NET w Visual Studio 2013 RTM i Update 1 nie zastąpi jednak najnowszych pakietów w swoich projektach. Jeśli używasz funkcja tworzenia szkieletu ASP.NET po zaktualizowaniu pakietów projektów sieci Web API 2.2 lub platformy ASP.NET MVC 5.2, upewnij się, że wersje interfejsu API sieci Web platformy ASP.NET MVC są spójne.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Instalacja pakietu Microsoft.jQuery.Unobtrusive.Validation NuGet zakończy się niepowodzeniem, ponieważ nie można odnaleźć wersji Microsoft.jQuery.Unobtrusive.Validation zgodny z jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation wymaga jQuery &gt;= 1.8 i jQuery.Validation &gt;= 1.8. But,jQuery.Validation (1.8) wymaga jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). W związku z tym NuGet instaluje JQuery 1.8 i jQuery.Validation 1.8, w tym samym czasie jej nie powiedzie się. Gdy pojawi się ten problem, można po prostu zaktualizuj wersję jQuery.Validation do &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) mającym ustalony limit jQuery najpierw powinno być możliwe do zainstalowania Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery. Wersja pakietu nuget weryfikacji 1.13.0 nie rozpoznaje kilka adresów e-mail międzynarodowych

Wersja pakietu nuget jQuery.Validation 1.11.1 jest ostatnią znaną wersję, która rozpoznaje następujących prawidłowych adresów e-mail. Wszystkie nowsze wersje nie może być rozpoznawany je. Na przykład:

Standardowa internacjonalizacji adresów (EAI) wiadomości e-Mail (np. [ &#29992; &#25143; @domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + Internationalized identyfikatory zasobów (IRIs) (np., [ &#29992; &#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088; &#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Ten problem zostanie zgłoszone w [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Wyróżnianie składni Razor widoków w programie Visual Studio 2013

Jeśli aktualizacja platformy ASP.NET MVC 5.2 bez aktualizowania programu Visual Studio 2013, nie uzyskasz pomoc techniczna do edytora programu Visual Studio do wyróżniania składni podczas edytowania widokami Razor. Będzie konieczne zaktualizowanie programu Visual Studio 2013 można pobrać tę obsługę.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Poprawki i aktualizacje funkcji pomocniczych

Ta wersja zawiera także kilka poprawek usterek i funkcji drobne aktualizacje. Pełną listę można znaleźć:

- [5.2 pakietu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

Tej wersji nie zawarto żadnych nowych funkcji ani poprawek błędów składnika MVC. Wprowadziliśmy [zmiany na stronach sieci Web](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) dla istotnie poprawiającą wydajność i Zaktualizowaliśmy wszystkie inne zależne pakiety firma Microsoft właścicielem, aby była zależna od tej nowej wersji składnika strony sieci Web.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 w wersji Beta

Informacje o wersji [tutaj](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Ta wersja zawiera tylko poprawki. Możesz użyć [to zapytanie](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) Aby wyświetlić listę problemów rozwiązanych w tej wersji.
