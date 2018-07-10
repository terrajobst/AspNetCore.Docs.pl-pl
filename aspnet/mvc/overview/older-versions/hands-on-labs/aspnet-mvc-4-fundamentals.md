---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 — podstawy | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym laboratorium praktyczne opiera się na MVC (Model View Controller) Music Store, samouczek aplikacji, która wprowadza i wyjaśnia instrukcje krok po kroku, jak używać ASP.NET MV...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 2b3f8916bdca1df0dd2855f02ae46f5e5d13311a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819974"
---
# <a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4 — podstawy

Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](https://aka.ms/webcamps-training-kit)

W tym laboratorium praktyczne opiera się na MVC (Model View Controller) Music Store, aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio. W całym laboratorium dowiesz się uproszczenia jeszcze mocy ze sobą przy użyciu tych technologii. Rozpoczyna się od prostej aplikacji i utworzy go, dopóki nie uzyskasz funkcjonalnej programu ASP.NET MVC 4 aplikacji sieci Web.

W tym laboratorium w programach ASP.NET MVC 4.

Jeśli chcesz zapoznać się z aplikacją z samouczka w wersji platformy ASP.NET MVC 3, można znaleźć w [MVC — muzyka-Store](https://github.com/evilDave/MVC-Music-Store).

W tym laboratorium praktyczne przyjęto założenie, że deweloper ma doświadczenie w technologii sieci Web, takich jak HTML i JavaScript.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzne dla tego laboratorium znajduje się w temacie [podstawowe informacje dotyczące programu ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Aplikacja Music Store

Music Store aplikacji sieci web, który zostanie skompilowany w całym tym środowisku laboratoryjnym składa się z trzech głównych części: zakupów, finalizacja zakupu i administracyjnej. Osoby odwiedzające będą mogli przeglądać albumów według gatunku, Dodaj albumów ich koszyka, przejrzyj elementy wybrane do ich i na koniec kontynuować wyewidencjonowanie kodu do logowania i wypełnij zamówienie. Ponadto administratorzy magazynu będą mogli zarządzać dostępne ze zdjęciami, a także ich właściwości głównego.

![Ekrany Music Store](aspnet-mvc-4-fundamentals/_static/image1.png "ekrany Music Store")

*Ekrany Music Store*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

Aplikacja Music Store zostanie utworzona przy użyciu **kontrolera MVC (Model View)**, wzorzec architektury, która dzieli aplikację na trzy główne składniki:

- **Modele**: obiekty modelu są częściami aplikacji, które implementują logikę domeny. Często obiekty modelu również pobrać i przechowywanie stanu modelu w bazie danych.
- **Widoki:** widoki są składnikami aplikacji interfejsu użytkownika (UI). Zazwyczaj interfejs ten jest tworzony w modelu danych. Przykładem może być widok edycji albumów, który wyświetla pola tekstowe i listy rozwijanej, w oparciu o bieżący stan obiektu albumu.
- **Kontrolery:** kontrolery są składnikami, które obsługują interakcję z użytkownikiem, manipulowania modelem i ostatecznie wybierają widok do renderowania interfejsu użytkownika. W aplikacji MVC widok zawiera tylko informacje; Kontroler obsługuje i reaguje na dane wejściowe użytkownika i interakcji.

Wzorzec MVC ułatwia tworzenie aplikacji, które rozdzielają różnych aspektów aplikacji (logika danych wejściowych, logika biznesowa i logika interfejsu użytkownika), zapewniając tym luźne powiązanie tych elementów. Ta separacja ułatwia zarządzanie złożonością podczas tworzenia aplikacji, ponieważ pozwala skupić się na jednym aspekcie implementacji w danym momencie. Ponadto wzorzec MVC ułatwia testowanie aplikacji, również zachęcanie do wykorzystania Programowanie oparte na testach (TDD) do tworzenia aplikacji.

**Platformy ASP.NET MVC** framework stanowi alternatywę dla wzorca ASP.NET Web Forms do tworzenia aplikacji sieci Web opartych na programie ASP.NET MVC. **Platformy ASP.NET MVC** framework to struktura prezentacji niewielka, wysoce testowalna która (tak jak aplikacje oparte na formularzach sieci Web) jest zintegrowana z istniejącymi funkcjami platformy ASP.NET, takimi jak strony wzorcowe i oparte na członkostwie uwierzytelnienia, pozwalając korzystać ze wszystkich możliwości programu .NET core. Jest to przydatne, jeśli znasz już przy użyciu wzorca ASP.NET Web Forms, ponieważ wszystkie biblioteki, które już używają są również dostępne na platformie ASP.NET MVC 4.

Ponadto luźne powiązanie między trzema głównymi składnikami aplikacji MVC wspiera także Programowanie równoległe. Na przykład jeden deweloper może pracować w widoku, drugi może pracować nad logiką kontrolera, a trzeci skupić się na logice biznesowej w modelu.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktyczne dowiesz się jak:

- Tworzenie aplikacji ASP.NET MVC od podstaw, bazując na samouczek Music Store aplikacji
- Dodaj kontrolery do obsługi adresów URL do strony głównej witryny i przeglądania jego główne funkcje
- Dodaj widok, aby dostosować zawartość wyświetlana wraz z jego styl
- Dodawanie klasy modelu zawierają i zarządzanie nimi danych i domeny logiki
- Użyj wzorca Model widoku do przekazywania informacji z akcji kontrolera do widoku szablonów
- Zapoznaj się z nowego szablonu aplikacji internetowych platformy ASP.NET MVC 4

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Należy dysponować następującymi elementami do przygotowania tego laboratorium:

- [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (odczyt [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.

Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek C: za pomocą wstawek kodu programu](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

W tym laboratorium praktyczne składa się przez następujących czynnościach:

1. [Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC MusicStore](#Exercise1)
2. [Ćwiczenie 2: Tworzenie kontrolera](#Exercise2)
3. [Ćwiczenie 3: Przekazanie parametrów do kontrolera](#Exercise3)
4. [Ćwiczenie 4: Tworzenie widoku](#Exercise4)
5. [Ćwiczenie 5: Tworzenie modelu widoku](#Exercise5)
6. [Ćwiczenie 6: W widoku przy użyciu parametrów](#Exercise6)
7. [Ćwiczenie 7: Omówienie platformy ASP.NET MVC 4 nowy szablon](#Exercise7)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia. Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.


Szacowany czas do ukończenia tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC MusicStore

W tym ćwiczeniu dowiesz się, jak utworzyć aplikację ASP.NET MVC w programie Visual Studio 2012 Express dla sieci Web, a także swojej organizacji w głównym folderze. Ponadto nauczysz Dodaj nowy kontroler i prostego ciągu wyświetlanego w strony głównej aplikacji.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Zadanie 1. Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC

1. W tym zadaniu utworzysz pusty projekt aplikacji platformy ASP.NET MVC przy użyciu szablonu MVC programu Visual Studio. Rozpocznij **VS Express for Web**.
2. Na **pliku** menu, kliknij przycisk **nowy projekt**.
3. W **nowy projekt** wybierz okno dialogowe **aplikacji sieci Web programu ASP.NET MVC 4** typ, znajdujący się w folderze projektu **Visual C#,** **Web** szablonu Lista.
4. Zmiana **nazwa** do *MvcMusicStore*.
5. Ustaw lokalizację rozwiązania wewnątrz nowego **rozpocząć** folder, w tym ćwiczeniu folder źródłowy, na przykład **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-dzień HOL-PATH]**. Kliknij przycisk **OK**.

    ![Utwórz okno dialogowe Nowy projekt](aspnet-mvc-4-fundamentals/_static/image2.png "utworzyć okno dialogowe Nowy projekt")

    *Utwórz okno dialogowe Nowy projekt*
6. W **nowego projektu programu ASP.NET MVC 4** wybierz okno dialogowe **podstawowe** szablonu i upewnij się, że **aparat widoku** wybrana **Razor**. Kliknij przycisk **OK**.

    ![Okno dialogowe Nowy projekt ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "okno dialogowe Nowy projekt ASP.NET MVC 4")

    *Okno dialogowe Nowy projekt ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Zadanie 2 — poznawanie struktury rozwiązania

Platforma ASP.NET MVC zawiera szablon projektu Visual Studio, która pomaga tworzyć aplikacje sieci Web obsługujące wzorzec MVC. Ten szablon tworzy nową aplikację sieci Web platformy ASP.NET MVC z wymagane foldery, szablonów elementów i we wpisach w plikach konfiguracji.

W tym zadaniu zbada struktury rozwiązania, aby zrozumieć elementy, które są zaangażowane i ich wzajemne relacje. Następujące foldery znajdują się w aplikacji ASP.NET MVC, ponieważ platforma ASP.NET MVC, która domyślnie używa &quot;Konwencji nad konfiguracją&quot; podejście i sprawia, że niektóre założenia domyślne oparte na nazwy folderu konwencje.

1. Po utworzeniu projektu, Sprawdź strukturę folderów, która została utworzona w Eksploratorze rozwiązań po prawej stronie:

    ![Struktura ASP.NET MVC Folder w Eksploratorze rozwiązań](aspnet-mvc-4-fundamentals/_static/image4.png "struktury ASP.NET MVC Folder w Eksploratorze rozwiązań")

    *Struktura ASP.NET MVC Folder w Eksploratorze rozwiązań*

   1. **Kontrolery**. Ten folder będzie zawierać klasy kontrolera. W aplikacji MVC, na podstawie kontrolery są odpowiedzialne za obsługi interakcji użytkownika końcowego, manipulowanie modelem i ostatecznie wybierając widok do renderowania interfejsu użytkownika.

       > [!NOTE]
       > Platforma MVC wymaga nazwy wszystkich kontrolerów kończącej się znakiem &quot;kontrolera&quot;— na przykład HomeController, LoginController lub ProductController.
   2. **Modele**. Ten folder jest udostępniana dla klas, które reprezentują model aplikacji dla aplikacji sieci Web MVC. Obejmuje to zwykle kod, który definiuje obiekty i logikę do interakcji z magazynem danych. Zazwyczaj obiekty modelu rzeczywiste będzie w osobnej klasy biblioteki. Jednak podczas tworzenia nowej aplikacji może zawierać klasy i następnie przenieść do osobnej klasy biblioteki w dowolnym momencie w cyklu rozwoju.
   3. **Widoki**. Ten folder jest zalecana lokalizacja widoków, składniki odpowiada za wyświetlanie interfejsu użytkownika aplikacji. Widoki używają plików .aspx, ascx, cshtml i .master, oprócz innych plików, które są powiązane z widokami renderowania. Widoki zawiera folder dla każdego kontrolera; folder nosi nazwę z prefiksem nazwy kontrolera. Na przykład, jeśli masz kontroler o nazwie **HomeController**, folder widoków zawiera folder o nazwie Home. Domyślnie, gdy platforma ASP.NET MVC ładuje widoku, szuka pliku .aspx o nazwie żądany widok w folderze Views\controllerName (**widoków [ControllerName] [Action] .aspx**) lub (**widoków [ControllerName] [Akcja] .cshtml**) dla widokami Razor.

      > [!NOTE]
      > Oprócz folderów wymienionych powyżej, korzysta z aplikacji sieci Web MVC **Global.asax** plik, aby ustawić globalny routingu adresów URL, przy czym **Web.config** pliku, aby skonfigurować aplikację.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Zadanie 3 — Dodawanie HomeController

W aplikacjach ASP.NET, które nie korzystają z struktura MVC interakcji z użytkownikiem jest zorganizowane wokół stron oraz wokół podnoszenia i obsługa zdarzeń z tych stron. Z kolei interakcji użytkownika z aplikacji ASP.NET MVC są zorganizowane wokół kontrolerów i ich metod akcji.

Z drugiej strony platforma ASP.NET MVC mapuje adresy URL do klas, które są określane jako kontrolerów. Kontrolery przetworzyć żądań przychodzących, Obsługa danych wejściowych użytkownika i interakcje, wykonywanie logiki aplikacji odpowiednie i określić odpowiedzi do odesłania do klienta (wyświetlania kodu HTML, Pobierz plik, przekierowanie na inny adres URL itp.). W przypadku wyświetlania kodu HTML, klasę kontrolera zazwyczaj wywołuje składnik osobny widok, aby wygenerować kod znaczników HTML dla żądania. W aplikacji MVC widok zawiera tylko informacje; Kontroler obsługuje i reaguje na dane wejściowe użytkownika i interakcji.

W tym zadaniu doda Klasa kontrolera, który będzie obsługiwać adresy URL do strony głównej witryny Music Store.

1. Kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań, wybierz pozycję **Dodaj** i następnie **kontrolera** polecenia:

    ![Dodaj polecenie kontrolera](aspnet-mvc-4-fundamentals/_static/image5.png "Dodaj polecenie kontrolera")

    *Dodaj kontroler — polecenie*
2. **Dodaj kontroler** zostanie wyświetlone okno dialogowe. Nazwa kontrolera *HomeController* i naciśnij klawisz **Dodaj**.

    ![Dodaj okno dialogowe kontrolera](aspnet-mvc-4-fundamentals/_static/image6.png "Dodaj kontroler, okno dialogowe")

    *Dodaj kontroler, okno dialogowe*
3. Plik **HomeController.cs** jest tworzony w **kontrolerów** folderu. Aby mogła mieć **HomeController** zwrócenia ciągu w przypadku jego akcji indeksu, Zastąp **indeksu** metoda następującym kodem:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 — indeks HomeController Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

W tym zadaniu zostanie Wypróbuj aplikację w przeglądarce sieci web.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji. Projekt jest skompilowany i uruchamia serwer sieci Web IIS lokalnej. Lokalny serwer sieci Web usług IIS automatycznie otworzy przeglądarkę internetową i wpisz adres URL serwera sieci Web.

    ![Aplikacja działająca w przeglądarce sieci web](aspnet-mvc-4-fundamentals/_static/image7.png "aplikacji działającej w przeglądarce sieci web")

    *Aplikacja działająca w przeglądarce sieci web*

    > [!NOTE]
    > Lokalny serwer internetowy IIS będą uruchamiane witryny sieci Web na liczby losowe wolnego portu. Na powyższej ilustracji lokacji działa w `http://localhost:50103/`, więc używa portu 50103. Numer portu, mogą się różnić.
2. Zamknij przeglądarkę.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Ćwiczenie 2: Tworzenie kontrolera

W tym ćwiczeniu dowiesz się, jak zaktualizować kontroler do implementowania prostego Music Store aplikacji. Ten kontroler będą definiować metody akcji, umożliwiający obsługę każdego z następujących określone żądania:

- Strona listy gatunki muzyki w Store utworów muzycznych
- Strony przeglądania, który wymienia wszystkie albumów muzycznych dla określonego rodzaju
- Strona zawierająca szczegóły informacji na temat albumu określonych utworów muzycznych

Dla zakresu tego ćwiczenia te akcje po prostu zwrócenia ciągu przeprowadzona.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Zadanie 1 — Dodawanie nowej klasy StoreController

To zadanie dodasz nowy kontroler.

1. Jeśli nie już jest otwarty, uruchom **VS Express for Web 2012**.
2. W **pliku** menu, wybierz **Otwórz projekt**. W oknie dialogowym otwierania projektu, przejdź do **Source\Ex02 CreatingAController\Begin**, wybierz opcję **Begin.sln** i kliknij przycisk **Otwórz**. Alternatywnie możesz nadal z rozwiązaniem uzyskany po ukończeniu poprzedniego ćwiczenia.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
3. Dodaj nowy kontroler. Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań, wybierz pozycję **Dodaj** i następnie **kontrolera** polecenia. Zmiana **nazwy kontrolera** do *StoreController*i kliknij przycisk **Dodaj**.

    ![Dodaj okno dialogowe kontrolera](aspnet-mvc-4-fundamentals/_static/image8.png "Dodaj kontroler, okno dialogowe")

    *Dodaj kontroler, okno dialogowe*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Zadanie 2 — modyfikowanie StoreController akcji

W ramach tego zadania zmodyfikujesz metodach kontrolera które są nazywane **akcje**. Akcje są odpowiedzialne za obsługę żądań z adresu URL i określania zawartości, która ma zostać odesłana do przeglądarki lub użytkownik, który wywołał adres URL.

1. **StoreController** klasa ma już **indeksu** metody. Użyjesz go w dalszej części tego laboratorium do zaimplementowania Strona która zawiera listę wszystkich gatunki music store —. Teraz, po prostu zastąpić **indeksu** metoda następującym kodem, które zwraca ciąg &quot;pozdrowienia z Store.Index()&quot;:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 — indeks StoreController Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Dodaj **Przeglądaj** i **szczegóły** metody. Aby to zrobić, Dodaj następujący kod do **StoreController**:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3. uruchamianie aplikacji

W tym zadaniu zostanie Wypróbuj aplikację w przeglądarce sieci web.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Projekt jest uruchamiany w **Home** strony. Zmień adres URL, aby sprawdzić wykonania każdej akcji.

    1. **/ Store**. Zostanie wyświetlony  **&quot;pozdrowienia ze Store.Index()&quot;**.
    2. **/ Store/przeglądania**. Zostanie wyświetlony  **&quot;pozdrowienia ze Store.Browse()&quot;**.
    3. **/ Store/Details**. Zostanie wyświetlony  **&quot;pozdrowienia ze Store.Details()&quot;**.

        ![Przeglądanie StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "przeglądania StoreBrowse")

        *Przeglądanie /Store/Browse*
3. Zamknij przeglądarkę.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Ćwiczenie 3: Przekazanie parametrów do kontrolera

Do tej pory ma został zwracania stałych ciągów z kontrolerów. W tym ćwiczeniu dowiesz się, jak przekazać parametry do kontrolera przy użyciu adresu URL i ciąg zapytania, a następnie sprawdzając akcji metoda reagować z tekstem w przeglądarce.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Zadanie 1 — Dodawanie parametru gatunku StoreController

W tym zadaniu zostanie użyty **querystring** parametry, aby wysyłać **Przeglądaj** metody akcji w **StoreController**.

1. Jeśli nie już jest otwarty, uruchom **VS Express for Web**.
2. W **pliku** menu, wybierz **Otwórz projekt**. W oknie dialogowym otwierania projektu, przejdź do **Source\Ex03 PassingParametersToAController\Begin**, wybierz opcję **Begin.sln** i kliknij przycisk **Otwórz**. Alternatywnie możesz nadal z rozwiązaniem uzyskany po ukończeniu poprzedniego ćwiczenia.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
3. Otwórz **StoreController** klasy. Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie plik **StoreController.cs**.
4. Zmiana **Przeglądaj** metody, dodając parametr ciągu, aby poprosić o dodanie do określonego rodzaju. ASP.NET MVC przekazać żadnych querystring automatycznie lub nazwane parametry post formularza **gatunku** się tą metodą akcji po wywołaniu. Aby to zrobić, Zastąp **Przeglądaj** metoda następującym kodem:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Używasz **HttpUtility.HtmlEncode** metody narzędzie uniemożliwia użytkownikom wprowadzanie Javascript do widoku z linkiem, takich jak   **/Store/przeglądania? Gatunku =&lt;skryptu&gt;window.location= "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.
> 
> Aby uzyskać dokładniejsze objaśnienie, odwiedź [w tym artykule msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — uruchamianie aplikacji

W tym zadaniu zostanie wypróbuj działanie aplikacji w przeglądarce sieci web i używać **gatunku** parametru.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Projekt jest uruchamiany w **Home** strony. Zmień adres URL do   */Store/Przeglądaj? Gatunku = Najdywania* Aby sprawdzić, czy akcja odbiera parametr gatunku.

    ![Przeglądanie StoreBrowseGenre = Najdywania](aspnet-mvc-4-fundamentals/_static/image10.png "przeglądania StoreBrowseGenre = Najdywania")

    *Przeglądanie /Store/Browse? Gatunku = Najdywania*
3. Zamknij przeglądarkę.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Zadanie 3. Dodawanie parametru Id osadzone w adresie URL

W tym zadaniu zostanie użyty **adresu URL** do przekazania **identyfikator** parametr **szczegóły** metody akcji **StoreController**. ASP.NET MVC domyślnej konwencji routingu jest segment adresu URL po nazwie metody akcji jako parametr o nazwie **identyfikator**. Jeśli metoda akcji zawiera parametr o nazwie identyfikator, następnie platformy ASP.NET MVC zostanie automatycznie przekazać segment adresu URL dla Ciebie jako parametr. W adresie URL **Store/szczegóły/5**, **identyfikator** będzie interpretowany jako **5**.

1. Zmiana **szczegóły** metody **StoreController**, dodając **int** parametr o nazwie **identyfikator**. Aby to zrobić, Zastąp **szczegóły** metoda następującym kodem:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

W tym zadaniu zostanie wypróbuj działanie aplikacji w przeglądarce sieci web i używać **identyfikator** parametru.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Projekt jest uruchamiany w **Home** strony. Zmień adres URL do */Store/Details/5* Aby sprawdzić, czy akcja odbiera parametru identyfikatora.

    ![Przeglądanie StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "przeglądania StoreDetails5")

    *Przeglądanie /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Ćwiczenie 4: Tworzenie widoku

Do tej pory ma zostały zwracania ciągów z akcji kontrolera. Mimo że jest to wygodny sposób zrozumienie, jak działają kontrolery, jest to, jak rzeczywistych aplikacji sieci Web są kompilowane. Widoki są składnikami, które zapewniają lepszym rozwiązaniem do generowania kodu HTML do przeglądarki przy użyciu plików szablonów.

W tym ćwiczeniu dowiesz się, jak dodać stronę wzorcową układu, aby skonfigurować szablon dla typowych zawartość HTML, arkusz stylów, aby poprawić wygląd i działanie witryny, a na koniec Wyświetl szablon, aby umożliwić HomeController do zwrócenia HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Zadanie 1 — modyfikowanie pliku \_layout.cshtml

Plik **~/Views/Shared/\_layout.cshtml** służy do konfiguracji szablonu dla wspólnego kodu HTML do użycia w całej witryny sieci Web. W tym zadaniu dodasz stroną wzorcową układu z nagłówkiem wspólnej wraz z łączami do obszar strony głównej i Store.

1. Jeśli nie już jest otwarty, uruchom **VS Express for Web**.
2. W **pliku** menu, wybierz **Otwórz projekt**. W oknie dialogowym otwierania projektu, przejdź do **Source\Ex04 CreatingAView\Begin**, wybierz opcję **Begin.sln** i kliknij przycisk **Otwórz**. Alternatywnie możesz nadal z rozwiązaniem uzyskany po ukończeniu poprzedniego ćwiczenia.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
3. Plik  <strong>\_layout.cshtml</strong> zawiera układ kontenera HTML na wszystkich stronach w witrynie. Zawiera on <strong>&lt;html&gt;</strong> element dla odpowiedzi HTML, jak również <strong>&lt;head&gt;</strong> i <strong>&lt;treści&gt;</strong> elementów. <strong>@RenderBody()</strong> w kodzie HTML treści regiony rozpoznać widoku szablonów będzie można wypełnić przy użyciu zawartości dynamicznej.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Dodaj nagłówek wspólnej wraz z łączami do strony głównej i Store obszaru na wszystkich stronach w witrynie. Aby to zrobić, Dodaj następujący kod poniżej &lt;treści&gt; instrukcji.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Obejmują div renderować tę sekcję treści każdej strony. Zastąp  <strong>@RenderBody()</strong> następującym kodem higlighted: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Czy wiesz? Program Visual Studio 2012 zawiera fragmenty kodu, które ułatwiają Dodaj kod często używane w formacie HTML, pliki kodu i innych! Wypróbuj to wpisując **&lt;div&gt;** i naciskając klawisz **kartę** dwa razy, aby wstawić kompletna **div** tagu.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Zadanie 2 — Dodawanie arkusza stylów CSS

Szablonu pusty projekt zawiera bardzo usprawnione pliku CSS, zawierającą tylko style używane do wyświetlania podstawowych formularzy i komunikatów dotyczących sprawdzania poprawności. Aby poprawić wygląd i działanie witryny użyje dodatkowe CSS i obrazów (potencjalnie udostępnione przez projektanta).

W tym zadaniu należy dodać arkusz stylów CSS do definiowania stylów witryny.

1. Plik CSS i obrazów, które są uwzględnione w **Source\Assets\Content** folder w tym laboratorium. Aby dodać je do aplikacji, przeciągnij ich zawartość z **Eksplorator Windows** okna do **Eksploratora rozwiązań** w programie Visual Studio Express for Web, jak pokazano poniżej:

    ![Przeciąganie zawartość stylu](aspnet-mvc-4-fundamentals/_static/image12.png "przeciąganie zawartość stylu")

    *Przeciąganie zawartość stylu*
2. Ostrzeżenie zostanie wyświetlone okno dialogowe, pytaniem o potwierdzenie zamiaru zastąpienia **Site.css** plików i niektórych istniejących obrazów. Sprawdź **Zastosuj do wszystkich elementów** i kliknij przycisk **tak**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Zadanie 3 — Dodawanie szablonu widoku

W tym zadaniu doda Wyświetl szablon do generowania odpowiedzi HTML, który będzie używał układ strony wzorcowej i CSS dodane w tym ćwiczeniu.

1. Aby użyć szablonu widoku podczas przeglądania strony głównej, najpierw należy wskazać, że zamiast zwracać ciąg, **indeksu HomeController** metoda zwróci **widoku**. Otwórz **HomeController** klasy i zmień jego **indeksu** metodę, aby zwrócić **ActionResult**, i zwraca **View()**.

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 — indeks HomeController Ex4*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Teraz należy dodać odpowiedni szablon widoku. Aby to zrobić, **kliknij prawym przyciskiem myszy** wewnątrz **indeksu** metody akcji i wybierz **Dodaj widok**. Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.

    ![Dodawanie widoku z wewnątrz metody indeksu](aspnet-mvc-4-fundamentals/_static/image13.png "Dodawanie widoku z wewnątrz metody indeksu")

    *Dodawanie widoku z wewnątrz metody indeksu*
3. **Dodaj widok** zostanie wyświetlone okno dialogowe, aby wygenerować plik szablonu widoku. Domyślnie to okno dialogowe wstępnie wypełnia nazwę szablonu widoku, aby był zgodny z metody akcji, która będzie go używać. Ponieważ użyto **Dodaj widok** menu kontekstowe w obrębie **indeksu** metody akcji w obrębie HomeController **Dodaj widok** okno dialogowe ma indeks jako domyślnej nazwy widoku. Kliknij przycisk **Dodaj**.

    ![Okno dialogowe dodawania widoku](aspnet-mvc-4-fundamentals/_static/image14.png "okno dialogowe dodawania widoku")

    *Okno dialogowe dodawania widoku*
4. Program Visual Studio generuje **Index.cshtml** Wyświetl szablon wewnątrz **Views\Home** folder i następnie otwiera go.

    ![Strona główna widok indeksu utworzony](aspnet-mvc-4-fundamentals/_static/image15.png "widok Home Indeks utworzony")

    *Utworzony widok indeks strony głównej*

    > [!NOTE]
    > Nazwa i lokalizacja **Index.cshtml** plików ma zastosowanie i jest zgodna z konwencjami nazewnictwa platformy ASP.NET MVC domyślne.
    > 
    > \Views folderu\**Home** jest zgodna z nazwą kontrolera (**Home** kontrolera). Nazwa szablonu widoku (**indeksu**), pasuje do metody akcji kontrolera, które będą wyświetlane w widoku.
    > 
    > W ten sposób platformy ASP.NET MVC pozwala uniknąć konieczności jawnego określania nazwy lub lokalizacji szablonu widoku, używając tę konwencję nazewnictwa do zwrócenia widoku.
5. Wygenerowany szablon widoku opiera się na  **\_layout.cshtml** wcześniej zdefiniowanego szablonu. Aktualizowanie właściwości ViewBag.Title **Home**i zmień głównej zawartości do **jest stroną główną**, jak pokazano w poniższym kodzie:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Wybierz **MvcMusicStore** projekt w Eksploratorze rozwiązań, a następnie naciśnij klawisz **F5** do uruchomienia aplikacji.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Zadanie 4: Weryfikacja

Aby sprawdzić, czy poprawnie wykonano wszystkie kroki opisane w poprzednim ćwiczeniu kontynuować:

Z aplikacją otwierane w przeglądarce należy zauważyć, że:

1. Metody akcji indeksu HomeController znalezione i wyświetlane **\Views\Home\Index.cshtml** wyświetlić szablon, mimo że kod wywołuje **zwracają View()**, ponieważ szablon widoku, a następnie standardowej konwencji nazewnictwa.
2. Na stronie głównej wyświetli komunikat powitalny, zdefiniowany w ramach **\Views\Home\Index.cshtml** szablon widoku.
3. Strona główna używa  **\_layout.cshtml** szablonu, a zatem komunikat powitalny znajduje się w układzie standardowej witrynie HTML.

    ![Strona główna widoku indeksu przy użyciu zdefiniowanych LayoutPage i styl](aspnet-mvc-4-fundamentals/_static/image16.png "Home widoku indeksu przy użyciu zdefiniowanych LayoutPage i style")

    *Widok strony głównej indeksu przy użyciu zdefiniowanych LayoutPage i style*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Ćwiczenie 5: Tworzenie modelu widoku

Do tej pory wprowadzono widoków wyświetlić zapisane na stałe HTML, ale aby można było utworzyć dynamicznych aplikacji sieci web, Wyświetl szablon powinien zostać wyświetlony informacji od kontrolera. Jedną z typowych technik posłuży do tego celu jest **ViewModel** wzorzec, co pozwala kontroler spakowanie wszystkie informacje potrzebne do generowania odpowiednich odpowiedzi HTML.

W tym ćwiczeniu zostanie dowiesz się, jak utworzyć klasę ViewModel i dodać wymagane właściwości: liczba gatunki w magazynie oraz lista tych gatunki. Zostanie również zaktualizować StoreController do użycia utworzonej ViewModel, a na koniec utworzysz nowy szablon widoku, który spowoduje wyświetlenie właściwości wymienionego na stronie.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Zadanie 1. Tworzenie klas ViewModel

W tym zadaniu utworzysz klas ViewModel, który będzie implementować scenariusza listy gatunku Store.

1. Jeśli nie już jest otwarty, uruchom **VS Express for Web**.
2. W **pliku** menu, wybierz **Otwórz projekt**. W oknie dialogowym otwierania projektu, przejdź do **Source\Ex05 CreatingAViewModel\Begin**, wybierz opcję **Begin.sln** i kliknij przycisk **Otwórz**. Alternatywnie możesz nadal z rozwiązaniem uzyskany po ukończeniu poprzedniego ćwiczenia.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
3. Tworzenie **modele widoków** folder przechowujący ViewModel. Aby to zrobić, kliknij prawym przyciskiem myszy najwyższego poziomu **MvcMusicStore** projektu, wybierz opcję **Dodaj** i następnie **nowy Folder**.

    ![Dodawanie nowego folderu](aspnet-mvc-4-fundamentals/_static/image17.png "dodanie nowego folderu")

    *Dodawanie nowego folderu*
4. Nazwa folderu *modele widoków*.

    ![Modele widoków folder w Eksploratorze rozwiązań](aspnet-mvc-4-fundamentals/_static/image18.png "modele widoków folder w Eksploratorze rozwiązań")

    *Modele widoków folder w Eksploratorze rozwiązań*
5. Tworzenie **ViewModel** klasy. Aby to zrobić, kliknij prawym przyciskiem myszy **modele widoków** niedawno utworzony folder wybierz **Dodaj** i następnie **nowy element**. W obszarze **kodu**, wybierz **klasy** elementu i nazwij plik *StoreIndexViewModel.cs*, następnie kliknij przycisk **Dodaj**.

    ![Dodawanie nowej klasy](aspnet-mvc-4-fundamentals/_static/image19.png "Dodawanie nowej klasy")

    *Dodawanie nowej klasy*

    ![Tworzenie klasy StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel Tworzenie klasy")

    *Tworzenie klasy StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Zadanie 2 — Dodawanie właściwości do klas ViewModel

Istnieją dwa parametry do przekazania z StoreController do szablonu widoku w celu wygenerowania oczekiwanego odpowiedzi HTML: liczba gatunki w magazynie oraz lista tych gatunki.

W ramach tego zadania spowoduje dodanie tych 2 właściwości w celu **StoreIndexViewModel** klasy: **NumberOfGenres** (liczba całkowita) i **gatunki** (lista ciągów znaków).

1. Dodaj **NumberOfGenres** i **gatunki** właściwości **StoreIndexViewModel** klasy. Aby to zrobić, Dodaj następujące wiersze 2 do definicji klasy:

    (Code Snippet — *platformy ASP.NET MVC 4 podstawowe - właściwości Ex5 StoreIndexViewModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> **{Pobierz; Ustaw;}**  notacji sprawia, że korzystanie z języka C# w funkcji właściwości zaimplementowane automatycznie. Zapewnia korzyści wynikające z właściwością bez konieczności NAS zadeklarować pole zapasowe.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Zadanie 3 — aktualizowanie StoreController używać StoreIndexViewModel

**StoreIndexViewModel** klasa hermetyzuje informacje wymagane do przekazywania z **StoreController**firmy **indeksu** metody Wyświetl szablon w celu wygenerowania odpowiedzi .

To zadanie zaktualizuje **StoreController** używać **StoreIndexViewModel**.

1. Otwórz **StoreController** klasy.

    ![Otwieranie klasy StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController otwierający, klasa")

    *Otwieranie StoreController klasy*
2. Aby można było używać **StoreIndexViewModel** klasy z **StoreController**, Dodaj następujące przestrzeni nazw w górnej części **StoreController** kodu:

    (Code Snippet — *platformy ASP.NET MVC 4 podstawowe - StoreIndexViewModel Ex5 przy użyciu modele widoków*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Zmiana **StoreController**firmy **indeksu** metody akcji tak, że utworzenie i wypełnienie **StoreIndexViewModel** obiektu i przekazuje je do szablonu widoku w celu Generowanie odpowiedzi HTML z nim.

    > [!NOTE]
    > W laboratorium &quot;platformy ASP.NET MVC modele i dostęp do danych&quot; trzeba napisać kod, który umożliwia pobranie listy gatunki magazynu z bazy danych. W poniższym kodzie zostanie utworzony **listy** gatunków fikcyjnymi danymi, które zostanie wypełniony **StoreIndexViewModel**.
    > 
    > Po utworzeniu i konfigurowania **StoreIndexViewModel** obiektu zostanie przekazany jako argument do **widoku** metody. Oznacza to, że szablon widoku będzie używać tego obiektu do generowania odpowiedzi HTML z nim.
4. Zastąp **indeksu** metoda następującym kodem:

    (Code Snippet — *platformy ASP.NET MVC 4 podstawowe - metoda indeksu StoreController Ex5*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Jeśli jesteś zaznajomiony z C#, mogą zakładać, że to przy użyciu **var** oznacza, że **viewModel** zmienna jest z późnym wiązaniem. Nie jest prawidłowe — kompilator języka C# używa wnioskowanie typów w oparciu o przypisania do zmiennej do określenia, który **viewModel** typu **StoreIndexViewModel**. Ponadto, kompilując lokalnej **viewModel** zmiennej jako **StoreIndexViewModel** typ sprawdzania kompilacji get i obsługa Edytor kodu programu Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Zadanie 4 — tworzenie Wyświetl szablon, który używa StoreIndexViewModel

W tym zadaniu utworzysz Wyświetl szablon, który będzie używany obiekt StoreIndexViewModel przekazywane z kontrolera do wyświetlania listy gatunki.

1. Przed utworzeniem nowego szablonu widoku, utworzymy projekt tak, aby **dialogowe dodawania widoku** obsługującemu **StoreIndexViewModel** klasy. Skompiluj projekt, wybierając **kompilacji** element menu i następnie **kompilacji MvcMusicStore**.

    ![Tworzenie projektu](aspnet-mvc-4-fundamentals/_static/image22.png "kompilowania projektu")

    *Tworzenie projektu*
2. Utwórz nowy szablon widoku. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz **indeksu** i wybierz opcję **Dodaj widok**.

    ![Dodawanie widoku](aspnet-mvc-4-fundamentals/_static/image23.png "Dodawanie widoku")

    *Dodawanie widoku*
3. Ponieważ **dialogowe dodawania widoku** została wywołana z **StoreController**, spowoduje to dodanie Wyświetl szablon domyślny **\Views\Store\Index.cshtml** pliku. Sprawdź **tworzenia silnie typizowane — widoku** pole wyboru, a następnie wybierz **StoreIndexViewModel** jako **klasa modelu**. Ponadto upewnij się, że aparat widoku, wybrane jest **Razor**. Kliknij przycisk **Dodaj**.

    ![Okno dialogowe dodawania widoku](aspnet-mvc-4-fundamentals/_static/image24.png "okno dialogowe dodawania widoku")

    *Okno dialogowe dodawania widoku*

    **\Views\Store\Index.cshtml** plik szablonu widok zostanie utworzony i otwarty. Oparte na informacjach dostarczonych do **Dodaj widok** okna dialogowego w ostatnim kroku, Wyświetl szablon będzie oczekiwać **StoreIndexViewModel** wystąpienia jako dane w celu wygenerowania odpowiedzi HTML. Zauważysz, że szablon dziedziczy `ViewPage<musicstore.viewmodels.storeindexviewmodel>` w języku C#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Zadanie 5 aktualizowanie szablonu widoku.

W tym zadaniu zostanie zaktualizowana Wyświetl szablon utworzony w poprzednim zadaniu można pobrać liczby gatunki i ich nazwy na stronie.

> [!NOTE]
> Użyjesz @ składni (często nazywany &quot;kodu nuggets&quot;) do wykonywania kodu w ramach szablonu widoku.

1. W **Index.cshtml** plików w ramach **Store** folderu, Zastąp kod następującym kodem:

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Pętlę gatunku liście **StoreIndexViewModel** i tworzenia kodu HTML **&lt;ul&gt;** listy przy użyciu **foreach** pętli.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Naciśnij klawisz **F5** do uruchamiania aplikacji i Przeglądaj **/Store**. Zobaczysz listę gatunki przekazanej **StoreIndexViewModel** obiektu z **StoreController** do szablonu widoku.

    ![Wyświetl listę gatunki](aspnet-mvc-4-fundamentals/_static/image26.png "Wyświetl listę gatunki")

    *Wyświetl listę gatunki*
4. Zamknij przeglądarkę.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Ćwiczenie 6: W widoku przy użyciu parametrów

Ćwiczenie 3 przedstawiono sposób przekazania parametrów do kontrolera. W tym ćwiczeniu dowiesz się, jak użyć tych parametrów w szablonie widoku. W tym celu użytkownik zostanie wprowadzona najpierw klasy modelu, ułatwiające zarządzanie logika danych i domeny. Ponadto będzie dowiesz się, jak utworzyć łącza do stron w aplikacji ASP.NET MVC nie martwiąc się z elementów, takich jak ścieżki URL kodowania.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Zadanie 1 — Dodawanie klasy modelu

W odróżnieniu od modele widoków, które są tworzone tylko w celu przekazywania informacji z kontrolera do widoku, klasy modelu są zbudowane zawierają i zarządzanie nimi danych i domeny logiki. W tym zadaniu należy dodać dwie klasy modelu do reprezentowania tych pojęć: **gatunku** i **albumu**.

1. Jeśli nie już jest otwarty, uruchom **VS Express for Web**
2. W **pliku** menu, wybierz **Otwórz projekt**. W oknie dialogowym otwierania projektu, przejdź do **Source\Ex06 UsingParametersInView\Begin**, wybierz opcję **Begin.sln** i kliknij przycisk **Otwórz**. Alternatywnie możesz nadal z rozwiązaniem uzyskany po ukończeniu poprzedniego ćwiczenia.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
3. Dodaj **gatunku** klasa modelu. Aby to zrobić, kliknij prawym przyciskiem myszy **modeli** folderu w **Eksploratora rozwiązań**, wybierz opcję **Dodaj** i następnie **nowy element** opcji. W obszarze **kodu**, wybierz **klasy** elementu i nazwij plik *Genre.cs*, następnie kliknij przycisk **Dodaj**.

    ![Dodawanie klasy](aspnet-mvc-4-fundamentals/_static/image27.png "Dodawanie klasy")

    *Dodawanie nowego elementu*

    ![Dodawanie klasy modelu gatunku](aspnet-mvc-4-fundamentals/_static/image28.png "Dodaj klasę modelu gatunku")

    *Dodawanie klasy modelu gatunku*
4. Dodaj **nazwa** właściwości do klasy gatunku. Aby to zrobić, Dodaj następujący kod:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 — gatunku Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Po tej samej procedury, Dodaj **albumu** klasy. Aby to zrobić, kliknij prawym przyciskiem myszy **modeli** folderu w **Eksploratora rozwiązań**, wybierz opcję **Dodaj** i następnie **nowy element** opcji. W obszarze **kodu**, wybierz **klasy** elementu i nazwij plik *Album.cs*, następnie kliknij przycisk **Dodaj**.
6. Dodaj dwie właściwości do klasy albumu: **gatunku** i **tytuł**. Aby to zrobić, Dodaj następujący kod:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 — albumu Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Zadanie 2 — Dodawanie StoreBrowseViewModel

A **StoreBrowseViewModel** będzie używany w tym zadaniu pokazanie ze zdjęciami, spełniających wybrane gatunku. W tym zadaniu zostanie Utwórz tej klasy, a następnie dodaj dwie właściwości, aby obsłużyć **gatunku** i jego **albumu**użytkownika na liście.

1. Dodaj **StoreBrowseViewModel** klasy. Aby to zrobić, kliknij prawym przyciskiem myszy **modele widoków** folderu w **Eksploratora rozwiązań**, wybierz opcję **Dodaj** i następnie **nowy element** opcji. W obszarze **kodu**, wybierz **klasy** elementu i nazwij plik *StoreBrowseViewModel.cs*, następnie kliknij przycisk **Dodaj**.
2. Dodawanie odwołania do modeli w **StoreBrowseViewModel** klasy. Aby to zrobić, Dodaj następujące za pomocą przestrzeni nazw:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 - Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Dodaj dwie właściwości do **StoreBrowseViewModel** klasy: **gatunku** i **albumów**. Aby to zrobić, Dodaj następujący kod:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 - Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> Co to jest **listy&lt;albumu&gt;**  ?: Ta definicja używa **listy&lt;T&gt;**  typu, gdzie **T** ogranicza Typ elementów, które tego **listy** należą do użytkownika, w tym przypadku **albumu** (i jego elementy podrzędne).
> 
> Ta możliwość projektowania klas i metod, które Odrocz specyfikację jeden lub więcej typów, aż do klasy lub metody jest zadeklarowana i tworzone przez kod klienta jest funkcją języka C# o nazwie **ogólne**.
> 
> **Lista&lt;T&gt;**  ogólny odpowiednik **ArrayList** typu i jest dostępny w **System.Collections.Generic** przestrzeni nazw. Jedną z korzyści z używania **ogólne** , ponieważ właściwość type jest określona, nie trzeba zadbać o sprawdzanie operacji, takich jak elementy do rzutowania typów **albumu** tak jak **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Zadanie 3 — przy użyciu nowego ViewModel w StoreController

W ramach tego zadania zmodyfikujesz **StoreController**firmy **Przeglądaj** i **szczegóły** metody akcji, aby użyć nowego **StoreBrowseViewModel** .

1. Dodaj odwołanie do folderu modeli w **StoreController** klasy. Aby to zrobić, należy rozwinąć **kontrolerów** folderu w **Eksploratora rozwiązań** , a następnie otwórz **StoreController** klasy. Następnie dodaj następujący kod:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 - Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Zastąp **Przeglądaj** metody akcji, aby użyć **StoreViewBrowseController** klasy. Utworzysz z fikcyjnymi danymi na określonego rodzaju i dwa nowe obiekty albumów (w następnym warsztatów będą używać prawdziwe dane pochodzące z bazy danych). Aby to zrobić, Zastąp **Przeglądaj** metoda następującym kodem:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 - Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Zastąp **szczegóły** metody akcji, aby użyć **StoreViewBrowseController** klasy. Zostanie utworzony nowy **albumu** obiekt ma zostać zwrócony **widoku**. Aby to zrobić, Zastąp **szczegóły** metoda następującym kodem:

    (Code Snippet — *podstawy platformy ASP.NET MVC 4 - Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Zadanie 4 — Dodawanie szablonu widoku przeglądania

W tym zadaniu należy dodać **Przeglądaj** widok, aby wyświetlić albumów znalezione dla określonego rodzaju.

1. Przed utworzeniem nowego szablonu widoku, należy skompilować projekt, aby **Dodaj widok** okna dialogowego obsługującemu **ViewModel** klasy. Skompiluj projekt, wybierając **kompilacji** element menu i następnie **kompilacji MvcMusicStore**.
2. Dodaj **Przeglądaj** widoku. Aby to zrobić, kliknij prawym przyciskiem myszy **Przeglądaj** metody akcji **StoreController** i kliknij przycisk **Dodaj widok**.
3. W **Dodaj widok** okna dialogowego upewnij się, że nazwa widoku jest **Przeglądaj**. Sprawdź **utworzyć widok silnie typizowane** pole wyboru i wybrać **StoreBrowseViewModel** z **klasa modelu** listy rozwijanej. Pozostaw inne pola przywrócić wartości domyślne. Następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku przeglądania](aspnet-mvc-4-fundamentals/_static/image29.png "Dodawanie widoku przeglądania")

    *Dodawanie widoku przeglądania*
4. Modyfikowanie **Browse.cshtml** do wyświetlania informacji gatunku, uzyskiwanie dostępu do **StoreBrowseViewModel** obiektu, który jest przekazywany do szablonu widoku. Aby to zrobić, Zastąp zawartość następującym kodem: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

W tym zadaniu przetestujesz, **Przeglądaj** metoda pobiera albumy ze **Przeglądaj** metody akcji.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do   **/Store/Przeglądaj? Gatunku = Najdywania** Aby zweryfikować, że akcja zwraca dwa albumów.

    ![Przeglądanie w albumy Disco Store](aspnet-mvc-4-fundamentals/_static/image30.png "przeglądanie w albumy Disco Store")

    *Przeglądanie w albumy Disco Store*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Zadanie 6 — wyświetlanie informacji o określonych albumu

W ramach tego zadania zostaną zaimplementowane **Store/Details** widok, aby wyświetlić informacje o określonych albumu. W tym laboratorium praktyczne wszystko, co spowoduje wyświetlenie o album znajduje się już w **widoku** szablonu. Tak, zamiast tworzyć **StoreDetailsViewModel** klasy, użyje bieżącego **StoreBrowseViewModel** szablonu, przekazując Album do niego.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Dodaj nową **szczegóły** wyświetlić **StoreController**firmy **szczegóły** metody akcji. Aby to zrobić, kliknij prawym przyciskiem myszy **szczegóły** method in Class metoda **StoreController** klasy, a następnie kliknij przycisk **Dodaj widok**.
2. W **Dodaj widok** okna dialogowego, upewnij się, że **nazwy widoku** jest **szczegóły**. Sprawdź **utworzyć widok silnie typizowane** pole wyboru i wybrać **albumu** z **klasa modelu** listy rozwijanej. Pozostaw inne pola przywrócić wartości domyślne. Następnie kliknij przycisk **Dodaj**. Spowoduje to utworzenie i Otwórz **\Views\Store\Details.cshtml** pliku.

    ![Dodawanie widoku szczegółów](aspnet-mvc-4-fundamentals/_static/image31.png "Dodawanie widoku szczegółów")

    *Dodawanie widoku szczegółów*
3. Modyfikowanie **Details.cshtml** plik, aby wyświetlać informacje fotograficzne, uzyskiwanie dostępu do **albumu** obiektu, który jest przekazywany do szablonu widoku. Aby to zrobić, Zastąp zawartość następującym kodem: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Zadanie 7. uruchamianie aplikacji

W tym zadaniu przetestujesz, **szczegóły** widok pobiera informacje firmy Album z **szczegóły akcji** metody.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Projekt jest uruchamiany w **Home** strony. Zmień adres URL do **/Store/Details/5** zweryfikowania informacji albumu.

    ![Przeglądanie w albumy szczegółów](aspnet-mvc-4-fundamentals/_static/image32.png "przeglądanie w albumy szczegółów")

    *Album przeglądania szczegółów*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Zadanie 8 — Dodawanie łącza między stronami

W tym zadaniu doda linkiem w widoku Store, aby mieć łącze w każdej nazwie gatunku do odpowiedniego **/Store/przeglądania** adresu URL. Dzięki temu po kliknięciu gatunku, na przykład **Najdywania**, spowoduje przejście do **/Store/przeglądania? gatunku = Najdywania** adresu URL.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Aktualizacja **indeksu** stronę, aby dodać łącze do **Przeglądaj** strony. Aby to zrobić, w **Eksploratora rozwiązań** rozwiń **widoków** folder, a następnie **Store** folder i kliknij dwukrotnie plik **Index.cshtml** strony.
2. Dodaj link do widoku przeglądania wskazujący gatunku wybrane. Aby to zrobić, Zastąp następujący wyróżniony kod w ramach **&lt;li&gt;** tagi: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > innym rozwiązaniem będzie łączenia bezpośrednio do strony, przy użyciu kodu, jak pokazano poniżej:
   > 
   > &lt;href =&quot;/Store/przeglądania? gatunku =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Mimo że ta metoda działa, zależy ciąg zapisane na stałe. Jeśli później zmienisz kontrolera, należy ręcznie zmienić tę instrukcję. Lepszą alternatywą jest użycie **pomocnika kodu HTML** metody. Platforma ASP.NET MVC zawiera metodą pomocnika kodu HTML, który jest dostępny dla tych zadań. **Html.ActionLink()** metody pomocnika ułatwia tworzenie HTML **&lt;&gt;** łącza, upewniając się, ścieżek URL są poprawnie zakodowane w adresie URL.
   > 
   > Htlm.ActionLink ma kilka przeciążeń. W tym ćwiczeniu zostanie użyty, który przyjmuje trzy parametry:
   > 
   > 1. Tekst łącza, co spowoduje wyświetlenie nazwy gatunku
   > 2. Nazwa akcji kontrolera (**Przeglądaj**)
   > 3. Wartości parametrów, określając nazwę trasy (**gatunku**) i wartości (**nazwę gatunku**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Zadanie 9 — uruchamianie aplikacji

W tym zadaniu przetestujesz czy każdego gatunku jest wyświetlany Link do odpowiedniego **/Store/przeglądania** adresu URL.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/Store** Aby sprawdzić, czy każdego gatunku linki do odpowiednich **/Store/przeglądania** adresu URL.

    ![Przeglądanie gatunki wraz z łączami do strony Przegląd](aspnet-mvc-4-fundamentals/_static/image33.png "przeglądania gatunki wraz z łączami do przeglądania stron")

    *Przeglądanie gatunki wraz z łączami do przeglądania stron*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Zadanie 10 - przy użyciu kolekcji dynamicznych ViewModel przekazania wartości

W tym zadaniu dowiesz się, prosta i skuteczna metoda przekazać wartości między kontrolerem a widokiem bez wprowadzania żadnych zmian w modelu. ASP.NET MVC 4 dostarcza kolekcję &quot;ViewModel&quot;, który przypisany do żadnej wartości dynamicznych i dostęp do wewnątrz także widoków i kontrolerów.

Teraz użyjesz kolekcji dynamicznych elementów ViewBag przekazać listę &quot; **oznaczone gatunki** &quot; z kontrolera do widoku. Widok indeksu Store będą miały dostęp do **ViewModel** i wyświetlić informacje.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Otwórz **StoreController.cs** i modyfikować **indeksu** metodę, aby utworzyć listę oznaczone gatunki do kolekcji ViewModel:

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Można również używać składni **obiekt ViewBag [&quot;Starred&quot;]** uzyskać dostęp do właściwości.
2. Ikona gwiazdki **&quot;starred.png&quot;** znajduje się w **Source\Assets\Images** folder w tym laboratorium. Aby dodać go do aplikacji, przeciągnij ich zawartość z **Eksplorator Windows** okna do **Eksploratora rozwiązań** w Visual Web Developer Express, jak pokazano poniżej:

    ![Dodawanie obrazu gwiazdy z rozwiązaniem](aspnet-mvc-4-fundamentals/_static/image34.png "obraz gwiazdki dodawanie do rozwiązania")

    *Dodawanie obrazu gwiazdki w rozwiązaniu*
3. Powoduje ono otwarcie widoku **Store/Index.cshtml** i modyfikować zawartość. Będzie odczytywał &quot;oznaczone&quot; właściwość **obiekt ViewBag** kolekcji i zapytaj, czy bieżąca nazwa gatunku na liście. W takim przypadku wyświetli ikonę gwiazdki bezpośrednio do łącza gatunku.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Zadanie 11 — uruchamianie aplikacji

W tym zadaniu przetestujesz że gwiazdkami gatunki wyświetlać ikonę gwiazdki.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Projekt jest uruchamiany w **Home** strony. Zmień adres URL do **/Store** do sprawdzenia, czy każdego gatunku polecane ma wagę do poszanowania etykiety:

    ![Przeglądanie gatunki z elementami gwiazdkami](aspnet-mvc-4-fundamentals/_static/image35.png "przeglądania gatunki z elementami gwiazdkami")

    *Przeglądanie gatunki z elementami gwiazdkami*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Ćwiczenie 7: Omówienie nowego szablonu platformy ASP.NET MVC 4

W tym ćwiczeniu zostanie Poznaj rozszerzenia w szablonach projektu ASP.NET MVC 4, biorąc się najbardziej istotne cechy nowy szablon.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Zadanie 1: Eksplorowanie szablonu aplikacji internetowych platformy ASP.NET MVC 4

1. Jeśli nie już jest otwarty, uruchom **VS Express for Web**
2. Wybierz **pliku | Nowe | Projekt** polecenia menu. W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w okienku po lewej stronie drzewa i wybierz polecenie **aplikacji sieci Web programu ASP.NET MVC 4**. **Nazwa** projektu *MusicStore* i zaktualizuj **Nazwa rozwiązania** do *rozpocząć*, a następnie wybierz lokalizację (lub pozostaw wartość domyślną) i kliknij przycisk **OK** .

    ![Tworzenie nowego projektu ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "tworzenia nowego projektu ASP.NET MVC 4")

    *Tworzenie nowego projektu ASP.NET MVC 4*
3. W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** szablonu projektu i kliknij przycisk **OK**. Należy zauważyć, że można wybrać jako aparat widoku Razor lub ASPX.

    ![Tworzenie nowej platformy ASP.NET MVC 4 Internet aplikacji](aspnet-mvc-4-fundamentals/_static/image37.png "Tworzenie nowej aplikacji internetowych 4 platformy ASP.NET MVC")

    *Tworzenie nowej aplikacji internetowych 4 platformy ASP.NET MVC*

    > [!NOTE]
    > Składnia razor został wprowadzony w ASP.NET MVC 3. Jej celem jest zminimalizowanie liczby znaków i naciśnięć klawiszy wymagane w pliku, umożliwiając szybkie i płynów, przepływ pracy kodowania. Razor wykorzystuje istniejące języków C# /VB (lub inną) umiejętności językowe i oferuje składni znaczników szablonu, która włącza awesome przepływ pracy tworzenia kodu HTML.
4. Naciśnij klawisz **F5** do uruchamiania rozwiązania i wyświetlić szablon, odnowienia. Możesz sprawdzić następujące funkcje:

    1. **Szablony aplikacji w stylu Modern**

        Szablony Odnowiono, podając więcej styli nowoczesnym wyglądzie.

        ![Szablony ASP.NET MVC 4 restyled](aspnet-mvc-4-fundamentals/_static/image38.png "restyled szablony programu ASP.NET MVC 4")

        *Szablony ASP.NET MVC 4 restyled*
    2. **Funkcje adaptacyjnego sterowania renderowania**

        Zapoznaj się z zmiany rozmiaru okna przeglądarki i zwróć uwagę, jak układ strony dynamicznie dostosowuje się do nowego rozmiaru okna. Te szablony umożliwiają technika adaptacyjne renderowania są prawidłowo renderowane na platformach zarówno komputerów i urządzeń przenośnych, bez potrzeby dostosowywania.

        ![Szablon projektu platformy ASP.NET MVC 4 w innej przeglądarce rozmiarach](aspnet-mvc-4-fundamentals/_static/image39.png "szablon projektu platformy ASP.NET MVC 4 w rozmiarach innej przeglądarki")

        *Szablon projektu platformy ASP.NET MVC 4 w rozmiarach innej przeglądarki*
5. Zamknij przeglądarkę, aby zatrzymać debuger i powrócić do programu Visual Studio.
6. Teraz jesteś w stanie rozwiązania i zapoznaj się z nowych funkcji wprowadzonych przez program ASP.NET MVC 4 w szablonie projektu.

    ![ASP.NET MVC4-internetowego aplikacji-— szablon projektu](aspnet-mvc-4-fundamentals/_static/image40.png "szablon projektu aplikacji internetowych platformy ASP.NET MVC 4")

    *Szablon projektu aplikacji internetowych platformy ASP.NET MVC 4*

   1. **Kod znaczników języka HTML5**

       Przeglądaj widoków szablonu, aby dowiedzieć się, nowy motyw języka znaczników, na przykład otwórz **About.cshtml** wyświetlać w **Home** folderu.

       ![Nowy szablon, przy użyciu znaczników Razor i HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nowego szablonu, za pomocą znaczników Razor i HTML5")

       *Nowy szablon, przy użyciu znaczników Razor i HTML5*
   2. **Biblioteki JavaScript uwzględnione**

      1. **jQuery**: jQuery upraszcza przechodzenie dokumentu HTML, obsługa zdarzeń, animowanie i interakcje Ajax.
      2. **Interfejs użytkownika jQuery**: Ta biblioteka zawiera elementy abstrakcji do interakcji z niskiego poziomu, zaawansowana efektów animacji i widżetów elementów WebParts, zbudowany na podstawie biblioteki JavaScript jQuery.

         > [!NOTE]
         > Informacje na temat jQuery i interfejs użytkownika jQuery w [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).
      3. **KnockoutJS**: szablonu domyślnego platformy ASP.NET MVC 4 zawiera teraz **KnockoutJS**, struktura JavaScript MVVM, która umożliwia tworzenie rozbudowanych i reagujących aplikacji sieci web przy użyciu języka JavaScript i HTML. Podobnie jak w programie ASP.NET MVC 3, jQuery i biblioteki interfejsu użytkownika jQuery znajdują się również w ASP.NET MVC 4.

          > [!NOTE]
          > Można uzyskać więcej informacji na temat biblioteki KnockOutJS w to łącze: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).
      4. **Modernizr**: Ta biblioteka jest uruchamiana automatycznie, dzięki czemu witryny zgodny z starsze przeglądarki przy użyciu technologii HTML5 i CSS3.

          > [!NOTE]
          > Można uzyskać więcej informacji na temat biblioteki Modernizr w to łącze: [ http://www.modernizr.com/ ](http://www.modernizr.com/).
   3. **SimpleMembership zawarty w rozwiązaniu**

       SimpleMembership został zaprojektowany jako zamiennika dla poprzedniego systemu dostawcy ról ASP.NET i członkostwa. Ma wiele nowych funkcji, które ułatwiają deweloperom bezpieczne stron sieci web w sposób bardziej elastyczne.

       Szablon Internet już skonfigurował kilka rzeczy, aby zintegrować SimpleMembership, na przykład elementu AccountController jest gotowa do używania OAuthWebSecurity (w przypadku rejestracji konto uwierzytelniania OAuth, logowania, zarządzania, itp.) i zabezpieczenia sieci Web.

       ![SimpleMembership zawarty w rozwiązaniu](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership zawarty w rozwiązaniu")

       *SimpleMembership zawarty w rozwiązaniu*

       > [!NOTE]
       > Dowiedz się więcej informacji na temat [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) w bibliotece MSDN.

> [!NOTE]
> Ponadto można wdrożyć tę aplikację do witryny sieci Web systemu Windows Azure następujące [dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Realizując ten warsztatów wiesz podstawy platformy ASP.NET MVC:

- Podstawowe elementy aplikacji MVC i sposób ich interakcji
- Jak utworzyć aplikację ASP.NET MVC
- Jak dodać i skonfigurować kontrolery, aby obsłużyć parametry przekazywane za pośrednictwem adresu URL i querystring
- Jak dodać stronę wzorcową układu, aby skonfigurować szablon dla typowych zawartość HTML, arkusz stylów, aby poprawić wygląd i działanie i Wyświetl szablon do wyświetlania zawartości HTML
- Jak używać wzorzec ViewModel przekazywania właściwości w szablonie widok do wyświetlania informacji dynamicznych
- Jak używać parametrów przekazanych do kontrolerów widoku szablonu
- Jak dodać łącza do stron w aplikacji ASP.NET MVC
- Jak dodać i korzystać z właściwości dynamicznych w widoku
- Ulepszenia w szablonach projektu ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "zainstalować program Visual Studio Express")

    *Zainstaluj program Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-fundamentals/_static/image47.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

Ten dodatek będzie pokazują, jak utworzyć nową witrynę sieci web w portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskany postępując zgodnie z laboratorium, korzystając z zalet funkcji publikowania narzędzia Web Deploy oferowanego przez system Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1. Tworzenie nowej witryny sieci Web z Windows Azure Portal

1. Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft, powiązaną z Twoją subskrypcją.

    > [!NOTE]
    > Platforma Windows Azure można bezpłatny hosting 10 witryn sieci Web platformy ASP.NET i skalowanie w miarę wzrostu ruchu. Możesz zarejestrować się [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu usługi Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Zaloguj się do portalu usługi Windows Azure")

    *Zaloguj się do portalu zarządzania systemu Azure Windows*
2. Kliknij przycisk **New** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-fundamentals/_static/image49.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczenia** | **witryny sieci Web**. Następnie wybierz pozycję **szybkie tworzenie** opcji. Podaj adres URL dostępny dla nowej witryny sieci web, a następnie kliknij przycisk **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Witryny sieci Web Windows Azure jest hostem dla aplikacji sieci web działające w chmurze, które można kontrolować i zarządzać nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do Windows Azure witryny sieci Web z poza portalem. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkiego tworzenia](aspnet-mvc-4-fundamentals/_static/image50.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj, aż nowe **witryny sieci Web** zostanie utworzony.
5. Po utworzeniu witryny sieci Web kliknij link w obszarze **adresu URL** kolumny. Sprawdź, czy działa nową witrynę sieci Web.

    ![Przejście do nowej witryny sieci web](aspnet-mvc-4-fundamentals/_static/image51.png "przejście do nowej witryny sieci web")

    *Przejście do nowej witryny sieci web*

    ![Witryna sieci Web działa](aspnet-mvc-4-fundamentals/_static/image52.png "witryna sieci Web działa")

    *Witryna sieci Web działa*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie strony zarządzania witryny sieci web](aspnet-mvc-4-fundamentals/_static/image53.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie strony zarządzania witryny sieci Web*
7. W **pulpit nawigacyjny** w obszarze **Przegląd** kliknij **Pobierz profil publikowania** łącza.

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web w witrynie sieci Web usługi Windows Azure dla poszczególnych metod włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do łączenia się i uwierzytelnianie w odniesieniu do każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** Obsługa odczytywania profili publikowania do automatyzowania konfiguracji tych programów dla Publikowanie aplikacji sieci web w usłudze Windows Azure websites.

    ![Pobieranie witryny sieci web profil publikowania](aspnet-mvc-4-fundamentals/_static/image54.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz plik profilu publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do Windows Azure Web Sites z programu Visual Studio za pomocą tego pliku.

    ![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-fundamentals/_static/image55.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, musisz utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Należy serwera usługi SQL Database do przechowywania bazy danych aplikacji. Możesz wyświetlić serwery baz danych SQL z subskrypcji w portalu zarządzania Azure Windows pod **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**. Jeśli nie masz serwer, który został utworzony, można utworzyć ją przy użyciu **Dodaj** przycisk na pasku poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, ponieważ będziesz ich używać w kolejne zadania podrzędne. Nie należy tworzyć bazy danych, ponieważ zostanie on utworzony w późniejszym terminie.

    ![Pulpit nawigacyjny z serwera bazy danych SQL](aspnet-mvc-4-fundamentals/_static/image56.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny z serwera bazy danych SQL*
2. W ramach następnego zadania spowoduje przetestowanie połączenia z bazą danych z programu Visual Studio, dlatego trzeba uwzględnić adres IP lokalnego serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżący adres IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) przycisku.

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP listy, kliknij pozycję **Zapisz** aby potwierdzić zmiany.

    ![Potwierdź zmiany](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Potwierdź zmiany*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **Publikuj**.

    ![Publikowanie aplikacji](aspnet-mvc-4-fundamentals/_static/image60.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Importuj profil publikowania, zapisaną w pierwszym zadaniem.

    ![Trwa importowanie profilu publikowania](aspnet-mvc-4-fundamentals/_static/image61.png "importowania profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **sprawdzanie poprawności połączenia**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Sprawdzanie poprawności zostało ukończone, gdy zostanie wyświetlony z zielonym znacznikiem wyboru pojawiają się obok przycisku Waliduj połączenie.

    ![Sprawdzanie poprawności połączenia](aspnet-mvc-4-fundamentals/_static/image62.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk obok pola tekstowego połączenia bazy danych (czyli **DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-fundamentals/_static/image63.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W **nazwy serwera** wpisz swoją bazę danych SQL server adresu URL przy użyciu *tcp:* prefiks.
   - W **nazwa_użytkownika** wpisz nazwę logowania administratora serwera.
   - W **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.

     ![Konfigurowanie parametrów połączenia z docelowym](aspnet-mvc-4-fundamentals/_static/image64.png "Konfigurowanie parametrów połączenia z docelowym")

     *Konfigurowanie parametrów połączenia z docelowym*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu, aby utworzyć kliknij bazę danych **tak**.

    ![Tworzenie bazy danych](aspnet-mvc-4-fundamentals/_static/image65.png "Tworzenie parametrów bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do łączenia z bazą danych SQL na platformie Windows Azure jest wyświetlana w polu tekstowym połączenia domyślne. Następnie kliknij przycisk **Dalej**.

    ![Ciąg połączenia wskazujący bazę danych SQL](aspnet-mvc-4-fundamentals/_static/image66.png "ciąg połączenia wskazujący bazę danych SQL")

    *Ciąg połączenia wskazujący bazę danych SQL*
8. W **Podgląd** kliknij **Publikuj**.

    ![Publikowanie aplikacji sieci web](aspnet-mvc-4-fundamentals/_static/image67.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

    ![Aplikacja została opublikowana na platformie Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "aplikacja została opublikowana na platformie Windows Azure")

    *Aplikacja opublikowana na platformie Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: korzystania z fragmentów kodu

Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki. Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.

![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](aspnet-mvc-4-fundamentals/_static/image69.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*

***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).
3. Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.
4. Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragmentu kodu](aspnet-mvc-4-fundamentals/_static/image70.png "Rozpocznij wpisywanie nazwy fragmentu kodu")

*Rozpocznij wpisywanie nazwy fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](aspnet-mvc-4-fundamentals/_static/image71.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](aspnet-mvc-4-fundamentals/_static/image72.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")

*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*

***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](aspnet-mvc-4-fundamentals/_static/image73.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")

*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-fundamentals/_static/image74.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
