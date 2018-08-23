---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: U góry funkcji we wzorcu ASP.NET Web Pages 2 | Dokumentacja firmy Microsoft
author: microsoft
description: Ten temat zawiera omówienie Najważniejsze nowe funkcje programu ASP.NET Web Pages 2, uproszczone sieci web struktury programistycznej, który jest dołączony WebMatr...
ms.author: riande
ms.date: 02/13/2012
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: e06db7eb33dc2891d86b65fa56c20b9e8cae1970
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756701"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Najważniejsze funkcje we wzorcu ASP.NET Web Pages 2
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Ten artykuł zawiera omówienie najważniejszych nowych funkcji w ASP.NET Web Pages 2 RC, struktury programistycznej uproszczone sieci web, który jest dołączony [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **Co obejmuje:** 
> 
> - [Instalowanie programu WebMatrix](#install)
> - [Nowe i ulepszone funkcje](#New_and_Enhanced_Features)
> 
>     - [Zmiany w wersji RC](#Changes_for_the_RC_Version)
>     - [Zmiany w wersji Beta](#Changes_for_the_Beta_Version)
>     - [Za pomocą szablonów nowych i zaktualizowanych lokacji](#templates)
>     - [Walidacja danych wejściowych użytkownika](#validation)
>     - [Włączanie logowania do usług Facebook i innych lokacji za pomocą protokołu OAuth i OpenID](#oauthsetup)
>     - [Dodawanie map przy użyciu Pomocnika mapy](#maphelper)
>     - [Uruchamianie aplikacji stron sieci Web obok siebie](#sidebyside)
>     - [Renderowanie stron dla urządzeń przenośnych](#mobile)
> - [Dodatkowe zasoby](#resources)
> 
> > [!NOTE]
> > W tym temacie założono, że używasz programu WebMatrix do pracy z kodem ASP.NET Web Pages 2. Jednak za pomocą stron sieci Web 1, podczas tworzenia stron sieci Web 2 witryn sieci Web przy użyciu programu Visual Studio, co daje rozszerzone funkcje IntelliSense i debugowania. Aby pracować ze stronami sieci Web w programie Visual Studio, należy najpierw zainstalować program Visual Studio 2010 z dodatkiem SP1, Visual Web Developer Express 2010 z dodatkiem SP1 lub programu Visual Studio 11 Beta. Następnie zainstaluj ASP.NET Beta 4 MVC, która zawiera szablony i narzędzia do tworzenia aplikacji ASP.NET MVC 4 i Web Pages 2 w programie Visual Studio.
> 
> 
> *Data ostatniej aktualizacji: 18 czerwca 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Instalowanie programu WebMatrix

Aby zainstalować stron sieci Web, należy użyć Instalatora platformy sieci Web firmy Microsoft, czyli bezpłatnej aplikacji, która umożliwia łatwe instalowanie i konfigurowanie technologii związanych z sieci web. Zainstalujesz Beta 2 programu WebMatrix, która zawiera wersję Beta 2 stron sieci Web.

1. Przejdź na stronę instalacji, aby uzyskać najnowszą wersję Instalatora platformy sieci Web:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Jeśli masz już zainstalowany program WebMatrix 1, ta instalacja aktualizacji do wersji Beta 2 programu WebMatrix. Można uruchomić witryny sieci Web, które zostały utworzone przy użyciu wersji 1 lub 2 na tym samym komputerze. Aby uzyskać więcej informacji, zobacz sekcję na [aplikacji stron sieci Web działa obok siebie](#sidebyside).
2. Wybierz **Zainstaluj teraz**. 

    Jeśli używasz programu Internet Explorer, przejdź do następnego kroku. Jeśli używasz innej przeglądarki, takich jak Mozilla Firefox i Google Chrome, monit o zapisanie *Webmatrix.exe* plik na swoim komputerze. Zapisz plik, a następnie kliknij go, aby uruchomić Instalatora.
3. Uruchom Instalatora i wybrać **zainstalować** przycisku. Spowoduje to zainstalowanie programu WebMatrix i stron sieci Web.

## <a id="New_and_Enhanced_Features"></a>  Nowe i ulepszone funkcje

### <a id="Changes_for_the_RC_Version"></a>  Zmiany w wersji RC (czerwiec 2012)

Wydania wersji RC w czerwca 2012 ma kilka zmian, odświeżanie w wersji Beta, wydanej w marcu 2012. Te zmiany są następujące:

- A `Validation.AddFormError` metoda została dodana do `Validation` pomocnika. Jest to przydatne, jeśli ręcznie wykonać sprawdzanie poprawności (na przykład można zweryfikować wartości, który jest przekazywany w ciągu zapytania) i chcesz dodać komunikat o błędzie, który może być wyświetlany przez `Html.ValidationSummary` metody. Aby uzyskać więcej informacji, zobacz sekcję [sprawdzanie poprawności danych, nie pochodzą bezpośrednio z użytkowników](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) w [sprawdzanie poprawności danych wejściowych użytkownika w witrynach ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253002).
- Funkcja tworzenie pakietów i minimalizowanie został usunięty z zestawów podstawowych ASP.NET Web Pages 2. W konsekwencji `Assets` Pomocnik wymienione w dalszej części tego dokumentu nie jest dostępny. Zamiast tego należy zainstalować [optymalizacji ASP.NET](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) pakietu NuGet. Aby uzyskać więcej informacji, zobacz [tworzenie pakietów i Minifikacja zasobów w witrynie ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=255373).
- Dodano dodatkowe zestawy do obsługi platformy ASP.NET Web Pages 2. Tylko zauważalnego wpływu tej zmiany jest napotkać większej liczby zestawów w witrynie *bin* folderu po utworzeniu witryny sieci lub wdrażanie witryny do dostawcy usług hostingowych.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Zmiany dotyczące wersji Beta (luty 2012)

Wersja Beta, wydanej w lutym 2012 zawiera tylko kilku zmian w wersji Beta, który został wydany w grudnia 2011. Te zmiany są następujące:

- Razor obsługuje teraz atrybuty warunkowe. W kodzie HTML elementu, jeśli atrybut zostanie ustawiony na wartość, jest rozpoznawany inaczej w kodzie serwera, aby `false` lub `null`, ASP.NET nie jest renderowana atrybutu w ogóle. Na przykład Wyobraź sobie, że masz następujące znaczniki dla pola wyboru:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Jeśli wartość `checked1` jest rozpoznawana jako `false` lub `null`, `checked` atrybut nie jest renderowany. Jest to istotnej zmiany.
- `Validation.GetHtml` Metoda została zmieniona na `Validation.For`. To jest zmianą przerywającą; `Validation.GetHtml` nie będzie działać w wersji Beta.
- Teraz możesz uwzględnić `~` odwoływać się do katalogu głównego witryny bez użycia operatora znaczników `Href` funkcji. (To znaczy, analizator Razor można teraz znajdowaniem i usuwaniem `~` operator bez konieczności wywołanie metody jawne `Href`.) `Href` Metoda nadal działa, więc nie jest istotną zmianę.

    Na przykład, jeśli wcześniej była znaczników w następujący sposób:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Można teraz użyć znaczników w następujący sposób:

    `<a href="~/Default.cshtml">Home</a>`
- `Scripts` Pomocnika na potrzeby zarządzania zasobami (zasób) został zastąpiony `Assets` pomocnika, która ma nieco innych metod, takich jak następujące:

  - Aby uzyskać `Scripts.Add`, użyj `Assets.AddScript`
  - Aby uzyskać `Scripts.GetScriptTags`, użyj `Assets.GetScripts`

    To jest zmianą przerywającą; `Scripts` klasy nie jest dostępna w wersji Beta. Przykłady kodu w tym dokumencie, korzystaj z zarządzania zasobami, które zostały zaktualizowane przy użyciu tej zmiany.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Za pomocą szablonów nowych i zaktualizowanych lokacji

**Witryny początkowej** szablon został zaktualizowany tak, aby była uruchamiana na stronach sieci Web 2 domyślnie. Zawiera również następujące nowe funkcje:

- Renderowanie strony przyjazne dla urządzeń przenośnych. Przy użyciu stylów CSS oraz `@media` selektor **witryny początkowej** zapewnia ulepszone renderowania stron na mniejszych ekranach, w tym ekrany urządzeń przenośnych.
- Ulepszone opcje członkostwa i uwierzytelniania. Można pozwolić użytkownikom logowanie do witryny przy użyciu ich kont z innymi witrynami sieci społecznościowych, takich jak Twitter, Facebook i Windows Live. Aby uzyskać więcej informacji, zobacz [Włączanie logowania do usług Facebook i innych lokacji za pomocą protokołu OAuth i OpenID](#oauthsetup) sekcji.
- Elementy języka HTML5.

Nowy **witryny osobistej** szablon umożliwia tworzenie witryny sieci Web, która zawiera osobistych blogów, strona zdjęć i stronę w usłudze Twitter. Można dostosować witrynę na podstawie **witryny osobistej** szablonu, wykonując następujące czynności:

- Zmiana wyglądu witryny, edytując plik układu (*\_SiteLayout.cshtml*) i plik style (*Site.css*).
- Instalowanie pakietów NuGet, które dodają funkcje do witryny. Aby uzyskać informacje o instalowaniu pakietów, tym bibliotekę pomocników platformy ASP.NET w sieci Web, znajduje się w samouczku [instalowanie pomocników](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Aby uzyskać dostęp do **witryny osobistej** szablonu, wybierz **szablony** na jest oknem programu WebMatrix **— Szybki Start** ekranu.

[![topseven personalsite 1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

W **szablony** okna dialogowego wybierz **witryny osobistej** szablonu.

[![topseven personalsite 2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

Na stronie docelowej **witryny osobistej** szablon umożliwia skorzystaj z linków, aby skonfigurować blogu, Twitter, strona i strona zdjęć.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Walidacja danych wejściowych użytkownika

Na stronach sieci Web 1, aby sprawdzić poprawność danych wejściowych użytkownika w formularzach przesłane używasz `System.Web.WebPages.Html.ModelState` klasy. (Jest to zilustrowane w kilka przykładów kodu w tym samouczku 1 stron sieci Web pod tytułem [Praca z danymi](../data/5-working-with-data.md).) Takie podejście może nadal używać w sieci Web Pages 2. Web Pages 2 oferuje również ulepszone narzędzia do sprawdzania poprawności danych wejściowych użytkownika:

- Nowe klasy sprawdzania poprawności, w tym `System.Web.WebPages.ValidationHelper` i `System.Web.WebPages.Validator`, które umożliwiają wykonywanie zadań zaawansowanych weryfikacji przy użyciu kilku wierszy kodu.
- Ewentualnie weryfikacji po stronie klienta, która zapewnia natychmiastowe informacje zwrotne do użytkownika zamiast komunikacji dwustronnej z serwerem, aby sprawdzić, błędy sprawdzania poprawności. (Ze względów bezpieczeństwa sprawdzania poprawności jest wykonywane na serwerze nawet wtedy, gdy testy zostały wykonane w kliencie wcześniej).

Aby korzystać z nowych funkcji sprawdzania poprawności, wykonaj następujące czynności:

W kodzie strony należy zarejestrować element ma zostać zweryfikowana przy użyciu metody `Validation` pomocnika: `Validation.RequireField`, `Validation.RequireFields` (na przykład aby zarejestrować wiele elementów, aby wymagać), lub `Validation.Add`. `Add` Metody umożliwia określenie innych typów testów weryfikacyjnych, takich jak typ danych, sprawdzanie, porównanie wpisów w różnych obszarach, sprawdza długość ciągu i wzorców (przy użyciu wyrażeń regularnych). Oto kilka przykładów:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Aby wyświetlić błąd specyficzne dla pola, należy wywołać `Html.ValidationMessage` w znaczniku dla każdego elementu w trakcie sprawdzania poprawności:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Aby wyświetlić podsumowanie (`<ul>` listy) wszystkich błędów, na stronie `Html.ValidationSummary` w znaczniku:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Te kroki są wystarczająco dużo, aby zaimplementować weryfikację po stronie serwera. Jeśli chcesz dodać weryfikację po stronie klienta, wykonaj następujące czynności oprócz.

Dodaj następujące odwołania do pliku skryptu wewnątrz `<head>` części strony sieci web. Pierwsze dwa odwołania do skryptu wskazują pliki zdalne na serwerze, content delivery network (CDN). Trzeci punktów odniesienia do pliku lokalnego skryptu. Aplikacje produkcyjne powinny implementować rezerwowe, gdy sieć CDN jest niedostępny. Testuj plan awaryjny.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

Najprostszym sposobem uzyskania lokalnych kopii *jquery.validate.unobtrusive.min.js* biblioteki jest utworzenie nowej lokacji stron sieci Web na podstawie jednego z szablonów lokacji (takich jak witryny początkowej). Zawiera ona utworzona przez szablon *jquery.validate.unobtrusive.js* pliku w jego folder skryptów, z którego można skopiować go do swojej witryny.

Jeśli witryna internetowa używa<em>\_SiteLayout</em> strony do kontrolowania układu strony, możesz dołączyć te odwołania do skryptu na tej stronie, tak aby Weryfikacja jest dostępne dla wszystkich stron zawartości. Jeśli chcesz zweryfikować tylko dla konkretnych stron, można użyć Menedżera zasobów można zarejestrować skrypty na tylko na tych stronach. Aby to zrobić, należy wywołać `Assets.AddScript(path)` na stronie, którą chcesz zweryfikować i odwoływać się do każdego z plików skryptów. Następnie dodaj wywołanie `Assets.GetScripts` w  <em>\_SiteLayout</em> strony, aby było możliwe renderowanie zarejestrowaną `<script>` tagów. Aby uzyskać więcej informacji, zobacz sekcję [rejestrowanie skryptów za pomocą Menedżera zasobów](#resmanagement).

W znaczniku dla pojedynczego elementu, należy wywołać `Validation.For` metody. Ta metoda generuje atrybutów tego jQuery można dołączyć w celu udostępnienia weryfikacji po stronie klienta. Na przykład:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

Poniższy przykład pokazuje strony, która sprawdza poprawność danych wejściowych użytkownika na formularzu. Aby uruchomić i przetestować kod sprawdzania poprawności, wykonaj następujące czynności:

1. Utwórz nową witrynę sieci web przy użyciu jednego z szablonów witryny programu WebMatrix 2, które obejmuje *skrypty* folder, taki jak **witryny początkowej** szablonu.
2. W nowej lokacji, Utwórz nową *.cshtml* strony, a następnie zastąp zawartość strony z następującym kodem.
3. Uruchom stronę w przeglądarce. Wprowadź prawidłowe i nieprawidłowe wartości, aby zobaczyć efekty podczas weryfikacji. Na przykład wymagane pole puste, lub wprowadź literę w **środki na korzystanie z** pola.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Poniżej znajduje się Strona, gdy użytkownik przesyła prawidłowych danych wejściowych:

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Poniżej znajduje się Strona, gdy użytkownik przesyła je z polem wymaganym puste:

[![topSeven nieprawidłowa-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Poniżej znajduje się Strona, gdy użytkownik przesyła ją za pomocą coś innego niż całkowitą **środki na korzystanie z** pola:

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Aby uzyskać więcej informacji zobacz następujące wpisy na blogu:

- [Zaktualizowano sprawdzania poprawności w stronach sieci Web w wersji 2](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) podstawowe informacje dotyczące dodawania sprawdzania poprawności przy użyciu `Validation` pomocnika (po stronie serwera tylko)
- [Zaktualizowano sprawdzania poprawności w stronach sieci Web w wersji 2, część 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) Dodawanie walidacji po stronie klienta.
- [Zaktualizowano sprawdzania poprawności w stronach sieci Web w wersji 2, część 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) formatowanie błędy sprawdzania poprawności.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>Rejestrowanie skryptów przy użyciu Menedżera zasobów

Menedżer zasobów jest nową funkcję, która można użyć w kodzie serwera do zarejestrowania i renderowania skryptów klienta. Ta funkcja jest przydatna podczas pracy z kodem z wielu plików (takich jak strony układu strony z zawartością, pomocników, itp.), które są połączone w ramach pojedynczej strony w czasie wykonywania. Menedżer zasobów służy do koordynowania plików źródłowych i upewnij się, że pliki skryptów są poprawne odwołania do i efektywnie na renderowanej stronie niezależnie od tego, które pliki kodu są wywoływane z lub ile razy są wywoływane. Ponadto renderuje Menedżera zasobów `<script>` tagów w odpowiednim miejscu, aby załadować stronę szybko (bez pobierania skryptów podczas renderowania) i aby uniknąć błędów, które mogą wystąpić, jeśli skrypty są wywoływane przed renderowanie zostanie zakończone.

Na przykład załóżmy, że tworzenie niestandardowego elementu pomocniczego, który wywołuje plik języka JavaScript, a wywołanie tego pomocnika w trzech miejscach w kodzie strony zawartości. Jeśli nie używasz Menedżera zasobów, aby zarejestrować skrypt wywołuje pomocnika, trzy różne `<script>` tagi, które wskazujących na tym samym pliku skryptu pojawi się na renderowanej stronie. Dodatkowo, w zależności od tego, gdzie `<script>` znaczniki są wstawiane do renderowanej strony, mogą wystąpić błędy, jeśli skrypt próbuje uzyskać dostęp niektórych elementów strony, zanim strona ładuje się całkowicie. Jeśli używasz Menedżera zasobów do zarejestrowania skrypt pozwala uniknąć tych problemów.

Skrypt można zarejestrować za pomocą Menedżera zasobów, w ten sposób:

- W kodzie, który należy utworzyć odwołanie do skryptu, należy wywołać `Assets.AddScript` metody.
- W  *\_SiteLayout* strony, wywołań `Assets.GetScripts` metody do renderowania `<script>` tagów. 

    > [!NOTE]
    > Umieszczenie wywołania `Assets.GetScripts` jako bardzo ostatniego elementu w `<body>` elementu  *\_SiteLayout* strony. Dzięki temu strony ładować się szybciej i mogą pomóc uniknąć błędów skryptów.

Poniższy przykład pokazuje, jak działa Menedżera zasobów. Kod zawiera następujące elementy:

- Niestandardowego elementu pomocniczego o nazwie `MakeNote`. Tego pomocnika renderuje ciąg w polu opakowując `div` element wokół niego, ma różne z obramowaniem i dodając &quot;Uwaga:&quot; do niego. Pomocnik wywołuje również plik języka JavaScript, który dodaje zachowanie w czasie wykonywania do uwagi. Zamiast odwoływać się do skryptu z `<script>` tagu pomocnika rejestruje skrypt przez wywołanie metody `Assets.AddScript` .
- Plik języka JavaScript. To jest plik, który jest wywoływany przez pomocnika i tymczasowo zwiększa rozmiar czcionki dla elementów należy pamiętać podczas `mouseover` zdarzeń.
- Strony zawartości, który odwołuje się<em>\_SiteLayout</em> stronie renderuje część zawartości w treści, a następnie wywołuje `MakeNote` pomocnika.
- A  *\_SiteLayout* strony. Ta strona zawiera typowego nagłówka i strukturze układu strony. Obejmuje również wywołanie `Assets.GetScripts`, czyli jak Menedżer zasobów renderuje skrypt wywołuje na stronie.

Do uruchomienia przykładu:

1. Utwórz witrynę sieci Web Pages 2 puste. Możesz użyć programu WebMatrix **pusta witryna** ten szablon.
2. Utwórz folder o nazwie *skrypty* w lokacji.
3. W *skrypty* folderze utwórz plik o nazwie *Test.js*, kopia *Test.js* zawartości do niego z przykładu, a następnie zapisz plik...
4. Utwórz folder o nazwie *aplikacji\_kodu* w lokacji.
5. W *aplikacji\_kodu* folderze utwórz plik o nazwie *Helpers.cshtml*, skopiuj przykładowy kod do niego i zapisz go w folderze o nazwie *aplikacji\_kodu*w folderze głównym.
6. W folderze głównym lokacji, Utwórz plik o nazwie  *\_SiteLayout.cshtml,* skopiować przykładu do niego, a następnie zapisz plik.
7. W katalogu głównym witryny tak, Utwórz plik o nazwie *ContentPage.cshtml*, Dodaj kod przykładu i zapisz go.
8. Uruchom *ContentPage* w przeglądarce. Parametry przekazane do `MakeNote` pomocnika jest renderowany jako notatki ramce.
9. Przekaż wskaźnik myszy nad notatki. Skrypt tymczasowo zwiększa rozmiar czcionki notatki.
10. Wyświetl źródło renderowanej strony. Ze względu na którym została umieszczona wywołanie `Assets.GetScripts`, renderowanych `<script>` tag, który wywołuje *Test.js* jest bardzo ostatniego elementu w treści strony.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

Poniższy zrzut ekranu przedstawia *ContentPage.cshtml* w przeglądarce, jeśli przytrzymasz wskaźnik myszy nad uwagi:

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Włączanie logowania do usług Facebook i innych lokacji za pomocą protokołu OAuth i OpenID

Web Pages 2 udostępnia rozszerzone opcje członkostwa i uwierzytelniania. Rozszerzenie głównego zakłada, że nowe [OAuth](http://oauth.net/) i [OpenID](http://openid.net/) dostawców. Korzystając z tych dostawców, można pozwolić użytkownikom logowanie do witryny przy użyciu istniejących poświadczeń usług Facebook, Twitter, Windows Live, Google i Yahoo. Na przykład aby zalogować się przy użyciu konta w serwisie Facebook, użytkowników po prostu wybrać ikonę usługi Facebook, który przekierowuje go do strony logowania usługi Facebook, gdzie użytkownik podał ich informacji o użytkowniku. Można następnie skojarzyć logowania do usługi Facebook przy użyciu swojego konta w witrynie. Powiązane rozszerzenie do funkcji przynależności stron sieci Web jest czy użytkownicy mogą powiązać logowania (w tym logowania z witrynami sieci społecznościowych) za pomocą jednego konta w witrynie sieci Web.

Ta ilustracja przedstawia strony logowania z **witryny początkowej** szablonu, w którym użytkownik może wybrać ikonę usługi Facebook, Twitter lub Windows Live, aby włączyć logowanie przy użyciu zewnętrznego konta:

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

Można włączyć OAuth i OpenID członkostwa przy użyciu zaledwie kilku wierszy kodu. Metody i właściwości można używać do pracy z uwierzytelniania OAuth i OpenID dostawców znajdują się w `WebMatrix.Security.OAuthWebSecurity` klasy.

Jednak zamiast przy użyciu kodu, aby włączyć logowania z innych witryn, zalecanym sposobem wprowadzenie do nowych dostawców jest, aby użyć nowego **witryny początkowej** szablon, który jest dołączony do programu WebMatrix 2 Beta. **Witryny początkowej** szablon zawiera następujące elementy infrastruktury pełnego członkostwa, ze strony logowania, bazy danych członkostwa i cały kod należy umożliwić użytkownikom logowanie do witryny przy użyciu poświadczeń lokalnych lub z innej lokacji .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Jak włączyć logowania przy użyciu protokołu OAuth i dostawcy OpenID

Ta sekcja zawiera przykładowy sposób zezwolić użytkownikom na logowanie z zewnętrznych witryn (Facebook, Twitter, Windows Live, Google lub Yahoo) do lokacji, która opiera się na **witryny początkowej** szablonu. Po utworzeniu witryny początkowej, należy wykonać ten (szczegóły poniżej):

- Dla witryn, które korzystają z dostawcy uwierzytelniania OAuth (Facebook, Twitter i Windows Live) należy utworzyć aplikację w witrynie zewnętrznej. Dzięki temu klucze aplikacji, które należy w celu wywołania funkcji logowania dla tych witryn. Dla witryn, które korzystają z dostawcy openid o nazwie (Google, Yahoo) nie masz do tworzenia aplikacji. Dla wszystkich tych lokacji musi mieć konto, aby się zalogować i tworzenie aplikacji dla deweloperów. 

    > [!NOTE]
    > Aplikacje Windows Live akceptować tylko na żywo adresu URL dla roboczej witryny sieci Web, więc nie możesz użyć adresu URL lokalną witrynę sieci Web do testowania nazwy logowania.
- Edytuj kilka plików w witrynie sieci Web, aby określić dostawcę uwierzytelniania i przesłanie logowania do witryny, do której chcesz użyć.

**Aby włączyć logowania Google i Yahoo**:

1. W swojej witrynie sieci Web, należy edytować  *\_AppStart.cshtml* strony i dodaj następujące dwa wiersze kodu w bloku kodu Razor po wywołaniu `WebSecurity.InitializeDatabaseConnection` metody. Ten kod umożliwia dostawcom Google i Yahoo OpenID. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. W *~/Account/Login.cshtml* strony, Usuń komentarze spośród następujących `<fieldset>` bloku znaczników w końcowej części strony. Aby Usuń komentarz kodu, należy usunąć `@*` znaki, które są przed i po `<fieldset>` bloku. Wynikowy blok kodu wygląda następująco:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Dodaj `<input>` elementu przez dostawcę Google lub Yahoo `<fieldset>` w *~/Account/Login.cshtml* strony. Zaktualizowany interfejs `<fieldset>` grupy za pomocą `<input>` elementy Google i Yahoo wygląda jak w przykładzie poniżej: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. W *~/Account/AssociateServiceAccount.cshtml* strony, należy dodać `<input>` elementy Google lub Yahoo do `<fieldset>` grupy osiągnie koniec pliku. Możesz skopiować takie same `<input>` elementy, które właśnie został dodany do `<fieldset>` sekcji *~/Account/Login.cshtml* strony. 

    *~/Account/AssociateServiceAccount.cshtml* strony w szablonie witryny początkowej może być używany, jeśli chcesz utworzyć stronę, na którym użytkownicy mogą powiązać logowania z innych lokacji za pomocą jednego konta w witrynie sieci Web.

Teraz można przetestować logowania Google i Yahoo.

1. Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **Zaloguj** przycisku.
2. Na *logowania* stronie **Zaloguj się za pomocą innej usługi** sekcji, wybierają **Google** lub **Yahoo** przycisk Prześlij. W tym przykładzie użyto logowania usługi Google. 

    Strony sieci web przekierowuje żądanie do strony logowania firmy Google.

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Wprowadź poświadczenia dla istniejącego konta Google.
4. Jeśli Google zapyta, czy chcesz zezwolić Localhost informacjami z konta, kliknij pozycję **Zezwalaj**.

    Kod używa tokenu Google, aby uwierzytelnić użytkownika, a następnie wróci do tej strony w witrynie sieci Web. Ta strona umożliwia kojarzenie ich logowania usługi Google przy użyciu istniejącego konta w witrynie sieci Web lub mogą rejestrować nowe konto w witrynie do skojarzenia zewnętrznych danych logowania za pomocą.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Wybierz **skojarzyć** przycisku. Zwraca przeglądarce do strony głównej aplikacji.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Aby włączyć logowania do usługi Facebook**:

1. Przejdź do [Facebook deweloperów witryny](https://developers.facebook.com/apps) (Zaloguj się, jeśli jeszcze nie zalogowano Cię).
2. Wybierz **Utwórz nową aplikację** przycisk, a następnie postępuj zgodnie z monitami, aby utworzyć nową aplikację.
3. W sekcji **wybierz, jak Twoja aplikacja integruje się z usługi Facebook**, wybierz **witryny sieci Web** sekcji.
4. Wypełnij **adres URL witryny** pole adres URL witryny (na przykład [ `http://www.example.com` ](http://www.example.com)). **Domeny** pole jest opcjonalne; służy to do zapewnienia uwierzytelniania dla całej domeny (takie jak *example.com*). 

    > [!NOTE]
    > Jeśli używasz witryny na komputerze lokalnym przy użyciu adresu URL, takich jak `http://localhost:12345` (których liczba jest numer portu lokalnego), można dodać tę wartość, aby **adres URL witryny** pola do testowania witryny. Jednak każdy razem numer portu zmiany lokacji lokalnej, należy zaktualizować **adres URL witryny** pole aplikacji.
5. Wybierz **Zapisz zmiany** przycisku.
6. Wybierz **aplikacje** karcie ponownie, a następnie wyświetlić stronę początkową dla aplikacji.
7. Kopiuj **Identyfikatora aplikacji** i **klucz tajny aplikacji** wartości dla swojej aplikacji i wklej je do pliku tekstowego tymczasowych. W kodzie witryny sieci Web przekazuje te wartości do dostawcy usługi Facebook.
8. Zakończ działanie witryny dewelopera usługi Facebook.

Teraz możesz dokonać zmian dwie strony w witrynie sieci Web tak, aby użytkownicy będą mogli zalogować się do witryny za pomocą swoich kont usługi Facebook.

1. W swojej witrynie sieci Web, należy edytować  *\_AppStart.cshtml* strony, a następnie usuń komentarz kodu dla dostawcy uwierzytelniania Facebook OAuth. Blok kodu pozbawionym znaków komentarza wierszu wygląda podobnie do poniższego: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Kopiuj **Identyfikatora aplikacji** wartości z aplikacji usługi Facebook jako wartość `consumerKey` parametru (wewnątrz znaków cudzysłowu).
3. Kopiuj **klucz tajny aplikacji** wartość z aplikacji usługi Facebook jako `consumerSecret` wartość parametru.
4. Zapisz i zamknij plik.
5. Edytuj *~/Account/Login.cshtml* strony i usuwanie komentarzy z `<fieldset>` bloku w końcowej części strony. Aby Usuń komentarz kodu, należy usunąć `@*` znaki, które są przed i po `<fieldset>` bloku. Blok kodu z komentarzami usunięte wygląda następująco: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Zapisz i zamknij plik.

Teraz można przetestować logowania do usługi Facebook.

1. Uruchamianie witryny *default.cshtml* strony i wybierz **logowania** przycisku.
2. Na *logowania* stronie **Zaloguj się za pomocą innej usługi** wybierz pozycję **Facebook** ikony. 

    Strony sieci web przekierowuje żądanie do strony logowania usługi Facebook.

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Zaloguj się do konta w serwisie Facebook. 

    Kod używa tokenu usługi Facebook do uwierzytelniania, a następnie wróci do strony umożliwiające powiązanie logowania do usługi Facebook przy logowaniu do witryny. Adres nazwy lub adresu e-mail użytkownika jest wypełniony do **E-mail** pola w formularzu.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Wybierz **skojarzyć** przycisku. 

    Przeglądarka zwraca do strony głównej, a użytkownik jest zalogowany.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Aby włączyć logowania do usługi Twitter:** 

1. Przejdź do [Twitter deweloperów witryny](https://dev.twitter.com/).
2. Wybierz **tworzenie aplikacji** łącze, a następnie zaloguj się do witryny.
3. Na **tworzenia aplikacji** formularza, wypełnij **nazwa** i **opis** pola.
4. W **witryny sieci Web** wprowadź adres URL witryny (na przykład [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Jeśli testujesz witryny sieci lokalnie (przy użyciu adresu URL typu `http://localhost:12345`), Twitter, może nie zaakceptować adresu URL. Jednak można używać adresu IP lokalnego sprzężenia zwrotnego (na przykład `http://127.0.0.1:12345`). Upraszcza to proces testowania aplikacji w środowisku lokalnym. Jednak za każdym razem, gdy zmieni się numer portu lokacji lokalnej, należy zaktualizować **witryny sieci Web** pole aplikacji.
5. W **adresów URL wywołania zwrotnego** wprowadź adres URL dla strony witryny sieci Web, który ma użytkowników, aby powrócić do po zalogowaniu się do usługi Twitter. Na przykład wysłać do użytkowników na stronę główną witryny Starter (który będzie także rozpoznawał ich stan zalogowany), wprowadź ten sam adres URL, które wprowadziłeś w **witryny sieci Web** pola.
6. Zaakceptuj warunki i wybierz polecenie **tworzenie aplikacji usługi Twitter** przycisku.
7. Na **Moje aplikacje** początkowej strony, wybierz utworzoną aplikację.
8. Na **szczegóły** karty, przewiń w dół i wybierz **Utwórz Mój Token dostępu** przycisku.
9. Na **szczegóły** kartę, skopiuj **konsumenta** i **klucz tajny klienta** wartości dla swojej aplikacji i wklej je do pliku tekstowego tymczasowych. W kodzie witryny sieci Web będzie przekazać te wartości do dostawcy usługi Twitter.
10. Zakończ pracę w witrynie Twitter.

Teraz możesz dokonać zmian dwie strony w witrynie sieci Web tak, aby użytkownicy będą mogli zalogować się do witryny za pomocą swoich kont usługi Twitter.

1. W swojej witrynie sieci Web, należy edytować  *\_AppStart.cshtml* strony, a następnie usuń komentarz kodu dla dostawcy OAuth w usłudze Twitter. Blok pozbawionym znaków komentarza wierszu kodu wygląda następująco: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Kopiuj **konsumenta** wartość z aplikacji usługi Twitter jako wartość `consumerKey` parametru (wewnątrz znaków cudzysłowu).
3. Kopiuj **klucz tajny klienta** wartość z aplikacji usługi Twitter jako wartość `consumerSecret` parametru.
4. Zapisz i zamknij plik.
5. Edytuj *~/Account/Login.cshtml* strony i usuwanie komentarzy z `<fieldset>` bloku w końcowej części strony. Aby Usuń komentarz kodu, należy usunąć `@*` znaki, które są przed i po `<fieldset>` bloku. Blok kodu z komentarzami usunięte wygląda następująco: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Zapisz i zamknij plik.

Teraz można przetestować logowania usługi Twitter.

1. Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **logowania** przycisku.
2. Na *logowania* stronie **Zaloguj się za pomocą innej usługi** wybierz pozycję **Twitter** ikony. 

    Strony sieci web przekierowuje żądanie do strony logowania usługi Twitter dla aplikacji, który został utworzony.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Zaloguj się do konta w serwisie Twitter.
4. Kod używa token usługi Twitter w celu uwierzytelnienia użytkownika, a następnie powrót do strony umożliwiające powiązanie identyfikatora logowania przy użyciu konta z witryny sieci Web. Twoja nazwa lub adres e-mail jest wypełniana w **E-mail** pola w formularzu.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Wybierz **skojarzyć** przycisku. 

    Przeglądarka zwraca do strony głównej, a użytkownik jest zalogowany.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Dodawanie map przy użyciu Pomocnika mapy

Web Pages 2 obejmuje dodatki do bibliotekę pomocników platformy ASP.NET w sieci Web, czyli z pakietu dodatków dla witryny sieci Web Pages. Jednym z nich jest dostarczane przez składnik do mapowania `Microsoft.Web.Helpers.Maps` klasy. Możesz użyć `Maps` klasy, aby wygenerować mapy albo na podstawie adresu lub zbiór współrzędne długości i szerokości geograficznej. `Maps` Klasy pozwala wywołać bezpośrednio do aparatów popularnych mapy, takich jak Bing, Google, MapQuest i Yahoo.

Aby użyć nowego `Maps` klasy w swojej witrynie sieci Web, należy najpierw zainstalować wersji 2 usługi sieci Web bibliotekę pomocników. Aby to zrobić, przejdź do instrukcji dotyczących instalowania obecnie wersji, [bibliotekę pomocników sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) i zainstalować w wersji 2.

Kroki, aby dodać mapowanie do strony są takie same niezależnie od tego, który aparatów mapy wywołania. Wystarczy dodać odwołanie do pliku JavaScript do strony mapowanie, a następnie dodać wywołanie, który renderuje `<script>` tagów na stronie. Następnie na stronie mapowania, należy wywołać aparat mapy, którego chcesz użyć.

Poniższy przykład pokazuje, jak utworzyć strona, która powoduje wyświetlenie mapy na podstawie adresu i inną stronę, która powoduje wyświetlenie mapy na podstawie współrzędnych długości i szerokości geograficznej. Przykładowe mapowanie adresu przy użyciu map Google, a w przykładzie współrzędnych mapowania przy użyciu map Bing. Należy zwrócić uwagę następujących elementów w kodzie:

- Wywołanie `Assets.AddScript` w górnej części dwie strony mapowania. Metoda ta umożliwia dodanie odwołania do *jquery 1.6.2.min.js* pliku, który jest dołączony **witryny początkowej** szablonu i jest to wymagane `Maps` klasy.
- Wywołanie `Assets.GetScripts` metody w pliku układu. Ta metoda renderuje `<script>` tagiem na dwóch stronach mapowania.
- Wywołanie `@Maps.GetGoogleHtml` i `@Maps.GetBingHtml` metody na stronach mapowania. Aby zamapować adresu, należy przekazać ciąg adresu. Na współrzędne mapy, musisz przekazać długości i szerokości geograficznej współrzędnych. Aparat usługi mapy Bing, możesz też przekazać klucz (uzyskane za darmo, rejestrując się w [lokacji deweloperów map Bing](https://www.microsoft.com/maps/developers/web.aspx)). Metody silników mapy działają w podobny sposób (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Aby utworzyć mapowanie strony:

1. Tworzenie witryny sieci Web, na podstawie **witryny początkowej** szablonu.
2. Utwórz plik o nazwie *MapAddress.cshtml* w katalogu głównym witryny. Na tej stronie spowoduje wygenerowanie mapy na podstawie adresu i przekażesz do niego.
3. Skopiuj następujący kod do pliku, zastępując istniejącą zawartość. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Utwórz plik o nazwie  *\_MapLayout.cshtml* w katalogu głównym witryny. Ta strona będzie strony układu dla dwóch stronach mapowania.
5. Skopiuj następujący kod do pliku, zastępując istniejącą zawartość. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Utwórz plik o nazwie *MapCoordinates.cshtml* w katalogu głównym witryny. Na tej stronie spowoduje wygenerowanie mapy, w oparciu o zestaw współrzędnych, które przekazujesz do niego.
7. Skopiuj następujący kod do pliku, zastępując istniejącą zawartość. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Aby przetestować strony mapowania:

1. Uruchom stronę *MapAddress.cshtml* pliku.
2. Wprowadź ciąg zawierający pełny adres, w tym adres ulicy, stanu lub prowincji i kod pocztowy, a następnie wybierz **mapy go** przycisku. Strona powoduje wyświetlenie mapy z map Google: 

    [![topseven maphelper 1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Znajdź zestaw szerokość i długość geograficzną dla określonej lokalizacji.
4. Uruchom stronę *MapCoordinates.cshtml*. Należy wprowadzić współrzędne, a następnie wybierz **mapy go** przycisku. Strona powoduje wyświetlenie mapy z usługi mapy Bing: 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Uruchamianie aplikacji stron sieci Web obok siebie

Web Pages 2 dodaje możliwość uruchamiania aplikacji obok siebie. Dzięki temu można w dalszym ciągu uruchamiać aplikacje stron sieci Web 1, tworzyć nowe aplikacje sieci Web Pages 2 i uruchomić wszystkie z nich na tym samym komputerze.

Oto kilka rzeczy do zapamiętania, po zainstalowaniu wersji Beta 2 stron sieci Web za pomocą programu WebMatrix:

- Domyślnie istniejących aplikacji stron sieci Web działa jako aplikacji w wersji 2 na tym komputerze. (Zestawy w wersji 2 są zainstalowane w pamięci podręcznej GAC i będą używane automatycznie).
- Jeśli chcesz uruchomić lokacji za pomocą stron sieci Web w wersji 1 (zamiast domyślnej, tak jak w poprzednim punkcie), można skonfigurować witryny, aby to zrobić. Jeśli witryna nie ma jeszcze *web.config* plików w katalogu głównym witryny, Utwórz nową i skopiuj następujący kod XML do niego, zastępując istniejącą zawartość. Jeśli witryna zawiera już *web.config* Dodaj `<appSettings>` element podobny do następującego do `<configuration>` sekcji.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  "— Jeśli nie określisz wersji w *web.config* pliku lokacji jest wdrażany jako lokację w wersji 2. (Zestawy w wersji 2 są kopiowane do *bin* folder w witrynie wdrożone.)
- Nowe aplikacje za pomocą szablonów witryny w wersji Web Matrix 2 Beta obejmują zestawów stron sieci Web w wersji 2 w tej witrynie utworzyć *bin* folderu.

Ogólnie rzecz biorąc, zawsze można kontrolować wersję stron sieci Web za pomocą witryny za pomocą pakietu NuGet do instalowania odpowiednich zestawów w witrynie *bin* folderu. Pakietów można znaleźć [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Renderowanie stron dla urządzeń przenośnych

Web Pages 2 umożliwia tworzenie niestandardowych służy do renderowania zawartości na telefon komórkowy lub innych urządzeń.

`System.Web.WebPages` Przestrzeń nazw zawiera następujące klasy, które pozwalają pracować z trybów wyświetlania: `DefaultDisplayMode`, `DisplayInfo`, i `DisplayModes`. Można użyć bezpośrednio w ramach tych zajęć i napisać kod, który renderuje odpowiednie dane wyjściowe do konkretnych urządzeń.

Alternatywnie można utworzyć strony określonego urządzenia przy użyciu wzorca nazewnictwa plików następująco: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>. Na przykład można utworzyć dwie wersje strony, jeden o nazwie <em>MyFile.cshtml</em> i jedną o nazwie <em>MyFile.Mobile.cshtml</em>. W czasie, gdy urządzenie przenośne żąda wykonywania <em>MyFile.cshtml</em>, stron sieci Web renderuje zawartość z <em>MyFile.Mobile.cshtml</em>. W przeciwnym razie <em>MyFile.cshtml</em> jest renderowany.

Poniższy przykład pokazuje, jak umożliwiające renderowanie mobilnych, dodając strony zawartości dla urządzeń przenośnych. *Page1.cshtml* zawiera zawartość oraz pasek boczny nawigacji. *Page1.Mobile.cshtml* zawiera tę samą zawartość, ale pomija pasku bocznym.

Aby skompilować i uruchomić przykładowy kod:

1. W witrynie sieci Web Pages, Utwórz plik o nazwie *Page1.cshtml* i skopiuj *Page1.cshtml* zawartości do niego z przykładu.
2. Utwórz plik o nazwie *Page1.Mobile.cshtml* i skopiuj *Page1.Mobile.cshtml* zawartości do niego z przykładu. Należy zauważyć, że mobilnej wersji strony pomija lepsze renderowanie na mniejsze ekranu można znaleźć w sekcji nawigacji.
3. Uruchom przeglądarkę dla komputerów, a następnie przejdź do *Page1.cshtml*.
4. Uruchom w przeglądarce dla urządzeń przenośnych (lub w emulatorze urządzenia przenośnego), a następnie przejdź do *Page1.cshtml*. Zwróć uwagę, że, tym razem stron sieci Web renderuje mobilnej wersji strony. 

    > [!NOTE]
    > Aby przetestować stron dla urządzeń przenośnych, można użyć symulatora urządzenia przenośnego, uruchomionym na komputerze stacjonarnym. To narzędzie umożliwia testowanie stron sieci web tak, jak będą wyglądały na urządzeniach przenośnych (oznacza to, zwykle za pomocą znacznie mniejszy powoduje wyświetlenie obszaru). Przykład symulatora [przełącznik agenta użytkownika dodatku](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) dla Mozilla Firefox, który umożliwia emulowanie różnych przeglądarek dla urządzeń przenośnych z wersji klasycznej programu Firefox.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* renderowane przy użyciu przeglądarki na komputerze:

[![topseven displaymodes 1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* wyświetlane w widoku symulatora telefonu iPhone firmy Apple w przeglądarce Firefox. Nawet jeśli żądanie dotyczy *Page1.cshtml*, renderuje aplikacji *Page1.Mobile.cshtml*.

[![topseven displaymodes 2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

### <a name="aspnet-web-pages-1-resources"></a>ASP.NET Web Pages zasobów: 1

> [!NOTE]
> Większość programowania stron sieci Web 1 i zasobów interfejsu API nadal mają zastosowanie do stron sieci Web 2.

- [Wprowadzenie do programowania stron ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>Zasoby programu WebMatrix

- [Program WebMatrix 2 What's New](http://webmatrix.com/next)
- [Microsoft WebMatrix Site](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Tworzenie aplikacji sieci Web począwszy od programu Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(w tym kompleksowo przykładowej aplikacji stron sieci Web)
