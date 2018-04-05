---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: Funkcje górnej w składniku ASP.NET Web Pages 2 | Dokumentacja firmy Microsoft
author: microsoft
description: Ten temat zawiera omówienie Najważniejsze nowe funkcje w wersji 2 stron sieci Web ASP.NET, architektura programowania lekkie sieci web dołączonego WebMatr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: e8fc758936953970ff3e9ba289516925dee9ef45
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Najważniejsze funkcje w składniku ASP.NET Web Pages 2
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Ten artykuł zawiera omówienie Najważniejsze nowe funkcje w wersji RC 2 stron sieci Web ASP.NET, lekkie sieci web struktury programistycznej, która jest zawarta w [programu Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **Zawartość pakietu:** 
> 
> - [Instalowanie programu WebMatrix](#install)
> - [Nowe i ulepszone funkcje](#New_and_Enhanced_Features)
> 
>     - [Zmiany w wersji RC](#Changes_for_the_RC_Version)
>     - [Zmiany w wersji Beta](#Changes_for_the_Beta_Version)
>     - [Za pomocą szablonów nowych i zaktualizowanych lokacji](#templates)
>     - [Walidacja danych wejściowych użytkownika](#validation)
>     - [Włączanie logowania z usługi Facebook i innych lokacji za pomocą protokołu OAuth i OpenID](#oauthsetup)
>     - [Dodawanie mapy, przy użyciu Pomocnika mapy](#maphelper)
>     - [Uruchamianie aplikacji stron sieci Web obok siebie](#sidebyside)
>     - [Renderowanie stron dla urządzeń przenośnych](#mobile)
> - [Dodatkowe zasoby](#resources)
> 
> > [!NOTE]
> > W tym temacie założono, że używasz programu WebMatrix do pracy z kodu ASP.NET Web Pages 2. Jednak z 1 stron sieci Web, podczas tworzenia witryn sieci Web 2 stron sieci Web przy użyciu programu Visual Studio, które zapewnia udoskonalone funkcje IntelliSense i debugowania. Aby pracować ze stronami sieci Web w programie Visual Studio, należy najpierw zainstalować program Visual Studio 2010 z dodatkiem SP1, Visual Web Developer Express 2010 z dodatkiem SP1 lub programu Visual Studio 11 Beta. Następnie zainstaluj platforma ASP.NET Beta 4 MVC, który zawiera szablony i narzędzia do tworzenia aplikacji ASP.NET MVC 4 i 2 stron sieci Web w programie Visual Studio.
> 
> 
> *Ostatnia aktualizacja: 18 czerwca 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Instalowanie programu WebMatrix

Aby zainstalować stron sieci Web, należy użyć Instalatora platformy sieci Web firmy Microsoft, który jest bezpłatną aplikację, który można łatwo zainstalować i skonfigurować technologii związanych z sieci web. Zainstaluje Beta 2 programu WebMatrix, w tym Beta 2 stron sieci Web.

1. Przejdź do strony instalacji najnowszą wersję Instalatora platformy sieci Web:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Jeśli masz już zainstalowany program WebMatrix 1, ta instalacja aktualizacji do wersji Beta 2 programu WebMatrix. Można uruchamiać witryn sieci Web, które zostały utworzone przy użyciu wersji 1 lub 2 na tym samym komputerze. Aby uzyskać więcej informacji, zobacz sekcję dotyczącą [aplikacji stron sieci Web uruchomiona obok siebie](#sidebyside).
2. Wybierz **teraz zainstalować**. 

    Jeśli używasz programu Internet Explorer, przejdź do następnego kroku. Jeśli używasz innej przeglądarki, takich jak Mozilla Firefox lub Google Chrome, monit o zapisanie *Webmatrix.exe* plik na swoim komputerze. Zapisz plik, a następnie kliknij go, aby uruchomić Instalatora.
3. Uruchom Instalatora i wybrać **zainstalować** przycisku. Spowoduje to zainstalowanie programu WebMatrix i stron sieci Web.

## <a id="New_and_Enhanced_Features"></a>Nowe i ulepszone funkcje

### <a id="Changes_for_the_RC_Version"></a>Zmiany dotyczące wersji RC (czerwiec 2012)

Wersja wersji RC w czerwca 2012 ma kilka zmian odświeżanie wersji Beta, wydanej w marcu 2012. Te zmiany są:

- A `Validation.AddFormError` metody został dodany do `Validation` pomocnika. Jest to przydatne, jeśli ręcznie przeprowadzić weryfikacji (na przykład możesz zweryfikować wartość, która jest przekazywany w ciągu zapytania) i chcesz dodać komunikat o błędzie, który może być wyświetlany przez `Html.ValidationSummary` metody. Aby uzyskać więcej informacji, zobacz sekcję [sprawdzanie poprawności danych czy nie pochodzi bezpośrednio z użytkowników](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) w [sprawdzanie poprawności danych wejściowych użytkownika w witrynach składnika ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253002).
- Funkcja tworzenie pakietów i minimalizowanie został usunięty z zestawów podstawowych ASP.NET Web Pages 2. W konsekwencji `Assets` pomocy wymienionych w dalszej części tego dokumentu jest niedostępny. Zamiast tego należy zainstalować [optymalizacji ASP.NET](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) pakietu NuGet. Aby uzyskać więcej informacji, zobacz [tworzenie pakietów i zminimalizowania liczby zasoby w witrynie stron sieci Web platformy ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=255373).
- Dodano następującej liczby dodatkowych zestawów do obsługi programu ASP.NET Web Pages 2. Tylko zauważalnego wpływu tej zmiany jest napotkać więcej zestawów w witrynie *bin* folder po utworzeniu witryny lub wdrożyć witrynę do dostawcy hostingu.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Zmiany w wersji Beta (luty 2012)

Wersji Beta, wydanej w lutego 2012 zawiera tylko kilka zmian z wersji Beta, który został wydany w grudnia 2011. Te zmiany są:

- Razor obsługuje teraz atrybuty warunkowe. W kodzie HTML elementu, jeśli atrybut zostanie ustawiony na wartość która rozpoznaje w kodzie serwera do `false` lub `null`, ASP.NET nie jest renderowana atrybutu w ogóle. Załóżmy na przykład, że masz następujące znacznika pola wyboru:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Jeśli wartość `checked1` jest rozpoznawana jako `false` lub `null`, `checked` atrybut nie jest renderowany. Jest to istotne zmiany.
- `Validation.GetHtml` Metody została zmieniona na `Validation.For`. Jest to istotne zmiany; `Validation.GetHtml` nie będzie działać w wersji Beta.
- Obecnie można uwzględnić `~` odwołania katalogu głównego witryny bez użycia operatora znaczników `Href` funkcji. (To znaczy analizator Razor można teraz znajdowania i usuwania `~` operator bez konieczności wywołanie metody jawne `Href`.) `Href` Metoda nadal działa, więc nie jest to istotne zmiany.

    Na przykład, jeśli uprzednio znaczników w następujący sposób:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Teraz można użyć znaczników w następujący sposób:

    `<a href="~/Default.cshtml">Home</a>`
- `Scripts` Pomocnika do zarządzania zasobami (zasobów) zostało zastąpione `Assets` pomocnika, który ma nieco inne metody, takie jak następujące:

    - Aby uzyskać `Scripts.Add`, użyj`Assets.AddScript`
    - Aby uzyskać `Scripts.GetScriptTags`, użyj`Assets.GetScripts`

    Jest to istotne zmiany; `Scripts` klasa nie jest dostępna w wersji Beta. Przykłady kodu w tym dokumencie, które używają zarządzania zasobami zostały zaktualizowane z tą zmianą.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Za pomocą szablonów nowych i zaktualizowanych lokacji

**Witryny początkowej** szablon został uaktualniony, aby była uruchamiana na stronach sieci Web 2 domyślnie. Zawiera również następujące nowe funkcje:

- Renderowanie strony przyjaznych dla urządzeń przenośnych. Przy użyciu stylów CSS oraz `@media` selektora, **witryny początkowej** udostępnia udoskonalone renderowania stron na mniejszych ekranach, w tym ekrany urządzeń przenośnych.
- Ulepszone opcje członkostwa i uwierzytelniania. Możesz pozwolić użytkowników logowania do witryny przy użyciu konta z innymi witrynami sieci społecznościowych, takich jak usługi Twitter, Facebook i usługi Windows Live. Aby uzyskać więcej informacji, zobacz [Włączanie logowania do usługi Facebook i innych lokacji za pomocą protokołu OAuth i OpenID](#oauthsetup) sekcji.
- Elementy HTML5.

Nowy **witryny osobistej** szablon umożliwia tworzenie witryn sieci Web zawierający osobisty blog, strona zdjęć i strony Twitter. Można dostosować witrynę na podstawie **witryny osobistej** szablonu, wykonując następujące czynności:

- Zmiana wyglądu witryny, edytując plik układu (*\_SiteLayout.cshtml*) i pliku style (*Site.css*).
- Instalowanie pakietów NuGet, które Dodawanie funkcji do swojej witryny. Aby uzyskać informacje o sposobie instalowania pakietów, bibliotekę pomocników platformy ASP.NET w sieci Web, w tym zobacz samouczek [instalowanie pomocników](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Aby uzyskać dostęp do **witryny osobistej** szablonu, wybierz **szablony** na WebMatrix **Szybki Start** ekranu.

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

W **szablony** oknie dialogowym wybierz **witryny osobistej** szablonu.

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

Strona docelowa z **witryny osobistej** szablon umożliwia skorzystaj z linków, aby skonfigurować blogu, Twitter, strony i strona zdjęć.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Walidacja danych wejściowych użytkownika

1 stron sieci Web, sprawdzanie poprawności danych wejściowych użytkownika dla przesłanych formularzy, należy użyć `System.Web.WebPages.Html.ModelState` klasy. (Jest to zilustrowane w kilka przykładów kodu w samouczku 1 stron sieci Web zatytułowany [Praca z danymi](../data/5-working-with-data.md).) Można nadal używać tej metody w wersji 2 stron sieci Web. Strony sieci Web 2 oferuje również ulepszone narzędzia do sprawdzania poprawności danych wejściowych użytkownika:

- Nowe klasy weryfikacji, w tym `System.Web.WebPages.ValidationHelper` i `System.Web.WebPages.Validator`, których można wykonywać zadania zaawansowanego weryfikacji przy użyciu kilku wierszy kodu.
- Opcjonalnie weryfikacji po stronie klienta, które zapewnia natychmiast uzyskuje opinie użytkownikowi zwalniając obiegu do serwera do sprawdzania błędów sprawdzania poprawności. (Ze względów bezpieczeństwa sprawdzanie poprawności jest wykonywane na serwerze nawet wtedy, gdy kontroli mogły zostać wykonane na komputerze klienckim wcześniej).

Aby korzystać z nowych funkcji sprawdzania poprawności, wykonaj następujące czynności:

W kodzie strony należy zarejestrować element do sprawdzenia poprawności przy użyciu metody `Validation` pomocnika: `Validation.RequireField`, `Validation.RequireFields` (Aby zarejestrować wiele elementów wymagana), lub `Validation.Add`. `Add` Metoda pozwala określić inne rodzaje sprawdzanie poprawności, takie jak typ danych sprawdzania, porównanie wpisów w różnych pól, sprawdza długość ciągu i wzorce (za pomocą wyrażeń regularnych). Oto kilka przykładów:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Aby wyświetlić błąd specyficznych, należy wywołać `Html.ValidationMessage` w znaczniku dla każdego elementu sprawdzania poprawności:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Aby wyświetlić podsumowanie (`<ul>` listy) wszystkie błędy na stronie `Html.ValidationSummary` w kod znaczników:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Te kroki są, aby zaimplementować weryfikację po stronie serwera. Jeśli chcesz dodać weryfikację po stronie klienta, wykonaj także następujące czynności.

Dodaj następujące odwołania do pliku skryptu wewnątrz `<head>` części strony sieci web. Pierwszych dwóch odwołań do skryptów wskaż zdalnego plików na sieciowym dostarczania zawartości (CDN). Trzeci punktów odniesienia do pliku lokalnego skryptu.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

Najprostszym sposobem uzyskania kopii lokalnej *jquery.validate.unobtrusive.min.js* biblioteki jest utworzenie nowej lokacji stron sieci Web na podstawie jednej z szablonów lokacji (np. witryny początkowej). Zawiera ona utworzona przez szablon *jquery.validate.unobtrusive.js* pliku w jego folderze skryptów, z którego można skopiować go do swojej witryny.

Jeśli korzysta z witryny sieci Web*\_SiteLayout* strony do sterowania układem strony, możesz dołączyć tych odwołań do skryptów na tej stronie, dzięki czemu sprawdzania poprawności jest dostępna dla wszystkich stron zawartości. Jeśli chcesz sprawdzania poprawności tylko dla określonej strony, można użyć Menedżera zasobów zarejestrować skryptów na tylko tych stron. Aby to zrobić, należy wywołać `Assets.AddScript(path)` w strony, którą chcesz zweryfikować i odwoływać się każdy z plików skryptów. Następnie dodaj wywołanie do `Assets.GetScripts` w  *\_SiteLayout* strony do renderowania zarejestrowaną `<script>` tagów. Aby uzyskać więcej informacji, zobacz sekcję [rejestrowanie skryptów z Menedżerem zasobów](#resmanagement).

W znaczniku dla pojedynczego elementu, należy wywołać `Validation.For` metody. Ta metoda emituje atrybutów tego jQuery można utworzenie punktu zaczepienia w celu udostępnienia weryfikacji po stronie klienta. Na przykład:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

W poniższym przykładzie przedstawiono strona, która sprawdza poprawność danych wejściowych użytkownika na formularzu. Aby uruchomić i przetestowania tego kodu sprawdzania poprawności, wykonaj następujące czynności:

1. Utwórz nową witrynę sieci web przy użyciu jednego z szablonów witryny programu WebMatrix 2, które obejmuje *skryptów* folder, taki jak **witryny początkowej** szablonu.
2. W nowej lokacji, Utwórz nową *.cshtml* strony i Zastąp zawartość strony z następującym kodem.
3. Uruchom strony w przeglądarce. Wprowadź prawidłowe oraz nieprawidłowe wartości, aby wyświetlić wyniki weryfikacji. Na przykład wymagane pole puste, lub wprowadź literę w **środków** pola.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Oto strony, gdy użytkownik prześle prawidłowe wartości wejściowe:

[![topSeven nieprawidłowy-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Gdy użytkownik prześle on wymagane pole pozostanie puste, Oto strony:

[![topSeven nieprawidłowy-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Oto strony, gdy użytkownik prześle ją z inną niż całkowitą **środków** pola:

[![topSeven nieprawidłowy-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Aby uzyskać więcej informacji zobacz następujących wpisach w blogu:

- [Zaktualizowano sprawdzanie poprawności w wersji 2 stron sieci Web](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) podstawy Dodawanie sprawdzania poprawności przy użyciu `Validation` pomocnika (po stronie serwera tylko)
- [Zaktualizowano sprawdzanie poprawności w wersji 2 stron sieci Web, część 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) Dodawanie walidacji po stronie klienta.
- [Zaktualizowano sprawdzanie poprawności w wersji 2 stron sieci Web, część 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) formatowania błędy sprawdzania poprawności.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>Zarejestrowanie skryptów za pomocą Menedżera zasobów

Menedżer zasobów jest nowa funkcja, która umożliwia w kodzie serwera zarejestrować i renderowania skrypty klienta. Ta funkcja jest przydatne podczas pracy z kodem z wielu plików (takich jak układ strony, strony z zawartością, pomocników, itp.), które są połączone w jedną stronę w czasie wykonywania. Menedżer zasobów koordynuje pliki źródłowe, aby upewnić się, że pliki skryptów są nieprawidłowe odwołania do wydajnie na renderowanej stronie niezależnie od tego, które pliki kodu zostaną wywołane i jak często są one nazywane. Menedżer zasobów również renderuje `<script>` tagów w odpowiednim miejscu, aby szybko (bez pobierania skrypty podczas renderowania) można załadować strony i w celu uniknięcia błędów, które mogą wystąpić, jeśli skrypty są wywoływane przed renderowanie jest pełny.

Na przykład załóżmy, że utworzenie niestandardowego pomocnika, który wywołuje plik JavaScript i wywołania tego pomocnika w trzech różnych miejscach w kodzie strony zawartości. Jeśli nie używasz Menedżera zasobów, aby zarejestrować pomocnika, trzech różnych odwołuje się skrypt `<script>` tagi wyświetlane wszystkie punktu do tego samego pliku skryptu w renderowanej strony. Ponadto w zależności od tego, gdzie `<script>` tagi są wstawiane na renderowanej stronie, mogą wystąpić błędy, jeśli skrypt próbuje uzyskać dostęp niektórych elementów strony, przed pełni ładowania strony. Jeśli używasz Menedżera zasobów do zarejestrowania skryptu, można uniknąć tych problemów.

Dzięki temu można zarejestrować skryptu z Menedżerem zasobów:

- W kodzie, który musi odwoływać się skrypt, wywołaj `Assets.AddScript` metody.
- W  *\_SiteLayout* strony, należy wywołać `Assets.GetScripts` metody do renderowania `<script>` tagów. 

    > [!NOTE]
    > Umieść wywołań `Assets.GetScripts` jako bardzo ostatniego elementu w `<body>` elementu  *\_SiteLayout* strony. Dzięki temu strony są ładowane szybciej i uniknięcie błędów skryptów.

W poniższym przykładzie pokazano, jak działa Menedżera zasobów. Kod zawiera następujące elementy:

- Obiekt pomocnika niestandardowy o nazwie `MakeNote`. Tego pomocnika renderuje ciąg wewnątrz pola zawijania `div` element wokół niego który ma styl obramowania, a przez dodanie &quot;Uwaga:&quot; do niego. Pomocnik wywołuje również plik JavaScript, który dodaje zachowanie czasu wykonania do uwagi. Zamiast odwołania skryptu `<script>` tagu pomocnika rejestruje skrypt przez wywołanie metody `Assets.AddScript` .
- Plik JavaScript. To jest plik, który jest wywoływany przez pomocnika i tymczasowo zwiększa rozmiar czcionki elementów Uwaga Podczas `mouseover` zdarzeń.
- Strony zawartości, który odwołuje się*\_SiteLayout* strony, renderuje zawartość w treści, a następnie wywołuje `MakeNote` pomocnika.
- A  *\_SiteLayout* strony. Ta strona zawiera typowe nagłówka i struktura układ strony. Zawiera również wywołanie `Assets.GetScripts`, czyli jak Menedżer zasobów renderuje skryptu wywołuje na stronie.

Aby uruchomić przykład:

1. Utwórz pusty witryny sieci Web 2 stron sieci Web. Korzystając z programu WebMatrix **pusta witryna** szablon dla tego.
2. Utwórz folder o nazwie *skryptów* w lokacji.
3. W *skryptów* folderu, Utwórz plik o nazwie *Test.js*, kopiowania *Test.js* zawartości do niego z przykładu i Zapisz plik.
4. Utwórz folder o nazwie *aplikacji\_kodu* w lokacji.
5. W *aplikacji\_kod* folderu, Utwórz plik o nazwie *Helpers.cshtml*, a następnie skopiuj do niego przykładowy kod i zapisz go w folderze o nazwie *aplikacji\_kodu*w folderze głównym.
6. W folderze głównym lokacji, Utwórz plik o nazwie  *\_SiteLayout.cshtml,* skopiuj do niego przykładzie i Zapisz plik.
7. W katalogu głównym witryny tak, Utwórz plik o nazwie *ContentPage.cshtml*, Dodaj przykładowy kod i zapisz go.
8. Uruchom *wartość ContentPage* w przeglądarce. Ciąg przekazany do `MakeNote` pomocnika jest renderowane jako Uwaga ramce.
9. Zatrzymaj wskaźnik myszy na uwagi. Skrypt tymczasowo spowoduje zwiększenie rozmiaru czcionki uwagi.
10. Wyświetl źródło renderowanej strony. Z powodu rozmieszczenia wywołanie `Assets.GetScripts`, renderowanych `<script>` tag, który wywołuje *Test.js* jest bardzo ostatniego elementu w treści strony.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

Poniższy zrzut ekranu przedstawia *ContentPage.cshtml* w przeglądarce po kursora myszy nad uwagi:

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Włączanie logowania z usługi Facebook i innych lokacji za pomocą protokołu OAuth i OpenID

Strony sieci Web 2 udostępnia rozszerzone opcje członkostwa i uwierzytelniania. Głównym ulepszenie jest, że istnieją nowe [OAuth](http://oauth.net/) i [OpenID](http://openid.net/) dostawców. Przy użyciu tych dostawców, możesz pozwolić, aby użytkowników logowania do witryny przy użyciu swoich istniejących poświadczeń z usługi Facebook, Twitter, Windows Live, Google i Yahoo. Na przykład aby zalogować się przy użyciu konta usługi Facebook, użytkowników można po prostu ikonę usługi Facebook, który przekierowuje go do strony logowania usługi Facebook, którym oni wprowadzić swoje informacje o użytkowniku. Można następnie skojarzyć logowania serwisu Facebook do swojego konta w witrynie. Powiązane rozszerzenie do funkcji przynależności stron sieci Web jest czy użytkownicy mogą powiązać wiele logowań (w tym logowania z witrynami sieci społecznościowych) z jednego konta w witrynie sieci Web.

Ten obraz zawiera strony logowania z **witryny początkowej** szablon, w którym użytkownik może wybrać ikonę Facebook, Twitter lub identyfikatora Windows Live, aby włączyć logowanie przy użyciu zewnętrznego konta:

[![topSeven oauth 1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

Można włączyć protokołu OAuth i OpenID członkostwa przy użyciu kilku wierszy kodu. Metody i właściwości można używać do pracy z uwierzytelniania OAuth i OpenID dostawców znajdują się w `WebMatrix.Security.OAuthWebSecurity` klasy.

Jednak zamiast włączenia logowania z innych lokacji za pomocą kodu zalecany sposób, aby zacząć korzystać z nowych dostawców jest do używania nowych **witryny początkowej** szablonu, która jest zawarta w wersji Beta 2 programu WebMatrix. **Witryny początkowej** szablon zawiera infrastruktury pełnego członkostwa, strony logowania, bazy danych członkostwa oraz cały kod należy powiadomić użytkowników logowania do witryny przy użyciu poświadczeń lokalnych lub z innej lokacji .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Włączanie logowania przy użyciu protokołu OAuth i OpenID dostawców

Ta sekcja zawiera przykładowy sposób umożliwić użytkownikom logowania z zewnętrznych witryn (Facebook, Twitter, Windows Live, Google lub Yahoo) do lokacji, która jest oparta na **witryny początkowej** szablonu. Po utworzeniu witryny początkowej, możesz wykonać tego (szczegóły poniżej):

- Dla witryn, które korzystają z dostawcy uwierzytelniania OAuth (Facebook, Twitter i usługi Windows Live) należy utworzyć aplikację w witrynie zewnętrznych. Udostępnia klucze aplikacji, które będą potrzebne, aby można było wywołać funkcję logowania dla tych witryn. Dla witryn, które korzystają z dostawcy uwierzytelniania OpenID (Google, Yahoo) nie trzeba tworzyć aplikacji. Wszystkie te witryny musi mieć konto, aby zalogować się i utworzyć aplikacjami dla deweloperów. 

    > [!NOTE]
    > Aplikacje Windows Live akceptować tylko na żywo adres URL witryny sieci Web pracy, więc nie można używać adresu URL lokalną witrynę sieci Web do testowania logowania.
- Edytuj kilka plików w witrynie sieci Web, aby określić dostawcę uwierzytelniania i przesłać dane logowania do witryny, której chcesz użyć.

**Aby włączyć logowania Google i Yahoo**:

1. W witrynie sieci Web, należy edytować  *\_AppStart.cshtml* strony i dodaj następujące dwa wiersze kodu w bloku kodu Razor po wywołaniu `WebSecurity.InitializeDatabaseConnection` metody. Ten kod umożliwia dostawcom Google i Yahoo OpenID. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. W *~/Account/Login.cshtml* pozycję Usuń komentarze z następujących `<fieldset>` bloku znaczników zbliża się koniec strony. Aby Usuń komentarz kodu, Usuń `@*` znaki, które przed i po `<fieldset>` bloku. Wynikowa blok kodu wygląda następująco:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Dodaj `<input>` element dostawcę Google lub Yahoo `<fieldset>` w *~/Account/Login.cshtml* strony. Zaktualizowany interfejs `<fieldset>` z `<input>` elementów dla wygląda zarówno Google i Yahoo, takich jak w poniższym przykładzie: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. W *~/Account/AssociateServiceAccount.cshtml* Dodaj `<input>` elementy Google lub Yahoo do `<fieldset>` grupy zbliża się koniec pliku. Możesz skopiować takie same `<input>` elementy, które zostały dodane tylko do `<fieldset>` sekcji *~/Account/Login.cshtml* strony. 

    *~/Account/AssociateServiceAccount.cshtml* strony w szablonie witryny początkowej mogą być używane, gdy chcesz utworzyć strony, na którym użytkownicy mogą powiązać wiele logowania z innych lokacji za pomocą jednego konta w witrynie sieci Web.

Teraz możesz przetestować logowania Google i Yahoo.

1. Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **Zaloguj** przycisku.
2. Na *logowania* strony w **Zaloguj się za pomocą innej usługi** albo wybierz **Google** lub **Yahoo** przycisk Prześlij. W tym przykładzie użyto logowania Google. 

    Strony sieci web przekierowuje żądanie do strony logowania usługi Google.

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Wprowadź poświadczenia dla istniejącego konta Google.
4. Jeśli Google zapyta, czy chcesz zezwolić Localhost informacje z konta, kliknij **Zezwalaj**.

    Kod używa tokenu Google do uwierzytelniania użytkownika, a następnie wróci do tej strony w witrynie sieci Web. Ta strona umożliwia użytkownikom skojarzyć ich Google logowania przy użyciu istniejącego konta w witrynie sieci Web lub ich zarejestrować nowe konto w witrynie do skojarzenia zewnętrznych danych logowania z.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Wybierz **skojarzyć** przycisku. Zwraca przeglądarki do strony głównej aplikacji.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Aby włączyć logowania do usługi Facebook**:

1. Przejdź do [lokacji deweloperzy Facebook](https://developers.facebook.com/apps) (dziennik, jeśli jeszcze nie jest zalogowany).
2. Wybierz **Utwórz nową aplikację** przycisk, a następnie postępuj zgodnie z monitami, aby utworzyć nową aplikację.
3. W sekcji **wybierz, jak aplikacja zostanie zintegrowana z usługą Facebook**, wybierz **witryny sieci Web** sekcji.
4. Wypełnij **adres URL witryny** pole adresu URL witryny (na przykład [ `http://www.example.com` ](http://www.example.com)). **Domeny** pole jest opcjonalne; umożliwia to zapewnia uwierzytelnianie dla całej domeny (takich jak *example.com*). 

    > [!NOTE]
    > Jeśli używasz lokacji na komputerze lokalnym przy użyciu adresu URL, takich jak `http://localhost:12345` (gdzie numer jest numerem portu lokalnego), można dodać tę wartość na **adres URL witryny** pole do testowania witryny. Jednak każdy razem numer portu lokacji lokalnej zmiany, musisz zaktualizować **adres URL witryny** pole aplikacji.
5. Wybierz **Zapisz zmiany** przycisku.
6. Wybierz **aplikacji** karcie ponownie, a następnie Wyświetl strony początkowej aplikacji.
7. Kopiuj **identyfikator aplikacji** i **klucz tajny aplikacji** wartości dla aplikacji i wklej je do pliku tekstowego tymczasowego. Te wartości zostaną spełnione dla dostawcy usługi Facebook w kodzie witryny sieci Web.
8. Zamknij stronę dewelopera usługi Facebook.

Teraz możesz zmienić dwie strony w witrynie sieci Web, dzięki czemu użytkownicy będą może logować się do witryny za pomocą ich kont usługi Facebook.

1. W witrynie sieci Web, należy edytować  *\_AppStart.cshtml* strony i Usuń komentarz kodu dla dostawcy uwierzytelniania Facebook OAuth. Blok kodu uncommented wygląda następująco: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Kopiuj **identyfikator aplikacji** wartość z zakresu od aplikacji usługi Facebook jako wartość `consumerKey` parametr (wewnątrz cudzysłowów).
3. Kopiuj **klucz tajny aplikacji** wartość z zakresu od aplikacji usługi Facebook jako `consumerSecret` wartość parametru.
4. Zapisz i zamknij plik.
5. Edytuj *~/Account/Login.cshtml* strony i Usuń komentarze z `<fieldset>` bloku zbliża się koniec strony. Aby Usuń komentarz kodu, Usuń `@*` znaki, które przed i po `<fieldset>` bloku. Blok kodu z komentarzami usunięte wygląda następująco: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Zapisz i zamknij plik.

Teraz możesz przetestować logowania usługi Facebook.

1. Uruchom witrynę *default.cshtml* strony i wybierz polecenie **logowania** przycisku.
2. Na *logowania* strony w **Zaloguj się za pomocą innej usługi** wybierz **Facebook** ikony. 

    Strony sieci web przekierowuje żądanie do strony logowania usługi Facebook.

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Zaloguj się do konta usługi Facebook. 

    Kod używa tokenu usługi Facebook do uwierzytelniania, a następnie zwraca stronę możesz skojarzyć nazwę użytkownika usługi Facebook z logowania w witrynie. Adres nazwę lub adres e-mail użytkownika jest wprowadzany do **E-mail** pola formularza.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Wybierz **skojarzyć** przycisku. 

    Zwraca przeglądarki do strony głównej i użytkownik jest zalogowany.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Aby włączyć logowania do usługi Twitter:** 

1. Przejdź do [lokacji deweloperzy Twitter](https://dev.twitter.com/).
2. Wybierz **Utwórz aplikację** łącza, a następnie zaloguj się do witryny.
3. Na **tworzenie aplikacji** tworzą, wypełnij **nazwa** i **opis** pola.
4. W **witryny sieci Web** wprowadź adres URL witryny sieci (na przykład [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Jeśli testujesz witryny lokalnie (przy użyciu adresu URL, takie jak `http://localhost:12345`), Twitter, adres URL nie może zaakceptować. Jednak można używać lokalnego sprzężenia zwrotnego adresu IP (na przykład `http://127.0.0.1:12345`). Upraszcza proces testowanie aplikacji lokalnie. Jednak za każdym razem, gdy zmienia numer portu witryny lokalnej, należy zaktualizować **witryny sieci Web** pole aplikacji.
5. W **wywołania zwrotnego adresu URL** wprowadź adres URL strony w witrynie sieci Web, które użytkowników, aby powrócić do po zalogowaniu w serwisie Twitter. Na przykład w celu wysłania użytkownikom do strony głównej witryny Starter (który rozpozna ich stan w zarejestrowany), wprowadź ten sam adres URL wprowadzony w **witryny sieci Web** pola.
6. Zaakceptuj postanowienia, a następnie wybierz **tworzenie aplikacji Twitter** przycisku.
7. Na **Moje aplikacje** początkowej strony, wybierz utworzoną aplikację.
8. Na **szczegóły** kartę, przewiń w dół i wybierz polecenie **Utwórz moje Token dostępu** przycisku.
9. Na **szczegóły** karcie, skopiuj **konsumenta** i **klucz tajny klienta** wartości dla aplikacji i wklej je do pliku tekstowego tymczasowego. W kodzie witryny sieci Web będzie przekazywać te wartości dostawcy usługi Twitter.
10. Zakończ witrynie Twitter.

Teraz możesz zmienić dwie strony w witrynie sieci Web, dzięki czemu użytkownicy będą mogli zalogować się do witryny za pomocą ich kont usługi Twitter.

1. W witrynie sieci Web, należy edytować  *\_AppStart.cshtml* strony i Usuń komentarz kodu dla dostawcy usługi Twitter OAuth. Blok kodu uncommented wygląda następująco: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Kopiuj **konsumenta** wartość z zakresu od aplikacji Twitter jako wartość `consumerKey` parametr (wewnątrz cudzysłowów).
3. Kopiuj **klucz tajny klienta** wartość z zakresu od aplikacji Twitter jako wartość `consumerSecret` parametru.
4. Zapisz i zamknij plik.
5. Edytuj *~/Account/Login.cshtml* strony i Usuń komentarze z `<fieldset>` bloku zbliża się koniec strony. Aby Usuń komentarz kodu, Usuń `@*` znaki, które przed i po `<fieldset>` bloku. Blok kodu z komentarzami usunięte wygląda następująco: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Zapisz i zamknij plik.

Teraz możesz przetestować logowania usługi Twitter.

1. Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **logowania** przycisku.
2. Na *logowania* strony w **Zaloguj się za pomocą innej usługi** wybierz **Twitter** ikony. 

    Strony sieci web przekierowuje żądanie do strony logowania usługi Twitter dla aplikacji, który został utworzony.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Zaloguj się do konta w usłudze Twitter.
4. Kod używa tokenu usługi Twitter do uwierzytelnienia użytkownika i następnie powrót do strony możesz skojarzyć logowanie za pomocą konta witryny sieci Web. Wprowadzany do Twojej nazwy lub adresu e-mail **E-mail** pola formularza.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Wybierz **skojarzyć** przycisku. 

    Zwraca przeglądarki do strony głównej i użytkownik jest zalogowany.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Dodawanie mapy, przy użyciu Pomocnika mapy

Strony sieci Web 2 zawiera dodatki do bibliotekę pomocników platformy ASP.NET w sieci Web, czyli pakietów dodatków dla lokacji stron sieci Web. Jednym z nich jest składnik mapowania dostarczony przez `Microsoft.Web.Helpers.Maps` klasy. Można użyć `Maps` klasy do wygenerowania na podstawie adresu lub zestaw długości i szerokości geograficznej współrzędnych mapy. `Maps` Klasy pozwala wywoływać bezpośrednio w tym Bing, MapQuest, Google i Yahoo aparaty popularnych mapy.

Aby korzystać z nowych `Maps` klasy w witrynie sieci Web, należy najpierw zainstalować wersję 2 biblioteki pomocników sieci Web. Aby to zrobić, przejdź do instrukcje dotyczące instalowania obecnie wydanej wersji systemu [bibliotekę pomocników platformy ASP.NET Web](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) i zainstaluj w wersji 2.

Procedura dodawania mapowania do strony są takie same niezależnie od tego, który aparatów mapy wywołania. Po prostu Dodaj odwołanie pliku JavaScript do strony mapowania, a następnie dodaj wywołanie, który renderuje `<script>` znaczników na stronie. Następnie na stronie mapowania wywołać aparat mapy, którego chcesz użyć.

Poniższy przykład przedstawia sposób tworzenia strony, który renderuje mapy na podstawie adresu i innej strony, który renderuje na podstawie długości i szerokości geograficznej współrzędnych mapy. Przykładowe mapowanie adresu wykorzystuje map programu Google, a w przykładzie współrzędnych mapowania używa usługi mapy Bing. Należy uwzględnić następujące elementy w kodzie:

- Wywołanie `Assets.AddScript` w górnej części dwie strony mapowania. Ta metoda dodaje odwołanie do *jquery 1.6.2.min.js* pliku, który jest dołączony **witryny początkowej** szablonu i co jest wymagane przez `Maps` klasy.
- Wywołanie `Assets.GetScripts` metody w pliku układu. Ta metoda renderuje `<script>` tagu dwie strony mapowania.
- Wywołanie `@Maps.GetGoogleHtml` i `@Maps.GetBingHtml` metody na stronach mapowania. Aby mapować adres, należy podać ciąg adresu. Aby mapować współrzędne, należy podać długość i szerokość geograficzną współrzędnych. Aparat mapy Bing, należy także podać klucz (który uzyskać bezpłatnie po zarejestrowaniu się w [lokacji deweloperów map Bing](https://www.microsoft.com/maps/developers/web.aspx)). Metody silników mapy działa w podobny sposób (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Aby utworzyć mapowanie stron:

1. Tworzenie witryny sieci Web na podstawie **witryny początkowej** szablonu.
2. Utwórz plik o nazwie *MapAddress.cshtml* w katalogu głównym witryny. Ta strona wygeneruje mapy na podstawie adresu, który przekazywania do niej.
3. Skopiuj następujący kod do pliku, zastępując istniejącej zawartości. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Utwórz plik o nazwie  *\_MapLayout.cshtml* w katalogu głównym witryny. Ta strona będzie strony układu dla dwóch stronach mapowania.
5. Skopiuj następujący kod do pliku, zastępując istniejącej zawartości. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Utwórz plik o nazwie *MapCoordinates.cshtml* w katalogu głównym witryny. Ta strona wygeneruje mapy na podstawie zestawu współrzędne, które przekazujesz do niego.
7. Skopiuj następujący kod do pliku, zastępując istniejącej zawartości. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Aby przetestować stronach mapowania:

1. Uruchom strony *MapAddress.cshtml* pliku.
2. Wprowadź ciąg pełny adres, w tym ulicę, stanu lub województwo i kod pocztowy, a następnie wybierz pozycję **mapy go** przycisku. Na stronie elementy mapy z map programu Google: 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Znajdź zestaw współrzędne geograficzne dla określonej lokalizacji.
4. Uruchom strony *MapCoordinates.cshtml*. Wprowadź współrzędne, a następnie wybierz pozycję **mapy go** przycisku. Na stronie elementy mapy z usługą mapy Bing: 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Uruchamianie aplikacji stron sieci Web obok siebie

Strony sieci Web 2 dodaje możliwość uruchamiania aplikacji obok siebie. Dzięki temu można nadal uruchamiać aplikacje 1 stron sieci Web, tworzyć nowe aplikacje sieci Web Pages 2 i uruchomić wszystkie z nich na tym samym komputerze.

Poniżej przedstawiono niektóre czynności podczas instalacji wersji Beta 2 stron sieci Web za pomocą programu WebMatrix:

- Domyślnie istniejących aplikacji stron sieci Web zostanie uruchomiony jako aplikacji w wersji 2 na tym komputerze. (Zestawy w wersji 2 są zainstalowane w pamięci podręcznej GAC i będzie używane automatycznie).
- Jeśli chcesz uruchomić lokacji za pomocą stron sieci Web w wersji 1 (zamiast domyślnej, jak poprzedni punkt), można skonfigurować witryny, w tym celu. Jeśli witryna nie ma jeszcze *web.config* plików w katalogu głównym witryny, Utwórz nową i skopiuj następujący kod XML, zastępowanie istniejącej zawartości. Jeśli witryna zawiera już *web.config* plików, dodawanie `<appSettings>` element podobny do następującego do `<configuration>` sekcji.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
"— Jeśli nie określisz wersji w *web.config* pliku lokacji jest wdrażana jako lokację w wersji 2. (Zestawy w wersji 2 są kopiowane do *bin* folder w witrynie wdrożonej.)
- Nowe aplikacje utworzyć przy użyciu szablonów witryny w macierzy sieci Web w wersji 2 Beta zawierają zestawy w wersji 2 stron sieci Web w tej witrynie *bin* folderu.

Ogólnie rzecz biorąc, zawsze można kontrolować wersję stron sieci Web do użycia z lokacji za pomocą NuGet odpowiednich zestawów do witryny *bin* folderu. Pakietów można znaleźć [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Renderowanie stron dla urządzeń przenośnych

Strony sieci Web 2 umożliwia tworzenie niestandardowych służy do renderowania zawartości na telefon komórkowy lub innych urządzeń.

`System.Web.WebPages` Przestrzeń nazw zawiera następujące klasy, które pozwalają pracować z trybów wyświetlania: `DefaultDisplayMode`, `DisplayInfo`, i `DisplayModes`. Można użyć tych klas bezpośrednio i pisania kodu, który renderuje prawo produktu wyjściowego dla określonych urządzeń.

Alternatywnie można utworzyć strony specyficzne dla urządzenia przy użyciu wzorca nazewnictwa plików następująco: *FileName.* *Mobile**.cshtml*. Na przykład można utworzyć dwie wersje strony, jedną o nazwie *MyFile.cshtml* i jedną o nazwie *MyFile.Mobile.cshtml*. W czasie, gdy urządzenie przenośne żąda wykonywania *MyFile.cshtml*, stron sieci Web renderuje zawartość z *MyFile.Mobile.cshtml*. W przeciwnym razie *MyFile.cshtml* jest renderowany.

Poniższy przykład pokazuje, jak umożliwiające renderowanie przenośnych przez dodanie strony zawartości dla urządzeń przenośnych. *Page1.cshtml* zawiera zawartości oraz pasek boczny nawigacji. *Page1.Mobile.cshtml* zawiera tę samą zawartość, ale pominięto paska bocznego.

Aby skompilować i uruchomić przykładowy kod:

1. W lokacji stron sieci Web, Utwórz plik o nazwie *Page1.cshtml* i skopiuj *Page1.cshtml* zawartości do niego w przykładzie.
2. Utwórz plik o nazwie *Page1.Mobile.cshtml* i skopiuj *Page1.Mobile.cshtml* zawartości do niego w przykładzie. Zwróć uwagę, że wersja urządzenia przenośne strony pomija sekcję nawigacji dla lepszej renderowania na ekranie mniejsze.
3. Uruchom przeglądarkę dla komputerów, a następnie przejdź do *Page1.cshtml*.
4. Uruchom przeglądarkę dla telefonów (lub w emulatorze urządzenia przenośnego) i przejdź do *Page1.cshtml*. Należy zauważyć, że wersja urządzenia przenośne strony renderuje teraz stron sieci Web. 

    > [!NOTE]
    > Aby przetestować stron dla urządzeń przenośnych, można użyć symulator urządzeń przenośnych, uruchomioną na komputerze stacjonarnym. To narzędzie umożliwia testowanie stron sieci web, jak wyglądają na urządzeniach przenośnych (oznacza to zwykle z dużo mniejszym wyświetlenie obszaru). Przykład symulatora [przełącznik agenta użytkownika dodatku](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) programie Mozilla Firefox, co umożliwia emulować różnych przeglądarkach dla urządzeń przenośnych z wersji pulpitu Firefox.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* renderowane w przeglądarce pulpitu:

[![topseven displaymodes 1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* wyświetlane w widoku symulatora telefonu iPhone Apple przeglądarki Firefox. Nawet jeśli żądanie dotyczy *Page1.cshtml*, renderuje aplikacji *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

### <a name="aspnet-web-pages-1-resources"></a>Zasoby 1 stron sieci Web ASP.NET

> [!NOTE]
> Większość programowania 1 stron sieci Web i interfejsu API zasobów nadal mają zastosowanie do stron sieci Web 2.

- [Wprowadzenie do programowania stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>Zasoby programu WebMatrix

- [Program WebMatrix 2 nowości](http://webmatrix.com/next)
- [Microsoft WebMatrix lokacji](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Uruchamianie aplikacji sieci Web Microsoft webmatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(w tym pełnej długości przykładowej aplikacji stron sieci Web)
