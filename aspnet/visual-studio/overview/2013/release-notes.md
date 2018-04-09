---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013 informacje o wersji | Dokumentacja firmy Microsoft
author: microsoft
description: Ten dokument zawiera opis wersji platformy ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: e9ddd96f186564834ff6bb2c30cf0ed5444cbf1b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013 informacje o wersji
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Ten dokument zawiera opis wersji platformy ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013.


## <a name="contents"></a>Spis treści

- [Informacje o instalacji](#TOC1)
- [Dokumentacja](#TOC2)
- [Wymagania dotyczące oprogramowania](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nowe funkcje programu ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013

- [One ASP.NET](#TOC6)
- [Nowe środowisko projektu sieci Web](#newproj)
- [ASP.NET Scaffolding](#scaffold)
- [Łączność z przeglądarkami](#browser-link)
- [Ulepszenia edytora sieci Web programu Visual Studio](#web-editor)
- [Obsługa aplikacji sieci Web usługi aplikacji Azure w programie Visual Studio](#waws)
- [Ulepszenia publikowania w sieci Web](#publish)
- [NuGet 2.7](#nuget)
- [Formularze sieci Web ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [Biblioteka SignalR platformy ASP.NET](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Składniki Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [Wstrzymać aplikacja ASP.NET](#TOC15)
- [Znane problemy i fundamentalne zmiany](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Informacje o instalacji

ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013 są powiązane w Instalatorze głównego i może zostać pobrany [tutaj](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje dotyczące platformy ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013 są dostępne z [witryny sieci web ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Wymagania programowe

ASP.NET i narzędzia sieci Web wymaga programu Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nowe funkcje programu ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013

W poniższych sekcjach opisano funkcje, które zostały wprowadzone w wersji.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Jeden ASP.NET

Wraz z wydaniem programu Visual Studio 2013 przekierowaliśmy krok na drodze jednorodnej obsługi programu za pomocą technologii ASP.NET, dzięki czemu można łatwo mieszać i dopasowywać te, które chcesz. Na przykład możesz można uruchomić projektu przy użyciu MVC i łatwo później dodać stron formularzy sieci Web do projektu lub utworzyć szkielet interfejsów API sieci Web w projekcie formularzy sieci Web. Jeden ASP.NET jest ułatwienie dla Ciebie jako deweloper, aby wykonać czynności, które lubisz w programie ASP.NET. Niezależnie od tego, jakie możesz wybrać technologii może mieć pewność, że tworzysz zaufanych podstawowej struktury programu ASP.NET jeden.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nowe środowisko projektu sieci Web

Firma Microsoft rozszerzoną środowisko tworzenia nowych projektów sieci web w programie Visual Studio 2013. W **nowego projektu sieci Web ASP.NET** okna dialogowego można wybrać typ projektu, skonfigurować dowolną kombinację technologii (Web Forms, MVC, Web API), skonfiguruj opcje uwierzytelniania i Dodaj jednostkowy projekt testowy.

![Nowy projekt ASP.NET](release-notes/_static/image1.png)

Nowe okno dialogowe umożliwia zmianę domyślne opcje uwierzytelniania dla wielu szablonów. Na przykład podczas tworzenia projektu formularzy sieci Web programu ASP.NET można wybrać jedną z następujących opcji:

- Bez uwierzytelniania
- Indywidualne konta użytkowników (członkostwa ASP.NET lub społecznościowych dostawcy logowania)
- Konta organizacyjne (Active Directory w aplikacji internetowej)
- Uwierzytelnianie systemu Windows (Active Directory w intranecie aplikacji)

![Opcje uwierzytelniania](release-notes/_static/image2.png)

Aby uzyskać więcej informacji na temat nowego procesu tworzenia projektów sieci web, zobacz [tworzenia projektów sieci Web ASP.NET w programie Visual Studio 2013](creating-web-projects-in-visual-studio.md). Aby uzyskać więcej informacji na temat nowej opcji uwierzytelniania, zobacz [ASP.NET Identity](#TOC8) dalszej części tego dokumentu.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

Rusztowania ASP.NET to platforma generowania kodu dla aplikacji sieci Web ASP.NET. Ułatwia on dodać schematyczny kod służący do projektu, który współdziała z modelem danych.

W poprzednich wersjach programu Visual Studio szkieletów została ograniczona do projektów platformy ASP.NET MVC. Z programu Visual Studio 2013 można teraz używać szkieletów dla żadnego projektu ASP.NET, w tym formularzy sieci Web. Visual Studio 2013 aktualnie nie obsługuje generowania stron dla projektu formularzy sieci Web, ale można nadal używać szkieletów z formularzy sieci Web, dodając zależności MVC do projektu. Obsługa generowania strony formularzy sieci Web zostanie dodana w przyszłej aktualizacji.

Jeśli przy użyciu funkcji szkieletów, Upewniamy się, że wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład uruchomienie z projektem formularzy sieci Web ASP.NET i następnie dodać Kontroler interfejsu API sieci Web za pomocą funkcja szkieletów, wymagane pakiety NuGet i odwołania są automatycznie dodawane do projektu.

Aby dodać szkieletów MVC do projektu formularzy sieci Web, Dodaj **nowy element szkieletu** i wybierz **zależności MVC 5** w oknie dialogowym. Dostępne są dwie opcje do tworzenia szkieletu MVC; Minimalne i pełne. W przypadku wybrania minimalny, tylko pakiety NuGet i odwołań dla platformy ASP.NET MVC zostaną dodane do projektu. Jeśli wybierzesz opcję pełne, minimalnym zależności zostaną dodane, a także wymagane pliki zawartości projektu MVC.

Obsługę tworzenia szkieletu kontrolerów async korzysta z nowych funkcji asynchronicznych z programu Entity Framework 6.

Aby uzyskać więcej informacji i samouczki, zobacz [omówienie szkieletów ASP.NET](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Łącze przeglądarki — SignalR kanału między przeglądarką i Visual Studio

Nowy [łącze przeglądarki](using-browser-link.md) funkcja umożliwia łączenie wielu przeglądarek do programu Visual Studio i odświeżanie ich wszystkich, klikając przycisk na pasku narzędzi. Możesz łączyć wiele przeglądarek do witryny programowanie, włączając emulatorów urządzeń mobilnych i kliknij przycisk Odśwież, aby odświeżania wszystkie przeglądarki wszystko na tym samym czasie. Łącze przeglądarki udostępnia również interfejs API umożliwiają deweloperom pisanie rozszerzeń łączy przeglądarki.

![](release-notes/_static/image3.png)

Umożliwia deweloperom korzystać z interfejsu API łącza przeglądarki, będzie można utworzyć bardzo zaawansowane scenariusze przecięcia granic między Visual Studio i dowolnej przeglądarki, która jest połączona. Podstawowe informacje dotyczące sieci Web korzysta z interfejsu API, aby utworzyć integrację między Visual Studio i narzędzi deweloperskich w przeglądarce, zdalne kontrolowanie emulatorów urządzeń mobilnych i wiele innych.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Ulepszenia edytora sieci Web programu Visual Studio

Visual Studio 2013 zawiera nowe edytora HTML, Razor, plików i plików HTML w aplikacji sieci web. Nowe edytora HTML zawiera jeden schemat ujednoliconego oparty na HTML5. Zawiera nawias klamrowy automatycznego uzupełniania, interfejsu użytkownika jQuery i AngularJS atrybutu IntelliSense, atrybut IntelliSense Grouping, identyfikator i nazwę klasy Intellisense i inne ulepszenia włącznie lepszą wydajność, formatowanie i tagi inteligentne.

Poniższy zrzut ekranu pokazuje, przy użyciu atrybutu Bootstrap IntelliSense w edytorze HTML.

![IntelliSense w edytorze HTML](release-notes/_static/image4.png)

Visual Studio 2013 również pochodzi z obu CoffeeScript mniej edytory wbudowane. Edytor LESS pochodzi ze świetnymi funkcjami z Edytor CSS i ma szczególne Intellisense dla zmiennych i mixins dla wszystkich mniej dokumentów w @import łańcucha.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Obsługa aplikacji sieci Web usługi aplikacji Azure w programie Visual Studio

W programie Visual Studio 2013 z zestawem Azure SDK dla platformy .NET 2.2, można użyć **Eksploratora serwera** na bezpośrednią interakcję z aplikacji sieci web do zdalnego. Możesz zalogować się do konta platformy Azure, tworzenie nowej aplikacji sieci web, konfigurowanie aplikacji, wyświetlać w czasie rzeczywistym dzienniki i inne. Zwolnieniu będzie wkrótce po zestawie SDK, 2.2, można uruchomić w trybie debugowania zdalnego na platformie Azure. Większość nowych funkcji dla aplikacji sieci Web usługi aplikacji Azure również działać w programie Visual Studio 2012 podczas instalowania bieżącej wersji zestawu Azure SDK dla platformy .NET.

Aby uzyskać więcej informacji, zobacz następujące zasoby:

- [Tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Rozwiązywanie problemów z aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Ulepszenia publikowania w sieci Web

Visual Studio 2013 udostępnia nowych i ulepszonych funkcji publikowania w sieci Web. Oto niektóre z nich:

- Łatwe [zautomatyzować szyfrowania plików Web.config](https://go.microsoft.com/fwlink/?LinkId=325529). (Ten link i dwa następujące polecenie dokumentacji w witrynie MSDN, które mogą nie być dostępne dopiero w dniu 10/17.)
- Łatwe [zautomatyzować tworzenie aplikacji w tryb offline podczas wdrażania](https://go.microsoft.com/fwlink/?LinkId=325530).
- Konfiguruje narzędzie Web Deploy do [użyj sumy kontrolnej plików zamiast Data ostatniej zmiany](https://go.microsoft.com/fwlink/?LinkId=325531) do określenia, które pliki powinien zostać skopiowany do serwera.
- Szybkie publikowanie poszczególnych wybranych plików (w tym pliku Web.config), gdy używasz FTP lub metody publikowania w systemie plików, a także z narzędzia Web Deploy.

Aby uzyskać więcej informacji dotyczących wdrażania sieci web ASP.NET, zobacz [witryny ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 zawiera bogaty zestaw nowych funkcji, które opisano szczegółowo w [informacje o wersji 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Ta wersja programu NuGet spowoduje również usunięcie trzeba podać wyraźnej zgody dla funkcji Przywracanie pakietu NuGet na pobieranie pakietów. Teraz udzielono zgody (i skojarzone pole wyboru w oknie dialogowym Preferencje NuGet), instalując NuGet. Przywracanie pakietu po prostu działa teraz domyślnie.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Formularze sieci Web ASP.NET

### <a name="one-aspnet"></a>Jeden ASP.NET

Szablony projektów formularzy sieci Web integrują się z nowego środowiska ASP.NET jeden. Możesz dodać obsługę MVC i interfejsu API sieci Web projektu formularzy sieci Web i można skonfigurować uwierzytelnianie przy użyciu Kreatora tworzenia projektu ASP.NET jeden. Aby uzyskać więcej informacji, zobacz [tworzenia projektów sieci Web ASP.NET w programie Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Szablony projektów formularzy sieci Web obsługi nowej struktury ASP.NET Identity. Ponadto szablony obsługuje teraz tworzenia projekt intranet formularzy sieci Web. Aby uzyskać więcej informacji, zobacz [metod uwierzytelniania](creating-web-projects-in-visual-studio.md#auth) w **tworzenia projektów sieci Web ASP.NET w programie Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Szablony formularzy sieci Web używają [Bootstrap](http://twitter.github.io/bootstrap/) zapewnienie elegancki i elastyczny wyglądu i działania, które można łatwo dostosować. Aby uzyskać więcej informacji, zobacz [Bootstrap w szablonach projektu sieci web programu Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Jeden ASP.NET

Szablony projektów sieci Web MVC integrują się z nowego środowiska ASP.NET jeden. Można dostosować projekt MVC i skonfigurować uwierzytelnianie przy użyciu Kreatora tworzenia projektu ASP.NET jeden. Samouczek wprowadzający do platformy ASP.NET MVC 5 można znaleźć w folderze [wprowadzenie do platformy ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Aby uzyskać informacje o uaktualnianiu projektów MVC 4 do MVC 5, zobacz [sposób uaktualnienia programu ASP.NET MVC 4 i projekt interfejsu API sieci Web platformy ASP.NET MVC 5 i Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Szablony projektów MVC zostały zaktualizowane do korzystania z tożsamości ASP.NET do uwierzytelniania i zarządzania tożsamościami. Samouczek uwierzytelniania serwisu Facebook i Google i nowy interfejs API członkostwa można znaleźć w folderze [tworzenie aplikacji ASP.NET MVC 5 z usługi Facebook i Google OAuth2 i OpenID logowania jednokrotnego](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) i [tworzenie aplikacji ASP.NET MVC z uwierzytelniania i Bazy danych SQL i wdrożyć w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

Szablon projektu MVC została zaktualizowana w celu użycia [Bootstrap](http://getbootstrap.com/) zapewnienie elegancki i elastyczny wyglądu i działania, które można łatwo dostosować. Aby uzyskać więcej informacji, zobacz [Bootstrap w szablonach projektu sieci web programu Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtry uwierzytelniania

Filtry uwierzytelniania są nowym typem filtru na platformie ASP.NET MVC przed filtry autoryzacji w potoku platformy ASP.NET MVC, umożliwiają określenie uwierzytelniania logiki na działania, które na kontroler lub globalnie do wszystkich kontrolerów. Filtry uwierzytelniania przetworzyć poświadczeń w żądaniu i podaj odpowiednie podmiot zabezpieczeń. Filtry uwierzytelniania można również dodać wezwań do uwierzytelnienia w odpowiedzi do nieautoryzowanego żądania.

### <a name="filter-overrides"></a>Zastępuje filtru

Teraz można zastąpić, które filtry mają zastosowanie do metody danej akcji lub kontrolera, określając filtrem zastępowania. Filtry zastąpienie określ zestaw typów filtrów, które nie powinny być uruchamiane dla danego zakresu (akcji lub kontrolera). Dzięki temu można skonfigurować filtry, które są stosowane globalnie, ale następnie wykluczyć określone filtry globalne mające zastosowanie do określonych akcji i kontrolerów.

### <a name="attribute-routing"></a>Atrybut routingu

ASP.NET MVC obsługuje teraz atrybutu routingu, dzięki użyciu udział Timowi McCall, Autor [ http://attributerouting.net ](http://attributerouting.net). Atrybut routingu, można określić trasy przez dodawanie adnotacji do Twojej akcji i kontrolerów.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Atrybut routingu

Interfejs API sieci Web platformy ASP.NET obsługuje teraz atrybutu routingu, dzięki użyciu udział Timowi McCall, Autor [ http://attributerouting.net ](http://attributerouting.net). Atrybut routingu, można określić trasy interfejsu API sieci Web przez dodawanie adnotacji do Twojej akcji i kontrolerów w następujący sposób:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Atrybut routingu zapewnia większą kontrolę nad identyfikatory URI w interfejsie API sieci web. Na przykład można łatwo zdefiniować hierarchii zasobów za pomocą jednego kontrolera interfejsu API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Atrybut routingu również zawiera wygodny składni w celu określenia następujące parametry opcjonalne, wartości domyślne i ograniczenia trasy:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Aby uzyskać więcej informacji na temat trasami atrybutów, zobacz [atrybutu routingu w sieci Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Szablony projektu interfejsu API sieci Web i jednej strony aplikacji obsługuje teraz autoryzację za pomocą protokołu OAuth 2.0. OAuth 2.0 to struktura służąca do autoryzowania dostępu klienta do chronionych zasobów. Działa on dla wielu klientów, w tym przeglądarki i urządzenia przenośne.

Obsługa protokołu OAuth 2.0 jest oparta na nowe oprogramowanie pośredniczące zabezpieczeń dostarczane przez składniki Microsoft OWIN dla uwierzytelniania elementu nośnego i implementowania roli serwera autoryzacji. Alternatywnie klientów może być upoważnionych przy użyciu serwera autoryzacji w organizacji, takich jak Azure Active Directory lub usług AD FS w systemie Windows Server 2012 R2.

### <a name="odata-improvements"></a>Ulepszenia OData

**Rozszerzona obsługa $select, $, $batch i $value**

ASP.NET Web API OData ma teraz pełną obsługę dla elementu $select, rozwiń węzeł $ i $value. Umożliwia także $batch dla żądania przetwarzania wsadowego i przetwarzania grupy zmian.

$Select i $expand Rozwiń opcje umożliwiają zmianę kształtu danych, który jest zwracany z punktu końcowego OData. Aby uzyskać więcej informacji, zobacz [Introducing $select i $expand rozwiń węzeł Obsługa w protokole OData składnika Web API](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Ulepszone rozszerzalności**

Programy formatujące OData są teraz rozszerzonego. Można dodać metadanych wpisu Atom, nazwany wpisy link strumienia i nośnik obsługują, dodawanie adnotacji wystąpienia i dostosowanie sposobu generowania łączy.

**Obsługa bez typu**

Teraz można tworzyć usługi OData, bez konieczności Definiowanie typów CLR dla Twojego typów jednostek. Zamiast tego kontrolerów OData, może przejąć lub zwrócić wystąpienia **IEdmObject**, które są programów formatujących OData serializacji/deserializacji.

**Ponowne użycie istniejącego modelu**

Jeśli masz już istniejącego modelu entity data model (EDM), możesz teraz użyć ponownie go bezpośrednio, zamiast tworzenia nowej. Na przykład jeśli korzystasz z programu Entity Framework, można użyć EDM, która EF tworzy dla Ciebie.

### <a name="request-batching"></a>Żądanie, przetwarzanie wsadowe

Przetwarzanie wsadowe żądania łączy wiele operacji na pojedyncze żądanie HTTP POST, w celu zmniejszenie ruchu w sieci i zapewnienia lepszego mniej interfejsu chatty użytkownika. Interfejs API sieci Web platformy ASP.NET obsługuje teraz kilka strategii dla żądania przetwarzania wsadowego:

- Użyj punkt końcowy usługi OData $batch.
- Wiele żądań tworzenia pakietów w pojedyncze żądanie wieloczęściowej wiadomości MIME.
- Użyj formatu niestandardowego przetwarzanie wsadowe.

Aby włączyć przetwarzanie wsadowe żądania, po prostu Dodaj trasę przetwarzanie wsadowe obsługi konfiguracji interfejsu API sieci Web:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Można również sterować czy żądania lub wykonywane sekwencyjnie lub w dowolnej kolejności.

### <a name="portable-aspnet-web-api-client"></a>Klienta interfejsu API sieci Web ASP.NET przenośne

Klienta interfejsu API sieci Web platformy ASP.NET można teraz używać do tworzenia biblioteki klas przenośnych, które działają w aplikacjach Sklepu Windows i Windows Phone 8. Można również utworzyć przenośne elementy formatujące, które mogą być udostępniane między klientem a serwerem.

### <a name="improved-testability"></a>Ulepszone testowania

Sieci Web API 2 sprawia, że jej znacznie łatwiejsze do jednostki testu kontrolerów interfejsu API. Po prostu utworzyć wystąpienia Kontroler interfejsu API z komunikatu żądania i konfiguracji, a następnie wywołaj metodę akcji, którą chcesz przetestować. Jest również ułatwia mock **UrlHelper** klasy dla metody akcji, wykonujących generowania łącza.

### <a name="ihttpactionresult"></a>IHttpActionResult

Teraz można zaimplementować IHttpActionResult w celu hermetyzacji wynik metody akcji interfejsu API sieci Web. IHttpActionResult zwrócony z metody akcji interfejsu API sieci Web jest wykonywany przez środowisko uruchomieniowe interfejsu API sieci Web platformy ASP.NET do zwróci komunikat wynikowy odpowiedzi. IHttpActionResult może zwracać żadnych czynności interfejsu API sieci Web w celu uproszczenia jednostki testowania implementacji interfejsu API sieci Web. Dla wygody podać liczbę implementacje IHttpActionResult fabrycznej wyniki dla zwracania kodów stanu określonego w tym formacie zawartości lub negocjował zawartości odpowiedzi.

### <a name="httprequestcontext"></a>HttpRequestContext

Nowy **HttpRequestContext** śledzi tutaj stan, który jest powiązany z żądania, ale nie są natychmiast dostępne z żądania. Na przykład można użyć **HttpRequestContext** można pobrać danych trasy, podmiot zabezpieczeń skojarzony z żądaniem certyfikatu klienta **UrlHelper** i katalog główny ścieżki wirtualnej. Można jednak łatwo tworzyć **HttpRequestContext** testowania jednostki.

Ponieważ podmiot zabezpieczeń dla żądania jest umieszczane w żądaniu, zdejmując to zadanie **Thread.CurrentPrincipal**, podmiot zabezpieczeń jest teraz dostępna w okresie istnienia żądania, gdy jest on w potok składnika Web API.

### <a name="cors"></a>CORS

Dzięki użyciu udziału dużą innej firmy Brock Allen ASP.NET teraz w pełni obsługuje Cross pochodzenia żądania udostępniania (CORS).

Poziom zabezpieczeń przeglądarki uniemożliwia wprowadzanie żądania AJAX do innej domeny przez stronę sieci web. [Mechanizm CORS](http://www.w3.org/TR/cors/) jest standardem W3C, dzięki której serwer złagodzenie zasad tego samego źródła. Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin podczas odrzucenia innych użytkowników.

Składnik Web API 2 obsługuje teraz CORS, łącznie z automatyczną obsługę żądań wstępnych. Aby uzyskać więcej informacji, zobacz [Włączanie żądań Cross-Origin w interfejsie API sieci Web ASP.NET](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtry uwierzytelniania

Filtry uwierzytelniania są nowym typem filtru w interfejsie API sieci Web ASP.NET, uruchomione przed filtry autoryzacji w potoku składnika ASP.NET Web API, który pozwala na określenie uwierzytelniania logiki na działania, na kontroler lub globalnie do wszystkich kontrolerów. Filtry uwierzytelniania przetworzyć poświadczeń w żądaniu i podaj odpowiednie podmiot zabezpieczeń. Filtry uwierzytelniania można również dodać wezwań do uwierzytelnienia w odpowiedzi do nieautoryzowanego żądania.

### <a name="filter-overrides"></a>Zastąpienia filtru

Teraz można zastąpić, które filtry mają zastosowanie do metody danej akcji lub kontrolera, określając filtrem zastępowania. Filtry zastąpienie określ zestaw typów filtrów, które nie powinny być uruchamiane dla danego zakresu (akcji lub kontrolera). Dzięki temu można dodać filtry globalne, ale następnie wykluczyć niektórych z określonych akcji i kontrolerów.

### <a name="owin-integration"></a>Integracja OWIN

ASP.NET Web API teraz w pełni obsługuje OWIN i mogą być uruchamiane na żadnym hoście stanie OWIN. Dołączone są także **HostAuthenticationFilter** zapewnia integrację z systemem uwierzytelniania OWIN.

Dzięki integracji OWIN interfejsu API sieci Web można hosta samodzielnego w procesie równolegle z innymi oprogramowanie pośredniczące OWIN, takich jak SignalR. Aby uzyskać więcej informacji, zobacz [OWIN Użyj interfejsu API sieci Web ASP.NET Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

W poniższych sekcjach opisano funkcje SignalR 2.0.

- [Oparty na OWIN](#builtonowin)
- [MapHubs i MapConnection są teraz MapSignalR](#MapSignalR)
- [Obsługa między domenami](#crossdomain)
- [iOS i Android obsługują za pośrednictwem MonoTouch i MonoDroid](#mobile)
- [Klienta przenośnego .NET](#portable)
- [Nowy pakiet Host samodzielny](#selfhost)
- [Obsługa serwerów starszymi wersjami](#backwardcompat)
- [Usunięta obsługa serwera dla programu .NET 4.0](#remove40)
- [Wysyłanie wiadomości do listy klientów i grup](#messagelist)
- [Wysyłanie wiadomości do określonego użytkownika](#sendtouser)
- [Lepszą obsługę Obsługa błędów](#errorhandling)
- [Jednostka łatwiejsze testowanie koncentratory](#unittesting)
- [Obsługa błędów JavaScript](#javascripterror)

Na przykład sposobu uaktualniania istniejącego projektu 1.x 2.0 SignalR zobacz [uaktualniania SignalR 1.x projektu](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Oparty na OWIN

SignalR 2.0 jest oparty na całkowicie [OWIN (interfejsu Open Web dla platformy .NET)](http://owin.org/). Ta zmiana sprawia, że proces instalacji dla biblioteki SignalR bardziej spójne hostowanych w sieci web i samodzielnie hostowana aplikacji SignalR, ale wymagany jest również liczba zmian interfejsu API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs i MapConnection są teraz MapSignalR

Zgodność ze standardami OWIN, te metody zmieniono `MapSignalR`. `MapSignalR` wywoływana bez parametrów przypisze wszystkie koncentratory (jako `MapHubs` w wersji 1.x); do mapowania poszczególnych **klasy PersistentConnection** obiektów, określ typ połączenia jako parametr typu i rozszerzenia adresu URL dla połączenia jako pierwszy argument.

`MapSignalR` Metoda jest wywoływana w klasy początkowej Owin. Visual Studio 2013 zawiera nowy szablon dla klasy początkowej Owin; Aby użyć tego szablonu, wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy na projekt
2. Wybierz **dodać**, **nowy element...**
3. Wybierz **klasy początkowej Owin**. Nazwa nowej klasy **Startup.cs**.

W **aplikacji, sieci Web** Owin uruchamiania klasy zawierające `MapSignalR` metody jest dodawane do procesu uruchamiania dla Owin za pomocą wpisu w węźle Ustawienia aplikacji w pliku Web.Config, jak pokazano poniżej.

W **samodzielnego hostingu aplikacji**, klasa początkowa jest przekazywana jako parametr typu `WebApp.Start` metody.

**Mapowania koncentratorów i połączeniami w programie SignalR 1.x (z pliku globalnego aplikacji w aplikacji sieci web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Mapowania koncentratorów i połączeniami w programie SignalR 2.0 (z pliku klasy uruchamiania Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

W **samodzielnego hostingu aplikacji**, klasa początkowa jest przekazywana jako parametr typu dla `WebApp.Start` metody, jak pokazano poniżej.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Obsługa między domenami

W bibliotece SignalR 1.x żądania obejmujące różne domeny zostały kontrolowane przez pojedynczy flagi EnableCrossDomain. Ta flaga kontrolowane zarówno JSONP i CORS żądania. Większa elastyczność obsługuje wszystkie CORS został usunięty z składnik serwera biblioteki signalr (JavaScript lients nadal normalnie z niego korzystać mechanizmu CORS w przypadku wykrycia, że przeglądarka obsługuje on), i nowe oprogramowanie pośredniczące OWIN służące do obsługi tych scenariuszy.

W SignalR 2.0, czy format JSONP jest wymagany na kliencie (do obsługi żądań między domenami w starszych przeglądarkach), będzie musiała zostać jawnie włączyć, ustawiając `EnableJSONP` na `HubConfiguration` do obiektu `true`, jak pokazano poniżej. JSONP jest domyślnie wyłączony, ponieważ jest to mniej bezpieczna niż CORS.

Aby dodać nowe oprogramowanie pośredniczące CORS w SignalR 2.0, Dodaj `Microsoft.Owin.Cors` biblioteki do projektu i wywołanie `UseCors` przed oprogramowania pośredniczącego SignalR, jak pokazano w poniższej sekcji.

**Dodawanie do projektu Microsoft.Owin.Cors**: do zainstalowania tej biblioteki, uruchom następujące polecenie w konsoli Menedżera pakietów:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

To polecenie spowoduje dodanie 2.0.0 wersję pakietu do projektu.

**Wywoływanie UseCors**

Poniższe fragmenty kodu przedstawiają sposób wykonania połączenia między domenami w SignalR 1.x i 2.0.

**Implementowanie żądań między domenami w SignalR 1.x (z pliku globalnego aplikacji)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementowanie żądań między domenami w SignalR 2.0 (plik kodu C#)**

Poniższy kod przedstawia sposób włączania CORS lub JSONP w projekcie SignalR 2.0. Ten przykład kodu wykorzystuje `Map` i `RunSignalR` zamiast `MapSignalR`, dzięki czemu oprogramowanie pośredniczące CORS działa tylko dla żądań SignalR, które wymagają obsługi mechanizmu CORS (a nie dla całego ruchu w ścieżce określonej w `MapSignalR`.) `Map` może także służyć do innego oprogramowania pośredniczącego musi być uruchamiane dla określonego prefiksu adresu URL, a nie dla całej aplikacji.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS i Android obsługują za pośrednictwem MonoTouch i MonoDroid

Dodano obsługę dla systemów iOS i Android będących klientami przy użyciu składników MonoTouch i MonoDroid z [Xamarin biblioteki](https://xamarin.com/). Aby uzyskać więcej informacji na temat sposobu korzystania z nich, zobacz [przy użyciu składników Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Te składniki będzie dostępna w [sklepie Xamarin](https://store.xamarin.com/) Jeśli jest dostępny w wersji SignalR RTW.

<a id="portable"></a> ### Przenośny klient .NET

Aby lepiej ułatwiają aplikacji dla wielu platform, Silverlight, WinRT i Windows Phone klientów zostały zastąpione pojedynczego przenośny klient .NET, który obsługuje następujące platformy:

- NET 4.5
- Silverlight 5
- WinRT (platforma .NET dla aplikacji ze Sklepu Windows)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nowy pakiet Host samodzielny

Teraz jest pakietem NuGet, aby ułatwić rozpoczęcie pracy z SignalR hosta samodzielnego (aplikacji SignalR, które znajdują się w procesie lub innej aplikacji, a nie jest hostowany na serwerze sieci web). Aby uaktualnić projekt hosta samodzielnego skompilowanej za pomocą biblioteki SignalR 1.x, usuń pakiet Microsoft.AspNet.SignalR.Owin i Dodaj pakiet Microsoft.AspNet.SignalR.SelfHost. Aby uzyskać więcej informacji na wprowadzenie hosta samodzielnego pakietu, zobacz [samouczek: SignalR hosta samodzielnego](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Obsługa serwerów starszymi wersjami

W poprzednich wersjach programu SignalR, wersje pakietu SignalR używanego w kliencie i serwerze potrzebne, aby była taka sama. Aby zapewnić obsługę aplikacji gruby klienta, które jest trudna do aktualizacji, SignalR 2.0 obsługuje teraz, ze starszego klienta przy użyciu nowszej wersji serwera. **Uwaga: SignalR 2.0 nie obsługuje serwerów wbudowane ze starszymi wersjami z nowszych wersji klientów.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Usunięta obsługa serwera dla programu .NET 4.0

SignalR 2.0 spadła obsługę serwera współdziałania z programu .NET 4.0. .NET 4.5, należy użyć serwerom SignalR 2.0. Nadal jest klientem programu .NET 4.0 2.0 SignalR.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Wysyłanie wiadomości do listy klientów i grup

W SignalR 2.0 jest możliwe wysyłanie wiadomości przy użyciu listy klienta i identyfikatorów grup. Poniższe fragmenty kodu pokazują, jak to zrobić.

**Wysyłanie wiadomości do listy klientów i grup za pomocą klasy PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Wysyłanie wiadomości do listy klientów i grup za pomocą centrów**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Wysyłanie wiadomości do określonego użytkownika

Ta funkcja umożliwia użytkownikom na określenie, co to jest identyfikator userId oparte na IRequest za pomocą nowego interfejsu IUserIdProvider:

**Interfejs IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Domyślnie będzie implementację, która używa IPrincipal.Identity.Name użytkownika jako nazwy użytkownika.

W koncentratory będziesz mieć możliwość wysyłania komunikatów do tych użytkowników przy użyciu nowego interfejsu API:

**Przy użyciu Clients.User interfejsu API**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Lepszą obsługę Obsługa błędów

Użytkownicy mogą teraz throw **HubException** z dowolnego wywołania koncentratora. Konstruktor obiektu **HubException** ciąg komunikatu i obiektu dodatkowe dane dotyczące błędu. SignalR zostanie automatycznie serializować wyjątku i wysłania go do klienta, w której będzie służyć do odrzucenia/Niepowodzenie wywołania metody koncentratora.

**Pokaż Centrum szczegółowe wyjątki** ustawienie nie ma żadnego wpływu na **HubException** są wysyłane do klienta, czy nie; zawsze wysłaniem.

**Prezentacja wysyłania HubException do klienta kodu po stronie serwera**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**JavaScript — kod klienta prezentacja odpowiada na żądania HubException wysłanych z serwera**

[!code-html[Main](release-notes/samples/sample16.html)]

**Kod klienta .NET prezentacja odpowiada na żądania HubException wysłanych z serwera**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Jednostka łatwiejsze testowanie koncentratory

SignalR 2.0 obejmuje interfejs o nazwie `IHubCallerConnectionContext` na koncentratorów, które ułatwia tworzenie wywołania po stronie klienta zasymulować. Poniższe fragmenty kodu pokazują przy użyciu tego interfejsu z przewodów popularnych testu [xUnit.net](https://github.com/xunit/xunit) i [moq](https://code.google.com/p/moq/).

**Testy jednostkowe SignalR pomocą xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Testy jednostkowe SignalR pomocą moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Obsługa błędów JavaScript

W SignalR 2.0 wszystkie wywołania zwrotne obsługi błędów JavaScript zwracać obiekty błąd JavaScript zamiast ciągów raw. Dzięki temu SignalR bardziej rozbudowane informacji do programu obsługi błędów. Możesz uzyskać wyjątek wewnętrzny z `source` właściwości błędu.

**Kod JavaScript klienta, który obsługuje wyjątek Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nowe systemu członkostwa programu ASP.NET

ASP.NET Identity jest nowy system członkostwa aplikacji ASP.NET. ASP.NET Identity ułatwia integrowanie danych profilu dla użytkownika z danych aplikacji. ASP.NET Identity można również wybrać modelu trwałości profilów użytkowników w aplikacji. Dane można przechowywać w bazie danych programu SQL Server lub inny magazyn danych, w tym magazynów danych NoSQL, takie jak tabele magazynu Azure. Aby uzyskać więcej informacji, zobacz [indywidualnych kont użytkowników](creating-web-projects-in-visual-studio.md#indauth) w **tworzenia projektów sieci Web ASP.NET w programie Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Uwierzytelnianie oparte na oświadczeniach

ASP.NET obsługuje teraz uwierzytelnianie oparte na oświadczeniach, gdy tożsamość użytkownika jest reprezentowany jako zestaw oświadczeń z zaufanych wystawców. Użytkownicy mogą być uwierzytelnione przy użyciu nazwy użytkownika i hasła przechowywane w bazie danych aplikacji, lub dostawców tożsamości społecznościowych (na przykład: Accounts firmy Microsoft, Facebook, Google, Twitter), lub za pomocą konta organizacyjne w usłudze Azure Active Directory lub Usługi federacyjne Active Directory (AD FS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integracja z usługą Azure Active Directory i usługi Active Directory systemu Windows Server

Teraz można tworzyć projektów programu ASP.NET, które używają usługi Azure Active Directory lub systemu Windows Server Active Directory (AD) do uwierzytelniania. Aby uzyskać więcej informacji, zobacz [konta organizacyjne](creating-web-projects-in-visual-studio.md#orgauth) w **tworzenia projektów sieci Web ASP.NET w programie Visual Studio 2013**.

### <a name="owin-integration"></a>Integracja OWIN

Uwierzytelnianie ASP.NET jest teraz oparte na oprogramowanie pośredniczące OWIN służący na żadnym hoście standardem OWIN. Aby uzyskać więcej informacji o produkcie OWIN, zobacz następujące tematy [składniki Microsoft OWIN](#TOC7) sekcji.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Składniki Microsoft OWIN

[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (OWIN) definiuje abstrakcję między serwerami sieci web .NET i aplikacji sieci web. OWIN oddziela aplikacji sieci web z serwera, co funkcja hosta aplikacji sieci web. Można na przykład host aplikacji sieci web opartych na OWIN w usługach IIS lub samodzielną w procesie niestandardowych.

Zmiany wprowadzone w składniki Microsoft OWIN (znanej także jako projekt Katana) obejmują nowych składników serwera i hosta, oprogramowanie pośredniczące i nowe pomocnika biblioteki oraz nowego oprogramowania pośredniczącego uwierzytelniania.

Aby uzyskać więcej informacji na temat OWIN i Katana, zobacz [What's new in OWIN i Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Uwaga: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) nie mogą być uruchomione w trybie klasycznym IIS; musi działać w trybie zintegrowanym.**

**Uwaga: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplikacje muszą być uruchamiane w trybie pełnego zaufania.**

### <a name="new-servers-and-hosts"></a>Nowe serwery i hosty

W tej wersji dodano nowych składników na potrzeby scenariuszy z hosta samodzielnego. Składniki te zawierają następujące pakiety NuGet:

- **Microsoft.Owin.Host.HttpListener**. Udostępnia serwer OWIN, który używa **HttpListener** do nasłuchiwania żądań HTTP i kierować je do potoku OWIN.
- **Microsoft.Owin.Hosting** udostępnia bibliotekę dla deweloperów, którzy chcą hosta samodzielnego potok OWIN w procesie niestandardowych, takich jak konsola aplikacji lub usługi systemu Windows.
- **OwinHost**. Udostępnia autonomicznego pliku wykonywalnego, który opakowuje `Microsoft.Owin.Hosting` i umożliwia hosta samodzielnego potoku OWIN bez konieczności pisania aplikacji hosta niestandardowego.

Ponadto `Microsoft.Owin.Host.SystemWeb` pakiet umożliwia teraz oprogramowaniu pośredniczącym, aby zapewnić wskazówki **SystemWeb** serwera i wskazujący, że oprogramowanie pośredniczące powinna być wywoływana podczas określonego etap potoku ASP.NET. Ta funkcja jest szczególnie przydatne dla oprogramowania pośredniczącego uwierzytelniania, które powinno być ono uruchomione na początku potoku ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Pomocnik bibliotek i oprogramowania pośredniczącego

Mimo że można pisać przy użyciu tylko funkcji i typ definicje ze specyfikacji OWIN składniki OWIN nowe `Microsoft.Owin` pakiet zapewnia użytkownikom większy komfort zestaw obiektów abstrakcyjnych. Ten pakiet łączy kilka starszych pakietów (np. `Owin.Extensions`, `Owin.Types`) do modelu obiektu jednej, dobrze, które następnie mogą być łatwo używane przez inne składniki OWIN. W rzeczywistości większość składniki Microsoft OWIN używa teraz tego pakietu.

> [!NOTE]
> [OWIN](http://www.owin.org) nie mogą być uruchomione w trybie klasycznym IIS; musi działać w trybie zintegrowanym.

> [!NOTE]
> [OWIN](http://www.owin.org) aplikacje muszą być uruchamiane w trybie pełnego zaufania.

Ta wersja zawiera również pakietu Microsoft.Owin.Diagnostics, w tym oprogramowaniu pośredniczącym, aby zweryfikować uruchomionej aplikacji OWIN, a także oprogramowanie pośredniczące strony błędu ułatwiające zbadaj błędy.

### <a name="authentication-components"></a>Składniki uwierzytelniania

Dostępne są następujące składniki uwierzytelniania.

- **Microsoft.Owin.Security.ActiveDirectory**. Umożliwia uwierzytelnianie za pomocą usługi katalogowe lokalnie lub w chmurze.
- **Microsoft.Owin.Security.Cookies** umożliwia uwierzytelnianie za pomocą plików cookie. Ten pakiet został wcześniej nazwane `Microsoft.Owin.Security.Forms`.
- **Microsoft.Owin.Security.Facebook** umożliwia uwierzytelnianie przy użyciu usługi opartej na OAuth w serwisie Facebook.
- **Microsoft.Owin.Security.Google** umożliwia uwierzytelnianie przy użyciu usługi opartej na OpenID firmy Google.
- **Microsoft.Owin.Security.Jwt** umożliwia uwierzytelnianie za pomocą tokenów JWT.
- **Microsoft.Owin.Security.MicrosoftAccount** umożliwia uwierzytelnianie za pomocą konta Microsoft.
- **Microsoft.Owin.Security.OAuth**. Zapewnia serwera autoryzacji OAuth, jak również oprogramowanie pośredniczące uwierzytelniania tokenów elementu nośnego.
- **Microsoft.Owin.Security.Twitter** umożliwia uwierzytelnianie przy użyciu usługi opartej na OAuth w serwisie Twitter.

Ta wersja zawiera również `Microsoft.Owin.Cors` pakiet, który zawiera oprogramowanie pośredniczące do przetwarzania żądań HTTP cross-origin.

> [!NOTE]
> W ostatecznej wersji programu Visual Studio 2013 została usunięta obsługa do podpisywania tokenu JWT.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Aby uzyskać listę nowych funkcji oraz innych zmian wprowadzonych w programie Entity Framework 6, zobacz [Historia wersji programu Entity Framework](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 obejmuje następujące nowe funkcje:

- Obsługa kartę edycji. Preivously **dokumentu w formacie** polecenia, automatycznie wcięcia i automatycznego formatowania w programie Visual Studio nie działały prawidłowo po użyciu **zachować karty** opcji. Ta zmiana poprawia Visual Studio formatowanie kodu Razor dla karty formatowania.
- Obsługa ponowne zapisywanie adresów URL reguł podczas generowania łączy.
- Usunięcie atrybutu przezroczysty zabezpieczeń.
  > [!NOTE]
  > Jest to istotne zmiany i sprawia, że Razor 3 niezgodne z MVC4 i wcześniej, gdy Razor 2 jest niezgodna z MVC5 lub zestawy skompilowany MVC5.

Usunięto w programie Visual Studio 2013 z wersji wstępnych problemów razor 3 można znaleźć [tutaj](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Wstrzymać aplikacja ASP.NET

Wstrzymywanie aplikacji ASP.NET jest funkcją zmieniający w programie .NET Framework 4.5.1 które znacząco zmieniają środowisko użytkownika i ekonomiczne modelu do obsługi dużej liczby witryny ASP.NET na jednym komputerze. Aby uzyskać więcej informacji, zobacz [wstrzymania aplikacji platformy ASP.NET — reakcji udostępnionych hostingu sieci web .NET](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

W tej sekcji opisano znane problemy i fundamentalne zmiany w ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Nowe przywracanie pakietów nie działa w Mono przy użyciu pliku SLN](https://nuget.codeplex.com/workitem/3596) — zostanie naprawiony w przyszłych nuget.exe pobierania i [pakietu NuGet.CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) aktualizacji.
- [Nowe przywracanie pakietów nie działa z projektami Wix](https://nuget.codeplex.com/workitem/3598) — zostanie naprawiony w przyszłych nuget.exe pobierania i [pakietu NuGet.CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) aktualizacji.
- [Automatyczne przywracanie pakietów nie działa dla projektów w folderze rozwiązania](https://nuget.codeplex.com/workitem/3625) — zostanie rozwiązany w NuGet 2.8.

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` nie zwraca `IQueryable<T>` zawsze, jak Dodaliśmy obsługę `$select` i `$expand`.

    Nasze przykłady wcześniej dla `ODataQueryOptions<T>` zawsze rzutować wartość zwrotu z `ApplyTo` do `IQueryable<T>`. To już wcześniej, ponieważ opcje zapytania, które firma Microsoft obsługiwane wcześniej (`$filter`, `$orderby`, `$skip`, `$top`) nie należy zmieniać kształtu zapytania. Teraz, gdy firma Microsoft obsługuje `$select` i `$expand` wartość zwrotną z elementu `ApplyTo` nie będzie `IQueryable<T>` zawsze.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Jeśli używasz przykładowy kod z wcześniej, zostanie on kontynuować pracę Jeśli klient nie wysyła `$select` i `$expand`. Jednak jeśli chcesz obsługiwać `$select` i `$expand` należy zmienić ten kod do tego.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url lub RequestContext.Url ma wartość null podczas żądania wsadowego**

    W przypadku łączenia we wsady **UrlHelper** ma wartość null, gdy użytkowcy **Request.Url** lub **RequestContext.Url**.

    Ten problem jest obecnie śledzone tutaj: [BatchRequestContext.Url ma wartość null dla przetwarzania wsadowego żądania](http://aspnetwebstack.codeplex.com/workitem/1301).

    Obejście tego problemu jest utworzenie nowego wystąpienia **UrlHelper**, jak w poniższym przykładzie:

    **Tworzenie nowego wystąpienia UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Korzystając z MVC5 i OrgAuth, jeśli używane są widoki, które czy AntiForgerToken weryfikacji, mogą napotkać następujący błąd podczas publikowania danych w widoku:

    **Błąd**:

    *Błąd serwera w "/" aplikacji.*

    <em>Oświadczenie typu "<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>"lub"<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>" nie jest obecny na podanego elementu ClaimsIdentity. Aby włączyć obsługę token zabezpieczający przed sfałszowaniem przy użyciu uwierzytelniania opartego na oświadczeniach, sprawdź, czy dostawcy oświadczeń skonfigurowanych jest zapewnienie obu tych oświadczeń w wystąpieniach ClaimsIdentity, który generuje. Jeśli dostawcy oświadczeń skonfigurowanych użyje na inny typ oświadczenia jako unikatowy identyfikator, może być skonfigurowana przez ustawienie właściwości statycznej AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Obejście**:

    Dodaj następujący wiersz w pliku Global.asax go naprawić:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Ten problem zostanie rozwiązany w następnej wersji.
2. Po uaktualnieniu aplikacji MVC4 do MVC5, Skompiluj rozwiązanie, a następnie uruchom go. Powinien zostać wyświetlony następujący błąd:

    [A] Nie można rzutować System.Web.WebPages.Razor.Configuration.HostSection [B]System.Web.WebPages.Razor.Configuration.HostSection. Type A originates from 'System.Web.WebPages.Razor, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'. Type B originates from 'System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.

    Aby naprawić błąd powyżej, otwórz *wszystkich* plików Web.config (w tym te w folderze widoków) w projekcie i wykonaj następujące czynności:

   1. Zaktualizuj wszystkie wystąpienia "System.Web.Mvc" wersja "4.0.0.0" do "5.0.0.0".
   2. Zaktualizuj wszystkie wystąpienia "System.Web.Helpers", wersja "2.0.0.0" &quot;System.Web.WebPages&quot; i &quot;System.Web.WebPages.Razor&quot; do "3.0.0.0"

      Na przykład po wprowadzeniu zmiany powyżej powiązania zestawu powinna wyglądać następująco:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Aby uzyskać informacje o uaktualnianiu projektów MVC 4 do MVC 5, zobacz [sposób uaktualnienia programu ASP.NET MVC 4 i projekt interfejsu API sieci Web platformy ASP.NET MVC 5 i Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Jeśli weryfikacja po stronie klienta z jQuery sprawdzania poprawności dyskretnego kodu, komunikatu weryfikacji czasami jest niepoprawny dla elementu input języka HTML, z typem = 'number'. Błąd sprawdzania poprawności dla wymaganej wartości ("wieku pole jest wymagane") jest wyświetlany po wprowadzeniu nieprawidłową liczbę zamiast poprawne komunikat, że wymagany jest prawidłowy numer.

    Ten problem zwykle jest związany z szkieletu kodu dla modelu z właściwością liczby całkowitej na tworzenie i edytowanie widoków.

    Aby obejść ten problem, zmień pomocnika edytor z:

    `@Html.EditorFor(person => person.Age)`

    Do:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 nie obsługuje już częściowej relacji zaufania. Łączenie z plików binarnych MVC ani WebAPI projektów należy usunąć [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) atrybutu i [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) atrybutu. Wyeliminuje błędy kompilatora, takie jak usunąć te atrybuty.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Uwaga: zgodnie z efektem ubocznym to Ty, nie można używać zestawów 4.0 i 5.0 w tej samej aplikacji. Musisz zaktualizować je wszystkie do 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>SPA szablonu z serwisu Facebook autoryzacji może spowodować niestabilność w programie Internet Explorer, gdy witryna sieci web znajduje się w strefie intranetu

Szablon SPA udostępnia zewnętrznych Zaloguj się za pomocą usługi Facebook. Gdy utworzonego przy użyciu szablonu projektu działa lokalnie, logowania może spowodować IE awarię.

Rozwiązanie:

1. Host witryny sieci web w strefie internet; lub

2. Przetestowania tego scenariusza, w przeglądarce niż programu Internet Explorer.

### <a name="web-forms-scaffolding"></a>Formularze sieci Web tworzenia szkieletu

Funkcja szkieletów formularzy sieci Web został usunięty z VS2013 i będą dostępne w przyszłej aktualizacji programu Visual Studio. Można jednak nadal używać szkieletów w ramach projektu formularzy sieci Web przez dodanie zależności MVC i Generowanie szkieletów MVC. Projekt będzie zawierać kombinację MVC i formularzy sieci Web.

Aby dodać MVC do projektu formularzy sieci Web, Dodaj nowy element szkieletu i wybierz **zależności MVC 5**. Wybierz minimalnego lub pełną w zależności od tego, czy należy wszystkie pliki zawartości, takich jak skrypty. Następnie można dodać elementu szkieletu dla platformy MVC, co spowoduje utworzenie widoki i kontrolera w projekcie.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC i Web API szkieletów - HTTP 404 błędu nie znaleziono

Jeśli wystąpi błąd podczas dodawania elementu szkieletu do projektu, istnieje możliwość, projekt zostanie pozostawiony w stanie niespójnym. Niektóre zmiany wprowadzone można szkieletów zostanie wycofana, ale inne zmiany, takie jak zainstalowane pakiety NuGet zostaną nie można wycofać. Jeśli routingu zmiany konfiguracji zostaną przywrócone, użytkownicy będą otrzymywać błąd HTTP 404 podczas nawigowania do szkieletu elementów.

Obejście problemu:

- Aby naprawić ten błąd dla platformy MVC, Dodaj nowy element szkieletu i wybierz zależności MVC 5 (minimalnie lub pełna). Ten proces, zostaną dodane wszystkie wymagane zmiany do projektu.
- Aby naprawić ten błąd interfejsu API sieci Web:

  1. Klasa WebApiConfig należy dodać do projektu.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Skonfiguruj WebApiConfig.Register w aplikacji\_Start — metoda w pliku Global.asax w następujący sposób:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
