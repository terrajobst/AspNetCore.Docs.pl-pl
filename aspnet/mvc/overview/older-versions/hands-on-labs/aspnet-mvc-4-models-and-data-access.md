---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 — modele i dostęp do danych | Dokumentacja firmy Microsoft
author: rick-anderson
description: 'Uwaga: W tym laboratorium praktyczne przyjęto założenie, że masz podstawową wiedzę na temat platformy ASP.NET MVC. Jeśli nie używasz platformy ASP.NET MVC przed, zalecamy zapoznać się z platformy ASP.NET MVC 4...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: afc03d87431632bbb3ab59241de0edb4bb7af12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371132"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 — modele i dostęp do danych

Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](https://aka.ms/webcamps-training-kit)

W tym laboratorium praktyczne przyjęto założenie, masz podstawową wiedzę na temat **platformy ASP.NET MVC**. Jeśli nie używasz **platformy ASP.NET MVC** , zalecamy zapoznać się z **podstawowe informacje dotyczące programu ASP.NET MVC 4** warsztatów.

W tym laboratorium przeprowadzi Cię przez ulepszeń i nowych funkcji, które opisano wcześniej, stosując drobne zmiany do przykładowej aplikacji sieci Web, pod warunkiem w folderze źródłowym.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzne dla tego laboratorium znajduje się w temacie [platformy ASP.NET MVC 4 modele i dostęp do danych](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

W **podstawowe informacje dotyczące platformy ASP.NET MVC** warsztatów, możesz mieć zostały przekazywania zakodowanych danych z kontrolerów do Przeglądanie szablonów. Jednak aby można było tworzenie rzeczywistych aplikacji sieci Web, możesz chcieć użyć rzeczywista baza danych.

W tym laboratorium praktyczne opisano, jak za pomocą aparatu bazy danych, aby można było przechowywać i pobierać dane potrzebne do aplikacji Music Store. To zrobić, możesz uruchomić przy użyciu istniejącej bazy danych i utworzone na podstawie modelu Entity Data Model. W tym środowisku laboratoryjnym będzie spełniać **Database First** podejście, jak również **Code First** podejście.

Jednak możesz również użyć **pierwszy Model** podejście, utwórz ten sam model przy użyciu narzędzi, a następnie wygeneruj bazy danych z niego.

![Vs pierwszej bazy danych. Pierwszy model](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First w programie vs. Najpierw modelu")

*Vs pierwszej bazy danych. Najpierw modelu*

Po wygenerowaniu modelu, wprowadzisz odpowiednie korekty w StoreController zapewnienie widoków Store przy użyciu danych pobranych z bazy danych, zamiast korzystać z zakodowanych danych. Nie należy wprowadzać zmian do szablonów widoku ponieważ StoreController będzie zwracać ten sam modele widoków do Przeglądanie szablonów, mimo że w tej chwili dane będą pochodzić z bazy danych.

**Pierwszym sposobem kodu**

Podejścia Code First pozwala zdefiniować model z kodu bez generowania klasy, które są zazwyczaj powiązane z framework.

W kodzie, najpierw obiekty modelu są definiowane za pomocą POCOs, &quot;zwykłych starych obiektów CLR&quot;. POCOs są proste zwykły klasy, które nie dziedziczenia i nie należy implementować interfejsy. Firma Microsoft może automatycznie generować bazy danych z nich lub możemy Użyj istniejącej bazy danych i wygenerować mapowania klasy z kodu.

Korzyści z używania tej metody jest modelu pozostają niezależne od framework trwałości (w tym przypadku Entity Framework), zgodnie z klasy POCOs nie są powiązane z architekturą mapowania.

> [!NOTE]
> To laboratorium jest oparte na programie ASP.NET MVC 4 i wersję aplikacji przykładowej Music Store dostosowanej zminimalizowane, aby dopasować tylko te funkcje, które są wyświetlane w tym laboratorium praktyczne.
> 
> Jeśli chcesz zapoznać się z całym **Music Store** aplikacją z samouczka można znaleźć w [MVC — muzyka-Store](https://github.com/evilDave/MVC-Music-Store).


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

Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek C: za pomocą wstawek kodu programu](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

W tym laboratorium praktyczne składa się przez następujących czynnościach:

1. [Ćwiczenie 1: Dodawanie bazy danych](#Exercise1)
2. [Ćwiczenie 2: Tworzenie bazy danych za pomocą funkcji Code First](#Exercise2)
3. [Ćwiczenie 3: Zapytanie do bazy danych z parametrami](#Exercise3)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia. Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.


Szacowany czas do ukończenia tego laboratorium: **35 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Ćwiczenie 1: Dodawanie bazy danych

W tym ćwiczeniu dowiesz się, jak dodać bazę danych przy użyciu tabel aplikacji MusicStore do rozwiązania w celu korzystania z danych. Gdy baza danych jest generowane przy użyciu modelu i dodany do rozwiązania, należy zmodyfikować klasy StoreController, aby udostępnić szablon widoku danych pobranych z bazy danych, zamiast używać zakodowanych wartości.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Zadanie 1 — Dodawanie bazy danych

W tym zadaniu należy dodać już utworzoną bazę danych z głównego tabelami aplikacji MusicStore do rozwiązania.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex1-AddingADatabaseDBFirst/rozpoczęcia/** folderu.

   1. Musisz pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Dodaj **MvcMusicStore** plik bazy danych. W tym laboratorium praktyczne użyje zostały już utworzone bazy danych o nazwie **MvcMusicStore.mdf**. Aby to zrobić, kliknij prawym przyciskiem myszy **aplikacji\_danych** folderu, wskaż **Dodaj** a następnie kliknij przycisk **istniejący element**. Przejdź do **\Source\Assets** i wybierz **MvcMusicStore.mdf** pliku.

    ![Dodawanie istniejącego elementu](aspnet-mvc-4-models-and-data-access/_static/image2.png "Dodawanie istniejącego elementu")

    *Dodawanie istniejącego elementu*

    ![Plik bazy danych MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf plik bazy danych")

    *Plik bazy danych MvcMusicStore.mdf*

    Baza danych została dodana do projektu. Nawet wtedy, gdy baza danych znajduje się wewnątrz rozwiązania, możesz zapytania i aktualizować w miarę była hostowana na serwerze innej bazy danych.

    ![MvcMusicStore bazy danych, w Eksploratorze rozwiązań](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore bazy danych, w Eksploratorze rozwiązań")

    *MvcMusicStore bazy danych, w Eksploratorze rozwiązań*
3. Sprawdź połączenie z bazą danych. Aby to zrobić, kliknij dwukrotnie **MvcMusicStore.mdf** nawiązać połączenie.

    ![Nawiązywanie połączenia z MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "nawiązywania połączenia z MvcMusicStore.mdf")

    *Nawiązywanie połączenia z MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Zadanie 2 — Tworzenie modelu danych

W tym zadaniu utworzysz model danych do interakcji z bazy danych dodane w poprzednim zadaniu.

1. Tworzenie modelu danych, która będzie reprezentowała bazy danych. Aby to zrobić, w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modeli** folderu, wskaż **Dodaj** a następnie kliknij przycisk **nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **danych** szablonu i następnie **ADO.NET Entity Data Model** elementu. Zmień nazwę modelu danych, aby **StoreDB.edmx** i kliknij przycisk **Dodaj**.

    ![Dodawanie modelu danych jednostki ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "Dodawanie modelu danych jednostki ADO.NET StoreDB")

    *Dodawanie modelu danych jednostki ADO.NET StoreDB*
2. **Kreator modelu Entity Data Model** będą wyświetlane. Ten kreator przeprowadzi Cię przez tworzenie warstwy modelu. Ponieważ modelu należy tworzyć w oparciu o istniejące recentyl bazy danych dodane, wybierz opcję **Generuj z bazy danych** i kliknij przycisk **dalej**.

    ![Wybieranie zawartości modelu](aspnet-mvc-4-models-and-data-access/_static/image7.png "wybierania zawartości modelu")

    *Wybieranie modelu zawartości*
3. Ponieważ model jest generowany z bazy danych, należy określić tego połączenia. Kliknij przycisk **nowe połączenie**.
4. Wybierz **plik bazy danych programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**.

    ![Wybierz źródło danych](aspnet-mvc-4-models-and-data-access/_static/image8.png "wybierz źródło danych")

    *Wybierz źródło danych w oknie dialogowym*
5. Kliknij przycisk **Przeglądaj** i wybierz bazę danych **MvcMusicStore.mdf** zlokalizowanego w **aplikacji\_danych** folder i kliknij przycisk **OK**.

    ![Właściwości połączenia](aspnet-mvc-4-models-and-data-access/_static/image9.png "właściwości połączenia")

    *Właściwości połączenia*
6. Wygenerowana klasa powinna mieć taką samą nazwę jak parametry połączenia obiektu, więc zmień jego nazwę na **MusicStoreEntities** i kliknij przycisk **dalej**.

    ![Wybieranie połączenia danych](aspnet-mvc-4-models-and-data-access/_static/image10.png "Wybieranie połączenia danych")

    *Wybieranie połączenia danych*
7. Wybierz obiekty bazy danych do użycia. Zgodnie z modelem jednostki będą używane tylko tabele bazy danych, wybierz **tabel** opcji i upewnij się, że **zawierać kolumny klucza obcego w modelu** i **Pluralize lub końcówek wygenerowany nazwy obiektów** również są zaznaczone opcje. Zmień Namespace modelu do **MvcMusicStore.Model** i kliknij przycisk **Zakończ**.

    ![Wybieranie obiektów bazy danych](aspnet-mvc-4-models-and-data-access/_static/image11.png "wybierania obiektów bazy danych")

    *Wybieranie obiektów bazy danych*

    > [!NOTE]
    > Jeśli zostanie wyświetlone okno dialogowe z ostrzeżeniem zabezpieczeń, kliknij przycisk **OK** do uruchamiania szablonu i Generuj klasy dla jednostki modelu.
8. Diagram jednostek bazy danych pojawi się i gdy osobna klasę, która mapuje każdej tabeli w bazie danych zostanie utworzony. Na przykład **albumów** tabeli będą reprezentowane przez **albumu** klasy, w której każda kolumna w tabeli będzie zmapowana do właściwości klasy. Pozwoli to zapytanie i Praca z obiektami, które reprezentują wierszy w bazie danych.

    ![Diagram jednostek](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagram jednostki")

    *Diagram jednostek*

    > [!NOTE]
    > Szablony T4 (.tt) uruchamiać kod służący do generowania klasy jednostek i spowoduje zastąpienie istniejących klas o takiej samej nazwie. W tym przykładzie klasy &quot;albumu&quot;, &quot;gatunku&quot; i &quot;wykonawcy&quot; zostały zastąpione wygenerowanego kodu.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Zadanie 3 — Tworzenie aplikacji

W tym zadaniu można sprawdzi, mimo że generowania modelu zostały usunięte **albumu**, **gatunku** i **wykonawcy** klasy modeli, projekt zostanie skompilowany poprawnie przy użyciu nowe klasy modelu danych.

1. Skompiluj projekt, wybierając **kompilacji** element menu i następnie **kompilacji MvcMusicStore**.

    ![Tworzenie projektu](aspnet-mvc-4-models-and-data-access/_static/image13.png "kompilowania projektu")

    *Tworzenie projektu*
2. Projekt zostanie skompilowany poprawnie. Dlaczego to nadal działa? To działa, ponieważ tabele bazy danych ma pola, które zawierają właściwości, które były używane w klasach usunięto **albumu** i **gatunku**.

    ![Kompilacje, które zakończyło się pomyślnie](aspnet-mvc-4-models-and-data-access/_static/image14.png "kompilacji zakończyło się pomyślnie")

    *Kompilacje, które zakończyło się pomyślnie*
3. Gdy Projektant wyświetla obiekty w formacie diagram, są one naprawdę klas języka C#. Rozwiń **StoreDB.edmx** węzła w Eksploratorze rozwiązań i następnie **StoreDB.tt**, zobaczysz nowe jednostki wygenerowany.

    ![Wygenerowane pliki](aspnet-mvc-4-models-and-data-access/_static/image15.png "wygenerowanych plików")

    *Wygenerowane pliki*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Zadanie 4 — zapytanie do bazy danych

W tym zadaniu zostanie zaktualizować klasy StoreController tak, aby zamiast korzystać z danych zapisane na stałe, zostanie wykonane zapytanie względem bazy danych, aby pobrać informacje.

1. Otwórz **Controllers\StoreController.cs** i dodaj następujące pola do klasy, aby pomieścić wystąpienie **MusicStoreEntities** klasę o nazwie **storeDB**:

    (Code Snippet — *modele i dostęp do danych — Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. **MusicStoreEntities** klasa udostępnia właściwości kolekcji dla każdej tabeli w bazie danych. Aktualizacja **Przeglądaj** metody akcji, które można pobrać określonego rodzaju ze wszystkimi **albumów**.

    (Code Snippet — *modele i dostęp do danych — Przeglądaj Store Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > W przypadku korzystania z możliwości platformy .NET o nazwie **LINQ** (zapytanie o języku zintegrowanym) do pisania wyrażeń zapytań silnie typizowane względem te kolekcje — co spowoduje wykonanie kodu w bazie danych i zwracają obiekty, że można programować względem.
    > 
    > Aby uzyskać więcej informacji na temat LINQ, odwiedź [witryny msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie gatunki.

    (Code Snippet — *modele i dostęp do danych — indeks Store Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie gatunki i Przekształć kolekcji na listę.

    (Code Snippet — *modele i dostęp do danych — Ex1 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

W ramach tego zadania będzie sprawdzać strony indeksu Store będą teraz wyświetlane gatunki przechowywanych w bazie danych, zamiast zakodowaną z nich. Nie ma potrzeby zmiany szablon widoku, ponieważ **StoreController** zwraca tych samych jednostek tak jak poprzednio, ale tym razem dane będą pochodzić z bazy danych.

1. Ponownie skompiluj rozwiązanie, a następnie naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Upewnij się, że menu **gatunki** nie jest już listę zapisane na stałe, a dane bezpośrednio są pobierane z bazy danych.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Przeglądanie gatunki z bazy danych*
3. Teraz przejdź do dowolnego rodzaju i sprawdź, czy albumów zostają wypełnione z bazy danych.

    ![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image17.png "przeglądania albumów z bazy danych")

    *Przeglądanie albumów z bazy danych*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Ćwiczenie 2: Tworzenie bazy danych przy użyciu kodu najpierw

W tym ćwiczeniu dowiesz się, jak utworzyć bazę danych z tabelami aplikacji MusicStore za pomocą podejścia Code First i jak uzyskać dostęp do swoich danych.

Po wygenerowaniu modelu zmodyfikujesz StoreController, aby zapewnić wyświetlanie szablonu z danymi z bazy danych, a nie przy użyciu wartości zapisane na stałe.

> [!NOTE]
> Jeśli ukończono wykonywanie 1 i już mający doświadczenie z podejścia Database First, będzie teraz dowiesz się, jak uzyskać te same wyniki za pomocą innego procesu. Aby ułatwić odczytywanie Twojego zostały oznaczone zadania, które są wspólne dla ćwiczenie 1. Jeśli nie zostały wypełnione wykonywania 1, ale chcesz dowiedzieć się podejścia Code First, możesz rozpocząć od tego ćwiczenia i uzyskać pełne tego tematu.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Zadanie 1 — wypełniania przykładowych danych

W ramach tego zadania spowoduje wypełnienie bazy danych z przykładowymi danymi tworzonemu zasobowi początkowemu przy użyciu najpierw kod.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex2-CreatingADatabaseCodeFirst/rozpoczęcia/** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Dodaj **SampleData.cs** plik **modeli** folderu. Aby to zrobić, kliknij prawym przyciskiem myszy **modeli** folderu, wskaż **Dodaj** a następnie kliknij przycisk **istniejący element**. Przejdź do **\Source\Assets** i wybierz **SampleData.cs** pliku.

    ![Przykładowe dane Wypełnij kodu](aspnet-mvc-4-models-and-data-access/_static/image18.png "przykładowych danych wypełnienia kodem")

    *Przykładowe dane Wypełnij kodu*
3. Otwórz **Global.asax.cs** pliku i Dodaj następujący kod *przy użyciu* instrukcji.

    (Code Snippet — *modele i dostęp do danych — deklaracje Using Ex2 globalnego Asax*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. W **aplikacji\_Start()** metoda Dodaj poniższy wiersz, aby ustawić inicjatora bazy danych.

    (Code Snippet — *modele i dostęp do danych — globalne Asax SetInitializer Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Zadanie 2 — Konfigurowanie połączenia z bazą danych

Skoro już dodano bazę danych do naszego projektu, trzeba napisać **Web.config** pliku parametrów połączenia.

1. Dodaj parametry połączenia w **Web.config**. Aby to zrobić, otwórz **Web.config** w katalogu głównym projektu i Zastąp parametry połączenia o nazwie DefaultConnection ten wiersz w **&lt;connectionStrings&gt;** sekcji:

    ![Lokalizacja pliku Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "lokalizacji pliku Web.config")

    *Lokalizacja pliku Web.config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Zadanie 3 — Praca z modelem

Teraz, gdy skonfigurowano już połączenie z bazą danych, połączysz modelu z tabel bazy danych. W tym zadaniu utworzysz klasy, które zostanie połączone z bazą danych przy użyciu Code First. Należy pamiętać, że jest celowe klasy modelu POCO powinien być modyfikowany.

> [!NOTE]
> Jeśli ukończono wykonywanie 1 można zauważyć, że ten krok został wykonany przez kreatora. Wykonanie tych czynności Code First, ręcznie utworzysz klas, które zostaną połączone z jednostkami danych.

1. Otwórz klasę modelu POCO **gatunku** z **modeli** folderu projektu i zawierać tego identyfikatora. Użyj właściwości int o nazwie **GenreId**.

    (Code Snippet — *modele i dostęp do danych — gatunku pierwszy kod Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Aby pracować z Konwencji Code First, klasa gatunku musi mieć właściwość klucza podstawowego, który zostanie wykryty automatycznie.
    > 
    > Więcej informacji dotyczących Konwencji pierwsze kodu, w tym [artykuł w witrynie msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Teraz Otwórz klasę modelu POCO **albumu** z **modeli** folderu projektu i zawierają kluczy obcych, tworzenie właściwości z nazwami **GenreId** i  **ArtistId**. Ta klasa jeszcze **GenreId** dla klucza podstawowego.

    (Code Snippet — *modele i dostęp do danych — Ex2 kodu pierwszego albumu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Otwórz klasę modelu POCO **wykonawcy** i obejmują **ArtistId** właściwości.

    (Code Snippet — *modele i dostęp do danych — wykonawcy pierwszy kod Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Kliknij prawym przyciskiem myszy **modeli** folderu projektu i wybierz pozycję **Dodaj | Klasa**. Nadaj plikowi nazwę **MusicStoreEntities.cs**. Następnie kliknij przycisk **Dodaj.**

    ![Dodawanie klasy](aspnet-mvc-4-models-and-data-access/_static/image20.png "Dodawanie klasy")

    *Dodawanie nowego elementu*

    ![Dodawanie klasa2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Dodawanie klasa2")

    *Dodawanie klasy*
5. Otwórz klasę właśnie utworzony, **MusicStoreEntities.cs**i zawierać przestrzenie nazw **System.Data.Entity** i **System.Data.Entity.Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Zastąp deklaracji klasy, aby rozszerzyć **DbContext** klasy: deklarowanie publiczny **DBSet** i zastąpić **OnModelCreating** metody. Po wykonaniu tego kroku wystąpi klasę domeny, która połączy modelu za pomocą programu Entity Framework. Aby to zrobić, Zastąp kod klasy następujących czynności:

    (Code Snippet — *modele i dostęp do danych — MusicStoreEntities pierwszy kod Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Za pomocą platformy Entity Framework **DbContext** i **DBSet** można zbadać klasy POCO gatunku. Rozszerzając **OnModelCreating** metody, określasz w **kodu** odwzorowania gatunku do tabeli bazy danych. Więcej informacji na temat typu DBContext i DBSet można znaleźć w artykule msdn: [łącza](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Zadanie 4 — zapytanie do bazy danych

W tym zadaniu zostanie zaktualizować klasy StoreController, tak, aby zamiast korzystać z danych zapisane na stałe, pobierze go z bazy danych.

> [!NOTE]
> To zadanie jest wspólnych ćwiczenie 1.
> 
> Jeśli wykonano 1 ćwiczenia można zauważyć te kroki są takie same w obu metod (najpierw bazy danych lub najpierw Code). Są one różne, w jaki sposób dane są połączone za pomocą modelu, ale dostęp do jednostek danych jest jeszcze przezroczyste z kontrolera.


1. Otwórz **Controllers\StoreController.cs** i dodaj następujące pola do klasy, aby pomieścić wystąpienie **MusicStoreEntities** klasę o nazwie **storeDB**:

    (Code Snippet — *modele i dostęp do danych — Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. **MusicStoreEntities** klasa udostępnia właściwości kolekcji dla każdej tabeli w bazie danych. Aktualizacja **Przeglądaj** metody akcji, które można pobrać określonego rodzaju ze wszystkimi **albumów**.

    (Code Snippet — *modele i dostęp do danych — Przeglądaj Store Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > W przypadku korzystania z możliwości platformy .NET o nazwie **LINQ** (zapytanie o języku zintegrowanym) do pisania wyrażeń zapytań silnie typizowane względem te kolekcje — co spowoduje wykonanie kodu w bazie danych i zwracają obiekty, że można programować względem.
    > 
    > Aby uzyskać więcej informacji na temat LINQ, odwiedź [witryny msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie gatunki.

    (Code Snippet — *modele i dostęp do danych — indeks Store Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie gatunki i Przekształć kolekcji na listę.

    (Code Snippet — *modele i dostęp do danych — Ex2 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 działania aplikacji.

W ramach tego zadania będzie sprawdzać strony indeksu Store będą teraz wyświetlane gatunki przechowywanych w bazie danych, zamiast zakodowaną z nich. Nie ma potrzeby zmiany szablon widoku, ponieważ **StoreController** zwraca takie same **StoreIndexViewModel** tak jak poprzednio, ale tym razem, dane będą pochodzić z bazy danych.

1. Ponownie skompiluj rozwiązanie, a następnie naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Upewnij się, że menu **gatunki** nie jest już listę zapisane na stałe, a dane bezpośrednio są pobierane z bazy danych.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Przeglądanie gatunki z bazy danych*
3. Teraz przejdź do dowolnego rodzaju i sprawdź, czy albumów zostają wypełnione z bazy danych.

    ![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image23.png "przeglądania albumów z bazy danych")

    *Przeglądanie albumów z bazy danych*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Ćwiczenie 3: Zapytanie do bazy danych z parametrami

W tym ćwiczeniu dowiesz się, jak wykonać zapytanie bazy danych przy użyciu parametrów i sposobu używania kształtowania wyników zapytania, funkcja, która zmniejsza się liczba bazy danych uzyskuje dostęp do pobierania danych w bardziej efektywny sposób.

> [!NOTE]
> Więcej informacji na temat kształtowania wyników kwerendy, można znaleźć w następującej [artykuł w witrynie msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Zadanie 1 — modyfikowanie StoreController pobrać albumów z bazy danych

W ramach tego zadania zostanie zmieniony **StoreController** klasy dostępu do bazy danych można pobrać ze zdjęciami z określonego rodzaju.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** folderu, jeśli chcesz użyć podejścia najpierw kod lub **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** folderu, jeśli chcesz użyć podejścia bazy danych. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Otwórz **StoreController** klasy, aby zmienić **Przeglądaj** metody akcji. Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie plik **StoreController.cs**.
3. Zmiana **Przeglądaj** metody akcji, które można pobrać albumów dla określonego rodzaju. Aby to zrobić, Zastąp następujący kod:

    (Code Snippet — *modele i dostęp do danych — Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Aby wypełnić kolekcji jednostki, musisz użyć **Include** metodę, aby określić, ma zostać pobrane za albumów. Można użyć. **Funkcja Single()** rozszerzenia w składniku LINQ, ponieważ w tym przypadku tylko jeden gatunku oczekuje albumu. **Funkcja Single()** metoda przyjmuje wyrażenia Lambda jako parametru, w tym przypadku określa pojedynczy obiekt gatunku w taki sposób, że jego nazwa jest zgodna wartość zdefiniowana.
> 
> Będzie korzystać z funkcji, które można określić innych powiązanych jednostek, który ma być również załadowany po pobraniu obiektu gatunku. Ta funkcja jest nazywana **kształtowania wynik kwerendy**i pozwala zmniejszyć liczbę potrzebnych do dostępu do bazy danych, aby pobrać informacje. W tym scenariuszu można wstępnie pobrać albumów dla tego gatunku, możesz pobrać.
> 
> Zapytanie zawiera **Genres.Include (&quot;albumów&quot;)** aby wskazać, że ma także powiązane ze zdjęciami. To spowoduje aplikacji bardziej efektywne, ponieważ pobierze dane gatunku i albumów w żądaniu pojedynczej bazy danych.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — uruchamianie aplikacji

W ramach tego zadania spowoduje uruchomienia aplikacji i pobrać albumów określonego rodzaju z bazy danych.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/Store/przeglądania? gatunku = Pop** Aby sprawdzić, czy wyniki są pobierane z bazy danych.

    ![Przeglądanie według gatunku](aspnet-mvc-4-models-and-data-access/_static/image24.png "przeglądanie według gatunku")

    *Przeglądanie/Store/przeglądania? gatunku = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Zadanie 3. uzyskiwanie dostępu do albumów według identyfikatora

W tym zadaniu zostanie Powtórz poprzedniej procedury, aby uzyskać albumów według ich identyfikatorów.

1. Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do programu Visual Studio. Otwórz **StoreController** klasy, aby zmienić **szczegóły** metody akcji. Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie plik **StoreController.cs**.
2. Zmiana **szczegóły** na podstawie metody akcji, które można pobrać szczegółów albumów ich **identyfikator**. Aby to zrobić, Zastąp następujący kod:

    (Code Snippet — *modele i dostęp do danych — Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

W ramach tego zadania spowoduje uruchomienia aplikacji w przeglądarce sieci web i Uzyskiwanie szczegółów albumu według ich identyfikatorów.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Data rozpoczęcia projektu na stronie głównej. Zmień adres URL do **/Store/Details/51** lub gatunki Przeglądaj i wybierz pozycję album, aby sprawdzić, czy wyniki są pobierane z bazy danych.

    ![Przeglądanie szczegółów](aspnet-mvc-4-models-and-data-access/_static/image25.png "Przeglądanie szczegółowe informacje")

    *Przeglądanie /Store/Details/51*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację do witryny sieci Web systemu Windows Azure następujące [dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy](#AppendixB).

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Przez ukończenie tego laboratorium praktyczne, kiedy znasz już podstawy platformy ASP.NET MVC modele i dostęp do danych, przy użyciu **Database First** podejście, jak również **Code First** podejścia:

- Jak dodać bazę danych do rozwiązania w celu korzystania z danych
- Jak zaktualizować kontrolerów, aby zapewnić wyświetlanie szablonów przy użyciu danych pobranych z bazy danych zamiast zakodowanych
- Tworzenie zapytań względem bazy danych przy użyciu parametrów
- Jak używać zapytania wynik kształtowania, funkcja, która zmniejsza liczbę uzyskuje dostęp do bazy danych, pobieranie danych w sposób bardziej efektywny
- Sposób użycia podejścia Database First i Code First platformy Entity Framework Microsoft połączyć bazę danych przy użyciu modelu

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "zainstalować program Visual Studio Express")

    *Zainstaluj program Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Zaloguj się do portalu usługi Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Zaloguj się do portalu usługi Windows Azure")

    *Zaloguj się do portalu zarządzania systemu Azure Windows*
2. Kliknij przycisk **New** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczenia** | **witryny sieci Web**. Następnie wybierz pozycję **szybkie tworzenie** opcji. Podaj adres URL dostępny dla nowej witryny sieci web, a następnie kliknij przycisk **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Witryny sieci Web Windows Azure jest hostem dla aplikacji sieci web działające w chmurze, które można kontrolować i zarządzać nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do Windows Azure witryny sieci Web z poza portalem. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkiego tworzenia](aspnet-mvc-4-models-and-data-access/_static/image33.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj, aż nowe **witryny sieci Web** zostanie utworzony.
5. Po utworzeniu witryny sieci Web kliknij link w obszarze **adresu URL** kolumny. Sprawdź, czy działa nową witrynę sieci Web.

    ![Przejście do nowej witryny sieci web](aspnet-mvc-4-models-and-data-access/_static/image34.png "przejście do nowej witryny sieci web")

    *Przejście do nowej witryny sieci web*

    ![Witryna sieci Web działa](aspnet-mvc-4-models-and-data-access/_static/image35.png "witryna sieci Web działa")

    *Witryna sieci Web działa*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie strony zarządzania witryny sieci web](aspnet-mvc-4-models-and-data-access/_static/image36.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie strony zarządzania witryny sieci Web*
7. W **pulpit nawigacyjny** w obszarze **Przegląd** kliknij **Pobierz profil publikowania** łącza.

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web w witrynie sieci Web usługi Windows Azure dla poszczególnych metod włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do łączenia się i uwierzytelnianie w odniesieniu do każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** Obsługa odczytywania profili publikowania do automatyzowania konfiguracji tych programów dla Publikowanie aplikacji sieci web w usłudze Windows Azure websites.

    ![Pobieranie witryny sieci web profil publikowania](aspnet-mvc-4-models-and-data-access/_static/image37.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz plik profilu publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do Windows Azure Web Sites z programu Visual Studio za pomocą tego pliku.

    ![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image38.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, musisz utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Należy serwera usługi SQL Database do przechowywania bazy danych aplikacji. Możesz wyświetlić serwery baz danych SQL z subskrypcji w portalu zarządzania Azure Windows pod **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**. Jeśli nie masz serwer, który został utworzony, można utworzyć ją przy użyciu **Dodaj** przycisk na pasku poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, ponieważ będziesz ich używać w kolejne zadania podrzędne. Nie należy tworzyć bazy danych, ponieważ zostanie on utworzony w późniejszym terminie.

    ![Pulpit nawigacyjny z serwera bazy danych SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny z serwera bazy danych SQL*
2. W ramach następnego zadania spowoduje przetestowanie połączenia z bazą danych z programu Visual Studio, dlatego trzeba uwzględnić adres IP lokalnego serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżący adres IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) przycisku.

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP listy, kliknij pozycję **Zapisz** aby potwierdzić zmiany.

    ![Potwierdź zmiany](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Potwierdź zmiany*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **Publikuj**.

    ![Publikowanie aplikacji](aspnet-mvc-4-models-and-data-access/_static/image43.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Importuj profil publikowania, zapisaną w pierwszym zadaniem.

    ![Trwa importowanie profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image44.png "importowania profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **sprawdzanie poprawności połączenia**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Sprawdzanie poprawności zostało ukończone, gdy zostanie wyświetlony z zielonym znacznikiem wyboru pojawiają się obok przycisku Waliduj połączenie.

    ![Sprawdzanie poprawności połączenia](aspnet-mvc-4-models-and-data-access/_static/image45.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk obok pola tekstowego połączenia bazy danych (czyli **DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W **nazwy serwera** wpisz swoją bazę danych SQL server adresu URL przy użyciu *tcp:* prefiks.
   - W **nazwa_użytkownika** wpisz nazwę logowania administratora serwera.
   - W **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nazwę nowej bazy danych.

     ![Konfigurowanie parametrów połączenia z docelowym](aspnet-mvc-4-models-and-data-access/_static/image47.png "Konfigurowanie parametrów połączenia z docelowym")

     *Konfigurowanie parametrów połączenia z docelowym*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu, aby utworzyć kliknij bazę danych **tak**.

    ![Tworzenie bazy danych](aspnet-mvc-4-models-and-data-access/_static/image48.png "Tworzenie parametrów bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do łączenia z bazą danych SQL na platformie Windows Azure jest wyświetlana w polu tekstowym połączenia domyślne. Następnie kliknij przycisk **Dalej**.

    ![Ciąg połączenia wskazujący bazę danych SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "ciąg połączenia wskazujący bazę danych SQL")

    *Ciąg połączenia wskazujący bazę danych SQL*
8. W **Podgląd** kliknij **Publikuj**.

    ![Publikowanie aplikacji sieci web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: korzystania z fragmentów kodu

Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki. Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.

![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](aspnet-mvc-4-models-and-data-access/_static/image51.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*

***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).
3. Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.
4. Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragmentu kodu](aspnet-mvc-4-models-and-data-access/_static/image52.png "Rozpocznij wpisywanie nazwy fragmentu kodu")

*Rozpocznij wpisywanie nazwy fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](aspnet-mvc-4-models-and-data-access/_static/image53.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](aspnet-mvc-4-models-and-data-access/_static/image54.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")

*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*

***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](aspnet-mvc-4-models-and-data-access/_static/image55.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")

*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-models-and-data-access/_static/image56.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
