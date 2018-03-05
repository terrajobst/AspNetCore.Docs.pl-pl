---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Podstawowe informacje na temat platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: "W tym laboratorium Hands-On opiera się na magazynie utworów muzycznych MVC (Model View Controller), samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać ASP.NET MV..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: f93f51219403cd5aeca2dd3648444a84690c3d25
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-fundamentals"></a>Podstawowe informacje na temat platformy ASP.NET MVC 4

Przez [obozów sieci Web Team](https://twitter.com/webcamps)

[Pobierz obozów sieci Web uczenie Kit](https://aka.ms/webcamps-training-kit)

W tym laboratorium Hands-On jest oparta na sklep muzyczny MVC (Model View Controller), samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać programu ASP.NET MVC i Visual Studio. W laboratorium dowiesz się prostotę, jeszcze mocy przy użyciu tych technologii jednocześnie. Rozpoczyna się od prostej aplikacji i zostanie on skompilowany, aż do uzyskania funkcjonalnej ASP.NET MVC 4 aplikacji sieci Web.

W tym laboratorium współpracuje z platformy ASP.NET MVC 4.

Jeśli chcesz zapoznać się z wersji platformy ASP.NET MVC 3 samouczka aplikacji, można znaleźć w [MVC utworów muzycznych magazynu](https://github.com/evilDave/MVC-Music-Store).

W tym laboratorium Hands-On przyjęto założenie, że deweloper ma doświadczenie w sieci Web development technologii, takich jak HTML i JavaScript.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [wersje Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Specyficzne dla tego laboratorium projektu jest dostępna na [podstawowe informacje na temat platformy ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Aplikacji magazynu utworów muzycznych

Muzyka Magazyn aplikacji sieci web, który zostanie skompilowany w tym laboratorium obejmuje trzy główne części: zakupów, wyewidencjonowania i administracji. Osoby odwiedzające będzie Przeglądaj albumów według rodzaju, Dodaj do koszyka ich albumy, przejrzyj ich zaznaczenie i koniec przejść do wyewidencjonowania, aby się zalogować i ukończyć kolejności. Ponadto magazynu administratorzy będą mogli zarządzać albumów dostępne, a także ich głównych właściwości.

![Ekrany magazynu utworów muzycznych](aspnet-mvc-4-fundamentals/_static/image1.png "ekrany magazynu utworów muzycznych")

*Ekrany magazynu utworów muzycznych*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

Aplikację ze sklepu utworów muzycznych zostaną skompilowane z użyciem **kontrolera MVC (Model View)**, wzorzec architektury, która dzieli aplikację na trzy główne składniki:

- **Modele**: obiekty modelu są częściami aplikacji, które implementują logikę domeny. Często obiekty modelu również pobrać i przechowywanie stanu modelu w bazie danych.
- **Widoki:** widoki są składnikami wyświetlającymi interfejs użytkownika aplikacji (UI). Zazwyczaj interfejs ten jest utworzone na podstawie danych modelu. Przykładem może być widok edycji albumów zawierający pola tekstowe i listy rozwijanej, w oparciu o bieżący stan obiektu albumu.
- **Kontrolery:** kontrolery są składnikami, które obsługują interakcję z użytkownikiem, manipulować modelem i ostatecznie wybierają widok do renderowania interfejsu użytkownika. W aplikacji MVC widoku wyświetlane są tylko informacje; Kontroler obsługuje i ma odpowiadać na dane wejściowe użytkownika i interakcja.

Wzorzec MVC pomaga tworzyć aplikacji połączonych różne aspekty aplikacji (logika danych wejściowych, logika biznesowa i logika interfejsu użytkownika), zapewniając luźne powiązanie między tymi elementami. Taki rozdział pomaga w zarządzaniu złożonością podczas tworzenia aplikacji, ponieważ pozwala skupić się na jednym aspekcie implementacji naraz. Ponadto wzorzec MVC ułatwia testowanie aplikacji również wspieranie stosowania projektowanie oparte na (testach TDD) do tworzenia aplikacji.

**ASP.NET MVC** framework stanowi alternatywę dla składnika ASP.NET Web Forms wzorzec służący do tworzenia aplikacji sieci Web opartych na platformie ASP.NET MVC. **ASP.NET MVC** framework to struktura prezentacji niewielka, wysoce testowalna która (tak jak aplikacje oparte na formularzach sieci Web) jest zintegrowana z istniejącymi funkcjami platformy ASP.NET, takich jak strony wzorcowe i oparte na członkostwie uwierzytelnianie, aby pobrać wszystkie możliwości platformy .NET core. Jest to przydatne, jeśli już znasz z formularzy sieci Web ASP.NET wszystkie biblioteki, które już używają nie są dostępne w programie ASP.NET MVC 4 oraz.

Ponadto luźne powiązanie między trzema głównymi składnikami aplikacji MVC wspiera także Programowanie równoległe. Na przykład co może pracować w widoku, drugi może pracować nad logiką kontrolera i trzeci deweloper może skupić się na logiki biznesowej w modelu.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium Hands-On przedstawiono sposób:

- Tworzenie aplikacji platformy ASP.NET MVC od podstaw oparte na samouczek aplikacji magazynu utworów muzycznych
- Dodaj kontrolery do obsługi adresów URL do strony głównej witryny oraz przeglądanie jej funkcji main
- Dodaj widok, aby dostosować zawartość wyświetlana wraz z jego styl
- Dodawanie klasy modelu zawierają i zarządzanie nimi danych i domeny logiki
- Użyj wzorca Model widoku do przekazywania informacji z akcji kontrolera do widoku szablonów
- Eksploruj nowego szablonu platformy ASP.NET MVC 4 dla aplikacji internetowych

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Musi mieć następujące elementy do przygotowania tego laboratorium:

- [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

**Instalowanie wstawki kodu**

Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio. Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.

Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek C:](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

W tym laboratorium Hands-On składa się przez następujących czynnościach:

1. [Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC MusicStore](#Exercise1)
2. [Ćwiczenie 2: Tworzenie kontrolera](#Exercise2)
3. [Ćwiczenie 3: Przekazywanie parametrów do kontrolera](#Exercise3)
4. [Ćwiczenie 4: Tworzenie widoku](#Exercise4)
5. [Ćwiczenie 5: Tworzenie modelu widoku](#Exercise5)
6. [Ćwiczenie 6: W widoku przy użyciu parametrów](#Exercise6)
7. [Ćwiczenie 7: Laptop wokół nowego szablonu platformy ASP.NET MVC 4](#Exercise7)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach. Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.


Szacowany czas trwania tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC MusicStore

W tym ćwiczeniu dowiesz się, tworzenie aplikacji platformy ASP.NET MVC w programie Visual Studio 2012 Express dla sieci Web, a także jego organizacji folderu głównego. Ponadto dowiesz sposobu dodawania nowego kontrolera i zapewnić ich prostego ciągu wyświetlanego w strony głównej aplikacji.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Zadanie 1 — Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC

1. W ramach tego zadania spowoduje utworzenie pusty projekt aplikacji platformy ASP.NET MVC przy użyciu szablonu MVC programu Visual Studio. Uruchom **VS Express for Web**.
2. Na **pliku** menu, kliknij przycisk **nowy projekt**.
3. W **nowy projekt** wybierz okno dialogowe **aplikacji sieci Web programu ASP.NET MVC 4** typu znajduje się w obszarze projektu **Visual C#,** **Web** szablonu Lista.
4. Zmień **nazwa** do *MvcMusicStore*.
5. Ustaw lokalizację rozwiązania wewnątrz nowy **rozpocząć** folder, w tym ćwiczeniu folder źródłowy, na przykład **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-dzień HOL-PATH]**. Kliknij przycisk **OK**.

    ![Utwórz okno dialogowe Nowy projekt](aspnet-mvc-4-fundamentals/_static/image2.png "utworzyć okno dialogowe Nowy projekt")

    *Utwórz okno dialogowe Nowy projekt*
6. W **nowy projekt programu ASP.NET MVC 4** wybierz okno dialogowe **podstawowe** szablonu i upewnij się, że **aparat widoku** jest **Razor**. Kliknij przycisk **OK**.

    ![Okno dialogowe nowego projektu platformy ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "okno dialogowe Nowy projekt ASP.NET MVC 4")

    *Okno dialogowe nowego projektu platformy ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Zadanie 2 — poznawanie struktury rozwiązania

Platforma ASP.NET MVC zawiera szablon projektu Visual Studio, która ułatwia tworzenie aplikacji sieci Web obsługujące wzorzec MVC. Ten szablon tworzy nową aplikację sieci Web programu ASP.NET MVC z wymagane foldery, szablonów elementów i we wpisach w plikach konfiguracji.

To zadanie należy zbadać struktury rozwiązania zrozumienie elementy, które są zaangażowane oraz ich relacji. Następujące foldery są zawarte w aplikacji ASP.NET MVC, ponieważ platforma ASP.NET MVC domyślnie korzysta z &quot;Konwencji za pośrednictwem konfiguracji&quot; podejście i sprawia, że niektóre założenia domyślne oparte na folder nazewnictwa konwencje.

1. Po utworzeniu projektu, przejrzyj struktury folderów, który został utworzony w Eksploratorze rozwiązań po prawej stronie:

    ![Struktura ASP.NET MVC folderów w Eksploratorze rozwiązań](aspnet-mvc-4-fundamentals/_static/image4.png "struktury ASP.NET MVC folderów w Eksploratorze rozwiązań")

    *Struktura ASP.NET MVC folderów w Eksploratorze rozwiązań*

    1. **Kontrolery**. Ten folder będzie zawierać klasy kontrolera. W aplikacji MVC na podstawie kontrolery są zobowiązani do obsługi interakcji użytkownika końcowego, manipulowanie modelem i ostatecznie wybierając widoku do renderowania interfejsu użytkownika.

        > [!NOTE]
        > Platforma MVC wymaga nazwy wszystkich kontrolerów kończącej się &quot;kontrolera&quot;— na przykład HomeController, LoginController lub ProductController.
    2. **Modele**. Ten folder jest dostępna dla klasy reprezentujące model aplikacji dla aplikacji sieci Web MVC. Obejmuje to zazwyczaj kod, który definiuje obiekty i logika interakcji z magazynu danych. Zazwyczaj rzeczywiste modelu obiektów jest w bibliotekach osobnej klasy. Jednak podczas tworzenia nowej aplikacji może zawierać klasy i przenieś je do biblioteki klas oddzielne w późniejszym czasie w cyklu tworzenia.
    3. **Widoki**. Ten folder jest zalecana lokalizacja widoków, składniki odpowiada za wyświetlanie interfejsu użytkownika aplikacji. Widoki używają plików aspx, ascx, cshtml i .master, oprócz innych plików, które są powiązane z widokami renderowania. Widoki folder zawiera folder dla każdego kontrolera; folder nosi nazwę z prefiksem nazwy kontrolera. Na przykład, jeśli masz kontroler o nazwie **HomeController**, widoki folder zawiera folder o nazwie Home. Domyślnie, gdy platforma ASP.NET MVC ładuje widoku szuka plik .aspx o nazwie żądany widok w folderze Views\controllerName (**widoków [ControllerName] [Action] .aspx**) lub (**widoków [ControllerName] [Action] .cshtml**) dla widokami Razor.

    > [!NOTE]
    > Oprócz folderów wymienionymi wcześniej, korzysta z aplikacji sieci Web MVC **Global.asax** plik routingu adresów URL globalne ustawienia domyślne i używa **Web.config** plik do skonfigurowania aplikacji.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Zadanie 3. Dodawanie HomeController

W aplikacjach ASP.NET, które nie korzystają z struktura MVC interakcji z użytkownikiem są zorganizowane wokół stron i wywoływanie zdarzeń i obsługa zdarzeń z tych stron. Z kolei interakcji użytkowników z aplikacji ASP.NET MVC skupiono kontrolerów i metod akcji.

Z drugiej strony platforma ASP.NET MVC mapuje adresy URL do klasy, które są określane jako kontrolery. Kontrolery przetworzyć żądań przychodzących, obsługi danych wejściowych użytkownika i interakcji, wykonywanie logiki aplikacji odpowiednie i określić odpowiedzi do odesłania do klienta (wyświetlania kodu HTML, Pobierz plik, przekierowania do innego adresu URL itp.). W przypadku wyświetlania kodu HTML, klasę kontrolera zwykle wywołuje składnik osobny widok, aby wygenerować kod znaczników HTML dla żądania. W aplikacji MVC widoku wyświetlane są tylko informacje; Kontroler obsługuje i ma odpowiadać na dane wejściowe użytkownika i interakcja.

To zadanie spowoduje dodanie klasy kontrolera, która będzie obsługiwać adresy URL do strony głównej witryny sklep muzyczny.

1. Kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań wybierz **Dodaj** , a następnie **kontrolera** polecenia:

    ![Dodaj polecenie kontrolera](aspnet-mvc-4-fundamentals/_static/image5.png "Dodawanie polecenia kontrolera")

    *Dodaj polecenie kontrolera*
2. **Dodaj kontroler** zostanie wyświetlone okno dialogowe. Nazwa kontrolera *HomeController* i naciśnij klawisz **Dodaj**.

    ![Kontroler okno dialogowe Dodawanie](aspnet-mvc-4-fundamentals/_static/image6.png "kontrolera okno dialogowe Dodawanie")

    *Dodawanie kontrolera okna dialogowego*
3. Plik **HomeController.cs** jest tworzony w **kontrolerów** folderu. Aby przypisać **HomeController** zwraca ciąg na jego akcji indeksu, Zastąp **indeksu** metodę z następującym kodem:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu HomeController Ex1*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

To zadanie będzie Wypróbuj tę aplikację w przeglądarce sieci web.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji. Projekt jest kompilowany i uruchamia serwer sieci Web IIS lokalnej. Lokalny serwer sieci Web usług IIS zostanie automatycznie uruchomiona przeglądarki sieci web wskazuje adres URL serwera sieci Web.

    ![Aplikacja uruchomiona w przeglądarce sieci web](aspnet-mvc-4-fundamentals/_static/image7.png "aplikacja była uruchomiona w przeglądarce sieci web")

    *Aplikacja uruchomiona w przeglądarce sieci web*

    > [!NOTE]
    > Lokalnego serwera sieci Web usług IIS będą uruchamiane witryny sieci Web na numer portu wolne losowych. Na powyższej ilustracji lokacji działa w `http://localhost:50103/`, więc używa portu 50103. Numer portu może się różnić.
2. Zamknij przeglądarkę.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Ćwiczenie 2: Tworzenie kontrolera

W tym ćwiczeniu dowiesz się, jak zaktualizować kontroler do zaimplementowania proste funkcje aplikacji utworów muzycznych magazynu. Ten kontroler określi metod akcji do każdego z następujących określone żądania obsługi:

- Strony listę gatunkami muzyki utworów muzycznych w magazynie utworów muzycznych
- Strona przeglądania, która zawiera listę wszystkich albumów muzycznych dla określonego rodzaju
- Strona szczegółów, która zawiera informacje o albumu określonych utworów muzycznych

W zakresie tego ćwiczenia te akcje po prostu zwróci ciąg przez teraz.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Zadanie 1 — Dodawanie nowej klasy StoreController

To zadanie spowoduje dodanie nowego kontrolera.

1. Jeśli nie już otwarty, uruchom **VS Express for Web 2012**.
2. W **pliku** menu, wybierz **Otwórz projekt**. W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex02 CreatingAController\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**. Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Dodaj nowy kontroler. Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań wybierz **Dodaj** , a następnie **kontrolera** polecenia. Zmień **nazwy kontrolera** do *StoreController*i kliknij przycisk **Dodaj**.

    ![Kontroler okno dialogowe Dodawanie](aspnet-mvc-4-fundamentals/_static/image8.png "kontrolera okno dialogowe Dodawanie")

    *Dodawanie kontrolera okna dialogowego*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Zadanie 2 - modyfikowanie StoreController akcje

To zadanie, należy zmodyfikować metod kontrolera, które są nazywane **akcje**. Akcje są odpowiedzialne za obsługę żądań adresu URL i określanie zawartość, która ma zostać odesłany do przeglądarki lub użytkownik, który wywołał adres URL.

1. **StoreController** klasa ma już **indeksu** metody. Go w dalszej części tego laboratorium zostanie użyta do wdrożenia strony, który zawiera listę wszystkich genres magazynu utworów muzycznych. Teraz, po prostu zastąpić **indeksu** metodę z następującym kodem, które zwraca ciąg &quot;Hello z Store.Index()&quot;:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu StoreController Ex2*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Dodaj **Przeglądaj** i **szczegóły** metody. Aby to zrobić, Dodaj następujący kod, aby **StoreController**:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — uruchamianie aplikacji

To zadanie będzie Wypróbuj tę aplikację w przeglądarce sieci web.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Projekt jest uruchamiany w **Home** strony. Zmień adres URL, aby sprawdzić implementacji każdej akcji.

    1. **/ Przechowywania**. Zobaczysz  **&quot;Hello z Store.Index()&quot;**.
    2. **/ Magazynu/Przeglądaj**. Zobaczysz  **&quot;Hello z Store.Browse()&quot;**.
    3. **/ / Szczegóły magazynu**. Zobaczysz  **&quot;Hello z Store.Details()&quot;**.

        ![Przeglądanie StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "przeglądanie StoreBrowse")

        *Przeglądanie /Store/Browse*
3. Zamknij przeglądarkę.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Ćwiczenie 3: Przekazywanie parametrów do kontrolera

Do tej pory mieć zostały zwracanie stałej ciągów z kontrolerów. W tym ćwiczeniu dowiesz się, jak do przekazania parametrów do kontrolera przy użyciu adresu URL i querystring, a następnie sprawdzając akcje metody odpowiedzi z tekstem w przeglądarce.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Zadanie 1 — Dodawanie parametru Genre StoreController

W tym zadaniu użyjesz **querystring** wysłać parametry **Przeglądaj** metody akcji w **StoreController**.

1. Jeśli nie już otwarty, uruchom **VS Express for Web**.
2. W **pliku** menu, wybierz **Otwórz projekt**. W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex03 PassingParametersToAController\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**. Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Otwórz **StoreController** klasy. Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreController.cs**.
4. Zmień **Przeglądaj** metody, dodając parametr typu string na żądanie dla określonego rodzaju. ASP.NET MVC zostanie automatycznie przekazać żadnych querystring lub przesłanie formularza parametrów o nazwie **genre** do tej metody akcji, gdy została wywołana. Aby to zrobić, Zamień **Przeglądaj** metodę z następującym kodem:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > Używasz **HttpUtility.HtmlEncode** metodę narzędzie uniemożliwia użytkownikom wstrzykiwania Javascript do widoku z łączem, takich jak   **/magazynu/Przeglądaj? Genre =&lt;skryptu&gt;window.location= "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.
    > 
    > Aby uzyskać dokładniejsze objaśnienie, odwiedź stronę [ten artykuł w witrynie msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — w uruchomionej aplikacji

W tym zadaniu zostanie wypróbowanie aplikacji w przeglądarce sieci web i używać **genre** parametru.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Projekt jest uruchamiany w **Home** strony. Zmień adres URL do   */przechowywania/Przeglądaj? Genre = Disco* można zweryfikować, że akcja otrzyma parametru genre.

    ![Przeglądanie StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "przeglądanie StoreBrowseGenre = Disco")

    *Przeglądanie /Store/Browse? Genre = Disco*
3. Zamknij przeglądarkę.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Zadanie 3. Dodawanie parametru identyfikatora osadzone w adresie URL

W tym zadaniu użyjesz **adres URL** do przekazania **identyfikator** parametr **szczegóły** metody akcji **StoreController**. ASP.NET MVC domyślnej konwencji routingu jest Traktuj segment adresu URL po nazwy metody akcji, jako parametr o nazwie **identyfikator**. Jeśli metodę akcji ma parametr o nazwie identyfikator, ASP.NET MVC zostanie automatycznie przekazuj segment adresu URL do użytkownika jako parametr. W adresie URL **5-magazynu/szczegóły**, **identyfikator** zostanie potraktowany jako **5**.

1. Zmień **szczegóły** metody **StoreController**, dodając **int** parametr o nazwie **identyfikator**. Aby to zrobić, Zamień **szczegóły** metodę z następującym kodem:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

W tym zadaniu zostanie wypróbowanie aplikacji w przeglądarce sieci web i używać **identyfikator** parametru.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Projekt jest uruchamiany w **Home** strony. Zmień adres URL do */Store/Details/5* można zweryfikować, że akcja otrzyma parametru identyfikatora.

    ![Przeglądanie StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "przeglądanie StoreDetails5")

    *Przeglądanie /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Ćwiczenie 4: Tworzenie widoku

Do tej pory ma zostały zwracanie ciągów z akcji kontrolera. Chociaż jest to wygodny sposób zrozumienia, jak działają kontrolery, jest nie jak rzeczywistych aplikacji sieci Web są tworzone. Widoki są składnikami, które zapewniają lepszym rozwiązaniem dla generowania kodu HTML do przeglądarki przy użyciu plików szablonów.

W tym ćwiczeniu dowiesz się, jak dodać układ strony wzorcowej można skonfigurować szablon dla typowych zawartość HTML, arkusz stylów w celu zwiększenia wyglądu i działania witryny, a na koniec szablon widoku, aby włączyć HomeController do zwrócenia HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Zadanie 1 - modyfikowania pliku \_layout.cshtml

Plik **~/Views/Shared/\_layout.cshtml** pozwala skonfigurować szablon HTML wspólne korzystanie w całej witryny sieci Web. W tym zadaniu doda układ strony wzorcowej przy użyciu wspólnej nagłówka z łączami do obszar strony głównej i magazynu.

1. Jeśli nie już otwarty, uruchom **VS Express for Web**.
2. W **pliku** menu, wybierz **Otwórz projekt**. W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex04 CreatingAView\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**. Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Plik  **\_layout.cshtml** zawiera układ kontenera HTML dla wszystkich stron w witrynie. Obejmuje on  **&lt;html&gt;**  elementu w odpowiedzi HTML, jak również  **&lt;head&gt;**  i  **&lt;treści&gt;**  elementów. **@RenderBody()** w kodzie HTML treści identyfikowania regionów tego widoku szablonów będzie można wypełnić się przy użyciu zawartości dynamicznej.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Dodanie nagłówka wspólnego z łączami do obszar strony głównej i zapisać na wszystkich stron w witrynie. Aby to zrobić, Dodaj następujący kod poniżej &lt;treści&gt; instrukcji.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Obejmują div renderowanie części treści każdej strony. Zastąp  **@RenderBody()** następującym kodem higlighted: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Czy wiesz? Program Visual Studio 2012 ma fragmentów, które ułatwiają dodawania często używane kodu HTML i pliki kodu! Wypróbuj limit, wpisując  **&lt;div&gt;**  i naciskając klawisz **kartę** dwa razy, aby wstawić pełnego **div** tagu.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Zadanie 2 — Dodawanie arkusza stylów CSS

Pusty szablon projektu zawiera plik CSS bardzo prostsze, zawierający tylko stylów używany do wyświetlania podstawowych formularzy i komunikatów dotyczących sprawdzania poprawności. Aby poprawić wygląd i działanie witryny użyje dodatkowe CSS i obrazów (potencjalnie udostępnione przez projektanta).

To zadanie spowoduje dodanie arkusz stylów CSS do definiowania stylów witryny.

1. Pliku CSS i obrazy są uwzględnione w **Source\Assets\Content** folder tego laboratorium. Aby dodać je do aplikacji, przeciągnij ich zawartość z **Eksploratora Windows** okna do **Eksploratora rozwiązań** w programie Visual Studio Express for Web, jak pokazano poniżej:

    ![Przeciąganie styl treści](aspnet-mvc-4-fundamentals/_static/image12.png "przeciąganie styl treści")

    *Przeciąganie styl treści*
2. Okno dialogowe z ostrzeżeniem pojawi się pytaniem o potwierdzenie zastąpić **Site.css** plików i niektórych istniejących obrazów. Sprawdź **Zastosuj do wszystkich elementów** i kliknij przycisk **tak**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Zadanie 3. Dodawanie wyświetlanie szablonu

To zadanie spowoduje dodanie szablonu widoku do generowania odpowiedzi HTML, który będzie używany przez układ strony wzorcowej i CSS dodane w tym ćwiczeniu.

1. Aby użyć szablonu widoku podczas przeglądania strony głównej, najpierw musisz wskazują, że zamiast zwracać ciąg, **indeksu HomeController** metoda zwróci **widoku**. Otwórz **HomeController** klasy i zmień jego **indeksu** metodę, aby zwrócić **ActionResult**, i zwróć **View()**.

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu HomeController Ex4*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Teraz musisz dodać odpowiedni szablon widoku. Aby to zrobić, **kliknij prawym przyciskiem myszy** wewnątrz **indeksu** metody akcji i wybierz **Dodaj widok**. Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.

    ![Dodawanie widoku z w metodzie indeksu](aspnet-mvc-4-fundamentals/_static/image13.png "Dodawanie widoku z wewnątrz Index — metoda")

    *Dodawanie widoku z wewnątrz Index — metoda*
3. **Dodaj widok** wyświetli się okno dialogowe, aby wygenerować plik szablonu widoku. Domyślnie to okno dialogowe wstępnie wypełnia nazwę szablonu widoku, aby odpowiadały one metody akcji, która będzie go używać. Ponieważ użyto **Dodaj widok** menu kontekstowego w **indeksu** metody akcji w obrębie HomeController **Dodaj widok** okno dialogowe ma indeks jako domyślną nazwę widoku. Kliknij przycisk **Dodaj**.

    ![Okno dialogowe dodawania widoku](aspnet-mvc-4-fundamentals/_static/image14.png "okno dialogowe dodawania widoku")

    *Okno dialogowe dodawania widoku*
4. Visual Studio generuje **Index.cshtml** Wyświetl szablon wewnątrz **Views\Home** folder, a następnie go otwiera.

    ![Strona główna widok indeksu utworzony](aspnet-mvc-4-fundamentals/_static/image15.png "widok Home Indeks utworzony")

    *Macierzysty widok indeksu utworzony*

    > [!NOTE]
    > Nazwa i lokalizacja **Index.cshtml** plik ma zastosowanie i jest zgodna z domyślnych konwencji nazewnictwa platformy ASP.NET MVC.
    > 
    > Folder \Views\**Home** zgodna z nazwą kontrolera (**Home** kontrolera). Nazwa szablonu widoku (**indeksu**), metoda akcji kontrolera, które będą wyświetlane w widoku jest zgodna.
    > 
    > W ten sposób ASP.NET MVC pozwala uniknąć konieczności jawnego określania nazwy lub lokalizacji szablonu widoku, używając tę konwencję nazewnictwa do zwrócenia widoku.
5. Wygenerowany szablon widoku jest oparty na  **\_layout.cshtml** wcześniej zdefiniowanego szablonu. Zaktualizuj właściwość ViewBag.Title do **Home**i zmień głównej zawartości do **to strona główna**, jak pokazano w poniższym kodzie:


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Wybierz **MvcMusicStore** projekt w Eksploratorze rozwiązań i naciśnij klawisz **F5** do uruchomienia aplikacji.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Zadanie 4: Weryfikacja

Aby sprawdzić, prawidłowo wykonane wszystkie kroki opisane w poprzednim ćwiczeniu, wykonać następujące czynności:

Z tą aplikacją otwarta w przeglądarce należy zauważyć, że:

1. Metody akcji indeksu HomeController znaleźć i wyświetlić **\Views\Home\Index.cshtml** wyświetlić szablon, nawet jeśli kod wywołuje **zwracać View()**, ponieważ szablon widoku, a następnie standardowej konwencji nazewnictwa.
2. Strona główna wyświetla komunikat powitalny, zdefiniowanego w **\Views\Home\Index.cshtml** widok szablonu.
3. Strona główna używa  **\_layout.cshtml** szablonu, co komunikat powitalny jest zawarty w układu standardowe witryny HTML.

    ![Strona główna widoku indeksu przy użyciu zdefiniowanych LayoutPage i styl](aspnet-mvc-4-fundamentals/_static/image16.png "Home widoku indeksu przy użyciu zdefiniowanych LayoutPage i stylu")

    *Macierzysty widoku indeksu przy użyciu zdefiniowanych LayoutPage i stylu*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Ćwiczenie 5: Tworzenie modelu widoku

Do tej pory wprowadzono widoków wyświetlić zapisane na stałe HTML, ale, aby można było utworzyć dynamicznych aplikacji sieci web, Wyświetl szablon powinien zostać wyświetlony informacji od kontrolera. Jeden technika wspólnego do użycia w tym celu jest **ViewModel** wzorzec, dzięki czemu kontrolera spakować wszystkie informacje potrzebne do generowania odpowiednich odpowiedzi HTML.

W tym ćwiczeniu dowiesz Tworzenie klasy ViewModel i dodać wymagane właściwości: numer gatunkami muzyki w magazynie i listę tych gatunkami muzyki. Zostanie również zaktualizować StoreController do użycia ViewModel utworzony, a koniec utworzysz nowy szablon widoku, który spowoduje wyświetlenie właściwości wymienione na stronie.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Zadanie 1 — Tworzenie klasy ViewModel

W ramach tego zadania spowoduje utworzenie klasy ViewModel, która będzie wdrożyć scenariusz dla listy genre magazynu.

1. Jeśli nie już otwarty, uruchom **VS Express for Web**.
2. W **pliku** menu, wybierz **Otwórz projekt**. W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex05 CreatingAViewModel\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**. Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Utwórz **ViewModels** folder do przechowywania ViewModel. Aby to zrobić, kliknij prawym przyciskiem myszy najwyższego poziomu **MvcMusicStore** projektu, zaznacz **Dodaj** , a następnie **nowy Folder**.

    ![Dodawanie nowego folderu](aspnet-mvc-4-fundamentals/_static/image17.png "dodawanie nowy folder")

    *Dodawanie nowego folderu*
4. Nazwa folderu *ViewModels*.

    ![ViewModels folder w Eksploratorze rozwiązań](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder w Eksploratorze rozwiązań")

    *ViewModels folder w Eksploratorze rozwiązań*
5. Utwórz **ViewModel** klasy. Aby to zrobić, kliknij prawym przyciskiem myszy **ViewModels** folderu niedawno utworzona, wybierz **Dodaj** , a następnie **nowy element**. W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *StoreIndexViewModel.cs*, następnie kliknij przycisk **Dodaj**.

    ![Dodawanie nowej klasy](aspnet-mvc-4-fundamentals/_static/image19.png "Dodawanie nowej klasy")

    *Dodawanie nowej klasy*

    ![Tworzenie klasy StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel Tworzenie klasy")

    *Tworzenie klasy StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Zadanie 2 — Dodawanie właściwości do klasy ViewModel

Istnieją dwa parametry do przekazania z StoreController do szablonu widoku w celu wygenerowania oczekiwanej odpowiedzi HTML: numer gatunkami muzyki w magazynie i listę tych gatunkami muzyki.

To zadanie spowoduje dodanie tych właściwości 2 w celu **StoreIndexViewModel** klasy: **NumberOfGenres** (liczba całkowita) i **Genres** (lista ciągów).

1. Dodaj **NumberOfGenres** i **Genres** właściwości **StoreIndexViewModel** klasy. Aby to zrobić, Dodaj następujące wiersze 2 do definicji klasy:

    (Fragment - kodu *ASP.NET MVC 4 podstawy — właściwości Ex5 StoreIndexViewModel*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > **{Get; Ustaw;}**  notacji korzysta z C# w funkcji właściwości zaimplementowane automatycznie. Zapewnia korzyści właściwości bez konieczności nam zadeklarować polem zapasowym.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Zadanie 3 — aktualizowanie StoreController do użycia StoreIndexViewModel

**StoreIndexViewModel** klasa hermetyzuje informacje wymagane do przekazania z **StoreController**w **indeksu** metodę szablonu widoku w celu wygenerowania odpowiedzi .

To zadanie zaktualizuje **StoreController** do używania **StoreIndexViewModel**.

1. Otwórz **StoreController** klasy.

    ![Otwieranie klasy StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController otwierania — klasa")

    *Otwieranie StoreController klasy*
2. Aby można było używać **StoreIndexViewModel** klasę z **StoreController**, Dodaj następujący obszar nazw w górnej części **StoreController** kodu:

    (Fragment - kodu *ASP.NET MVC 4 podstawy — za pomocą ViewModels StoreIndexViewModel Ex5*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Zmień **StoreController**w **indeksu** metody akcji, którą tworzy i wypełnia **StoreIndexViewModel** obiektu i przekazuje ją do szablonu widoku w celu Generowanie odpowiedzi HTML z nim.

    > [!NOTE]
    > W laboratorium &quot;ASP.NET MVC modeli i dostęp do danych&quot; będzie pisania kodu, który pobiera listę gatunkami muzyki magazynu z bazy danych. W poniższym kodzie utworzysz **listy** gatunkami muzyki fikcyjny danych, które wypełnia **StoreIndexViewModel**.
    > 
    > Po utworzeniu i konfigurowanie **StoreIndexViewModel** obiektu, zostanie przekazany jako argument **widoku** metody. Oznacza to, że szablon widoku będzie używać tego obiektu do generowania odpowiedzi HTML z nim.
4. Zastąp **indeksu** metodę z następującym kodem:

    (Fragment - kodu *ASP.NET MVC 4 podstawy — metoda indeksu StoreController Ex5*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > Jeśli znasz C#, może założyć to przy użyciu **var** oznacza, że **viewModel** zmienna jest późnym wiązaniem. Nie jest to poprawna - kompilatora C# używa wnioskowanie typów w oparciu o przypisania do zmiennej do określenia, który **viewModel** jest typu **StoreIndexViewModel**. Ponadto uzyskując kompilowanie lokalnej **viewModel** zmiennej jako **StoreIndexViewModel** typ ma sprawdzanie kompilacji get i pomocy technicznej edytora kodu programu Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Zadanie 4 — Tworzenie szablonu widoku, który używa StoreIndexViewModel

W ramach tego zadania zostanie utworzony szablon widoku, który będzie używany obiekt StoreIndexViewModel przekazany z kontrolera do wyświetlenia listy gatunkami muzyki.

1. Przed utworzeniem nowego szablonu widoku, utworzymy projektu, aby **dialogowe dodawania widoku** zna **StoreIndexViewModel** klasy. Skompiluj projekt, wybierając **kompilacji** element menu, a następnie **kompilacji MvcMusicStore**.

    ![Tworzenie projektu](aspnet-mvc-4-fundamentals/_static/image22.png "skompilowanie projektu")

    *Tworzenie projektu*
2. Utwórz nowy szablon widoku. W tym celu kliknij prawym przyciskiem myszy wewnątrz **indeksu** metodę i wybierz **Dodaj widok**.

    ![Dodawanie widoku](aspnet-mvc-4-fundamentals/_static/image23.png "dodawania widoku")

    *Dodawanie widoku*
3. Ponieważ **dialogowe dodawania widoku** została wywołana z **StoreController**, spowoduje to dodanie Wyświetl szablon domyślny **\Views\Store\Index.cshtml** pliku. Sprawdź **utworzyć silnie wpisane — widok** pole wyboru, a następnie wybierz **StoreIndexViewModel** jako **klasa modelu**. Ponadto upewnij się, że wybrany aparat widoku jest **Razor**. Kliknij przycisk **Dodaj**.

    ![Okno dialogowe dodawania widoku](aspnet-mvc-4-fundamentals/_static/image24.png "okno dialogowe dodawania widoku")

    *Okno dialogowe dodawania widoku*

    **\Views\Store\Index.cshtml** widoku pliku szablonu jest utworzony i otwarty. Na podstawie informacji do **Dodaj widok** okna dialogowego w ostatnim kroku, widok szablonu będzie oczekiwać **StoreIndexViewModel** wystąpienia jako danych używany do generowania odpowiedzi HTML. Można zauważyć, że szablon dziedziczy `ViewPage<musicstore.viewmodels.storeindexviewmodel>` w języku C#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Zadanie 5 aktualizowanie szablonu widoku.

To zadanie zaktualizuje Wyświetl szablon utworzony w ostatnim zadaniem, aby pobrać liczbę genres i ich nazwy na stronie.

> [!NOTE]
> Użyjesz @ składni (często określany jako &quot;kodu nuggets&quot;) do wykonywania kodu w szablonie widoku.


1. W **Index.cshtml** pliku poziomu **magazynu** folderu, zastąp jego kod następującym kodem:


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > Zaraz po zakończeniu wpisywania okres, po słowie **modelu**, Intellisense programu Visual Studio wyświetli listę możliwych właściwości i metod do wyboru.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Pobieranie właściwości modelu i metody o IntelliSense programu Visual Studio*
    > 
    > **Modelu** odwołań do właściwości **StoreIndexViewModel** obiekt, który kontroler przekazanych do szablonu widoku. Oznacza to, że można uzyskać dostęp do wszystkich danych przekazanych z kontrolera do widoku szablonu za pomocą **modelu** właściwości i sformatuj go do odpowiedniego odpowiedzi HTML w szablonie widoku.
    > 
    > Można wybrać **NumberOfGenres** listy właściwości z funkcji Intellisense, zamiast wpisując je, a następnie go zostanie automatycznie uzupełniać go przez naciśnięcie przycisku **klawisza tab**.
2. Pętla za pośrednictwem listy genre w **StoreIndexViewModel** i tworzenia kodu HTML  **&lt;ul&gt;**  listy przy użyciu **foreach** pętli.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Naciśnij klawisz **F5** uruchomić aplikację, a następnie przejdź **/magazynu**. Zostanie wyświetlona lista gatunkami muzyki przekazano **StoreIndexViewModel** obiekt z **StoreController** do szablonu widoku.

    ![Wyświetlanie listy gatunkami muzyki widoku](aspnet-mvc-4-fundamentals/_static/image26.png "widoku wyświetlanie listy gatunkami muzyki")

    *Wyświetlanie listy gatunkami muzyki widoku*
4. Zamknij przeglądarkę.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Ćwiczenie 6: W widoku przy użyciu parametrów

Ćwiczenie 3 przedstawiono sposób przekazania parametrów do kontrolera. W tym ćwiczeniu dowiesz się, jak używać tych parametrów w szablonie widoku. W tym celu należy zostaną wprowadzone najpierw do klasy modeli, które ułatwiają zarządzanie logika danych i domeny. Ponadto dowiesz sposobu tworzenia łączy do stron w aplikacji ASP.NET MVC bez obaw rzeczy, takich jak kodowanie ścieżki adresu URL.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Zadanie 1 — Dodawanie klasy modelu

W odróżnieniu od ViewModels, które są tworzone tylko w celu przekazywania informacji z kontrolera do widoku, klasy modeli są tworzone zawierają i zarządzanie nimi logiki danych i domeny. W ramach tego zadania zostaną dodane dwie klasy modelu do reprezentowania tych pojęć: **Genre** i **albumu**.

1. Jeśli nie już otwarty, uruchom **VS Express for Web**
2. W **pliku** menu, wybierz **Otwórz projekt**. W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex06 UsingParametersInView\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**. Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Dodaj **Genre** klasa modelu. Aby to zrobić, kliknij prawym przyciskiem myszy **modele** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji. W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *Genre.cs*, następnie kliknij przycisk **Dodaj**.

    ![Dodawanie klasy](aspnet-mvc-4-fundamentals/_static/image27.png "Dodawanie klasy")

    *Dodawanie nowego elementu*

    ![Dodaj klasę modelu Genre](aspnet-mvc-4-fundamentals/_static/image28.png "Dodaj klasę modelu Genre")

    *Dodaj klasę modelu Genre*
4. Dodaj **nazwa** właściwości do klasy Genre. Aby to zrobić, Dodaj następujący kod:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 Genre*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Procedury jak wcześniej, Dodaj **albumu** klasy. Aby to zrobić, kliknij prawym przyciskiem myszy **modele** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji. W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *Album.cs*, następnie kliknij przycisk **Dodaj**.
6. Dodaj dwie właściwości do klasy albumu: **Genre** i **tytuł**. Aby to zrobić, Dodaj następujący kod:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - albumu Ex6*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Zadanie 2 — Dodawanie StoreBrowseViewModel

A **StoreBrowseViewModel** będzie używany w tym zadaniu pokazanie albumy, zgodne wybranego rodzaju. To zadanie będzie utworzyć tę klasę, a następnie dodaj dwie właściwości do obsługi **Genre** i jego **albumu**na liście.

1. Dodaj **StoreBrowseViewModel** klasy. Aby to zrobić, kliknij prawym przyciskiem myszy **ViewModels** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji. W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *StoreBrowseViewModel.cs*, następnie kliknij przycisk **Dodaj**.
2. Dodaj odwołanie do modeli w **StoreBrowseViewModel** klasy. Aby to zrobić, Dodaj następujący za pomocą przestrzeni nazw:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 UsingModel*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Dodaj dwie właściwości do **StoreBrowseViewModel** klasy: **Genre** i **albumów**. Aby to zrobić, Dodaj następujący kod:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 ModelProperties*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > Co to jest **listy&lt;albumu&gt;**  ?: używa tej definicji **listy&lt;T&gt;**  typu, których **T** ogranicza Typ, na których elementy tego **listy** należy w tym przypadku **albumu** (lub któregokolwiek z jej obiektów podrzędnych).
    > 
    > Ta możliwość projektowania klasy i metody, które mają być odroczone specyfikację jeden lub więcej typów, aż klasa lub metoda jest zadeklarowany i utworzone przez kod klienta jest funkcją języka C# o nazwie **ogólne**.
    > 
    > **Lista&lt;T&gt;**  ogólny odpowiednik **ArrayList** typu i jest dostępny w **system.Collections.Generic —** przestrzeni nazw. Jedną z korzyści wynikające ze stosowania **ogólne** jest, że ponieważ typem jest określona, nie trzeba zajmie się sprawdzanie operacji, takich jak elementy do rzutowania typu **albumu** jak z **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Zadanie 3 - w StoreController przy użyciu nowego ViewModel

To zadanie, należy zmodyfikować **StoreController**w **Przeglądaj** i **szczegóły** metod akcji do używania nowych **StoreBrowseViewModel** .

1. Dodaj odwołanie do folderu modeli w **StoreController** klasy. Aby to zrobić, rozwiń węzeł **kontrolerów** folderu w **Eksploratora rozwiązań** , a następnie otwórz **StoreController** klasy. Następnie dodaj następujący kod:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 UsingModelInController*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Zastąp **Przeglądaj** metody akcji, aby użyć **StoreViewBrowseController** klasy. Utworzysz określonego rodzaju i dwa nowe obiekty albumów fikcyjny danych (w następnej laboratorium Hands-on będą korzystać prawdziwe dane z bazy danych). Aby to zrobić, Zamień **Przeglądaj** metodę z następującym kodem:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Zastąp **szczegóły** metody akcji, aby użyć **StoreViewBrowseController** klasy. Utworzy nowy **albumu** obiekt ma zostać zwrócona do **widoku**. Aby to zrobić, Zamień **szczegóły** metodę z następującym kodem:

    (Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Zadanie 4 — Dodawanie szablonu widoku przeglądania

To zadanie spowoduje dodanie **Przeglądaj** widok, aby pokazać albumów znaleziono dla określonego rodzaju.

1. Przed utworzeniem nowego szablonu widoku, należy skompilować projekt, aby **Dodaj widok** okna dialogowego zna **ViewModel** klasy do użycia. Skompiluj projekt, wybierając **kompilacji** element menu, a następnie **kompilacji MvcMusicStore**.
2. Dodaj **Przeglądaj** widoku. Aby to zrobić, kliknij prawym przyciskiem myszy w **Przeglądaj** metody akcji **StoreController** i kliknij przycisk **Dodaj widok**.
3. W **Dodaj widok** okna dialogowego upewnij się, że nazwa widoku jest **Przeglądaj**. Sprawdź **utworzyć widok jednoznacznie** wyboru i wybierz **StoreBrowseViewModel** z **klasa modelu** listy rozwijanej. Pozostaw wartości domyślne innych pól. Następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku przeglądania](aspnet-mvc-4-fundamentals/_static/image29.png "Dodawanie widoku przeglądania")

    *Dodawanie widoku przeglądania*
4. Modyfikowanie **Browse.cshtml** do wyświetlania informacji Genre, uzyskiwanie dostępu do **StoreBrowseViewModel** obiekt, który jest przekazywany do szablonu widoku. Aby to zrobić, Zastąp zawartość następującym kodem: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

W tym zadaniu zostanie sprawdzić, czy **Przeglądaj** metoda pobiera albumów z **Przeglądaj** metody akcji.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do   **/przechowywania/Przeglądaj? Genre = Disco** Aby sprawdzić, czy akcji zwraca dwa albumów.

    ![Przeglądanie magazynu albumów Disco](aspnet-mvc-4-fundamentals/_static/image30.png "przeglądanie albumów Disco magazynu")

    *Przeglądanie albumów Disco magazynu*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Zadanie 6 — wyświetlanie informacji o określonych albumu

W tym zadaniu zostaną zaimplementowane **szczegóły dotyczące i magazynu** widok, aby wyświetlić informacje o określonym albumu. W tym laboratorium Hands-on wszystko, co spowoduje wyświetlenie o album znajduje się już w **widoku** szablonu. Tak, zamiast tworzyć **StoreDetailsViewModel** klasy, użyje bieżącego **StoreBrowseViewModel** szablonu przekazywanie Album do niego.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Dodaj nową **szczegóły** wyświetlić **StoreController**w **szczegóły** metody akcji. Aby to zrobić, kliknij prawym przyciskiem myszy **szczegóły** metody w **StoreController** klasy, a następnie kliknij przycisk **Dodaj widok**.
2. W **Dodaj widok** okna dialogowego, upewnij się, że **nazwy widoku** jest **szczegóły**. Sprawdź **utworzyć widok jednoznacznie** wyboru i wybierz **albumu** z **klasa modelu** listy rozwijanej. Pozostaw wartości domyślne innych pól. Następnie kliknij przycisk **Dodaj**. Spowoduje to utworzenie i otworzyć **\Views\Store\Details.cshtml** pliku.

    ![Dodawanie widoku szczegółów](aspnet-mvc-4-fundamentals/_static/image31.png "Dodawanie widoku szczegółów")

    *Dodawanie widoku szczegółów*
3. Modyfikowanie **Details.cshtml** plik, aby wyświetlić informacje albumu podczas uzyskiwania dostępu do **albumu** obiekt, który jest przekazywany do szablonu widoku. Aby to zrobić, Zastąp zawartość następującym kodem: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Zadanie 7 - uruchamiania aplikacji

W tym zadaniu zostanie sprawdzić, czy **szczegóły** widok pobiera informacje albumu z **szczegóły akcji** metody.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Projekt jest uruchamiany w **Home** strony. Zmień adres URL do **/Store/Details/5** do sprawdzenia informacji o albumu.

    ![Przeglądanie szczegółów albumów](aspnet-mvc-4-fundamentals/_static/image32.png "przeglądania szczegółów albumów")

    *Album przeglądania szczegółów*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Zadanie 8 — Dodawanie łącza między stronami

To zadanie będzie dodanie łącza w widoku magazynu w celu zapewnienia łącze każdego rodzaju nazwy do odpowiedniego **/magazynu/Przeglądaj** adresu URL. Dzięki temu po kliknięciu określonego rodzaju, na przykład **Disco**, spowoduje przejście do **/magazynu/Przeglądaj? genre = Disco** adresu URL.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Aktualizacja **indeksu** stronę, aby dodać łącze do **Przeglądaj** strony. Aby to zrobić, w **Eksploratora rozwiązań** rozwiń węzeł **widoków** folderu, a następnie **magazynu** folder i kliknij dwukrotnie **Index.cshtml** strony.
2. Dodaj łącze do widoku przeglądania wskazujący genre wybrane. Aby to zrobić, Zamień następujące wyróżniony kod w  **&lt;li&gt;**  tagi: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > innym rozwiązaniem będzie łączenie bezpośrednio do strony, przy użyciu kodu podobne do poniższych:
    > 
    > &lt;href =&quot;/magazynu/Przeglądaj? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;
    > 
    > Chociaż to rozwiązanie działa, jest zależna od ciągu zapisane na stałe. Później w przypadku zmiany nazwy kontrolera, należy ręcznie zmienić tej instrukcji. Lepszym jest użycie **pomocnika kodu HTML** metody. Platforma ASP.NET MVC zawiera metodę pomocnika kodu HTML, która jest dostępna dla takich zadań. **Html.ActionLink()** metody pomocnika ułatwia tworzenie HTML  **&lt;&gt;**  łącza, upewniając się, ścieżki adresu URL są poprawnie zakodowane w adresie URL.
    > 
    > Htlm.ActionLink ma kilka przeciążeń. W tym ćwiczeniu będzie używany, który przyjmuje trzy parametry:
    > 
    > 1. Tekst łącza, która będzie wyświetlana nazwa Genre
    > 2. Nazwa akcji kontrolera (**Przeglądaj**)
    > 3. Wartości parametru, określając nazwę trasy (**Genre**) i wartość (**nazwa Genre**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Zadanie 9 — uruchamianie aplikacji

W tym zadaniu zostanie przetestować, czy każdego rodzaju są wyświetlane z łączem do odpowiedniej **/magazynu/Przeglądaj** adresu URL.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/magazynu** można zweryfikować, że każdego rodzaju linki do odpowiednich **/magazynu/Przeglądaj** adresu URL.

    ![Przeglądanie Genres wraz z łączami do przeglądania strony](aspnet-mvc-4-fundamentals/_static/image33.png "przeglądanie Genres wraz z łączami do przeglądania strony")

    *Przeglądanie Genres wraz z łączami do przeglądania strony*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Zadanie 10 - przy użyciu kolekcji dynamicznych ViewModel do przekazania wartości

W tym zadaniu dowiesz się, prosta i skuteczna metoda przekazać wartości między kontrolerem a widokiem bez wprowadzania żadnych zmian w modelu. ASP.NET MVC 4 udostępnia kolekcję &quot;ViewModel&quot;, co przypisane do żadnej wartości dynamiczne i dostęp do wewnątrz również widoków i kontrolerów.

Teraz użyje kolekcji dynamicznych elementów ViewBag do przekazania listę &quot; **Starred genres** &quot; z kontrolera do widoku. Widok indeksu magazynu będą uzyskiwać dostęp do **ViewModel** i wyświetlić informacje.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Otwórz **StoreController.cs** i zmodyfikuj **indeksu** metodę w celu utworzenia listy starred genres do kolekcji ViewModel:


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Można również użyć składni **obiekt ViewBag [&quot;Starred&quot;]** uzyskać dostęp do właściwości.
2. Ikonę gwiazdki  **&quot;starred.png&quot;**  znajduje się w **Source\Assets\Images** folder tego laboratorium. Aby dodać go do aplikacji, przeciągnij ich zawartość z **Eksploratora Windows** okna do **Eksploratora rozwiązań** w Visual Web Developer Express, jak pokazano poniżej:

    ![Dodawanie obrazu gwiazdy do rozwiązania](aspnet-mvc-4-fundamentals/_static/image34.png "Dodawanie gwiazdy obrazu do rozwiązania")

    *Dodawanie obrazu gwiazdy do rozwiązania*
3. Otwórz widok **Store/Index.cshtml** i modyfikować zawartość. Będzie odczytywał &quot;starred&quot; właściwości w **obiekt ViewBag** kolekcji i poproś, jeśli bieżąca nazwa genre znajduje się na liście. W takim przypadku zostaną wyświetlone ikonę gwiazdki prawo do łącza rodzaju.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Zadanie 11 - uruchamiania aplikacji

W tym zadaniu zostanie przetestować, że jak pokazano genres wyświetlać ikonę gwiazdki.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Projekt jest uruchamiany w **Home** strony. Zmień adres URL do **/magazynu** do sprawdzenia, czy każdego rodzaju polecanych ma wagę do poszanowania etykiety:

    ![Przeglądanie Genres elementów jak pokazano na](aspnet-mvc-4-fundamentals/_static/image35.png "przeglądanie Genres elementów jak pokazano na")

    *Przeglądanie Genres elementów jak pokazano na*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Ćwiczenie 7: Laptop wokół nowego szablonu platformy ASP.NET MVC 4

W tym ćwiczeniu zostanie Poznaj ulepszenia w szablonach projektu programu ASP.NET MVC 4, biorąc spojrzeć na najbardziej istotne cechy nowy szablon.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Zadanie 1: Eksploracji szablon aplikacji internetowych platformy ASP.NET MVC 4

1. Jeśli nie już otwarty, uruchom **VS Express for Web**
2. Wybierz **pliku | Nowy | Projekt** polecenia menu. W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w lewym okienku drzewa, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 4**. **Nazwa** projektu *MusicStore* i zaktualizuj **Nazwa rozwiązania** do *rozpocząć*, następnie wybierz lokalizację (lub pozostaw wartość domyślną) i kliknij przycisk **OK** .

    ![Tworzenie nowego projektu platformy ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "tworzenia nowego projektu platformy ASP.NET MVC 4")

    *Tworzenie nowego projektu platformy ASP.NET MVC 4*
3. W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** szablonu projektu i kliknij przycisk **OK**. Należy zauważyć, że można wybrać jako aparat widoku Razor lub ASPX.

    ![Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC](aspnet-mvc-4-fundamentals/_static/image37.png "Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC")

    *Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC*

    > [!NOTE]
    > Składnia razor została wprowadzona w programie ASP.NET MVC 3. Jego celem jest zminimalizowanie liczby znaków i naciśnięcia klawiszy wymagane w pliku, umożliwiające szybkie oraz płynu kodowania przepływu pracy. Razor wykorzystuje istniejące C# /VB (lub innych) umiejętności język, a następnie dostarcza składnia znacznika szablonu, umożliwiającą świetny przepływu pracy konstrukcji kodu HTML.
4. Naciśnij klawisz **F5** uruchomić rozwiązanie i zobaczyć odnowionego szablonu. Można wyewidencjonować następujące funkcje:

    1. **Szablonów w stylu Modern**

        Szablony odnowienia, zapewniając więcej stylów nowoczesny wygląd.

        ![Szablony ASP.NET MVC 4 restyled](aspnet-mvc-4-fundamentals/_static/image38.png "restyled szablony programu ASP.NET MVC 4")

        *Szablony ASP.NET MVC 4 restyled*
    2. **Renderowanie adaptacyjną**

        Zapoznaj się z zmiany rozmiaru okna przeglądarki i zwróć uwagę, jak dynamicznie dostosowuje układ strony do nowego rozmiaru okna. Te szablony używać techniki adaptacyjną renderowania mają być renderowane poprawnie w zarówno wersje desktop i mobile platform bez potrzeby dostosowywania.

        ![Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiary](aspnet-mvc-4-fundamentals/_static/image39.png "szablonu projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach")

        *Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach*
5. Zamknij przeglądarkę, aby zatrzymać debuger i powrócić do programu Visual Studio.
6. Teraz masz możliwość Eksploruj rozwiązania i zapoznaj się z nowych funkcji wprowadzonych przez program ASP.NET MVC 4 w szablonie projektu.

    ![ASP.NET MVC4-internet-— szablon projektu aplikacji-](aspnet-mvc-4-fundamentals/_static/image40.png "szablonu projektu aplikacji internetowych platformy ASP.NET MVC 4")

    *Szablon projektu aplikacji internetowych platformy ASP.NET MVC 4*

    1. **HTML5 znaczników**

        Przeglądaj widoków szablonu, aby dowiedzieć się nowego znacznika motywu, na przykład otwórz **About.cshtml** wyświetlić w **Home** folderu.

        ![Nowy szablon, przy użyciu znaczników Razor i HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nowego szablonu, za pomocą znacznika Razor i HTML5")

        *Nowy szablon, przy użyciu znaczników Razor i HTML5*
    2. **JavaScript biblioteki dołączony**

        1. **jQuery**: jQuery ułatwia przechodzenie dokumentu HTML, obsługa zdarzeń animacji i interakcje Ajax.
        2. **interfejsu użytkownika jQuery**: Ta biblioteka zawiera obiektów abstrakcyjnych odpowiadających niskiego poziomu interakcji i zaawansowana efekty animacji i elementów WebParts elementy widget, rozszerzający jQuery biblioteka języka JavaScript.

            > [!NOTE]
            > Informacje na temat jQuery i jQuery interfejsu użytkownika w [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).
        3. **Elementami KnockoutJS**: szablon domyślny ASP.NET MVC 4 zawiera teraz **elementami KnockoutJS**, platforma JavaScript MVVM, która umożliwia tworzenie aplikacji sieci web bogaty i elastyczny wysokiej przy użyciu języka JavaScript i HTML. Podobnie jak w programie ASP.NET MVC 3, jQuery i jQuery biblioteki interfejsu użytkownika znajdują się również w technologii ASP.NET MVC 4.

            > [!NOTE]
            > Możesz uzyskać więcej informacji na temat biblioteki elementami KnockOutJS w to łącze: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).
        4. **Modernizr**: Ta biblioteka jest uruchamiana automatycznie, co zgodne ze starszych przeglądarek witryny przy użyciu technologii HTML5 i CSS3.

            > [!NOTE]
            > Możesz uzyskać więcej informacji na temat biblioteki Modernizr w to łącze: [http://www.modernizr.com/](http://www.modernizr.com/).
    3. **SimpleMembership zawarty w rozwiązaniu**

        SimpleMembership został zaprojektowany jako serwer zamienny dla poprzedniego systemu dostawcy ról ASP.NET i członkostwa. Ma wiele nowych funkcji, które ułatwiają deweloperom bezpiecznych stron sieci web w sposób bardziej elastyczne.

        Szablon Internet już ma skonfigurować kilka ustawień integracji SimpleMembership, na przykład elementu AccountController jest przygotowane do korzystania z OAuthWebSecurity (w przypadku rejestracji konta OAuth, logowania, zarządzania, itp.) i bezpieczeństwo sieci Web.

        ![SimpleMembership zawarty w rozwiązaniu](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership zawarty w rozwiązaniu")

        *SimpleMembership zawarty w rozwiązaniu*

        > [!NOTE]
        > Aby uzyskać więcej informacji o [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) w witrynie MSDN.

> [!NOTE]
> Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Wykonując tego laboratorium Hands-On znasz już podstawy ASP.NET MVC:

- Elementy podstawowe aplikacji MVC i sposób ich interakcji
- Tworzenie aplikacji platformy ASP.NET MVC
- Jak dodać i skonfigurować kontrolerów, aby obsłużyć parametry przekazywane przy użyciu adresu URL i querystring
- Jak dodać układ strony wzorcowej można skonfigurować szablon dla typowych zawartość HTML, arkusz stylów, aby zwiększyć wyglądu i działania i szablonu widoku, aby wyświetlić zawartość HTML
- Sposób użycia wzorca ViewModel przekazywania właściwości do szablonu widoku do wyświetlenia informacji dynamicznych
- Jak używać parametrów przekazanych do kontrolerów w szablonie widoku
- Dodawanie łączy do stron w aplikacji ASP.NET MVC
- Jak dodać i używania właściwości dynamicznych w widoku
- Ulepszenia w szablonach projektu programu ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji  **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK*&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Instalowanie programu Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instalacji programu Visual Studio Express")

    *Instalowanie programu Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-fundamentals/_static/image47.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure

1. Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu. Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Zaloguj się do portalu Windows Azure")

    *Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*
2. Kliknij przycisk **nowy** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-fundamentals/_static/image49.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczeniowe** | **witryny sieci Web**. Następnie wybierz **szybkie tworzenie** opcji. Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.

    > [!NOTE]
    > Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](aspnet-mvc-4-fundamentals/_static/image50.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj na nowe **witryny sieci Web** jest tworzony.
5. Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny. Sprawdź, czy działa nowej witryny sieci Web.

    ![Przeglądanie do nowej witryny sieci web](aspnet-mvc-4-fundamentals/_static/image51.png "przeglądanie do nowej witryny sieci web")

    *Przeglądanie do nowej witryny sieci web*

    ![Witryna sieci Web działa](aspnet-mvc-4-fundamentals/_static/image52.png "uruchamiania witryny sieci Web")

    *Witryna sieci Web uruchomiona*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie stron witryny sieci web zarządzania](aspnet-mvc-4-fundamentals/_static/image53.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie stron zarządzania witryny sieci Web*
7. W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.

    > [!NOTE]
    > *Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.

    ![Pobieranie witryny sieci web profilu publikowania](aspnet-mvc-4-fundamentals/_static/image54.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz profil publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.

    ![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-fundamentals/_static/image55.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji. Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**. Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania. Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.

    ![Pulpit nawigacyjny serwera bazy danych SQL](aspnet-mvc-4-fundamentals/_static/image56.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny serwera bazy danych SQL*
2. W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) przycisku.

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.

    ![Potwierdzenie zmian](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Potwierdzenie zmian*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.

    ![Publikowanie aplikacji](aspnet-mvc-4-fundamentals/_static/image60.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.

    ![Importowanie profilu publikowania](aspnet-mvc-4-fundamentals/_static/image61.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **Weryfikacja połączenia z**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.

    ![Sprawdzanie poprawności połączenia](aspnet-mvc-4-fundamentals/_static/image62.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-fundamentals/_static/image63.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

    - W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.
    - W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.
    - W **hasło** wpisz hasło logowania administratora serwera.
    - Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.

    ![Konfigurowanie parametrów połączenia z lokalizacją docelową](aspnet-mvc-4-fundamentals/_static/image64.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")

    *Konfigurowanie parametrów połączenia z lokalizacją docelową*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.

    ![Tworzenie bazy danych](aspnet-mvc-4-fundamentals/_static/image65.png "tworzenie parametry bazy danych")

    *Tworzenie bazy danych*
7. Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie. Następnie kliknij przycisk **Dalej**.

    ![Parametry połączenia wskazujące bazę danych SQL](aspnet-mvc-4-fundamentals/_static/image66.png "ciąg połączenia wskazujące bazę danych SQL")

    *Parametry połączenia wskazujące bazę danych SQL*
8. W **Podgląd** kliknij przycisk **publikowania**.

    ![Publikowanie aplikacji sieci web](aspnet-mvc-4-fundamentals/_static/image67.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

    ![Aplikacja opublikowana w systemie Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "aplikacji publikowanych w systemie Windows Azure")

    *Aplikacji publikowanych w systemie Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: korzystania z wstawek kodu

Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki. Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.

![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-fundamentals/_static/image69.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*

***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragment (bez spacji i łączniki).
3. Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.
4. Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-fundamentals/_static/image70.png "zacznij wpisywać nazwę wstawki programu")

*Rozpocznij wpisywanie nazwy fragment kodu*

![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-fundamentals/_static/image71.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-fundamentals/_static/image72.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")

*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*

***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-fundamentals/_static/image73.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")

*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-fundamentals/_static/image74.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
