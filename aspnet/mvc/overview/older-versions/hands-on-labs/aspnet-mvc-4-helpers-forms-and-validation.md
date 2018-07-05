---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 — pomocnicy, formularze i Walidacja | Dokumentacja firmy Microsoft
author: rick-anderson
description: ASP.NET MVC 4 modeli i danych programu Access warsztatów możesz zostały ładowanie i wyświetlanie danych z bazy danych. W tym laboratorium praktyczne zostanie dodany do...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: f2eb624e72d6f52d1694b5753ee2b1f8117c2851
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376740"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 — pomocnicy, formularze i Walidacja

Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](https://aka.ms/webcamps-training-kit)

W **platformy ASP.NET MVC 4 modele i dostęp do danych** warsztatów, nastąpiło ładowanie i wyświetlanie danych z bazy danych. W tym laboratorium praktyczne zostanie dodany do **Music Store** aplikacji możliwość edytowania tych danych.

Z tego celu należy pamiętać, najpierw utworzysz kontroler, który będzie obsługiwać operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) albumów. Spowoduje wygenerowanie szablonu widoku indeksu, korzystając z zalet funkcji tworzenia szkieletu ASP.NET MVC do wyświetlania właściwości albumów w tabeli HTML. Aby zwiększyć możliwości tego widoku, należy dodać niestandardowego pomocnika HTML, który obetnie długie opisy.

Później dodasz, czy edytowanie i tworzenie widoków, które pozwoli zmienić albumów w bazie danych za pomocą formularza elementów, takich jak listy rozwijane.

Ponadto umożliwi użytkownikom usuwanie albumu i również możesz uniemożliwi im wprowadzanie błędnych danych, sprawdzając poprawność danych wejściowych.

W tym laboratorium praktyczne przyjęto założenie, masz podstawową wiedzę na temat **platformy ASP.NET MVC**. Jeśli nie używasz **platformy ASP.NET MVC** , zalecamy zapoznać się z **podstawowe informacje dotyczące platformy ASP.NET MVC** warsztatów.

W tym laboratorium przeprowadzi Cię przez ulepszeń i nowych funkcji, które opisano wcześniej, stosując drobne zmiany do przykładowej aplikacji sieci Web, pod warunkiem w folderze źródłowym.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzne dla tego laboratorium znajduje się w temacie [platformy ASP.NET MVC 4 pomocnicy, formularze i Walidacja](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktyczne dowiesz się jak:

- Tworzenie kontrolera w celu obsługi operacji CRUD
- Generowanie widoku indeksu, aby wyświetlić właściwości jednostki w tabeli HTML
- Dodawanie niestandardowego pomocnika HTML
- Tworzenie i dostosowywanie widoku Edycja
- Rozróżnienie metod akcji, które reagują na HTTP GET lub POST protokołu HTTP wywołania
- Dodawanie i dostosowywanie Create View
- Dojście do usuwania jednostki
- Weryfikowanie danych wejściowych użytkownika

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Należy dysponować następującymi elementami do przygotowania tego laboratorium:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.

Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek B: za pomocą wstawek kodu programu](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

Następujące ćwiczeń składają się tym warsztatów:

1. [Tworzenie kontrolera Store Manager i jej widok indeksu](#Exercise1)
2. [Dodawanie pomocnika kodu HTML](#Exercise2)
3. [Tworzenie widoku edycji](#Exercise3)
4. [Dodawanie widoku Create](#Exercise4)
5. [Usuwanie obsługi](#Exercise5)
6. [Dodawanie weryfikacji](#Exercise6)
7. [Przy użyciu jQuery dyskretny kod po stronie klienta](#Exercise7)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia. Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.


Szacowany czas do ukończenia tego laboratorium: **60 minut**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Ćwiczenie 1: Tworzenie kontrolera Store Manager i jej widok indeksu

W tym ćwiczeniu zostanie dowiesz się, jak utworzyć nowy kontroler do obsługi operacji CRUD, dostosowywanie jej indeks metody akcji, aby zwrócić listę albumów z bazy danych i na koniec wygenerowanie szablonu widoku indeksu, korzystając z zalet tworzenia szkieletu ASP.NET MVC Funkcja, aby wyświetlić właściwości albumów w tabeli HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Zadanie 1. Tworzenie StoreManagerController

W tym zadaniu utworzysz nowy kontroler o nazwie **StoreManagerController** do obsługi operacji CRUD.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex1-CreatingTheStoreManagerController/rozpoczęcia/** folderu.

   1. Musisz pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Dodaj nowy kontroler. Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań, wybierz pozycję **Dodaj** i następnie **kontrolera** polecenia. Zmiana **kontrolera** **nazwa** do **StoreManagerController** i upewnić się, że opcja **kontroler MVC z akcjami odczytu/zapisu pusty**jest zaznaczone. Kliknij przycisk **Dodaj**.

    ![Okno dialogowe Dodawanie kontrolera](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "okno dialogowe Dodawanie kontrolera")

    *Dodaj kontroler, okno dialogowe*

    Nowa klasa kontrolera jest generowany. Ponieważ przekazały Dodawanie akcji dla odczytu/zapisu, metody klasy zastępczej dla osób, typowych operacji CRUD są tworzone przy użyciu komentarze TODO wypełnione, monitowania obejmujący logiki określonych aplikacji.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Zadanie 2 — Dostosowywanie indeksu StoreManager

To zadanie umożliwia dostosowywanie indeksu StoreManager metoda akcji do zwrócenia widoku z listą albumów z bazy danych.

1. W klasie StoreManagerController, Dodaj następujący kod *przy użyciu* dyrektywy.

    (Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex1 przy użyciu MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Dodaj pole do **StoreManagerController** zawierającą wystąpienia **MusicStoreEntities.**

    (Code Snippet — *pomocników platformy ASP.NET MVC 4 i formularze i Walidacja - Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implementowanie akcji indeksu StoreManagerController do zwrócenia widoku z listy albumów.

    Logika akcji kontrolera jest bardzo podobny do StoreController indeksu akcji w przypadku napisanych wcześniej. Użyj programu LINQ do pobrania wszystkich albumów, w tym wykonawcy i gatunku informacje do wyświetlenia.

    (Code Snippet — *pomocników platformy ASP.NET MVC 4 i formularze i Walidacja — indeks StoreManagerController Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Zadanie 3. Tworzenie widoku indeksu

W tym zadaniu zostanie utworzony szablon widoku indeksu, aby wyświetlić listę albumów zwrócony przez **StoreManager** kontrolera.

1. Przed utworzeniem nowego szablonu widoku, należy skompilować projekt, aby **dialogowe dodawania widoku** obsługującemu **albumu** klasy. Wybierz **kompilacji | Tworzenie MvcMusicStore** do skompilowania projektu.
2. Kliknij prawym przyciskiem myszy wewnątrz **indeksu** metody akcji i wybierz **Dodaj widok**. Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.

    ![Dodaj widok](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Dodawanie widoku")

    *(Code Snippet — *platformy ASP.NET MVC 4 podstawowe - StoreIndexViewModel Ex5 przy użyciu modele widoków*)*
3. W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **indeksu**. Wybierz **utworzyć widok silnie typizowane** opcji, a następnie wybierz **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej. Wybierz **listy** z **szablonu szkieletu** listy rozwijanej. Pozostaw **aparat widoku** do **Razor** i inne pola z ich domyślną wartość, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku indeksu](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Dodawanie widoku indeksu")

    *Dodawanie widoku indeksu*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Zadanie 4 — Dostosowywanie szkieletu widoku indeksu

W tym zadaniu zostanie dostosowana prostego szablonu Widok utworzony za pomocą funkcji tworzenia szkieletu ASP.NET MVC do jego wyświetlania pól, które chcesz.

> [!NOTE]
> **Tworzenia szkieletów** wsparcie w ramach platformy ASP.NET MVC wygeneruje prostego szablonu widoku, który wyświetla listę wszystkich pól w modelu albumu. **Tworzenia szkieletów** umożliwia szybkie rozpoczęcie pracy nad silnie typizowanego widoku: zamiast ręcznie napisać szablon widoku, tworzenia szkieletów szybko generuje domyślny szablon, a następnie można zmodyfikować wygenerowany kod.


1. Przejrzyj kod utworzony. Wygenerowany Lista pól będzie częścią następujących tabeli HTML, który **tworzenia szkieletów** używanej do wyświetlania danych tabelarycznych.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Zastąp **&lt;tabeli&gt;** kodu za pomocą następującego kodu, aby wyświetlić tylko **gatunku**, **wykonawcy**, **tytuł**, i **cena** pola. Spowoduje to usunięcie **AlbumId** i **URL grafikę albumu** kolumn. Ponadto zmienia GenreId i ArtistId kolumny do wyświetlenia ich właściwości klasy połączone **Artist.Name** i **Genre.Name**i usuwa **szczegóły** łącza.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Zmień poniższe opisy.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

W tym zadaniu przetestujesz, **StoreManager** **indeksu** Wyświetl szablon zawiera listę albumów zgodnie z projektem w poprzednich krokach.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/StoreManager** Aby sprawdzić, czy zostanie wyświetlona lista ze zdjęciami, przedstawiający ich **tytuł**, **wykonawcy** i **gatunku**.

    ![Przeglądanie listy albumów](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Przeglądanie listy albumów")

    *Przeglądanie listy albumów*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Ćwiczenie 2: Dodawanie pomocnika kodu HTML

Strona indeksu StoreManager ma jeden potencjalny problem: tytułu i nazwy wykonawcy właściwości mogą jednocześnie być wystarczająco długi, by wyrzucanie formatowanie tabeli. W tym ćwiczeniu dowiesz się, jak dodać niestandardowe pomocnika kodu HTML do obcinania ten tekst.

Na poniższej ilustracji widać, jak zmienić format ze względu na długość tekstu użycia o rozmiarze małe przeglądarki.

![Przeglądanie listy albumy ze nie obcięty tekst](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "nie Przeglądanie listy albumów z obcięty tekst")

*Przeglądanie listy albumy ze nie obcięty tekst*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Zadanie 1 — rozszerzając pomocnika kodu HTML

W tym zadaniu zostanie Dodaj nową metodę **Truncate** do **HTML** obiektu ujawniane w obrębie widoków MVC platformy ASP.NET. Aby to zrobić, zostaną zaimplementowane **— metoda rozszerzenia** do wbudowanej **System.Web.Mvc.HtmlHelper** klasy dostarczane przez platformę ASP.NET MVC.

> [!NOTE]
> Aby dowiedzieć się więcej o **metody rozszerzenia**, można znaleźć w tym artykule w witrynie msdn. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex2-AddingAnHTMLHelper/rozpoczęcia/** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Otwórz widok indeksu StoreManager firmy. Aby to zrobić, w Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **StoreManager** , a następnie otwórz **Index.cshtml** pliku.
3. Dodaj następujący kod poniżej <strong>@model</strong> dyrektywy do definiowania <strong>Truncate</strong> metody pomocnika.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Zadanie 2 — obcinanie tekstu na stronie

W tym zadaniu zostanie użyty **Truncate** metody powoduje obcięcie tekstu w szablonie widoku.

1. Otwórz widok indeksu StoreManager firmy. Aby to zrobić, w Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **StoreManager** , a następnie otwórz **Index.cshtml** pliku.
2. Zamień wiersze, które pokazują **nazwy wykonawcy** i albumu **tytuł**. Aby to zrobić, należy zastąpić następujące wiersze.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3. uruchamianie aplikacji

W tym zadaniu przetestujesz, **StoreManager** **indeksu** Wyświetl szablon obcina tytułu i nazwy wykonawcy albumu.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/StoreManager** Aby sprawdzić, okres czasu teksty w **tytuł** i **wykonawcy** kolumny są obcinane.

    ![Obcięte nazwy tytułów i artystów](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "obcięte nazwy tytułów i artystów")

    *Skrócona tytuły i nazwy wykonawców*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Ćwiczenie 3: Tworzenie widoku edycji

W tym ćwiczeniu dowiesz się, jak utworzyć formularz, aby umożliwić menedżerów store do edycji albumu. Będzie przeglądania **/StoreManager/Edit/id** adresu URL (**identyfikator** jest unikatowy identyfikator fotograficzne, aby edytować), dlatego wywołania HTTP GET do serwera.

Metody akcji kontrolera edycji będzie pobrać odpowiedni Album z bazy danych, tworzenie **StoreManagerViewModel** obiekt do hermetyzacji go (wraz z listy artystów i gatunki), a następnie przekazać go, do widoku szablonu w celu Renderuj stronę HTML do użytkownika. Ta strona będzie zawierać **&lt;formularza&gt;** elementu za pomocą pól tekstowych i menu rozwijane na potrzeby edytowania właściwości albumu.

Po użytkownik aktualizuje wartości formularza albumu i kliknie **Zapisz** przycisku, zmiany są przesyłane za pośrednictwem metody POST protokołu HTTP wywołania zwrotnego **/StoreManager/Edit/id**. Mimo, że adres URL pozostanie taki sam jak ostatniego wywołania, ASP.NET MVC określi, że teraz jest metodę POST protokołu HTTP i w związku z tym wykonuje różne metody akcji edycji (jeden ozdobione **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Zadanie 1 — implementacja metody akcji edycji HTTP GET

W tym zadaniu wdroży wersję HTTP GET metody akcji edycji można pobrać odpowiedni Album z bazy danych, a także wykaz wszystkich gatunki i artystów. Jej będzie spakować te dane do **StoreManagerViewModel** zdefiniowanego w ostatnim kroku, który następnie zostanie przekazany do szablonu widoku do renderowania odpowiedzi z obiektu.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex3-CreatingTheEditView/rozpoczęcia/** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Otwórz **StoreManagerController** klasy. Aby to zrobić, należy rozwinąć **kontrolerów** folder i kliknij dwukrotnie plik **StoreManagerController.cs**.
3. Zastąp **Edytuj HTTP GET** metodę akcji za pomocą następującego kodu, aby pobrać odpowiednią **albumu** , jak również **gatunki** i **artystów**listy.

    (Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex3 StoreManagerController HTTP GET Edytuj akcję*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Używasz **System.Web.Mvc** **SelectList** dla artystów i gatunki zamiast **System.Collections.Generic** listy.
    > 
    > **SelectList** jest bardziej przejrzysty sposób wypełniania list rozwijanych HTML i zarządzanie nimi elementów, takich jak bieżące zaznaczenie. Utworzenie wystąpienia i nowsze, konfigurując tych obiektów ViewModel w akcji kontrolera spowoduje, że scenariusz formularz edycji czyszczenia.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Zadanie 2 — Tworzenie widoku edycji

W tym zadaniu utworzysz szablon widoku Edycja, które później będą wyświetlane, właściwości albumu.

1. Utwórz widok edycji. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz **Edytuj** metody akcji i wybierz **Dodaj widok**.
2. W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Edytuj**. Sprawdź **utworzyć widok silnie typizowane** pole wyboru i wybierz **albumu (MvcMusicStore.Models)** z **wyświetlić klasy danych** listy rozwijanej. Wybierz **Edytuj** z **szablonu szkieletu** listy rozwijanej. Pozostaw wartości domyślne w pozostałych polach, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Dodawanie widoku edycji")

    *Dodawanie widoku edycji*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3. uruchamianie aplikacji

W tym zadaniu przetestujesz, **StoreManager** **Edytuj** strona widoku są wyświetlane wartości właściwości albumu przekazany jako parametr.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/StoreManager/Edit/1** Aby sprawdzić, czy wartości właściwości albumu przekazywane są wyświetlane.

    ![Przeglądanie widoku edycji albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "przeglądania widoku edycji albumu")

    *Przeglądanie albumu widoku edycji*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Zadanie 4 — Implementowanie list rozwijanych w szablonie albumu edytora

W tym zadaniu dodasz list rozwijanych do Wyświetl szablon utworzony w poprzednim zadaniu tak, aby użytkownik może wybrać z listy, artystów i gatunki.

1. Zamień wszystkie **albumu** fieldset kod następującym kodem:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > **Html.DropDownList** pomocnika została dodana do listy rozwijane wyboru artystów i gatunki renderowania. Parametry przekazywane do **Html.DropDownList** są:
    > 
    > 1. Nazwa pola formularza (**&quot;ArtistId&quot;**).
    > 2. **SelectList** wartości z listy rozwijanej.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

W tym zadaniu przetestujesz, **StoreManager** **Edytuj** wyświetlona strona widoku listy rozwijane zamiast wykonawców i ID gatunku pól tekstowych.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/StoreManager/Edit/1** Aby sprawdzić, czy powoduje wyświetlenie listy rozwijane zamiast wykonawców i ID gatunku pól tekstowych.

    ![Przeglądanie albumu widok do edycji z listy rozwijane](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "widoku Edycja przeglądania Album z listy rozwijane")

    *Widok edycji przeglądania fotograficzne, tym razem z list rozwijanych*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Zadanie 6 — Implementowanie metody akcji POST protokołu HTTP, Edytuj

Teraz, gdy edytowanie widoku są wyświetlane zgodnie z oczekiwaniami, należy zaimplementować metodę Edytuj akcję POST protokołu HTTP, aby zapisać zmiany wprowadzone do albumu.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Otwórz **StoreManagerController** z **kontrolerów** folderu.
2. Zastąp **edytować żądania HTTP POST** kod do metody akcji przy użyciu następujących (należy pamiętać, że metoda, którą należy zastąpić przeciążona wersja, która otrzymuje dwa parametry):

    (Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex3 StoreManagerController żądania HTTP POST Edytuj akcję*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Ta metoda zostanie wykonana, gdy użytkownik kliknie **Zapisz** przycisk widoku i wykonuje metodę POST protokołu HTTP-wartości formularza do serwera w celu utrwalenia ich w bazie danych. Dekorator **[HttpPost]** wskazuje, że metoda powinien być używany dla tych scenariuszy POST protokołu HTTP. Ta metoda przyjmuje **albumu** obiektu. ASP.NET MVC automatycznie utworzy obiekt Album z przesłanych &lt;formularza&gt; wartości.
    > 
    > Metoda będzie wykonywać następujące czynności:
    > 
    > 1. Jeśli model jest prawidłowy:
    > 
    >     1. Zaktualizuj wpis albumu w kontekście, aby oznaczyć go jako obiekt zmodyfikowany.
    >     2. Zapisz zmiany i Przekieruj do widoku indeksu.
    > 2. Jeśli model nie jest prawidłowy, spowoduje to wypełnienie elementów ViewBag, za pomocą **GenreId** i **ArtistId**, wówczas zostanie zwrócone widok z odebranego obiektu fotograficzne, aby umożliwić użytkownikowi wykonać dowolne wymagane aktualizacje.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Zadanie 7. uruchamianie aplikacji

W tym zadaniu przetestujesz, **edycji StoreManager** strona widoku faktycznie zapisuje zaktualizowanych danych albumu w bazie danych.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/StoreManager/Edit/1**. Zmień tytuł albumu **obciążenia** i kliknij pozycję **Zapisz**. Upewnij się, że jego tytuł rzeczywiste zmiany na liście albumów.

    ![Aktualizowanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "aktualizowanie albumu")

    *Aktualizowanie albumu*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Ćwiczenie 4: Dodawanie widoku Create

Teraz, gdy **StoreManagerController** obsługuje **Edytuj** możliwości, w tym ćwiczeniu zostanie dowiesz się, jak dodać szablon Create View umożliwiające przechowywanie menedżerowie dodawać nowe albumów do aplikacji.

Jak za pomocą funkcji Edytuj wdroży scenariusz tworzenia przy użyciu dwóch oddzielnych metod w obrębie **StoreManagerController** klasy:

1. Jedną z metod akcji zostanie wyświetlony pusty formularz menedżerów magazynu pierwszej wizyty w witrynie **/StoreManager/tworzenie** adresu URL.
2. Druga metoda akcji będzie obsługiwać scenariusz, gdzie kliknie Menedżer magazynu **Zapisz** znajdujący się w obrębie formularza i przesyła wartości z powrotem do **/StoreManager/tworzenie** adresu URL jako metodę POST protokołu HTTP.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Zadanie 1 — implementacja metody GET protokołu HTTP, Utwórz akcję

W tym zadaniu wdroży wersję HTTP GET metody akcji Utwórz, aby pobrać listę wszystkich gatunki i artystów, spakować te dane do **StoreManagerViewModel** obiektu, który następnie zostanie przekazany do szablonu widoku.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex4-AddingACreateView/rozpoczęcia/** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Otwórz **StoreManagerController** klasy. Aby to zrobić, należy rozwinąć **kontrolerów** folder i kliknij dwukrotnie plik **StoreManagerController.cs**.
3. Zastąp **Utwórz** kod do metody akcji następującym kodem:

    (Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex4 StoreManagerController HTTP GET utworzenie*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Zadanie 2 — Dodawanie widoku Create

W ramach tego zadania zostaną dodane szablon Utwórz widok, który będzie pojawi się nowy formularz albumu (pusty).

1. Kliknij prawym przyciskiem myszy wewnątrz **Utwórz** metody akcji i wybierz **Dodaj widok**. Zostanie wyświetlone okno dialogowe dodawania widoku.
2. W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Utwórz**. Wybierz **utworzyć widok silnie typizowane** opcji, a następnie wybierz pozycję **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej i **Utwórz** z **szablonu szkieletu** listy rozwijanej. Pozostaw wartości domyślne w pozostałych polach, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku Utwórz](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Dodawanie a-create-view.png")

    *Dodawanie widoku Create*
3. Aktualizacja **GenreId** i **ArtistId** pola, aby użyć listy rozwijanej, jak pokazano poniżej:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3. uruchamianie aplikacji

W tym zadaniu przetestujesz, **StoreManager** **Utwórz** widoku strony wyświetli pusty formularz albumu.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/StoreManager/tworzenie**. Sprawdź, czy pusty formularz jest wyświetlany do wypełniania nowej właściwości albumu.

    ![Utwórz widok przy użyciu pusty formularz](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View z pusty formularz")

    *Utwórz widok przy użyciu pusty formularz*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Zadanie 4 — Implementowanie POST protokołu HTTP, utwórz metody akcji

W tym zadaniu będą wdrażać wersję POST protokołu HTTP, utwórz metody akcji, która będzie wywołana, gdy użytkownik kliknie **Zapisz** przycisku. Metoda należy zapisać nowy album w bazie danych.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Otwórz **StoreManagerController** klasy. Aby to zrobić, należy rozwinąć **kontrolerów** folder i kliknij dwukrotnie plik **StoreManagerController.cs**.
2. Zastąp **POST protokołu HTTP, Utwórz** kod do metody akcji następującym kodem:

    (Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex4 StoreManagerController HTTP - POST tworzenie akcji*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Akcja Utwórz jest dość podobny do poprzedniego metody akcji, Edytuj, ale zamiast ustawienia obiektu, ponieważ zmodyfikowane, to są dodawane do kontekstu.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

W tym zadaniu przetestujesz, **tworzenie StoreManager** strona widoku umożliwia utworzenie nowego albumu, a następnie przekierowuje do widoku indeksu StoreManager.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/StoreManager/tworzenie**. Wypełnij wszystkie pola formularza z danych dla nowego albumu podobne do pokazanego na poniższym rysunku:

    ![Tworzenie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "tworzeniu albumu")

    *Tworzenie albumu*
3. Sprawdź, czy uzyskać przekierowanie, do widoku indeksu StoreManager, która obejmuje nowy Album właśnie utworzony.

    ![Nowy Album utworzone](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nowy Album utworzone")

    *Nowy Album utworzone*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Ćwiczenie 5: Obsługa usuwania

Możliwość usunięcia albumów nie została jeszcze zaimplementowana. Jest to ćwiczenie będzie o. Tak jak poprzednio, wdroży scenariusz Delete, przy użyciu dwóch oddzielnych metod w **StoreManagerController** klasy:

1. Jedną z metod akcji będą wyświetlane w formularzu potwierdzenia
2. Druga metoda akcji będzie obsługiwać przesyłania formularza

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Zadanie 1 — implementacja metody akcji Delete protokołu HTTP GET

W tym zadaniu wdroży wersję HTTP GET metody akcji usuwania można pobrać informacji o albumu.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex5-HandlingDeletion/rozpoczęcia/** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Otwórz **StoreManagerController** klasy. Aby to zrobić, należy rozwinąć **kontrolerów** folder i kliknij dwukrotnie plik **StoreManagerController.cs**.
3. Usuwanie akcji kontrolera jest dokładnie taka sama poprzedniej akcji kontrolera szczegóły Store: zapytań **albumu** obiektu z bazy danych przy użyciu **identyfikator** podany adres URL i zwraca odpowiednie **widoku**. Aby to zrobić, Zastąp HTTP GET **Usuń** kod do metody akcji następującym kodem:

    (Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex5 obsługi usuwania HTTP GET usunąć akcję*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Kliknij prawym przyciskiem myszy wewnątrz **Usuń** metody akcji i wybierz **Dodaj widok**. Zostanie wyświetlone okno dialogowe dodawania widoku.
5. W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Usuń**. Wybierz **utworzyć widok silnie typizowane** opcji, a następnie wybierz pozycję **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej. Wybierz **Usuń** z **szablonu szkieletu** listy rozwijanej. Pozostaw wartości domyślne w pozostałych polach, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Dodawanie widoku Delete")

    *Dodawanie widoku Delete*
6. Usuń szablon zawiera wszystkie pola z modelu. Zostaną wyświetlone tylko tytuł albumu. Aby to zrobić, Zastąp zawartość widoku z następującym kodem:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — uruchamianie aplikacji

W tym zadaniu przetestujesz, **StoreManager** **Usuń** widoku strony wyświetla formularz potwierdzenie usunięcia.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/StoreManager**. Wybierz jeden album można usunąć, klikając **Usuń** i sprawdź, czy zostanie przekazany nowy widok.

    ![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Usuwanie albumu")

    *Usuwanie albumu*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Zadanie 3 — implementacja metody akcji usuwania POST protokołu HTTP

W tym zadaniu będą wdrażać wersję POST protokołu HTTP, Usuń metody akcji, która będzie wywołana, gdy użytkownik kliknie **Usuń** przycisku. Metoda, należy usunąć albumu w bazie danych.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio. Otwórz **StoreManagerController** klasy. Aby to zrobić, należy rozwinąć **kontrolerów** folder i kliknij dwukrotnie plik **StoreManagerController.cs**.
2. Zastąp **POST protokołu HTTP, Usuń** kod do metody akcji następującym kodem:

    (Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex5 obsługi usuwania żądania HTTP POST akcji*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

W tym zadaniu przetestujesz, **StoreManager Delete** strona widoku służy do usuwania albumu, a następnie przekierowuje do widoku indeksu StoreManager.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/StoreManager**. Wybierz jeden album można usunąć, klikając **usunąć.** Potwierdzenie usunięcia, klikając **Usuń** przycisku:

    ![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Usuwanie albumu")

    *Usuwanie albumu*
3. Sprawdź, czy album została usunięta, ponieważ nie ma w **indeksu** strony.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Ćwiczenie 6: Dodawanie walidacji

Obecnie tworzyć i edytować formularze, które zostały spełnione, nie należy wykonywać wszelkiego rodzaju sprawdzania poprawności. Jeśli użytkownik opuści wymagane pole jest puste lub typ liter w polu Cena, pierwszy błąd, który zostanie wyświetlony będzie z bazy danych.

Aby dodać sprawdzanie poprawności do aplikacji, dodawanie adnotacji danych do klasy modelu. Adnotacje danych Zezwalaj, zawierająca opis reguły, które mają stosowane do właściwości modelu i platformy ASP.NET MVC zajmie się wymuszanie i wyświetlania odpowiedni komunikat do użytkowników.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Zadanie 1 — Dodawanie adnotacji danych

W ramach tego zadania zostaną dodane adnotacje danych do modelu fotograficzne, który spowoduje, że strona tworzyć i edytować wyświetlanie komunikatów dotyczących sprawdzania poprawności, gdy jest to konieczne.

Proste klasy modelu, dodawanie adnotacji danych po prostu odbywa się przez dodanie **przy użyciu** poufności informacji dotyczące **System.ComponentModel.DataAnnotation**, następnie umieszczenie **[wymagane]** atrybutu odpowiednie właściwości. Poniższy przykład czyniłyby **nazwa** właściwości wymagane pola w widoku.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Jest to nieco bardziej złożone, w przypadkach, takich jak tej aplikacji, gdzie jest generowany modelu Entity Data Model. Jeśli dodano adnotacje danych bezpośrednio do klasy modeli, ich zostaną zastąpione po aktualizacji modelu z bazy danych. Zamiast tego można tworzyć z metadanych klas częściowych, które będzie istnieć do przechowywania adnotacje i skojarzone z modelu należy używać klas, za pomocą **[MetadataType]** atrybutu.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex6-AddingValidation/rozpoczęcia/** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Otwórz **Album.cs** z **modeli** folderu.
3. Zastąp **Album.cs** zawartości ze wyróżniony kod, tak że wygląda podobnie do poniższego:

    > [!NOTE]
    > Wiersz **[DisplayFormat(ConvertEmptyStringToNull=false)]** wskazuje, czy puste ciągi z modelu nie zostaną przekonwertowane na wartości null, po zaktualizowaniu pola danych w źródle danych. To ustawienie pozwoli uniknąć wyjątek, gdy platforma Entity Framework przypisuje wartości null do modelu przed adnotacji danych sprawdza poprawność pól.

    (Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - klasy częściowej metadanych albumu Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > To **albumu** częściowa klasa ma **MetadataType** atrybut, który wskazuje na **AlbumMetaData** klasy dla adnotacji danych. Oto niektóre atrybuty adnotacji danych, których używasz dodawać adnotacje do modelu albumu:
    > 
    > - Niewymagana — wskazuje, że właściwość jest polem wymaganym
    > - DisplayName — Określa tekst, który ma być używany na pola formularza i komunikatów dotyczących sprawdzania poprawności
    > - DisplayFormat — określa sposób wyświetlania i sformatowane pola danych.
    > - StringLength - określa maksymalną długość pola ciągu
    > - Zakres — zapewnia maksymalne i minimalne wartości pola numerycznego
    > - ScaffoldColumn — umożliwia ukrycie pola w edytorze formularzy

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — uruchamianie aplikacji

W tym zadaniu przetestujesz, tworzyć i edytować stron sprawdzanie poprawności pola, przy użyciu nazw wyświetlanych wybrany w poprzednim zadaniu.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/StoreManager/tworzenie**. Sprawdź, czy nazwy wyświetlane zgodne z typami klasy częściowej (takich jak **URL grafikę albumu** zamiast **AlbumArtUrl**)
3. Kliknij przycisk **Utwórz**, bez wypełniania formularza. Upewnij się, że otrzymasz odpowiednie komunikaty sprawdzania poprawności.

    ![Zweryfikowane pola na stronie Utwórz](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "zweryfikowane pola na stronie Utwórz")

    *Sprawdzone pola na stronie Utwórz*
4. Możesz sprawdzić, że takie same odbywa się za pomocą **Edytuj** strony. Zmień adres URL do **/StoreManager/Edit/1** i sprawdź, czy nazwy wyświetlane zgodne z typami klasy częściowej (takich jak **URL grafikę albumu** zamiast **AlbumArtUrl**). Pusty **tytuł** i **cena** pola i kliknij przycisk **Zapisz**. Upewnij się, że otrzymasz odpowiednie komunikaty sprawdzania poprawności.

    ![Sprawdzone pola na stronie edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Sprawdzone pola na stronie edycji*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Ćwiczenie 7: Przy użyciu jQuery dyskretny kod po stronie klienta

W tym ćwiczeniu dowiesz się, jak włączyć dotyczącą weryfikacji jQuery MVC 4 dyskretny kod po stronie klienta.

> [!NOTE]
> Dyskretnego kodu jQuery używa prefiksu danych ajax JavaScript do wywołania metody akcji, na serwerze, a nie intrusively emitowanie wbudowanych skryptach klienta.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Zadanie 1. uruchomienie aplikacji przed włączeniem dyskretnego kodu jQuery

W tym zadaniu należy uruchomić aplikację przed dołączeniem jQuery, aby można było porównać oba modele sprawdzania poprawności.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex7-UnobtrusivejQueryValidation/rozpoczęcia/** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Naciśnij klawisz **F5** do uruchomienia aplikacji.
3. Data rozpoczęcia projektu na stronie głównej. Przeglądaj **/StoreManager/tworzenie** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby zweryfikować, że masz komunikatów dotyczących sprawdzania poprawności:

    ![Sprawdzanie poprawności klienta wyłączone](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "wyłączyć sprawdzanie poprawności klienta")

    *Sprawdzanie poprawności klienta wyłączone*
4. W przeglądarce otwórz kod źródłowy HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Zadanie 2 — Włączanie sprawdzania poprawności dyskretnego kodu klienta

W ramach tego zadania umożliwi jQuery **sprawdzania poprawności dyskretnego kodu klienta** z **Web.config** pliku, który jest domyślnie ustawiona na wartość false, we wszystkich nowych projektach programu ASP.NET MVC 4. Ponadto dodasz, że niezbędne skrypty odwołania się jQuery pracy dyskretny kod weryfikacji klienta.

1. Otwórz **Web.Config** plików w katalogu głównym projektu i upewnij się, że **ClientValidationEnabled** i **UnobtrusiveJavaScriptEnabled** kluczy wartości są ustawione na **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Można także włączyć sprawdzanie poprawności klienta przez kod w Global.asax.cs, aby uzyskać te same wyniki:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Ponadto można przypisać atrybutu ClientValidationEnabled do dowolnego kontrolera, aby określić niestandardowe zachowanie.
2. Otwórz **Create.cshtml** na **Views\StoreManager**.
3. Upewnij się, że następujące pliki skryptów **jquery.validate** i **jquery.validate.unobtrusive**, do których istnieją odwołania w poprzez wyświetlanie &quot; **~/bundles/jqueryval** &quot; pakietu.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Te biblioteki jQuery są uwzględniane w nowych projektów MVC 4. Można znaleźć więcej bibliotek w **/skrypty** folderu w projekcie.
    > 
    > Przed tym sprawdzania poprawności pracy bibliotek, musisz dodać odwołanie do biblioteki framework jQuery. Ponieważ ta dokumentacja została już dodana w  **\_Layout.cshtml** pliku, nie trzeba go dodać w tego widoku.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Zadanie 3 — uruchamianie dyskretnego kodu za pomocą aplikacji jQuery sprawdzania poprawności

W tym zadaniu przetestujesz, **StoreManager** Tworzenie szablonu przeprowadza weryfikacji po stronie klienta przy użyciu biblioteki jQuery, gdy użytkownik tworzy nowy album widoku.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Przeglądaj **/StoreManager/tworzenie** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby zweryfikować, że masz komunikatów dotyczących sprawdzania poprawności:

    ![Sprawdzanie poprawności klienta przy użyciu jQuery włączone](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "sprawdzanie poprawności klienta przy użyciu jQuery włączone")

    *Sprawdzanie poprawności klienta przy użyciu jQuery włączone*
3. W przeglądarce otwórz kod źródłowy Utwórz widok:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Dla każdej reguły weryfikacji klienta jQuery dyskretny kod dodaje atrybut z danymi — val —*rulename*=&quot;*komunikat*&quot;. Poniżej przedstawiono listę tagów tego Unobtrusive jQuery wstawia do pola wejściowego html, aby wykonać sprawdzanie poprawności klienta:
   > 
   > - Val danych
   > - Dane val liczb
   > - Dane val zakresu
   > - Data-val zakres min / danych val — — maksimum zakresu
   > - Wymagane val dane
   > - Dane o długości val
   > - Data-val długość max / danych val długość min
   > 
   > Wszystkie wartości danych są wypełniane przy użyciu modelu **adnotacji danych**. Następnie całą logikę, która działa na po stronie serwera może działać po stronie klienta. Na przykład cena atrybut ma następujące adnotacji danych w modelu:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Po zakończeniu korzystania z dyskretnego kodu jQuery jest wygenerowany kod:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Realizując ten warsztatów wiesz jak umożliwić użytkownikom zmienić dane przechowywane w bazie danych, korzystając z następujących czynności:

- Indeks, tworzenia, edytowania, usuwania, takie jak akcji kontrolera
- Funkcja tworzenia szkieletu ASP.NET MVC do wyświetlania właściwości tabeli HTML
- Środowisko niestandardowych pomocników HTML w celu zwiększenia użytkownika
- Metody akcji, które reagują na HTTP GET lub POST protokołu HTTP wywołania
- Szablon udostępniony edytora podobne szablony widoku, takie jak tworzenie i edytowanie
- Elementy formularza, takie jak listy rozwijane
- Adnotacje danych na potrzeby weryfikacji modelu
- Weryfikacja po stronie klienta przy użyciu dyskretnego kodu Biblioteka jQuery

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "zainstalować program Visual Studio Express")

    *Zainstaluj program Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Dodatek B: korzystania z fragmentów kodu

Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki. Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.

![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*

***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).
3. Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.
4. Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragmentu kodu](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Rozpocznij wpisywanie nazwy fragmentu kodu")

*Rozpocznij wpisywanie nazwy fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")

*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*

***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")

*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
