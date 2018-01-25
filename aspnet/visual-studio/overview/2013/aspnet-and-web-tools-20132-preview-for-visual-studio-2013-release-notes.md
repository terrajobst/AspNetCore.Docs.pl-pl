---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: "ASP.NET i sieć Web narzędzi 2013.2 dla programu Visual Studio 2013 informacje o wersji | Dokumentacja firmy Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 0e7ad52662f7ceaa1f087d007d0b14b610f90bee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET i narzędzia sieci Web 2013.2 dla programu Visual Studio 2013 informacje o wersji
====================
przez [firmy Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Informacje o instalacji

ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013.2 są powiązane w Instalatorze głównym, którą można pobrać jako część [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje dotyczące platformy ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013.2 są dostępne z [witryny sieci web ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Wymagania programowe

ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013.2 wymaga programu Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nowe funkcje programu ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013.2

W poniższych sekcjach opisano funkcje, które zostały wprowadzone w wersji.

- [Szablony projektów programu ASP.NET](#oneaspnet)
- [Obsługa protokołu SSL podczas uruchamiania aplikacji sieci Web w usługach IIS Express](#ssl)
- [Ulepszenia edytora sieci Web programu Visual Studio](#vswebeditor)
- [Łączność z przeglądarkami](#browserlink)
- [Obsługa aplikacji sieci Web usługi aplikacji Azure w programie Visual Studio](#waws)
- [Utwórz zasoby zdalne Azure podczas tworzenia nowego projektu sieci Web](#AzureResources)
- [Ulepszenia publikowania w sieci Web](#webpublish)
- [ASP.NET Scaffolding](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Formularze sieci Web ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [ASP.NET Web Pages 3.1.2](#webpages)
- [Entity Framework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Składniki Microsoft OWIN](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Szablony projektów programu ASP.NET

- Aktualizacje szablony projektów programu ASP.NET do obsługi potwierdzenia konta i resetowania hasła.
- Zaktualizuj szablon interfejsu API sieci Web platformy ASP.NET do obsługi uwierzytelniania przy użyciu na lokalne konta organizacyjne.
- Szablon ASP.NET SPA zawiera teraz uwierzytelniania opartego na widoku strony MVC i serwera. Szablon ma kontrolera WebAPI, który jest dostępny tylko dla użytkowników uwierzytelnionych.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Obsługa protokołu SSL podczas uruchamiania aplikacji sieci Web w usługach IIS Express

Aby wyeliminować ostrzeżenie o zabezpieczeniach podczas przeglądania i debugowanie HTTPS na hoście lokalnym, dodaliśmy okna dialogowego do zezwalania na program Internet Explorer i Chrome zaufania z podpisem własnym usług IIS express certyfikatu SSL.

Na przykład można ustawić właściwości projektu sieci web do używania protokołu SSL. Kliknij przycisk F4, aby wyświetlić okno dialogowe właściwości. Zmień **włączony protokół SSL** na wartość true. Skopiuj adres URL protokołu SSL.

![Właściwość Enabled protokołu SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Ustaw kartę sieci web strony właściwości projektu sieci web do używania protokołu HTTPS na podstawie adresu URL (adres URL protokołu SSL będą `https://localhost:44300/` chyba, że utworzono wcześniej SSL witryny sieci Web.)

![Ustaw adres URL projektu (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Postępuj zgodnie z instrukcjami, aby zaufać certyfikat z podpisem własnym, który wygenerował usług IIS Express.

![Ostrzeżenie protokołu SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Odczyt **ostrzeżenie o zabezpieczeniach** okna dialogowego, a następnie kliknij przycisk **tak** Jeśli chcesz zainstalować certyfikat reprezentujący localhost.

![Ostrzeżenie o zabezpieczeniach](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Witryny będą wyświetlane w programie IE lub Chrome bez ostrzeżenie o certyfikacie w przeglądarce.

![Strona HTTPS bez ostrzeżeń](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox używa magazynu certyfikatów, więc zostanie wyświetlone ostrzeżenie o.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Ulepszenia edytora sieci Web programu Visual Studio

- **Nowy element projektu JSON i edytor**: dodano element projektu w formacie JSON i edytor dla programu Visual Studio. Bieżące funkcje edytora JSON obejmują kolorowania, sprawdzanie poprawności składni, ukończenia nawias klamrowy, obramowanie, ustawienia opcji narzędzi i inne.

    ![Edytor JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    Obsługuje teraz IntelliSense [schematu JSON](http://json-schema.org/) w wersji 3 i 4. Brak pola kombi schematu wybierz istniejących schematów, edytowanie ścieżki lokalnej schematu lub po prostu przeciągania porzucić pliku JSON projektu, aby pobrać ścieżki względnej.

    ![JSON Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Edytor schematu JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nowy edytor Sass (SCSS)**: dodaliśmy mniej w VS2013 RTM i mamy teraz Sass elementu projektu i edytora. Edytor sass funkcje są porównywalne z Edytor LESS i obejmują kolorowania, zmiennej i Mixins IntelliSense, usuń znaczniki komentarza/komentarza, szybka podpowiedź, formatowanie, sprawdzanie poprawności składni, zwijania, przejdź do definicji, próbnika kolorów narzędzi opcję Ustawienia itp.

    ![Dodaj nowy element: SCSS arkusza stylów](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Edytor arkusza stylów](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nowy wybór adresu URL w HTML, Razor, arkuszy CSS mniej i Sass dokumentów:** VS 2013 dołączone nie selektora URL poza stron formularzy sieci Web. Nowy wybór adresu URL dla kodu HTML, Razor, arkuszy CSS mniej i Sass edytory jest bez okna dialogowego, fluent selektora pisania, która obsługuje usługę ".." i filtry plików list odpowiednio znaczników img i łącza.

    ![Wybór adresu URL dla znacznika obrazu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Wybór adresu URL dla widoków](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Wybór adresu URL CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Aktualizacje Edytor LESS, dodając więcej funkcji**
- **Uaktualnienie Intellisense odcinania**: dodaliśmy niestandardowa składnia odcinania dla VS intelliSense "viewModel ko-vs edytora:" składni. Można go powiązać z wielu modeli widok na stronie przy użyciu komentarzy w formularzu:

    ![Odcinania Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Dodaliśmy również obsługę zagnieżdżonych ViewModel IntelliSense, więc może przejść do szczegółów w głęboko zagnieżdżone obiekty na ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    IntelilSense wyświetlana jest pełna IntelliSense obiektu JavaScript.

    ![Obiekt IntelliSense przedstawiający pełne JavaScript](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nowy wybór adresu URL w HTML, Razor, arkuszy CSS mniej i Sass dokumentów**: VS 2013 dołączone nie selektora URL poza stron formularzy sieci Web. Nowy wybór adresu URL dla kodu HTML, Razor, arkuszy CSS mniej i Sass edytory jest bez okna dialogowego, fluent selektora pisania, która obsługuje usługę ".." i filtry plików list odpowiednio znaczników img i łącza.

    ![Wybór adresu URL dla znacznika obrazu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Wybór adresu URL dla widoków](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Wybór adresu URL CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Browser Link

- Łącze przeglądarki teraz obsługuje połączenia HTTPS i będzie listy na pulpicie nawigacyjnym przy użyciu innych połączeń tak długo, jak certyfikat jest zaufany przez przeglądarkę.
- Statyczne mapowania źródła HTML
- SPA obsługę danych mapowania
- Automatyczna aktualizacja danych mapowania

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Obsługa aplikacji sieci Web usługi aplikacji Azure w programie Visual Studio

- **Zaloguj się pomocy technicznej platformy Azure.**
- **Debugowanie zdalne i zdalnego dla aplikacji sieci web**: obsługujemy teraz [zdalne debugowanie aplikacji sieci web w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) i widoku zdalny pliki zawartości aplikacji sieci web w Eksploratorze serwera.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Utwórz zasoby zdalne Azure podczas tworzenia nowego projektu sieci Web

Dodaliśmy Azure ["Utwórz zasoby zdalne"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) pola wyboru w oknie dialogowym Nowy aplikacji sieci web. Wybierając je można zintegrować środowisko tworzenia nowej aplikacji sieci web, konfigurowanie usługi Azure site publikowania do testowania i tworzenia profilu publikowania w kilku prostych krokach.

![Nowy projekt z zasobów platformy Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publikowanie na platformie Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Ulepszenia publikowania w sieci Web

- Udoskonalanie użytkownika do publikowania.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

- **Obsługa wyliczenia:** Jeśli model jest przy użyciu wyliczenia, a następnie tworzenia szkieletu MVC wygeneruje listy rozwijanej dla wyliczenia. W nazwie wzorca MVC używa pomocników wyliczenia.
- **Wykryto obsługę**: Zaktualizowano szablonów EditorFor w szkieletów MVC, aby ich używać klas ładowania początkowego.
- **Pakiet pomocy technicznej**: MVC i Scaffolders interfejsu API sieci Web spowoduje dodanie pakietów 5.1 MVC i interfejsu API sieci Web

Poniższe zrzuty ekranu pokazują szkieletów modeli.

- Model kodu:

     ![Model kodu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Kompilowanie kodu modelu, kliknij prawym przyciskiem myszy i wybierz **Dodaj**, **nowy element szkieletu**.

     ![Dodaj nowy element szkieletu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Wybierz **kontroler MVC5 z widokami używający narzędzia Entity Framework**:

     ![Dodaj nowy kontroler MVC5 z widokami](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Dodawanie kontrolera** przy użyciu modelu:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Sprawdź wygenerowanego kodu, na przykład zawiera Views/WeekdayModels/Edit.cshtml `@Html.EnumDropDownListFor`: ![wyświetlić zawierające EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Uruchomić stronę, aby zobaczyć combobox wyliczenia wygenerowane, zwróć uwagę, że jeśli wartość może mieć wartości null, ciągiem pustym można wybrać pola kombi. Na przykład **Utwórz** strona zawiera następujące:

    ![Pola kombi, dzięki czemu pusty ciąg](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1, który RTM zostaną wydane w ramach kwietnia 2014 r. Poniżej przedstawiono istotne punkty z informacje o wersji, ale Sprawdź, czy [pełnej wersji](http://docs.nuget.org/docs/release-notes/nuget-2.8) Aby uzyskać więcej informacji na temat tych zmian.

- **Windows Phone 8.1 aplikacje docelowe**: NuGet 2.8.1 obsługuje teraz przeznaczonych dla Windows Phone 8.1 aplikacji, za pomocą monikerów struktury docelowej "WindowsPhoneApp", "WPA", "WindowsPhoneApp81" i "WPA81".
- **Poprawka rozpoznawania zależności**: podczas rozpoznawania zależności pakietów, NuGet w przeszłości zaimplementowała strategię wybranie Najniższa wersja pakietu główne i pomocnicze, spełniającego zależności w pakiecie. W odróżnieniu od wersji głównej i pomocniczej jednak wersji poprawki zawsze rozpoznano do najwyższej wersji. Chociaż zachowanie był dobrze tych, utworzyć braku determinizm podczas instalowania pakietów z zależności.
- **Przełącznik DependencyVersion**: zmienia chociaż 2.8 NuGet *domyślne* zachowanie dla rozpoznawania zależności, dodane również dokładniejszej kontroli nad procesem rozpoznawania zależności za pomocą przełącznika - DependencyVersion w Konsola Menedżera pakietów. Przełącznik umożliwia rozpoznawania zależności Najniższa wersja możliwe (domyślnie), jest najwyższa możliwa wersja najwyższy pomocnicze lub wersji poprawki. Ta opcja działa tylko dla pakiet instalacyjny polecenia programu powershell.
- **Atrybut DependencyVersion**: oprócz wymienione powyżej przełącznika - DependencyVersion NuGet również zezwolił na możliwość określenia nowy atrybut w pliku nuget.config definiowanie, co to jest wartością domyślną, jeśli DependencyVersion — przełącznik nie określono w wywołaniu pakiet instalacyjny. Ta wartość zostanie również przestrzegane za pomocą okna dialogowego Menedżer pakietów NuGet dla wszystkich operacji pakietu instalacji. Aby ustawić tę wartość, do pliku nuget.config. Dodaj atrybut poniżej:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Podgląd NuGet operacje z - whatif**: pakietów NuGet niektóre mogą mieć wykresy zależności bezpośrednich i, on być pomocne podczas instalacji, odinstalować ani zaktualizować operacji do najpierw Zobacz, co się stanie. NuGet 2.8 dodaje standardowe środowiska PowerShell — co zrobić, jeśli przełączyć się do polecenia install-package, odinstaluj pakiet i pakiet aktualizacji umożliwiające wizualizacja całego zamknięcia pakietów, do których zostanie zastosowana polecenia.
- **Starszą wersję pakietu**: nie jest rzadko do zainstalowania wstępnej wersji pakietu w celu zbadania nowe funkcje i zdecydować przywrócić ostatnią stabilną wersję. Przed NuGet 2.8 to proces wieloetapowych odinstalowaniu wersji wstępnej pakiet i jego zależności, a następnie zainstalować starszą wersję. Z 2.8 NuGet jednak pakiet aktualizacji teraz cofnie zamknięcia całego pakietu (np. drzewo zależności pakietu) do poprzedniej wersji.
- **Programowanie zależności**: wiele różnych typów możliwości mogą być dostarczane jako pakiety NuGet — w tym narzędzia, które są używane do optymalizacji procesu tworzenia. Nie należy traktować te składniki mogą być instrumentalnego Opracowując nowy pakiet, opublikowany zależność nowy pakiet, gdy jest ona nowsza. NuGet 2.8 umożliwia pakietu w celu identyfikacji pliku .nuspec w postaci developmentDependency. Po zainstalowaniu tych metadanych również zostanie dodany do pliku packages.config projektu, w którym został zainstalowany pakiet. Podczas tego pliku packages.config jest później przeanalizowane pod kątem zależności NuGet w pakiecie nuget.exe, pominie te zależności od oznaczona jako programowanie zależności.
- **Packages.config poszczególnych plików na różnych platformach**: podczas tworzenia aplikacji dla wielu platform docelowych, jest często mają różne pliki projektów dla poszczególnych środowisk odpowiednich kompilacji. Również jest często użycie różnych pakietów NuGet w plikach projektu różnych pakietów mają różne poziomy wsparcia dla różnych platform. NuGet 2.8 zapewnia ulepszoną obsługę tego scenariusza przez utworzenie pliku packages.config różnych plików dla plików inny projekt specyficzne dla platformy.
- **Powrót do lokalnej pamięci podręcznej**: pakietów NuGet chociaż są zazwyczaj używane z galerii zdalnego takich jak [galerii NuGet](http://www.nuget.org) połączenia z siecią, istnieje wiele scenariuszy, w których klient nie jest połączony. Bez połączenia sieciowego klienta NuGet nie może pomyślnie zainstalować pakiety — nawet wtedy, gdy pakiety zostały już na komputerze klienckim w lokalnej pamięci podręcznej NuGet. NuGet 2.8 dodaje automatyczne pamięci podręcznej powrotu do konsoli Menedżera pakietów.

    Funkcja rezerwowej pamięci podręcznej nie wymaga żadnych argumentów danego polecenia. Ponadto rezerwowej pamięci podręcznej jest obecnie obsługiwane tylko w konsoli Menedżera pakietów — zachowanie aktualnie nie działa w oknie dialogowym Menedżer pakietów.
- **Poprawki błędów**: jeden z głównych poprawki wprowadzone jest zwiększenie wydajności w pakiecie aktualizacji-Zainstaluj ponownie polecenie.

    Oprócz tych funkcji i popraw wyżej wymienione wydajności ta wersja programu NuGet zawiera również inne poprawki błędów. Znaleziono 181 całkowita problemy rozwiązane w wersji. Pełną listę prac elementów ustalone w NuGet 2.8, sprawdź widok [NuGet problem z obiektem śledzącym](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) w tej wersji.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Formularze sieci Web ASP.NET

- Szablony formularzy sieci Web teraz pokazują, jak wykonać potwierdzenie konta i resetowania haseł dla tożsamości ASP.NET.
- Sterującego Entity Data Source i dostawcę dynamicznych danych Entity Framework 6. Dla bardziej szczegółowe informacje można znaleźć w następującym wpisie w witrynie MSDN: [dostawcy danych dynamicznych i kontrola obiektu EntityDataSource Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Atrybut ulepszenia routingu](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Obsługa inicjowania szablony Edytora](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Obsługa wyliczenia w widokach](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Obsługa Unobstrusive MinLength / element MaxLength atrybutów](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Obsługa kontekstu "this" w dyskretnego kodu Ajax](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Różne [poprawki błędów](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Obsługa błędów globalne](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Atrybut rozszerzenia routingu](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Ulepszenia strona pomocy](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Obsługa IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Program formatujący typ nośnika BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Lepszą obsługę filtry async](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Zapytanie analizowania formatowania biblioteki klienta](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Różne [poprawki błędów](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Web Pages 3.1.2

- Różne [poprawki błędów](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework została zaktualizowana do wersji 6.1 dla środowiska uruchomieniowego i narzędzi. Entity Framework (EF) 6.1 jest aktualizacją pomocnicza do narzędzia Entity Framework 6 i zawiera szereg poprawek usterek i nowe funkcje. Szczegółowe informacje na temat EF6.1 oraz łącza do dokumentacji dla nowych funkcji, zobacz [Historia wersji programu Entity Framework](https://msdn.microsoft.com/data/jj574253). Nowe funkcje w tej wersji:

- **Narzędzia konsolidacji** zapewnia spójny sposób, aby utworzyć nowy model EF. Ta funkcja rozszerza kreatora modelu danych jednostki ADO.NET do obsługi tworzenia modeli Code First, łącznie z odtwarzania z istniejącej bazy danych. Funkcje te wcześniej były dostępne w wersji Beta jakość EF zaawansowanych narzędzi.
- **Obsługa błędów zatwierdzania transakcji** udostępnia nowe [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) korzystające z nowo wprowadzonych możliwości przechwycenia operacji transakcji. **CommitFailureHandler** umożliwia automatyczne odzyskiwanie po błędach połączenia podczas potwierdzania transakcji.
- **Atrybutu indeksu** umożliwia indeksów określoną przez umieszczenie atrybutu na właściwości (lub właściwości) w modelu Code First. Kod najpierw następnie utworzy odpowiedni indeks w bazie danych.
- **Mapowanie publiczny interfejs API** zapewnia dostęp do informacji EF ma na sposobie mapowania właściwości i typy kolumn i tabel w bazie danych. W przeszłości wersje tego interfejsu API było wewnętrzny.
- **Możliwość konfigurowania interceptory za pomocą pliku App/Web.config**(dzięki czemu interceptory do dodania bez konieczności ponownego kompilowania aplikacji).
- **DatabaseLogger** jest nowy interceptora, który ułatwia wszystkie operacje bazy danych w pliku dziennika. W połączeniu z poprzedniej funkcji dzięki temu można łatwo przełączać rejestrowania operacji bazy danych dla wdrożonej aplikacji bez konieczności Skompiluj ponownie.
- **Wykrywanie zmian modelu migracje** udoskonalono, tak aby były szkieletu migracje dokładniejsze; wydajność procesu wykrywania zmian również zostały znacznie rozszerzone.
- **Ulepszenia wydajności** tym zmniejszenie bazy danych operacji podczas inicjowania, optymalizacji dla porównania równości wartości null w zapytaniach LINQ szybciej wyświetlić generacji (Tworzenie modelu) w scenariuszach więcej i bardziej wydajnych materialization śledzone jednostek z wielu skojarzeń.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Uwierzytelnianie dwuskładnikowe**: tożsamości ASP.NET obsługuje teraz uwierzytelnianie dwuskładnikowe. Uwierzytelnianie dwuskładnikowe zapewnia dodatkową warstwę zabezpieczeń do kont użytkowników w przypadku, gdy hasło pobiera naruszenia zabezpieczeń. Istnieje również ochronę ataków siłowych wobec dwuczynnikowego.
- **Blokada konta:** zapewnia możliwość zablokowania użytkownika, jeśli użytkownik wprowadza swoje hasła lub kody dwuskładnikowego niepoprawnie. Liczba nieudanych prób podania i zakres czasu dla użytkowników są zablokowane można skonfigurować. Projektant możliwość wyłączenia blokady konta dla niektórych kont użytkowników powinny one konieczne.
- **Potwierdzenie konta:** systemu tożsamości ASP.NET obsługuje teraz potwierdzenie konta. Jest to dość typowy scenariusz, w większości witryn sieci Web dzisiaj where, podczas rejestrowania nowe konto w witrynie internetowej, należy Potwierdź swój adres e-mail, zanim można wykonywać wszystkie w witrynie internetowej. Wiadomości e-mail z potwierdzeniem jest przydatne, ponieważ uniemożliwia sfałszowany kont tworzona. Jest to bardzo przydatne, jeśli używasz poczty e-mail jako metodę komunikacji z użytkownikami witryny sieci Web, takich jak lokacje Forum, banku, handlu elektronicznego i społecznościowych witryn sieci web.
- **Resetowanie hasła:** resetowania haseł to funkcja, której użytkownik może resetowanie haseł, jeśli ich pamiętasz swojego hasła.
- **Sygnatura bezpieczeństwa (Wyloguj się wszędzie):** obsługuje sposób można ponownie wygenerować tokenu zabezpieczeń dla użytkownika w przypadku, gdy użytkownik zmieni swoje hasło lub innych zabezpieczeń powiązane informacje, takich jak usuwanie skojarzonym logowaniem (takimi jak Facebook, Google, Konta Microsoft i tak dalej). Jest to niezbędne do zapewnienia, że wszystkie tokeny generowane ze starym hasłem są unieważnione. W przykładowy projekt Jeśli zmienisz hasło użytkownika następnie wygenerowaniu nowy token dla użytkownika i wszystkie poprzednie tokeny są unieważnione. Ta funkcja zapewnia dodatkową warstwę zabezpieczeń do aplikacji, ponieważ po zmianie hasła, będzie można wylogować z wszędzie (dla innych przeglądarek) gdy użytkownik zalogował się do tej aplikacji.
- **Typ klucza podstawowego można extensible dla użytkowników i ról**: W ASP.NET Identity 1.0 ciągów została typu klucza podstawowego tabeli użytkowników i ról. Oznacza to, gdy system ASP.NET Identity zostało utrwalone w programie SQL Server przy użyciu programu Entity Framework, możemy korzystano z nvarchar. Wystąpiły wiele dyskusji wokół tej domyślnej implementacji w witrynie Stack Overflow i na podstawie opinii przychodzących. Firma Microsoft umieściła haku rozszerzalności, której można określić, jakie powinny być klucza podstawowego tabeli użytkowników i ról. Tego punktu zaczepienia rozszerzalności jest szczególnie przydatne, jeśli jest przeprowadzana migracja aplikacji i aplikacja została przechowywania nazw użytkownika są identyfikatory GUID lub wskazówki.
- **Obsługuje interfejs IQueryable dla użytkowników i ról**: Dodano obsługę IQueryable UsersStore i RolesStore, możesz łatwo uzyskiwać listę użytkowników i ról.
- **Operacja usuwania pomocy technicznej za pośrednictwem interfejs UserManager**
- **Indeksowanie użytkownika**: implementacja ASP.NET Identity Entity Framework, dodano unikatowego indeksu o nazwę użytkownika przy użyciu nowego atrybutu indeksu w EF 6.1.0. Zapewnia to, czy elementy Usernames są unikatowe i nie było żadnych sytuacji wyścigu, w którym można na końcu zduplikowanych nazw użytkowników.
- **Ulepszone moduł sprawdzania poprawności hasła:** modułu sprawdzania poprawności hasła, dostarczonej w ASP.NET Identity 1.0 został był tylko sprawdzanie poprawności minimalną długość sprawdzania poprawności hasła dosyć prosta. Brak nowego modułu weryfikacji hasła, który zapewnia większą kontrolę nad złożoności hasła. Należy pamiętać, że nawet jeśli włączysz wszystkie ustawienia w to hasło, zachęcamy do włączenia uwierzytelniania dwuskładnikowego dla kont użytkowników.
- **Oprogramowanie pośredniczące IdentityFactory / CreatePerOwinContext**:

    - **Menedżer użytkowników**: wdrożenie fabryki umożliwia uzyskanie wystąpienia interfejs UserManager z kontekstu OWIN. Ten wzorzec jest podobny do używamy uzyskania elementu AuthenticationManager z kontekstu OWIN logowanie i wylogowanie. Jest to zalecany sposób uzyskiwania wystąpienia interfejs UserManager na żądanie dla aplikacji.
    - **DbContextFactory**: używa ASP.NET Identity Entity Framework utrwalanie systemu tożsamości w programie SQL Server. W tym celu systemu tożsamości zawiera odwołanie do ApplicationDbContext. Oprogramowanie pośredniczące DbContextFactory Zwraca wystąpienie klasy ApplicationDbContext na żądanie, które można użyć w aplikacji.
- **Pakiet NuGet przykłady tożsamości ASP.NET**: pakiet NuGet próbki mogą ułatwić zainstalować i uruchomić przykłady dla tożsamości ASP.NET i stosuj najlepsze rozwiązania. To jest przykładowy aplikacji ASP.NET MVC. Zmodyfikuj kod do własnych aplikacji przed wdrożeniem to w środowisku produkcyjnym. Próbki, należy zainstalować w pustej aplikacji ASP.NET. Aby uzyskać więcej informacji o pakiecie, przejdź do następujący wpis w blogu: [Announcing RTM programu ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Składniki Microsoft OWIN

Znaleziono wiele błędów, które zostały usunięte w tej wersji. Zobacz [informacje o wersji programu 2.1.0 wersji](https://katanaproject.codeplex.com/releases/view/113281) więcej szczegółowych informacji.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Znaleziono wiele błędów, które zostały usunięte w tej wersji. Zobacz [informacje o wersji programu pkt 2.0.2 wersji](https://github.com/SignalR/SignalR/releases/tag/2.0.2) więcej szczegółowych informacji.
