---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Pomocników platformy ASP.NET MVC 4, formularzy i sprawdzania poprawności | Dokumentacja firmy Microsoft
author: rick-anderson
description: ASP.NET MVC 4 modeli i laboratorium Hands-on dostępu do danych możesz zostały ładowanie i wyświetlanie danych z bazy danych. W tym laboratorium Hands-on doda do...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 4cfa98144919c3f1bdb3608970af1a7952fe6ea7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Pomocników platformy ASP.NET MVC 4, formularzy i sprawdzania poprawności

Przez [obozów sieci Web Team](https://twitter.com/webcamps)

[Pobierz obozów sieci Web uczenie Kit](https://aka.ms/webcamps-training-kit)

W **ASP.NET MVC 4 modeli i dostęp do danych** laboratorium Hands-on nastąpiło ładowanie i wyświetlanie danych z bazy danych. W tym laboratorium Hands-on doda do **sklep muzyczny** aplikacji możliwość edytowania tych danych.

Tego celu pamiętać najpierw utworzysz kontroler, który będzie obsługiwać albumy akcje tworzenia, odczytu, aktualizacji i usuwania (CRUD). Wygeneruje szablonu widoku indeksu, korzystając z funkcji szkieletów ASP.NET MVC do wyświetlania właściwości albumów w tabeli HTML. Aby zwiększyć ten widok, zostaną dodane niestandardowe pomocnika kodu HTML, który obetnie długie opisy.

Następnie możesz spowoduje to dodanie edycji i Utwórz widoki, które pozwoli instrukcja alter albumów w bazie danych za pomocą formularza elementy, takie jak listę rozwijaną.

Ponadto umożliwi użytkownikowi usunięcie albumu i również uniemożliwi ich wprowadzania danych niewłaściwy przez sprawdzanie poprawności danych wejściowych.

W tym laboratorium Hands-on zakłada mieć podstawową wiedzę na temat **ASP.NET MVC**. Jeśli nie używasz **ASP.NET MVC** przed, zalecamy zapoznać się z **podstawowe informacje na temat platformy ASP.NET MVC** Hands-on laboratorium.

W tym laboratorium przeprowadzi Cię przez rozszerzenia oraz nowe funkcje opisane wcześniej przez zastosowanie drobne zmiany do przykładowej aplikacji sieci Web w folderze źródłowym.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [wersje Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Specyficzne dla tego laboratorium projektu jest dostępna na [pomocników programu ASP.NET MVC 4, formularzy i sprawdzania poprawności](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium Hands-On przedstawiono sposób:

- Tworzenie kontrolera w celu obsługi operacji CRUD
- Generowanie widoku indeksu, aby wyświetlić właściwości jednostki tabeli HTML
- Dodaj niestandardowy pomocnika kodu HTML
- Tworzenie i dostosowywanie Edytowanie widoku
- Rozróżnienie metod akcji, które reagują na HTTP GET lub POST protokołu HTTP wywołania
- Dodawanie i dostosowywanie Utwórz widok
- Dojście usunięcie jednostki
- Sprawdzanie poprawności danych wejściowych użytkownika

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Musi mieć następujące elementy do przygotowania tego laboratorium:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

**Instalowanie wstawki kodu**

Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio. Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.

Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek B:](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

Następujące ćwiczeń tworzą tego laboratorium Hands-On:

1. [Tworzenie kontrolera Menedżer magazynu i jego widoku indeksu](#Exercise1)
2. [Dodawanie pomocnika kodu HTML](#Exercise2)
3. [Tworzenie widoku edycji](#Exercise3)
4. [Dodawanie widoku Create](#Exercise4)
5. [Obsługa usuwania](#Exercise5)
6. [Dodawanie weryfikacji](#Exercise6)
7. [Przy użyciu jQuery dyskretny kod po stronie klienta](#Exercise7)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach. Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.


Szacowany czas trwania tego laboratorium: **60 minut**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Ćwiczenie 1: Tworzenie kontrolera Menedżer magazynu i jego widoku indeksu

W tym ćwiczeniu dowiesz sposób tworzenia nowego kontrolera w celu obsługi operacji CRUD, można dostosować jego indeksu metody akcji, aby zwrócić listę albumów z bazy danych i na koniec generowania szablon widoku indeksu wykorzystaniu szkieletów ASP.NET MVC Funkcja, aby wyświetlić właściwości albumów w tabeli HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Zadanie 1 — Tworzenie StoreManagerController

W ramach tego zadania spowoduje utworzenie nowego kontrolera o nazwie **StoreManagerController** do obsługi operacji CRUD.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex1-CreatingTheStoreManagerController/Begin/** folderu.

   1. Należy pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Dodaj nowy kontroler. Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań wybierz **Dodaj** , a następnie **kontrolera** polecenia. Zmień **kontrolera** **nazwa** do **StoreManagerController** i upewnij się, że opcja **kontroler MVC z akcjami odczytu/zapisu pusty**jest zaznaczone. Kliknij przycisk **Dodaj**.

    ![Dodaj kontroler w oknie dialogowym](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Dodaj kontroler w oknie dialogowym")

    *Dodawanie kontrolera okna dialogowego*

    Nowa klasa kontrolera jest generowany. Ponieważ wskazane Dodaj akcje dla odczytu/zapisu, metod klasy zastępczej dla osób, typowych operacji CRUD są tworzone z komentarze TODO wypełnione, monitowanie Dołącz logikę określonych aplikacji.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Zadanie 2 — dostosowanie indeksu StoreManager

W tym zadaniu zostanie dostosować indeksu StoreManager metody akcji do zwrócenia widoku z listy albumy z bazy danych.

1. W klasie StoreManagerController, Dodaj następujący *przy użyciu* dyrektywy.

    (Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex1 przy użyciu MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Dodawanie pola do **StoreManagerController** do przechowywania wystąpienia **MusicStoreEntities.**

    (Fragment - kodu *pomocników platformy ASP.NET MVC 4 oraz formularzy i sprawdzanie poprawności — Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implementuje akcji indeksu StoreManagerController do zwrócenia widoku z listy Albumy.

    Logika akcji kontrolera jest bardzo podobny do akcji indeksu StoreController zapisane wcześniej. Użyj rozszerzenia LINQ, można pobrać wszystkich albumów, w tym informacje o rodzaju i wykonawcy do wyświetlenia.

    (Fragment - kodu *pomocników platformy ASP.NET MVC 4 oraz formularzy i sprawdzanie poprawności — indeks StoreManagerController Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Zadanie 3 — Tworzenie widoku indeksu

W ramach tego zadania spowoduje utworzenie szablonu widoku indeksu do wyświetlenia na liście Albumy zwrócony przez **StoreManager** kontrolera.

1. Przed utworzeniem nowego szablonu widoku, należy skompilować projekt, aby **dialogowe dodawania widoku** zna **albumu** klasy do użycia. Wybierz **kompilacji | Tworzenie MvcMusicStore** Aby skompilować projekt.
2. Kliknij prawym przyciskiem myszy wewnątrz **indeksu** metody akcji i wybierz **Dodaj widok**. Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.

    ![Dodaj widok](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Dodawanie widoku")

    *Dodawanie widoku z wewnątrz Index — metoda*
3. W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **indeksu**. Wybierz **utworzyć widok jednoznacznie** i wybrać **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej. Wybierz **listy** z **szablonu szkieletu** listy rozwijanej. Pozostaw **aparat widoku** do **Razor** i innych pól z ich domyślną wartość, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widok indeksu](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Dodawanie widok indeksu")

    *Dodawanie widok indeksu*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Zadanie 4 — Dostosowywanie szkieletu widoku indeksu

W tym zadaniu zostanie dostosowana prostego szablonu Widok utworzony za pomocą funkcji szkieletów ASP.NET MVC ona żądane pola.

> [!NOTE]
> **Szkieletów** wsparcia w ramach platformy ASP.NET MVC generuje prosty szablon widoku, który wyświetla wszystkie pola w modelu albumu. **Tworzenia szkieletu** umożliwia szybkie rozpoczęcie pracy na silnie typizowanego widoku: zamiast ręcznie zapisać szablon widoku, tworzenia szkieletu szybko generuje szablon domyślny, a następnie można zmodyfikować wygenerowanego kodu.


1. Przejrzyj kod utworzony. Generowanie listy pól będzie częścią następujące tabeli HTML, który **szkieletów** używanej do wyświetlania danych tabelarycznych.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Zastąp **&lt;tabeli&gt;** kodu z następującym kodem, które mają być wyświetlane tylko **Genre**, **wykonawcy**, **tytuł**, i **cen** pola. Spowoduje to usunięcie **AlbumId** i **albumów graficznych URL** kolumn. Ponadto zmienia GenreId i ArtistId kolumny do wyświetlenia ich właściwości klasy połączonego **Artist.Name** i **Genre.Name**i usuwa **szczegóły** łącza.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Zmień poniższe opisy.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

W tym zadaniu zostanie sprawdzić, czy **StoreManager** **indeksu** szablon widoku zostanie wyświetlona lista albumów zgodnie z projektem z poprzednich kroków.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/StoreManager** Aby sprawdzić, czy zostanie wyświetlona lista albumy, przedstawiający ich **tytuł**, **wykonawcy** i **Genre**.

    ![Przeglądanie listy albumów](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "przeglądania listy albumów")

    *Przeglądanie listy albumów*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Ćwiczenie 2: Dodawanie pomocnika kodu HTML

Strona indeksu StoreManager ma jeden potencjalny problem: tytułu i nazwy wykonawcy właściwości mogą jednocześnie być wystarczająco długie, by wyrzucanie formatowanie tabeli. W tym ćwiczeniu dowiesz się, jak dodać niestandardowe pomocnika kodu HTML do obcięcia tekst.

Na poniższej ilustracji widać, jak zmienić format ze względu na długość tekstu użycia rozmiar małych przeglądarki.

![Nie przeglądania listy albumów z obcięte tekst](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "nie przeglądania listy albumów z obcięte tekstu")

*Nie przeglądania listy albumów z obcięte tekstu*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Zadanie 1 - rozszerzanie pomocnika kodu HTML

To zadanie spowoduje dodanie nowej metody **Truncate** do **HTML** obiektu w widoków MVC ASP.NET. Aby to zrobić, zostaną zaimplementowane **— metoda rozszerzenia** do wbudowanej **System.Web.Mvc.HtmlHelper** klasy przez platformę ASP.NET MVC.

> [!NOTE]
> Aby dowiedzieć się więcej o **metody rozszerzenia**, odwiedź ten artykuł w witrynie msdn. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex2-AddingAnHTMLHelper/Begin/** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

   1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz widok indeksu StoreManager firmy. Aby to zrobić, w Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **StoreManager** , a następnie otwórz **Index.cshtml** pliku.
3. Dodaj następujący kod poniżej <strong>@model</strong> dyrektywy do definiowania <strong>Truncate</strong> metody pomocnika.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Zadanie 2 — obcinanie tekstu na stronie

W tym zadaniu użyjesz **Truncate** metody obcięcia tekst w szablonie widoku.

1. Otwórz widok indeksu StoreManager firmy. Aby to zrobić, w Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **StoreManager** , a następnie otwórz **Index.cshtml** pliku.
2. Zamień wiersze, które zawierają **nazwy wykonawcy** i albumu **tytuł**. Aby to zrobić, Zamień następujące wiersze.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — uruchamianie aplikacji

W tym zadaniu zostanie sprawdzić, czy **StoreManager** **indeksu** Wyświetl szablon obcina tytułu i nazwy wykonawcy albumu.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/StoreManager** zweryfikowanie, że long teksty w **tytuł** i **wykonawcy** kolumny są obcinane.

    ![Obcięty nazwy tytułów i artystów](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "obcięty tytułów i artystów nazwy")

    *Skrócona tytułów i nazwy wykonawców*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Ćwiczenie 3: Tworzenie widoku edycji

W tym ćwiczeniu dowiesz się, jak utworzyć formularz umożliwia menedżerów magazynu do edycji albumu. Będzie przeglądania **/StoreManager/Edit/id** adresu URL (**identyfikator** jest unikatowy identyfikator albumy, aby edytować), dzięki czemu wywołanie HTTP GET do serwera.

Metody akcji kontrolera Edytuj będzie pobrać odpowiednią Album z bazy danych, Utwórz **StoreManagerViewModel** obiekt, aby hermetyzować go (wraz z listy artystów i Genres), a następnie przekazać go do szablonu widoku w celu renderowanie strony HTML do użytkownika. Ta strona będzie zawierać **&lt;formularza&gt;** element z pola tekstowe i listę rozwijaną do edycji właściwości albumu.

Po aktualizacji albumu wartości formularza i klika użytkownika **zapisać** przycisku zmiany są przesyłane za pośrednictwem HTTP POST wywołania zwrotnego **/StoreManager/Edit/id**. Mimo że adres URL jest taka sama jak w ostatnim wywołaniu, ASP.NET MVC określi, że teraz POST protokołu HTTP, a w związku z tym wykonuje różne metody akcji edycji (jeden ozdobione **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Zadanie 1 - implementacja metody akcji HTTP GET edycji

To zadanie będzie implementowany wersji HTTP GET metody akcji edycji można pobrać odpowiednią Album z bazy danych, a także listę wszystkich Genres i artystów. Go będzie spakować dane do **StoreManagerViewModel** zdefiniowane w ostatnim kroku, który następnie zostanie przekazany do szablonu widoku do renderowania odpowiedzi z obiektu.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex3-CreatingTheEditView/Begin/** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

   1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz **StoreManagerController** klasy. Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.
3. Zastąp **Edytuj HTTP GET** metodę akcji za pomocą następujący kod, aby pobrać odpowiednią **albumu** , jak również **Genres** i **artystów**listy.

    (Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Akcja Ex3 StoreManagerController HTTP GET Edytuj*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Używasz **System.Web.Mvc** **SelectList** dla artystów i Genres zamiast **system.Collections.Generic —** listy.
    > 
    > **SelectList** bardziej przejrzysty sposób wypełnić listę rozwijaną HTML i zarządzanie nimi elementów, jak bieżące zaznaczenie. Tworzenie wystąpień i nowsze, konfigurując te obiekty ViewModel w akcji kontrolera spowoduje, że scenariusza formularza edycji oczyszczarki.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Zadanie 2 — Tworzenie widoku edycji

W ramach tego zadania spowoduje utworzenie szablonu Edytuj widok, który później będzie mógł wyświetlić właściwości albumu.

1. Utwórz widok edycji. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz **Edytuj** metody akcji i wybierz **Dodaj widok**.
2. W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Edytuj**. Sprawdź **utworzyć widok jednoznacznie** wyboru i wybierz **albumu (MvcMusicStore.Models)** z **wyświetlić klasy danych** listy rozwijanej. Wybierz **Edytuj** z **szablonu szkieletu** listy rozwijanej. Pozostaw innych pól ich wartości domyślne, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Dodawanie widoku edycji")

    *Dodawanie widoku edycji*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — uruchamianie aplikacji

W tym zadaniu zostanie sprawdzić, czy **StoreManager** **edytować** strona widoku są wyświetlane wartości właściwości albumu przekazany jako parametr.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/StoreManager/Edit/1** Aby sprawdzić, czy są wyświetlane wartości właściwości dla albumu przekazany.

    ![Przeglądanie widoku edycji albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "przeglądanie albumu widoku edycji")

    *Przeglądanie widoku edycji albumu*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Zadanie 4 — wdrażanie listach rozwijanych w szablonie albumu edytora

W tym zadaniu dodasz listach rozwijanych do Wyświetl szablon utworzony w ostatnim zadaniem, dzięki czemu użytkownik może wybrać z listy artystów i Genres.

1. Zamień wszystkie **albumu** fieldset kod następującym kodem:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > **Html.DropDownList** pomocnika został dodany do listy rozwijane wyboru artystów i Genres renderowania. Parametry przekazywane do **Html.DropDownList** są:
    > 
    > 1. Nazwa pola formularza (**&quot;ArtistId&quot;**).
    > 2. **SelectList** wartości dla listy rozwijanej.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

W tym zadaniu zostanie sprawdzić, czy **StoreManager** **Edytuj** listach rozwijanych zamiast wykonawcy i identyfikator Genre pól tekstowych zostanie wyświetlona strona widoku.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/StoreManager/Edit/1** można zweryfikować Wyświetla listach rozwijanych zamiast wykonawcy i identyfikator Genre pól tekstowych.

    ![Przeglądanie albumu Edytowanie widoku z listy rozwijane](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "przeglądanie albumu Edytowanie widoku z listy rozwijane")

    *Widok edycji przeglądania albumu, tym razem z list rozwijanych*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Zadanie 6 - implementacja metody akcji POST protokołu HTTP, Edytuj

Teraz, edytowanie widoku wyświetlane zgodnie z oczekiwaniami, należy wdrożyć metody akcji Edytuj POST protokołu HTTP, aby zapisać zmiany wprowadzone do albumu.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Otwórz **StoreManagerController** z **kontrolerów** folderu.
2. Zastąp **Edytuj HTTP POST** kod do metody akcji z następującymi (należy pamiętać, że metoda, którą należy zastąpić przeciążone wersji, która otrzymuje dwa parametry):

    (Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Edytuj HTTP POST StoreManagerController Ex3 akcji*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Ta metoda zostanie wykonany, gdy użytkownik kliknie **zapisać** przycisk widoku i wykonuje HTTP POST wartości formularza do serwera zachowywał je w bazie danych. Dekorator **[HttpPost]** wskazuje, czy można użyć metody w tych scenariuszach POST protokołu HTTP. Metoda korzysta z **albumu** obiektu. ASP.NET MVC zostanie automatycznie utworzony obiekt Album z ogłaszanymi &lt;formularza&gt; wartości.
    > 
    > Metoda zostaną wykonane następujące kroki:
    > 
    > 1. Jeśli model jest prawidłowy:
    > 
    >     1. Zaktualizuj wpis albumu w kontekście oznaczyć je jako zmodyfikowanego obiektu.
    >     2. Zapisz zmiany i Przekierowanie do widoku indeksu.
    > 2. Jeśli model nie jest prawidłowy, wypełnia obiekt ViewBag z **GenreId** i **ArtistId**, wówczas zostanie zwrócone widok z odebranego obiektu albumy, aby zezwolić użytkownikowi wykonać wszelkie wymagane aktualizacji.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Zadanie 7 - uruchamiania aplikacji

W tym zadaniu zostanie sprawdzić, czy **edycji StoreManager** — Wyświetl stronę faktycznie zapisuje zaktualizowanych danych albumu w bazie danych.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/StoreManager/Edit/1**. Zmień tytuł albumu do **obciążenia** i wybierz polecenie **zapisać**. Sprawdź, czy tytuł albumu rzeczywiste zmiany na liście albumów.

    ![Aktualizowanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "aktualizowanie albumu")

    *Aktualizowanie albumu*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Ćwiczenie 4: Dodawanie widoku Create

Teraz, gdy **StoreManagerController** obsługuje **Edytuj** możliwości, w tym ćwiczeniu dowiesz się jak dodać szablon Utwórz widok pozwala przechowywać menedżerowie dodawać nowe albumów do aplikacji.

Tak jak w przypadku funkcji Edytuj, zostaną zaimplementowane scenariusz Utwórz za pomocą dwóch osobnych metod w ramach **StoreManagerController** klasy:

1. Jedna metoda akcji zostanie wyświetlony pusty formularz menedżerów magazynu najpierw odwiedź **/StoreManager/Utwórz** adresu URL.
2. Druga metoda akcji obsłuży scenariusza gdzie kliknie Menedżer magazynu **zapisać** przycisk w formularzu i przesyła wartości z powrotem do **/StoreManager/Utwórz** adresem URL HTTP POST.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Zadanie 1 - implementacja metody akcji tworzenia HTTP GET

W tym zadaniu wdroży wersji HTTP GET, utwórz metody akcji, aby pobrać listę wszystkich Genres i artystów, spakować dane do **StoreManagerViewModel** obiektu, który następnie zostanie przekazany do szablonu widoku.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex4-AddingACreateView/Begin/** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

   1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz **StoreManagerController** klasy. Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.
3. Zastąp **Utwórz** kod metody akcji z następujących czynności:

    (Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — utworzenie HTTP GET StoreManagerController Ex4*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Zadanie 2 — Dodawanie widoku Create

To zadanie spowoduje dodanie szablonu tworzenia widoku, który wyświetli nowy formularz albumu (pusta).

1. Kliknij prawym przyciskiem myszy wewnątrz **Utwórz** metody akcji i wybierz **Dodaj widok**. Zostanie wyświetlone okno dialogowe dodawania widoku.
2. W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Utwórz**. Wybierz **utworzyć widok jednoznacznie** opcję i zaznacz **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej i **Utwórz** z **szablonu szkieletu** listy rozwijanej. Pozostaw innych pól ich wartości domyślne, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku create](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Dodawanie a-create-view.png")

    *Dodawanie widoku Create*
3. Aktualizacja **GenreId** i **ArtistId** pola, aby użyć listy rozwijanej, jak pokazano poniżej:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — uruchamianie aplikacji

W tym zadaniu zostanie sprawdzić, czy **StoreManager** **Utwórz** pusty formularz albumu zostanie wyświetlona strona widoku.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/StoreManager/utworzyć**. Sprawdź, czy pusty formularz jest wyświetlany do wypełniania nowe właściwości albumu.

    ![Utwórz widok z pusty formularz](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Utwórz widok z pusty formularz")

    *Utwórz widok z pusty formularz*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Zadanie 4 — wdrażanie POST protokołu HTTP, utwórz metody akcji

W tym zadaniu wdroży wersji HTTP POST Utwórz metody akcji, która będzie wywoływana, gdy użytkownik kliknie **zapisać** przycisku. Metoda należy zapisać nowy album w bazie danych.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Otwórz **StoreManagerController** klasy. Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.
2. Zastąp **POST protokołu HTTP, Utwórz** kod metody akcji z następujących czynności:

    (Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex4 StoreManagerController HTTP POST tworzenie akcji*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Akcja Utwórz jest bardzo podobne do poprzedniej edycji metody akcji, ale zamiast ustawienia obiektu, ponieważ zmodyfikowane, jego jest dodawany do kontekstu.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

W tym zadaniu zostanie sprawdzić, czy **StoreManager utworzyć** strony widoku umożliwia utworzenie nowego albumu, a następnie przekierowuje do widoku indeksu StoreManager.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/StoreManager/utworzyć**. Należy wypełnić wszystkie pola formularza z danymi dla nowego albumu, tak jak na poniższej ilustracji:

    ![Tworzenie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "tworzenia albumu")

    *Tworzenie albumu*
3. Sprawdź, czy następuje przekierowanie do widoku indeksu StoreManager, która obejmuje nowy Album właśnie utworzony.

    ![Nowy Album utworzony](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nowy Album utworzone")

    *Nowy Album utworzone*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Ćwiczenie 5: Obsługa usuwania

Możliwość usunięcia albumów nie została jeszcze zaimplementowana. To jest tym ćwiczeniu będzie o. Tak jak wcześniej, zostanie wdrożyć scenariusz usuwania za pomocą dwóch osobnych metod w ramach **StoreManagerController** klasy:

1. Jedna metoda akcji spowoduje wyświetlenie formularza potwierdzenia
2. Druga metoda akcji obsłuży przesyłania formularza

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Zadanie 1 - implementacja metody akcji Delete HTTP GET

W tym zadaniu zaimplementowaniem wersji HTTP GET metody akcji usuwania można pobrać informacji o albumu.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex5-HandlingDeletion/Begin/** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

   1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz **StoreManagerController** klasy. Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.
3. Akcja usuwania kontrolera jest dokładnie taka sama poprzedniej akcji kontrolera szczegóły magazynu: wysyła zapytanie **albumu** obiektów z bazy danych przy użyciu **identyfikator** podany adres URL i zwraca odpowiednie **widoku**. Aby to zrobić, Zamień HTTP GET **usunąć** kod metody akcji z następujących czynności:

    (Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex5 obsługi usunięcia HTTP GET akcję usunięcia*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Kliknij prawym przyciskiem myszy wewnątrz **usunąć** metody akcji i wybierz **Dodaj widok**. Zostanie wyświetlone okno dialogowe dodawania widoku.
5. W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **usunąć**. Wybierz **utworzyć widok jednoznacznie** opcję i zaznacz **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej. Wybierz **usunąć** z **szablonu szkieletu** listy rozwijanej. Pozostaw innych pól ich wartości domyślne, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Dodawanie widoku Delete")

    *Dodawanie widoku Delete*
6. Usuń szablon pokazuje wszystkie pola z modelu. Zostaną wyświetlone tylko tytuł albumu. Aby to zrobić, Zamień zawartość widoku z następującym kodem:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — w uruchomionej aplikacji

W tym zadaniu zostanie sprawdzić, czy **StoreManager** **usunąć** potwierdzenie usunięcia formularza zostanie wyświetlona strona widoku.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/StoreManager**. Wybierz albumów można usunąć, klikając **usunąć** i upewnij się, że nowy widok została przekazana.

    ![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Usuwanie albumu")

    *Usuwanie albumu*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Zadanie 3 — implementacja metody akcji POST protokołu HTTP Delete

W tym zadaniu wdroży wersji HTTP POST metody akcji usuwania, która będzie wywoływana, gdy użytkownik kliknie **usunąć** przycisku. Metoda, należy usunąć albumu w bazie danych.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Otwórz **StoreManagerController** klasy. Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.
2. Zastąp **usunąć HTTP POST** kod metody akcji z następujących czynności:

    (Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex5 obsługi usunięcia HTTP POST akcji*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

W tym zadaniu zostanie sprawdzić, czy **StoreManager Delete** strony widoku umożliwia usuwanie albumu, a następnie przekierowuje do widoku indeksu StoreManager.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/StoreManager**. Wybierz albumów można usunąć, klikając **usunąć.** Potwierdź decyzję, klikając **usunąć** przycisk:

    ![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Usuwanie albumu")

    *Usuwanie albumu*
3. Sprawdź, czy album została usunięta, ponieważ nie ma w **indeksu** strony.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Ćwiczenie 6: Dodawanie walidacji

Obecnie tworzenie i edytowanie formularzy, które zostały spełnione, nie należy wykonywać dowolny rodzaj sprawdzania poprawności. Jeśli użytkownik opuści wymaganego pola puste lub typ litery w polu Cena, zostanie wyświetlony błąd będzie się z bazy danych.

Sprawdzanie poprawności można dodać do aplikacji przez dodanie do własnej klasy modelu adnotacji danych. Adnotacje danych umożliwiają zawierająca opis reguły, które chcesz zastosować do właściwości modelu i ASP.NET MVC zajmie się wymuszanie i komunikatem odpowiednie dla użytkowników.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Zadanie 1 — Dodawanie adnotacji danych

To zadanie spowoduje dodanie adnotacji danych do modelu albumy, aby ułatwić tworzenie i edytowanie strony ekranu komunikatów dotyczących sprawdzania poprawności, gdy jest to konieczne.

Proste klasy modelu, dodawanie adnotacji danych tylko odbywa się przez dodanie **przy użyciu** instrukcji dla **System.ComponentModel.DataAnnotation**, następnie umieszczenie **[wymagane]**atrybutu na odpowiednie właściwości. Poniższy przykład spowodowałoby **nazwa** właściwości wymaganego pola w widoku.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

To nieco bardziej złożone w przypadkach, takich jak ta aplikacja gdzie jest generowany modelu danych jednostki. Jeśli dodano adnotacji danych bezpośrednio do klasy modeli, będą one zastąpione po zaktualizowaniu modelu z bazy danych. Zamiast tego możesz wprowadzić korzystanie z klasy częściowe metadanych, które będą istnieć do przechowywania adnotacje i są skojarzone z tym modelem klasy przy użyciu **[MetadataType]** atrybutu.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex6-AddingValidation/Begin/** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

   1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz **Album.cs** z **modele** folderu.
3. Zastąp **Album.cs** zawartości ze wyróżniony kod, aby wyglądały one podobnie do następującej:

    > [!NOTE]
    > Wiersz **[DisplayFormat(ConvertEmptyStringToNull=false)]** wskazuje, czy puste ciągi z modelu nie będą konwertowane do wartości null po zaktualizowaniu pole danych w źródle danych. To ustawienie można będzie zapobiec Wystąpił wyjątek podczas programu Entity Framework przed adnotacji danych sprawdza poprawność pól przypisuje wartości null do modelu.

    (Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — częściowej klasy metadanych albumu Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > To **albumu** ma częściowej klasy **MetadataType** atrybut, który wskazuje **AlbumMetaData** klasy dla adnotacji danych. Oto niektóre atrybuty adnotacji danych używanego dla adnotacji modelu albumu:
    > 
    > - Niewymagana — wskazuje, że właściwość jest polem obowiązkowym
    > - Nazwa wyświetlana — Określa tekst, który ma być używany z pola formularza i komunikatów dotyczących sprawdzania poprawności
    > - DisplayFormat — określa sposób wyświetlania i sformatowane pola danych.
    > - StringLength — określa maksymalną długość pola ciągu
    > - Zakres — zapewnia maksymalne i minimalne wartości pola numerycznego
    > - ScaffoldColumn — umożliwia ukrywanie pól z edytora formularzy

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — w uruchomionej aplikacji

To zadanie będzie testowane czy tworzenie i edytowanie strony weryfikacji pól, przy użyciu nazw wyświetlanych w ostatnim zadaniem.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/StoreManager/utworzyć**. Sprawdź, czy nazwy wyświetlane są zgodne z typami klasy częściowej (takich jak **albumów graficznych URL** zamiast **AlbumArtUrl**)
3. Kliknij przycisk **Utwórz**, bez wypełniania formularza. Sprawdź, czy możesz uzyskać odpowiednie komunikatów dotyczących sprawdzania poprawności.

    ![Zweryfikowany pola na stronie Utwórz](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "zweryfikowane pola na stronie Utwórz")

    *Sprawdzone pola na stronie Utwórz*
4. Możesz sprawdzić, czy występuje takie same z **Edytuj** strony. Zmień adres URL do **/StoreManager/Edit/1** i upewnij się, że nazwy wyświetlane są zgodne z typami klasy częściowej (takich jak **albumów graficznych URL** zamiast **AlbumArtUrl**). Pusty **tytuł** i **cen** pola i kliknij przycisk **zapisać**. Sprawdź, czy możesz uzyskać odpowiednie komunikatów dotyczących sprawdzania poprawności.

    ![Sprawdzone pola na stronie edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Sprawdzone pola na stronie edycji*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Ćwiczenie 7: Przy użyciu jQuery dyskretny kod po stronie klienta

W tym ćwiczeniu dowiesz się, jak włączyć dotyczącą weryfikacji jQuery MVC 4 dyskretny kod po stronie klienta.

> [!NOTE]
> Dyskretnego kodu jQuery używa prefiksu danych ajax JavaScript do wywołania metody akcji na serwer, a nie emisji intrusively wbudowane skrypty klienta.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Zadanie 1 - uruchamiania aplikacji przed włączeniem dyskretnego kodu jQuery

W ramach tego zadania należy uruchomić aplikację przed tym jQuery, aby porównać oba modele sprawdzania poprawności.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex7-UnobtrusivejQueryValidation/Begin/** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

   1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Naciśnij klawisz **F5** do uruchomienia aplikacji.
3. Na stronie głównej datę rozpoczęcia projektu. Przeglądaj **/StoreManager/Utwórz** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby sprawdzić, czy możesz uzyskać komunikatów dotyczących sprawdzania poprawności:

    ![Sprawdzanie poprawności klienta wyłączone](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "weryfikacji klienta wyłączone")

    *Sprawdzanie poprawności klienta wyłączone*
4. W przeglądarce otwórz kod źródłowy HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Zadanie 2 — Włączanie sprawdzania poprawności dyskretnego kodu klienta

To zadanie spowoduje włączenie jQuery **sprawdzania poprawności dyskretnego kodu klienta** z **Web.config** pliku, który jest domyślnie ustawiony na wartość false w wszystkie nowe projekty programu ASP.NET MVC 4. Ponadto należy dodać się, że niezbędne skrypty odwołania, aby jQuery pracy dyskretny kod weryfikacji klienta.

1. Otwórz **Web.Config** plików w katalogu głównym projektu i upewnij się, że **ClientValidationEnabled** i **UnobtrusiveJavaScriptEnabled** klucze wartości są ustawiane na **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Można także włączyć sprawdzanie poprawności klienta przez kod w Global.asax.cs, aby uzyskać takie same wyniki:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Ponadto można przypisać atrybutu ClientValidationEnabled w żadnym kontrolerze mają zachowania niestandardowego.
2. Otwórz **Create.cshtml** w **Views\StoreManager**.
3. Upewnij się, że następujące pliki skryptu **jquery.validate** i **jquery.validate.unobtrusive**, odwołuje się widok poprzez &quot; **~/bundles/jqueryval** &quot; pakietu.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Te biblioteki jQuery zostaną uwzględnione w nowych projektów MVC 4. Można znaleźć więcej bibliotek w **/skrypty** folderu projektu należy.
    > 
    > Aby tej weryfikacji pracy biblioteki, musisz dodać odwołanie do biblioteki framework jQuery. Ponieważ to odwołanie zostało już dodane na  **\_Layout.cshtml** plików, nie trzeba go dodać w tego widoku.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Zadanie 3 — uruchomiona dyskretnego kodu przy użyciu aplikacji jQuery sprawdzania poprawności

W tym zadaniu zostanie sprawdzić, czy **StoreManager** utworzyć widok szablonu przeprowadza weryfikacji po stronie klienta przy użyciu biblioteki jQuery podczas tworzenia nowego albumu.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Przeglądaj **/StoreManager/Utwórz** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby sprawdzić, czy możesz uzyskać komunikatów dotyczących sprawdzania poprawności:

    ![Sprawdzanie poprawności klienta z jQuery włączone](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "weryfikacji klienta z jQuery włączone")

    *Sprawdzanie poprawności klienta z jQuery włączone*
3. W przeglądarce otwórz kod źródłowy Utwórz widok:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Dla każdej reguły weryfikacji klienta jQuery dyskretny kod dodaje atrybut z danymi-val -*rulename*=&quot;*komunikat*&quot;. Poniżej znajduje się lista znaczników tej Unobtrusive jQuery wstawia do pola wejściowego html do sprawdzania poprawności klienta:
   > 
   > - Val danych
   > - Data-val-number
   > - Zakres danych val
   > - Dane val zakresu min / danych val zakresu max.
   > - Wymagane wartości danych
   > - Data-val-length
   > - Dane val długość max / danych val długość min
   > 
   > Wszystkie wartości są wypełniane modelu **adnotacji danych elementu**. Następnie całą logikę, która działa po stronie serwera może działać po stronie klienta. Na przykład atrybut cen ma następujące adnotacji danych w modelu:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Po zakończeniu korzystania z dyskretnego kodu jQuery jest wygenerowanego kodu:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Wykonując tego laboratorium Hands-On zapoznaniu umożliwienie użytkownikom na zmianę danych przechowywanych w bazie danych przy użyciu następujących czynności:

- Indeks, tworzenia, edytowania, usuwania, takich jak akcji kontrolera
- Funkcja szkieletów ASP.NET MVC do wyświetlania właściwości tabeli HTML
- Środowisko niestandardowych pomocników HTML zwiększające użytkownika
- Metody akcji, które reagują na HTTP GET lub POST protokołu HTTP wywołania
- Szablon udostępniony edytora dla podobnych szablonów widoków, takich jak tworzenie i edytowanie
- Formularz elementów, takich jak listy rozwijane
- Adnotacje danych na potrzeby sprawdzania poprawności modelu
- Weryfikacja po stronie klienta za pomocą biblioteki dyskretnego kodu jQuery

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Instalowanie programu Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "instalacji programu Visual Studio Express")

    *Instalowanie programu Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Dodatek B: korzystania z wstawek kodu

Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki. Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.

![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*

***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragment (bez spacji i łączniki).
3. Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.
4. Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "zacznij wpisywać nazwę wstawki programu")

*Rozpocznij wpisywanie nazwy fragment kodu*

![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")

*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*

***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")

*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
