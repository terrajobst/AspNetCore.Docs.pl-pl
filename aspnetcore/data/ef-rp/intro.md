---
title: Stron razor z Entity Framework Core w platformy ASP.NET Core - 1 samouczka 8
author: rick-anderson
description: Pokazuje, jak utworzyć aplikację stron Razor przy użyciu programu Entity Framework Core
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/intro
ms.openlocfilehash: a31b3e0ad72964a0c01c0b855a70d2f3e8966ab9
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033293"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Stron razor z Entity Framework Core w platformy ASP.NET Core - 1 samouczka 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji platformy ASP.NET Core Razor strony za pomocą Core Entity Framework (EF).

Przykładowa aplikacja jest witryną sieci web dla fikcyjnej uniwersytetu Contoso. Obejmuje funkcje, takie jak wprowadzenia studentów, tworzenie kursu i instruktora przypisania. Ta strona jest to pierwszy z serii samouczków, które opisują sposób kompilowania aplikacji przykładowej Contoso University.

[Pobrania lub wyświetlenia ukończonej aplikacji.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[! OBEJMUJĄ [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

[! [] INCLUDE (~ / includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]

------

Znajomość [stron Razor](xref:razor-pages/index). Nowe programistów powinno zakończyć się [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start) przed uruchomieniem tej serii.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli napotkasz problem, nie można rozpoznać zwykle można znaleźć rozwiązania na podstawie porównania ilości kodu do [projektu zakończone](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Dobrym sposobem uzyskania pomocy jest publikując pytanie, aby [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) dla [platformy ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

## <a name="the-contoso-university-web-app"></a>Aplikacja sieci web firmy Contoso University

Aplikacji utworzony w tych samouczkach jest witryną sieci web university podstawowe.

Użytkownicy mogą wyświetlać i uczniów, kursu i informacje o wykładowcach aktualizacji. Poniżej przedstawiono kilka ekranów utworzone w samouczku.

![Strona indeksu uczniów lub studentów](intro/_static/students-index.png)

![Strona Edytuj uczniów lub studentów](intro/_static/student-edit.png)

Styl interfejsu użytkownika w tej lokacji jest bliski co to jest generowany przez wbudowane szablony. Samouczek koncentruje się na podstawowych EF Razor strony, nie interfejsu użytkownika.

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>Tworzenie aplikacji sieci web ContosoUniversity stron Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.
* Utwórz nową aplikację sieci Web platformy ASP.NET Core. Nazwij projekt **ContosoUniversity**. Ważne jest, aby nazwa projektu *ContosoUniversity* tak dopasuj przestrzenie nazw, jeśli kod jest skopiowane i wklejone.
* Wybierz **platformy ASP.NET Core 2.1** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.

W przypadku obrazów powyższych kroków, zobacz [utworzenia aplikacji sieci web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).
Uruchom aplikację.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka zmian ustawienia lokacji menu, układu i strony głównej. Aktualizacja *Pages/Shared/_Layout.cshtml* z następującymi zmianami:

* Zmienić każde wystąpienie "ContosoUniversity" na "Contoso University". Istnieją trzy wystąpienia.

* Dodaj elementy menu dla **studentów**, **kursów**, **instruktorów**, i **działów**i Usuń **skontaktuj się z** wpisu menu.

Zmiany zostały wyróżnione. (Kod znaczników jest *nie* wyświetlane.)

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

W *Pages/Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zastąpić tekst o ASP.NET i MVC tekst o tej aplikacji:

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Tworzenie klas jednostek dla aplikacji Contoso University. Rozpocząć następujące trzy jednostki:

![Diagram modelu danych kursu-rejestracji — dla użytkowników domowych](intro/_static/data-model-diagram.png)

Brak relacji jeden do wielu między `Student` i `Enrollment` jednostek. Brak relacji jeden do wielu między `Course` i `Enrollment` jednostek. Gdy uczniowie mogą rejestrować się w dowolnej liczby kursów. Kursu może mieć dowolną liczbę studentów zarejestrowane w nim.

W poniższych sekcjach utworzeniu klasy dla każdego z tych obiektów.

### <a name="the-student-entity"></a>Jednostki dla użytkowników domowych

![Diagram jednostek dla użytkowników domowych](intro/_static/student-entity.png)

Utwórz *modele* folderu. W *modele* folderu, Utwórz plik klasy o nazwie *Student.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

`ID` Właściwości staje się kolumna klucza podstawowego tabeli bazy danych (bazy danych), która odnosi się do tej klasy. Domyślnie EF rdzenie będą interpretowane przez właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy. W `classnameID`, `classname` jest nazwą klasy. Alternatywne rozpoznawane automatycznie, klucz podstawowy jest `StudentID` w poprzednim przykładzie.

`Enrollments` Właściwość jest właściwością nawigacji. Właściwości nawigacji, połącz się z innymi obiektami, które są powiązane z tą jednostką. W takim przypadku `Enrollments` właściwość `Student entity` przechowuje wszystkie `Enrollment` jednostek, które są powiązane z których `Student`. Na przykład, jeśli wiersz uczniów w bazie danych ma dwa powiązane wiersze rejestracji `Enrollments` właściwość nawigacji zawiera tych dwóch `Enrollment` jednostek. Powiązane `Enrollment` wiersz jest wierszem zawiera Studenta dla tej wartości klucza podstawowego w `StudentID` kolumny. Na przykład, załóżmy, że uczniów o identyfikatorze = 1 ma dwa wiersze `Enrollment` tabeli. `Enrollment` Tabela zawiera dwa wiersze z `StudentID` = 1. `StudentID` jest to kolumna klucza obcego w `Enrollment` tabeli określający student w `Student` tabeli.

Jeśli właściwość nawigacji może zawierać wiele jednostek, właściwość nawigacji musi być typem listy, takich jak `ICollection<T>`. `ICollection<T>` można określić, takie jak typ lub `List<T>` lub `HashSet<T>`. Gdy `ICollection<T>` jest używana, tworzy EF Core `HashSet<T>` kolekcji domyślnie. Właściwości nawigacji, które zawierają wiele jednostek pochodzą z relacji wiele do wielu oraz jeden do wielu.

### <a name="the-enrollment-entity"></a>Jednostka rejestracji

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

W *modele* folderu, Utwórz *Enrollment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Właściwość jest kluczem podstawowym. Używa tej jednostki `classnameID` wzorca zamiast `ID` jak `Student` jednostki. Zwykle programiści wybierz jeden wzorzec i używać go w modelu danych. W samouczku nowsze za pomocą Identyfikatora bez classname przedstawiono ułatwiające wdrażanie dziedziczenia w modelu danych.

`Grade` Jest właściwość `enum`. Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość dopuszcza wartość null. Ma wartość null, która różni się od zera klasy — wartość null oznacza, że klasa nie jest znana lub nie została jeszcze przypisana.

`StudentID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Student`. `Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, więc ta właściwość zawiera jeden `Student` jednostki. `Student` Jednostki różni się od `Student.Enrollments` właściwość nawigacji, która zawiera wiele `Enrollment` jednostek.

`CourseID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Course`. `Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.

EF Core interpretuje właściwość jako klucz obcy, jeśli jest o nazwie `<navigation property name><primary key property name>`. Na przykład`StudentID` dla `Student` właściwość nawigacji, ponieważ `Student` klucza podstawowego jednostki jest `ID`. Właściwości klucza obcego może również być nazwany `<primary key property name>`. Na przykład `CourseID` ponieważ `Course` klucza podstawowego jednostki jest `CourseID`.

### <a name="the-course-entity"></a>Jednostki ciągu

![Diagram jednostek kursu](intro/_static/course-entity.png)

W *modele* folderu, Utwórz *Course.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Właściwość jest właściwością nawigacji. A `Course` jednostka może być powiązane z dowolną liczbę `Enrollment` jednostek.

`DatabaseGenerated` Atrybutu umożliwia aplikacji określenie klucza podstawowego, a nie bazy danych o jego wygenerowaniu.

## <a name="scaffold-the-student-model"></a>Tworzenie szkieletu modelu dla użytkowników domowych

W tej sekcji jest szkieletu modelu studenta. Oznacza to, że narzędzie szkieletów zawiera strony dla operacji tworzenia, odczytu, aktualizacji i usuwania (CRUD) dla uczniów modelu.

* Skompiluj projekt.
* Utwórz *stron/uczniów lub studentów* folderu.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron/uczniów lub studentów* folder > **Dodaj** > **nowy element szkieletu**.
* W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **stron Razor przy użyciu programu Entity Framework (CRUD)** > **dodać**.

Zakończenie **Dodaj Razor strony przy użyciu programu Entity Framework (CRUD)** okna dialogowego:

* W **klasa modelu** listy rozwijanej, wybierz pozycję **uczniów (ContosoUniversity.Models)**.
* W **klasa kontekstu danych** wierszu, wybierz opcję **+** (oraz) Zaloguj się i Zmień nazwę wygenerowanego **ContosoUniversity.Models.SchoolContext**.
* W **klasa kontekstu danych** listy rozwijanej, wybierz pozycję **ContosoUniversity.Models.SchoolContext**
* Wybierz **dodać**.

![Okno dialogowe CRUD](intro/_static/s1.png)

Zobacz [szkieletu modelu filmu](xref:tutorials/razor-pages/model#scaffold-the-movie-model) Jeśli masz problem z poprzedniego kroku.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uruchom następujące polecenia, aby utworzyć szkielet uczniów modelu.

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

Proces szkieletu tworzonych i zmienianych następujące pliki:

### <a name="files-created"></a>Utworzone pliki

* *Strony/uczniów lub studentów* tworzenie, usuwanie i uzyskać szczegółowe informacje, edytowanie indeksu.
* *Data/ContosoUniversityContext.cs*

### <a name="files-updates"></a>Pliki aktualizacji

* *Startup.cs* : wyszczególnione modyfikacje tego pliku w następnej sekcji.
* *appSettings.JSON* : parametry połączenia używane do nawiązania połączenia z lokalną bazą danych został dodany.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Sprawdź kontekst zarejestrowany iniekcji zależności

Zbudowany platformy ASP.NET Core za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst danych podstawowych EF) są zarejestrowane w usłudze iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (takich jak strony Razor) są udostępniane tych usług za pomocą parametrów konstruktora. Kod konstruktora, który pobiera wystąpienia kontekstu db przedstawiono w dalszej części tego samouczka.

Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i zarejestrowana kontenera iniekcji zależności.

Sprawdź `ConfigureServices` metody w *Startup.cs*. Wybrany element został dodany w procesie tworzenia szkieletu:

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

Nazwa ciągu połączenia jest przekazywany do kontekstu, wywołując metodę na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu. Dla wdrożenia lokalnego [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.

## <a name="update-main"></a>Aktualizowanie głównego

W *Program.cs*, zmodyfikuj `Main` metody wykonać następujące czynności:

* Pobierz wystąpienia kontekstu DB z kontenera iniekcji zależności.
* Wywołanie [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).
* Usuwanie kontekście podczas `EnsureCreated` ukończeniu metody.

Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated` zapewnia, że istnieje bazy danych dla kontekstu. Jeśli istnieje, nie podjęto żadnej akcji. Jeśli nie istnieje, bazę danych i jego schematu są tworzone. `EnsureCreated` nie używa migracji można utworzyć bazy danych. Bazy danych, który jest tworzony z `EnsureCreated` nie można później zaktualizować przy użyciu migracji.

`EnsureCreated` jest wywoływana po uruchomieniu aplikacji, dzięki czemu następującego przepływu pracy:

* Usuń bazę danych.
* Zmień schemat bazy danych (na przykład dodać `EmailAddress` pól).
* Uruchom aplikację.
* `EnsureCreated` Tworzy baza danych o`EmailAddress` kolumny.

`EnsureCreated` szybko ewoluuje schematu, jest wygodne wczesnym etapie programowania. Później w samouczku bazy danych są usuwane i migracji są używane.

### <a name="test-the-app"></a>Testowanie aplikacji

Uruchom aplikację i zaakceptuj zasady pliku cookie. Ta aplikacja nie zachowanie poufności informacji osobistych. Informacje o zasad pliku cookie na [obsługę interfejsów UE ogólne dane ochrony rozporządzenia (GDPR)](xref:security/gdpr).

* Wybierz **studentów** łącza, a następnie **Utwórz nowy**.
* Testowanie edycji, uzyskać szczegółowe informacje i Usuń linki.

## <a name="examine-the-schoolcontext-db-context"></a>Sprawdź kontekst bazy danych SchoolContext

Klasy głównym, która koordynuje EF podstawową funkcjonalność o dany model danych jest klasa kontekstu bazy danych. Kontekst danych jest określana na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst danych określa jednostki, które znajdują się w modelu danych. W tym projekcie klasy o nazwie `SchoolContext`.

Aktualizacja *SchoolContext.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

Tworzy wyróżniony kod [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwości dla każdego zestawu jednostek. W terminologii EF rdzeni:

* Zwykle zestawu jednostek odnosi się do tabeli bazy danych.
* Jednostka odpowiada wiersza w tabeli.

`DbSet<Enrollment>` i `DbSet<Course>` może być pominięte. Podstawowe EF są uwzględniane niejawnie ponieważ `Student` odwołań do jednostek `Enrollment` jednostki i `Enrollment` odwołań do jednostek `Course` jednostki. W tym samouczku, Zachowaj `DbSet<Enrollment>` i `DbSet<Course>` w `SchoolContext`.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB.

Określa parametry połączenia [bazy danych LocalDB programu SQL Server](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). LocalDB jest wersja programu SQL Server Express aparatu bazy danych i jest przeznaczony do tworzenia aplikacji, a nie do użytku produkcyjnego. LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonych konfiguracji. Domyślnie tworzy LocalDB *.mdf* pliki bazy danych w `C:/Users/<user>` katalogu.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Dodaj kod, aby zainicjować bazy danych z danych testowych

Podstawowe EF tworzy puste bazy danych. W tej sekcji `Initialize` zapisywana jest metoda wypełnić je danymi testu.

W *danych* folderu, Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Dodaj następujący kod:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

Kod sprawdza, czy wszystkie studentów w bazie danych. Jeśli w bazie danych nie nie studentów, bazy danych jest inicjowany z danych testowych. Ładuje dane testowe do tablic zamiast `List<T>` kolekcje w celu optymalizacji wydajności.

`EnsureCreated` Metoda automatycznie tworzy bazy danych dla kontekst bazy danych. Jeśli baza danych istnieje, `EnsureCreated` zwraca bez modyfikowania bazy danych.

W *Program.cs*, zmodyfikuj `Main` metodę do wywołania `Initialize`:

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

Usuń żadnych rekordów dla użytkowników domowych i uruchom ponownie aplikację. Jeśli bazy danych nie został zainicjowany, ustaw punkt przerwania `Initialize` do zdiagnozowania problemu.

## <a name="view-the-db"></a>Widok bazy danych

Otwórz **Eksplorator obiektów SQL Server** (SSOX) z **widoku** menu w programie Visual Studio.
W SSOX, kliknij przycisk **(localdb) \MSSQLLocalDB > baz danych > ContosoUniversity1**.

Rozwiń węzeł **tabel** węzła.

Kliknij prawym przyciskiem myszy **uczniowie** tabeli, a następnie kliknij przycisk **danych widoku** kolumn utworzona i wiersze wstawione do tabeli.

## <a name="asynchronous-code"></a>Kod asynchroniczny

Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i EF Core.

Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach wysokie obciążenie wszystkich dostępnych wątków może być używana. W takim przypadku serwer nie może przetworzyć nowe żądania aż wątki są zwalniane. Z kodem synchroniczne wiele wątków może powiązany, gdy nie są faktycznie wykonują pracę, ponieważ ich oczekiwania operacji We/Wy zakończyć. Asynchroniczne kodu podczas procesu jest oczekiwania operacji We/Wy zakończyć, jego wątku zostanie zwolniona dla serwera na potrzeby przetwarzania innych żądań. W związku z tym kod asynchroniczny umożliwia bardziej efektywne wykorzystanie zasobów serwera, a serwer jest skonfigurowany do obsługi ruchu więcej bez opóźnień.

Asynchroniczne kodu wprowadzenie niewielkiej liczby dodatkowych czynności w czasie wykonywania. W sytuacjach niski ruchu trafień wydajności jest niewielka, podczas w sytuacjach dużego natężenia ruchu sieciowego, potencjalne zwiększenie wydajności jest znacząca.

W poniższym kodzie [async](/dotnet/csharp/language-reference/keywords/async) — słowo kluczowe, `Task<T>` zwrócić wartość, `await` — słowo kluczowe, i `ToListAsync` metoda powoduje, że kod asynchroniczne.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` — Słowo kluczowe informuje kompilator, aby:
  * Generuj wywołania zwrotne dla części treści metody.
  * Automatyczne tworzenie [zadań](/dotnet/api/system.threading.tasks.task) obiekt, który jest zwracany. Aby uzyskać więcej informacji, zobacz [typu zwracanego zadań](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Typ zwracany niejawne `Task` reprezentuje pracy w toku.
* `await` — Słowo kluczowe powoduje, że kompilator podzielić na dwie części metodę. Pierwsza część kończy operację, który jest uruchamiany asynchronicznie. Druga część są umieszczane w metodę wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.
* `ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.

Należy pamiętać o podczas pisania kodu asynchroniczne EF Core używa w kilku kwestiach:

* Tylko te instrukcje, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie. Zawierającej, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, i `SaveChangesAsync`. Nie zawiera instrukcji, które można zmienić `IQueryable`, takich jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* Kontekst EF Core nie jest bezpieczne dla wątków: nie należy próbować wykonać kilka operacji wykonywane równolegle.
* Można wykorzystać zalety wydajności async kodu, sprawdź, czy pakiety bibliotekę (takie jak w przypadku stronicowania) używają async Jeśli wywołują metody EF podstawowych, które wysyłanie zapytań do bazy danych.

Aby uzyskać więcej informacji na temat programowania asynchronicznego w programie .NET, zobacz [omówienie Async](/dotnet/articles/standard/async) i [programowanie asynchroniczne z async i await](/dotnet/csharp/programming-guide/concepts/async/).

W następnym samouczku basic CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) operacje są sprawdzane.
::: moniker-end

> [!div class="step-by-step"]
> [Next](xref:data/ef-rp/crud)