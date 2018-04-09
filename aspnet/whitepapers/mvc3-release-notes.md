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
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 0bfe9cdc215226457ccfafff2b85ace87325b91b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [Omówienie](#overview)
- [Informacje o instalacji](#installation-notes)
- [Wymagania dotyczące oprogramowania](#software-requirements)
- [Dokumentacja](#documentation)
- [Obsługa](#support)
- [Uaktualnianie projektów programu ASP.NET MVC 2 do platformy ASP.NET MVC 3 narzędzia aktualizacji](#upgrading)
- [ASP.NET MVC 3 narzędzia aktualizacji (12 kwietnia 2011)](#tu-changes)

    - [Okno dialogowe "Dodaj kontroler" można teraz utworzyć szkielet kontrolerów z kodem dostępu do widoków i danych](#tu-AddControllerDialog)
    - [Ulepszenia "platformy ASP.NET MVC 3 nowego projektu" — okno dialogowe](#tu-ImprovementsNewDialogBox)
    - [Szablony projektów zawierają teraz Modernizr 1.7](#tu-Modernizr)
    - [Szablony projektu obejmują zaktualizowane wersje jQuery, interfejsu użytkownika jQuery i jQuery sprawdzania poprawności](#tu-UpdatedJQuery)
    - [Szablony projektów zawierają teraz ADO.NET Entity Framework 4.1 jako wstępnie zainstalowany pakiet NuGet](#tu-EF)
    - [Szablony projektu obejmują bibliotek JavaScript jako wstępnie zainstalowane pakiety NuGet](#tu-JavaScriptLibsNuget)
    - [Znane problemy](#tu-KI)
- [RTM programu ASP.NET MVC 3 (13 stycznia 2011)](#MVC3RTM)

    - [Zmień: Zaktualizowana wersja interfejsu użytkownika jQuery do 1.8.7](#RTM-1)
    - [Zmień: Zmieniono domyślny ModelMetadataProvider z powrotem do DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Stały: Wklejanie części wyrażenia Razor zawierający wyników odstępu, w tym zostały zamienione](#RTM-3)
    - [Stałym: Zmiana nazwy pliku Razor, który jest otwarty w edytorze wyłącza kolorowanie składni i IntelliSense](#RTM-4)
    - [Znane problemy](#RTM-KI)
    - [Fundamentalne zmiany](#RTM-BC)
- [Program ASP.NET MVC 3 Release Candidate 2 (10 grudnia 2010)](#_Toc2)

    - [Zmieniać szablonów projektu o jQuery 1.4.4, 1.7 weryfikacji jQuery i jQuery 1.8.6y interfejsu użytkownika 1.8.6 interfejsu użytkownika](#_Toc2_1)
    - [Klasa dodano "AdditionalMetadataAttribute"](#_Toc2_2)
    - [Ulepszone widoku szkieletów](#_Toc2_3)
    - [Dodano Html.Raw — metoda](#_Toc2_3)
    - [Właściwości zmieniono nazwę "Controller.ViewModel" i "Widok" do "Obiekt ViewBag."](#_Toc2_4)
    - [Zmieniono nazwę "ControllerSessionStateAttribute" klasy "SessionStateAttribute"](#_Toc2_5)
    - [Zmieniono nazwę RemoteAttribute "Pola" właściwości "AdditionalFields"](#_Toc2_6)
    - [Zmieniona "SkipRequestValidationAttribute" do "AllowHtmlAttribute"](#_Toc2_7)
    - [Metoda zmienione "Html.ValidationMessage" w celu wyświetlenia pierwszy przydatne komunikat](#_Toc2_8)
    - [Stałe @model deklaracji nie dodać odstępu do dokumentu](#_Toc2_9)
    - [Dodano "FileExtensions" dla właściwości aparatów widoków do obsługi nazw plików specyficznych dla aparatu](#_Toc2_10)
    - [Pomocnik stałej "LabelFor" Aby emitować poprawną wartość dla atrybutu "For"](#_Toc2_11)
    - [Metody stałej "RenderAction" pierwszeństwo jawne wartości podczas tworzenia powiązania modelu](#_Toc2_12)
    - [Fundamentalne zmiany](#_Toc2_BC)
    - [Znane problemy](#_Toc2_KI)
- [Program ASP.NET MVC 3 Release Candidate (9 lis 2010)](#TOC_ASP_NET_3_RC)

    - [Nowe funkcje w wersji platformy ASP.NET MVC 3 RC](#_Toc276711785)
    - [NuGet Package Manager](#_Toc276711786)
    - [Ulepszone "Nowego projektu" — okno dialogowe](#_Toc276711787)
    - [Bezsesyjne kontrolerów](#_Toc276711788)
    - [Nowe atrybuty weryfikacji](#_Toc276711789)
    - [Nowe przeciążenia metod "LabelForModel" i "LabelFor"](#_Toc276711790)
    - [Akcja podrzędna buforowanie danych wyjściowych](#_Toc276711791)
    - ["Dodaj widok" ulepszenia — okno dialogowe](#_Toc276711792)
    - [Sprawdzanie poprawności żądań szczegółowego](#_Toc276711793)
    - [Fundamentalne zmiany](#_Toc276711794)
    - [Znane problemy](#_Toc276711795)
- [ŚRODOWISKO ASP. Informacje o wersji Beta MVC 3 (6 Oct 2010)](#TOC_ASP_NET_3_Beta)

    - [Nowe funkcje w wersji Beta programu ASP.NET MVC 3](#0.1__Toc274034215)
    - [NuPack Package Manager](#0.1__Toc274034216)
    - [Ulepszone okno dialogowe Nowy projekt](#0.1__Toc274034217)
    - [Uproszczony sposób określania silnie wpisana modeli widokami Razor](#0.1__Toc274034218)
    - [Obsługa nowej metody pomocnicze stron sieci Web ASP.NET](#0.1__Toc274034219)
    - [Obsługa iniekcji zależności dodatkowe](#0.1__Toc274034220)
    - [Nowa funkcja obsługi dyskretnego kodu na podstawie jQuery Ajax](#0.1__Toc274034221)
    - [Nowa funkcja obsługi dyskretnego kodu jQuery sprawdzania poprawności](#0.1__Toc274034222)
    - [Nowe flagi całej aplikacji dla weryfikacji klienta i dyskretny kod JavaScript](#0.1__Toc274034223)
    - [Nowa funkcja obsługi kodu uruchamianego przed uruchomieniem widoków](#0.1__Toc274034224)
    - [Nowa funkcja obsługi VBHTML składni Razor](#0.1__Toc274034225)
    - [Bardziej szczegółową kontrolę nad atrybut ValidateInputAttribute](#0.1__Toc274034226)
    - [Pomocnicy konwertowania podkreślenia łączniki do nazwy atrybutu HTML określony przy użyciu anonimowego obiektów](#0.1__Toc274034227)
    - [Poprawki błędów](#0.1__Toc274034228)
    - [Fundamentalne zmiany](#0.1__Toc274034229)
    - [Znane problemy](#0.1__Toc274034230)
- [Zrzeczenie odpowiedzialności](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Omówienie

Ten dokument zawiera opis wersji RTM programu ASP.NET MVC 3 dla programu Visual Studio 2010. ASP.NET MVC to platforma do tworzenia aplikacji sieci Web, która korzysta ze wzorca Model-widok-kontroler (MVC). Instalator programu ASP.NET MVC 3 obejmuje następujące składniki:

- Składniki wykonawcze platformy ASP.NET MVC 3
- Narzędzia platformy ASP.NET MVC 3 programu Visual Studio 2010
- Składniki wykonawcze stron sieci Web ASP.NET
- Narzędzia Visual Studio 2010 stron sieci Web ASP.NET
- Menedżer pakietów dla platformy .NET (NuGet)
- Aktualizacja dla programu Visual Studio 2010, który umożliwia obsługę składni Razor. (Aby uzyskać więcej informacji, zobacz artykuł bazy wiedzy 2483190).

Pełny zestaw informacje o wersji dla każdej wersji wstępnej programu ASP.NET MVC 3 można znaleźć w witrynie internetowej platformy ASP.NET z następującego adresu URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Informacje o instalacji

Aby zainstalować program ASP.NET MVC 3 RTM za pomocą Instalatora platformy sieci Web (Web PI), odwiedź następującą stronę:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Alternatywnie można pobrać Instalatora RTM programu ASP.NET MVC 3 dla programu Visual Studio 2010 z następującej strony:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 można zainstalować i uruchomić side-by-side z platformy ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Wymagania programowe

Składniki wykonawcze ASP.NET MVC 3 wymagają następującego oprogramowania:

- .NET framework w wersji 4. 

    ASP.NET MVC 3 programu Visual Studio 2010 narzędzia wymagają następującego oprogramowania:
- Visual Studio 2010 ani Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

Dokumentacja dla platformy ASP.NET MVC jest dostępna w witrynie sieci Web MSDN pod adresem URL:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Samouczki i inne informacje o platformie ASP.NET MVC są dostępne na stronie MVC witryny sieci Web ASP.NET z następującego adresu URL:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Obsługa

Jest to pełnej wersji. Informacje o uzyskiwaniu pomocy technicznej można znaleźć w folderze [witryny sieci Web Microsoft Support](https://support.microsoft.com/).

Możesz również post pytania dotyczące tej wersji na forum platformy ASP.NET MVC, w których często można zapewnić obsługę nieformalne są członkami społeczności programu ASP.NET:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Uaktualnianie projektów programu ASP.NET MVC 2 do platformy ASP.NET MVC 3 narzędzia aktualizacji

ASP.NET MVC 3 można zainstalować równolegle program ASP.NET MVC 2 na tym samym komputerze, który zapewnia elastyczność w wyborze, gdy do uaktualniania aplikacji ASP.NET MVC 2 platformy ASP.NET MVC 3.

Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 2 do wersji 3, wykonaj następujące czynności:

1. Utwórz nowy pusty projekt ASP.NET MVC 3 na komputerze. Ten projekt będzie zawierać pliki, które są wymagane do uaktualnienia.
2. Skopiuj następujące pliki z projektu programu ASP.NET MVC 3 w odpowiedniej lokalizacji projektu programu ASP.NET MVC 2. Należy zaktualizować wszystkie odwołania do biblioteki jQuery w celu uwzględnienia nowej nazwy pliku (jQuery 1.5.1.js): 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*js
    - /Zawartości/motywów/\*.\*
3. Kopiuj *pakiety* folderu w folderze głównym puste rozwiązanie projektu programu ASP.NET MVC 3, w folderze głównym Twojego rozwiązania, które znajduje się w katalogu, w którym znajduje się plik .sln rozwiązania.
4. Jeśli projekt platformy ASP.NET MVC 2 zawiera obszary, skopiować plik /Views/Web.config *widoków* folderu każdego obszaru.
5. W obu plikach Web.config w projekcie platformy ASP.NET MVC 2 globalnie wyszukiwania i zamieniania wersji platformy ASP.NET MVC. Znajdź następujące czynności: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Zastąp go z następujących czynności:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. W Eksploratorze rozwiązań, Usuń odwołanie do *System.Web.Mvc* (który wskazuje plik DLL, z wersji 2), a następnie dodaj odwołanie do *System.Web.Mvc* (v3.0.0.0).
7. Dodaj odwołanie do System.Web.WebPages.dll i System.Web.Helpers.dll. Te zestawy znajdują się w następujących folderach: 

    - %ProgramFiles%\ 3\Assemblies MVC ASP.NET\ASP.NET firmy Microsoft
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Pages\v1.0\Assemblies sieci Web
8. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu i wybierz Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę projektu i wybierz polecenie Edytuj *ProjectName*.csproj.
9. Zlokalizuj *ProjectTypeGuids* elementu i zastępowanie {F85E285D-A4E0-4152-9332-AB1D724D3325} z {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Zapisać zmiany, kliknij prawym przyciskiem myszy projekt i wybierz pozycję Załaduj ponownie projekt.
11. W pliku Web.config katalogu głównego aplikacji, Dodaj następujące ustawienia, aby *zestawy* sekcji. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Jeśli projekt odwołuje się do żadnych bibliotek innych firm, które są kompilowane przy użyciu platformy ASP.NET MVC 2, Dodaj następujący wyróżniony *bindingRedirect* elementu do pliku Web.config w katalogu głównym aplikacji, w obszarze  *Konfiguracja* sekcji: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Zmiany w programie ASP.NET MVC 3 narzędzia aktualizacji

W tej sekcji opisano zmiany wprowadzone w wersji aktualizacji narzędzi programu ASP.NET MVC 3 od wersji RTM programu ASP.NET MVC 3.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Okno dialogowe "Dodaj kontroler" można teraz utworzyć szkielet kontrolerów z kodem dostępu do widoków i danych

Funkcja szkieletów jest pozwala szybko Generowanie kontrolera i widoków dla aplikacji. Po wygenerowaniu kodu można edytować go, aby spełnić wymagania dotyczące projektu.

Aby uruchomić *Dodaj kontroler* okno dialogowe w programie ASP.NET MVC 3, kliknij prawym przyciskiem myszy *kontrolerów* folderu w *Eksploratora rozwiązań*, kliknij przycisk *Dodaj*, a następnie kliknij przycisk *kontrolera*. Okno dialogowe rozszerzono szkieletów dodatkowych opcji.

![](mvc3-release-notes/_static/image1.png)

Domyślnie dostępne są trzy szablony szkieletów.

#### <a name="empty-controller"></a>Pusty kontroler

Ten szablon zostanie wygenerowany plik pusty kontroler. Ten szablon jest odpowiednikiem nie sprawdza *Dodawanie akcji do tworzenia, edytowania, szczegóły, Usuń scenariusze* w poprzednich wersjach programu ASP.NET MVC. Jeśli wybierzesz, są dostępne żadne dodatkowe opcje.

#### <a name="controller-with-empty-readwrite-actions"></a>Kontroler z akcjami odczytu/zapisu pusty

Ten szablon generuje plik kontrolera, który zawiera wszystkie metody niezbędne czynności, ale żaden kod implementacji metod. Ten szablon jest odpowiednikiem sprawdzanie *Dodawanie akcji do tworzenia, edytowania, szczegóły, Usuń scenariusze* w poprzednich wersjach programu ASP.NET MVC. Jeśli wybierzesz, są dostępne żadne dodatkowe opcje.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Kontroler z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework

Ten szablon umożliwia szybkie tworzenie interfejsu użytkownika pracy wprowadzania danych. Generuje kod, który obsługuje szereg typowych wymagań i scenariusze, takie jak następujące:

- *Dostęp do danych*. Wygenerowany kod odczytuje i zapisuje jednostek w bazie danych. Po wybraniu istniejącej klasy kontekstu danych, lub jeśli umożliwisz wygenerować nowy szablon działa z podejścia Entity Framework Code First *DbContext* klasy. Współdziała również z podejścia Entity Framework bazy danych First lub Model First wybranie istniejącego *ObjectContext* klasy.
- *Sprawdzanie poprawności*. Wygenerowany kod używa wiązania modelu platformy ASP.NET MVC i funkcje metadanych, przesyłanie formularza są weryfikowane zgodnie z regułami zadeklarowana w klasie modelu. Obejmuje to wbudowane reguły sprawdzania poprawności, takich jak *wymagane* i *StringLength* atrybuty i niestandardowych reguł walidacji.
- *Relacje jeden do wielu*. W przypadku definiowania relacji klucza obcego jeden do wielu między klasach modeli, wygenerowany kod spowoduje utworzenie listy rozwijane wyboru powiązanych jednostek. Na przykład można zdefiniować następujące klasy modelu Entity Framework Code First konwencjami: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Po utworzeniu szkieletu następnie kontrolera dla *produktu* klasy, widokach umożliwi użytkownikom wybór *kategorii* obiekt dla każdego *produktu* wystąpienia.

    Ten szablon umożliwia dodatkowe opcje w *Dodaj kontroler* okno dialogowe. Aby uzyskać *klasa modelu*, można wybrać dowolną klasę modelu w rozwiązaniu, który określa typ danych, które użytkownicy będą mogli tworzyć lub edytować:
- Jeśli chcesz użyć programu Entity Framework Code First, można wybrać dowolną klasę modelu.
- Jeśli korzystasz z programu Entity Framework bazy danych First lub Entity Framework Model First, należy wybrać klasę jednostek zdefiniowanych w modelu koncepcyjnym.

Aby uzyskać *klasy kontekstu danych*, można wybrać następujące opcje:

- Jeśli chcesz używać Code First i mają kontekstu danych istniejącej klasy, wybierz ** nowy kontekst danych **. Klasa kontekstu danych następnie zostanie wygenerowany dla Ciebie.
- Jeśli chcesz używać Code First i mają istniejącej klasy kontekstu danych, wybierz go tutaj. Będzie można zaktualizować do utrwalenia klasy modelu, który wybrano.
- Jeśli korzystasz z pierwszego bazy danych lub Model First, należy wybrać klasy kontekstu obiektów.

W widokach wybierz aparat widoku, który ma być używany, lub wybierz wyrażenie None, jeśli nie chcesz utworzyć szkielet żadnych widoków.

Możesz wybrać zaawansowane Optionsto określić dodatkowe opcje wygenerowanych widoków. Można na przykład, układu i strony wzorcowej do użycia.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Ulepszenia "platformy ASP.NET MVC 3 nowego projektu" — okno dialogowe

Okno dialogowe, które służy do tworzenia nowych projektów programu ASP.NET MVC 3 zawiera wiele ulepszeń wymienione poniżej.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Nowy szablon "Projekt Intranet"

Lista szablon projektu zawiera nowy szablon aplikacji w sieci Intranet. Ten szablon zawiera ustawienia umożliwiające tworzenie aplikacji sieci web przy użyciu uwierzytelniania systemu Windows zamiast uwierzytelniania formularzy. Ponieważ aplikacja intranet wymaga niektóre ustawienia usług IIS, które nie są umieszczane w szablonie projektu, szablon zawiera plik readme z instrukcjami, jak szablon projektu w programie IIS działają. Dokumentacja szablonu aplikacji sieci Intranet jest dostępna w witrynie MSDN pod adresem URL:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Szablony projektu są teraz włączone HTML5

Okno dialogowe Nowy projekt zawiera teraz opcję, aby dodać funkcje specyficzne dla HTML5 szablonów projektu. Wybranie opcji powoduje, że widoki ma zostać wygenerowane, które zawierają nowe HTML5 `<header>`, `<footer>`, i `<navigation>` elementy.

Pamiętaj, że starsze wersje przeglądarek nie obsługują tagów specyficznych dla HTML5. Aby rozwiązać ten limit, szablony projektu HTML5 zawierają odwołanie do biblioteki Modernizr. (Zobacz następną sekcję).

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Szablony projektów zawierają teraz Modernizr 1.7

Modernizr jest biblioteka języka JavaScript, która umożliwia obsługę CSS 3 oraz HTML5 w przeglądarkach, które jeszcze nie obsługuje te funkcje. Ta biblioteka jest uwzględniona jako wstępnie zainstalowany pakiet NuGet w szablonach dla projektów ASP.NET MVC 3. Aby uzyskać więcej informacji o Modernizr, zobacz [ http://www.modernizr.com/ ](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Szablony projektu obejmują zaktualizowane wersje jQuery, interfejsu użytkownika jQuery i jQuery sprawdzania poprawności

Szablony projektów zawierają teraz skryptów jQuery następujące wersje:

- jQuery 1.5.1
- 1.8 weryfikacji jQuery
- jQuery UI 1.8.11

Te biblioteki są dołączone jako wstępnie zainstalowane pakiety NuGet.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Szablony projektów zawierają teraz ADO.NET Entity Framework 4.1 jako wstępnie zainstalowany pakiet NuGet

4.1 ADO.NET Entity Framework zawiera funkcję Code First. Kod jest najpierw nowy wzorzec programowanie ADO.NET Entity Framework, który stanowi alternatywę dla istniejących wzorców pierwszy bazy danych i Model First.

Kod najpierw koncentruje się wokół Definiowanie modelu za pomocą klasy POCO ("zwykły starego CLR obiekty"), napisany w języku Visual Basic lub C#. Te klasy można mapować do istniejącej bazy danych lub używane do generowania schematu bazy danych. Dodatkowej konfiguracji mogą być dostarczane za pomocą *DataAnnotations* atrybuty lub przy użyciu interfejsów API fluent.

Dokumentacji przy użyciu kodu Firstwith ASP.NET MVC jest dostępna w witrynie sieci Web programu ASP.NET, w następujących adresów URL:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Szablony projektu obejmują bibliotek JavaScript jako wstępnie zainstalowane pakiety NuGet

Podczas tworzenia nowego projektu platformy ASP.NET MVC 3, projekt zawiera już wspomniano pliki JavaScript (na przykład biblioteki Modernizr), instalując je przy użyciu narzędzia NuGet zamiast bezpośrednio dodawania skryptów w folderze skryptów w szablonie projektu zawartość. Umożliwia to użycie narzędzia NuGet można zaktualizować skrypty do najnowszej wersji, po udostępnieniu nowej wersji skryptów.

Na przykład, biorąc pod uwagę częstotliwość nowych wersji jQuery, wersja jQuery zawarte w szablonie projektu w pewnym momencie będzie nieaktualny. Jednak ponieważ jQuery jest uwzględniona jako zainstalowanego pakietu NuGet, użytkownik otrzyma powiadomienie w oknie dialogowym NuGet po są dostępne nowsze wersje jQuery.

Ponieważ jQuery zawiera numeru wersji w nazwie pliku, aktualizowanie do najnowszej wersji jQuery również wymaga zaktualizowania `<script>` tag, który odwołuje się do pliku jQuery do użycia nowej nazwy pliku. Inne biblioteki dołączony skryptu nie zawierają numer wersji w polu Nazwa skryptu, dlatego może być łatwiej aktualizowana do swoich najnowszej wersji.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Znane problemy

- W niektórych przypadkach instalacja może zakończyć się błąd komunikat "Instalacja nie powiodła się z kodem błędu (0x80070643)". Aby uzyskać informacje dotyczące sposobu obejścia tego problemu, zobacz [artykule bazy wiedzy 2531566](https://support.microsoft.com/kb/2531566).
- Funkcja szkieletów dodawania kontrolera nie szkieletu podmiotom wykorzystać zalety jednostki dziedziczenia w ramach programu Entity Framework. Przykładowo, podana podstawowej *osoby* klasy, która jest dziedziczona przez *uczniowie* klasy tworzenia szkieletu *uczniowie* klasy spowoduje wygenerowany kod, który nie kompiluje się.
- Tworzenie nowego projektu platformy ASP.NET MVC 3 w folderze rozwiązania przyczyny *NullReferenceException* błędu. Obejście jest utworzenie projektu programu ASP.NET MVC 3 w folderze głównym rozwiązania, a następnie przenieś go do folderu rozwiązania.
- IntelliSense dla składni Razor nie działa, gdy ReSharper jest zainstalowany. Jeśli masz zainstalowany ReSharper i chcesz skorzystać obsługę funkcji IntelliSense Razor ASP.NET MVC 3, zobacz wpis [Razor Intellisense i ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri, który opisano sposoby użyć ich razem dzisiaj.
- Podczas instalacji postanowienia licencyjne wyświetlone okno dialogowe akceptacji umowy licencyjnej w oknie, która jest mniejsza niż zamierzony.
- Podczas edytowania widoku Razor (cshtml lub. *vbhtml* pliku), widoki. ASP.NET MVC 3 nie obejmuje wszystkie fragmenty widokami Razor. przedstawia fragmenty aspxselecting fragment kodu dla platformy ASP.NET MVC
- Jeśli instalowanie platformy ASP.NET MVC 3 dla programu Visual Web Developer Express na komputerze, na którym nie zainstalowano programu Visual Studio, a następnie zainstalować program Visual Studio, należy ponownie zainstalować program ASP.NET MVC 3. Udostępnianie składników, które są uaktualniane przez Instalator programu ASP.NET MVC 3, Visual Studio i Visual Web Developer Express. Ten sam problem w przypadku zainstalowania programu ASP.NET MVC 3 programu Visual Studio na komputerze, które nie mają Visual Web Developer Express, a następnie został zainstalowany program Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Zmiany w wersji RTM programu ASP.NET MVC 3

W tej sekcji opisano zmiany i poprawki wprowadzone w wersji RTM programu ASP.NET MVC 3 od czasu wydania RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Zmień: Zaktualizowana wersja interfejsu użytkownika jQuery do 1.8.7

Szablony projektów programu ASP.NET MVC dla programu Visual Studio zostały zaktualizowane do najnowszej wersji biblioteki interfejsu użytkownika jQuery dołączenia. Szablony zawierają również minimalny zestaw plików zasobów wymaganych przez jQuery interfejsu użytkownika, takie jak skojarzone CSS i pliki obrazów.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Zmień: Zmieniono domyślny ModelMetadataProvider z powrotem do DataAnnotationsModelMetadataProvider

Wydanie RC2 programu ASP.NET MVC 3 wprowadzono *CachedDataAnnotationsMetadataProvider* zapewnianej buforowanie na istniejące klasy *DataAnnotationsModelMetadataProvider* klasy zwiększenie wydajności. Jednak niektóre usterki, były zgłaszane z tą implementacją, więc zmiany został przywrócony i przenoszony do projektu prognozy MVC, który znajduje się w temacie [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Stały: Wklejanie części wyrażenia Razor zawierający wyników odstępu, w tym zostały zamienione

W wersji wstępnych programu ASP.NET MVC 3 podczas wklejania część wyrażenia Razor zawierający odstępu w pliku Razor wynikowe wyrażenie została odwrócona. Rozważmy na przykład następujący blok kodu Razor:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Jeśli zaznacz tekst "param pierwszy" w metodzie pierwszy i wklej go jako argument do drugiej metody, wynik jest następujący:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Poprawne zachowanie jest, że operacja wklejania powinno spowodować następujące:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Ten problem został rozwiązany w wersji RTM, aby wyrażenie poprawnie są zachowywane w trakcie operacji wklejania.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Stałym: Zmiana nazwy pliku Razor, który jest otwarty w edytorze wyłącza kolorowanie składni i IntelliSense

Zmiana nazwy pliku Razor przy użyciu Eksploratora rozwiązań, gdy plik jest otwarty w oknie edytora powoduje wyróżnianie składni i IntelliSense przestanie działać dla tego pliku. Ten problem został naprawiony, dzięki czemu wyróżnianie i IntelliSense mają być przechowywane po zmiany nazwy.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Znane problemy

- Jeśli zamkniesz programu Visual Studio 2010 z dodatkiem SP1 Beta otwarciu konsoli Menedżera pakietów NuGet, Visual Studio ulega awarii i podejmie próbę ponownego uruchomienia. Ten problem zostanie rozwiązany w wersji RTM programu Visual Studio 2010 z dodatkiem SP1.
- Instalator programu ASP.NET MVC 3 może tylko zainstalować początkowa wersja Menedżera pakietów NuGet. Po zainstalowaniu wersji początkowej NuGet można zainstalować i zaktualizować przy użyciu Menedżera rozszerzeń programu Visual Studio. Jeśli masz już zainstalowany NuGet, przejdź do galerii rozszerzeń programu Visual Studio do aktualizacji do najnowszej wersji programu NuGet.
- Tworzenie nowego projektu platformy ASP.NET MVC 3 w folderze rozwiązania przyczyny *NullReferenceException* błędu. Obejście jest utworzenie projektu programu ASP.NET MVC 3 w folderze głównym rozwiązania, a następnie przenieś go do folderu rozwiązania.
- Instalator może trwać dłużej niż w poprzednich wersjach programu ASP.NET MVC, aby zakończyć. Jest to spowodowane aktualizuje składników programu Visual Studio 2010.
- IntelliSense dla składni Razor nie działa, gdy ReSharper jest zainstalowany. Jeśli masz zainstalowany ReSharper i chcesz skorzystać obsługę funkcji IntelliSense Razor ASP.NET MVC 3, zobacz wpis [Razor Intellisense i ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri, który opisano sposoby użyć ich razem dzisiaj.
- Widoki CCSHTML i VBHTML utworzone za pomocą wersji Beta programu ASP.NET MVC 3 ma ich akcji kompilacji ustawione poprawnie, w wyniku czego wyświetlić te typy zostały pominięte, gdy projekt zostanie opublikowany. "Zawartość" powinien mieć ustawioną wartość Akcja kompilacji dla tych plików. RTM programu ASP.NET MVC 3 rozwiązuje ten problem dla nowych plików, ale nie poprawne ustawienie dla istniejących plików projektu utworzonych za pomocą wersji wstępnej.
- ![](mvc3-release-notes/_static/image3.png)
- Podczas instalacji postanowienia licencyjne wyświetlone okno dialogowe akceptacji umowy licencyjnej w oknie, która jest mniejsza niż zamierzony.
- Podczas edytowania widoku Razor (cshtml plik), przejdź do kontrolera elementu menu w programie Visual Studio nie będą dostępne, a nie ma żadnych wstawki kodu.
- Jeśli instalowanie platformy ASP.NET MVC 3 dla programu Visual Web Developer Express na komputerze, na którym nie zainstalowano programu Visual Studio, a następnie zainstalować program Visual Studio, należy ponownie zainstalować program ASP.NET MVC 3. Udostępnianie składników, które są uaktualniane przez Instalator programu ASP.NET MVC 3, Visual Studio i Visual Web Developer Express. Ten sam problem w przypadku zainstalowania programu ASP.NET MVC 3 programu Visual Studio na komputerze, które nie mają Visual Web Developer Express, a następnie został zainstalowany program Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Fundamentalne zmiany

- W poprzednich wersjach programu ASP.NET MVC działania, które są następujące filtry tworzenie na żądanie z wyjątkiem w niektórych przypadkach. To zachowanie nie zostały zachowanie gwarantuje, ale tylko szczegóły implementacji i kontraktu dla filtrów była wziąć pod uwagę ich bezstanowych. W programie ASP.NET MVC 3 filtry są buforowane bardziej agresywnie. W związku z tym wszystkie filtry akcji niestandardowej, które nieprawidłowo przechowują stan wystąpienia może być uszkodzona.
- Kolejność wykonywania filtrów wyjątków została zmieniona dla filtry wyjątków, które mają taki sam *kolejności* wartość. W programie ASP.NET MVC 2 i starszych wyjątek filtry na kontrolerze, który ma takie same *kolejności* wartość jak te na metody akcji są wykonywane przed filtry wyjątków dla metody akcji. Zazwyczaj można sytuacji, gdy są stosowane filtry wyjątków bez określonej *kolejności* wartość. W programie ASP.NET MVC 3 ta kolejność została wycofana tak, aby najpierw wykonana specyficzny obsługi wyjątków. Jak w starszych wersjach Jeśli *kolejności* jawnie określono właściwość, filtry są uruchamiane w określonej kolejności.
- Nowe właściwości o nazwie *FileExtensions* został dodany do *VirtualPathProviderViewEngine* klasy podstawowej. Gdy ASP.NET wyszukuje widoku za pomocą ścieżki (a nie według nazwy), są traktowane jako tylko widoki z rozszerzeniem zawarte na liście określonej przez tę właściwość nowe. W przypadku, gdy dostawca niestandardowej kompilacji jest zarejestrowany w celu umożliwienia rozszerzenie pliku niestandardowych widoków formularza sieci Web, a dostawca odwołuje się do tych widoków przy użyciu pełnej ścieżki, a nie nazwę jest istotne zmiany w aplikacjach. Należy zmodyfikować wartość *FileExtensions* właściwości do niestandardowego pliku rozszerzenia.
- Implementacje fabryka kontrolera niestandardowego, bezpośrednio implementujących *IControllerFactory* interfejs musi zapewniać implementację nowej *GetControllerSessionBehavior* metody dodaje do interfejsu w tej wersji. Ogólnie rzecz biorąc, zaleca się, możesz nie implementuje ten interfejs bezpośrednio i pochodzi z klasy *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Zmiany w platformie ASP.NET MVC 3 RC2

W tej sekcji opisano zmiany (nowe funkcje i poprawki błędów) w wersji platformy ASP.NET MVC 3 RC2 od wersji RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Zmieniać szablonów projektu o jQuery 1.4.4, 1.7 weryfikacji jQuery i jQuery 1.8.6 interfejsu użytkownika

Szablony projektów programu ASP.NET MVC 3 zawierają teraz najnowsze wersje jQuery, weryfikacji jQuery i jQuery interfejsu użytkownika. jQuery interfejsu użytkownika jest nowe uzupełnienie szablony projektów i zawiera elementy widget interfejsu użytkownika użyteczne. Aby uzyskać więcej informacji na temat interfejsu użytkownika jQuery, odwiedź stronę ich strony głównej: [ http://jqueryui.com/ ](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Klasa dodano "AdditionalMetadataAttribute"

Można użyć *AdditionalMetadataAttribute* klasę, aby wypełnić *ModelMetadata.AdditionalValues* słownika właściwości modelu.

Na przykład załóżmy, że model widoku ma właściwości, które powinny być wyświetlane tylko dla administratora. Modelu mogą być adnotowany przy nowy atrybut, używając AdminOnly jako klucz i wartość true jako wartość, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Te metadane były dostępne dla dowolnego szablonu ekranu lub edytorze podczas renderowania widoku modelu produktu. Od użytkownika jest Deweloper aplikacji, aby zinterpretować informacji o metadanych.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Ulepszone widoku szkieletów

Szablony T4 używany dla widoki szkieletów teraz wygenerowania wywołania metody pomocnicze szablonu, taką jak *EditorFor* zamiast pomocników, takich jak *TextBoxFor*. Modyfikacja ta poprawia obsługę metadanych modelu w postaci atrybuty adnotacji danych, gdy okno dialogowe dodawania widoku generuje widoku.

Dodaj widok szkieletu także ulepszone wykrywania i użycia informacje o kluczu podstawowym w modelu, na podstawie Konwencji. Na przykład okno dialogowe dodawania widoku używa tych informacji, aby upewnić się, że wartości klucza podstawowego nie jest szkieletu jako pola formularza można edytować.

Domyślne edycji i Utwórz szablony zawierają odwołań do skryptów jQuery potrzebne dla weryfikacji klienta.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Dodano Html.Raw — metoda

Domyślnie Razor wyświetlić aparat koduje HTML wszystkie wartości. Na przykład poniższy fragment kodu koduje HTML w zmiennej pozdrowienia, tak aby był wyświetlany na stronie jako `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Nowy *Html.Raw* metoda zapewnia prosty sposób wyświetlania niekodowany kod HTML, gdy zawartość jest znana jako bezpieczne. W poniższym przykładzie przedstawiono te same parametry, ale ten ciąg jest renderowany jako kod znaczników:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Właściwości zmieniono nazwę "Controller.ViewModel" i "Widok" do "Obiekt ViewBag."

Wcześniej *ViewModel* właściwość *kontrolera* zgodnych do *widoku* właściwości widoku. Obie te właściwości umożliwiają dostęp do wartości *ViewDataDictionary* przy użyciu składni metody dostępu właściwości dynamicznych. Obie właściwości, że zmieniono być takie same, aby uniknąć pomyłek i być bardziej spójny.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Zmieniono nazwę "ControllerSessionStateAttribute" klasy "SessionStateAttribute"

*ControllerSessionStateAttribute* klasy została wprowadzona w wersji RC programu ASP.NET MVC 3. Właściwość została zmieniona na musi być bardziej zwięzły.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Zmieniono nazwę RemoteAttribute "Pola" właściwości "AdditionalFields"

*RemoteAttribute* klasy *pola* właściwości spowodowała dezorientację wśród użytkowników. Zmiana nazwy tę właściwość, aby *AdditionalFields* wyjaśnia jej celem.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Zmieniona "SkipRequestValidationAttribute" do "AllowHtmlAttribute"

*SkipRequestValidationAttribute* atrybutu została zmieniona na *AllowHtmlAttribute* do reprezentowania lepiej jego przeznaczenia.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Metoda zmienione "Html.ValidationMessage" w celu wyświetlenia pierwszy przydatne komunikat

*Html.ValidationMessage* metody został rozwiązany pokazanie pierwszy komunikat przydatne o błędzie zamiast po prostu wyświetlanie pierwszego błędu.

Podczas wiązania modelu *ModelState* słownika można wypełniać z wielu źródeł z komunikatów o błędach dotyczących właściwości, w tym samym modelu (jeśli implementuje *IValidatableObject* ), z atrybutów weryfikacji właściwości i wyjątki zgłaszane, gdy właściwość jest dostępny.

Gdy *Html.ValidationMessage* metoda wyświetla komunikat dotyczący sprawdzania poprawności, pomija wpisów stanu modelu, które obejmują: exception, ponieważ te zwykle nie są przeznaczone dla użytkownika końcowego. Zamiast tego metoda szuka pierwszy komunikat weryfikacji, który nie jest skojarzony z powodu wyjątku i wyświetla ten komunikat. Jeśli taki komunikat nie zostanie znaleziony, domyślnie ogólny komunikat o błędzie skojarzony z pierwszy wyjątek.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Stałe @model deklaracji nie dodać odstępu do dokumentu

We wcześniejszych wersjach <em>@model</em> deklaracji w górnej części widoku dodany pusty wiersz do renderowanej danych wyjściowych HTML. Ten został rozwiązany, aby deklaracji nie wprowadza odstępu.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Dodano "FileExtensions" dla właściwości aparatów widoków do obsługi nazw plików specyficznych dla aparatu

Aparat widoku można zwrócić widok przy użyciu ścieżki widoku jawne, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

Aparat widoku pierwszy próbuje zawsze renderowania widoku. Domyślnie aparat widoku formularzy sieci Web jest pierwszy aparat widoku; ponieważ aparat formularzy sieci Web nie można renderować widoku Razor, występuje błąd. Aparaty widoku mają teraz *FileExtensions* obsługują właściwości, która pozwala określić listę rozszerzeń plików. Ta właściwość jest sprawdzana podczas ASP.NET określa, czy aparat widoku umożliwiający renderowanie pliku. Jest to istotne zmiany, a także bardziej szczegółowe informacje znajdują się w [fundamentalne zmiany](#_Toc2_BC) sekcji tego dokumentu.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Pomocnik stałej "LabelFor" Aby emitować poprawną wartość dla atrybutu "For"

Usterki został rozwiązany, gdzie *LabelFor* renderowane — metoda *dla* atrybut, który odpowiada *wejściowych* elementu *nazwa* zamiast tego atrybutu jej identyfikatora. Zgodnie z W3C *dla* atrybutu powinna być zgodna *wejściowych* identyfikator elementu.

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Metody stałej "RenderAction" pierwszeństwo jawne wartości podczas tworzenia powiązania modelu

W starszych wersjach, jawne wartości, które zostały przekazane do *RenderAction* — metoda zostały zostanie zignorowany na rzecz bieżące wartości formularza podczas wiązania modelu w akcji podrzędnej. Poprawka gwarantuje, że jawne wartości pierwszeństwo podczas wiązania modelu.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Fundamentalne zmiany

- W poprzednich wersjach programu ASP.NET MVC filtry akcji zostały utworzone na żądanie, chyba że w niektórych przypadkach. To zachowanie nie zostały zachowanie gwarantuje, ale tylko szczegóły implementacji i kontraktu dla filtrów była wziąć pod uwagę ich bezstanowych. W programie ASP.NET MVC 3 filtry są buforowane bardziej agresywnie. W związku z tym wszystkie filtry akcji niestandardowej, które nieprawidłowo przechowują stan wystąpienia może być uszkodzona.
- Kolejność wykonywania filtrów wyjątków została zmieniona dla filtry wyjątków, które mają taki sam *kolejności* wartość. W programie ASP.NET MVC 2 i starszych wyjątek filtry na kontrolerze, który ma takie same *kolejności* wartość jak te na metody akcji zostały wykonane przed filtrami wyjątek w metodzie akcji. Zwykle można sytuacji, gdy zostały zastosowane filtry wyjątków bez określonej *kolejności* wartość. W programie ASP.NET MVC 3 ta kolejność została wycofana tak, aby najpierw wykonana specyficzny obsługi wyjątków. Jak w starszych wersjach Jeśli *kolejności* jawnie określono właściwość, filtry są uruchamiane w określonej kolejności.
- Nowe właściwości o nazwie *FileExtensions* został dodany do *VirtualPathProviderViewEngine* klasy podstawowej. Gdy ASP.NET wyszukuje widoku za pomocą ścieżki (a nie według nazwy), są traktowane jako tylko widoki z rozszerzeniem zawarte na liście określonej przez tę właściwość nowe. W przypadku, gdy dostawca niestandardowej kompilacji jest zarejestrowany w celu umożliwienia rozszerzenie pliku niestandardowych widoków formularza sieci Web, a dostawca odwołuje się do tych widoków przy użyciu pełnej ścieżki, a nie nazwę jest istotne zmiany w aplikacjach. Należy zmodyfikować wartość *FileExtensions* właściwości do niestandardowego pliku rozszerzenia.
- Implementacje fabryka kontrolera niestandardowego, bezpośrednio implementujących <em>IControllerFactory</em> interfejs musi zapewniać implementację nowej <em>GetControllerSessionBehavior</em>  <em>Metoda, która została dodana do interfejsu w tej wersji</em>. Ogólnie rzecz biorąc, zaleca się, możesz nie implementuje ten interfejs bezpośrednio i pochodzi z klasy <em>DefaultControllerFactory</em>.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Znane problemy

- Instalator programu ASP.NET MVC 3 może tylko zainstalować początkowa wersja Menedżera pakietów NuGet. Po zainstalowaniu wersji początkowej NuGet można zainstalować i zaktualizować przy użyciu Menedżera rozszerzeń programu Visual Studio. Jeśli masz już zainstalowany NuGet, przejdź do galerii rozszerzeń programu Visual Studio do aktualizacji do najnowszej wersji programu NuGet.
- Tworzenie nowego projektu platformy ASP.NET MVC 3 w folderze rozwiązania przyczyny *NullReferenceException* błędu. Obejście jest utworzenie projektu programu ASP.NET MVC 3 w folderze głównym rozwiązania, a następnie przenieś go do folderu rozwiązania.
- Instalator może trwać dłużej niż w poprzednich wersjach programu ASP.NET MVC, aby zakończyć. Jest to spowodowane aktualizuje składników programu Visual Studio 2010.
- IntelliSense dla składni Razor nie działa, gdy ReSharper jest zainstalowany. Jeśli masz zainstalowany ReSharper i chcesz skorzystać obsługę funkcji IntelliSense Razor platformy ASP.NET MVC 3 RC2, zobacz wpis [Razor Intellisense i ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri, który opisano sposoby użyć ich razem dzisiaj.
- Widoki CSHTML i VBHTML utworzone za pomocą wersji Beta programu ASP.NET MVC 3 ma ich akcji kompilacji ustawione poprawnie, w wyniku czego wyświetlić te typy zostały pominięte, gdy projekt zostanie opublikowany. *Akcja kompilacji* wartość dla tych plików należy ustawić na zawartość ". ASP.NET MVC 3 RC2 rozwiązuje ten problem dla nowych plików, ale nie poprawne ustawienie dla istniejących plików projektu utworzonych za pomocą wersji Beta.![](mvc3-release-notes/_static/image4.png)
- Podczas instalacji postanowienia licencyjne wyświetlone okno dialogowe akceptacji umowy licencyjnej w oknie, która jest mniejsza niż zamierzony.
- Podczas edytowania widoku Razor (cshtml plik), przejdź do kontrolera elementu menu w programie Visual Studio nie będą dostępne, a nie ma żadnych wstawki kodu.
- Jeśli instalowanie platformy ASP.NET MVC 3 dla programu Visual Web Developer Express na komputerze, na którym nie zainstalowano programu Visual Studio, a następnie zainstalować program Visual Studio, należy ponownie zainstalować program ASP.NET MVC 3. Udostępnianie składników, które są uaktualniane przez Instalator programu ASP.NET MVC 3, Visual Studio i Visual Web Developer Express. Ten sam problem w przypadku zainstalowania programu ASP.NET MVC 3 programu Visual Studio na komputerze, które nie mają Visual Web Developer Express, a następnie został zainstalowany program Visual Web Developer Express.
- Instalowanie platformy ASP.NET MVC 3 RC 2 nie powoduje aktualizacji NuGet, jeśli masz już zainstalowany. Aby uaktualnić program NuGet, przejdź do Menedżera rozszerzenia usługi Visual Studio i powinny być widoczne jako dostępna aktualizacja. Można uaktualnić narzędzie NuGet do najnowszej wersji z tego miejsca.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>Program ASP.NET MVC 3 Release Candidate

ASP.NET MVC w wersji Release Candidate 9 listopada 2010 został wydany.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Nowe funkcje w wersji platformy ASP.NET MVC 3 RC

W tej sekcji opisano funkcje, które zostały wprowadzone w wersji platformy ASP.NET MVC 3 RC od czasu wydania Beta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet Package Manager

Platforma ASP.NET MVC 3 zawiera Menedżera pakietów NuGet (wcześniej znane jako NuPack), który jest zintegrowany pakiet narzędzia do zarządzania dodawania biblioteki i narzędzia do projektów programu Visual Studio. To narzędzie automatyzuje czynności, które deweloperzy obecnie wykonać próba pobrania biblioteki do drzewa ich źródła.

Możesz pracować z programem NuGet jako narzędzie wiersza polecenia, w oknie konsoli zintegrowane, Visual Studio 2010, z menu kontekstowego programu Visual Studio i jako zestaw poleceń cmdlet programu PowerShell.

Aby uzyskać więcej informacji na temat narzędzia NuGet, przeczytaj [dokumentacji Nuget](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Ulepszone "Nowego projektu" — okno dialogowe

Podczas tworzenia nowego projektu, okno dialogowe Nowy projekt teraz pozwala określić aparat widoku, jak również typ projektu programu ASP.NET MVC.

![](mvc3-release-notes/_static/image5.png)

Do modyfikowania listy szablonów i widokiem wymienionym aparaty w oknie dialogowym są obsługiwane w tej wersji.

Domyślne szablony są następujące:

Pusta. Zawiera minimalnego zestawu plików dla projektu programu ASP.NET MVC, w tym domyślnej struktury katalogów dla projektów ASP.NET MVC pliku Site.css, zawierający domyślne style ASP.NET MVC i katalog skryptów, który zawiera domyślne pliki JavaScript.

Aplikacja Internet. Zawiera funkcji przykładowych pokazano, jak użyć dostawcy członkostwa z platformą ASP.NET MVC.

Lista Szablony projektów, która jest wyświetlana w oknie dialogowym jest określona w rejestrze systemu Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Bezsesyjne kontrolerów

Nowy *ControllerSessionStateAttribute* zapewnia większą kontrolę nad zachowanie stanu sesji dla kontrolerów, określając [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) wartości wyliczenia.

Poniższy przykład pokazuje, jak wyłączyć stanu sesji dla wszystkich żądań do kontrolera.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Poniższy przykład pokazuje, jak ustawić stan sesji tylko do odczytu dla wszystkich żądań do kontrolera.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Nowe atrybuty weryfikacji

#### <a name="compareattribute"></a>CompareAttribute

Nowy *CompareAttribute* atrybut weryfikacji umożliwia porównanie wartości z dwóch różnych właściwości modelu. W poniższym przykładzie *ComparePassword* musi odpowiadać właściwości *hasło* pola, aby był prawidłowy.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

Nowy *RemoteAttribute* atrybut weryfikacji korzysta z jQuery weryfikacji plug w zdalnego modułu weryfikacji, który umożliwia weryfikację po stronie klienta do wywołania metody na serwerze, który wykonuje logiki rzeczywista weryfikacja.

W poniższym przykładzie *UserName* ma właściwość *RemoteAttribute* zastosowane. Podczas edycji tej właściwości w widoku edycji, sprawdzanie poprawności klienta wywoła akcję o nazwie *UserNameAvailable* na *UsersController* klasy do weryfikacji w tym polu.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

W poniższym przykładzie przedstawiono odpowiedniego kontrolera.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Domyślnie nazwy właściwości, która dotyczy ten atrybut jest wysyłane do metody akcji jako parametr ciągu zapytania.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Nowe przeciążenia metod "LabelForModel" i "LabelFor"

Dodano nowe przeciążenia *LabelFor* i *LabelForModel* metod, które pozwalają określić tekst etykiety. Poniższy przykład przedstawia sposób użycia tych przeciążenia.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Akcja podrzędna buforowanie danych wyjściowych

*OutputCacheAttribute* obsługuje buforowanie danych wyjściowych akcji podrzędnych, które są wywoływane przy użyciu *Html.RenderAction* lub *Html.Action* metody pomocnicze. W poniższym przykładzie przedstawiono widok wywołujący inną akcję.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

*GetDate* akcji jest oznaczony za pomocą *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Po uruchomieniu tego kodu wynik wywołania Html.Action("GetDate") są buforowane na 100 sekund.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>"Dodaj widok" ulepszenia — okno dialogowe

Po dodaniu silnie typizowanego widoku, okno dialogowe dodawania widoku odfiltrowuje teraz więcej typów nie dotyczy niż w poprzednich wersjach, takich jak wiele podstawowych typów .NET Framework. Ponadto lista jest sortowana teraz według nazwy klasy, a nie w pełni kwalifikowaną nazwę typu, co ułatwia znajdowanie typów. Na przykład nazwa typu jest teraz wyświetlone jak w poniższym przykładzie:

ClassName (przestrzeń nazw)

We wcześniejszych wersjach to czy zostały wyświetlone następujące parametry:

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Sprawdzanie poprawności żądań szczegółowego

*Wykluczyć* właściwość *atrybut ValidateInputAttribute* już nie istnieje. Aby weryfikacji żądania właściwości specyficzne dla modelu pominięte podczas tworzenia powiązania modelu, użyj nowej *SkipRequestValidationAttribute*.

Na przykład załóżmy, że metoda akcji jest używany do edytowania wpisu w blogu:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Poniższy przykład przedstawia model widoku dla wpisu w blogu.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Gdy użytkownik prześle niektórych kod znaczników dla właściwości Description, tworzenia powiązania modelu zakończy się niepowodzeniem z powodu weryfikacji żądań. Aby wyłączyć sprawdzanie poprawności żądań podczas wiązania modelu dla blogu opis, zastosuj *SkipRequpestValidationAttribute* do właściwości, jak pokazano w poniższym przykładzie:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Można również wyłączyć weryfikację żądań dla każdej właściwości w modelu, zastosować *atrybut ValidateInputAttribute* o wartości *false* do metody akcji:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Fundamentalne zmiany

- Kolejność wykonywania filtrów wyjątków została zmieniona dla filtry wyjątków, które mają taki sam *kolejności* wartość. W programie ASP.NET MVC 2 i starszych wyjątek filtry na kontrolerze, który ma takie same *kolejności* zgodnie z tymi w metody akcji zostały wykonane przed filtrami wyjątek w metodzie akcji. Zwykle można sytuacji, gdy zostały zastosowane filtry wyjątków bez określonej *kolejności* wartość. W programie ASP.NET MVC 3 ta kolejność została wycofana tak, aby najpierw wykonana specyficzny obsługi wyjątków. Jak w starszych wersjach Jeśli *kolejności* jawnie określono właściwość, filtry są uruchamiane w określonej kolejności.
- Dodaje nową właściwość o nazwie *FileExtensions* do *VirtualPathProviderViewEngine* klasy podstawowej. Podczas wyszukiwania widoku przez ścieżki (a nie przez nazwę), tylko widoków z rozszerzeniem zawartych w określonej przez tę właściwość nowej listy jest traktowany jako. Jest to istotne zmiany osób zarejestrować dostawcę niestandardowej kompilacji, aby włączyć rozszerzenie pliku niestandardowych widoków formularza sieci web i odwołują się do tych widoków przy użyciu pełnej ścieżki, a nie nazwę. Należy zmodyfikować wartość *FileExtensions* właściwości do niestandardowego pliku rozszerzenia.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Znane problemy

- Instalator może trwać znacznie dłużej niż w poprzednich wersjach programu ASP.NET MVC, aby zakończyć, ponieważ powoduje zaktualizowanie składników programu Visual Studio 2010.
- Dodaj widok funkcja szkieletów, wybierając astrongly uwzględniających typy właściwości tylko do zapisu rusztowania widoku. Te powinny zawsze być ignorowane przez funkcję szkieletów. Okno dialogowe dodawania widoku również rusztowania właściwości tylko do odczytu podczas generowania widoku "Edit" lub "Utwórz". Właściwości tylko do odczytu powinny być szkieletu tylko wyświetlania i listy widoki.
- Debugowanie nie działa, po zainstalowaniu programu ASP.NET MVC 3 obok CTP asynchronicznego. ASP.NET MVC 3 nie może być zainstalowane obok siebie z CTP asynchronicznego. Odinstaluj CTP Async naprawić debugowania. Aby uzyskać więcej informacji, przeczytaj [ten wpis w blogu](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) o odinstalowywaniu wszystkich części programu ASP.NET MVC 3 RC.
- Razor Intellisense nie działa, gdy Resharper jest zainstalowany. Jeśli masz zainstalowany ReSharper i chcesz skorzystać obsługę funkcji intellisense Razor ASP.NET MVC 3 RC, przeczytaj [ten wpis w blogu](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) z JetBrains, w którym omówiono sposoby użyć ich razem dzisiaj.
- Widoki CSHTML i VBHTML utworzone za pomocą wersji Beta programu ASP.NET MVC 3 nie mają swoje działania kompilacji poprawnie które pomija je z publikowania. *Akcja kompilacji* dla tych plików należy ustawić na "Zawartość". ASP.NET MVC 3 RC rozwiązuje ten problem dla nowych plików, ale nie poprawne ustawienie dla istniejących plików projektu utworzonych za pomocą wersji Beta.
- Instalator może trwać znacznie dłużej niż w poprzednich wersjach programu ASP.NET MVC, aby zakończyć, ponieważ powoduje zaktualizowanie składników programu Visual Studio 2010.
- Właściwości tylko do odczytu szkieletów Dodaj widok, gdy zaznaczenie "Edit" silnie wpisywane rusztowania widoku. Podobnie właściwości tylko do zapisu są szkieletu dla widoków "Display".
- Podczas instalacji postanowienia licencyjne wyświetlone okno dialogowe akceptacji umowy licencyjnej w oknie, która jest mniejsza niż zamierzony.
- Instalowanie programu Visual Studio Async CTP powoduje konflikt z wersji Razor, który jest częścią programu ASP.NET MVC 3 instalacji narzędzi. Upewnij się, że należy próbować zainstalować zarówno programu Visual Studio Async CTP, jak i wersji aparatu Razor na tym samym komputerze.
- Podczas edytowania widoku Razor (cshtml plik), przejdź do kontrolera elementu menu w programie Visual Studio nie będą dostępne, a nie ma żadnych wstawki kodu.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 Beta

Platforma ASP.NET MVC 3 Beta 6 października 2010 został wydany. Poniższe uwagi są specyficzne dla wersji Beta i podlegają aktualizacje lub zmiany w powyższej sekcji platformy ASP.NET MVC 3 Release Candidate.

## <a id="0.1__Toc274034215"></a>  Nowe Featuresin platformy ASP.NET MVC 3 w wersji Beta

<a id="0.1__Default_validation_system"></a>W tej sekcji opisano funkcje, które zostały wprowadzone w wersji Beta programu ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>  Menedżer pakietów NuGet

Platforma ASP.NET MVC 3 zawiera Menedżera pakietów NuGet, który jest zintegrowany pakiet narzędzia do zarządzania Dodawanie bibliotek i narzędzia do projektów programu Visual Studio. W większości przypadków jest to automatyzacja czynności, które deweloperzy obecnie wykonać próba pobrania biblioteki do drzewa ich źródła.

Możesz pracować z programem NuGet jako narzędzie wiersza polecenia, w oknie konsoli zintegrowane, Visual Studio 2010, z menu kontekstowego programu Visual Studio i jako zestaw poleceń cmdlet programu PowerShell.

Aby uzyskać więcej informacji na temat narzędzia NuGet, przeczytaj [dokumentacji NuGet](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>  Ulepszone okno dialogowe Nowy projekt

Podczas tworzenia nowego projektu, okno dialogowe Nowy projekt teraz pozwala określić aparat widoku, jak również typ projektu programu ASP.NET MVC.

![](mvc3-release-notes/_static/image6.png)

Obsługa modyfikowania listy szablonów i widokiem wymienionym aparaty w oknie dialogowym jest niedostępna w tej wersji.

Domyślne szablony są następujące:

Pusta. Zawiera minimalnego zestawu plików w projekcie platformy ASP.NET MVC, łącznie z domyślną strukturą katalogów dla projektów ASP.NET MVC mały plik Site.css zawierający domyślne style ASP.NET MVC i katalog skryptów, który zawiera domyślne pliki JavaScript.

Aplikacja Internet. Zawiera funkcji przykładowych pokazano, jak korzystać z dostawcy członkostwa w ramach platformy ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>  Uproszczony sposób określania silnie wpisana modeli widokami Razor

Jest teraz prostszy sposób Określ typ modelu jednoznacznie widokami Razor przy użyciu nowej @model dyrektywy CSHTML widoków i @ModelType dyrektywy w ramach VBHTML widoków. We wcześniejszych wersjach programu ASP.NET MVC należy określić, że jednoznacznie modelu dla elementu Razor widoków w ten sposób:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

W tej wersji można użyć następującej składni:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  Obsługa nowej metody pomocnicze stron sieci Web ASP.NET

Nowej technologii ASP.NET Web Pages zawiera zestaw metody pomocnicze, które są przydatne w przypadku dodawania najczęściej używane funkcje do widoków i kontrolerów. ASP.NET MVC 3 obsługuje przy użyciu tych metod pomocnika, w ramach kontrolery i widoki (w razie potrzeby). Te metody są zawarte w zestawie System.Web.Helpers. W poniższej tabeli wymieniono kilka stron ASP.NET Web Pages metody pomocnicze.

| **Pomocnik** | **Opis** |
| --- | --- |
| Wykres | Renderuje wykresu w widoku. Zawiera metody, takie jak Chart.ToWebImage, Chart.Save i Chart.Write. |
| Kryptograficznego | Używa algorytmów, aby prawidłowo utworzyć mieszania ciągu inicjującego i wartość skrótu hasła. |
| WebGrid | Renderuje kolekcji obiektów (zazwyczaj dane z bazy danych) jako siatkę. Obsługuje stronicowania i sortowania. |
| Obiekt WebImage | Renderuje obraz. |
| Poczty w sieci Web | Wysyła wiadomość e-mail. |

Temat krótki przewodnik zawierający listę pomocników i podstawowa składnia jest dostępna jako część składni ASP.NET Razor dokumentacji pod adresem URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  Obsługa iniekcji zależności dodatkowe

Korzystając z wersji platformy ASP.NET MVC 3 Preview 1, bieżącej wersji obejmuje dodanie obsługi dwóch nowych usług oraz cztery istniejących usług i lepszą obsługę rozpoznawania zależności i wspólnego lokalizatora usług.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nowy interfejs IControllerActivator dla szczegółowych kontrolera podczas tworzenia wystąpienia

Nowy interfejs IControllerActivator zapewnia bardziej precyzyjną kontrolę nad jak kontrolery są tworzone za pomocą iniekcji zależności. W poniższym przykładzie przedstawiono interfejsu:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Natomiast to rola fabryki kontrolerów. Fabryka kontrolera jest implementacją interfejsu IControllerFactory, która odpowiada zarówno do lokalizowania typ kontrolera, jak i dla instancji typu kontrolera.

Aktywatory kontrolerów są odpowiedzialne tylko dla instancji typu kontrolera. Nie należy wykonywać wyszukiwanie typu kontrolera. Po zlokalizowaniu typu prawidłowego kontrolera, fabryki kontrolerów powinny delegować do wystąpienia IControllerActivator do obsługi rzeczywistych podczas tworzenia wystąpienia kontrolera.

Klasa DefaultControllerFactory ma nowego Konstruktora akceptującego wystąpienia IControllerFactory. Dzięki temu można zastosować iniekcji zależności do zarządzania tym aspektem tworzenia kontrolera bez konieczności zastąpić domyślne zachowanie wyszukiwania Typ kontrolera.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Interfejs IServiceLocator zastąpione elementu IDependencyResolver

Oparte na opinii społeczności, wersji Beta programu ASP.NET MVC 3 zastąpiło Użyj interfejsu IServiceLocator z interfejsem elementu IDependencyResolver slimmed rozwijanej określonych na potrzeby platformy ASP.NET MVC. W poniższym przykładzie przedstawiono nowego interfejsu:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

W ramach tej zmiany klasa ServiceLocator również został zastąpiony w klasie klasy DependencyResolver. Rejestracja rozpoznawania zależności jest podobny do wcześniejszych wersji platformy ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Implementacje tego interfejsu po prostu powinny być delegowane do odpowiedniego kontenera iniekcji zależności do udostępnienia zarejestrowanej usługi dla żądanego typu.

Jeśli nie ma żadnych zarejestrowanych usług żądanego typu, ASP.NET MVC oczekuje, że implementacje tego interfejsu, aby zwrócić wartość null z elementu GetService i aby zwrócić pustą kolekcję z metodę GetServices.

Nowa klasa klasy DependencyResolver umożliwia rejestrowanie klas implementujących interfejs wspólnego lokalizatora usług (IServiceLocator) albo znajduje się nowy interfejs elementu IDependencyResolver. Aby uzyskać więcej informacji o wspólnym lokalizatorze usług, zobacz [CommonServiceLocator w serwisie GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nowy interfejs IViewActivator dla szczegółowych widoku strony podczas tworzenia wystąpienia

Nowy interfejs IViewPageActivator zapewnia bardziej precyzyjną kontrolę nad jak utworzyć widok strony wystąpienia za pomocą iniekcji zależności. Dotyczy to zarówno wystąpień WebFormView i RazorView wystąpień. W poniższym przykładzie przedstawiono nowego interfejsu:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Te klasy teraz akceptuje argumentu konstruktora IViewPageActivator pozwalającej umożliwia iniekcji zależności kontroli, jak utworzyć typy ViewPage, ViewUserControl i WebViewPage wystąpienia.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Nowa funkcja obsługi mechanizmu rozpoznawania zależności dla istniejących usług

Nowe wydanie obejmuje obsługę rozpoznawania zależności dla następujących usług:

- Dostawców weryfikacji modelu. Klasy, które implementują ModelValidatorProvider może być zarejestrowany w mechanizmu rozpoznawania zależności, a system będzie używać ich do obsługi sprawdzania poprawności strony klienta i serwera.
- Dostawca metadanych modelu. Jednej klasy, która implementuje ModelMetadataProvider może być zarejestrowany w mechanizmu rozpoznawania zależności, a system zostanie użyty do udostępnienia metadanych dla systemów tworzenia szablonów i sprawdzania poprawności.
- Dostawców wartości. Klasy, które implementują ValueProviderFactory może być zarejestrowany w mechanizmu rozpoznawania zależności, a system zostanie użyte do tworzenia dostawców wartości, które są używane przez kontroler i podczas tworzenia powiązania modelu.
- Integratorów modeli. Klasy, które implementują IModelBinderProvider może być zarejestrowany w mechanizmu rozpoznawania zależności, a system zostanie użyte do tworzenia integratorów modeli, które są używane przez system powiązania modelu.

### <a id="0.1__Toc274034221"></a>  Nowa funkcja obsługi dyskretnego kodu na podstawie jQuery Ajax

Platforma ASP.NET MVC zawiera metody pomocnicze Ajax, takie jak następujące:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Te metody używany język JavaScript do wywołania metody akcji na serwerze, a nie przy użyciu pełnego ogłaszania zwrotnego. Ta funkcja została zaktualizowana przeprowadzać jQuery w sposób dyskretny kod. Zamiast intrusively emitowanie wbudowane skrypty klienta, te metody pomocnicze do rozdzielania zachowanie z znaczników emitowanie atrybutów HTML5 przy użyciu *danych ajax* prefiks. Zachowanie jest następnie stosowany znaczników odwołując odpowiednie pliki JavaScript. Upewnij się, że odwołują się następujące pliki JavaScript:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Ta funkcja jest domyślnie włączone w pliku Web.config w nowych szablonów projektu programu ASP.NET MVC 3, ale jest domyślnie wyłączona dla istniejących projektów. Aby uzyskać więcej informacji, zobacz [dodać flag całej aplikacji dla weryfikacji klienta i dyskretny kod JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) dalszej części tego dokumentu.

### <a id="0.1__Toc274034222"></a>  Nowa funkcja obsługi dyskretnego kodu jQuery sprawdzania poprawności

Domyślnie platforma ASP.NET MVC 3 Beta używa weryfikacji jQuery w sposób dyskretny kod w celu sprawdzenia poprawności po stronie klienta. Aby włączyć sprawdzanie poprawności dyskretnego kodu klienta, należy wykonać wywołania podobnie do następującej od w widoku:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Wymaga to, że właściwość ViewContext.UnobtrusiveJavaScriptEnabled jest ustawiona na wartość true, co można zrobić za pomocą następujących wywołania:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Upewnij się, że odwołują się następujące pliki JavaScript.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Ta funkcja jest domyślnie włączone na w pliku Web.config w nowych szablonów projektu programu ASP.NET MVC 3, ale jest domyślnie wyłączona dla istniejących projektów. Aby uzyskać więcej informacji, zobacz [nowe flagi całej aplikacji dla weryfikacji klienta i dyskretny kod JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) dalszej części tego dokumentu.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  Nowe flagi całej aplikacji dla weryfikacji klienta i dyskretny kod JavaScript

Można włączyć lub wyłączyć sprawdzanie poprawności klienta i dyskretny kod JavaScript za pomocą statyczne elementy członkowskie klasy HtmlHelper, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Domyślnych szablonów projektu włącza dyskretny kod JavaScript domyślnie. Można również włączyć lub wyłączyć te funkcje w głównym pliku Web.config aplikacji przy użyciu następujących ustawień:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Ponieważ te funkcje umożliwiają domyślnie, nowe przeciążenia zostały wprowadzone do klasy HtmlHelper, które umożliwiają zastępują ustawienia domyślne, jak pokazano w poniższych przykładach:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Zgodność z poprzednimi wersjami obie te funkcje są domyślnie wyłączone.

### <a id="0.1__Toc274034224"></a>  Nowa funkcja obsługi kodu uruchamianego przed uruchomieniem widoków

Teraz możesz umieścić plik o nazwie \_viewstart.cshtml (lub \_viewstart.vbhtml) w katalogu widoki i Dodaj do niej, która będzie współdzielona przez wiele widoków w tym katalogu i jego podkatalogach kod. Na przykład może stanowić następujący kod do \_viewstart.cshtml strony w folderze ~/Views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

To ustawienie strony układu dla każdego widoku w folderze Widoki i rekursywnie wszystkie jego podfoldery. Gdy widok jest renderowany, kod w \_viewstart.cshtml plik jest uruchamiany przed uruchomieniem kodu widoku. \_Viewstart.cshtml kod ma zastosowanie do każdego widoku w tym folderze.

Domyślnie ten kod w \_viewstart.cshtml plików mają zastosowanie również do widoków w dowolnym podfolderze. Jednak poszczególnych podfolderów może mieć własną wersję z \_viewstart.cshtml pliku; w tym przypadku lokalnej wersji ma pierwszeństwo. Na przykład, aby uruchomić kod, który jest wspólny dla wszystkich widoków dla HomeController, umieść \_viewstart.cshtml pliku w folderze ~/Views/Home.

### <a id="0.1__Toc274034225"></a>  Nowa funkcja obsługi VBHTML składni Razor

W poprzedniej wersji zapoznawczej programu ASP.NET MVC uwzględnione obsługę widoków przy użyciu składni Razor oparty na języku C#. Widoki te używają z rozszerzeniem cshtml. W ramach trwającej pracy do obsługi Razor ASP.NET MVC 3 Beta wprowadzono obsługę składni Razor w języku Visual Basic, który korzysta z rozszerzeniem .vbhtml.

Aby obejrzeć wprowadzenie do przy użyciu składni języka Visual Basic na stronach VBHTML zobacz samouczek z następującego adresu URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  Bardziej szczegółową kontrolę nad atrybut ValidateInputAttribute

ASP.NET MVC ma zawsze dołączane atrybut ValidateInputAttribute klasy, która wywołuje infrastruktury weryfikacji żądania ASP.NET core aby upewnić się, czy przychodzące żądanie nie zawiera potencjalnie złośliwych danych. Sprawdzania poprawności danych wejściowych jest domyślnie włączone. Istnieje możliwość wyłączono weryfikację żądań przy użyciu atrybutu atrybut ValidateInputAttribute, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Jednak wiele aplikacji sieci web ma poszczególne pola muszą zezwalać na HTML, podczas gdy pozostałe pola nie powinny. Klasa atrybut ValidateInputAttribute umożliwia teraz określenie listy pól, które nie powinny znajdować się w weryfikacji żądań.

Na przykład jeśli tworzysz aparat blogu można umożliwić znacznik, w polach treści i podsumowanie. Tych pól może być reprezentowany przez dwa element input, każde z nich atrybutu nazwy odpowiadającej nazwie właściwości ("Treść" i "Summary"). Aby wyłączyć żądania weryfikacji dla tych pól tylko, określ nazwy (rozdzielanych przecinkami) we właściwości Wyklucz klasy ValidateInput, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  Pomocnicy konwertowania podkreślenia łączniki do nazwy atrybutu HTML określony przy użyciu anonimowego obiektów

Metody pomocnicze umożliwiają określenie atrybutu pary nazwa/wartość, przy użyciu obiektu anonimowego, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Takie podejście nie umożliwiają używanie łączników w nazwie atrybutu, ponieważ łącznik nie może służyć do nazwy właściwości w programie ASP.NET. Łączniki są jednak istotne dla atrybutów niestandardowych HTML5; na przykład HTML5 używa prefiksu "data-".

W tym samym czasie znaki podkreślenia nie można używać dla atrybutu nazwy w formacie HTML, ale są prawidłowe w nazw właściwości. W związku z tym Jeśli określisz atrybutów przy użyciu obiektu anonimowego i nazwy atrybutu zawierać znaku podkreślenia,, metody pomocnicze przekonwertuje podkreślenia łączniki. Na przykład następującej składni pomocnika używa znaku podkreślenia:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Poprzedni przykład renderuje kod znaczników następujące po uruchomieniu pomocnika:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Poprawki błędów

Domyślny szablon obiektu dla pomocników szablonu EditorFor i DisplayFor obsługuje teraz porządkowania określonego we właściwości DisplayAttribute.Order. (W poprzednich wersjach, ustawienie kolejności nie użyto.)

Teraz sprawdzanie poprawności klienta obsługuje weryfikacji zastąpione właściwości, które mają zastosowanie atrybutów sprawdzania poprawności.

JsonValueProviderFactory teraz jest zarejestrowana domyślnie.

## <a id="0.1__Toc274034229"></a>  Fundamentalne zmiany

Kolejność wykonywania filtrów wyjątków została zmieniona dla filtry wyjątków, które mają taką samą wartość kolejności. W programie ASP.NET MVC 2 i starszych wyjątek filtruje na kontrolerze o takiej samej kolejności jak te na metody akcji zostały wykonane przed filtrami wyjątek w metodzie akcji. Zwykle będzie to wielkość liter podczas zostały zastosowane filtry wyjątków bez określoną wartość kolejności. W programie ASP.NET MVC 3 ta kolejność została wycofana tak, aby najpierw wykonana specyficzny obsługi wyjątków. Jak w starszych wersjach Jeśli właściwość kolejności jawnie określono, filtry są uruchamiane w określonej kolejności.

## <a id="0.1__Toc274034230"></a>  Znane problemy

Podczas instalacji postanowienia licencyjne wyświetlone okno dialogowe akceptacji umowy licencyjnej w oknie, która jest mniejsza niż zamierzony.

Nie masz widokami razor, obsługę funkcji IntelliSense, ani wyróżnianie składni. Przewiduje się, że Obsługa składni Razor w programie Visual Studio będą dołączane jako część nowszej wersji.

Podczas edytowania widoku Razor (CSHTML plik), <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> przejdź do kontrolera elementu menu w programie Visual Studio nie będą dostępne i nie ma żadnych wstawki kodu.

Korzystając z @model wyświetlić składnię, aby określić jednoznacznie CSHTML, specyficzny dla języka skróty typów nie są rozpoznawane. Na przykład @model int nie będzie działać, ale @model Int32 będzie działać. Rozwiązania dla tego błędu jest używać nazwy typu rzeczywistego po określeniu typu modelu.

Korzystając z @model składni, aby określić silnie typizowanego widoku CSHTML (lub @ModelType do określenia silnie typizowanego widoku VBHTML), typy dopuszczające wartości zerowe i deklaracje tablicy nie są obsługiwane. Na przykład @model int? nie jest obsługiwane. Zamiast tego należy użyć `@model Nullable<Int32>`. Składnia @model ciąg [] nie jest także obsługiwane; zamiast tego należy użyć `@model IList<string>`.

Po uaktualnieniu projektu programu ASP.NET MVC 2 platformy ASP.NET MVC 3, upewnij się, że Dodaj następującą wartość do sekcji appSettings w pliku Web.config:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Jest to znany problem, który powoduje, że uwierzytelnianie formularzy zawsze przekierowywania nieuwierzytelnionych użytkowników ~/Account/logowania, ignorowanie ustawienia uwierzytelniania formularzy, które są używane w pliku Web.config. Obejście polega na Dodaj następujące ustawienie aplikacji.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Zrzeczenie odpowiedzialności

© 2011 Microsoft Corporation. Wszelkie prawa zastrzeżone. Niniejszy dokument jest udostępniany "jako — jest." Informacje i poglądy wyrażone w tym dokumencie, w tym adresy URL i inne odsyłacze do witryn internetowych, mogą ulec zmianie bez uprzedzenia. Użytkownik ponosi ryzyko związane z użyciem jej.

Ten dokument nie daje użytkownikowi żadnych praw do jakiejkolwiek własności intelektualnej związanej z jakimkolwiek produktem firmy Microsoft. Można kopiować i używać tego dokumentu do wewnętrznych celów referencyjnych.
