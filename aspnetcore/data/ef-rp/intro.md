---
title: Strony razor za pomocą platformy Entity Framework Core w programie ASP.NET Core — samouczek 1 8
author: rick-anderson
description: Pokazuje, jak utworzyć aplikację strony Razor za pomocą platformy Entity Framework Core
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/intro
ms.openlocfilehash: 2f6408f2381721c450519818a5973bad0f86ccad
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938410"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Strony razor za pomocą platformy Entity Framework Core w programie ASP.NET Core — samouczek 1 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University pokazuje, jak utworzyć aplikację stron Razor programu ASP.NET Core przy użyciu Core Entity Framework (EF).

Przykładowa aplikacja jest witryną sieci web dla uniwersytetu fikcyjnej firmy Contoso. Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora. Ta strona jest to pierwszy z serii samouczków, które opisują sposób tworzenia przykładowej aplikacji Contoso University.

[Pobieranie i wyświetlanie ukończonej aplikacji.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[! OBEJMUJĄ [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

[! Dołącz [] (~ / includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]

------

Znajomość [stron Razor](xref:razor-pages/index). Należy wykonać początkujących programistów [Rozpoczynanie pracy ze stronami Razor](xref:tutorials/razor-pages/razor-pages-start) przed rozpoczęciem tej serii.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli napotkasz problem, nie można rozpoznać ogólnie można znaleźć rozwiązania, porównując swój kod, aby [projektu ukończona](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Dobrym sposobem, aby uzyskać pomoc polega na publikowanie pytań do [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) dla [platformy ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [programu EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

## <a name="the-contoso-university-web-app"></a>Aplikacja sieci web firmy Contoso University

Aplikacja wbudowana w tych samouczkach to podstawowe university witryna sieci web.

Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów. Oto kilka ekranów, utworzone w tym samouczku.

![Strona indeksu uczniów](intro/_static/students-index.png)

![Strona edytowania uczniów](intro/_static/student-edit.png)

Styl interfejsu użytkownika w tej lokacji znajduje się w pobliżu co to jest generowany przez wbudowane szablony. Samouczek koncentruje się na programu EF Core ze stronami Razor, nie interfejs użytkownika.

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>Tworzenie aplikacji sieci web ContosoUniversity stron Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core. Nadaj projektowi nazwę **ContosoUniversity**. Ważne jest, aby nadaj projektowi nazwę *ContosoUniversity* , przestrzenie nazw dopasować sytuacje, kiedy kod jest skopiowane i wklejone.
* Wybierz **platformy ASP.NET Core 2.1** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.

Obrazy te czynności, zobacz [tworzenie aplikacji sieci web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).
Uruchom aplikację.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka zmian, skonfiguruj w menu witryny, układu i strony głównej. Aktualizacja *Pages/Shared/_Layout.cshtml* z następującymi zmianami:

* Należy zmienić każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso". Istnieją trzy wystąpienia.

* Dodaj elementy menu dla **studentów**, **kursów**, **Instruktorzy**, i **działów**i Usuń **skontaktuj się z pomocą** wpis menu.

Zmiany są wyróżnione. (Wszystkich znaczników jest *nie* wyświetlane.)

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

W *Pages/Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zamienić tekst o platformie ASP.NET i MVC z tekstem o tej aplikacji:

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Tworzenie klas jednostek dla aplikacji Contoso University. Uruchom przy użyciu następujących trzech jednostek:

![Diagram modelu danych kurs — rejestracja-ucznia](intro/_static/data-model-diagram.png)

Istnieje relacja jeden do wielu między `Student` i `Enrollment` jednostek. Istnieje relacja jeden do wielu między `Course` i `Enrollment` jednostek. Uczniem/uczennicą mogą rejestrować się w dowolnej liczby kursów. Kurs może mieć dowolną liczbę uczniów zarejestrowane w nim.

W poniższych sekcjach tworzenia klasy dla każdego z tych jednostek.

### <a name="the-student-entity"></a>Jednostki dla uczniów

![Diagram jednostek dla uczniów](intro/_static/student-entity.png)

Tworzenie *modeli* folderu. W *modeli* folderze utwórz plik klasy o nazwie *Student.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

`ID` Właściwość stanie się kolumna klucza podstawowego w tabeli bazy danych (baza danych), która odnosi się do tej klasy. Domyślnie program EF Core interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy. W `classnameID`, `classname` jest nazwą klasy. Alternatywnie rozpoznawane automatycznie, klucz podstawowy jest `StudentID` w poprzednim przykładzie.

`Enrollments` Właściwość jest właściwością nawigacji. Właściwości nawigacji link do innych jednostek, które są powiązane z tej jednostki. W tym przypadku `Enrollments` właściwość `Student entity` przechowuje wszystkie `Enrollment` jednostek, które są powiązane z tym, które `Student`. Na przykład, jeśli wiersz dla uczniów w bazy danych ma dwa powiązane wiersze rejestracji `Enrollments` właściwość nawigacji zawiera tych dwóch `Enrollment` jednostek. Powiązane `Enrollment` wiersz jest wierszem, który zawiera ten uczniów wartość klucza podstawowego w `StudentID` kolumny. Na przykład, załóżmy, że dla uczniów o identyfikatorze = 1 ma dwa wiersze `Enrollment` tabeli. `Enrollment` Tabela ma dwa wiersze z `StudentID` = 1. `StudentID` to klucz obcy w `Enrollment` tabela, która określa uczniów w `Student` tabeli.

Jeśli właściwość nawigacji może zawierać wiele jednostek, właściwość nawigacji musi być typu listy, takich jak `ICollection<T>`. `ICollection<T>` można określić, lub typu, takie jak `List<T>` lub `HashSet<T>`. Gdy `ICollection<T>` jest używany, tworzy programu EF Core `HashSet<T>` kolekcji domyślnie. Właściwości nawigacji, które zawierają wiele jednostek pochodzą z relacji wiele do wielu i jeden do wielu.

### <a name="the-enrollment-entity"></a>Jednostki rejestracji

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

W *modeli* folderze utwórz *Enrollment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Właściwość jest kluczem podstawowym. Używa tej jednostki `classnameID` wzorca zamiast `ID` takich jak `Student` jednostki. Zwykle programiści wybierz jednym ze wzorców i używać go w całym modelu danych. Przy użyciu Identyfikatora bez classname jest wyświetlany później w samouczku, aby ułatwić implementują dziedziczenie w modelu danych.

`Grade` Właściwość `enum`. Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość ma wartość null. Ma wartość null, która różni się od zera klasy korporacyjnej — wartość null oznacza, że wartość nie jest znana lub nie została jeszcze przypisana.

`StudentID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Student`. `Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, więc ta właściwość zawiera pojedynczy `Student` jednostki. `Student` Jednostki różni się od `Student.Enrollments` właściwość nawigacji, która zawiera wiele `Enrollment` jednostek.

`CourseID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Course`. `Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.

EF Core interpretuje właściwość jako klucz obcy, jeśli jest on nazwany `<navigation property name><primary key property name>`. Na przykład`StudentID` dla `Student` właściwość nawigacji, ponieważ `Student` jest klucz podstawowy jednostki `ID`. Może również mieć nazwę właściwości klucza obcego `<primary key property name>`. Na przykład `CourseID` ponieważ `Course` jest klucz podstawowy jednostki `CourseID`.

### <a name="the-course-entity"></a>Jednostki kursu

![Diagram jednostek kursu](intro/_static/course-entity.png)

W *modeli* folderze utwórz *Course.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Właściwość jest właściwością nawigacji. A `Course` jednostki mogą być one związane z dowolną liczbę `Enrollment` jednostek.

`DatabaseGenerated` Atrybut umożliwia aplikacji określenie klucza podstawowego, a nie bazy danych po jego wygenerowaniu.

## <a name="scaffold-the-student-model"></a>Tworzenie szkieletu modelu dla uczniów

W tej sekcji jest szkieletu modelu dla uczniów. Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) dla modelu dla uczniów.

* Skompiluj projekt.
* Tworzenie *stron/uczniów* folderu.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron/uczniów* folder > **Dodaj** > **nowy element szkieletu**.
* W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **strony Razor za pomocą programu Entity Framework (CRUD)** > **Dodaj**.

Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:

* W **klasa modelu** listę rozwijaną, wybierz opcję **uczniów (ContosoUniversity.Models)**.
* W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i Zmień nazwę wygenerowanego na **ContosoUniversity.Models.SchoolContext**.
* W **klasa kontekstu danych** listę rozwijaną, wybierz opcję **ContosoUniversity.Models.SchoolContext**
* Wybierz **Dodaj**.

![Okno dialogowe CRUD](intro/_static/s1.png)

Zobacz [tworzenia szkieletu modelu movie](xref:tutorials/razor-pages/model#scaffold-the-movie-model) Jeśli masz problem z poprzedniego kroku.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uruchom następujące polecenia, aby utworzyć szkielet modelu dla uczniów.

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

Proces szkieletu tworzonych i zmienianych następujące pliki:

### <a name="files-created"></a>Utworzone pliki

* *Strony/uczniów* tworzenie, edytowanie usuwania, uzyskać szczegółowe informacje, indeksu.
* *Data/ContosoUniversityContext.cs*

### <a name="files-updates"></a>Pliki aktualizacji

* *Startup.cs* : wyszczególniono zmiany do tego pliku w następnej sekcji.
* *appSettings.JSON* : parametry połączenia używane do łączenia z lokalnej bazy danych zostanie dodany.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności

Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora. Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.

Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.

Sprawdź `ConfigureServices` method in Class metoda *Startup.cs*. Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu. Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.

## <a name="update-main"></a>Zaktualizuj główne

W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności:

* Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.
* Wywołaj [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).
* Usuń kontekst podczas `EnsureCreated` ukończeniu metody.

Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated` zapewnia, że istnieje baza danych dla kontekstu. Jeśli istnieje, nie podjęto żadnej akcji. Jeśli nie istnieje, bazy danych i jego schematu są tworzone. `EnsureCreated` Używaj migracji do tworzenia bazy danych. Bazy danych, która jest tworzona przy użyciu `EnsureCreated` nie można później zaktualizować przy użyciu migracji.

`EnsureCreated` jest wywoływana przy uruchomieniu aplikacji, co pozwala następujący przepływ pracy:

* Usuwanie bazy danych.
* Zmiany schematu bazy danych (na przykład dodać `EmailAddress` pola).
* Uruchom aplikację.
* `EnsureCreated` Tworzy BAZĘ danych za pomocą`EmailAddress` kolumny.

`EnsureCreated` jest wygodne na wczesnym etapie projektowania, gdy schemat szybko się zmienia. Później w samouczku baza danych zostanie usunięty i migracje są używane.

### <a name="test-the-app"></a>Testowanie aplikacji

Uruchom aplikację i zaakceptować zasady plików cookie. Ta aplikacja nie zachowuje informacji osobistych. Możesz przeczytać temat zasady plików cookie w [Obsługa Unii Europejskiej ogólnego danych (GDPR Protection Regulation)](xref:security/gdpr).

* Wybierz **studentów** link a następnie **Utwórz nowy**.
* Przetestuj Edytuj, szczegóły i usuwać łącza.

## <a name="examine-the-schoolcontext-db-context"></a>Badanie kontekstu SchoolContext DB

Główna klasa, która służy do koordynowania funkcji EF Core dla modelu danych jest z klasy kontekstu bazy danych. Kontekst danych jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst danych określa, które jednostki są uwzględnione w modelu danych. W tym projekcie nosi nazwę klasy `SchoolContext`.

Aktualizacja *SchoolContext.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

Wyróżniony kod tworzy [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwości dla każdego zestawu jednostek. W terminologii programu EF Core:

* Zwykle zestaw jednostek odnosi się do tabeli bazy danych.
* Jednostki odnosi się do wiersza w tabeli.

`DbSet<Enrollment>` i `DbSet<Course>` mógł zostać pominięty. EF Core je uwzględniał niejawnie ponieważ `Student` odwołań do jednostek `Enrollment` jednostki, a `Enrollment` odwołań do jednostek `Course` jednostki. Na potrzeby tego samouczka Zachowaj `DbSet<Enrollment>` i `DbSet<Course>` w `SchoolContext`.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Parametry połączenia określają [programu SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych i jest przeznaczony do tworzenia aplikacji, a nie do użytku produkcyjnego. LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji. Domyślnie tworzy LocalDB *.mdf* pliki bazy danych `C:/Users/<user>` katalogu.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Dodaj kod, aby zainicjować bazy danych przy użyciu danych testowych

EF Core tworzy pustą bazy danych. W tej sekcji `Initialize` metody są zapisywane do wypełniania danych testowych.

W *danych* folderu, Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Dodaj następujący kod:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

Kod sprawdza, czy wszystkie studentów w bazie danych. W przypadku nie studentów w bazie danych, baza danych jest inicjowany z danych testowych. Ładuje dane testowe do tablic zamiast `List<T>` kolekcje w celu zoptymalizowania wydajności.

`EnsureCreated` Metoda automatycznie tworzy bazy danych, aby uzyskać kontekst bazy danych. Jeśli baza danych istnieje, `EnsureCreated` zwraca bez modyfikowania bazy danych.

W *Program.cs*, zmodyfikuj `Main` metodę do wywołania `Initialize`:

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

Usuń wszystkie rekordy dla uczniów i uruchom ponownie aplikację. Jeśli baza danych nie został zainicjowany, należy ustawić punkt przerwania w `Initialize` Aby zdiagnozować problem.

## <a name="view-the-db"></a>Widok bazy danych

Otwórz **Eksplorator obiektów SQL Server** (SSOX) z **widoku** menu w programie Visual Studio.
SSOX, kliknij **(localdb) \MSSQLLocalDB > bazy danych > ContosoUniversity1**.

Rozwiń **tabel** węzła.

Kliknij prawym przyciskiem myszy **uczniów** tabeli, a następnie kliknij przycisk **dane widoku** kolumn utworzonych i wiersze wstawione do tabeli.

## <a name="asynchronous-code"></a>Kod asynchroniczny

Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i programem EF Core.

Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte. Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane. Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć. Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań. W rezultacie kod asynchroniczny umożliwia bardziej efektywne wykorzystanie zasobów serwera i serwera jest włączona, aby obsłużyć większy ruch, bez opóźnień.

Kod asynchroniczny wprowadzić niewielkiej ilości obciążenia w czasie wykonywania. W sytuacjach o niewielkim ruchu wpływający na wydajność jest nieistotny, podczas w sytuacjach dużego ruchu, potencjalna poprawa wydajności jest znacząca.

W poniższym kodzie [async](/dotnet/csharp/language-reference/keywords/async) — słowo kluczowe, `Task<T>` zwracają wartość, `await` — słowo kluczowe, i `ToListAsync` metoda powoduje, że kod, są wykonywane asynchronicznie.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` — Słowo kluczowe informuje kompilator, aby:
  * Generuj wywołania zwrotne dla części treści metody.
  * Automatycznie twórz [zadań](/dotnet/api/system.threading.tasks.task) obiekt, który jest zwracany. Aby uzyskać więcej informacji, zobacz [typie zwracanym zadań](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Niejawny zwracany typ `Task` reprezentuje pracę w toku.
* `await` — Słowo kluczowe powoduje, że kompilator podzielić metodę na dwie części. Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie. Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.
* `ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.

Kilka rzeczy, w których trzeba pamiętać podczas pisania kodu asynchronicznego, który używa programu EF Core:

* Tylko te instrukcje, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie. Zawierającej, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, i `SaveChangesAsync`. Nie zawiera instrukcji, które można zmienić `IQueryable`, takich jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* Z kontekstu programu EF Core nie jest bezpieczny dla wątków: nie należy próbować wykonać wiele operacji wykonywane równolegle.
* Aby móc korzystać z zalet wydajności kod asynchroniczny, sprawdź, czy biblioteka pakietów (np. stronicowania) używają asynchronicznej, jeśli wywołują metod programu EF Core, które wysyłają zapytania do bazy danych.

Aby uzyskać więcej informacji na temat programowania asynchronicznego w .NET, zobacz [Przegląd Async](/dotnet/articles/standard/async) i [programowanie asynchroniczne z async i await](/dotnet/csharp/programming-guide/concepts/async/).

W następnym samouczku basic CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) są badane.
::: moniker-end

> [!div class="step-by-step"]
> [Next](xref:data/ef-rp/crud)
