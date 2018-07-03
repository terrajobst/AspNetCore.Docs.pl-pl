---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: ASP.NET and Web Tools 2012.2 informacje o wersji | Dokumentacja firmy Microsoft
author: rick-anderson
description: Informacje o wersji platformy ASP.NET i Web Tools 2012.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: fe26f1f312415d97d71d2ab75d04877dc4a9cfbb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362401"
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET and Web Tools 2012.2 — informacje o wersji
====================
> Ten dokument zawiera opis wersji platformy ASP.NET i Web Tools 2012.2. Jest aktualizacja sieci Web narzędzi programu Visual Studio i platformy ASP.NET.


- [Uwagi dotyczące instalacji](#_Installation)
- [Dokumentacja](#_Documentation)
- [Obsługa](#_Support)
- [Wymagania dotyczące oprogramowania](#_Software_Requirements)
- [Nowe funkcje w ASP.NET and Web Tools 2012.2](#_New_Features_in)

    - [Narzędzia](#_Tooling)
    - [Publikowanie w sieci Web](#_Web_Publishing)
    - [Szablony ASP.NET MVC](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [Biblioteki SignalR platformy ASP.NET](#_ASP.NET_SignalR)
    - [ASP.NET, przyjazne adresy URL](#_ASP.NET_Friendly_URLs)
- [Znane problemy i fundamentalne zmiany](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

Program ASP.NET i Web Tools 2012.2 dla programu Visual Studio 2012 można zainstalować przy użyciu [Instalator platformy sieci Web](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Jest to aktualizacja programu Visual Studio 2012 lub Visual Studio Express 2012 for Web, która jest wymagana. Jeśli nie masz zainstalowanego programu Visual Studio, zostanie zainstalowany program Visual Studio Express 2012 for Web.

Możesz także zainstalować program ASP.NET i Web Tools 2012.2 ręcznie. Konieczne jest posiadanie programu Visual Studio 2012 lub Visual Studio Express 2012 for Web zainstalowany. Następnie użyj poniższych instrukcji: 

1. Pobierz [ASP.NET i sieci Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) Instalatora z Centrum pobierania.
2. Gdy monitem kliknij polecenie Uruchom. Można również zapisać plik, aby później można go uruchomić.
3. Sprawdź wersję programu Visual Studio spowoduje zaktualizowanie. Można to zrobić poprzez uruchomienie programu Visual Studio, które chcesz zaktualizować. Następnie kliknij element menu Pomoc.   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. Jeśli zostanie wyświetlony element menu &quot;o Microsoft Visual Studio 2012 for Web&quot; następnie pobierz [2012.2 narzędzi dla deweloperów sieci Web — Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkID=282228). W przeciwnym razie Pobierz [2012.2 narzędzi dla deweloperów sieci Web — Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Gdy monitem kliknij polecenie Uruchom. Można również zapisać plik, aby później można go uruchomić.

> [!NOTE]
> Wersja platformy ASP.NET i Web Tools 2012.2 nie zawiera programu SQL Server Data Tools. SQL Server i Windows Azure SQL Database udostępnia bogatszy zestaw narzędzi, w tym tworzenia opartych na projekt w trybie offline, porównanie schematu i możliwości wdrażania rozszerzone bazy danych w bazie danych. Aby uzyskać więcej informacji lub zainstalować SQL Server Data Tools można znaleźć [ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje o platformie ASP.NET i Web Tools 2012.2 są dostępne w witrynie sieci web platformy ASP.NET ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Obsługa

Program ASP.NET i Web Tools 2012.2 oficjalnie zostanie zwolniony i obsługiwane. Można użyć swojego kanału normalne pomocy technicznej. Może także umieszczać pytania na forach platformy ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), gdzie są często w stanie zapewnić obsługę nieformalne członków społeczności platformy ASP.NET.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Wymagania programowe

Program ASP.NET i Web Tools 2012.2 wymaga programu Visual Studio 2012 lub Visual Studio Express 2012 for Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nowe funkcje w ASP.NET and Web Tools 2012.2

W tej sekcji opisano funkcje, które zostały wprowadzone w wersji platformy ASP.NET i Web Tools 2012.2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Narzędzia

- Inspektor strony 

    - Obsługuje mapowania wybór języka JavaScript, dzięki czemu narzędzia Page Inspector do mapowania elementy, które zostały dynamicznie dodawane do strony do odpowiedniego kodu JavaScript.
    - Możliwość wyświetlania CSS aktualizacji w czasie rzeczywistym.
    - Aby uzyskać więcej informacji, przeczytaj [automatyczna synchronizacja CSS i JavaScript wybór mapowanie w narzędzia Page Inspector](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Edytor 

    - Obsługuje wyróżniania składni języka CoffeeScript, Mustache i Handlebars oraz JsRender.
    - Edytor HTML udostępnia funkcji Intellisense dla wiązania Knockout.
    - MNIEJ edycji i obsługują umożliwia tworzenie dynamicznych CSS przy użyciu mniejsza.
    - Wklej dane JSON jako klasy .NET. Polecenie to specjalne Wklej wkleić JSON w języku C# lub VB.NET plik kodu, a program Visual Studio automatycznie wygeneruje klasy .NET, wywnioskowane z kodu JSON.
- Obsługa emulatora Mobile dodaje punkty zaczepienia rozszerzalności, co emulatorów innej firmy można zainstalować jako VSIX. Zainstalowanych emulatorów pojawi się na liście rozwijanej F5, aby deweloperów Podgląd można wyświetlić ich witryn sieci Web na różnych urządzeniach przenośnych. Przeczytaj więcej na temat tej funkcji na wpis w blogu Scotta Hanselmana [nowej BrowserStack integracji z programem Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Publikowanie w sieci Web

- Projekt witryny sieci Web ma teraz takie samo środowisko publikowania, jak projekty aplikacji sieci Web, w tym publikowania witryny sieci Web systemu Azure Windows.
- Selektywne publikowanie &#8211; dla jednego lub więcej plików (po opublikowaniu punkt końcowy Web Deploy) można wykonać następujące czynności: 

    - Opublikuj wybrane pliki.
    - Zobaczyć różnicę między pliku lokalnego i zdalnego pliku.
    - Aktualizowanie pliku lokalnego za pomocą pliku zdalnego lub zaktualizować pliku zdalnego pliku lokalnego.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Szablony ASP.NET MVC

- Nowy szablon aplikacji usługi Facebook ułatwia pisanie aplikacji łatwo usługi Facebook. W kilku prostych krokach można utworzyć aplikacji usługi Facebook, która pobiera dane od zalogowanego użytkownika oraz integruje się z jego znajomymi. Szablon zawiera nową bibliotekę automatyzującą wszystkie żmudne procesy związanie z tworzeniem aplikacji usługi Facebook, łącznie z uwierzytelnianiem, uprawnieniami i uzyskiwania dostępu do danych usługi Facebook. Aby uzyskać więcej informacji na temat korzystania z szablonu aplikacji usługi Facebook zobacz [ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921).
- Nowy szablon pojedynczej aplikacji jednostronicowej platformy MVC umożliwia deweloperom tworzenie aplikacji sieci web interactive po stronie klienta przy użyciu języków HTML 5, CSS 3 oraz popularnych Knockout i biblioteki JavaScript jQuery, Web API platformy ASP.NET. Szablon zawiera aplikację do listy "todo", który pokazuje typowe rozwiązania dotyczące tworzenia aplikacji JavaScript HTML5 korzystającej z interfejsu API serwera REST. Możesz dowiedzieć się więcej o [ https://www.asp.net/single-page-application ](../../../single-page-application/index.md).
- Teraz można utworzyć VSIX, który dodaje nowe szablony do okna dialogowego Nowy projekt programu ASP.NET MVC. Dowiedz się jak tutaj: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Pakiet FixedDisplayModes &#8211; projektu MVC zostały zaktualizowanie, aby uwzględnić nowy pakiet NuGet "FixedDisplayModes", który zawiera rozwiązanie dla usterki w MVC 4. Więcej informacji na temat poprawki zawarte w pakiecie, można znaleźć w tym wpisie w blogu ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) od zespołu MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET Web API została rozszerzona o kilka nowych funkcji:

- ASP.NET Web API OData
- Śledzenie interfejsu API sieci Web platformy ASP.NET
- Strona pomocy interfejsu API sieci Web platformy ASP.NET

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData zapewnia elastyczność potrzebne do tworzenia punktów końcowych OData z logiką biznesową sformatowanego w każdym źródle danych. Za pomocą programu ASP.NET Web API OData, możesz kontrolować ilość semantyki OData, którą chcesz udostępnić. ASP.NET Web API OData jest dostarczany razem z szablonów projektów platformy ASP.NET MVC 4 i jest również dostępna z pakietu NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData obecnie obsługuje następujące funkcje:

- Włącz semantyki zapytań OData, stosując atrybut [Queryable].
- Z łatwością weryfikacji zapytań OData i ograniczyć zestaw opcji obsługiwane zapytania, operatory i funkcje.
- Powiązanie parametru z ODataQueryOptions bezpośrednio, aby uzyskać reprezentację w postaci drzewo abstrakcyjnej składni, zapytania, które można następnie sprawdzane i stosowane do interfejsu IEnumerable lub IQueryable.
- Włączanie stronicowania opartych na usługi oraz Nowa generacja łącze strony, określając limity wyników dla atrybutu [Queryable].
- Żądanie śródwierszowych liczba całkowita liczba zgodnych zasobów przy użyciu $inlinecount.
- Kontrolowanie propagacji wartości null.
- Operatory/All w $filter.
- Wnioskowanie modelu danych jednostki według Konwencji lub jawnie dostosowania modelu w sposób podobny do Entity Framework najpierw kod.
- Uwidacznianie zestawy jednostek, wynikające z EntitySetController.
- Proste, możliwych do dostosowania konwencje udostępnianie właściwości nawigacji, manipulacji łączami i wykonywania akcji OData.
- Uproszczone, routing przy użyciu metody rozszerzenia MapODataRoute.
- Obsługa wersji, zapewniając wiele modeli EDM.
- Udostępnić dokument usługi i $metadata aby umożliwić generowanie klientów (.NET, Windows Phone, Windows Store itp.) dla interfejsu API sieci Web.
- Obsługa formatów pełne OData Atom, JSON i JSON.
- Tworzenie, aktualizowanie, częściowo (poprawki) aktualizacji i usuwania jednostek.
- Zapytania i modyfikować relacje między jednostkami.
- Tworzenie łączy relacji, które połączenie do trasy.
- Typy złożone.
- Dziedziczenie typu jednostki.
- Właściwości kolekcji.
- Wyliczenia.
- Akcje protokołu OData.
- Utworzonych na podstawie tych samych podstawach co WCF Data Services, a mianowicie ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Aby uzyskać więcej informacji na temat platformy ASP.NET Web API OData zobacz [ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Śledzenie interfejsu API sieci Web platformy ASP.NET

Śledzenia ASP.NET Web API integruje śledzenie danych z sieci web, interfejsów API z włączonym śledzeniem .NET. Jego jest teraz domyślnie włączona w szablonie projektu interfejsu API sieci Web. Śledzenie danych w sieci Web interfejsów API jest wysyłany do okna danych wyjściowych i są udostępniane za pośrednictwem funkcji IntelliTrace. Śledzenia interfejsu API sieci Web ASP.NET umożliwia śledzenie informacji o interfejsie API sieci Web podczas hostowanej na platformie Windows Azure, dzięki integracji z usługą [Windows Azure Diagnostics](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Można również zainstalować i włączyć śledzenia interfejsu API sieci Web platformy ASP.NET w dowolnej aplikacji przy użyciu pakietu NuGet śledzenia interfejsu API sieci Web platformy ASP.NET ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Aby uzyskać więcej informacji na temat konfigurowania i używania śledzenia interfejsu API sieci Web platformy ASP.NET zobacz [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Strona pomocy interfejsu API sieci Web platformy ASP.NET

Stronę pomocy interfejsu API sieci Web platformy ASP.NET jest teraz domyślnie w szablonie projektu interfejsu API sieci Web. Stronę pomocy interfejsu API sieci Web platformy ASP.NET automatycznie generuje dokumentację dotyczącą interfejsów API, w tym punktów końcowych HTTP obsługiwane metody HTTP, parametry oraz przykład ładunkami komunikatów żądań i odpowiedzi sieci web. Dokumentacja jest automatycznie pobierany z komentarzy w kodzie. Można również dodać stronę pomocy interfejsu API sieci Web platformy ASP.NET do dowolnej aplikacji przy użyciu pakietu pomocy NuGet strony ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Aby uzyskać więcej informacji na temat instalowania i dostosowywania, zobacz stronę pomocy interfejsu API sieci Web ASP.NET [ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR ułatwia dodawanie funkcji internetowych w czasie rzeczywistym do aplikacji platformy ASP.NET używa funkcji WebSockets, jeśli jest dostępny i automatycznie nastąpi powrót do innych technik, gdy nie jest.

Aby uzyskać więcej informacji na temat korzystania z biblioteki SignalR platformy ASP.NET, zobacz [ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET, przyjazne adresy URL

ASP.NET FriendlyURLs sprawia, że znacznie ułatwia deweloperom formularzy sieci web do wygenerowania czyszcząca wyszukiwania adresów URL (bez rozszerzenia .aspx). Ona wymaga nieco do konfiguracji i może być używany z istniejących aplikacji programu ASP.NET w wersji 4.0. Funkcja FriendlyURLs również ułatwia deweloperom dodawać obsługę operacji mobilnych ze swoimi aplikacjami dzięki obsłudze przełączania się między widokami pulpitu i na urządzeniach przenośnych.

Aby uzyskać więcej informacji o instalowaniu i używaniu przyjazne adresy URL platformy ASP.NET zobacz [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

W tej sekcji opisano znane problemy i przełomowe zmiany, które znajdują się w wersji platformy ASP.NET i Web Tools 2012.2.

### <a name="installation-issues"></a>Problemy z instalacją

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Poza kolejnością instalacji programu Visual Studio 2012

Instalowanie dodatkowych jednostek SKU programu Visual Studio 2012, po zainstalowaniu programu ASP.NET i Web Tools 2012.2 będzie wymagać operacji naprawy. Należy wziąć pod uwagę następującej sekwencji:

1. Zainstaluj program Visual Studio Express 2012 for Web
2. Instalowanie platformy ASP.NET i narzędzi Web Tools 2012.2
3. Instalowanie programu Visual Studio 2012 Professional, Premium lub Ultimate

Krok 2 spowoduje tylko instalowanie aktualizacji programu Express for Web. Aby upewnić się, że dodatkowe jednostki SKU instalowane w ramach kroku 3 zawiera aktualizacji będzie potrzeba naprawy środowiska ASP.NET i Web Tools 2012.2, aby zainstalować aktualizacje dla ostatnich jednostki SKU zainstalowane. Dotyczy to również, jeśli jednostki SKU w kroku 1 i 3 zostały cofnięte.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Instalowanie programu Microsoft ASP.NET and Web Tools 2012.2 po otwarciu programu Visual Studio

Jeśli program VS jest otwarty podczas instalacji programu Microsoft ASP.NET and Web Tools 2012.2, Visual Studio może znajdą się w nieprawidłowym stanie. Zaleca się, że użytkownicy, zamknij wszystkie wystąpienia programu Visual Studio przed kontynuowaniem instalacji.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Trwa anulowanie platformy ASP.NET i Web Tools 2012.2 instalacji w trakcie instalacji

Anulowanie platformy ASP.NET i Web Tools 2012.2 instalacji w trakcie instalacji spowoduje, że program Visual Studio w złym stanie. Aby rozwiązać ten problem, postępuj zgodnie z następujące kroki: 

- Przejdź do pozycji Dodaj/Usuń programy
- Odinstaluj Microsoft ASP.NET and Web Tools 2012.2, jeśli jest obecny.
- Ponownie zainstaluj program Microsoft ASP.NET and Web Tools 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Po odinstalowaniu programu ASP.NET i Web Tools 2012.2 platformy ASP.NET MVC 4 brakuje szablony i Razor v2 witryny sieci Web

Odinstalowywanie programu ASP.NET i Web Tools 2012.2 spowoduje także odinstalowanie wszystkich platformy ASP.NET MVC 4 i szablony witryn sieci Web w wersji 2 Razor z programu Visual Studio 2012.

Obejście polega na napraw instalację programu Visual Studio 2012, ponowne zainstalowanie programu ASP.NET MVC 4 i szablony witryn sieci Web w wersji 2 Razor.

### <a name="tooling-issues"></a>Problemy dotyczące narzędzi

#### <a name="nuget-error-reported-during-project-creation"></a>Błąd NuGet zgłoszone podczas tworzenia projektu

Po zainstalowaniu programu ASP.NET i Web Tools 2012.2 może zostać wyświetlony następujący błąd podczas tworzenia projektu MVC 4

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

Program ASP.NET i Web Tools 2012.2 jest dostarczany NuGet 2.1 i uaktualni rozszerzenia programu Visual Studio 2012. W niektórych przypadkach Instalator VSIX zakończy się niepowodzeniem, można właściwie zaktualizować pliku VSIX. Poniższe kroki pozwoli rozwiązać ten problem:

1. Uruchom program Visual Studio 2012 jako Administrator
2. Przejdź do pozycji narzędzia -&gt;rozszerzenia i aktualizacje i odinstalowywania NuGet.
3. Zamknij program Visual Studio
4. Przejdź do folderu instalacji programu ASP.NET i Web Tools 2012.2:

    1. Dla programu Visual Studio 2012: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Aby uzyskać program Visual Studio Express 2012 for Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 for Web**
5. Kliknij dwukrotnie NuGet.Tools.vsix do ponownej instalacji NuGet

### <a name="web-api-issues"></a>Problemy z interfejsu API sieci Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Podczas analizowania problemów z $filter i wszystkie literały daty/godziny

Parser identyfikatora URI OData kończy się niepowodzeniem, można poprawnie przeanalizować literały częściowe daty/godziny. Na przykład $filter = datą i godziną początkową eq'2012-12-31T12:00 "nie można poprawnie przeanalizować. Obejście tego problemu jest użycie pełnej literału $filter = datą i godziną początkową eq'2012-12-31T12:00:00 ".

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData nie obsługuje nazw właściwości bez uwzględniania wielkości liter.

OData nie obsługuje nazw właściwości bez uwzględniania wielkości liter w zapytaniach OData i ścieżki odata. Zobacz elementy robocze:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Jeśli użytkownicy mają inną wielkością liter po stronie klienta javascript i po stronie serwera, są prawdopodobnie wystąpi ten problem. Ten problem jest celowe w protokole odata. Wielu użytkowników raportów, jednak ten problem. Aby go obejść, użytkownicy muszą poprawić ich przypadki, w adresie URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>OData domyślnej konwencji routingu nie obsługuje POST/PUT na właściwość nawigacji.

OData domyślnej konwencji routingu nie obsługuje POST/PUT na właściwość nawigacji. Zobacz workitem [ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366). Czy pominęliśmy jakiś to powszechnie używanych Konwencji w domyślnych Konwencji.

Aby go obejść, użytkownicy muszą rozszerzać nowej Konwencji routingu do jego obsługi.

### <a name="facebook-template-issues"></a>Problemy dotyczące szablonu usługi Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Szablon aplikacji usługi Facebook działa tylko przy użyciu platformy .NET 4.5

Na liście rozwijanej framework w oknie dialogowym Nowy projekt, aby wyświetlić szablon aplikacji usługi Facebook w ASP.NET MVC 4, należy wybrać .NET 4.5.

#### <a name="real-time-update-controller"></a>Kontroler aktualizacji w czasie rzeczywistym

Szablon aplikacji usługi Facebook umożliwia użytkownikowi łatwo utworzyć kontroler interfejsu API sieci Web do obsługi w czasie rzeczywistym aktualizacje z usługi Facebook. W przypadku komputera deweloperskiego za translatora adresów Sieciowych, kontroler może nie działać bez dalszej konfiguracji sieci. Zobacz tutaj, aby uzyskać szczegółowe informacje: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Zapytanie konflikt wartości ciąg z parametrami OAuth usługi Facebook

Następujące pola są w konflikcie ze wywołanie uwierzytelniania Facebook OAuth okno kopii adresu URL. Nie należy dodawać własne wartości ciągu zapytania o następujących nazwach: kod, błąd, błąd\_opis, błąd\_przyczyna.

#### <a name="using-page-inspector-with-facebook-template"></a>Za pomocą narzędzia Page Inspector przy użyciu szablonu usługi Facebook

Nie można użyć funkcji narzędzia Page Inspector w programie Visual Studio 2012, podczas debugowania aplikacji usługi Facebook. Narzędzie Page Inspector aktualnie nie obsługuje elementy IFRAME.

### <a name="single-page-application-template-issues"></a>Generuje szablon aplikacji jednostronicowej

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Przy użyciu JQuery wprowadź 1.9/Knockout 2.2.1 aktualizacji, podczas uruchamiania projektu MVC SPA domyślnego, nowe edycji elementu todo zdarzenia fokusu nie odbywa się poprawnie.

Przy użyciu JQuery 1.9/Knockout 2.2.1 aktualizacji, gdy używany jest domyślny projekt MVC SPA, nowe edycji elementu todo wprowadź nie jest już fokus do pola edycji elementu nowych zadań do wykonania po wprowadzeniu nowego elementu todo do listy zadań do wykonania.

Odwołanie obejście [ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html)i wprowadzić poprawkę podobne następujący przykładowy kod:

Todo.model.js pliku  
 Funkcja todolist(data), Dodaj następujące:  
 **self.isSelected = ko.observable(false);**

Funkcja todoList.prototype.addTodo, Dodaj następujący tekst blacked:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Plik index.cshtml, Dodaj następujący tekst blacked:  
 &lt;Formularz data-bind =&quot;przesłać: addTodo&quot;&gt;  
 &lt;dane wejściowe klasy =&quot;addTodo&quot; typu =&quot;tekstu&quot; wiązania danych =&quot;wartość: newTodoTitle, symbolu zastępczego: "Typ w tym miejscu można dodać", blurOnEnter: ma wartość true, **hasfocus: isSelected**, zdarzenia: {Rozmycie: addTodo}&quot; /&gt;  
 &lt;/ Form&gt;
