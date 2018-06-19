---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: ASP.NET i sieć Web narzędzi 2012.2 informacje o wersji | Dokumentacja firmy Microsoft
author: rick-anderson
description: Informacje o wersji programu ASP.NET i 2012.2 narzędzia sieci Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: ab1642f1a3de298919aa9c6c1ddbd6bbb0cb99b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
ms.locfileid: "28037285"
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>Informacje o wersji platformy ASP.NET i narzędzia sieci Web 2012.2
====================
> Ten dokument zawiera opis wersji platformy ASP.NET i 2012.2 narzędzia sieci Web. Jest to aktualizacja do narzędzi Visual Studio Web i platformy ASP.NET.


- [Informacje o instalacji](#_Installation)
- [Dokumentacja](#_Documentation)
- [Obsługa](#_Support)
- [Wymagania dotyczące oprogramowania](#_Software_Requirements)
- [Nowe funkcje programu ASP.NET i narzędzia sieci Web 2012.2](#_New_Features_in)

    - [Narzędzia](#_Tooling)
    - [Publikowanie w sieci Web](#_Web_Publishing)
    - [ASP.NET MVC Templates](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [ASP.NET Friendly URLs](#_ASP.NET_Friendly_URLs)
- [Znane problemy i fundamentalne zmiany](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Informacje o instalacji

ASP.NET i 2012.2 narzędzia sieci Web dla programu Visual Studio 2012 można zainstalować przy użyciu [Instalatora platformy sieci Web](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Jest to aktualizacja programu Visual Studio 2012 lub Visual Studio Express 2012 for Web, która jest wymagana. Jeśli nie masz zainstalowanego programu Visual Studio, Visual Studio Express 2012 for Web zostanie zainstalowany.

Możesz także zainstalować ASP.NET i 2012.2 narzędzia sieci Web ręcznie. Musi mieć programu Visual Studio 2012 lub Visual Studio Express 2012 for Web zainstalowane. Następnie użyj poniższych instrukcji: 

1. Pobierz [ASP.NET i sieci Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) Instalatora z Centrum pobierania.
2. Gdy zostanie wyświetlony monit o kliknij polecenie Uruchom. Można także zapisać plik później uruchomić.
3. Sprawdź wersję programu Visual Studio spowoduje zaktualizowanie. Można to zrobić, uruchamiając programu Visual Studio, które chcesz zaktualizować. Następnie kliknij element menu Pomoc.   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. Jeśli zostanie wyświetlony element menu &quot;o Microsoft Visual Studio 2012 for Web&quot; następnie pobierz [Web Developer Tools 2012.2 — Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkID=282228). W przeciwnym razie Pobierz [Web Developer Tools 2012.2 - programu Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Gdy zostanie wyświetlony monit o kliknij polecenie Uruchom. Można także zapisać plik później uruchomić.

> [!NOTE]
> Wersja platformy ASP.NET i 2012.2 narzędzia sieci Web nie ma programu SQL Server Data Tools. SQL Server i bazy danych SQL Azure z systemem Windows zapewnia bogaty zestaw narzędzi, w tym tworzenia kopii projektu w trybie offline, porównanie schematu i możliwości wdrożenia rozszerzonego bazy danych w bazie danych. Aby uzyskać więcej informacji lub zainstalować program SQL Server Data Tools odwiedź [ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje dotyczące platformy ASP.NET i 2012.2 narzędzia sieci Web są dostępne w witrynie sieci web platformy ASP.NET ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Obsługa

ASP.NET 2012.2 narzędzia sieci Web oficjalnie zwolnione i które są obsługiwane. Można użyć kanału normalne pomocy technicznej. Można również zadawać pytania na forach platformy ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), gdzie są często można zapewnić obsługę nieformalne członkami społeczności programu ASP.NET.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Wymagania programowe

ASP.NET i 2012.2 narzędzia sieci Web wymaga programu Visual Studio 2012 lub Visual Studio Express 2012 for Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nowe funkcje programu ASP.NET i narzędzia sieci Web 2012.2

W tej sekcji opisano funkcje, które zostały wprowadzone w wersji platformy ASP.NET i 2012.2 narzędzia sieci Web.

<a id="_Tooling"></a>
### <a name="tooling"></a>Narzędzia

- Inspektor strony 

    - Obsługuje stosowanie narzędzie Page Inspector mapować elementy, które były dodawane dynamicznie do strony do odpowiedniego kodu JavaScript mapowania wybór języka JavaScript.
    - Możliwość sprawdzenia aktualizacji CSS w czasie rzeczywistym.
    - Aby uzyskać więcej informacji, przeczytaj [automatyczna synchronizacja CSS i JavaScript wybór mapowanie w narzędzie Page Inspector](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Edytor 

    - Obsługuje wyróżnianie składni języka CoffeeScript, Mustache, odległość i JsRender.
    - Edytor HTML zapewnia Intellisense odcinania powiązania.
    - MNIEJ edycji i kompilatora obsługuje umożliwia tworzenie dynamicznych CSS przy użyciu mniejsza.
    - Wklej dane JSON jako klasy .NET. To polecenie Wklej specjalne wkleić JSON w języku C# lub VB.NET pliku kodu, a program Visual Studio automatycznie wygeneruje klasy .NET wywnioskować na podstawie JSON.
- Obsługa emulatora Mobile dodaje punkty zaczepienia rozszerzalności tak, aby emulatory innej firmy można zainstalować jako VSIX. Emulatory zainstalowanych zostaną wyświetlone na liście rozwijanej F5, dzięki czemu deweloperzy mogą podglądu swoich witryn sieci Web na różnych urządzeniach przenośnych. Dowiedz się więcej o tej funkcji, w którym Scott Hanselman wpis w blogu na [nowe BrowserStack integrację z programem Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Publikowanie w sieci Web

- Projekt witryny sieci Web ma teraz tego samego środowiska publikowania jako projektów aplikacji sieci Web, w tym publikowanie do systemu Windows Azure Web Sites.
- Publikowanie selektywne &#8211; dla jednego lub więcej plików (po opublikowaniu do punktu końcowego narzędzia Web Deploy) można wykonać następujące akcje: 

    - Opublikuj wybrane pliki.
    - Widocznej różnicy między pliku lokalnego i zdalnego pliku.
    - Zaktualizuj plik lokalny z pliku zdalnego lub zaktualizować pliku zdalnego pliku lokalnego.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Szablony ASP.NET MVC

- Nowy szablon aplikacji usługi Facebook sprawia, że pisanie łatwe aplikacji usługi Facebook. W kilku prostych krokach można utworzyć aplikacji usługi Facebook, która pobiera dane od zalogowanego użytkownika i integruje się z jego znajomymi. Szablon zawiera nową bibliotekę automatyzującą wszystkie żmudne procesy związanie z tworzeniem aplikacji usługi Facebook, łącznie z uwierzytelnianiem, uprawnieniami i uzyskiwania dostępu do danych usługi Facebook. Aby uzyskać więcej informacji na temat przy użyciu szablonu aplikacji usługi Facebook zobacz [ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921).
- Nowy szablon jednej aplikacji jednostronicowej platformy MVC umożliwia deweloperom tworzenie aplikacji sieci web po stronie klienta interactive przy użyciu HTML 5, CSS 3 oraz popularnych Knockout i jQuery JavaScript bibliotek interfejsu API sieci Web platformy ASP.NET. Szablon zawiera aplikację do listy "todo", której przedstawiono typowe rozwiązania do tworzenia aplikacji JavaScript HTML5 korzystającej z interfejsu API serwera REST. Więcej w [ https://www.asp.net/single-page-application ](../../../single-page-application/index.md).
- Można teraz utworzyć VSIX, który dodaje nowe szablony do okna dialogowego Nowy projekt programu ASP.NET MVC. Dowiedz się, jak poniżej: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Pakiet FixedDisplayModes &#8211; szablony projektów MVC zostały zaktualizowane, aby dołączyć nowy pakiet NuGet "FixedDisplayModes" zawiera obejścia usterki w MVC 4. Aby uzyskać więcej informacji na zawartych w pakiecie poprawki dotyczą ten wpis w blogu ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) od zespołu MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

Interfejs API sieci Web programu ASP.NET została rozszerzona o kilka nowych funkcji:

- ASP.NET Web API OData
- ASP.NET Web API Tracing
- Strona pomocy interfejsu API sieci Web ASP.NET

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData zapewnia elastyczność jest potrzebne do tworzenia punktów końcowych OData z logiki biznesowej sformatowanego w każdym źródle danych. Z programu ASP.NET Web API OData kontrolujesz ilość semantyki OData, które chcesz udostępnić. ASP.NET Web API OData jest dołączony do platformy ASP.NET MVC 4 szablony projektów i jest dostępna również w NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData obsługuje obecnie następujące funkcje:

- Włącz semantyki zapytań OData przez zastosowanie atrybutu [Queryable].
- Możesz łatwo weryfikacji zapytań OData i ograniczyć zestaw opcji zapytania obsługiwanych, Operatorzy i funkcje.
- Powiązanie parametru z ODataQueryOptions bezpośrednio, aby uzyskać reprezentację w postaci drzewa składni abstrakcyjnej, zapytania, które można następnie sprawdzane i stosowane do IQueryable lub IEnumerable.
- Włącz stronicowania obsługiwanego przez usługi i generacji łącza strony, określając limity wyników dla atrybutu [Queryable].
- Żądanie wbudowanego liczba całkowita liczba pasujących zasobów przy użyciu $inlinecount.
- Formant propagacji wartości null.
- Operatory/All w $filter.
- Wnioskowanie modelu danych jednostki według Konwencji lub jawnie Dostosowywanie modelu w sposób podobny do Entity Framework kod pierwszego.
- Ustawia jednostki Uwidacznianie przez wynikających z EntitySetController.
- Konwencje proste, które można dostosowywać udostępnianie właściwości nawigacji, manipulowanie łącza i wykonywania akcji OData.
- Uproszczone routingu przy użyciu metody rozszerzenia MapODataRoute.
- Obsługa wersji przez udostępnianie wielu modelach EDM.
- Udostępnianie dokumentu usługi i $metadata, można wygenerować klientów (.NET, Windows Phone, Sklep Windows itd.) do interfejsu API sieci Web.
- Obsługa formatów pełne OData Atom, JSON i JSON.
- Tworzenie, aktualizowanie, częściowo aktualizacji (poprawki) i usuwania jednostek.
- Zapytania i manipulowania relacje między obiektami.
- Tworzenie łączy relacji, które okablować do trasy.
- Typy złożone.
- Dziedziczenie typu jednostki.
- Właściwości kolekcji.
- Typy wyliczeniowe.
- Akcji OData.
- Na tej samej podstawie co usługi danych WCF, czyli ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Aby uzyskać więcej informacji na temat programu ASP.NET Web API OData zobacz [ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>ASP.NET Web API Tracing

Śledzenia ASP.NET Web API integruje dane śledzenia z sieci web API z włączonym śledzeniem .NET. Teraz włączono domyślny szablon projektu interfejsu API sieci Web. Śledzenie danych w sieci Web jest wysyłane do okna wyjściowego interfejsów API i jest udostępniana przy użyciu funkcji IntelliTrace. ASP.NET Web API Tracing pozwala do śledzenia informacji na temat interfejsu API sieci Web podczas udostępniania w systemie Windows Azure dzięki integracji z [Windows Azure Diagnostics](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Można również zainstalować i włączyć ASP.NET Web API Tracing w dowolnej aplikacji przy użyciu pakietu NuGet śledzenia interfejsu API sieci Web platformy ASP.NET ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Aby uzyskać więcej informacji na temat konfigurowania i używania programu ASP.NET Web API Tracing zobacz [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Strona pomocy interfejsu API sieci Web ASP.NET

Stronę pomocy programu ASP.NET Web API jest teraz zawarta domyślny szablon projektu interfejsu API sieci Web. Stronę pomocy interfejsu API sieci Web ASP.NET jest automatycznie generuje tym punktów końcowych HTTP, obsługiwane metody HTTP, parametry oraz przykład ładunków komunikatów żądań i odpowiedzi interfejsów API sieci web w dokumentacji. Dokumentacja są automatycznie pobierane z komentarze w kodzie. Stronę pomocy interfejsu API sieci Web platformy ASP.NET można również dodać do aplikacji przy użyciu pakietu pomocy NuGet strony ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Aby uzyskać więcej informacji na temat instalowania i dostosowywania Zobacz strona pomocy interfejsu API sieci Web ASP.NET [ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

Biblioteka SignalR platformy ASP.NET ułatwia dodawanie funkcji sieci web w czasie rzeczywistym do aplikacji ASP.NET przy użyciu protokołu WebSockets, jeśli jest dostępny i automatycznie nastąpi powrót do innych technik, gdy nie jest.

Aby uzyskać więcej informacji na temat używania biblioteki SignalR platformy ASP.NET, zobacz [ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET, przyjazne adresy URL

ASP.NET FriendlyURLs ułatwia bardzo dla deweloperów formularzy sieci web do wygenerowania czyszcząca wyszukiwania adresów URL (bez rozszerzenia aspx). Mały do nie wymaga konfiguracji, a może być używany z istniejących aplikacji programu ASP.NET 4.0. Funkcja FriendlyURLs również ułatwia deweloperom dodać obsługę przenośnych do swoich aplikacji dzięki obsłudze przełączania się między widokami komputerów stacjonarnych i przenośnych.

Aby uzyskać więcej informacji o instalowaniu i używaniu przyjazne adresy URL platformy ASP.NET zobacz [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

W tej sekcji opisano znane problemy i fundamentalne zmiany, które są w wersji platformy ASP.NET i 2012.2 narzędzia sieci Web.

### <a name="installation-issues"></a>Problemy z instalacją

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Poza kolejnością instalacji programu Visual Studio 2012

Instalowanie dodatkowych jednostki SKU programu Visual Studio 2012, po zainstalowaniu programu ASP.NET i 2012.2 narzędzia sieci Web wymaga operacji naprawy. Należy wziąć pod uwagę następującej kolejności:

1. Zainstaluj program Visual Studio Express 2012 for Web
2. Zainstaluj program ASP.NET i narzędzia sieci Web 2012.2
3. Zainstaluj program Visual Studio 2012 Professional, Premium lub Ultimate

Krok 2 spowoduje tylko instalowanie aktualizacji Express for Web. Aby upewnić się, że dodatkowe jednostki SKU zainstalowany w kroku 3 zawiera aktualizację należy naprawić ASP.NET i 2012.2 narzędzia sieci Web do zainstalowania aktualizacji dla jednostki SKU ostatniego zainstalowane. Dotyczy to również jednostki SKU w kroku 1 i 3 zostały zamienione.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Instalowanie programu Microsoft ASP.NET i 2012.2 narzędzia sieci Web po otwarciu programu Visual Studio

Jeśli VS jest otwarty podczas instalacji programu Microsoft ASP.NET i 2012.2 narzędzia sieci Web, Visual Studio może spowodować nieprawidłowy stan. Zaleca się, że użytkownicy, zamknij wszystkie wystąpienia programu Visual Studio przed kontynuowaniem instalacji.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Anulowanie instalacji programu ASP.NET i 2012.2 narzędzia sieci Web w trakcie instalacji

Anulowanie ASP.NET i 2012.2 narzędzia sieci Web instalacji w trakcie instalacji spowoduje zamknięcie programu Visual Studio w złym stanie. W celu rozwiązania tego problemu wykonaj następujące kroki: 

- Przejdź do Dodaj/Usuń programy
- Odinstaluj program Microsoft ASP.NET i 2012.2 narzędzia sieci Web, jeśli jest obecny.
- Ponownie zainstaluj program Microsoft ASP.NET i narzędzia sieci Web 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Po odinstalowaniu programu ASP.NET i 2012.2 narzędzia sieci Web platformy ASP.NET MVC 4 brakuje szablony i Razor v2 witryny sieci Web

Odinstalowanie programu ASP.NET i 2012.2 narzędzia sieci Web spowoduje również odinstalowanie wszystkich ASP.NET MVC 4 i szablony witryn sieci Web Razor v2 z programu Visual Studio 2012.

Obejście polega na napraw instalację programu Visual Studio 2012 w celu ponownej instalacji programu ASP.NET MVC 4 oraz szablony witryn sieci Web Razor v2.

### <a name="tooling-issues"></a>Problemy z narzędziami

#### <a name="nuget-error-reported-during-project-creation"></a>Błąd NuGet zgłoszony podczas tworzenia projektu

Po zainstalowaniu programu ASP.NET i 2012.2 narzędzia sieci Web mogą pojawić następujący błąd podczas tworzenia projektu składnika MVC 4

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

ASP.NET i 2012.2 narzędzia sieci Web NuGet 2.1 jest dostarczany i uaktualni rozszerzenia w programie Visual Studio 2012. W niektórych przypadkach Instalatora VSIX nie będzie można poprawnie zaktualizować pliku VSIX. Poniższe kroki pozwala rozwiązać ten problem:

1. Uruchom jako Administrator programu Visual Studio 2012
2. Przejdź do pozycji narzędzia -&gt;rozszerzenia i aktualizacje i odinstalowywania NuGet.
3. Zamknij program Visual Studio
4. Przejdź do folderu instalacji programu ASP.NET i 2012.2 narzędzia sieci Web:

    1. For Visual Studio 2012: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. For Visual Studio 2012 Express for Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 for Web**
5. Kliknij dwukrotnie NuGet.Tools.vsix ponowna instalacja NuGet

### <a name="web-api-issues"></a>Problemy z interfejsu API sieci Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Podczas analizowania problemów w $filter i literały daty i godziny

Analizatora składni identyfikatora URI OData nie można poprawnie przeanalizować literały z częściowa daty/godziny. Na przykład $filter = godziną początkową eq'2012-12-31T12:00 "nie powiedzie się prawidłowo przeanalizować. Obejście tego problemu jest użycie pełnej literału $filter = godziną początkową eq'2012-12-31T12:00:00 ".

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData nie obsługuje nazw właściwości bez uwzględniania wielkości liter.

OData nie obsługuje nazw właściwości bez uwzględniania wielkości liter w zapytaniach OData i ścieżki odata. Zobacz elementów roboczych:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Jeśli użytkownicy mają innej wielkości znaków na javascript po stronie klienta i po stronie serwera, ich prawdopodobnie wystąpi ten problem. Ten problem jest celowe w protokole odata. Jednak w przypadku wielu użytkowników Raport ten problem. Aby obejść go, użytkownicy muszą Popraw ich przypadków w adresie URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Routing Konwencji OData domyślny nie obsługuje POST/PUT we właściwości nawigacji.

Routing Konwencji OData domyślny nie obsługuje POST/PUT we właściwości nawigacji. Zobacz element roboczy [ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366). Firma Microsoft Brak tę Konwencję często używane w domyślnych Konwencji.

Aby obejść go, użytkownicy muszą rozszerzyć nowej Konwencji routingu do jego obsługi.

### <a name="facebook-template-issues"></a>Problemy dotyczące szablonu usługi Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Szablon aplikacji usługi Facebook działa tylko przy użyciu platformy .NET 4.5

Musisz wybrać .NET 4.5, w ramach listy rozwijanej w oknie dialogowym Nowy projekt, aby wyświetlić szablon aplikacji usługi Facebook platformie ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Kontroler aktualizacji w czasie rzeczywistym

Szablon aplikacji usługi Facebook umożliwia użytkownikowi łatwe tworzenie kontrolera interfejsu API sieci Web do obsługi aktualizacji w czasie rzeczywistym z usługi Facebook. Jeśli na komputerze deweloperskim jest za translatorem adresów Sieciowych, kontroler może nie działać bez dalszej konfiguracji sieci. Aby uzyskać więcej informacji, zobacz tutaj: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Ciąg wartości będących w konflikcie z usługi Facebook OAuth parametrów zapytania

Następujące pola powodują konflikt z wywołaniem Facebook OAuth okna dialogowego Utwórz kopię adresu URL. Nie należy dodawać własne wartości ciągu zapytania o następujących nazwach: kodu, błąd, błąd\_opis, błąd\_przyczyny.

#### <a name="using-page-inspector-with-facebook-template"></a>Za pomocą narzędzia Page Inspector z szablonem usługi Facebook

Nie można użyć funkcji narzędzie Page Inspector programu Visual Studio 2012 podczas debugowania aplikacji usługi Facebook. Narzędzie Page Inspector aktualnie nie obsługuje ramek IFRAME.

### <a name="single-page-application-template-issues"></a>Szablon aplikacji jednostronicowej problemów

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Z JQuery wprowadź 1.9/odcinania 2.2.1 aktualizacji, podczas uruchamiania domyślny projekt JEDNOSTRONICOWEJ platformy MVC, nowe edycji elementu todo zdarzenie fokus nie jest obsługiwane poprawnie.

Z JQuery 1.9/odcinania 2.2.1 aktualizacji, gdy używany jest domyślny projekt JEDNOSTRONICOWEJ platformy MVC, nowe edycji elementu todo wprowadź już fokus do nowego pola edycji elementu todo po wprowadzeniu nowego elementu todo do listy zadań do wykonania.

Odwołania do rozwiązania [ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html)i wprowadzić poprawki podobne następujący przykładowy kod:

Todo.model.js pliku  
 Funkcja todolist(data), należy dodać następujące:  
 **self.isSelected = ko.observable(false);**

Funkcja todoList.prototype.addTodo, Dodaj poniższy tekst blacked:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Plik index.cshtml, Dodaj poniższy tekst blacked:  
 &lt;tworzą data-bind =&quot;przesyłania: addTodo&quot;&gt;  
 &lt;dane wejściowe klasy =&quot;addTodo&quot; typu =&quot;tekst&quot; wiązania danych =&quot;wartość: newTodoTitle, symbol zastępczy: "Wpisz tutaj", blurOnEnter: ma wartość true, **hasfocus: isSelected**, zdarzenia: {rozmycia: addTodo}&quot; /&gt;  
 &lt;/ Form&gt;
