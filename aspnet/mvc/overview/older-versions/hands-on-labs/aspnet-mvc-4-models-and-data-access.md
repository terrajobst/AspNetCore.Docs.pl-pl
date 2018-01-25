---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: "Modele platformy ASP.NET MVC 4 oraz dostęp do danych | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Uwaga: W tym laboratorium Hands-on przyjęto założenie, że masz podstawową wiedzę na temat platformy ASP.NET MVC. Jeśli nie użyto programu ASP.NET MVC przed, zalecamy zapoznać się z platformy ASP.NET MVC 4..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 076fa87eff140a3e7ff6855e4876abac40419c57
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-models-and-data-access"></a>Modele platformy ASP.NET MVC 4 oraz dostęp do danych
====================
przez [obozów sieci Web Team](https://twitter.com/webcamps)

> [!NOTE]
> W tym laboratorium Hands-on zakłada mieć podstawową wiedzę na temat **ASP.NET MVC**. Jeśli nie używasz **ASP.NET MVC** przed, zalecamy zapoznać się z **podstawowe informacje na temat platformy ASP.NET MVC 4** Hands-on laboratorium.
> 
> W tym laboratorium przeprowadzi Cię przez rozszerzenia oraz nowe funkcje opisane wcześniej przez zastosowanie drobne zmiany do przykładowej aplikacji sieci Web w folderze źródłowym.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).


W **podstawowe informacje na temat platformy ASP.NET MVC** laboratorium Hands-on można mieć zostały przekazywanie ustalony danych z kontrolerów do widoku szablonów. Jednak aby można było tworzyć rzeczywista aplikacji sieci Web, możesz chcieć użyć rzeczywistych bazy danych.

W tym laboratorium Hands-on będzie pokazują, jak korzystać z aparatu bazy danych w celu przechowywania i pobierania danych potrzebnych do aplikacji Sklep utworów muzycznych. Do wykonania, które będzie rozpoczynać się od istniejącej bazy danych i tworzenia modelu Entity Data Model z niego. W tym laboratorium będzie spełniać **Database First** podejście, jak również **Code First** podejście.

Jednak umożliwia także **Model First** podejścia, utwórz ten sam model przy użyciu narzędzia, a następnie wygeneruj bazy danych z niego.

![Pierwszy vs bazy danych. Model pierwszy](aspnet-mvc-4-models-and-data-access/_static/image1.png "bazy danych pierwszej wersji programu vs. Najpierw modelu")

*Pierwszy vs bazy danych. Najpierw modelu*

Po wygenerowaniu modelu, można utworzyć odpowiednie korekty w StoreController dostarczanie widoków magazynu danych pobranych z bazy danych, zamiast ustalony danych. Nie należy wprowadzać żadnych zmian widoku szablonów ponieważ StoreController będzie zwracać tego samego ViewModels szablony widoku, mimo że teraz dane będą pobierane z bazy danych.

**Pierwszym sposobem kodu**

Code First podejście pozwala zdefiniować modelu z kodu bez generowania klasy, które zwykle są powiązane z architekturą.

W kodzie, najpierw obiekty modelu są zdefiniowane z POCOs, &quot;zwykły stare obiekty CLR&quot;. POCOs są proste klasy zwykły, nie dziedziczenia, które nie implementują interfejsy. Firma Microsoft może automatycznie generować bazy danych z ich lub możemy Użyj istniejącej bazy danych i Generowanie mapowania klasy z kodu.

Korzyści wynikające ze stosowania tego podejścia jest modelu pozostaje niezależnie od framework trwałości (w tym przypadku Entity Framework), zgodnie z klasy POCOs nie są połączone z framework mapowania.

> [!NOTE]
> To laboratorium jest oparte na programie ASP.NET MVC 4 i wersji magazynu utworów muzycznych przykładowej aplikacji dostosowanej zminimalizowany, aby dopasować tylko te funkcje, które są wyświetlane w tym laboratorium Hands-On.
> 
> Jeśli chcesz zapoznać się z całym **sklep muzyczny** samouczek aplikacji można znaleźć w [MVC utworów muzycznych magazynu](https://github.com/evilDave/MVC-Music-Store).


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

Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek C:](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

W tym laboratorium Hands-on składa się przez następujących czynnościach:

1. [Ćwiczenie 1: Dodawanie bazy danych](#Exercise1)
2. [Ćwiczenie 2: Utworzenie bazy danych za pomocą funkcji Code First](#Exercise2)
3. [Ćwiczenie 3: Zapytanie bazy danych z parametrami](#Exercise3)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach. Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.


Szacowany czas trwania tego laboratorium: **35 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Ćwiczenie 1: Dodawanie bazy danych

W tym ćwiczeniu dowiesz się, jak dodać bazy danych z tabel aplikacji MusicStore do rozwiązania, aby można było korzystać z jego dane. Po bazy danych jest generowany z modelem i dodany do rozwiązania, należy zmodyfikować klasy StoreController dostarczanie szablon widoku danych pobranych z bazy danych, zamiast zakodowanych wartości.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Zadanie 1 — Dodawanie bazy danych

To zadanie spowoduje dodanie już utworzono bazę danych z głównych tabel aplikacji MusicStore do rozwiązania.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex1-AddingADatabaseDBFirst/Begin/** folderu.

    1. Należy pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Dodaj **MvcMusicStore** pliku bazy danych. W tym laboratorium Hands-on użyje utworzone bazę danych o nazwie **MvcMusicStore.mdf**. W tym celu kliknij prawym przyciskiem myszy **aplikacji\_danych** folderu, wskaż **Dodaj** , a następnie kliknij przycisk **istniejący element**. Przejdź do **\Source\Assets** i wybierz **MvcMusicStore.mdf** pliku.

    ![Dodawanie istniejącego elementu](aspnet-mvc-4-models-and-data-access/_static/image2.png "Dodawanie istniejącego elementu")

    *Dodawanie istniejącego elementu*

    ![Plik bazy danych MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf pliku bazy danych")

    *MvcMusicStore.mdf pliku bazy danych*

    Bazy danych dodano do projektu. Nawet wtedy, gdy baza danych znajduje się wewnątrz rozwiązania, możesz znaleźć, a następnie go zaktualizować, ponieważ została ona hostowana na serwerze z inną bazą danych.

    ![Baza danych MvcMusicStore w Eksploratorze rozwiązań](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore bazy danych, w Eksploratorze rozwiązań")

    *Baza danych MvcMusicStore w Eksploratorze rozwiązań*
3. Sprawdź połączenie z bazą danych. Aby to zrobić, kliknij dwukrotnie **MvcMusicStore.mdf** ustanowić połączenie.

    ![Łączenie z MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "nawiązywania MvcMusicStore.mdf")

    *Łączenie z MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Zadanie 2 — Tworzenie modelu danych

W ramach tego zadania spowoduje utworzenie modelu danych do interakcji z dodanym w poprzednim zadaniu bazy danych.

1. Tworzenie modelu danych, która będzie reprezentowała bazy danych. Aby to zrobić, w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modele** folderu, wskaż **Dodaj** , a następnie kliknij przycisk **nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **danych** szablonu, a następnie **modelu danych jednostki ADO.NET** elementu. Zmień nazwę modelu danych, aby **StoreDB.edmx** i kliknij przycisk **Dodaj**.

    ![Dodawanie modelu danych jednostki ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "Dodawanie modelu danych jednostki ADO.NET StoreDB")

    *Dodawanie modelu danych jednostki ADO.NET StoreDB*
2. **Kreatora modelu danych jednostki** będą wyświetlane. Ten kreator przeprowadzi Cię przez tworzenie warstwy modelu. Ponieważ modelu należy tworzyć w oparciu o istniejących recentyl bazy danych dodane, wybierz opcję **generowania z bazy danych** i kliknij przycisk **dalej**.

    ![Wybieranie zawartość modelu](aspnet-mvc-4-models-and-data-access/_static/image7.png "Wybieranie zawartość modelu")

    *Wybieranie modelu zawartości*
3. Ponieważ są generowania modelu z bazy danych, należy określić połączenie. Kliknij przycisk **nowe połączenie**.
4. Wybierz **plik bazy danych programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**.

    ![Wybierz źródło danych](aspnet-mvc-4-models-and-data-access/_static/image8.png "wybierz źródło danych")

    *Wybierz źródło danych w oknie dialogowym*
5. Kliknij przycisk **Przeglądaj** i wybierz bazę danych **MvcMusicStore.mdf** zlokalizowanego w **aplikacji\_danych** folder i kliknij przycisk **OK**.

    ![Właściwości połączenia](aspnet-mvc-4-models-and-data-access/_static/image9.png "właściwości połączenia")

    *Właściwości połączenia*
6. Wygenerowana klasa powinna mieć taką samą nazwę jak parametry połączenia jednostki, zmienia jego nazwę, aby **MusicStoreEntities** i kliknij przycisk **dalej**.

    ![Wybieranie połączenia danych](aspnet-mvc-4-models-and-data-access/_static/image10.png "Wybieranie połączenia danych")

    *Wybieranie połączenia danych*
7. Wybierz obiekty bazy danych do użycia. Modelu jednostki będzie używać tylko tabele bazy danych, wybierz **tabel** opcję i upewnij się, że **Dołącz kolumny klucza obcego do modelu** i **Pluralize lub nadaj wygenerowany nazwy obiektów** również są zaznaczone opcje. Zmień Namespace modelu do **MvcMusicStore.Model** i kliknij przycisk **Zakończ**.

    ![Wybieranie obiektów bazy danych](aspnet-mvc-4-models-and-data-access/_static/image11.png "Wybieranie obiektów bazy danych")

    *Wybieranie obiektów bazy danych*

    > [!NOTE]
    > Okno dialogowe ostrzeżenia zabezpieczeń jest wyświetlany, kliknij przycisk **OK** uruchomić szablon i Generuj klasy dla jednostek w modelu.
8. Diagram jednostek dla bazy danych będą wyświetlane, gdy zostanie utworzony oddzielny klasy, która mapuje każdej tabeli w bazie danych. Na przykład **albumów** tabeli będą reprezentowane przez **albumu** klasy, w której każda kolumna w tabeli przypisze do właściwości klasy. Pozwoli to zapytanie i pracować z obiektami, które reprezentują wierszy w bazie danych.

    ![Diagram jednostek](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagram jednostek")

    *Diagram jednostek*

> [!NOTE]
> Szablony T4 (.TT —) uruchom kod można wygenerować klas jednostek i spowoduje zastąpienie istniejących klas o takiej samej nazwie. W tym przykładzie klasy &quot;albumu&quot;, &quot;Genre&quot; i &quot;wykonawcy&quot; zostały zastąpione wygenerowanego kodu.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Zadanie 3 — Tworzenie aplikacji

W tym zadaniu można będzie sprawdzać, mimo że generowania modelu zostały usunięte **albumu**, **Genre** i **wykonawcy** klasy modelu projektu tworzy pomyślnie przy użyciu nowe klasy modelu danych.

1. Skompiluj projekt, wybierając **kompilacji** element menu, a następnie **kompilacji MvcMusicStore**.

    ![Tworzenie projektu](aspnet-mvc-4-models-and-data-access/_static/image13.png "skompilowanie projektu")

    *Tworzenie projektu*
2. Projekt tworzy się pomyślnie. Dlaczego nadal działa? To działa, ponieważ tabele bazy danych ma pola, które zawierają właściwości, które były używane w klasach usuniętych **albumu** i **Genre**.

    ![Kompilacje zakończyło się pomyślnie](aspnet-mvc-4-models-and-data-access/_static/image14.png "kompilacji zakończyło się pomyślnie")

    *Kompilacje powiodło się.*
3. Gdy Projektant wyświetla jednostek w formacie diagramu, są one naprawdę klas C#. Rozwiń węzeł **StoreDB.edmx** węzła w Eksploratorze rozwiązań, a następnie **StoreDB.tt**, pojawi się nowe jednostki wygenerowany.

    ![Pliki generowane](aspnet-mvc-4-models-and-data-access/_static/image15.png "wygenerowanych plików")

    *Wygenerowane pliki*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Zadanie 4 — kwerendy bazy danych

To zadanie spowoduje zaktualizowanie klasy StoreController tak, aby zamiast dane zapisane na stałe będzie zapytania bazy danych do pobrania informacji.

1. Otwórz **Controllers\StoreController.cs** i dodaj następujące pole do klasy, aby pomieścić wystąpienia **MusicStoreEntities** klasę o nazwie **storeDB**:

    (Fragment - kodu *modeli i dostęp do danych - Ex1 storeDB*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. **MusicStoreEntities** klasy udostępnia właściwości kolekcji dla każdej tabeli w bazie danych. Aktualizacja **Przeglądaj** metody akcji można pobrać określonego rodzaju ze wszystkimi **albumów**.

    (Fragment - kodu *modeli i dostęp do danych - przeglądania magazynu Ex1*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > W przypadku korzystania z możliwości .NET o nazwie **LINQ** (zapytanie o języku zintegrowanym) do zapisania wyrażeń jednoznacznie zapytania względem tych kolekcje — co spowoduje wykonanie kodu w bazie danych i zwracać obiekty można umieszczonych przed.
    > 
    > Aby uzyskać więcej informacji na temat LINQ, odwiedź stronę [witrynę msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres.

    (Fragment - kodu *modeli i Data Access — indeks magazynu Ex1*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres i przekształcenie kolekcji na listę.

    (Fragment - kodu *modeli i dostęp do danych - GenreMenu magazynu Ex1*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

To zadanie będzie sprawdzać, czy strona indeksu magazynu wyświetli Genres przechowywane w bazie danych, zamiast ustalony te. Nie istnieje potrzeba zmiany szablonu widoku, ponieważ **StoreController** zwraca tej samej jednostki jak poprzednio, ale tym razem dane będą pobierane z bazy danych.

1. Ponowne kompilowanie rozwiązania i naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Sprawdź, czy menu **Genres** nie jest już listy zapisane na stałe, a dane bezpośrednio są pobierane z bazy danych.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Przeglądanie Genres z bazy danych*
3. Teraz przejdź do dowolnego rodzaju i sprawdź, czy albumów zostają wypełnione z bazy danych.

    ![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image17.png "przeglądanie albumów z bazy danych")

    *Przeglądanie albumów z bazy danych*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Ćwiczenie 2: Utworzenie bazy danych najpierw przy użyciu kodu

W tym ćwiczeniu dowiesz się, jak użyć metody Code First, aby utworzyć bazę danych z tabel MusicStore aplikacji oraz jak dostępu do danych.

Po wygenerowaniu modelu, należy zmodyfikować StoreController dostarczanie szablon widoku danych z bazy danych, zamiast wartości zapisane na stałe.

> [!NOTE]
> Jeśli ukończono ćwiczenie 1 i pracowali już z bazy danych pierwszego podejścia, będzie teraz Dowiedz się jak uzyskać takie same wyniki z innego procesu. Zadania, które są wspólne dla wykonywania 1 została oznaczona ułatwi Twojej odczytu. Jeśli nie została ukończona wykonywania 1, ale chcesz dowiedzieć się podejście Code First, możesz rozpocząć od tego ćwiczenia i uzyskać pełny zakres tego tematu.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Zadanie 1 - wypełnianie przykładowe dane

W tym zadaniu zostanie Wypełnianie bazy danych z przykładowymi danymi tworzonemu początkowemu przy użyciu pierwszej kodu.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex2-CreatingADatabaseCodeFirst/Begin/** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Dodaj **SampleData.cs** pliku **modele** folderu. W tym celu kliknij prawym przyciskiem myszy **modele** folderu, wskaż **Dodaj** , a następnie kliknij przycisk **istniejący element**. Przejdź do **\Source\Assets** i wybierz **SampleData.cs** pliku.

    ![Przykładowe dane wypełnić kodu](aspnet-mvc-4-models-and-data-access/_static/image18.png "przykładowych danych wypełnić kodu")

    *Przykładowe dane wypełnić kodu*
3. Otwórz **Global.asax.cs** pliku i dodaj następującą *przy użyciu* instrukcje.

    (Fragment - kodu *modeli i Data Access — deklaracje Using Ex2 globalne Asax*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. W **aplikacji\_Start()** metody Dodaj następujący wiersz do ustawienia inicjatora bazy danych.

    (Fragment - kodu *modeli i dostęp do danych - globalne Asax SetInitializer Ex2*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Zadanie 2 — Konfigurowanie połączenia z bazą danych

Teraz, gdy masz już dodany bazy danych do naszej projektu, będzie zapisywać **Web.config** pliku parametrów połączenia.

1. Dodaj parametry połączenia w **Web.config**. Aby to zrobić, otwórz **Web.config** w głównym projektu i Zastąp ciąg połączenia o nazwie parametru DefaultConnection z tego wiersza w  **&lt;connectionStrings&gt;**  sekcji:

    ![Lokalizacja pliku Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "lokalizację pliku Web.config")

    *Lokalizacja pliku Web.config*


    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Zadanie 3 — Praca z modelu

Teraz, gdy skonfigurowano już połączenie z bazą danych, zostaną połączone modelu z tabel bazy danych. W ramach tego zadania spowoduje utworzenie klasy, który będzie połączony z bazą danych z Code First. Należy pamiętać, że istnieje istniejących klasy modelu POCO, które powinno zostać zmodyfikowane w.

> [!NOTE]
> Jeśli ukończono ćwiczenie 1 można zauważyć, że ten krok został wykonany przez kreatora. Wykonując Code First, ręcznie utworzysz klasy, które zostaną połączone obiekty danych.


1. Otwórz klasy modelu POCO **Genre** z **modele** folderu projektu i zawiera identyfikatora. Użyj właściwość o nazwie **GenreId**.

    (Fragment - kodu *modeli i dostęp do danych - Genre pierwszy kod Ex2*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Aby pracować z konwencjami Code First, klasa Genre musi mieć właściwości klucza podstawowego, który będzie wykrywane automatycznie.
    > 
    > Możesz przeczytać więcej na temat Konwencji pierwszy kod w tym [artykuł w witrynie msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Teraz Otwórz klasy modelu POCO **albumu** z **modele** folderu projektu i zawierają kluczy obcych, Utwórz właściwości o nazwach **GenreId** i  **ArtistId**. Ta klasa już **GenreId** klucza podstawowego.

    (Fragment - kodu *modeli i dostęp do danych - Ex2 kod pierwszego albumu*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Otwórz klasy modelu POCO **wykonawcy** i obejmują **ArtistId** właściwości.

    (Fragment - kodu *modeli i dostęp do danych - wykonawcy pierwszy kod Ex2*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Kliknij prawym przyciskiem myszy **modele** folderu projektu i wybierz **Dodaj | Klasa**. Nadaj nazwę plikowi **MusicStoreEntities.cs**. Następnie kliknij przycisk **Dodaj.**

    ![Dodawanie klasy](aspnet-mvc-4-models-and-data-access/_static/image20.png "Dodawanie klasy")

    *Dodawanie nowego elementu*

    ![Dodawanie class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Dodawanie class2")

    *Dodawanie klasy*
5. Otwórz właśnie utworzony, klasa **MusicStoreEntities.cs**i uwzględnić przestrzenie nazw **System.Data.Entity** i **System.Data.Entity.Infrastructure**.


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Zastąp deklaracji klasy, aby rozszerzyć **DbContext** klasy: deklarowanie publiczny **DBSet** i zastąpienia **OnModelCreating** — metoda. Po wykonaniu tego kroku wystąpi klasy domeny, która połączy modelu za pomocą programu Entity Framework. Aby to zrobić, Zastąp kod klasy następujące czynności:

    (Fragment - kodu *modeli i dostęp do danych - MusicStoreEntities pierwszy kod Ex2*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

    > [!NOTE]
    > Z programu Entity Framework **DbContext** i **DBSet** można zbadać klasy POCO Genre. Rozszerzając **OnModelCreating** metody, określasz w **kod** jak Genre zostaną zmapowane do tabeli bazy danych. Więcej informacji na temat obiektu DBContext i DBSet można znaleźć w tym artykule w witrynie msdn: [łącza](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Zadanie 4 — kwerendy bazy danych

To zadanie spowoduje zaktualizowanie klasy StoreController tak, aby zamiast dane zapisane na stałe, pobierze go z bazy danych.

> [!NOTE]
> To zadanie jest wspólne ćwiczenie 1.
> 
> Jeśli ukończono ćwiczenie 1 można zauważyć następujące kroki są takie same, w obu podejść (najpierw bazy danych lub najpierw Code). Różnią się one w jaki sposób dane są połączone z modelem dostęp do obiektów danych jest jednak jeszcze przezroczysty z kontrolera.


1. Otwórz **Controllers\StoreController.cs** i dodaj następujące pole do klasy, aby pomieścić wystąpienia **MusicStoreEntities** klasę o nazwie **storeDB**:

    (Fragment - kodu *modeli i dostęp do danych - Ex1 storeDB*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. **MusicStoreEntities** klasy udostępnia właściwości kolekcji dla każdej tabeli w bazie danych. Aktualizacja **Przeglądaj** metody akcji można pobrać określonego rodzaju ze wszystkimi **albumów**.

    (Fragment - kodu *modeli i dostęp do danych - przeglądania magazynu Ex2*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > W przypadku korzystania z możliwości .NET o nazwie **LINQ** (zapytanie o języku zintegrowanym) do zapisania wyrażeń jednoznacznie zapytania względem tych kolekcje — co spowoduje wykonanie kodu w bazie danych i zwracać obiekty można umieszczonych przed.
    > 
    > Aby uzyskać więcej informacji na temat LINQ, odwiedź stronę [witrynę msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres.

    (Fragment - kodu *modeli i Data Access — indeks magazynu Ex2*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres i przekształcenie kolekcji na listę.

    (Fragment - kodu *modeli i dostęp do danych - GenreMenu magazynu Ex2*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

To zadanie będzie sprawdzać, czy strona indeksu magazynu wyświetli Genres przechowywane w bazie danych, zamiast ustalony te. Nie istnieje potrzeba zmiany szablonu widoku, ponieważ **StoreController** zwraca takie same **StoreIndexViewModel** jak poprzednio, ale tym razem dane będą pobierane z bazy danych.

1. Ponowne kompilowanie rozwiązania i naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Sprawdź, czy menu **Genres** nie jest już listy zapisane na stałe, a dane bezpośrednio są pobierane z bazy danych.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Przeglądanie Genres z bazy danych*
3. Teraz przejdź do dowolnego rodzaju i sprawdź, czy albumów zostają wypełnione z bazy danych.

    ![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image23.png "przeglądanie albumów z bazy danych")

    *Przeglądanie albumów z bazy danych*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Ćwiczenie 3: Zapytanie bazy danych z parametrami

W tym ćwiczeniu dowiesz się, jak wykonać zapytanie dotyczące bazy danych przy użyciu parametrów i sposobu użycia, kształtowania wyników zapytania, funkcja, która ogranicza numerów bazy danych uzyskuje dostęp do pobierania danych w bardziej wydajny sposób.

> [!NOTE]
> Aby uzyskać więcej informacji na kształtowania wyników zapytania, można znaleźć w następującej [artykuł w witrynie msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Zadanie 1 - modyfikowanie StoreController pobrać albumów z bazy danych

W tym zadaniu zostanie zmieniony **StoreController** klasy dostęp do bazy danych można pobrać albumów z określonego rodzaju.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** folderu, jeśli chcesz użyć pierwszej kodu metody lub **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** folderu, jeśli chcesz użyć metody pierwszej bazy danych. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz **StoreController** klasę, aby zmienić **Przeglądaj** metody akcji. Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreController.cs**.
3. Zmień **Przeglądaj** metody akcji można pobrać albumów dla określonego rodzaju. Aby to zrobić, Zastąp następujący kod:

    (Fragment - kodu *modeli i dostęp do danych - Ex3 StoreController BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

    > [!NOTE]
    > Aby wypełnić kolekcji jednostki, należy użyć **Include** metodę, aby określić ma zostać pobrane za albumów. Można użyć. **Single()** rozszerzenia LINQ, ponieważ w takim przypadku oczekuje tylko jednego rodzaju albumu. **Single()** metoda przyjmuje jako parametr w takim przypadku określa pojedynczy obiekt Genre w taki sposób, że jego nazwa jest zgodna z wartością zdefiniowane wyrażenia Lambda.
    > 
    > Będzie korzystać z funkcji, które można określić innych powiązanych jednostek, który ma być również załadowany po pobraniu obiektu Genre. Ta funkcja jest nazywana **kształtowania wynik zapytania**i pozwala zmniejszyć liczbę potrzebnych do dostępu do bazy danych można pobrać informacji o. W tym scenariuszu można wstępnie pobrać albumów dla rodzaju pobierania.
    > 
    > Zapytanie zawiera **Genres.Include (&quot;albumów&quot;)** aby wskazać, że ma również albumów pokrewne. To spowoduje większą wydajność aplikacji, ponieważ będą pobierane dane zarówno Genre, jak i albumu w żądaniu pojedynczej bazy danych.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — w uruchomionej aplikacji

W tym zadaniu zostanie Uruchom aplikację i pobrać albumów określonego rodzaju z bazy danych.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/magazynu/Przeglądaj? genre = Pop** można zweryfikować, że wyniki są pobierane z bazy danych.

    ![Przeglądanie według rodzaju](aspnet-mvc-4-models-and-data-access/_static/image24.png "przeglądanie według rodzaju")

    *Przeglądanie/magazynu/Przeglądaj? genre = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Zadanie 3. uzyskiwanie dostępu do albumów według identyfikatora

W tym zadaniu zostanie powtórzony poprzedniej procedury, aby uzyskać albumów według ich identyfikatorów.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do programu Visual Studio. Otwórz **StoreController** klasę, aby zmienić **szczegóły** metody akcji. Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreController.cs**.
2. Zmień **szczegóły** na podstawie metody akcji można pobrać szczegółów albumów ich **identyfikator**. Aby to zrobić, Zastąp następujący kod:

    (Fragment - kodu *modeli i dostęp do danych - Ex3 StoreController DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

W tym zadaniu zostanie uruchomić aplikację w przeglądarce sieci web i Uzyskiwanie szczegółów albumu według ich identyfikatorów.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Na stronie głównej datę rozpoczęcia projektu. Zmień adres URL do **/Store/Details/51** lub gatunki Przeglądaj i wybierz album, aby zweryfikować, że wyniki są pobierane z bazy danych.

    ![Przeglądanie szczegółów](aspnet-mvc-4-models-and-data-access/_static/image25.png "przeglądania szczegółów")

    *Przeglądanie /Store/Details/51*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Poprzez wykonanie tego laboratorium Hands-on, kiedy znasz już podstawy ASP.NET MVC modeli i dostęp do danych, za pomocą **Database First** podejście, jak również **Code First** podejścia:

- Jak dodać bazy danych do rozwiązania, aby można było korzystać z jego danych
- Jak zaktualizować kontrolerów dostarczanie szablonów widoków danych z bazy danych zamiast ustalony
- Jak wykonać zapytanie dotyczące bazy danych przy użyciu parametrów
- Jak używać zapytania wynik Przekształcanie, funkcja, która ogranicza liczbę uzyskuje dostęp do bazy danych, pobieranie danych w bardziej wydajny sposób
- Sposób korzystania z bazy danych imiona i Code First zbliża się do programu Microsoft Entity Framework do połączenia z modelem bazy danych

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji  **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK*&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Instalowanie programu Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "instalacji programu Visual Studio Express")

    *Instalowanie programu Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Zaloguj się do portalu Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Zaloguj się do portalu Windows Azure")

    *Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*
2. Kliknij przycisk **nowy** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczeniowe** | **witryny sieci Web**. Następnie wybierz **szybkie tworzenie** opcji. Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.

    > [!NOTE]
    > Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](aspnet-mvc-4-models-and-data-access/_static/image33.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj na nowe **witryny sieci Web** jest tworzony.
5. Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny. Sprawdź, czy działa nowej witryny sieci Web.

    ![Przeglądanie do nowej witryny sieci web](aspnet-mvc-4-models-and-data-access/_static/image34.png "przeglądanie do nowej witryny sieci web")

    *Przeglądanie do nowej witryny sieci web*

    ![Witryna sieci Web działa](aspnet-mvc-4-models-and-data-access/_static/image35.png "uruchamiania witryny sieci Web")

    *Witryna sieci Web uruchomiona*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie stron witryny sieci web zarządzania](aspnet-mvc-4-models-and-data-access/_static/image36.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie stron zarządzania witryny sieci Web*
7. W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.

    > [!NOTE]
    > *Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.

    ![Pobieranie witryny sieci web profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image37.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz profil publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.

    ![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image38.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji. Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**. Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania. Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.

    ![Pulpit nawigacyjny serwera bazy danych SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny serwera bazy danych SQL*
2. W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) przycisku.

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.

    ![Potwierdzenie zmian](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Potwierdzenie zmian*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.

    ![Publikowanie aplikacji](aspnet-mvc-4-models-and-data-access/_static/image43.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.

    ![Importowanie profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **Weryfikacja połączenia z**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.

    ![Sprawdzanie poprawności połączenia](aspnet-mvc-4-models-and-data-access/_static/image45.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

    - W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.
    - W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.
    - W **hasło** wpisz hasło logowania administratora serwera.
    - Wpisz nazwę nowej bazy danych.

    ![Konfigurowanie parametrów połączenia z lokalizacją docelową](aspnet-mvc-4-models-and-data-access/_static/image47.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")

    *Konfigurowanie parametrów połączenia z lokalizacją docelową*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.

    ![Tworzenie bazy danych](aspnet-mvc-4-models-and-data-access/_static/image48.png "tworzenie parametry bazy danych")

    *Tworzenie bazy danych*
7. Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie. Następnie kliknij przycisk **Dalej**.

    ![Parametry połączenia wskazujące bazę danych SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "ciąg połączenia wskazujące bazę danych SQL")

    *Parametry połączenia wskazujące bazę danych SQL*
8. W **Podgląd** kliknij przycisk **publikowania**.

    ![Publikowanie aplikacji sieci web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: korzystania z wstawek kodu

Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki. Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.

![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-models-and-data-access/_static/image51.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*

***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragment (bez spacji i łączniki).
3. Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.
4. Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-models-and-data-access/_static/image52.png "zacznij wpisywać nazwę wstawki programu")

*Rozpocznij wpisywanie nazwy fragment kodu*

![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-models-and-data-access/_static/image53.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-models-and-data-access/_static/image54.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")

*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*

***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-models-and-data-access/_static/image55.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")

*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-models-and-data-access/_static/image56.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
