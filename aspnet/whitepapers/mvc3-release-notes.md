---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: ''
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: fe3536f3a40f3a74df47bcc1ca0d0b8416895169
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381681"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [Omówienie](#overview)
- [Uwagi dotyczące instalacji](#installation-notes)
- [Wymagania dotyczące oprogramowania](#software-requirements)
- [Dokumentacja](#documentation)
- [Obsługa](#support)
- [Uaktualnianie projektu ASP.NET MVC 2 do wzorca ASP.NET MVC 3 Tools Update](#upgrading)
- [ASP.NET MVC 3 Tools Update (April 12, 2011)](#tu-changes)

    - [Okno dialogowe "Dodaj kontroler" można teraz tworzenia szkieletu kontrolerów z kodem dostępu do widoków i danych](#tu-AddControllerDialog)
    - [Ulepszenia "platformy ASP.NET MVC 3 nowego projektu" okno dialogowe](#tu-ImprovementsNewDialogBox)
    - [Szablony projektów obejmują teraz Modernizr 1.7](#tu-Modernizr)
    - [Szablony projektu obejmują zaktualizowane wersje jQuery, interfejs użytkownika jQuery i jQuery sprawdzania poprawności](#tu-UpdatedJQuery)
    - [Szablony projektów obejmują teraz ADO.NET Entity Framework 4.1 jako wstępnie zainstalowany pakiet NuGet](#tu-EF)
    - [Szablony projektów zawierają biblioteki JavaScript jako wstępnie zainstalowanych pakietów NuGet](#tu-JavaScriptLibsNuget)
    - [Znane problemy](#tu-KI)
- [ASP.NET MVC 3 RTM (January 13, 2011)](#MVC3RTM)

    - [Zmiana: Zaktualizowanej wersji interfejs użytkownika jQuery, aby 1.8.7](#RTM-1)
    - [Zmiana: Zmienić domyślną ModelMetadataProvider z powrotem do DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Naprawiono: Wklejanie część wyrażenia Razor, zawierającego wyniki białe znaki w nim zostały zamienione](#RTM-3)
    - [Poprawiono: Zmiana nazwy pliku Razor, który jest otwierany w edytorze powoduje wyłączenie kolorowania składni i technologii IntelliSense](#RTM-4)
    - [Znane problemy](#RTM-KI)
    - [Fundamentalne zmiany](#RTM-BC)
- [Program ASP.NET MVC 3 w wersji Release Candidate 2 (10 grudnia 2010)](#_Toc2)

    - [Zmienić szablony projektu do uwzględnienia jQuery 1.4.4, jQuery 1.7 sprawdzania poprawności i interfejs użytkownika 1.8.6y 1.8.6 interfejsu użytkownika jQuery](#_Toc2_1)
    - [Klasa dodano "AdditionalMetadataAttribute"](#_Toc2_2)
    - [Tworzenie szkieletu ulepszone widoku](#_Toc2_3)
    - [Dodano Html.Raw — metoda](#_Toc2_3)
    - [Właściwości zmieniono nazwę "Controller.ViewModel" i "View" do "Obiekt ViewBag."](#_Toc2_4)
    - [Zmieniono nazwę "ControllerSessionStateAttribute" klasy "SessionStateAttribute"](#_Toc2_5)
    - [Zmieniono nazwę RemoteAttribute "Fields" właściwości "AdditionalFields"](#_Toc2_6)
    - [Zmieniono nazwę "SkipRequestValidationAttribute" do "AllowHtmlAttribute"](#_Toc2_7)
    - [Metoda zmienione "Html.ValidationMessage" w celu wyświetlenia pierwszy komunikat o błędzie przydatne](#_Toc2_8)
    - [Naprawiono @model deklaracji, nie należy dodawać spacji do dokumentu](#_Toc2_9)
    - [Dodano "FileExtensions" właściwość aparatów widoków do obsługi nazw plików specyficzne dla aparatu](#_Toc2_10)
    - [Naprawiono "LabelFor" element pomocniczy służący do emisji poprawną wartość dla atrybutu "For"](#_Toc2_11)
    - [Naprawiono "RenderAction" metodę, aby nadać priorytet jawne wartości podczas wiązania modelu](#_Toc2_12)
    - [Fundamentalne zmiany](#_Toc2_BC)
    - [Znane problemy](#_Toc2_KI)
- [Program ASP.NET MVC 3 w wersji Release Candidate (9 listopada 2010 r.)](#TOC_ASP_NET_3_RC)

    - [Nowe funkcje w wersji RC platformy ASP.NET MVC 3](#_Toc276711785)
    - [NuGet Package Manager](#_Toc276711786)
    - [Ulepszone "Nowego projektu" okno dialogowe](#_Toc276711787)
    - [Bezsesyjne kontrolerów](#_Toc276711788)
    - [Nowe atrybuty weryfikacji](#_Toc276711789)
    - [Nowe przeciążenia dla metod "LabelForModel" i "LabelFor"](#_Toc276711790)
    - [Akcja podrzędna buforowania danych wyjściowych](#_Toc276711791)
    - ["Dodaj widok" ulepszenia okno dialogowe](#_Toc276711792)
    - [Weryfikacja żądania szczegółowe](#_Toc276711793)
    - [Fundamentalne zmiany](#_Toc276711794)
    - [Znane problemy](#_Toc276711795)
- [ASP. Informacje o wersji Beta MVC 3 (6 października 2010)](#TOC_ASP_NET_3_Beta)

    - [Nowe funkcje w wersji Beta programu ASP.NET MVC 3](#0.1__Toc274034215)
    - [NuPack Package Manager](#0.1__Toc274034216)
    - [Ulepszone okno dialogowe Nowy projekt](#0.1__Toc274034217)
    - [Uproszczony sposób na określenie silnie wpisany modeli w widokami Razor](#0.1__Toc274034218)
    - [Obsługa nowych stron ASP.NET Web Pages metody pomocnika](#0.1__Toc274034219)
    - [Obsługa iniekcji zależności dodatkowe](#0.1__Toc274034220)
    - [Nowa funkcja obsługi dyskretnego kodu Ajax opartych na technologii jQuery](#0.1__Toc274034221)
    - [Nowa funkcja obsługi dyskretnego kodu jQuery sprawdzania poprawności](#0.1__Toc274034222)
    - [Nowe flagi całej aplikacji do sprawdzania poprawności klienckich i dyskretny kod JavaScript](#0.1__Toc274034223)
    - [Nowa funkcja obsługi kodu, który jest uruchamiany przed uruchomieniem widoków](#0.1__Toc274034224)
    - [Nowa funkcja obsługi VBHTML składni Razor](#0.1__Toc274034225)
    - [Bardziej precyzyjną kontrolę nad atrybut ValidateInputAttribute](#0.1__Toc274034226)
    - [Pomocnicy przekonwertować podkreślenia łączniki dla atrybutu HTML nazw określony za pomocą anonimowego obiektów](#0.1__Toc274034227)
    - [Poprawki błędów](#0.1__Toc274034228)
    - [Fundamentalne zmiany](#0.1__Toc274034229)
    - [Znane problemy](#0.1__Toc274034230)
- [Zrzeczenie odpowiedzialności](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Omówienie

Ten dokument zawiera opis wersji RTM programu ASP.NET MVC 3 dla programu Visual Studio 2010. ASP.NET MVC to platforma do tworzenia aplikacji sieci Web, która używa wzorca Model-View-Controller (MVC). Instalator programu ASP.NET MVC 3 obejmuje następujące składniki:

- Składniki środowiska uruchomieniowego platformy ASP.NET MVC 3
- Narzędzia programu ASP.NET MVC 3 programu Visual Studio 2010
- Składniki czasu wykonywania stron sieci Web platformy ASP.NET
- Narzędzia Visual Studio 2010 stron sieci Web platformy ASP.NET
- Menedżer pakietów firmy Microsoft dla platformy .NET (NuGet)
- Aktualizacja dla programu Visual Studio 2010, który umożliwia obsługę składni Razor. (Aby uzyskać więcej informacji, zobacz artykuł bazy wiedzy 2483190).

Pełny zestaw informacje o wersji dla każdej wersji wstępnej programu ASP.NET MVC 3, można znaleźć w witrynie internetowej platformy ASP.NET pod adresem URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

Aby zainstalować program ASP.NET MVC 3 RTM za pomocą Instalatora platformy sieci Web (Web PI), odwiedź następującą stronę:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Alternatywnie możesz pobrać Instalator RTM programu ASP.NET MVC 3 dla programu Visual Studio 2010 z następującej strony:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 można zainstalować i uruchomić side-by-side przy użyciu wzorca ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Wymagania programowe

ASP.NET MVC 3 składniki czasu wykonywania wymaga następującego oprogramowania:

- .NET framework w wersji 4. 

    ASP.NET MVC 3 programu Visual Studio 2010 tools wymagają następującego oprogramowania:
- Visual Studio 2010 ani Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

Dokumentacja platformy ASP.NET MVC jest dostępna w witrynie sieci Web MSDN pod adresem URL:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

W samouczkach i innych informacji na temat platformy ASP.NET MVC są dostępne na stronie MVC witryny sieci Web ASP.NET pod adresem URL:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Obsługa

To jest w pełni obsługiwana wersja. Informacje dotyczące uzyskiwania pomocy technicznej znajduje się w temacie [witryny sieci Web Microsoft Support](https://support.microsoft.com/).

Możesz także publikowanie pytań dotyczących tej wersji na forum platformy ASP.NET MVC, w których często w stanie zapewnić obsługę nieformalne członków społeczności platformy ASP.NET:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Uaktualnianie projektu ASP.NET MVC 2 do wzorca ASP.NET MVC 3 Tools Update

ASP.NET MVC 3 można zainstalować równolegle z programem ASP.NET MVC 2 na tym samym komputerze, co daje elastyczności w wyborze kiedy aktualizować aplikację ASP.NET MVC 2 do ASP.NET MVC 3.

Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 2 do wersji 3, wykonaj następujące czynności:

1. Utwórz nowy pusty projekt ASP.NET MVC 3 na komputerze. Projekt ten będzie zawierał niektóre pliki, które są wymagane do uaktualnienia.
2. Skopiuj następujące pliki z projektu ASP.NET MVC 3 do odpowiedniej lokalizacji projektu ASP.NET MVC 2. Należy zaktualizować wszystkie odwołania do biblioteki jQuery w celu uwzględnienia nowej nazwy pliku (jQuery 1.5.1.js): 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*js
    - /Zawartości/motywy/\*.\*
3. Kopiuj *pakietów* folder w katalogu głównym puste rozwiązanie projektu ASP.NET MVC 3 do głównego folderu do rozwiązania, w którym znajduje się w katalogu, w którym znajduje się w rozwiązaniu pliku .sln.
4. Jeśli projekt programu ASP.NET MVC 2 zawiera obszary, należy skopiować plik /Views/Web.config *widoków* folderu każdego obszaru.
5. W obu plikach Web.config w projekcie ASP.NET MVC 2 globalnie wyszukiwania i zamieniania wersji platformy ASP.NET MVC. Znajdź następujące czynności: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Zastąp go następujące czynności:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. W Eksploratorze rozwiązań, usunięcie odwołania do *System.Web.Mvc* (które punkty do biblioteki DLL w wersji 2), a następnie dodaj odwołanie do *System.Web.Mvc* (v3.0.0.0).
7. Dodaj odwołanie do System.Web.WebPages.dll i System.Web.Helpers.dll. Te zestawy znajdują się w następujących folderów: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę projektu i wybierz pozycję Edytuj *ProjectName*csproj.
9. Znajdź *ProjectTypeGuids* i Zastąp ciąg {F85E285D-A4E0-4152-9332-AB1D724D3325} z {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Zapisz zmiany, kliknij prawym przyciskiem myszy projekt i następnie wybierz pozycję Załaduj ponownie projekt.
11. W pliku Web.config katalogu głównego aplikacji, Dodaj następujące ustawienia, aby *zestawy* sekcji. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Jeśli projekt odwołuje się do żadnych bibliotek innych firm, które są kompilowane przy użyciu wzorca ASP.NET MVC 2, Dodaj następujący wyróżniony *bindingRedirect* elementu do pliku Web.config w katalogu głównym aplikacji, w obszarze  *Konfiguracja* sekcji: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Zmiany we wzorcu ASP.NET MVC 3 Tools Update

W tej sekcji opisano zmiany wprowadzone w wersji platformy ASP.NET MVC 3 Tools Update od czasu wersji RTM programu ASP.NET MVC 3.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Okno dialogowe "Dodaj kontroler" można teraz tworzenia szkieletu kontrolerów z kodem dostępu do widoków i danych

Tworzenie szkieletu to sposób szybkiego generowania kontrolera i widoki dla aplikacji. Po wygenerowaniu kodu można edytować go do wymagań projektu.

Aby uruchomić *Dodaj kontroler* okno dialogowe na platformie ASP.NET MVC 3, kliknij prawym przyciskiem myszy *kontrolerów* folderu w *Eksploratora rozwiązań*, kliknij przycisk *Dodaj*, a następnie kliknij przycisk *kontrolera*. Okno dialogowe zostało ulepszone, aby szkieletu dodatkowych opcji.

![](mvc3-release-notes/_static/image1.png)

Domyślnie dostępne są trzy szablony do tworzenia szkieletów.

#### <a name="empty-controller"></a>Pusty kontroler

Ten szablon generuje plik pusty kontroler. Ten szablon jest odpowiednikiem nie sprawdza *dodać akcje w przypadku tworzenia, edytowania, szczegółowe informacje, Usuń scenariusze* w poprzednich wersjach programu ASP.NET MVC. Jeśli wybierzesz, nie dalsze opcje są dostępne.

#### <a name="controller-with-empty-readwrite-actions"></a>Kontroler z akcjami odczytu/zapisu pusty

Ten szablon generuje plik kontrolera, który ma wszystkie metody niezbędne czynności, ale żaden kod implementacji w metodach. Ten szablon jest odpowiednikiem sprawdzanie *dodać akcje w przypadku tworzenia, edytowania, szczegółowe informacje, Usuń scenariusze* w poprzednich wersjach programu ASP.NET MVC. Jeśli wybierzesz, nie dalsze opcje są dostępne.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Kontroler z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework

Ten szablon umożliwia szybkie tworzenie interfejsu użytkownika wprowadzania danych pracy. Generuje kod, który obsługuje szereg typowych wymagań i scenariusze, takie jak następujące:

- *Dostęp do danych*. Wygenerowany kod odczytuje i zapisuje jednostek w bazie danych. Współdziała ona z podejściem Entity Framework Code First wybranie istniejącej klasy kontekstu danych lub jeśli wybierzesz szablon wygenerować nowy *DbContext* klasy. Współdziała również z podejścia First platformy Entity Framework bazy danych lub pierwszy Model Jeśli wybierzesz istniejącą *ObjectContext* klasy.
- *Sprawdzanie poprawności*. Wygenerowany kod używa wiązania modelu programu ASP.NET MVC i metadanych funkcji tak, aby formularz zgłoszenia są weryfikowane zgodnie z regułami zadeklarowanych w klasie modelu. Obejmuje to wbudowane reguły sprawdzania poprawności, takie jak *wymagane* i *StringLength* atrybuty i niestandardowe reguły sprawdzania poprawności.
- *Relacje jeden do wielu*. Jeśli zdefiniujesz relacje klucza obcego jeden do wielu między klasach modeli wygenerowanego kodu powoduje wygenerowanie list rozwijanych służąca do wybierania powiązanych jednostek. Na przykład można zdefiniować następujące klasy modelu Entity Framework Code First konwencjami: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Kiedy należy następnie tworzenia szkieletu kontrolera dla *produktu* klasy, widokach pozwoli użytkownikom na wybór *kategorii* obiekt dla każdego *produktu* wystąpienia.

    Ten szablon umożliwia dodatkowe opcje w *Dodaj kontroler* okno dialogowe. Aby uzyskać *klasa modelu*, możesz wybrać dowolną klasę modelu w Twoim rozwiązaniu, który określa typ danych, które użytkownicy będą mogli tworzyć lub edytować:
- Jeśli chcesz używać programu Entity Framework Code First, można wybrać dowolną klasę modelu.
- Jeśli używasz First platformy Entity Framework bazy danych lub First platformy Entity Framework modelu, pamiętaj wybrać klasę jednostki, zdefiniowane w model koncepcyjny.

Aby uzyskać *klasy kontekstu danych*, można wybrać następujące opcje:

- Jeśli chcesz użyć Code First i istniejącego kontekstu danych klasy, wybierz pozycję ** nowy kontekst danych **. Klasa kontekstu danych następnie zostanie wygenerowany dla Ciebie.
- Jeśli chcesz użyć Code First i istniejącej klasy kontekstu danych, wybierz go tutaj. Zostanie zaktualizowana do utrwalenia klasy modelu, który wybrano.
- Jeśli używasz pierwszej bazy danych lub pierwszy Model Wybierz klasy kontekstu obiektów.

W widokach wybierz aparat widoku, którego chcesz użyć, lub wybierz wyrażenie None, jeśli nie chcesz utworzyć szkielet żadnych widoków.

Możesz wybrać zaawansowane Optionsto określić dodatkowe opcje wygenerowanych widoków. Można na przykład wybierz układ lub strony wzorcowej do użycia.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Ulepszenia "platformy ASP.NET MVC 3 nowego projektu" okno dialogowe

Okno dialogowe, którego używasz do tworzenia nowych projektów platformy ASP.NET MVC 3 zawiera wiele ulepszeń wymienione poniżej.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Nowy szablon "Projekt Intranet"

Lista szablonów projektu obejmuje nowy szablon aplikacji intranetowych. Ten szablon zawiera ustawienia umożliwiające tworzenie aplikacji sieci web przy użyciu uwierzytelniania Windows zamiast uwierzytelniania formularzy. Aplikacji intranetowych wymagają niektórych ustawień usług IIS, które nie są umieszczane w szablonie projektu, dlatego szablon zawiera plik readme z instrukcjami, jak utworzyć szablon projektu, pracy w usługach IIS. Dokumentacja dla nowego szablonu aplikacji sieci Intranet jest dostępny w witrynie MSDN pod adresem URL:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Szablony projektów są teraz włączone HTML5

Okno dialogowe Nowy projekt zawiera teraz opcję, aby dodać funkcje specyficzne dla języka HTML5 do szablonów projektu. Wybranie opcji powoduje, że widoki zostanie wygenerowany, które zawierają nowe HTML5 `<header>`, `<footer>`, i `<navigation>` elementów.

Należy pamiętać, starsze wersje przeglądarek nie obsługują tagów specyficznych dla języka HTML5. Aby spełnić tego ograniczenia, szablony projektów HTML5 dołączyć odwołanie do biblioteki Modernizr. (Zobacz następną sekcję).

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Szablony projektów obejmują teraz Modernizr 1.7

Modernizr jest biblioteki JavaScript, która umożliwia obsługę CSS 3 i HTML5 w przeglądarkach, które jeszcze nie obsługują tych funkcji. Ta biblioteka jest uwzględniane jako pakiet NuGet wstępnie zainstalowanych szablonów dla projektów ASP.NET MVC 3. Aby uzyskać więcej informacji na temat Modernizr zobacz [ http://www.modernizr.com/ ](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Szablony projektu obejmują zaktualizowane wersje jQuery, interfejs użytkownika jQuery i jQuery sprawdzania poprawności

Szablony projektów zawierają teraz następujące wersje skrypty jQuery:

- jQuery 1.5.1
- jQuery 1.8 sprawdzania poprawności
- jQuery UI 1.8.11

Biblioteki te są dołączone jako wstępnie zainstalowanych pakietów NuGet.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Szablony projektów obejmują teraz ADO.NET Entity Framework 4.1 jako wstępnie zainstalowany pakiet NuGet

ADO.NET Entity Framework 4.1 obejmuje funkcję Code First. Kod jest najpierw nowy wzorzec projektowania dla ADO.NET Entity Framework, który stanowi alternatywę dla istniejących wzorców pierwszej bazy danych i modelu pierwszy.

Kod najpierw koncentruje się wokół definiowania modelu przy użyciu klas POCO ("zwykłych starych obiektów CLR") w języku Visual Basic lub C#. Te klasy można mapować do istniejącej bazy danych lub służyć do generowania schemat bazy danych. Dodatkowe czynności konfiguracyjne, mogą być dostarczane za pomocą *DataAnnotations* atrybutów lub przy użyciu interfejsów API fluent.

Dokumentacja korzystania z kodu Firstwith platformy ASP.NET MVC jest dostępna w witrynie internetowej platformy ASP.NET na następujące adresy URL:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Szablony projektów zawierają biblioteki JavaScript jako wstępnie zainstalowanych pakietów NuGet

Podczas tworzenia nowego projektu ASP.NET MVC 3, projekt zawiera już wspomniano pliki JavaScript (na przykład biblioteki Modernizr) przez zainstalowanie ich za pomocą narzędzia NuGet zamiast bezpośrednio dodawania skryptów do folderu skryptów w szablonie projektu zawartość. Dzięki temu można użyć NuGet, aby zaktualizować skrypty do najnowszej wersji, gdy wydawane są nowe wersje skryptów.

Na przykład biorąc pod uwagę częstotliwość nowe wersje jQuery, wersja jQuery zawarte w szablonie projektu w pewnym momencie będzie nieaktualna. Jednak ponieważ jQuery jest dołączony jako zainstalowanego pakietu NuGet, otrzymasz powiadomienie w oknie dialogowym NuGet, gdy są dostępne nowsze wersje jQuery.

Ponieważ jQuery zawiera numer wersji w nazwie pliku, aktualizowanie jQuery do najnowszej wersji wymaga również aktualizowanie `<script>` tag, który odwołuje się do pliku jQuery na użycie nowej nazwy pliku. Inne biblioteki uwzględniony skrypt nie zawierają numer wersji w polu Nazwa skryptu, dlatego mogą być łatwiejsze aktualizowane do ich najnowszych wersji.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Znane problemy

- W niektórych przypadkach instalacja może zakończyć się błąd komunikat "Instalacja nie powiodła się z kodem błędu (0x80070643)". Aby uzyskać informacje dotyczące sposobu obejścia tego problemu, zobacz [artykuł bazy wiedzy 2531566](https://support.microsoft.com/kb/2531566).
- Tworzenie szkieletu dodawania kontrolera nie tworzenia szkieletu jednostek, które wykorzystują Obsługa dziedziczenia jednostek w ramach programu Entity Framework. Na przykład, biorąc pod uwagę podstawowej *osoby* klasę, która jest dziedziczona przez *uczniów* klasy tworzenia szkieletów *uczniów* klasy spowoduje wygenerowany kod, który nie można skompilować.
- Tworzenie nowego projektu ASP.NET MVC 3, wewnątrz folderu rozwiązania powoduje, że *obiektu NullReferenceException* błędu. Obejście polega na tworzenie projektu ASP.NET MVC 3 w katalogu głównym rozwiązania, a następnie przenieś go do folderu rozwiązania.
- Funkcja IntelliSense dla składni Razor nie działa po zainstalowaniu ReSharper. Jeśli masz zainstalowanym rozszerzeniem ReSharper i wykorzystać obsługę funkcji IntelliSense Razor programu ASP.NET MVC 3, zobacz wpis [Razor Intellisense i ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri omówiono sposoby używania ich ze sobą już dziś.
- Podczas instalacji okno dialogowe akceptacji umowy licencyjnej wyświetla postanowienia licencyjne w oknie, która jest mniejsza niż planowano.
- Podczas edytowania widoku Razor (cshtml lub. *vbhtml* pliku), widoki. ASP.NET MVC 3 nie zawiera żadnych fragmentów dla widokami Razor... aspxselecting fragment kodu dla platformy ASP.NET MVC zostanie wyświetlone fragmenty kodu dla
- Jeśli Zainstaluj ASP.NET MVC 3 dla programu Visual Web Developer Express na komputerze, na których nie zainstalowano programu Visual Studio, a następnie zainstalować program Visual Studio, należy ponownie zainstalować program ASP.NET MVC 3. Program Visual Studio i Visual Web Developer Express udostępniać składniki, które są uaktualniane przez Instalatora programu ASP.NET MVC 3. Ten sam problem w przypadku zainstalowania programu ASP.NET MVC 3 dla programu Visual Studio na komputerze, na które nie mają Visual Web Developer Express, a następnie później zainstalować Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Zmiany w wersji RTM programu ASP.NET MVC 3

W tej sekcji opisano zmiany i poprawki błędów wprowadzonych w wersji RTM programu ASP.NET MVC 3 od wersji RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Zmiana: Zaktualizowanej wersji interfejs użytkownika jQuery, aby 1.8.7

Szablony projektów programu ASP.NET MVC dla programu Visual Studio zostały zaktualizowane, aby uwzględnić najnowszą wersję biblioteki interfejsu użytkownika jQuery. Szablony zawierają również minimalny zestaw plików zasobów wymaganych przez interfejs użytkownika, takie jak skojarzone CSS i pliki obrazów jQuery.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Zmiana: Zmienić domyślną ModelMetadataProvider z powrotem do DataAnnotationsModelMetadataProvider

W wersji RC2 ASP.NET MVC 3 wprowadzono *CachedDataAnnotationsMetadataProvider* podanym buforowania na podstawie istniejącej klasy *DataAnnotationsModelMetadataProvider* klasy poprawa wydajności. Jednak pewne błędy zostały zgłoszone za pomocą tej implementacji, aby zmiana została przywrócona i przenoszony do projektu MVC prognoz, który znajduje się w temacie [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Naprawiono: Wklejanie część wyrażenia Razor, zawierającego wyniki białe znaki w nim zostały zamienione

W wersji wstępnych programu ASP.NET MVC 3 podczas wklejania części wyrażenia Razor, który zawiera białe znaki w pliku Razor, wyrażenie wynikowe została odwrócona. Na przykład rozważmy następujący blok kodu Razor:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Jeśli wybierzesz tekst "param pierwszy" w przypadku pierwszej metody i wklej go jako argument do drugiej metody, wynik jest następujący:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Poprawne zachowanie jest czy operacji wklejania powinna być rozwiązywana WE następujące czynności:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Ten problem został rozwiązany w wersji RTM, tak aby wyrażenie poprawnie są zachowywane w trakcie operacji wklejania.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Poprawiono: Zmiana nazwy pliku Razor, który jest otwierany w edytorze powoduje wyłączenie kolorowania składni i technologii IntelliSense

Zmiana nazwy pliku Razor przy użyciu Eksploratora rozwiązań, gdy plik jest otwarty w oknie edytora powoduje wyróżnianie składni czy funkcja IntelliSense, przestaną działać dla tego pliku. Ten problem został rozwiązany, tak aby wyróżniania i technologii IntelliSense są zachowywane po zmianie nazwy.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Znane problemy

- Jeśli zamkniesz programu Visual Studio 2010 z dodatkiem SP1 Beta, gdy konsola Menedżera pakietów NuGet jest otwarty, Visual Studio ulega awarii i podejmie próbę ponownego uruchomienia. Ten problem zostanie rozwiązany w wersji RTM programu Visual Studio 2010 SP1.
- Instalator programu ASP.NET MVC 3 jest tylko można instalować wstępną wersję Menedżera pakietów NuGet. Po zainstalowaniu pierwszej wersji NuGet można zainstalować i zaktualizować przy użyciu Menedżera rozszerzeń programu Visual Studio. Jeśli masz jeszcze zainstalowanego Menedżera NuGet, przejdź do galerii Visual Studio rozszerzenia do aktualizacji do najnowszej wersji pakietu NuGet.
- Tworzenie nowego projektu ASP.NET MVC 3, wewnątrz folderu rozwiązania powoduje, że *obiektu NullReferenceException* błędu. Obejście polega na tworzenie projektu ASP.NET MVC 3 w katalogu głównym rozwiązania, a następnie przenieś go do folderu rozwiązania.
- Instalator może zająć więcej czasu niż w poprzednich wersjach programu ASP.NET MVC, aby zakończyć. Jest tak, ponieważ powoduje zaktualizowanie składniki programu Visual Studio 2010.
- Funkcja IntelliSense dla składni Razor nie działa po zainstalowaniu ReSharper. Jeśli masz zainstalowanym rozszerzeniem ReSharper i wykorzystać obsługę funkcji IntelliSense Razor programu ASP.NET MVC 3, zobacz wpis [Razor Intellisense i ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri omówiono sposoby używania ich ze sobą już dziś.
- Widoki CCSHTML i VBHTML utworzone przy użyciu wersji Beta programu ASP.NET MVC 3 ma ich akcji kompilacji poprawnie, w wyniku czego wyświetlić te typy zostały pominięte podczas publikowania projektu. Wartość akcji kompilacji dla tych plików powinna być równa "Treści". ASP.NET MVC 3 RTM rozwiązuje ten problem dla nowych plików, ale nie rozwiąże to ustawienie dla istniejących plików projektu utworzonych za pomocą wersji wstępnej.
- ![](mvc3-release-notes/_static/image3.png)
- Podczas instalacji okno dialogowe akceptacji umowy licencyjnej wyświetla postanowienia licencyjne w oknie, która jest mniejsza niż planowano.
- Podczas edytowania widoku Razor (cshtml plik), przejdź do kontrolera elementu menu w programie Visual Studio nie będą dostępne, i nie Brak fragmentów kodu.
- Jeśli Zainstaluj ASP.NET MVC 3 dla programu Visual Web Developer Express na komputerze, na których nie zainstalowano programu Visual Studio, a następnie zainstalować program Visual Studio, należy ponownie zainstalować program ASP.NET MVC 3. Program Visual Studio i Visual Web Developer Express udostępniać składniki, które są uaktualniane przez Instalatora programu ASP.NET MVC 3. Ten sam problem w przypadku zainstalowania programu ASP.NET MVC 3 dla programu Visual Studio na komputerze, na które nie mają Visual Web Developer Express, a następnie później zainstalować Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Fundamentalne zmiany

- W poprzednich wersjach programu ASP.NET MVC działań, które są filtry tworzenie na żądanie z wyjątkiem sytuacji, w niektórych przypadkach. To zachowanie nigdy nie było gwarantowane działanie, ale jedynie szczegółowo opisuje implementacja i kontrakt dla filtrów było należy wziąć pod uwagę ich bezstanowe. W programie ASP.NET MVC 3 filtry są buforowane bardziej agresywne. W związku z tym wszelkie filtry akcji niestandardowej, które nieprawidłowo przechowują stan wystąpienia może nie działać.
- Kolejność wykonywania filtrów wyjątek został zmieniony na filtry wyjątków, które mają taki sam *kolejności* wartość. W programie ASP.NET MVC 2 i starszych wyjątek filtrów na kontrolerze, które mają taką samą *kolejności* wartości, ponieważ osoby korzystające z metody akcji są wykonywane przed filtry wyjątków dla metody akcji. Zazwyczaj można tak, gdy są stosowane filtry wyjątków bez określonej *kolejności* wartość. W programie ASP.NET MVC 3 to zamówienie została wycofana, aby najpierw wykonana bardziej konkretny od pozostałych obsługi wyjątków. Tak jak w starszych wersjach Jeśli *kolejności* właściwość jest jawnie określona, filtry są uruchamiane w określonej kolejności.
- Nową właściwość o nazwie *FileExtensions* została dodana do *VirtualPathProviderViewEngine* klasy bazowej. Gdy ASP.NET wyszukuje widoku przy użyciu ścieżki (a nie według nazwy), są traktowane jako widoki tylko z rozszerzeniem pliku zawarte w określonej przez tę właściwość nowej listy. W przypadku, gdy dostawca niestandardowej kompilacji jest zarejestrowany, aby włączyć rozszerzenie pliku niestandardowe widoki formularzy sieci Web, a w przypadku, gdy dostawca odwołuje się do tych widoków przy użyciu pełnej ścieżki, a nie nazwę jest istotną zmianę w aplikacjach. Obejście polega na zmodyfikowanie wartości *FileExtensions* właściwość, aby dołączyć rozszerzenie niestandardowego pliku.
- Niestandardowe kontrolera fabryki implementacji, które bezpośrednio zaimplementować *IControllerFactory* interfejs musi dostarczyć implementację nowej *GetControllerSessionBehavior* metody dodane do interfejsu w tej wersji. Ogólnie rzecz biorąc, zaleca się, możesz nie bezpośrednio zaimplementować niniejszy interfejs i pochodzi od klasy *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Zmiany w wersji RC2 platformy ASP.NET MVC 3

W tej sekcji opisano zmiany (nowe funkcje i poprawki błędów) w wersji platformy ASP.NET MVC 3 RC2 od czasu wersji RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Zmienić szablony projektu do uwzględnienia jQuery 1.4.4, jQuery 1.7 sprawdzania poprawności i interfejs użytkownika jQuery 1.8.6

Szablony projektów dla platformy ASP.NET MVC 3 obejmują teraz najnowsze wersje jQuery, jQuery, weryfikacji i jQuery interfejsu użytkownika. jQuery interfejsu użytkownika to nowy dodatek do szablonów projektu i zapewnia przydatnych widżetów interfejsu. Aby uzyskać więcej informacji na temat interfejs użytkownika jQuery, odwiedź stronę ich strony głównej: [ http://jqueryui.com/ ](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Klasa dodano "AdditionalMetadataAttribute"

Możesz użyć *AdditionalMetadataAttribute* klasy do wypełniania *ModelMetadata.AdditionalValues* słownik właściwości modelu.

Na przykład załóżmy, że model widoku ma właściwości, które powinny być wyświetlane tylko dla administratora. Modelu mogą być adnotowane przy użyciu nowego atrybutu przy użyciu AdminOnly jako klucz i wartość true, jako wartość, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Te metadane ma zostać udostępnione dowolnego szablonu ekranu lub edytorze, po wyrenderowaniu modelu widoku produktu. Jest do Ciebie jako Deweloper aplikacji do interpretacji informacji o metadanych.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Tworzenie szkieletu ulepszone widoku

Szablony T4, używany do tworzenia szkieletów widoków teraz wygenerowania wywołania metody pomocnika szablonu, taką jak *EditorFor* zamiast pomocników, takich jak *TextBoxFor*. Ta zmiana zwiększa obsługę metadanych modelu w postaci atrybuty adnotacji danych okno dialogowe dodawania widoku generuje widoku.

Dodaj widok szkieletu także ulepszone wykrywanie i użycia informacje o kluczu podstawowym na modelu na podstawie Konwencji. Na przykład okno dialogowe dodawania widoku używa tych informacji, aby upewnić się, że wartość klucza podstawowego nie jest szkieletu jako pole edytowalnego formularza.

Domyślnie edytowanie i tworzenie szablonów zawierają odwołania do skryptów jQuery potrzebnych do przeprowadzenia weryfikacji klienta.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Dodano Html.Raw — metoda

Domyślnie Razor przeglądać aparatu koduje HTML wszystkie wartości. Na przykład poniższy fragment kodu koduje HTML wewnątrz zmiennej powitanie, tak aby był wyświetlany na stronie jako `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Nowy *Html.Raw* metoda zapewnia prosty sposób wyświetlania niekodowany kod HTML, gdy zawartość jest znane jako bezpieczne. Poniższy przykład wyświetla te same parametry, ale ten ciąg jest renderowany jako kod znaczników:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Właściwości zmieniono nazwę "Controller.ViewModel" i "View" do "Obiekt ViewBag."

Wcześniej *ViewModel* właściwość *kontrolera* zgodnych do *widoku* właściwości widoku. Obie te właściwości umożliwiają dostęp do wartości *ViewDataDictionary* przy użyciu składni metody dostępu właściwości dynamicznych. Obie te właściwości została zmieniona na być takie same, aby uniknąć nieporozumień i są bardziej spójne.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Zmieniono nazwę "ControllerSessionStateAttribute" klasy "SessionStateAttribute"

*ControllerSessionStateAttribute* klasa została wprowadzona w wersji RC programu ASP.NET MVC 3. Właściwości została zmieniona na musi być bardziej zwięzły.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Zmieniono nazwę RemoteAttribute "Fields" właściwości "AdditionalFields"

*RemoteAttribute* klasy *pola* właściwości spowodowała wystąpienia pewnych niejasności między użytkownikami. Zmiana nazwy tej właściwości, aby *AdditionalFields* wyjaśnia przeznaczeniem.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Zmieniono nazwę "SkipRequestValidationAttribute" do "AllowHtmlAttribute"

*SkipRequestValidationAttribute* atrybut została zmieniona na *AllowHtmlAttribute* lepiej odpowiada jego przeznaczenia.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Metoda zmienione "Html.ValidationMessage" w celu wyświetlenia pierwszy komunikat o błędzie przydatne

*Html.ValidationMessage* metoda został naprawiony pokazanie pierwszy komunikat o błędzie przydatne, a nie po prostu wyświetlanie pierwszego błędu.

Podczas wiązania modelu *ModelState* słownika można wypełnić z wielu źródeł, z komunikatami o błędach dotyczące właściwości, w tym samym modelu (jeśli implementuje *IValidatableObject* ), z atrybutów sprawdzania poprawności stosowany do właściwości i wyjątki zgłaszane, gdy uzyskano dostęp do właściwości.

Gdy *Html.ValidationMessage* metoda wyświetla komunikat dotyczący sprawdzania poprawności, pomija wpisów stanów modelu, które obejmują: exception, ponieważ te zwykle nie są przeznaczone dla użytkownika końcowego. Zamiast tego metody wyszukuje pierwszy komunikat sprawdzania poprawności, który nie jest skojarzony z powodu wyjątku i wyświetla ten komunikat. Jeśli zostanie znaleziony żaden taki komunikat, domyślnie ogólny komunikat, który jest skojarzony z pierwszy wyjątek.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Naprawiono @model deklaracji, nie należy dodawać spacji do dokumentu

We wcześniejszych wersjach <em>@model</em> deklaracji w górnej części widoku dodać pusty wiersz do wyniku renderowania kodu HTML. Ten problem został rozwiązany, tak, aby deklaracji nie wprowadza białe znaki.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Dodano "FileExtensions" właściwość aparatów widoków do obsługi nazw plików specyficzne dla aparatu

Aparat widoku może zwrócić widok przy użyciu ścieżki widoku jawne, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

Pierwszy aparat widoku zawsze próbuje do renderowania widoku. Domyślnie aparat widoku w formularzach sieci Web jest pierwszym aparat widoku; ponieważ aparat formularzy sieci Web nie można renderować widoku Razor, wystąpi błąd. Aparaty widoku mają teraz *FileExtensions* właściwość, która służy do określania rozszerzeń plików, które obsługują. Ta właściwość jest sprawdzana podczas ASP.NET określa, czy aparat widoku umożliwiający renderowanie pliku. To jest zmianą przerywającą i szczegółowe informacje znajdują się w [fundamentalne zmiany](#_Toc2_BC) części tego dokumentu.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Naprawiono "LabelFor" element pomocniczy służący do emisji poprawną wartość dla atrybutu "For"

Usunięto usterkę gdzie *LabelFor* metody renderowania *dla* atrybut, który pasuje *wejściowych* elementu *nazwa* zamiast tego atrybutu jego identyfikatora. Zgodnie z W3C *dla* atrybut powinien być zgodny *wejściowych* identyfikatora elementu.

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Naprawiono "RenderAction" metodę, aby nadać priorytet jawne wartości podczas wiązania modelu

We wcześniejszych wersjach, jawne wartości, które zostały przekazane do *RenderAction* metody zostały on zignorowany na rzecz bieżących wartości formularza podczas wiązania modelu w akcji podrzędnej. Poprawka gwarantuje, że jawne wartości mają pierwszeństwo podczas wiązania modelu.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Fundamentalne zmiany

- W poprzednich wersjach platformy ASP.NET MVC filtry akcji zostały utworzone na żądanie z wyjątkiem sytuacji, w niektórych przypadkach. To zachowanie nigdy nie było gwarantowane działanie, ale jedynie szczegółowo opisuje implementacja i kontrakt dla filtrów było należy wziąć pod uwagę ich bezstanowe. W programie ASP.NET MVC 3 filtry są buforowane bardziej agresywne. W związku z tym wszelkie filtry akcji niestandardowej, które nieprawidłowo przechowują stan wystąpienia może nie działać.
- Kolejność wykonywania filtrów wyjątek został zmieniony na filtry wyjątków, które mają taki sam *kolejności* wartość. W programie ASP.NET MVC 2 i starszych wyjątek filtrów na kontrolerze, który miał takie same *kolejności* wartości, ponieważ osoby korzystające z metody akcji był wykonywany przed filtry wyjątków dla metody akcji. Zazwyczaj można tak, gdy zostały zastosowane filtry wyjątków bez określonej *kolejności* wartość. W programie ASP.NET MVC 3 to zamówienie została wycofana, aby najpierw wykonana bardziej konkretny od pozostałych obsługi wyjątków. Tak jak w starszych wersjach Jeśli *kolejności* właściwość jest jawnie określona, filtry są uruchamiane w określonej kolejności.
- Nową właściwość o nazwie *FileExtensions* została dodana do *VirtualPathProviderViewEngine* klasy bazowej. Gdy ASP.NET wyszukuje widoku przy użyciu ścieżki (a nie według nazwy), są traktowane jako widoki tylko z rozszerzeniem pliku zawarte w określonej przez tę właściwość nowej listy. W przypadku, gdy dostawca niestandardowej kompilacji jest zarejestrowany, aby włączyć rozszerzenie pliku niestandardowe widoki formularzy sieci Web, a w przypadku, gdy dostawca odwołuje się do tych widoków przy użyciu pełnej ścieżki, a nie nazwę jest istotną zmianę w aplikacjach. Obejście polega na zmodyfikowanie wartości *FileExtensions* właściwość, aby dołączyć rozszerzenie niestandardowego pliku.
- Niestandardowe kontrolera fabryki implementacji, które bezpośrednio zaimplementować <em>IControllerFactory</em> interfejs musi dostarczyć implementację nowej <em>GetControllerSessionBehavior</em>  <em>Metoda, która została dodana do interfejsu w tej wersji</em>. Ogólnie rzecz biorąc, zaleca się, możesz nie bezpośrednio zaimplementować niniejszy interfejs i pochodzi od klasy <em>DefaultControllerFactory</em>.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Znane problemy

- Instalator programu ASP.NET MVC 3 jest tylko można instalować wstępną wersję Menedżera pakietów NuGet. Po zainstalowaniu pierwszej wersji NuGet można zainstalować i zaktualizować przy użyciu Menedżera rozszerzeń programu Visual Studio. Jeśli masz jeszcze zainstalowanego Menedżera NuGet, przejdź do galerii Visual Studio rozszerzenia do aktualizacji do najnowszej wersji pakietu NuGet.
- Tworzenie nowego projektu ASP.NET MVC 3, wewnątrz folderu rozwiązania powoduje, że *obiektu NullReferenceException* błędu. Obejście polega na tworzenie projektu ASP.NET MVC 3 w katalogu głównym rozwiązania, a następnie przenieś go do folderu rozwiązania.
- Instalator może zająć więcej czasu niż w poprzednich wersjach programu ASP.NET MVC, aby zakończyć. Jest tak, ponieważ powoduje zaktualizowanie składniki programu Visual Studio 2010.
- Funkcja IntelliSense dla składni Razor nie działa po zainstalowaniu ReSharper. Jeśli masz zainstalowanym rozszerzeniem ReSharper i wykorzystać obsługę funkcji IntelliSense Razor programu ASP.NET MVC 3 RC2, zobacz wpis [Razor Intellisense i ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri omówiono sposoby używania ich ze sobą już dziś.
- Widoki CSHTML i VBHTML utworzone przy użyciu wersji Beta programu ASP.NET MVC 3 ma ich akcji kompilacji poprawnie, w wyniku czego wyświetlić te typy zostały pominięte podczas publikowania projektu. *Build Action* wartość dla tych plików powinna być ustawiona na zawartość ". ASP.NET MVC 3 RC2 rozwiązuje ten problem dla nowych plików, ale nie rozwiąże to ustawienie dla istniejących plików projektu utworzonych za pomocą wersji Beta.![](mvc3-release-notes/_static/image4.png)
- Podczas instalacji okno dialogowe akceptacji umowy licencyjnej wyświetla postanowienia licencyjne w oknie, która jest mniejsza niż planowano.
- Podczas edytowania widoku Razor (cshtml plik), przejdź do kontrolera elementu menu w programie Visual Studio nie będą dostępne, i nie Brak fragmentów kodu.
- Jeśli Zainstaluj ASP.NET MVC 3 dla programu Visual Web Developer Express na komputerze, na których nie zainstalowano programu Visual Studio, a następnie zainstalować program Visual Studio, należy ponownie zainstalować program ASP.NET MVC 3. Program Visual Studio i Visual Web Developer Express udostępniać składniki, które są uaktualniane przez Instalatora programu ASP.NET MVC 3. Ten sam problem w przypadku zainstalowania programu ASP.NET MVC 3 dla programu Visual Studio na komputerze, na które nie mają Visual Web Developer Express, a następnie później zainstalować Visual Web Developer Express.
- Instalowanie platformy ASP.NET MVC 3 RC 2 nie powoduje aktualizacji NuGet, jeśli masz już zainstalowany. Uaktualnij NuGet, przejdź do Menedżera rozszerzenia Visual Studio i powinno stanowić dostępnej aktualizacji. NuGet można uaktualnić do najnowszej wersji, w tym miejscu.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>Program ASP.NET MVC 3 w wersji Release Candidate

ASP.NET MVC w wersji Release Candidate została wydana 9 listopada 2010 r.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Nowe funkcje w wersji RC platformy ASP.NET MVC 3

W tej sekcji opisano funkcje, które zostały wprowadzone w wersji platformy ASP.NET MVC 3 RC od czasu wydania Beta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Menedżer pakietów NuGet

Platforma ASP.NET MVC 3 zawiera Menedżera pakietów NuGet (wcześniej znane jako NuPack), czyli narzędzia do zarządzania pakietami zintegrowane dodawania biblioteki i narzędzia do projektów programu Visual Studio. To narzędzie automatyzuje kroki, które deweloperzy dziś wykonać, aby pobrać bibliotekę do ich drzewa źródłowego.

Narzędzie wiersza polecenia, jako okno zintegrowana konsola, Visual Studio 2010, z menu kontekstowego programu Visual Studio oraz zestawu poleceń cmdlet programu PowerShell, można pracować z NuGet.

Aby uzyskać więcej informacji na temat programu NuGet, przeczytaj [dokumentacja programu Nuget](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Ulepszone "Nowego projektu" okno dialogowe

Podczas tworzenia nowego projektu w oknie dialogowym Nowy projekt teraz umożliwia określenie aparat widoku, a także typu projektu MVC programu ASP.NET.

![](mvc3-release-notes/_static/image5.png)

Obsługa modyfikowania listę szablonów i widokiem aparatów wymienionym w oknie dialogowym znajduje się w tej wersji.

Domyślne szablony są następujące:

Pusty. Zawiera minimalny zestaw plików dla projektu platformy ASP.NET MVC, w tym domyślna struktura katalogów dla projektów platformy ASP.NET MVC pliku Site.css, który zawiera domyślne style platformy ASP.NET MVC i katalogu skryptów, który zawiera domyślne pliki JavaScript.

Aplikację internetową. Zawiera przykładowe funkcji pokazuje sposób użycia dostawcy członkostwa ASP.NET MVC.

Listy szablonów projektu, który jest wyświetlany w oknie dialogowym jest określona w rejestrze systemu Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Bezsesyjne kontrolerów

Nowy *ControllerSessionStateAttribute* daje większą kontrolę nad zachowaniem stanu sesji dla kontrolerów, określając [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) wartość wyliczenia.

Poniższy przykład pokazuje, jak wyłączyć stanu sesji dla wszystkich żądań do kontrolera.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Poniższy przykład pokazuje, jak można ustawić stanu sesji w trybie tylko do odczytu dla wszystkich żądań do kontrolera.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Nowe atrybuty weryfikacji

#### <a name="compareattribute"></a>CompareAttribute

Nowy *CompareAttribute* atrybut weryfikacji umożliwia porównanie wartości dwóch różnych właściwości obiektu modelu. W poniższym przykładzie *ComparePassword* właściwość musi być zgodna *hasło* pola, aby był prawidłowy.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

Nowy *RemoteAttribute* atrybut weryfikacji wykorzystuje jQuery weryfikacji wtyczki w zdalnego modułu weryfikacji, który umożliwia weryfikację po stronie klienta do wywołania metody na serwerze, który wykonuje logiki rzeczywista weryfikacja.

W poniższym przykładzie *UserName* właściwość ma *RemoteAttribute* stosowane. Podczas edycji tej właściwości w widoku do edycji, sprawdzanie poprawności klienta wywoła akcję o nazwie *UserNameAvailable* na *UsersController* klasy w celu zweryfikowania tego pola.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

Poniższy przykład pokazuje odpowiedniego kontrolera.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Domyślnie nazwa właściwości, który jest stosowany do są wysyłane do metody akcji, jako parametr ciągu zapytania.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Nowe przeciążenia dla metod "LabelForModel" i "LabelFor"

Dodano nowe przeciążenia *LabelFor* i *LabelForModel* metod, które pozwalają na określenie tekstu etykiety. Poniższy przykład przedstawia sposób użycia tych przeciążeń.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Akcja podrzędna buforowania danych wyjściowych

*OutputCacheAttribute* obsługuje buforowanie danych wyjściowych akcji podrzędnych, które są wywoływane przy użyciu *Html.RenderAction* lub *Html.Action* metody pomocnika. Poniższy przykład przedstawia widok, który wywołuje inną akcję.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

*GetDate* akcji jest oznaczony za pomocą *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Po uruchomieniu ten kod wyniku wywołania metody Html.Action("GetDate") jest buforowana na 100 sekund.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>"Dodaj widok" ulepszenia okno dialogowe

Po dodaniu silnie typizowanego widoku, okno dialogowe dodawania widoku odfiltrowuje teraz większą liczbę typów nieodpowiednich niż w poprzednich wersjach, takich jak wiele typów programu .NET Framework core. Ponadto lista jest sortowana teraz według nazwy klasy, a nie przez w pełni kwalifikowana nazwa typu, co ułatwia znajdowanie typów. Na przykład nazwa typu jest teraz wyświetlane jak w poniższym przykładzie:

ClassName (przestrzeń nazw)

We wcześniejszych wersjach to będzie wyświetlany jako następujące czynności:

ObszarNazw.NazwaKlasy

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Weryfikacja żądania szczegółowe

*Wykluczyć* właściwość *atrybut ValidateInputAttribute* już nie istnieje. Aby Weryfikacja żądania pomijana dla określonej właściwości modelu podczas wiązania modelu, użyj nowej *SkipRequestValidationAttribute*.

Na przykład załóżmy, że metody akcji jest używany do edytowania wpisu w blogu:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Poniższy przykład pokazuje model widoku dla wpisu w blogu.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Gdy użytkownik przesyła niektóre kod znaczników dla właściwości Description, wiązanie modelu zakończy się niepowodzeniem z powodu weryfikacji żądań. Aby wyłączyć weryfikację żądań podczas wiązania modelu dla wpisu w blogu opis, zastosuj *SkipRequpestValidationAttribute* do właściwości, jak pokazano w poniższym przykładzie:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Alternatywnie, aby wyłączyć weryfikację żądań dla każdej właściwości w modelu, należy zastosować *atrybut ValidateInputAttribute* o wartości *false* do metody akcji:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Fundamentalne zmiany

- Kolejność wykonywania filtrów wyjątek został zmieniony na filtry wyjątków, które mają taki sam *kolejności* wartość. W programie ASP.NET MVC 2 i starszych wyjątek filtrów na kontrolerze, który miał takie same *kolejności* jako osoby korzystające z metody akcji był wykonywany przed filtry wyjątków dla metody akcji. Zazwyczaj można tak, gdy zostały zastosowane filtry wyjątków bez określonej *kolejności* wartość. W programie ASP.NET MVC 3 to zamówienie została wycofana, aby najpierw wykonana bardziej konkretny od pozostałych obsługi wyjątków. Tak jak w starszych wersjach Jeśli *kolejności* właściwość jest jawnie określona, filtry są uruchamiane w określonej kolejności.
- Dodaje nową właściwość o nazwie *FileExtensions* do *VirtualPathProviderViewEngine* klasy bazowej. Podczas wyszukiwania widoku przy użyciu ścieżki (a nie według nazwy) tylko widoki z rozszerzeniem pliku zawarte w określonej przez tę właściwość nowej listy jest traktowany jako. Jest to istotnej zmiany dla tych, którzy Zarejestruj dostawcę niestandardowej kompilacji, aby włączyć rozszerzenie pliku niestandardowe widoki formularzy sieci web i odwołują się do tych widoków przy użyciu pełnej ścieżki, a nie nazwę. Obejście polega na zmodyfikowanie wartości *FileExtensions* właściwość, aby dołączyć rozszerzenie niestandardowego pliku.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Znane problemy

- Instalator może zająć więcej czasu niż w poprzednich wersjach programu ASP.NET MVC, aby zakończyć, ponieważ powoduje zaktualizowanie składniki programu Visual Studio 2010.
- Dodaj widok szkieletu podczas wybierania astrongly wpisane szkielety mechanizmów Wyświetl właściwości tylko do zapisu. Powinny one zawsze ignorowane przez tworzenie szkieletów. Okno dialogowe dodawania widoku również szkielety mechanizmów właściwości tylko do odczytu podczas generowania widoku "Edit" lub "Utwórz". Właściwości tylko do odczytu powinny tylko szkieletu w liście i wyświetlanie widoków.
- Debugowanie nie działa w przypadku platformy ASP.NET MVC 3 jest instalowany obok Async CTP. ASP.NET MVC 3 nie może być zainstalowane obok siebie wersją Async CTP. Odinstaluj CTP Async naprawić debugowania. Aby uzyskać więcej informacji, przeczytaj [ten wpis w blogu](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) o odinstalowaniu wszystkich części programu ASP.NET MVC 3 RC.
- Razor Intellisense nie działa po zainstalowaniu Resharper. Jeśli masz zainstalowanym rozszerzeniem ReSharper i wykorzystać obsługę funkcji intellisense Razor programu ASP.NET MVC 3 RC, przeczytaj [ten wpis w blogu](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) firmy JetBrains, w którym omówiono sposoby używania ich ze sobą już dziś.
- Widoki CSHTML i VBHTML utworzone przy użyciu wersji Beta programu ASP.NET MVC 3 nie mają swoje działania kompilacji poprawnie które pomija je z publikacji. *Build Action* dla tych plików powinna być ustawiona na "Treści". ASP.NET MVC 3 RC rozwiązuje ten problem dla nowych plików, ale nie rozwiąże to ustawienie dla istniejących plików projektu utworzonych za pomocą wersji Beta.
- Instalator może zająć więcej czasu niż w poprzednich wersjach programu ASP.NET MVC, aby zakończyć, ponieważ powoduje zaktualizowanie składniki programu Visual Studio 2010.
- Dodaj widok szkieletu podczas wybierając "Edit" silnie wpisany szkielety mechanizmów widoku właściwości tylko do odczytu. Podobnie właściwości tylko do zapisu czy szkielet dla widoków "Display".
- Podczas instalacji okno dialogowe akceptacji umowy licencyjnej wyświetla postanowienia licencyjne w oknie, która jest mniejsza niż planowano.
- Instalowanie programu Visual Studio CTP Async powoduje konflikt z wersji Razor, który jest częścią programu ASP.NET MVC 3, Instalacja narzędzi. Upewnij się, że należy próbować zainstalować zarówno w Visual Studio Async CTP, jak i w wersji aparatu Razor na tym samym komputerze.
- Podczas edytowania widoku Razor (cshtml plik), przejdź do kontrolera elementu menu w programie Visual Studio nie będą dostępne, i nie Brak fragmentów kodu.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 w wersji Beta

ASP.NET MVC 3 w wersji Beta została wydana 6 października 2010. Poniższe informacje o są specyficzne dla wersji Beta i podlegają one wszystkich aktualizacji lub zmiany w powyższej sekcji platformy ASP.NET MVC 3 Release Candidate.

## <a id="0.1__Toc274034215"></a>  Nowe Featuresin platformy ASP.NET MVC 3 w wersji Beta

<a id="0.1__Default_validation_system"></a>W tej sekcji opisano funkcje, które zostały wprowadzone w wersji Beta programu ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>  Menedżer pakietów NuGet

Platforma ASP.NET MVC 3 zawiera Menedżer pakietów NuGet, czyli narzędzia do zarządzania pakietami zintegrowane Dodawanie bibliotek i narzędzi do projektów programu Visual Studio. W większości przypadków automatyzuje kroki, które deweloperzy dziś wykonać, aby pobrać bibliotekę do ich drzewa źródłowego.

Możesz pracować z NuGet jako narzędzie wiersza polecenia, jako okno zintegrowana konsola, Visual Studio 2010, z menu kontekstowego programu Visual Studio oraz zestawu poleceń cmdlet programu PowerShell.

Aby uzyskać więcej informacji na temat programu NuGet, przeczytaj [dokumentacja programu NuGet](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>  Ulepszone okno dialogowe Nowy projekt

Podczas tworzenia nowego projektu w oknie dialogowym Nowy projekt teraz umożliwia określenie aparat widoku, a także typu projektu MVC programu ASP.NET.

![](mvc3-release-notes/_static/image6.png)

Obsługa modyfikowania listę szablonów i widokiem aparatów wymienionym w oknie dialogowym nie znajduje się w tej wersji.

Domyślne szablony są następujące:

Pusty. Zawiera minimalny zestaw plików dla projektu platformy ASP.NET MVC, w tym domyślna struktura katalogów dla projektów platformy ASP.NET MVC, mały plik Site.css, który zawiera domyślne style platformy ASP.NET MVC i katalogu skryptów, który zawiera domyślne pliki JavaScript.

Aplikację internetową. Zawiera przykładowe funkcji pokazuje sposób użycia dostawcy członkostwa, w ramach platformy ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>  Uproszczony sposób na określenie silnie wpisany modeli w widokami Razor

Został uproszczony sposób, aby określić typ modelu do silnie typizowane widoki Razor za pomocą nowego @model dyrektywy CSHTML widoków i @ModelType dyrektywy VBHTML widoków. We wcześniejszych wersjach programu ASP.NET MVC należy określić, że silnie typizowany model dla elementu Razor widoków w ten sposób:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

W tej wersji można użyć następującej składni:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  Obsługa nowych stron ASP.NET Web Pages metody pomocnika

Technologia nowego składnika ASP.NET Web Pages zawiera zbiór metody pomocnika, które są przydatne podczas dodawania funkcji często używane do widoków i kontrolerów. ASP.NET MVC 3 obsługuje, za pomocą te metody pomocnika, w ramach widoków i kontrolerów (w razie potrzeby). Te metody są zawarte w zestawie System.Web.Helpers. W poniższej tabeli wymieniono kilka metod pomocniczych stron ASP.NET Web Pages.

| **Pomocnik** | **Opis** |
| --- | --- |
| Wykres | Renderowanie wykresu w widoku. Zawiera metody takie jak Chart.ToWebImage, Chart.Save i Chart.Write. |
| Kryptograficznego | Algorytmy, aby utworzyć poprawnie wyznaczania wartości skrótu ciągu inicjującego i skrótu hasła. |
| WebGrid | Renderuje kolekcji obiektów (zazwyczaj dane z bazy danych) jako siatkę. Obsługuje stronicowanie i sortowanie. |
| WebImage | Renderuje obraz. |
| Poczty w sieci Web | Wysyła wiadomość e-mail. |

Temat krótki przewodnik, który wyświetla listę wątków i podstawowa składnia jest dostępny jako część dokumentacji składni ASP.NET Razor następującego adresu URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  Obsługa iniekcji zależności dodatkowe

Tworzenie wersji platformy ASP.NET MVC 3 w wersji zapoznawczej 1, bieżąca wersja obejmuje dodano obsługę dwie nowe usługi i cztery istniejących usług i ulepszona obsługa rozdzielczości zależnościach i wspólnego lokalizatora usług.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nowy interfejs IControllerActivator dla wystąpienia szczegółowych kontrolera

Nowy interfejs IControllerActivator zapewnia bardziej precyzyjną kontrolę nad jak kontrolery są tworzone za pomocą iniekcji zależności. Poniższy kod przedstawia interfejs:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Natomiast to do roli fabryki kontrolerów. Fabryki kontrolerów jest implementacją interfejsu IControllerFactory, który jest odpowiedzialny za zarówno lokalizowanie typ kontrolera i instancji tego typu kontrolera.

Aktywatory są odpowiedzialne wyłącznie instancji typu kontrolera. Nie należy wykonywać wyszukiwanie typu kontrolera. Po zlokalizowaniu typu odpowiedniego kontrolera, fabryki kontrolerów powinny delegować metody do wystąpienia IControllerActivator do obsługi rzeczywistych podczas tworzenia wystąpienia kontrolera.

Klasa DefaultControllerFactory ma nowego konstruktora, który akceptuje IControllerFactory wystąpienia. Dzięki temu można zastosować wstrzykiwanie zależności do zarządzania tym aspektem tworzenia kontrolera bez konieczności zastąpić domyślne zachowanie wyszukiwania typu kontrolera.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Interfejs IServiceLocator zastępowane elementu IDependencyResolver

Na podstawie opinii społeczności, wersji platformy ASP.NET MVC 3 w wersji Beta została wymieniona korzystanie z interfejsu IServiceLocator z interfejsem elementu IDependencyResolver slimmed szczegółów określonych na potrzeby platformy ASP.NET MVC. Poniższy przykład przedstawia nowy interfejs:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

W ramach tej zmiany klasa ServiceLocator również zostało zastąpione klasy klasy DependencyResolver. Rejestracja mechanizmu rozpoznawania zależności jest podobna we wcześniejszych wersjach programu ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Implementacje tego interfejsu powinny być po prostu delegowane do odpowiedniego kontenera iniekcji zależności w celu udostępnienia zarejestrowanej usługi dla żądanego typu.

W przypadku braku zarejestrowanych usług żądanego typu platformy ASP.NET MVC oczekuje, że implementacje tego interfejsu, aby zwrócić wartość null z GetService i zwrócić pustą kolekcję z funkcji GetServices.

Nowa klasa klasy DependencyResolver umożliwia rejestrowanie klasy, które implementują interfejs wspólnego lokalizatora usług (IServiceLocator) albo znajduje się nowy interfejs elementu IDependencyResolver. Aby uzyskać więcej informacji o wspólnym lokalizatorze usług, zobacz [CommonServiceLocator w serwisie GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nowy interfejs IViewActivator dla szczegółowych widoku strony wystąpienia

Nowy interfejs IViewPageActivator zapewnia bardziej precyzyjną kontrolę nad jak widoku strony są tworzone za pomocą iniekcji zależności. Dotyczy to zarówno w przypadku wystąpienia WebFormView, jak i wystąpienia RazorView. Poniższy przykład przedstawia nowy interfejs:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

W ramach tych zajęć akceptują teraz argument konstruktora IViewPageActivator pozwalającej używasz wstrzykiwanie zależności do kontrolowania, jak ViewPage, ViewUserControl i WebViewPage typy są tworzone.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Obsługa nowych mechanizmu rozpoznawania zależności dla istniejących usług

Nowe wydanie obejmuje obsługę rozpoznawania zależności dla następujących usług:

- Dostawców weryfikacji modelu. Klasy, które implementują ModelValidatorProvider mogą być rejestrowane w mechanizmu rozpoznawania zależności, a system użyje ich do obsługi sprawdzania poprawności strony klienta i serwera.
- Dostawca metadanych modelu. Jedną klasę, która implementuje ModelMetadataProvider mogą być rejestrowane w mechanizmu rozpoznawania zależności, a system użyje go do udostępnienia metadanych dla systemów tworzenia szablonów i sprawdzania poprawności.
- Dostawców wartości. Klasy, które implementują ValueProviderFactory mogą być rejestrowane w mechanizmu rozpoznawania zależności, a system będzie używać ich do tworzenia dostawców wartości, które są używane przez kontroler i podczas wiązania modelu.
- Integratorów modelu. Klasy, które implementują IModelBinderProvider mogą być rejestrowane w mechanizmu rozpoznawania zależności, a system będzie używać ich do tworzenia integratorów modeli, które są używane przez system powiązania modelu.

### <a id="0.1__Toc274034221"></a>  Nowa funkcja obsługi dyskretnego kodu Ajax opartych na technologii jQuery

Platforma ASP.NET MVC zawiera metody pomocnika technologii Ajax, takie jak następujące:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Metody te za pomocą języka JavaScript do wywołania metody akcji na serwerze, a nie przy użyciu pełnego zwrotu. Ta funkcja został zaktualizowany tak, aby móc korzystać z technologii jQuery w sposób dyskretny kod. Zamiast intrusively emitowania wbudowanych skryptach klienta, te metody pomocnika oddzielić zachowanie z kodu znaczników emitowanie atrybutów HTML5 przy użyciu *danych ajax* prefiks. Zachowanie jest następnie stosowane do języka znaczników, odwołując się do odpowiednich plików JavaScript. Upewnij się, że odwołuje się następujące pliki JavaScript:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Ta funkcja jest domyślnie włączone w pliku Web.config w nowe szablony projektów programu ASP.NET MVC 3, ale jest domyślnie wyłączona dla istniejących projektów. Aby uzyskać więcej informacji, zobacz [dodano flagi całej aplikacji do sprawdzania poprawności klienckich i dyskretny kod JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) w dalszej części tego dokumentu.

### <a id="0.1__Toc274034222"></a>  Nowa funkcja obsługi dyskretnego kodu jQuery sprawdzania poprawności

Domyślnie program ASP.NET MVC 3 w wersji Beta używa dotyczącą weryfikacji jQuery w sposób dyskretny kod w celu sprawdzenia poprawności po stronie klienta. Aby włączyć sprawdzanie poprawności dyskretnego kodu klienta, wywoływania podobnie do poniższego z w widoku:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Wymaga to, że ViewContext.UnobtrusiveJavaScriptEnabled właściwość jest ustawiona na wartość true, co można zrobić, wprowadzając następujące wywołanie:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Ponadto upewnij się, że następujące pliki JavaScript są przywoływane.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Ta funkcja jest włączona domyślnie w pliku Web.config w nowe szablony projektów programu ASP.NET MVC 3, ale jest domyślnie wyłączona dla istniejących projektów. Aby uzyskać więcej informacji, zobacz [nowe flagi całej aplikacji do sprawdzania poprawności klienckich i dyskretny kod JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) w dalszej części tego dokumentu.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  Nowe flagi całej aplikacji do sprawdzania poprawności klienckich i dyskretny kod JavaScript

Można włączyć lub wyłączyć sprawdzanie poprawności klienta i dyskretny kod JavaScript za pomocą statyczne elementy członkowskie klasy HtmlHelper, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Domyślne szablony projektów włącza dyskretny kod JavaScript domyślnie. Można również włączyć lub wyłączyć tych funkcji w głównym pliku Web.config swoją aplikację przy użyciu następujących ustawień:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Ponieważ te funkcje można włączyć domyślnie, nowe przeciążenia zostały wprowadzone do klasy HtmlHelper, które umożliwiają zastępują ustawienia domyślne, jak pokazano w poniższych przykładach:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Zgodność ze starszymi wersjami obie te funkcje są domyślnie wyłączone.

### <a id="0.1__Toc274034224"></a>  Nowa funkcja obsługi kodu, który jest uruchamiany przed uruchomieniem widoków

Możesz teraz umieścić plik o nazwie \_viewstart.cshtml (lub \_viewstart.vbhtml) w katalogu Views i dodać do niego, który będzie współdzielona przez wielu widoków w tym katalogu i jego podkatalogach kod. Na przykład możesz umieścić następujący kod do \_viewstart.cshtml strony w folderze ~/Views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Spowoduje to ustawienie strony układu dla każdego widoku w folderze Widoki i wszystkie jego podfoldery cyklicznie. Jeśli widok jest renderowany, kod w \_viewstart.cshtml plik jest uruchamiany przed uruchomieniem Wyświetl kod. \_Viewstart.cshtml kod ma zastosowanie do każdego widoku w tym folderze.

Domyślnie, kod w \_viewstart.cshtml plik ma również zastosowanie do widoków w dowolnego podfolderu. Jednak poszczególnych podfolderów może mieć własną wersję programu \_viewstart.cshtml pliku; w tym przypadku wersja lokalna ma pierwszeństwo. Na przykład, aby uruchomić kod, który jest wspólny dla wszystkich widoków dla HomeController, należy umieścić \_viewstart.cshtml pliku w folderze ~/Views/Home.

### <a id="0.1__Toc274034225"></a>  Nowa funkcja obsługi VBHTML składni Razor

Poprzedniej wersji zapoznawczej platformy ASP.NET MVC uwzględnione Obsługa widoków przy użyciu składni Razor na podstawie języka C#. Widoki te używają z rozszerzeniem cshtml. Jako część pracy w toku do obsługi Razor ASP.NET MVC 3 Beta wprowadzono obsługę składni Razor w języku Visual Basic, wykorzystują rozszerzenie pliku .vbhtml.

Wprowadzenie do przy użyciu składni języka Visual Basic na stronach VBHTML zobacz samouczek następującego adresu URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  Bardziej precyzyjną kontrolę nad atrybut ValidateInputAttribute

ASP.NET MVC ma zawsze włączone klasy atrybut ValidateInputAttribute, która wywołuje infrastruktury sprawdzania poprawności żądania ASP.NET core aby upewnić się, czy przychodzące żądanie nie zawiera potencjalnie złośliwych danych. Domyślnie sprawdzenie poprawności danych wejściowych jest włączona. Istnieje możliwość wyłączyć sprawdzanie poprawności żądań za pomocą atrybutu atrybut ValidateInputAttribute, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Jednak wiele aplikacji sieci web ma pojedynczych pól formularza, które muszą zezwalać na języku HTML, podczas gdy pozostałe pola nie powinna. Klasa atrybut ValidateInputAttribute umożliwia teraz określenie listy pól, które nie powinny znajdować się w weryfikacji żądań.

Na przykład jeśli tworzysz aparatu blogu można umożliwić znacznik, w polu treści i podsumowanie. Te pola może być reprezentowany przez dwa element input, każdy atrybut nazwy odpowiadającej nazwie właściwości ("Treść" i "Summary"). Aby wyłączyć żądania weryfikacji dla tych pól Podaj tylko nazwy, (przecinkami) we właściwości wykluczania klasy ValidateInput, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  Pomocnicy przekonwertować podkreślenia łączniki dla atrybutu HTML nazw określony za pomocą anonimowego obiektów

Metody pomocników umożliwiają określenie atrybutu pary nazwa/wartość, przy użyciu obiektu anonimowego, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

To podejście nie umożliwiają używanie łączników w nazwę atrybutu, ponieważ nie można używać łącznika dla nazwy właściwości w programie ASP.NET. Jednak łączniki są ważne w przypadku atrybutów niestandardowych HTML5; na przykład HTML5 używa prefiksu "data-".

W tym samym czasie znaki podkreślenia nie można używać nazw atrybutów w formacie HTML, ale są prawidłowe w nazwy właściwości. W związku z tym Jeśli określisz atrybutów przy użyciu obiektu anonimowego i nazwy atrybutu zawierają znaku podkreślenia,, metody pomocnika przekonwertuje podkreślenia łączników. Na przykład następująca składnia pomocnika używa znaku podkreślenia:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Poprzedni przykład renderuje następujące znaczniki, po uruchomieniu elementu pomocniczego:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Poprawki błędów

Domyślny szablon obiektu dla pomocników szablonu EditorFor i DisplayFor obsługuje teraz porządkowanie określony we właściwości DisplayAttribute.Order. (W poprzednich wersjach, ustawienie kolejności nie był używany.)

Teraz sprawdzanie poprawności klienta obsługuje walidację zastąpione właściwości, które mają atrybuty weryfikacji.

JsonValueProviderFactory teraz jest zarejestrowana domyślnie.

## <a id="0.1__Toc274034229"></a>  Fundamentalne zmiany

Kolejność wykonywania filtrów wyjątek został zmieniony na filtry wyjątków, które mają taką samą wartość kolejności. W programie ASP.NET MVC 2 i starszych wyjątek służy do przefiltrowania na kontrolerze o takiej samej kolejności jak osoby korzystające z metody akcji był wykonywany przed filtry wyjątków dla metody akcji. Zwykle będzie to wymagane podczas zastosowane filtry wyjątków bez określonej wartości kolejności. W programie ASP.NET MVC 3 to zamówienie została wycofana, aby najpierw wykonana bardziej konkretny od pozostałych obsługi wyjątków. Tak jak w starszych wersjach Jeśli właściwość kolejność jest jawnie określona, filtry są uruchamiane w określonej kolejności.

## <a id="0.1__Toc274034230"></a>  Znane problemy

Podczas instalacji okno dialogowe akceptacji umowy licencyjnej wyświetla postanowienia licencyjne w oknie, która jest mniejsza niż planowano.

Nie masz widokami razor, obsługę funkcji IntelliSense, ani wyróżniania składni. Przewidywany jest Obsługa składni Razor w programie Visual Studio będą dołączane jako część nowszej wersji.

Podczas edytowania widoku Razor (CSHTML plik), <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> przejdź do kontrolera elementu menu w programie Visual Studio nie będą dostępne, a istnieją nie fragmentów kodu.

Korzystając z @model składnia określająca silnie typizowaną CSHTML służy do wyświetlania, skróty charakterystyczny dla typów nie są rozpoznawane. Na przykład @model int nie będzie działać, ale @model Int32 będzie działać. Obejście dla tej usterki dotyczy użycia nazwą rzeczywistego typu, podczas określania typu modelu.

Korzystając z @model składnia określająca silnie typizowanego widoku CSHTML (lub @ModelType do określenia silnie typizowanego widoku VBHTML), typy dopuszczające wartości zerowe i deklaracje tablic nie są obsługiwane. Na przykład @model int? nie jest obsługiwane. Zamiast tego należy użyć `@model Nullable<Int32>`. Składnia @model ciąg [] nie jest również obsługiwany; zamiast tego należy użyć `@model IList<string>`.

Kiedy uaktualniasz projekt platformy ASP.NET MVC 2 do ASP.NET MVC 3, upewnij się, że Dodaj następujący element do sekcji appSettings pliku Web.config:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Istnieje znany problem, który powoduje, że uwierzytelnianie formularzy zawsze przekierowywania nieuwierzytelnionych użytkowników do ~/Account lub identyfikator logowania, ignorowanie ustawienia uwierzytelniania formularzy, które są używane w pliku Web.config. Obejście polega na Dodaj następujące ustawienie aplikacji.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Zrzeczenie odpowiedzialności

© 2011 Microsoft Corporation. Wszelkie prawa zastrzeżone. Niniejszy dokument jest udostępniany "jako-to." Informacje i poglądy wyrażone w tym dokumencie, w tym adresy URL i inne odniesienia do witryn internetowych, mogą ulec zmianie bez powiadomienia. Użytkownik jest odpowiedzialny za jej pomocą.

W tym dokumencie nie umożliwiają żadnych praw do własności intelektualnej w jakimkolwiek produkcie firmy Microsoft. Można kopiować i używać tego dokumentu do wewnętrznych celów referencyjnych.
