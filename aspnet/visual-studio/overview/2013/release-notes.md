---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET and Web Tools dla programu Visual Studio 2013 Release Notes | Dokumentacja firmy Microsoft
author: microsoft
description: Ten dokument zawiera opis wersji programu ASP.NET and Web Tools for Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 43878bc101ef97e8bbb6c150f4125707da7660c9
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244960"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET and Web Tools dla programu Visual Studio 2013 Release Notes
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Ten dokument zawiera opis wersji programu ASP.NET and Web Tools for Visual Studio 2013.


## <a name="contents"></a>Spis treści

- [Uwagi dotyczące instalacji](#TOC1)
- [Dokumentacja](#TOC2)
- [Wymagania dotyczące oprogramowania](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nowe funkcje w ASP.NET and Web Tools for Visual Studio 2013

- [One ASP.NET](#TOC6)
- [Nowe środowisko projektu sieci Web](#newproj)
- [Funkcja tworzenia szkieletu ASP.NET](#scaffold)
- [Łączność z przeglądarkami](#browser-link)
- [Ulepszenia edytora sieci Web programu Visual Studio](#web-editor)
- [Obsługa aplikacji sieci Web usługi Azure App Service w programie Visual Studio](#waws)
- [Web Publish ulepszenia](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET Web Forms](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [Biblioteki SignalR platformy ASP.NET](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Składniki Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [ASP.NET App suspend usprawnia](#TOC15)
- [Znane problemy i fundamentalne zmiany](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

ASP.NET and Web Tools for Visual Studio 2013 są powiązane w Instalatorze głównego i można je pobrać [tutaj](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje o ASP.NET and Web Tools for Visual Studio 2013 są dostępne z [witryny sieci web platformy ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Wymagania programowe

ASP.NET and Web Tools wymaga programu Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nowe funkcje w ASP.NET and Web Tools for Visual Studio 2013

W poniższych sekcjach opisano funkcje, które zostały wprowadzone w wersji.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>One ASP.NET

Wraz z wydaniem programu Visual Studio 2013 iż podejmujemy krok na drodze środowisko za pomocą technologii ASP.NET, dzięki czemu można łatwo łączyć i są zgodne z typami, które mają, ujednolicając naukę. Na przykład możesz można utworzyć projekt za pomocą MVC i łatwo później dodać stron formularzy sieci Web do projektu lub tworzenia szkieletu interfejsów API sieci Web w projekcie formularzy sieci Web. One ASP.NET jest ułatwiając dla Ciebie jako programista do wykonywania czynności, które kochasz w programie ASP.NET. Niezależnie od tego, jakich technologii, możesz wybrać możesz mieć pewność, że tworzysz zaufanych podstawowej struktury One ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nowe środowisko projektu sieci Web

Rozszerzyliśmy możliwości tworzenia nowych projektów sieci web w programie Visual Studio 2013. W **nowego projektu sieci Web platformy ASP.NET** okna dialogowego, można wybrać typ projektu, skonfigurować dowolną kombinację technologii (Web Forms, MVC, interfejs API sieci Web), skonfiguruj opcje uwierzytelniania i Dodaj projekt testu jednostkowego.

![Nowy projekt ASP.NET](release-notes/_static/image1.png)

Nowe okno dialogowe umożliwia zmianę domyślne opcje uwierzytelniania dla wielu szablonów. Na przykład po utworzeniu projektu ASP.NET Web Forms można wybrać jedną z następujących opcji:

- Bez uwierzytelniania
- Indywidualne konta użytkowników (członkostwa ASP.NET lub społecznościowych dostawcy logowania)
- Konta organizacji (Active Directory w aplikacji internetowej)
- Windows Authentication (Active Directory w intranecie aplikacji)

![Opcje uwierzytelniania](release-notes/_static/image2.png)

Aby uzyskać więcej informacji na temat nowego procesu do tworzenia projektów sieci web, zobacz [tworzenia projektów sieci Web platformy ASP.NET w programie Visual Studio 2013](creating-web-projects-in-visual-studio.md). Aby uzyskać więcej informacji na temat nowej opcji uwierzytelniania, zobacz [produktu ASP.NET Identity](#TOC8) w dalszej części tego dokumentu.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>Funkcja tworzenia szkieletu ASP.NET

Funkcja tworzenia szkieletu ASP.NET jest struktura generowania kodu dla aplikacji sieci Web ASP.NET. Ułatwia on dodać schematyczny kod służący do projektu, która współdziała z modelu danych.

W poprzednich wersjach programu Visual Studio tworzenia szkieletu była ograniczona do projektów programu ASP.NET MVC. Za pomocą programu Visual Studio 2013 można teraz używać tworzenia szkieletów dla każdego projektu programu ASP.NET, w tym formularzy sieci Web. Visual Studio 2013 aktualnie nie obsługuje generowania strony dla projektu formularzy sieci Web, ale nadal umożliwia tworzenie szkieletów przy użyciu formularzy sieci Web przez dodanie zależności MVC do projektu. Obsługa generowania strony formularzy sieci Web zostanie dodana w przyszłej aktualizacji.

Korzystając z tworzenia szkieletu, Upewniamy się, że wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład jeśli rozpoczęcie projektu programu ASP.NET Web Forms, a następnie dodaj Kontroler interfejsu API sieci Web, korzystając z tworzenia szkieletów, wymagane pakiety NuGet i odwołania są automatycznie dodawane do projektu.

Aby dodać MVC scaffolding projekt formularzy sieci Web, należy dodać **nowy element szkieletu** i wybierz **MVC 5 zależności** w oknie dialogowym. Dostępne są dwie opcje na potrzeby tworzenia szkieletów MVC; Minimalna i pełne. Jeśli wybierzesz przycisku minimalnych tylko pakiety NuGet i odwołania dla platformy ASP.NET MVC są dodawane do projektu. Jeśli wybierzesz opcję pełnej, minimalnym zależności zostaną dodane, a także wymagane pliki zawartości projektu MVC.

Obsługę tworzenia szkieletów kontrolerów async korzysta z nowych funkcji asynchronicznej z platformy Entity Framework 6.

Aby uzyskać więcej informacji i samouczków, zobacz [omówienie tworzenia szkieletu ASP.NET](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Łącze przeglądarki — SignalR kanału między przeglądarką a Visual Studio

Nowy [łączność z przeglądarkami](using-browser-link.md) pozwala połączyć wiele przeglądarek do programu Visual Studio i odświeżanie ich wszystkich, klikając przycisk na pasku narzędzi. Można połączyć wiele przeglądarek do swojej witryny programowania, w tym emulatorów urządzeń mobilnych, a następnie kliknij przycisk odświeżania do odświeżenia wszystkich przeglądarek w wszystko na tym samym czasie. Łączność z przeglądarkami udostępnia również interfejs API, aby umożliwić programistom pisanie rozszerzeń łączy przeglądarki.

![](release-notes/_static/image3.png)

Dzięki czemu deweloperzy mogą korzystać z interfejsu API łącza przeglądarki, możliwe staje się utworzyć bardzo zaawansowane scenariusze, które przecięcia granic między Visual Studio i dowolnej przeglądarce, która jest połączona. Web Essentials korzysta z interfejsu API w celu utworzenia integrację między Visual Studio i narzędzi dla deweloperów w przeglądarce, zdalne kontrolowanie emulatorów urządzeń mobilnych i wiele innych.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Ulepszenia edytora sieci Web programu Visual Studio

Visual Studio 2013 obejmuje nowe edytora HTML w plikach Razor i pliki HTML w aplikacji sieci web. Nowy edytor HTML zawiera jeden schemat ujednoliconego oparte na języku HTML5. Ma ona automatyczne uzupełnianie nawiasów, interfejs użytkownika jQuery i AngularJS atrybutu IntelliSense, atrybutu grupowania IntelliSense, identyfikator i nazwa klasy funkcji Intellisense i innych ulepszeń, w tym lepszą wydajność, formatowania i tagi inteligentne.

Poniższy zrzut ekranu pokazuje, za pomocą atrybutu ładowania funkcji IntelliSense w edytorze HTML.

![Funkcja IntelliSense w edytorze HTML](release-notes/_static/image4.png)

Visual Studio 2013 jest dostarczany, za pomocą obu CoffeeScript i mniej edytory wbudowane. Edytor LESS pochodzi ze świetnymi funkcjami Edytor CSS i ma określonych technologii Intellisense dla zmiennych i domieszki na mniejsze dokumenty w @import łańcucha.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Obsługa aplikacji sieci Web usługi Azure App Service w programie Visual Studio

W programie Visual Studio 2013 z zestawem Azure SDK dla platformy .NET 2.2, można użyć **Eksploratora serwera** użytkownikowi bezpośrednią interakcję z aplikacji sieci web do zdalnego. Możesz zalogować się do konta platformy Azure, tworzyć nowe aplikacje sieci web, skonfigurować aplikacje, wyświetlać w czasie rzeczywistym dzienniki i inne. Przychodzących wkrótce po zestawie SDK 2.2 jest zwalniany, będzie można uruchomić w trybie debugowania zdalnego na platformie Azure. Większość nowych funkcji dla usługi Azure App Service Web Apps działa również w programie Visual Studio 2012 podczas instalowania bieżącej wersji zestawu Azure SDK dla platformy .NET.

Aby uzyskać więcej informacji, zobacz następujące zasoby:

- [Tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Web Publish ulepszenia

Visual Studio 2013 obejmuje nowych i ulepszonych funkcji publikowania w sieci Web. Poniżej przedstawiono niektóre z nich:

- Łatwo [zautomatyzować szyfrowanie plików Web.config](https://go.microsoft.com/fwlink/?LinkId=325529). (Ten link i dwie poniższe polecenie dokumentacji w witrynie MSDN, które mogą być niedostępne do czasu opóźnienia w dniu 10/17.)
- Łatwo [zautomatyzować przełączania aplikacji w trybie offline podczas wdrażania](https://go.microsoft.com/fwlink/?LinkId=325530).
- Konfiguruje narzędzie Web Deploy na [użyj sumy kontrolnej pliku zamiast Data zmiany od ostatniego](https://go.microsoft.com/fwlink/?LinkId=325531) do określania, które pliki powinny zostać skopiowane na serwer.
- Szybkie opublikowanie poszczególnych wybranych plików (w tym pliku Web.config), gdy używasz FTP lub systemu plików publikuje metody, jak również za pomocą narzędzia Web Deploy.

Aby uzyskać więcej informacji na temat wdrażania sieci web platformy ASP.NET, zobacz [witryny ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet w wersji 2.7 zawiera bogaty zestaw nowych funkcji, które opisano szczegółowo w [informacjach o wersji programu NuGet w wersji 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Konieczność zapewnienia jawnego zgodę dotyczącą funkcja przywracania pakietów NuGet pobrać pakiety spowoduje również usunięcie tej wersji programu NuGet. Instalując program NuGet teraz udzielono zgody (i skojarzone pole wyboru w oknie dialogowym Preferencje NuGet). Przywracanie pakietu po prostu działa teraz domyślnie.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Formularze sieci Web ASP.NET

### <a name="one-aspnet"></a>One ASP.NET

Szablony projektu formularzy sieci Web integrują się z nowego środowiska aplikacji One ASP.NET. Możesz dodać MVC i interfejs API sieci Web pomocy technicznej do projektu formularzy sieci Web i można skonfigurować uwierzytelnianie przy użyciu Kreatora tworzenia projektu aplikacji One ASP.NET. Aby uzyskać więcej informacji, zobacz [tworzenia projektów sieci Web platformy ASP.NET w programie Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Szablony projektu formularzy sieci Web obsługi nowej struktury tożsamości ASP.NET. Ponadto szablony obsługuje teraz tworzenia projekt intranet formularzy sieci Web. Aby uzyskać więcej informacji, zobacz [metod uwierzytelniania](creating-web-projects-in-visual-studio.md#auth) w **tworzenia projektów sieci Web platformy ASP.NET w programie Visual Studio 2013**.

### <a name="bootstrap"></a>Usługa ładowania początkowego

Szablony formularzy sieci Web używają [Bootstrap](http://twitter.github.io/bootstrap/) zapewnienie elegancki i elastyczny wyglądu i działania, można łatwo dostosować. Aby uzyskać więcej informacji, zobacz [Bootstrap w szablonach projektu sieci web programu Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Szablony projektów sieci Web MVC bezproblemowo integrować z nowego środowiska aplikacji One ASP.NET. Można dostosować swój projekt MVC i skonfigurować uwierzytelnianie przy użyciu Kreatora tworzenia projektu aplikacji One ASP.NET. Samouczek wprowadzający do ASP.NET MVC 5, można znaleźć w folderze [wprowadzenie do ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Aby uzyskać informacje na temat uaktualniania projektów MVC 4 do MVC 5, zobacz [uaktualnianie ASP.NET MVC 4 i projektu interfejsu Web API platformy ASP.NET MVC 5 i Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Szablony projektów MVC zostały zaktualizowane do użycia produktu ASP.NET Identity do uwierzytelniania i zarządzania tożsamościami. Samouczek uwierzytelniania serwisu Facebook i firmą Google i nowego członkostwa interfejsu API, można znaleźć w folderze [tworzenie aplikacji platformy ASP.NET MVC 5 za pomocą usługi Facebook i Google OAuth2 i OpenID logowanie jednokrotne](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) i [tworzenie aplikacji ASP.NET MVC za pomocą uwierzytelniania i Baza danych SQL i wdrażanie w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Usługa ładowania początkowego

Szablon projektu MVC została zaktualizowana w celu użycia [Bootstrap](http://getbootstrap.com/) zapewnienie elegancki i elastyczny wyglądu i działania, można łatwo dostosować. Aby uzyskać więcej informacji, zobacz [Bootstrap w szablonach projektu sieci web programu Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtry uwierzytelniania

Filtry uwierzytelniania to nowy rodzaj filtru we wzorcu ASP.NET MVC, zaplanowane przed filtry autoryzacji w potoku platformy ASP.NET MVC, która pozwala użytkownikowi na określenie uwierzytelniania logiki akcję, na kontrolerze lub globalnie dla wszystkich kontrolerów. Filtry uwierzytelniania przetwarzania poświadczenia w żądaniu i zapewnienia odpowiedniej jednostki. Filtry uwierzytelniania można również dodać wezwań do uwierzytelnienia w odpowiedzi na nieautoryzowanego żądania.

### <a name="filter-overrides"></a>Przesłonięcia filtru

Możesz teraz zastąpić, które filtry mają zastosowanie do metody danej akcji lub kontrolera, określając filtrem zastępowania. Filtry zastąpienie określają zestaw typów filtrów, które nie powinna być uruchamiana dla danego zakresu (akcji lub kontrolera). Dzięki temu można skonfigurować filtry, które są stosowane globalnie, ale następnie wykluczyć określone filtry globalne zastosowanie do określonych akcji i kontrolerów.

### <a name="attribute-routing"></a>Routing atrybutów

ASP.NET MVC obsługuje teraz trasowanie atrybutów, dzięki udział Tim McCall, Autor [ http://attributerouting.net ](http://attributerouting.net). Z routingiem atrybutów można określić trasy, dodawanie adnotacji do Twojej akcji i kontrolerów.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Routing atrybutów

ASP.NET Web API obsługuje teraz trasowanie atrybutów, dzięki udział Tim McCall, Autor [ http://attributerouting.net ](http://attributerouting.net). Z routingiem atrybutów można określić trasy interfejsu API sieci Web, dodawanie adnotacji do Twojej akcji i kontrolerów w następujący sposób:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Trasowanie atrybutów zapewnia większą kontrolę nad identyfikatory URI w interfejsie API sieci web. Na przykład można łatwo zdefiniować hierarchii zasobów za pomocą jednego kontrolera interfejsu API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Również trasowanie atrybutów zawiera wygodnej składni określania parametrów opcjonalnych, wartości domyślne i ograniczenia trasy:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Aby uzyskać więcej informacji na temat trasowanie atrybutów, zobacz [atrybut routingu w sieci Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Szablony projektów interfejsu API sieci Web i aplikacji jednostronicowej obsługują teraz autoryzację przy użyciu protokołu OAuth 2.0. OAuth 2.0 to architektura służąca do uwierzytelniania klientów dostępu do chronionych zasobów. Działa to w przypadku różnych klientów, w tym przeglądarek i urządzeń przenośnych.

Obsłudze standardu OAuth 2.0 opiera się na nowe oprogramowanie pośredniczące zabezpieczeń udostępniane przez składniki Microsoft OWIN dla uwierzytelniania elementu nośnego i implementowanie roli serwera autoryzacji. Alternatywnie klientów może być autoryzowane przy użyciu serwera autoryzacji w organizacji, takich jak Azure Active Directory lub usług AD FS w systemie Windows Server 2012 R2.

### <a name="odata-improvements"></a>Ulepszenia OData

**Rozwiń węzeł Obsługa $select, $, $batch and $value**

ASP.NET Web API OData ma teraz pełną pomoc techniczną dla $select, $expand and $value. Umożliwia także $batch dla żądania przetwarzania wsadowego i przetwarzania z zestawami zmian.

Opcje $select i $expand umożliwiają zmianę kształtu danych, który jest zwracany z punktu końcowego OData. Aby uzyskać więcej informacji, zobacz [Introducing $select i $rozszerzać obsługę w protokole OData składnika Web API](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Ulepszone rozszerzalności**

Programy formatujące OData są teraz rozszerzonego. Można dodawać metadane wpisu Atom, obsługuje nazwane wpisów łącze strumienia i multimedia, dodawanie adnotacji wystąpienia i dostosowywać, sposobu generowania łączy.

**Obsługa bez typu**

Można teraz tworzyć usługi OData, bez konieczności używania w celu definiowania typów CLR dla swojego typu jednostki. Zamiast tego kontrolerach OData może przejąć lub zwrócić wystąpienia **IEdmObject**, które są programy formatujące OData serializację i deserializację.

**Ponowne użycie istniejącego modelu**

Jeśli masz już istniejące modelu entity data model (EDM) struktury, można teraz wykorzystać go bezpośrednio, zamiast tworzyć nowy. Na przykład jeśli używasz programu Entity Framework, można użyć EDM, który EF tworzy za Ciebie.

### <a name="request-batching"></a>Żądania, przetwarzanie wsadowe

Przetwarzanie wsadowe żądania łączy wiele operacji w pojedyncze żądanie HTTP POST, w celu zmniejszenie ruchu w sieci i zapewnienia płynniejszy, mniej interfejsu użytkownika z dużą liczbą. Web API platformy ASP.NET teraz obsługuje kilka strategii dzielenia na partie żądania:

- Użyj punkt końcowy usługi OData $batch.
- Wiele żądań tworzenia pakietów w pojedynczym żądaniu wieloczęściowej wiadomości MIME.
- Użyj niestandardowego formatu przetwarzania wsadowego.

Aby włączyć żądanie dzielenia na partie, po prostu Dodaj trasę z obsługi przetwarzania wsadowego do konfiguracji interfejsu API sieci Web:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Możesz również kontrolować czy żądań lub wykonywane po kolei lub w dowolnej kolejności.

### <a name="portable-aspnet-web-api-client"></a>Klient interfejsu API sieci Web ASP.NET przenośny

Można teraz używać klienta interfejsu API sieci Web platformy ASP.NET do tworzenia biblioteki klas przenośnych, które działają w aplikacjach Windows Store i Windows Phone 8. Można również utworzyć przenośne elementy formatujące, które mogą być współużytkowane przez klienta i serwera.

### <a name="improved-testability"></a>Ulepszone możliwości testowania

Web API 2 ułatwia ona znacznie łatwiejsze do jednostki testu z kontrolerami interfejsu API. Po prostu Utwórz wystąpienie Kontroler interfejsu API przy użyciu komunikatu żądania i konfiguracji, a następnie wywołaj metodę akcji, którą chcesz przetestować. Jest również łatwe testowanie **UrlHelper** klasy dla metody akcji, wykonujących generowania łączy.

### <a name="ihttpactionresult"></a>IHttpActionResult

Teraz można zaimplementować IHttpActionResult do hermetyzacji wynik metody akcji interfejsu API sieci Web. IHttpActionResult zwrócony z metody akcji interfejsu API sieci Web jest wykonywany przez środowisko uruchomieniowe ASP.NET Web API, który zwróci komunikat odpowiedzi wynikowe. IHttpActionResult może zwracać żadnych czynności interfejsu API sieci Web w celu uproszczenia jednostki testowania implementacji interfejsu API sieci Web. Dla wygody, liczba IHttpActionResult implementacje są udostępniane poza tym wyniki dla zwracania konkretnymi kodami stanu sformatowane zawartości lub negocjowane zawartości odpowiedzi.

### <a name="httprequestcontext"></a>HttpRequestContext

Nowy **HttpRequestContext** śledzi stan, który jest powiązany z żądaniem, ale nie jest natychmiast dostępna z żądania. Na przykład, można użyć **HttpRequestContext** można pobrać danych trasy, podmiot zabezpieczeń skojarzony z tym żądaniem, a certyfikat klienta **UrlHelper** i katalog główny ścieżki wirtualnej. Możesz łatwo tworzyć **HttpRequestContext** jednostki do celów testowych.

Ponieważ podmiot zabezpieczeń dla żądania jest przekazane z żądaniem zamiast polegania na **SE vlastnost Thread.CurrentPrincipal**, podmiot zabezpieczeń jest teraz dostępna w okresie istnienia żądania, gdy są one potok składnika Web API.

### <a name="cors"></a>MECHANIZM CORS

Dzięki rozłożeniu w innym przygotowania doskonałego wkładu z firmy Brock Allen platformy ASP.NET teraz w pełni obsługuje Cross pochodzenia żądania Sharing (CORS).

Poziom zabezpieczeń przeglądarki uniemożliwia strony sieci web wprowadzanie wysyłanie żądań AJAX do innej domeny. [Mechanizm CORS](http://www.w3.org/TR/cors/) jest to standard W3C, dzięki któremu serwer może Poluzować zasady tego samego źródła. Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin jednocześnie odrzucając inne.

Składnik Web API 2 obsługuje teraz CORS, włączając automatyczną obsługę żądań wstępnych. Aby uzyskać więcej informacji, zobacz [Włączanie żądań Cross-Origin w interfejsie API sieci Web platformy ASP.NET](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtry uwierzytelniania

Filtry uwierzytelniania to nowy rodzaj filtru w interfejsie API sieci Web platformy ASP.NET, zaplanowane przed filtry autoryzacji w potoku interfejsu API sieci Web platformy ASP.NET, która pozwala użytkownikowi na określenie uwierzytelniania logiki akcję, na kontrolerze lub globalnie dla wszystkich kontrolerów. Filtry uwierzytelniania przetwarzania poświadczenia w żądaniu i zapewnienia odpowiedniej jednostki. Filtry uwierzytelniania można również dodać wezwań do uwierzytelnienia w odpowiedzi na nieautoryzowanego żądania.

### <a name="filter-overrides"></a>Zastępowanie filtrów

Możesz teraz zastąpić, które filtry mają zastosowanie do metody danej akcji lub kontrolera, określając filtrem zastępowania. Filtry zastąpienie określają zestaw typów filtrów, które nie powinny być uruchamiane dla danego zakresu (akcji lub kontrolera). Dzięki temu można dodać filtry globalne, ale następnie wykluczyć niektóre z określonych akcji i kontrolerów.

### <a name="owin-integration"></a>Integracja z OWIN

Web API platformy ASP.NET teraz w pełni obsługuje OWIN i mogą być uruchamiane na żadnym hoście możliwością OWIN. Dostępna jest także **HostAuthenticationFilter** który zapewnia integrację z systemem uwierzytelniania OWIN.

Dzięki integracji z usługą OWIN interfejsu API sieci Web można hosta samodzielnego we własnym procesie wraz z innym oprogramowaniu pośredniczącym OWIN, takich jak SignalR. Aby uzyskać więcej informacji, zobacz [Użyj OWIN do interfejsu API sieci Web platformy ASP.NET Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

Funkcje w wersji 2.0 biblioteki SignalR można znaleźć w poniższych sekcjach.

- [Oparta na OWIN](#builtonowin)
- [MapHubs i MapConnection są teraz MapSignalR](#MapSignalR)
- [Obsługa wielu domen](#crossdomain)
- [iOS i Android obsługują za pośrednictwem MonoTouch i MonoDroid](#mobile)
- [Klienta przenośnego platformy .NET](#portable)
- [Nowy pakiet hosta samodzielnego](#selfhost)
- [Pomoc techniczna dotycząca serwera zgodne z poprzednimi wersjami](#backwardcompat)
- [Usunięto obsługę serwera dla programu .NET 4.0](#remove40)
- [Wysyłanie komunikatu do listy klientów i grup](#messagelist)
- [Wysyłanie komunikatu do określonego użytkownika](#sendtouser)
- [Lepsze wsparcie obsługi błędów](#errorhandling)
- [Jednostka łatwiejsze testowanie centrów](#unittesting)
- [Obsługa błędów języka JavaScript](#javascripterror)

Na przykład sposobu uaktualniania istniejącego projektu 1.x do SignalR 2.0 zobacz [uaktualnianie SignalR 1.x projektu](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Oparta na OWIN

SignalR w wersji 2.0 jest wbudowana w całkowicie na [OWIN (Otwórz interfejs sieci Web dla platformy .NET)](http://owin.org/). Ta zmiana sprawia, że proces instalacji dla elementu SignalR znacznie bardziej spójną, między hostowanymi w sieci web i samodzielnie hostowanej aplikacji SignalR, ale jest to wymagane również szereg zmian interfejsu API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs i MapConnection są teraz MapSignalR

Zgodność ze standardami OWIN, te metody została zmieniona na `MapSignalR`. `MapSignalR` wywoływana bez parametrów spowoduje mapowanie wszystkich centrów (jako `MapHubs` w wersji 1.x); do mapowania poszczególnych **PersistentConnection** obiektów, określ typ połączenia jako parametr typu i rozszerzenia adresu URL dla połączenia jako pierwszy argument.

`MapSignalR` Metoda jest wywoływana w klasę początkową Owin. Visual Studio 2013 zawiera nowego szablonu dla klasy początkowej Owin; Aby użyć tego szablonu, wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy nad projektem
2. Wybierz **Dodaj**, **nowy element...**
3. Wybierz **Klasa początkowa Owin**. Nadaj nowej klasie **Startup.cs**.

W **aplikacja internetowa,** Owin uruchamiania klasy zawierające `MapSignalR` metody jest dodawane do procesu uruchamiania firmy Owin za pomocą wpisu w węźle Ustawienia aplikacji w pliku Web.Config, jak pokazano poniżej.

W **może być samodzielnie hostowane aplikacji**, klasa początkowa jest przekazywany jako parametr typu `WebApp.Start` metody.

**Mapowanie centra i połączenia w SignalR 1.x (z pliku globalnej aplikacji w aplikacji sieci web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Mapowanie centra i połączenia SignalR w wersji 2.0 (z pliku Klasa początkowa Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

W **może być samodzielnie hostowane aplikacji**, klasa początkowa jest przekazywany jako parametr typu dla `WebApp.Start` metody, jak pokazano poniżej.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Obsługa wielu domen

W SignalR 1.x żądania obejmujące różne domeny była kontrolowana przez pojedynczej flagi EnableCrossDomain. Ta flaga kontrolowane żądania CORS i JSONP. Funkcje i elastyczność, obsługa wszystkich mechanizmu CORS zostało usunięte z składnika serwera SignalR (JavaScript lients nadal CORS normalnie z niego korzystać w przypadku wykrycia, czy przeglądarka obsługuje on), i nowego oprogramowania pośredniczącego OWIN został udostępniony do obsługi tych scenariuszy.

SignalR w wersji 2.0, jeśli JSONP jest wymagany na kliencie (do obsługi żądań między domenami w starszych przeglądarkach), trzeba będzie ją można jawnie włączyć, ustawiając `EnableJSONP` na `HubConfiguration` obiekt `true`, jak pokazano poniżej. JSONP jest domyślnie wyłączony, ponieważ jest to mniej bezpieczna niż mechanizmu CORS.

Aby dodać nowe oprogramowanie pośredniczące CORS SignalR w wersji 2.0, Dodaj `Microsoft.Owin.Cors` biblioteki do projektu, a wywołanie `UseCors` przed oprogramowania pośredniczącego SignalR, jak pokazano w dalszej części tego artykułu.

**Dodawanie Microsoft.Owin.Cors do projektu**: Aby zainstalować tę bibliotekę, uruchom następujące polecenie w konsoli Menedżera pakietów:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

To polecenie spowoduje dodanie 2.0.0 wersji pakietu do projektu.

**Wywoływanie UseCors**

Poniższe fragmenty kodu przedstawiają sposób wykonania połączenia między domenami w SignalR 1.x i w wersji 2.0.

**Implementowanie żądania między domenami w SignalR 1.x (z pliku aplikacji globalnej)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementowanie żądania między domenami w SignalR 2.0 (plik kodu C#)**

Poniższy kod ilustruje sposób włączyć mechanizm CORS i JSONP w projekcie biblioteki SignalR w wersji 2.0. Ten przykładowy kod korzysta `Map` i `RunSignalR` zamiast `MapSignalR`, dzięki czemu oprogramowanie pośredniczące CORS działa tylko w przypadku żądania SignalR, które wymagają obsługi mechanizmu CORS (a nie dla całego ruchu w ścieżce określonej w `MapSignalR`.) `Map` może również służyć do innego oprogramowania pośredniczącego, które musi zostać uruchomiony dla określonego prefiksu adresu URL, a nie dla całej aplikacji.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS i Android obsługują za pośrednictwem MonoTouch i MonoDroid

Dodano obsługę dla systemów iOS i Android klientów przy użyciu składników MonoTouch i MonoDroid z [biblioteki Xamarin](https://xamarin.com/). Aby uzyskać więcej informacji na temat sposobu ich używania, zobacz [składników platformy Xamarin przy użyciu](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Te składniki będzie dostępna w [Store Xamarin](https://store.xamarin.com/) gdy jest dostępna w wersji SignalR RTW.

<a id="portable"></a> ### Przenośny klient modelu .NET

Aby lepiej ułatwienia programowania dla wielu platform, Silverlight, WinRT i Windows Phone klientów zostały zastąpione pojedynczego przenośne klienta platformy .NET, który obsługuje następujące platformy:

- NET 4.5
- Silverlight 5
- WinRT (platforma .NET dla aplikacji Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nowy pakiet hosta samodzielnego

Jest teraz pakiet NuGet, aby ułatwić wprowadzenie Host samodzielny SignalR (SignalR aplikacje, które są hostowane w procesie lub innej aplikacji, a nie jest hostowana na serwerze sieci web). Aby uaktualnić projekt hosta samodzielnego skompilowanych przy użyciu SignalR 1.x, usuń pakiet Microsoft.AspNet.SignalR.Owin i Dodaj pakiet Microsoft.AspNet.SignalR.SelfHost. Aby uzyskać więcej informacji na temat rozpoczynania pracy z pakietem hosta samodzielnego, zobacz [samouczek: Host samodzielny SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Pomoc techniczna dotycząca serwera zgodne z poprzednimi wersjami

W poprzednich wersjach SignalR, wersje pakietu SignalR, używane w obiekcie klienta i serwer musiał być identyczne. W celu obsługi aplikacji gruby klienta, które byłyby trudne do aktualizacji, SignalR w wersji 2.0 obsługuje teraz, przy użyciu starszego klienta przy użyciu nowszej wersji serwera. **Uwaga: SignalR w wersji 2.0 nie obsługuje serwerów utworzonych za pomocą starszych wersji przy użyciu nowszych klientów.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Usunięto obsługę serwera dla programu .NET 4.0

2.0 biblioteki SignalR została obniżona obsługę współdziałania serwera za pomocą programu .NET 4.0. .NET 4.5, należy użyć z serwerami biblioteki SignalR w wersji 2.0. Nadal jest klientem programu .NET 4.0 dla SignalR w wersji 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Wysyłanie komunikatu do listy klientów i grup

SignalR w wersji 2.0 jest możliwe do wysyłania komunikatów przy użyciu listy klienta i identyfikatorów grup. Poniższe fragmenty kodu pokazują, jak to zrobić.

**Wysyłanie komunikatu do listy klientów i grup za pomocą PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Wysyłanie komunikatu do listy klientów i grup za pomocą koncentratory**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Wysyłanie komunikatu do określonego użytkownika

Ta funkcja pozwala użytkownikom na określenie, co to jest identyfikator userId oparte na IRequest za pomocą nowego interfejsu IUserIdProvider:

**Interfejs IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Domyślnie będzie implementacji, który używa IPrincipal.Identity.Name użytkownika jako nazwy użytkownika.

W koncentratorów będziesz mieć możliwość wysyłania wiadomości do wybranych użytkowników za pomocą nowego interfejsu API:

**Za pomocą Clients.User interfejsu API**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Lepsze wsparcie obsługi błędów

Użytkownicy mogą teraz zgłosić **HubException** z dowolnego wywołania koncentratora. Konstruktor obiektu **HubException** może potrwać komunikat w formacie ciągu i obiekt dodatkowe dane dotyczące błędu. SignalR spowoduje automatyczne serializować wyjątku i wysyłać je do klienta, w którym będzie służyć do odrzucania/Niepowodzenie wywołania metody koncentratora.

**Pokaż wyjątkach Centrum szczegółowe** ustawienie nie ma wpływu na **HubException** są wysyłane z powrotem do klienta lub nie; zawsze wysłaniem.

**Kod po stronie serwera, demonstrując wysyłania HubException do klienta**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Kod klienta JavaScript ukazujące odpowiadanie na HubException, wysyłane z serwera**

[!code-html[Main](release-notes/samples/sample16.html)]

**Kod klienta .NET ukazujące odpowiadanie na HubException, wysyłane z serwera**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Jednostka łatwiejsze testowanie centrów

SignalR w wersji 2.0 obejmuje interfejs o nazwie `IHubCallerConnectionContext` w centrach, które ułatwia tworzenie wywołania po stronie klienta makiety. Poniższe fragmenty kodu ukazują, przy użyciu tego interfejsu z popularnych test, wiązka [xUnit.net](https://github.com/xunit/xunit) i [moq](https://code.google.com/p/moq/).

**Testowanie jednostek biblioteki SignalR za pomocą xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Testowanie jednostek biblioteki SignalR za pomocą moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Obsługa błędów języka JavaScript

SignalR w wersji 2.0 wszystkie wywołania zwrotne obsługi błędów języka JavaScript zwraca błąd JavaScript — obiekty zamiast ciągów raw. Dzięki temu SignalR bogatsze informacji do usługi obsługi błędów. Możesz uzyskać wewnętrzny wyjątek z `source` właściwości błędu.

**Kod klienta JavaScript, który obsługuje wyjątek Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nowy System członkostwa platformy ASP.NET

ASP.NET Identity jest nowy system członkostwa aplikacji ASP.NET. ASP.NET Identity ułatwia integrowanie danych profilu określonego użytkownika z danych aplikacji. ASP.NET Identity umożliwia również wybierz model stanów trwałych dla profilów użytkowników w aplikacji. Dane można przechowywać w bazie danych programu SQL Server lub innym magazynem danych, w tym magazynów danych NoSQL, takie jak tabele magazynu platformy Azure. Aby uzyskać więcej informacji, zobacz [indywidualne konta użytkowników](creating-web-projects-in-visual-studio.md#indauth) w **tworzenia projektów sieci Web platformy ASP.NET w programie Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Uwierzytelnianie oparte na oświadczeniach

Program ASP.NET obsługuje teraz uwierzytelnianie oparte na oświadczeniach, gdzie tożsamość użytkownika jest reprezentowana jako zestaw oświadczeń z zaufanych wystawców. Użytkownicy mogą być uwierzytelniony przy użyciu nazwy użytkownika i hasła przechowywane w bazie danych aplikacji lub dostawców tożsamości społecznościowych (na przykład: Accounts firmy Microsoft, Facebook, Google i Twitter), lub przy użyciu konta organizacji za pomocą usługi Azure Active Directory lub Usługi Active Directory Federation Services (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integracja z usługą Azure Active Directory i usługi Active Directory systemu Windows Server

Można teraz tworzyć projekty ASP.NET, korzystające z usługi Azure Active Directory lub Windows Server Active Directory (AD) do uwierzytelniania. Aby uzyskać więcej informacji, zobacz [kont organizacyjnych](creating-web-projects-in-visual-studio.md#orgauth) w **tworzenia projektów sieci Web platformy ASP.NET w programie Visual Studio 2013**.

### <a name="owin-integration"></a>Integracja z OWIN

Uwierzytelnianie ASP.NET teraz opiera się na oprogramowania pośredniczącego OWIN, które mogą być używane na dowolnego hosta, na podstawie OWIN. Aby uzyskać więcej informacji na temat OWIN, zapoznaj się z poniższymi [składniki programu Microsoft OWIN](#TOC7) sekcji.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Składniki Microsoft OWIN

[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (OWIN) definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web. OWIN oddziela aplikacji sieci web z serwera, dzięki czemu niezainteresowana hosta aplikacji sieci web. Można na przykład hostowanie aplikacji sieci web opartych na OWIN w usługach IIS lub samodzielnego hostowania w procesie niestandardowym.

Zmiany wprowadzone w składniki Microsoft OWIN (znany także jako projektu Katana) obejmują nowe składników serwera i hosta, nowe biblioteki pomocnika i oprogramowanie pośredniczące i nowego oprogramowania pośredniczącego uwierzytelniania.

Aby uzyskać więcej informacji na temat OWIN i Katana, zobacz [What's new in OWIN i Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Uwaga: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) nie mogą być uruchomione w trybie klasycznym IIS; musi działać w trybie zintegrowanym.**

**Uwaga: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplikacje muszą działać w trybie pełnego zaufania.**

### <a name="new-servers-and-hosts"></a>Nowe serwery i hosty

W tej wersji nowe składniki zostały dodane do samodzielnego hostowania scenariuszy. Te składniki obejmują następujące pakiety NuGet:

- **Microsoft.Owin.Host.HttpListener**. Udostępnia serwer OWIN, który używa **HttpListener** aby nasłuchiwała żądań HTTP i kierować je do potoku OWIN.
- **Microsoft.Owin.Hosting** zawiera biblioteki dla deweloperów, którzy chcą hosta samodzielnego potoku OWIN w procesie niestandardowym, takich jak aplikacji konsoli lub usługę Windows.
- **OwinHost**. Udostępnia autonomiczny plik wykonywalny, który otacza `Microsoft.Owin.Hosting` i umożliwia hosta samodzielnego potoku OWIN bez konieczności pisania aplikacja niestandardowego hosta.

Ponadto `Microsoft.Owin.Host.SystemWeb` pakiet umożliwia oprogramowaniu pośredniczącym, aby zapewnić wskazówki **SystemWeb** serwera, wskazujący, że oprogramowanie pośredniczące powinna być wywoływana podczas określonego etapu potoku ASP.NET. Ta funkcja jest szczególnie użyteczna w przypadku oprogramowania pośredniczącego uwierzytelniania, który należy uruchamiać na wczesnym etapie w potoku ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Pomocnik bibliotek i oprogramowania pośredniczącego

Mimo że można napisać składników OWIN za pomocą tylko funkcji i typ definicje ze specyfikacji OWIN nowy `Microsoft.Owin` pakiet zawiera zestaw abstrakcje bardziej przyjazny dla użytkownika. Ten pakiet łączy kilka starszych pakietów (np. `Owin.Extensions`, `Owin.Types`) do modelu jednej, dobrze ze strukturą obiektu, który następnie może być bez problemów używany przez inne składniki OWIN. W rzeczywistości większość składniki Microsoft OWIN teraz używać tego pakietu.

> [!NOTE]
> [OWIN](http://www.owin.org) nie mogą być uruchomione w trybie klasycznym IIS; musi działać w trybie zintegrowanym.

> [!NOTE]
> [OWIN](http://www.owin.org) aplikacje muszą działać w trybie pełnego zaufania.

Ta wersja zawiera również pakiet Microsoft.Owin.Diagnostics, który zawiera oprogramowanie pośredniczące do sprawdzania poprawności uruchomionej aplikacji OWIN, a także stronę błędu oprogramowaniu pośredniczącym, aby ułatwić badanie błędów.

### <a name="authentication-components"></a>Składniki uwierzytelniania

Dostępne są następujące składniki uwierzytelniania.

- **Microsoft.Owin.Security.ActiveDirectory**. Umożliwia uwierzytelnianie przy użyciu usług katalogowych w środowisku lokalnym i w chmurze.
- **Microsoft.Owin.Security.Cookies** umożliwia uwierzytelnianie przy użyciu plików cookie. Poprzednia nazwa tego pakietu `Microsoft.Owin.Security.Forms`.
- **Microsoft.Owin.Security.Facebook** umożliwia uwierzytelnianie przy użyciu usługi w usłudze Facebook OAuth.
- **Microsoft.Owin.Security.Google** umożliwia uwierzytelnianie za pomocą usługi Google OpenID.
- **Microsoft.Owin.Security.Jwt** umożliwia uwierzytelnianie przy użyciu tokenów JWT.
- **Microsoft.Owin.Security.MicrosoftAccount** umożliwia uwierzytelnianie za pomocą konta Microsoft.
- **Microsoft.Owin.Security.OAuth**. Udostępnia serwer autoryzacji OAuth, a także oprogramowanie pośredniczące dla uwierzytelniania elementu nośnego tokenów.
- **Microsoft.Owin.Security.Twitter** umożliwia uwierzytelnianie przy użyciu usługi w usłudze Twitter OAuth.

Ta wersja zawiera również `Microsoft.Owin.Cors` pakiet, który zawiera oprogramowanie pośredniczące do przetwarzania żądań HTTP cross-origin.

> [!NOTE]
> W ostatecznej wersji programu Visual Studio 2013 usunięciu obsługę podpisywania tokenu JWT.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Aby uzyskać listę nowych funkcji i innych zmian wprowadzonych w programie Entity Framework 6, zobacz [Historia wersji programu Entity Framework](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 obejmuje następujące nowe funkcje:

- Obsługa kartę do edycji. Wcześniej **Formatuj dokument** polecenia, automatycznie wcięcia i automatycznego formatowania w programie Visual Studio nie działały prawidłowo przy użyciu **zachować karty** opcji. Ta zmiana poprawia formatowania dla kodu Razor dla karty formatowania programu Visual Studio.
- Obsługa reguł ponownego zapisywania adresów URL podczas generowania łączy.
- Usuwanie atrybutu przezroczyste dla zabezpieczeń.
  > [!NOTE]
  > Zmiana powodująca niezgodność i sprawia, że Razor 3 niezgodne z MVC4 i starszych aparatu Razor 2 jest niezgodne z MVC5 lub zestawy skompilowane MVC5.

Razor 3 problemy rozwiązane w programie Visual Studio 2013 z wersji wstępnych można znaleźć [tutaj](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET App suspend usprawnia

ASP.NET App Suspend to funkcja ogromna zmiana podejścia w .NET Framework 4.5.1 znacznie zmienia środowisko użytkownika i ekonomiczne model hostowania dużą liczbę witryn platformy ASP.NET na jednym komputerze. Aby uzyskać więcej informacji, zobacz [ASP.NET App Suspend — dynamiczne udostępnione hosting sieci web platformy .NET](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

W tej sekcji opisano znane problemy i przełomowe zmiany w ASP.NET and Web Tools for Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Nowe Przywracanie pakietu nie działa w przypadku platformy Mono, korzystając z pliku SLN](https://nuget.codeplex.com/workitem/3596) — problem zostanie rozwiązany w przyszłych nuget.exe pobierania i [pakietu NuGet.CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) aktualizacji.
- [Nowe Przywracanie pakietu nie działa z projektami Wix](https://nuget.codeplex.com/workitem/3598) — problem zostanie rozwiązany w przyszłych nuget.exe pobierania i [pakietu NuGet.CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) aktualizacji.
- [Automatyczne przywracanie pakietu nie działa dla projektów w folderze rozwiązania](https://nuget.codeplex.com/workitem/3625) — problem zostanie rozwiązany w NuGet 2.8.

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` nie zwraca `IQueryable<T>` zawsze jako Dodaliśmy obsługę `$select` i `$expand`.

    Nasze wcześniejsze przykłady dla `ODataQueryOptions<T>` zawsze rzutować wartość zwracana z `ApplyTo` do `IQueryable<T>`. To już wcześniej, ponieważ zapytanie opcje obsługiwane wcześniej (`$filter`, `$orderby`, `$skip`, `$top`) nie należy zmieniać kształt zapytania. Teraz, gdy obsługujemy `$select` i `$expand` wartość zwrotną z elementu `ApplyTo` nie będzie `IQueryable<T>` zawsze.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Jeśli używasz przykładowego kodu z wcześniejszego jej będą działać, jeśli klient nie będzie przesyłał `$select` i `$expand`. Jednakże jeśli chcesz obsługiwać `$select` i `$expand` należy zmienić ten kod do tego.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url lub RequestContext.Url ma wartość null podczas żądania wsadowego**

    W przypadku łączenia we wsady **UrlHelper** ma wartość null, gdy dostępne z **Request.Url** lub **RequestContext.Url**.

    Ten problem jest obecnie śledzone tutaj: [BatchRequestContext.Url ma wartość null dla dzielenia na partie żądania](http://aspnetwebstack.codeplex.com/workitem/1301).

    Obejście tego problemu jest utworzenie nowego wystąpienia **UrlHelper**, jak w poniższym przykładzie:

    **Tworzenie nowego wystąpienia UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Korzystając z MVC5 i OrgAuth, jeśli używane są widoki weryfikacji AntiForgerToken to zrobić, gdy opublikujesz dane do widoku mogą pochodzić między następujący błąd:

    **Błąd**:

    *Błąd serwera w aplikacji» /».*

    <em>Oświadczenie typu "<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>"lub"<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>" nie istnieje w podanej ClaimsIdentity. Aby włączyć obsługę token zabezpieczający przed sfałszowaniem przy użyciu uwierzytelniania opartego na oświadczeniach, sprawdź, czy dostawcy oświadczeń skonfigurowanej jest zapewnienie oba te oświadczenia w wystąpieniach ClaimsIdentity, który generuje. Jeśli dostawcy oświadczeń skonfigurowanej zamiast tego używa na inny typ oświadczenia jako unikatowy identyfikator, można skonfigurować, ustawiając właściwość statyczna AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Obejście**:

    W pliku Global.asax, aby rozwiązać ten problem, należy dodać następujący wiersz:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Ten problem zostanie rozwiązany w następnej wersji.
2. Po uaktualnieniu aplikacji MVC4 do MVC5, Skompiluj rozwiązanie, a następnie uruchom go. Powinien zostać wyświetlony następujący błąd:

    [A] Nie można rzutować System.Web.WebPages.Razor.Configuration.HostSection [B]System.Web.WebPages.Razor.Configuration.HostSection. Type A originates from 'System.Web.WebPages.Razor, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'. Type B originates from 'System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.

    Aby naprawić błąd powyżej, otwórz *wszystkich* plików Web.config (łącznie z tymi w folderze widoków) w projekcie i wykonaj następujące czynności:

   1. Zaktualizuj wszystkie wystąpienia wersji "4.0.0.0" "System.Web.Mvc" do "5.0.0.0".
   2. Zaktualizuj wszystkie wystąpienia wersji "2.0.0.0" "System.Web.Helpers" &quot;System.Web.WebPages&quot; i &quot;System.Web.WebPages.Razor&quot; do "3.0.0.0"

      Na przykład po wprowadzeniu zmian powyżej powiązania zestawu powinna wyglądać następująco:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Aby uzyskać informacje na temat uaktualniania projektów MVC 4 do MVC 5, zobacz [uaktualnianie ASP.NET MVC 4 i projektu interfejsu Web API platformy ASP.NET MVC 5 i Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Jeśli weryfikacja po stronie klienta z jQuery sprawdzania poprawności dyskretnego kodu, komunikat sprawdzania poprawności czasami jest niepoprawny dla elementu input języka HTML, z typem = "number". Błąd sprawdzania poprawności dla wymaganej wartości ("okres ważności pole jest wymagane") jest wyświetlany po wprowadzeniu nieprawidłową liczbę zamiast poprawne komunikat, że wymagana jest prawidłowa liczba.

    Tego problemu znajduje się zwykle z utworzony szkielet kodu dla modelu z właściwością liczby całkowitej w widokach tworzyć i edytować.

    Aby obejść ten problem, zmień pomocy edytora od:

    `@Html.EditorFor(person => person.Age)`

    Do:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 nie obsługuje już częściowej relacji zaufania. Łączenie plików binarnych MVC lub interfejsu WebAPI projektów, należy usunąć [atrybutów SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) atrybutu i [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) atrybutu. Usuwania tych atrybutów zostanie całkowicie wyeliminować błędy kompilatora, takich jak poniżej.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Uwaga: jako efekt uboczny to Ty nie można używać zestawów 4.0 i 5.0 w tej samej aplikacji. Należy zaktualizować je wszystkie w wersji 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>SPA szablon z serwisu Facebook autoryzacji może spowodować niestabilność w programie Internet Explorer, gdy witryna sieci web jest hostowana w strefie intranetu

Szablon SPA zawiera zewnętrzne Zaloguj się za pomocą usługi Facebook. W przypadku projektów utworzonych za pomocą szablonu działa lokalnie, logowania może spowodować IE awarię.

Rozwiązanie:

1. Hostowanie witryny sieci web w strefie internet; lub

2. Testowanie scenariusza w przeglądarce niż programu Internet Explorer.

### <a name="web-forms-scaffolding"></a>Formularze sieci Web, tworzenia szkieletów

Tworzenie szkieletu formularzy sieci Web została usunięta z VS2013 i będzie dostępna w przyszłej aktualizacji programu Visual Studio. Można jednak nadal używać tworzenia szkieletów w obrębie projektu formularzy sieci Web przez dodanie zależności MVC i generowania szkieletu dla platformy MVC. Projekt będzie zawierać kombinację MVC i formularzy sieci Web.

Aby dodać projekt formularzy sieci Web MVC, Dodaj nowy element szkieletu i wybierz **MVC 5 zależności**. Wybierz minimalny lub pełną w zależności od tego, czy należy wszystkie pliki zawartości, takie jak skrypty. Następnie dodaj element szkieletowy dla platformy MVC, które spowoduje utworzenie widoków i kontrolerów w projekcie.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC i Web interfejsu API tworzenia szkieletów - HTTP 404 Nie znaleziono błąd

Jeśli błąd występuje w przypadku Dodawanie elementu szkieletu do projektu, jest to możliwe, projekt zostanie pozostawiony w stanie niespójnym. Niektóre zmiany można szkieletu zostanie wycofana, ale inne zmiany, takie jak zainstalowane pakiety NuGet zostaną nie można wycofać. Jeśli routingu zmiany konfiguracji zostaną wycofane, użytkownicy otrzymają błąd HTTP 404 podczas przechodzenia do szkieletu elementów.

Obejście problemu:

- Aby naprawić ten błąd dla platformy MVC, Dodaj nowy element szkieletu, a następnie wybierz zależności programu MVC 5 (minimalny lub pełna). Ten proces doda wszystkie wymagane zmiany do swojego projektu.
- Aby naprawić ten błąd interfejsu API sieci Web:

  1. Dodawanie klasy WebApiConfig do projektu.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Konfigurowanie WebApiConfig.Register w aplikacji\_Start metoda w pliku Global.asax w następujący sposób:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
