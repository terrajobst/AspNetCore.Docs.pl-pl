---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 'Samouczek: Rozpoczynanie pracy z usługą Entity Framework 6 Code First wykorzystaniem MVC 5 | Dokumentacja firmy Microsoft'
description: W tej serii samouczków dowiesz się, jak utworzyć aplikację ASP.NET MVC 5, która używa platformy Entity Framework 6 na potrzeby dostępu do danych.
author: tdykstra
ms.author: riande
ms.date: 01/10/2019
ms.topic: tutorial
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5d524c981af2d9d8f44254c61631937e6f049cdb
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341709"
---
# <a name="tutorial-get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Samouczek: Rozpoczynanie pracy z usługą Entity Framework 6 Code First wykorzystaniem MVC 5

> [!NOTE]
> W nowych wdrożeniach, firma Microsoft zaleca [stronami ASP.NET Core Razor](/aspnet/core/razor-pages) za pośrednictwem widoków i kontrolerów platformy ASP.NET MVC. Dla serii samouczków, podobny do następującego przy użyciu stron Razor, zobacz [samouczka: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start). Samouczek nowe:
> * Łatwiej jest je wykonać.
> * Zapewnia więcej najlepszych rozwiązań programu EF Core.
> * Używa wydajniejszych zapytań.
> * Jest bardziej aktualny przy użyciu najnowszych interfejsu API.
> * Obejmuje więcej funkcji.
> * Jest preferowanym podejściem w przypadku nowych wdrożeń aplikacji.

W tej serii samouczków dowiesz się, jak utworzyć aplikację ASP.NET MVC 5, która używa platformy Entity Framework 6 na potrzeby dostępu do danych. Ten samouczek używa kodu pierwszego przepływu pracy. Aby dowiedzieć się, jak dokonać wyboru między Code First Database First i pierwszego modelu, zobacz [utworzyć model](/ef/ef6/modeling/).

Tej serii samouczków opisano sposób tworzenia przykładowej aplikacji Contoso University. Przykładowa aplikacja jest university prostą witrynę sieci Web. Dzięki niemu można można przeglądać i aktualizować informacje przez instruktorów, kurs i uczniów. Poniżej przedstawiono dwa ekrany, które możesz utworzyć:

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Edytowanie ucznia](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Tworzenie aplikacji sieci web MVC
> * Ustawianie stylów lokacji
> * Instalowanie programu Entity Framework 6
> * Tworzenie modelu danych
> * Utwórz kontekst bazy danych
> * Zainicjuj kontekst bazy danych przy użyciu danych testowych
> * Konfigurowanie programów EF 6, aby użyć programu LocalDB
> * Tworzenie widoków i kontrolerów
> * Widok bazy danych

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="create-an-mvc-web-app"></a>Tworzenie aplikacji sieci web MVC

1. Otwórz program Visual Studio i Utwórz C# projekt sieci web za pomocą **aplikacji sieci Web platformy ASP.NET (.NET Framework)** szablonu. Nadaj projektowi nazwę *ContosoUniversity* i wybierz **OK**.

   ![Okno dialogowe Nowy projekt w programie Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

1. W **nowej aplikacji sieci Web ASP.NET - ContosoUniversity**, wybierz opcję **MVC**.

   ![Nowe okno dialogowe aplikacji sieci web w programie Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

    > [!NOTE]
    > Domyślnie **uwierzytelniania** ustawiono opcję **bez uwierzytelniania**. W tym samouczku aplikacji sieci web nie wymaga użytkownikom zalogowanie się. Ponadto go nie ogranicza dostępu opartego na który jest zalogowany.

1. Wybierz **OK** do tworzenia projektu.

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka prostych zmian zostanie skonfigurowany w menu witryny, układu i strony głównej.

1. Otwórz *Views\Shared\\_Layout.cshtml*i wprowadź następujące zmiany:

   - Zmienić każde wystąpienie "Moja aplikacja platformy ASP.NET" i "Nazwa aplikacji" do "University firmy Contoso".
   - Dodawanie elementów menu dla studentów, kursy, instruktorów i działów, a następnie usuń wpis skontaktuj się z pomocą.

   Zmiany zostaną wyróżnione w poniższym fragmencie kodu:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. W *Views\Home\Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zamienić tekst o platformie ASP.NET i MVC z tekstem o tej aplikacji:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Naciśnij klawisze Ctrl + F5, aby uruchomić witrynę sieci web. Zostanie wyświetlona strona główna z menu głównego.

## <a name="install-entity-framework-6"></a>Instalowanie programu Entity Framework 6

1. Z **narzędzia** menu, wybierz **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.

2. W **Konsola Menedżera pakietów** okna, wprowadź następujące polecenie:

   ```text
   Install-Package EntityFramework
   ```

Ten krok jest jednym z kilku krokach mającego tego samouczka, możesz wykonać ręcznie, ale która może zostały wykonane automatycznie za pomocą funkcji tworzenia szkieletu ASP.NET MVC. Wykonujesz je ręcznie, aby zobaczyć kroki wymagane do użycia Entity Framework (EF). Tworzenie szkieletu będą używane później do tworzenia widoków i kontrolerów MVC. Alternatywą jest umożliwiające tworzenie szkieletów automatycznie zainstalować pakiet NuGet platformy EF, tworzenia klasy kontekstu bazy danych i Tworzenie parametrów połączenia. Gdy wszystko będzie gotowe to zrobić w ten sposób, to wszystko, co należy zrobić, pominąć te kroki i tworzenia szkieletu kontrolera MVC po utworzeniu usługi klas jednostek.

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Następnie utworzysz klas jednostek dla aplikacji Contoso University. Będzie rozpoczynać trzech następujących elementach:

**Kurs** <-> **rejestracji** <-> **ucznia**

| Jednostki | Relacja |
| -------- | ------------ |
| Kurs do rejestracji | Jeden do wielu |
| Uczniów do rejestracji | Jeden do wielu |

Istnieje relacja jeden do wielu między `Student` i `Enrollment` jednostek i relacji jeden do wielu między `Course` i `Enrollment` jednostek. Innymi słowy uczniem/uczennicą mogą być rejestrowane w dowolnej liczbie kursów i kursu mogą mieć dowolną liczbę uczniów zarejestrowane w nim.

W poniższych sekcjach utworzysz klasy dla każdego z tych jednostek.

> [!NOTE]
> Jeśli zostanie podjęta próba skompilowania projektu, przed zakończeniem, tworzenie wszystkich tych klas jednostek, uzyskasz błędy kompilatora.

### <a name="the-student-entity"></a>Jednostki dla uczniów

- W *modeli* folderze utwórz plik klasy o nazwie *Student.cs* przez kliknięcie prawym przyciskiem myszy folder, w **Eksploratora rozwiązań** i wybierając pozycję **Dodaj**  >  **Klasy**. Zastąp kod szablonu poniższym kodem:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` Właściwości staną się kolumna klucza podstawowego w tabeli bazy danych, która odnosi się do tej klasy. Domyślnie platforma Entity Framework interpretuje właściwość o nazwie `ID` lub *classname* `ID` jako klucz podstawowy.

`Enrollments` Właściwość *właściwość nawigacji*. Właściwości nawigacji przechowywania innych jednostek, które są powiązane z tej jednostki. W tym przypadku `Enrollments` właściwość `Student` jednostki będą przechowywane wszystkie `Enrollment` jednostek, które są powiązane z tym, które `Student` jednostki. Innymi słowy Jeśli danego `Student` wierszy w bazie danych ma dwa powiązane `Enrollment` wierszy (wiersze, które zawierają klucz podstawowy tego uczniów wartość w ich `StudentID` kolumna klucza obcego), które `Student` jednostki `Enrollments` właściwość nawigacji będzie zawierać tych dwóch `Enrollment` jednostek.

Właściwości nawigacji są zazwyczaj definiowane jako `virtual` tak, aby można było korzystać z niektórych funkcji programu Entity Framework takich jak *powolne ładowanie*. (Ładowanie z opóźnieniem zostaną wyjaśnione w dalszej części [odczytywanie powiązanych danych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczków w dalszej części tej serii.)

Jeśli właściwość nawigacji może zawierać wiele jednostek (tak jak w relacji wiele do wielu lub jeden do wielu), jego typ musi być listy, w którym wpisy mogą być dodawane, usuwane lub zaktualizowane, takich jak `ICollection`.

### <a name="the-enrollment-entity"></a>Jednostki rejestracji

- W *modeli* folderze utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID` Właściwość będzie miała klucz podstawowy; używa tej jednostki *classname* `ID` wzorca zamiast `ID` sam jak opisany w `Student` jednostki. Zazwyczaj będzie wybrać jeden wzorzec i używać go w całym modelu danych. W tym miejscu odmiany ilustruje, czy można używać obu wzorca. Później w samouczku, zobaczysz jak przy użyciu `ID` bez `classname` ułatwia implementują dziedziczenie w modelu danych.

`Grade` Właściwość [wyliczenia](/ef/ef6/modeling/code-first/data-types/enums). Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość jest [nullable](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Ma wartość null, która różni się od zera klasy korporacyjnej — wartość null oznacza, że wartość nie jest znana lub nie została jeszcze przypisana.

`StudentID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Student`. `Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, dzięki czemu właściwość może zawierać tylko jeden `Student` jednostki (w przeciwieństwie do `Student.Enrollments` właściwość nawigacji był wyświetlany poprzednio, która zawiera wiele `Enrollment` jednostek).

`CourseID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Course`. `Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.

Platforma Entity Framework interpretuje właściwość jako właściwość klucza obcego, jeśli jest on nazwany *&lt;nazwy właściwości nawigacji&gt;&lt;nazwa właściwość klucza podstawowego&gt;* (na przykład `StudentID`dla `Student` właściwość nawigacji od `Student` jest klucz podstawowy jednostki `ID`). Właściwości klucza obcego może również mieć taką samą nazwę po prostu *&lt;nazwa właściwość klucza podstawowego&gt;* (na przykład `CourseID` ponieważ `Course` jest klucz podstawowy jednostki `CourseID`).

### <a name="the-course-entity"></a>Jednostki kursu

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

## <a name="initialize-db-with-test-data"></a>Zainicjuj kontekst bazy danych przy użyciu danych testowych

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

## <a name="set-up-ef-6-to-use-localdb"></a>Konfigurowanie programów EF 6, aby użyć programu LocalDB

[LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) to Uproszczona wersja aparatu bazy danych programu SQL Server Express. Jest łatwa do zainstalowania i skonfigurowania, rozpoczyna się na żądanie i działa w trybie użytkownika. LocalDB działa w specjalnego trybu wykonania programu SQL Server Express, która umożliwia pracę z bazami danych jako *.mdf* plików. Możesz umieścić pliki bazy danych LocalDB w *aplikacji\_danych* folderu projektu sieci web, jeśli chcesz można było skopiować bazę danych z projektem. Funkcja wystąpienia użytkownika programu SQL Server Express umożliwia także pracować z *.mdf* pliki, ale funkcja wystąpienia użytkownika jest przestarzała; dlatego LocalDB jest zalecane w przypadku pracy z *.mdf* plików. LocalDB jest instalowany domyślnie z programem Visual Studio.

Zazwyczaj programu SQL Server Express nie jest używana dla aplikacji sieci web w środowisku produkcyjnym. LocalDB w szczególności nie jest zalecane do użytku produkcyjnego z aplikacją sieci web, ponieważ nie zostało zaprojektowane do pracy z usługami IIS.

- W tym samouczku będziesz pracować z bazą danych LocalDB. Otwórz aplikację *Web.config* pliku i Dodaj `connectionStrings` poprzedni element `appSettings` elementu, jak pokazano w poniższym przykładzie. (Pamiętaj o zaktualizowaniu *Web.config* pliku w folderze głównym projektu. Istnieje również *Web.config* w pliku *widoków* podfolder, który nie jest potrzebny do zaktualizowania.)

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Parametry połączenia zostały dodane Określa, że platformy Entity Framework będzie używać bazy danych LocalDB o nazwie *ContosoUniversity1.mdf*. (Jeszcze nie istnieje baza danych, ale zostanie ona utworzona EF). Jeśli chcesz utworzyć bazę danych w Twojej *aplikacji\_danych* folderu, można dodać `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` parametry połączenia. Aby uzyskać więcej informacji dotyczących parametrów połączenia, zobacz [parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](/previous-versions/aspnet/jj653752(v=vs.110)).

Nie jest potrzebna parametrów połączenia w *Web.config* pliku. Jeśli nie zostanie podane parametry połączenia, platformy Entity Framework używa domyślne parametry połączenia oparte na klasie kontekstu. Aby uzyskać więcej informacji, zobacz [Code First dla nowej bazy danych](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-controller-and-views"></a>Tworzenie widoków i kontrolerów

Teraz utworzysz stronę sieci web do wyświetlania danych. Proces żądania danych automatycznie wyzwala utworzenie bazy danych. Rozpocznie się przez utworzenie nowego kontrolera. Jednak zanim to zrobisz, skompiluj projekt, aby udostępnić klasy modelu i kontekstu do tworzenia szkieletów kontrolerów MVC.

1. Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań**, wybierz opcję **Dodaj**, a następnie kliknij przycisk **nowy element szkieletu**.
2. W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC 5 z widokami używający narzędzia Entity Framework**, a następnie wybierz **Dodaj**.

     ![Dodaj okno dialogowe Tworzenie szkieletu w programie Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. W **Dodaj kontroler** okno dialogowe, wybierz następujące opcje, a następnie wybierz **Dodaj**:

   - Klasa modelu: **Dla uczniów (ContosoUniversity.Models)**. (Jeśli nie widzisz tej opcji na liście rozwijanej, skompiluj projekt i spróbuj ponownie.)
   - Klasa kontekstu danych: **SchoolContext (ContosoUniversity.DAL)**.
   - Nazwa kontrolera: **StudentController** (nie StudentsController).
   - Pozostaw wartości domyślne dla pozostałych pól.

     Po kliknięciu **Dodaj**, tworzy Generator szkieletu *StudentController.cs* plików i zestaw widoków (*.cshtml* plików), pracować z kontrolerem. W przyszłości podczas tworzenia projektów, które korzystają z programu Entity Framework, można również korzystać z zalet niektóre dodatkowe funkcje Generator szkieletu: tworzenie swojej pierwszej klasy modelu, nie należy tworzyć parametry połączenia, a następnie w polu **Dodaj kontroler** Określ pole **nowy kontekst danych** , wybierając **+** znajdujący się obok **klasa kontekstu danych**. Generator szkieletu spowoduje utworzenie Twojego `DbContext` klasy oraz połączenie z ciągu oraz kontrolera i widoki.
4. Zostanie otwarty program Visual Studio *Controllers\StudentController.cs* pliku. Zobacz, czy zmienna klasa została utworzona, tworzy wystąpienie obiektu kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     `Index` Metody akcji pobiera listę uczniów z *studentów* zestaw, czytając jednostek `Students` właściwości wystąpienia kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     *Student\Index.cshtml* widoku tej liście są wyświetlane w tabeli:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Naciśnij klawisze Ctrl + F5, aby uruchomić projekt. (Jeśli zostanie wyświetlony błąd "Nie można utworzyć kopii w tle", zamknij przeglądarkę i spróbuj ponownie.)

     Kliknij przycisk **studentów** kartę, aby wyświetlić dane z badań, `Seed` metoda wstawiony. W zależności od sposobu wąskie okno przeglądarki jest, zobaczysz link kartę uczniów na pasku adresu w górnym lub musisz kliknij prawy górny róg, aby zobaczyć łącza.

     ![Przycisk menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

## <a name="view-the-database"></a>Widok bazy danych

Po uruchomieniu strony studentów i aplikacja próbowała dostęp do bazy danych, EF odnalezione, które nie zostało Brak bazy danych oraz utworzone. EF następnie uruchomiono seed — metoda, aby wypełnić bazę danych.

Można użyć dowolnego **Eksploratora serwera** lub **Eksplorator obiektów SQL Server** (SSOX), aby wyświetlić bazy danych w programie Visual Studio. W tym samouczku użyjesz **Eksploratora serwera**.

1. Zamknij przeglądarkę.
2. W **Eksploratora serwera**, rozwiń węzeł **połączeń danych** (konieczne może być najpierw wybrać przycisk Odśwież), rozwiń węzeł **kontekstu służbowego (ContosoUniversity)**, a następnie rozwiń węzeł  **Tabele** aby zobaczyć tabele w nowej bazy danych.

3. Kliknij prawym przyciskiem myszy **uczniów** tabeli, a następnie kliknij przycisk **Pokaż dane tabeli** kolumn, które zostały utworzone i wierszy, które zostały wstawione do tabeli.

4. Zamknij **Eksploratora serwera** połączenia.

*ContosoUniversity1.mdf* i *ldf* pliki bazy danych znajdują się w *% USERPROFILE %* folderu.

Ponieważ używasz `DropCreateDatabaseIfModelChanges` inicjatora, można teraz wprowadzone zmiany `Student` klasy, uruchomić ponownie aplikację i bazy danych będzie automatycznie ponownie tworzone, aby dopasować zmiany. Na przykład jeśli dodasz `EmailAddress` właściwości `Student` klasy, uruchom ponownie na stronie studentów i Wyświetlę tabeli ponownie, zostanie wyświetlony nowy `EmailAddress` kolumny.

## <a name="conventions"></a>Konwencje

Ilość kodu, trzeba było pisać w kolejności Entity Framework można było tworzenie pełną bazy danych jest minimalny z powodu *konwencje*, lub założenia, które czynią Entity Framework. Niektóre z nich zostały już zanotowane lub zostały użyte bez Twojej wiedziały z nich:

- Pluralized formy nazwy klas jednostek są używane jako nazwy tabeli.
- Nazwy właściwości jednostki są używane dla nazw kolumn.
- Właściwości jednostki, które są nazwane `ID` lub *classname* `ID` są rozpoznawane jako właściwości klucza podstawowego.
- Właściwość jest interpretowany jako właściwość klucza obcego, jeśli jest on nazwany *&lt;nazwy właściwości nawigacji&gt;&lt;nazwa właściwość klucza podstawowego&gt;* (na przykład `StudentID` dla `Student` właściwość nawigacji od `Student` jest klucz podstawowy jednostki `ID`). Właściwości klucza obcego może również mieć taką samą nazwę po prostu &lt;nazwa właściwość klucza podstawowego&gt; (na przykład `EnrollmentID` ponieważ `Enrollment` jest klucz podstawowy jednostki `EnrollmentID`).

Po zapoznaniu się konwencje może zostać zastąpiona. Na przykład określić, nie powinien być pluralized nazwy tabel, a zobaczysz później jak wyraźnie oznaczyć właściwość jako właściwość klucza obcego.
## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać więcej informacji o programów EF 6 zobacz następujące artykuły:

* [Dostęp do danych na platformie ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md)

* [Pierwszy konwencje związane z](/ef/ef6/modeling/code-first/conventions/built-in)

* [Tworzenie bardziej złożonego modelu danych](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Utworzona aplikacja internetowa MVC
> * Ustawianie stylów lokacji
> * Zainstalowane Entity Framework 6
> * Utworzony model danych
> * Utworzone kontekst bazy danych
> * Zainicjowana klasa bazy danych przy użyciu danych testowych
> * Konfigurowanie programów EF 6, aby użyć programu LocalDB
> * Utworzony kontroler i widoków
> * Wyświetlać bazy danych

Przejdź do następnego artykułu, aby dowiedzieć się, jak przeglądanie i dostosowywanie tworzenia, odczytywać, aktualizować, Usuń kod (CRUD) w widokach i kontrolerach.
> [!div class="nextstepaction"]
> [Implementowanie podstawowych funkcji CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)