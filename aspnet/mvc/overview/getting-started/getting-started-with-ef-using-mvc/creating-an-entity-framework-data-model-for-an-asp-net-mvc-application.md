---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Wprowadzenie do programu Entity Framework 6 Code First wykorzystaniem MVC 5 | Dokumentacja firmy Microsoft
author: tdykstra
ms.author: riande
ms.date: 12/04/2018
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c7ab9458f83e05af84f72d9a2519a8c1c39b84b5
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861436"
---
# <a name="get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Rozpoczynanie pracy z usługą Entity Framework 6 Code First wykorzystaniem MVC 5

przez [Tom Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> [!NOTE]
> W nowych wdrożeniach, firma Microsoft zaleca [stronami ASP.NET Core Razor](/aspnet/core/razor-pages) za pośrednictwem widoków i kontrolerów platformy ASP.NET MVC. Podobny do tej serii samouczków jest dostępny dla stron Razor [Samouczek stron Razor](/aspnet/core/tutorials/razor-pages/razor-pages-start):
>
> * Łatwiej jest je wykonać.
> * Zapewnia więcej najlepszych rozwiązań programu EF Core.
> * Używa wydajniejszych zapytań.
> * Jest bardziej aktualny przy użyciu najnowszych interfejsu API.
> * Obejmuje więcej funkcji.
> * Jest preferowanym podejściem w przypadku nowych wdrożeń aplikacji.

> W tym artykule przedstawiono sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu platformy Entity Framework 6 i Visual Studio. Ten samouczek używa kodu pierwszego przepływu pracy. Aby dowiedzieć się, jak dokonać wyboru między Code First Database First i pierwszego modelu, zobacz [utworzyć model](/ef/ef6/modeling/).
>
> Przykładowa aplikacja jest witryną sieci web dla fikcyjnej uniwersytetu o nazwie Contoso University. Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora. Tej serii samouczków opisano sposób tworzenia przykładowej aplikacji Contoso University. Możesz [pobrać gotową aplikację](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
>
> Dostępna jest wersja języka Visual Basic, tłumaczyć Mike Brind: [MVC 5 z programów EF 6 w języku Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) witrynie Mikesdotnetting.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - [Entity Framework 6](https://www.nuget.org/packages/EntityFramework)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (opcjonalnie)
>
> ## <a name="tutorial-versions"></a>Samouczek wersji
>
> Dla poprzednich wersji po ukończeniu tego samouczka, zobacz [EF 4.1 / MVC 3-e-book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) i [rozpoczęcie korzystania z programów EF 5 za pomocą MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
>
> ## <a name="questions-and-comments"></a>Pytania i komentarze
>
> Wystaw opinię w sposób zbędne w tym samouczku i co można było ulepszyć proces przy użyciu komentarze w dolnej części strony. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx) lub [StackOverflow.com](http://stackoverflow.com/).
>
> Jeśli napotkasz problem, którego nie można rozpoznać rozwiązanie tego problemu można znaleźć zwykle porównując kodu do projektu ukończona, który można pobrać. Niektóre typowe błędy i sposobu rozwiązania tych problemów można znaleźć [typowych błędów i rozwiązania lub obejścia](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors).

## <a name="the-contoso-university-web-app"></a>Aplikacja sieci web firmy Contoso University

Aplikacja, którą utworzysz w tych samouczkach to proste university witryna sieci web. Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów. Poniżej przedstawiono kilka ekranów, które zostaną utworzone:

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Edytowanie ucznia](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Tak, aby samouczka można skoncentrować się głównie na temat korzystania z programu Entity Framework, interfejs użytkownika witryny sieci web nie będzie można zmienić dużo się od co to jest generowany przez wbudowane szablony.

## <a name="prerequisites"></a>Wymagania wstępne

Zobacz **wersje oprogramowania** w górnej części strony. Entity Framework 6 nie jest to warunek wstępny, ponieważ zainstaluj pakiet NuGet platformy EF w ramach tego samouczka.

## <a name="create-an-mvc-web-app"></a>Tworzenie aplikacji sieci web MVC

1. Otwórz program Visual Studio i Utwórz nowe C# w sieci web projektu za pomocą **aplikacji sieci Web platformy ASP.NET (.NET Framework)** szablonu. Nazwa projektu "ContosoUniversity".

   ![Okno dialogowe Nowy projekt w programie Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

2. W oknie dialogowym Nowy projekt ASP.NET, wybierz **MVC** szablonu.

   ![Nowe okno dialogowe aplikacji sieci web w programie Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

3. Jeśli **uwierzytelniania** nie jest ustawiony na **bez uwierzytelniania**, zmień ją, klikając **Zmień uwierzytelnianie**.

   W **Zmień uwierzytelnianie** okno dialogowe, wybierz opcję **bez uwierzytelniania**, a następnie wybierz **OK**. W tym samouczku aplikacji sieci web nie wymaga użytkownicy mogą logować się ani ogranicza dostępu opartego na który jest zalogowany.

   ![Okno dialogowe uwierzytelniania zmiany w programie Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/change-authentication.png)

4. W oknie dialogowym Nowy projekt ASP.NET, kliknij **OK** do tworzenia projektu.

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka prostych zmian zostanie skonfigurowany w menu witryny, układu i strony głównej.

1. Otwórz *Views\Shared\\_Layout.cshtml*i wprowadź następujące zmiany:

   - Zmienić każde wystąpienie "Moja aplikacja platformy ASP.NET" i "Nazwa aplikacji" do "University firmy Contoso".
   - Dodawanie elementów menu dla studentów, kursy, instruktorów i działów, a następnie usuń wpis skontaktuj się z pomocą.

   Zmiany zostaną wyróżnione w poniższym fragmencie kodu:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. W *Views\Home\Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zamienić tekst o platformie ASP.NET i MVC z tekstem o tej aplikacji:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Naciśnij klawisz **Ctrl**+**F5** do uruchomienia witryny sieci web. Zostanie wyświetlona strona główna z menu głównego.

   ![Strona główna University firmy Contoso](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Instalowanie programu Entity Framework 6

1. Z **narzędzia** menu, wybierz **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.

2. W **Konsola Menedżera pakietów** okna, wprowadź następujące polecenie:

   ```text
   Install-Package EntityFramework
   ```

   ![EF zainstalowany](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

   Na ilustracji przedstawiono 6.0.0 instalowane, ale NuGet zainstaluje najnowszą wersję programu Entity Framework (z wyjątkiem wersji wstępnych), począwszy od najnowszej aktualizacji do samouczka, czyli 6.2.0.

Ten krok jest jednym z kilku krokach mającego tego samouczka, możesz wykonać ręcznie, ale która może zostały wykonane automatycznie za pomocą funkcji tworzenia szkieletu ASP.NET MVC. Wykonujesz je ręcznie, aby zobaczyć kroki wymagane do użycia Entity Framework (EF). Tworzenie szkieletu będą używane później do tworzenia widoków i kontrolerów MVC. Alternatywą jest umożliwiające tworzenie szkieletów automatycznie zainstalować pakiet NuGet platformy EF, tworzenia klasy kontekstu bazy danych i Tworzenie parametrów połączenia. Gdy wszystko będzie gotowe to zrobić w ten sposób, to wszystko, co należy zrobić, pominąć te kroki i tworzenia szkieletu kontrolera MVC po utworzeniu usługi klas jednostek.

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Następnie utworzysz klas jednostek dla aplikacji Contoso University. Będzie rozpoczynać trzech następujących elementach:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Istnieje relacja jeden do wielu między `Student` i `Enrollment` jednostek i relacji jeden do wielu między `Course` i `Enrollment` jednostek. Innymi słowy uczniem/uczennicą mogą być rejestrowane w dowolnej liczbie kursów i kursu mogą mieć dowolną liczbę uczniów zarejestrowane w nim.

W poniższych sekcjach utworzysz klasy dla każdego z tych jednostek.

> [!NOTE]
> Jeśli zostanie podjęta próba skompilowania projektu, przed zakończeniem, tworzenie wszystkich tych klas jednostek, uzyskasz błędy kompilatora.

### <a name="the-student-entity"></a>Jednostki dla uczniów

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

- W *modeli* folderze utwórz plik klasy o nazwie *Student.cs* przez kliknięcie prawym przyciskiem myszy folder, w **Eksploratora rozwiązań** i wybierając pozycję **Dodaj**  >  **Klasy**. Zastąp kod szablonu poniższym kodem:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` Właściwości staną się kolumna klucza podstawowego w tabeli bazy danych, która odnosi się do tej klasy. Domyślnie platforma Entity Framework interpretuje właściwość o nazwie `ID` lub *classname* `ID` jako klucz podstawowy.

`Enrollments` Właściwość *właściwość nawigacji*. Właściwości nawigacji przechowywania innych jednostek, które są powiązane z tej jednostki. W tym przypadku `Enrollments` właściwość `Student` jednostki będą przechowywane wszystkie `Enrollment` jednostek, które są powiązane z tym, które `Student` jednostki. Innymi słowy Jeśli danego `Student` wierszy w bazie danych ma dwa powiązane `Enrollment` wierszy (wiersze, które zawierają klucz podstawowy tego uczniów wartość w ich `StudentID` kolumna klucza obcego), które `Student` jednostki `Enrollments` właściwość nawigacji będzie zawierać tych dwóch `Enrollment` jednostek.

Właściwości nawigacji są zazwyczaj definiowane jako `virtual` tak, aby można było korzystać z niektórych funkcji programu Entity Framework takich jak *powolne ładowanie*. (Ładowanie z opóźnieniem zostaną wyjaśnione w dalszej części [odczytywanie powiązanych danych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczków w dalszej części tej serii.)

Jeśli właściwość nawigacji może zawierać wiele jednostek (tak jak w relacji wiele do wielu lub jeden do wielu), jego typ musi być listy, w którym wpisy mogą być dodawane, usuwane lub zaktualizowane, takich jak `ICollection`.

### <a name="the-enrollment-entity"></a>Jednostki rejestracji

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

- W *modeli* folderze utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID` Właściwość będzie miała klucz podstawowy; używa tej jednostki *classname* `ID` wzorca zamiast `ID` sam jak opisany w `Student` jednostki. Zazwyczaj będzie wybrać jeden wzorzec i używać go w całym modelu danych. W tym miejscu odmiany ilustruje, czy można używać obu wzorca. Później w samouczku, zobaczysz jak przy użyciu `ID` bez `classname` ułatwia implementują dziedziczenie w modelu danych.

`Grade` Właściwość [wyliczenia](/ef/ef6/modeling/code-first/data-types/enums). Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość jest [nullable](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Ma wartość null, która różni się od zera klasy korporacyjnej — wartość null oznacza, że wartość nie jest znana lub nie została jeszcze przypisana.

`StudentID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Student`. `Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, dzięki czemu właściwość może zawierać tylko jeden `Student` jednostki (w przeciwieństwie do `Student.Enrollments` właściwość nawigacji był wyświetlany poprzednio, która zawiera wiele `Enrollment` jednostek).

`CourseID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Course`. `Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.

Platforma Entity Framework interpretuje właściwość jako właściwość klucza obcego, jeśli jest on nazwany *&lt;nazwy właściwości nawigacji&gt;&lt;nazwa właściwość klucza podstawowego&gt;* (na przykład `StudentID`dla `Student` właściwość nawigacji od `Student` jest klucz podstawowy jednostki `ID`). Właściwości klucza obcego może również mieć taką samą nazwę po prostu *&lt;nazwa właściwość klucza podstawowego&gt;* (na przykład `CourseID` ponieważ `Course` jest klucz podstawowy jednostki `CourseID`).

### <a name="the-course-entity"></a>Jednostki kursu

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

- W *modeli* folderze utwórz *Course.cs*, zastępując kod szablonu poniższym kodem:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments` Właściwość jest właściwością nawigacji. A `Course` jednostki mogą być one związane z dowolną liczbę `Enrollment` jednostek.

Przycisk więcej na temat <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute> atrybut później w samouczku z tej serii. Zasadniczo ten atrybut umożliwia wprowadzenie klucza podstawowego dla kurs zamiast bazy danych, do jego wygenerowania.

## <a name="create-the-database-context"></a>Utwórz kontekst bazy danych

Główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework jest *kontekst bazy danych* klasy. Tworzenie tej klasy, wynikające z [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) klasy. W kodzie należy określić, które jednostki są uwzględnione w modelu danych. Można również dostosować określone zachowanie programu Entity Framework. W tym projekcie nosi nazwę klasy `SchoolContext`.

- Aby utworzyć folder w projekcie ContosoUniversity, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i kliknij przycisk **Dodaj**, a następnie kliknij przycisk **nowy Folder**. Nadaj nazwę nowego folderu *DAL* (w przypadku warstwy dostępu do danych). W tym folderze utwórz nowy plik klasy o nazwie *SchoolContext.cs*i Zastąp kod szablonu poniższym kodem:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Określ zestawy jednostek

Ten kod tworzy [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) właściwości dla każdego zestawu jednostek. W terminologii programu Entity Framework *zestaw jednostek* zazwyczaj odnosi się do tabeli bazy danych i *jednostki* odnosi się do wiersza w tabeli.

> [!NOTE]
>
> Możesz pominąć `DbSet<Enrollment>` i `DbSet<Course>` instrukcji i będzie działać tak samo. Entity Framework będzie je uwzględnić niejawnie, ponieważ `Student` odwołań do jednostek `Enrollment` jednostki i `Enrollment` odwołań do jednostek `Course` jednostki.

### <a name="specify-the-connection-string"></a>Określ parametry połączenia

Nazwa ciągu połączenia, (które można będzie później dodać do pliku Web.config) jest przekazywana do konstruktora.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Możesz też może przekazać w ciągu połączenia zamiast nazwy, który jest przechowywany w pliku Web.config. Aby uzyskać więcej informacji o opcjach Określanie bazy danych do użycia, zobacz [parametry połączenia i modeli](/ef/ef6/fundamentals/configuring/connection-strings).

Jeśli nie zostanie określony ciąg połączenia lub nazwa jednej jawnie, platformy Entity Framework zakłada, że nazwa parametrów połączenia jest taka sama jak nazwa klasy. Domyślna nazwa parametrów połączenia w tym przykładzie będzie wówczas `SchoolContext`, taka sama jak co wpisujesz jawnie.

### <a name="specify-singular-table-names"></a>Określanie nazw pojedynczej tabeli

`modelBuilder.Conventions.Remove` Instrukcji w [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metoda zapobiega on pluralized nazwy tabel. Jeśli nie możesz tego zrobić, wygenerowany tabele w bazie danych będą miały postać `Students`, `Courses`, i `Enrollments`. Zamiast tego nazwy tabel będą `Student`, `Course`, i `Enrollment`. Deweloperzy nie zgadzają się na temat tego, czy nazwy tabel należy pluralized czy nie. Ten samouczek używa pojedynczej formularza, ale istotną kwestią jest to, którą można wybrać niezależnie od tego formularza preferowanych przez uwzględnienie lub pominięcie ten wiersz kodu.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Konfigurowanie programu EF zainicjować w bazie danych testowych

Entity Framework można automatycznie utworzyć (lub porzucić i ponownie utworzyć) bazę danych dla Ciebie, gdy aplikacja zostanie uruchomiona. Można określić, że należy to zrobić za każdym razem, gdy Twoja aplikacja jest uruchamiana, lub tylko wtedy, gdy model jest zsynchronizowany z istniejącej bazy danych. Można także napisać `Seed` metoda tej platformy Entity Framework wywołuje automatycznie po utworzeniu bazy danych, aby wypełnić je danymi testu.

Zachowaniem domyślnym jest utworzenie bazy danych, tylko wtedy, gdy go nie istnieje (i zgłosić wyjątek, jeśli model został zmieniony i baza danych już istnieje). W tej sekcji określisz, że bazy danych powinny być porzucić i ponownie utworzyć zawsze po zmianie modelu. Porzucanie bazy danych powoduje utratę wszystkich danych. Zwykle jest to OK podczas tworzenia aplikacji, ponieważ `Seed` metoda będzie uruchamiany, gdy baza danych jest ponownie tworzony i utworzyć dane z badań. Jednak w środowisku produkcyjnym zwykle nie chcesz utracić wszystkie dane, za każdym razem, gdy zajdzie potrzeba zmiany schematu bazy danych. Później zobaczysz jak obsługiwać zmiany modelu, używając migracje Code First do zmiany schematu bazy danych zamiast porzucenie i ponowne utworzenie bazy danych.

1. W folderze DAL Utwórz nowy plik klasy o nazwie *SchoolInitializer.cs* i Zastąp kod szablonu poniższym kodem, co powoduje, że bazy danych ma zostać utworzony, w razie i ładowania danych testowych do nowej bazy danych.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   `Seed` Metoda przyjmuje obiekt kontekstu bazy danych jako parametr wejściowy, a kod w metodzie używa tego obiektu, aby dodać nowe jednostki w bazie danych. Dla każdego typu jednostki, ten kod tworzy kolekcję nowe jednostki, dodaje je do odpowiednich `DbSet` właściwości, a następnie zapisuje zmiany w bazie danych. Nie trzeba wywoływać `SaveChanges` metody po każdej grupie jednostek, tak jak to zrobić w tym miejscu, ale sposób, który pomoże Ci Znajdź źródło problemu, jeśli wystąpi wyjątek podczas pisania kodu w bazie danych.

2. Powiedzieć Entity Framework do użycia klasy inicjatora, Dodaj element do `entityFramework` elementu w aplikacji *Web.config* pliku (znajdującego się w folderze głównym projektu), jak pokazano w poniższym przykładzie:

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   `context type` Określa kontekst w pełni kwalifikowaną nazwę klasy i zestawu, jest on, i `databaseinitializer type` określa pełni kwalifikowaną nazwę klasy inicjatora i zestawu, jest on. (Jeśli nie chcesz, aby EF używać inicjatora, można ustawić atrybutu na `context` element: `disableDatabaseInitialization="true"`.) Aby uzyskać więcej informacji, zobacz [ustawień pliku konfiguracji](/ef/ef6/fundamentals/configuring/config-file).

   Alternatywa ustawienie inicjatora w *Web.config* plik jest konieczne wykonanie kodu, dodając `Database.SetInitializer` instrukcję, aby `Application_Start` method in Class metoda *Global.asax.cs* pliku. Aby uzyskać więcej informacji, zobacz [opis inicjatory bazy danych w Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Aplikacja jest teraz skonfigurowane tak, że gdy uzyskujesz dostęp do bazy danych po raz pierwszy w danym przebiegu aplikacji platformy Entity Framework porównuje bazy danych do modelu (Twoje `SchoolContext` i klas jednostek). Jeśli tak, aplikacja spadnie i ponownie tworzy bazę danych.

> [!NOTE]
> Podczas wdrażania aplikacji na serwerze sieci web w środowisku produkcyjnym, musisz usunąć lub wyłączyć kod, który umieszcza i ponownie tworzy bazę danych. Należy to zrobić później w samouczku z tej serii.

## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Konfigurowanie programu EF do korzystania z bazy danych programu SQL Server Express LocalDB

[LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) to Uproszczona wersja aparatu bazy danych programu SQL Server Express. Jest łatwa do zainstalowania i skonfigurowania, rozpoczyna się na żądanie i działa w trybie użytkownika. LocalDB działa w specjalnego trybu wykonania programu SQL Server Express, która umożliwia pracę z bazami danych jako *.mdf* plików. Możesz umieścić pliki bazy danych LocalDB w *aplikacji\_danych* folderu projektu sieci web, jeśli chcesz można było skopiować bazę danych z projektem. Funkcja wystąpienia użytkownika programu SQL Server Express umożliwia także pracować z *.mdf* pliki, ale funkcja wystąpienia użytkownika jest przestarzała; dlatego LocalDB jest zalecane w przypadku pracy z *.mdf* plików. LocalDB jest instalowany domyślnie z programem Visual Studio.

Zazwyczaj programu SQL Server Express nie jest używana dla aplikacji sieci web w środowisku produkcyjnym. LocalDB w szczególności nie jest zalecane do użytku produkcyjnego z aplikacją sieci web, ponieważ nie zostało zaprojektowane do pracy z usługami IIS.

- W tym samouczku będziesz pracować z bazą danych LocalDB. Otwórz aplikację *Web.config* pliku i Dodaj `connectionStrings` poprzedni element `appSettings` elementu, jak pokazano w poniższym przykładzie. (Pamiętaj o zaktualizowaniu *Web.config* pliku w folderze głównym projektu. Istnieje również *Web.config* w pliku *widoków* podfolder, który nie jest potrzebny do zaktualizowania.)

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Parametry połączenia zostały dodane Określa, że platformy Entity Framework będzie używać bazy danych LocalDB o nazwie *ContosoUniversity1.mdf*. (Jeszcze nie istnieje baza danych, ale zostanie ona utworzona EF). Jeśli chcesz utworzyć bazę danych w Twojej *aplikacji\_danych* folderu, można dodać `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` parametry połączenia. Aby uzyskać więcej informacji dotyczących parametrów połączenia, zobacz [parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](/previous-versions/aspnet/jj653752(v=vs.110)).

Nie jest potrzebna parametrów połączenia w *Web.config* pliku. Jeśli nie zostanie podane parametry połączenia, platformy Entity Framework używa domyślne parametry połączenia oparte na klasie kontekstu. Aby uzyskać więcej informacji, zobacz [Code First dla nowej bazy danych](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-a-student-controller-and-views"></a>Tworzenie kontrolera dla uczniów i widoków

Teraz utworzysz stronę sieci web do wyświetlania danych. Proces żądania danych automatycznie wyzwala utworzenie bazy danych. Rozpocznie się przez utworzenie nowego kontrolera. Jednak zanim to zrobisz, skompiluj projekt, aby udostępnić klasy modelu i kontekstu do tworzenia szkieletów kontrolerów MVC.

1. Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań**, wybierz opcję **Dodaj**, a następnie kliknij przycisk **nowy element szkieletu**.
2. W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC 5 z widokami używający narzędzia Entity Framework**, a następnie wybierz **Dodaj**.

     ![Dodaj okno dialogowe Tworzenie szkieletu w programie Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. W **Dodaj kontroler** okno dialogowe, wybierz następujące opcje, a następnie wybierz **Dodaj**:

   - Klasa modelu: **uczniów (ContosoUniversity.Models)**. (Jeśli nie widzisz tej opcji na liście rozwijanej, skompiluj projekt i spróbuj ponownie.)
   - Klasa kontekstu danych: **SchoolContext (ContosoUniversity.DAL)**.
   - Nazwa kontrolera: **StudentController** (nie StudentsController).
   - Pozostaw wartości domyślne dla pozostałych pól.

     ![Dodaj okno dialogowe kontrolera w programie Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-controller.png)

     Po kliknięciu **Dodaj**, tworzy Generator szkieletu *StudentController.cs* plików i zestaw widoków (*.cshtml* plików), pracować z kontrolerem. W przyszłości podczas tworzenia projektów, które korzystają z programu Entity Framework, można również korzystać z zalet niektóre dodatkowe funkcje Generator szkieletu: tworzenie swojej pierwszej klasy modelu, nie należy tworzyć parametry połączenia, a następnie w polu **Dodaj kontroler** Określ pole **nowy kontekst danych** , wybierając **+** znajdujący się obok **klasa kontekstu danych**. Generator szkieletu spowoduje utworzenie Twojego `DbContext` klasy oraz połączenie z ciągu oraz kontrolera i widoki.
4. Zostanie otwarty program Visual Studio *Controllers\StudentController.cs* pliku. Zobacz, czy zmienna klasa została utworzona, tworzy wystąpienie obiektu kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     `Index` Metody akcji pobiera listę uczniów z *studentów* zestaw, czytając jednostek `Students` właściwości wystąpienia kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     *Student\Index.cshtml* widoku tej liście są wyświetlane w tabeli:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Naciśnij klawisz **Ctrl**+**F5** Aby uruchomić projekt. (Jeśli zostanie wyświetlony błąd "Nie można utworzyć kopii w tle", zamknij przeglądarkę i spróbuj ponownie.)

     Kliknij przycisk **studentów** kartę, aby wyświetlić dane z badań, `Seed` metoda wstawiony. W zależności od sposobu wąskie okno przeglądarki jest, zobaczysz link kartę uczniów na pasku adresu w górnym lub musisz kliknij prawy górny róg, aby zobaczyć łącza.

     ![Przycisk menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Strona indeksu dla uczniów](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Widok bazy danych

Po uruchomieniu strony studentów i aplikacja próbowała dostęp do bazy danych, EF odnalezione, które nie zostało Brak bazy danych oraz utworzone. EF następnie uruchomiono seed — metoda, aby wypełnić bazę danych.

Można użyć dowolnego **Eksploratora serwera** lub **Eksplorator obiektów SQL Server** (SSOX), aby wyświetlić bazy danych w programie Visual Studio. W tym samouczku użyjesz **Eksploratora serwera**.

1. Zamknij przeglądarkę.
2. W **Eksploratora serwera**, rozwiń węzeł **połączeń danych** (konieczne może być najpierw wybrać przycisk Odśwież), rozwiń węzeł **kontekstu służbowego (ContosoUniversity)**, a następnie rozwiń węzeł  **Tabele** aby zobaczyć tabele w nowej bazy danych.

    ![Tabele bazy danych w Eksploratorze serwera](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)

3. Kliknij prawym przyciskiem myszy **uczniów** tabeli, a następnie kliknij przycisk **Pokaż dane tabeli** kolumn, które zostały utworzone i wierszy, które zostały wstawione do tabeli.

    ![Tabela student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/table-data.png)
4. Zamknij **Eksploratora serwera** połączenia.

*ContosoUniversity1.mdf* i *ldf* pliki bazy danych znajdują się w *% USERPROFILE %* folderu.

Ponieważ używasz `DropCreateDatabaseIfModelChanges` inicjatora, można teraz wprowadzone zmiany `Student` klasy, uruchomić ponownie aplikację i bazy danych będzie automatycznie ponownie tworzone, aby dopasować zmiany. Na przykład jeśli dodasz `EmailAddress` właściwości `Student` klasy, uruchom ponownie na stronie studentów i Wyświetlę tabeli ponownie, zostanie wyświetlony nowy `EmailAddress` kolumny.

## <a name="conventions"></a>Konwencje

Ilość kodu, trzeba było pisać w kolejności Entity Framework można było tworzenie pełną bazy danych jest minimalny z powodu *konwencje*, lub założenia, które czynią Entity Framework. Niektóre z nich zostały już zanotowane lub zostały użyte bez Twojej wiedziały z nich:

- Pluralized formy nazwy klas jednostek są używane jako nazwy tabeli.
- Nazwy właściwości jednostki są używane dla nazw kolumn.
- Właściwości jednostki, które są nazwane `ID` lub *classname* `ID` są rozpoznawane jako właściwości klucza podstawowego.
- Właściwość jest interpretowany jako właściwość klucza obcego, jeśli jest on nazwany *&lt;nazwy właściwości nawigacji&gt;&lt;nazwa właściwość klucza podstawowego&gt;* (na przykład `StudentID` dla `Student` właściwość nawigacji od `Student` jest klucz podstawowy jednostki `ID`). Właściwości klucza obcego może również mieć taką samą nazwę po prostu &lt;nazwa właściwość klucza podstawowego&gt; (na przykład `EnrollmentID` ponieważ `Enrollment` jest klucz podstawowy jednostki `EnrollmentID`).

Po zapoznaniu się konwencje może zostać zastąpiona. Na przykład określić, nie powinien być pluralized nazwy tabel, a zobaczysz później jak wyraźnie oznaczyć właściwość jako właściwość klucza obcego. Dowiesz się więcej na temat Konwencji i jak je przesłonić [tworzenie więcej złożonego modelu danych](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) samouczków w dalszej części tej serii. Aby uzyskać więcej informacji na temat Konwencji, zobacz [pierwszy konwencje związane z](/ef/ef6/modeling/code-first/conventions/built-in).

## <a name="summary"></a>Podsumowanie

Utworzono prostą aplikację, która korzysta z programu Entity Framework i programu SQL Server Express LocalDB do przechowywania i wyświetlania danych. W ciągu następnych dowiesz się, jak wykonywać podstawowe tworzenie, Odczyt, aktualizowanie i usuwanie operacji (CRUD).

Jak się podoba w tym samouczku, i co można było ulepszyć proces Wystaw opinię.

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
