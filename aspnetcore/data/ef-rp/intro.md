---
title: Strony razor za pomocą platformy Entity Framework Core w programie ASP.NET Core — samouczek 1 8
author: rick-anderson
description: Pokazuje, jak utworzyć aplikację strony Razor za pomocą platformy Entity Framework Core
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 1a9d83be9180b1d32ab941932eb3cab8612dff01
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213405"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Strony razor za pomocą platformy Entity Framework Core w programie ASP.NET Core — samouczek 1 8

Autorzy [Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Jest to pierwsza z serii samouczków, które pokazują, jak używać rdzenia Entity Framework (EF) w aplikacji [ASP.NET Core Razor Pages](xref:razor-pages/index) . Samouczki budują witrynę sieci Web dla fikcyjnej uczelni firmy Contoso. Lokacja obejmuje funkcje, takie jak przyjmowanie uczniów, tworzenie kursu i przydziały instruktora. Samouczek używa metody Code First. Aby uzyskać informacje na temat korzystania z pierwszego podejścia do bazy danych, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/16897)w usłudze GitHub.

[Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

## <a name="prerequisites"></a>Wymagania wstępne

* Jeśli dopiero zaczynasz korzystać z Razor Pages, przejdź do serii samouczek wprowadzenie [do Razor Pages](xref:tutorials/razor-pages/razor-pages-start) , przed rozpoczęciem tej.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a>Aparaty bazy danych

Instrukcje programu Visual Studio używają [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), wersji SQL Server Express, która działa tylko w systemie Windows.

Instrukcje Visual Studio Code korzystają z [oprogramowania SQLite](https://www.sqlite.org/), wieloplatformowego aparatu bazy danych.

Jeśli zdecydujesz się na korzystanie z oprogramowania SQLite, Pobierz i zainstaluj narzędzie innej firmy służące do zarządzania bazą danych programu SQLite i wyświetlania jej, na przykład w [przeglądarce DB dla oprogramowania SQLite](https://sqlitebrowser.org/).

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli wystąpi problem, którego nie można rozwiązać, porównaj swój kod z [zakończonym projektem](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Dobrym sposobem uzyskania pomocy jest opublikowanie pytania do StackOverflow.com przy użyciu [tagu ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [tagu EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

## <a name="the-sample-app"></a>Przykładowa aplikacja

Aplikacja wbudowana w tych samouczkach to podstawowe university witryna sieci web. Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów. Oto kilka ekranów, utworzone w tym samouczku.

![Strona indeksu uczniów](intro/_static/students-index30.png)

![Strona edytowania uczniów](intro/_static/student-edit30.png)

Styl interfejsu użytkownika tej witryny jest oparty na wbudowanych szablonach projektów. Fokus samouczka dotyczy korzystania z EF Core, a nie dostosowywania interfejsu użytkownika.

Skorzystaj z linku w górnej części strony, aby uzyskać kod źródłowy dla ukończonego projektu. Folder *cu30* ma kod dla ASP.NET Core wersji 3,0 samouczka. Pliki odzwierciedlające stan kodu dla samouczków 1-7 można znaleźć w folderze *cu30snapshots* .

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Aby uruchomić aplikację po pobraniu ukończonego projektu:

* Usuń trzy pliki i jeden folder, w którym znajduje się nazwa *oprogramowania SQLite* .
* Skompiluj projekt.
* W konsoli Menedżera pakietów (PMC) Uruchom następujące polecenie:

  ```powershell
  Update-Database
  ```

* Uruchom projekt, aby wypełniać bazę danych.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aby uruchomić aplikację po pobraniu ukończonego projektu:

* Usuń *ContosoUniversity. csproj*i Zmień nazwę *ContosoUniversitySQLite. csproj* na *ContosoUniversity. csproj*.
* Usuń *Startup.cs*i zmień nazwę *StartupSQLite.cs* na *Startup.cs*.
* Usuń plik *appSettings. JSON*i Zmień nazwę *appSettingsSQLite. JSON* na *appSettings. JSON*.
* Usuń folder *migracji* , a następnie zmień nazwę *MigrationsSQL* na *migracji*.
* Skompiluj projekt.
* W wierszu polecenia w folderze projektu uruchom następujące polecenia:

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet ef database update
  ```

* W narzędziu SQLite uruchom następującą instrukcję SQL:

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* Uruchom projekt, aby wypełniać bazę danych.

---

## <a name="create-the-web-app-project"></a>Tworzenie projektu aplikacji sieci Web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.
* Wybierz **ASP.NET Core aplikacji sieci Web**.
* Nazwij projekt *ContosoUniversity*. Ważne jest, aby użyć tej dokładnej nazwy, łącznie z wielką literą, więc przestrzenie nazw są zgodne, gdy kod jest kopiowany i wklejany.
* Na liście rozwijanej wybierz pozycję **.NET Core** i **ASP.NET Core 3,0** , a następnie wybierz pozycję **aplikacja sieci Web**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* W terminalu przejdź do folderu, w którym ma zostać utworzony folder projektu.

* Uruchom następujące polecenia, aby utworzyć projekt Razor Pages i `cd` do nowego folderu projektu:

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Skonfiguruj nagłówek, stopkę i menu witryny przez aktualizację *stron/Shared/_Layout. cshtml*:

* Należy zmienić każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso". Istnieją trzy wystąpienia.

* Usuń wpisy menu **Home** i **privacy** oraz Dodaj wpisy dotyczące **osób,** **studentów**, **kursów**, **instruktorów**i **działów**.

Zmiany są wyróżnione.

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

W obszarze *Pages/index. cshtml*Zastąp zawartość pliku następującym kodem, aby zamienić tekst na ASP.NET Core z tekstem dotyczącym tej aplikacji:

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

Uruchom aplikację, aby sprawdzić, czy zostanie wyświetlona strona główna.

## <a name="the-data-model"></a>Model danych

W poniższych sekcjach opisano tworzenie modelu danych:

![Diagram modelu danych kurs — rejestracja-ucznia](intro/_static/data-model-diagram.png)

Student może zarejestrować się w dowolnej liczbie kursów, a kurs może mieć dowolną liczbę studentów, którzy zarejestrowali się w nim.

## <a name="the-student-entity"></a>Jednostki dla uczniów

![Diagram jednostek dla uczniów](intro/_static/student-entity.png)

* Utwórz folder *models* w folderze projektu. 

* Utwórz *modele/uczniów. cs* przy użyciu następującego kodu:

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

Właściwość `ID` jest kolumną klucza podstawowego tabeli bazy danych, która odnosi się do tej klasy. Domyślnie EF Core interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy. W związku z tym alternatywną automatycznie rozpoznawaną nazwą klucza podstawowego klasy `Student` jest `StudentID`.

Właściwość `Enrollments` jest [właściwością nawigacji](/ef/core/modeling/relationships). Właściwości nawigacji zawierają inne jednostki, które są powiązane z tą jednostką. W takim przypadku Właściwość `Enrollments` jednostki `Student` zawiera wszystkie jednostki `Enrollment`, które są powiązane z tym uczniem. Na przykład jeśli wiersz student w bazie danych ma dwa powiązane wiersze rejestracji, właściwość nawigacji `Enrollments` zawiera te dwie jednostki rejestracji. 

W bazie danych wiersz rejestracji jest powiązany z wierszem studenta, jeśli jego kolumna StudentID zawiera wartość identyfikatora studenta. Załóżmy na przykład, że wiersz student ma identyfikator = 1. Powiązane wiersze rejestracji będą mieć StudentID = 1. StudentID jest *kluczem obcym* w tabeli rejestracji. 

Właściwość `Enrollments` jest definiowana jako `ICollection<Enrollment>`, ponieważ może istnieć wiele powiązanych jednostek rejestracji. Można użyć innych typów kolekcji, takich jak `List<Enrollment>` lub `HashSet<Enrollment>`. Gdy `ICollection<Enrollment>` jest używany, EF Core domyślnie tworzy kolekcję `HashSet<Enrollment>`.

## <a name="the-enrollment-entity"></a>Jednostki rejestracji

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

Utwórz *modele/rejestrację. cs* przy użyciu następującego kodu:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

Właściwość `EnrollmentID` jest kluczem podstawowym; Ta jednostka używa wzorca `classnameID`, a nie `ID`. Dla modelu danych produkcyjnych wybierz jeden wzorzec i użyj go spójnie. Ten samouczek używa obu tych metod, aby zilustrować, że obie działają. Używanie `ID` bez `classname` ułatwia implementowanie niektórych rodzajów zmian modelu danych.

Właściwość `Grade` jest `enum`. Znak zapytania po deklaracji typu `Grade` wskazuje, że właściwość `Grade` ma [wartość null](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/). Klasa o wartości null różni się od klasy zerowej,&mdash;wartość null oznacza, że Klasa nie jest znana lub nie została jeszcze przypisana.

Właściwość `StudentID` jest kluczem obcym, a odpowiednia właściwość nawigacji jest `Student`. Jednostka `Enrollment` jest skojarzona z jedną jednostką `Student`, więc Właściwość zawiera jedną jednostkę `Student`.

Właściwość `CourseID` jest kluczem obcym, a odpowiednia właściwość nawigacji jest `Course`. Jednostka `Enrollment` jest skojarzona z jedną jednostką `Course`.

EF Core interpretuje właściwość jako klucz obcy, jeśli ma nazwę `<navigation property name><primary key property name>`. Na przykład`StudentID` jest kluczem obcym dla właściwości nawigacji `Student`, ponieważ klucz podstawowy jednostki `Student` jest `ID`. Właściwości klucza obcego mogą być również nazwane `<primary key property name>`. Na przykład `CourseID`, ponieważ klucz podstawowy jednostki `Course` jest `CourseID`.

## <a name="the-course-entity"></a>Jednostki kursu

![Diagram jednostek kursu](intro/_static/course-entity.png)

Utwórz *modele/kurs. cs* przy użyciu następującego kodu:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

Właściwość `Enrollments` jest właściwością nawigacji. Jednostka `Course` może być powiązana z dowolną liczbą `Enrollment` jednostek.

Atrybut `DatabaseGenerated` umożliwia aplikacji określenie klucza podstawowego zamiast wygenerowania go przez bazę danych.

Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilatora.

## <a name="scaffold-student-pages"></a>Strony uczniów tworzenia szkieletu

W tej sekcji użyjesz narzędzia do tworzenia szkieletów ASP.NET Core do wygenerowania:

* Klasa *kontekstu* EF Core. Kontekst jest klasą główną, która koordynuje Entity Framework funkcji dla danego modelu danych. Pochodzi ona z klasy `Microsoft.EntityFrameworkCore.DbContext`.
* Strony Razor obsługujące operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD) dla jednostki `Student`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Utwórz folder *uczniów* w folderze *strony* .
* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *strony/uczniowie* i wybierz polecenie **Dodaj** > **nowy element szkieletowy**.
* W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.
* W oknie dialogowym **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :
  * Z listy rozwijanej **Klasa modelu** wybierz pozycję **student (ContosoUniversity. models)** .
  * W wierszu **klasy kontekstu danych** wybierz znak **+** (plus).
  * Zmień nazwę kontekstu danych z *ContosoUniversity. models. ContosoUniversityContext* na *ContosoUniversity. Data. SchoolContext*.
  * Wybierz pozycję **Dodaj**.

Następujące pakiety są instalowane automatycznie:

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core, aby zainstalować wymagane pakiety NuGet:
<!-- TO DO  After testing, Replace with
[!INCLUDE[](~/includes/includes/add-EF-NuGet-SQLite-CLI.md)]
remove dotnet tool install --global  below
 -->
  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet add package Microsoft.EntityFrameworkCore.Tools
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet add package Microsoft.Extensions.Logging.Debug
  ```

  Pakiet Microsoft. VisualStudio. Web. CodeGeneration. Design jest wymagany do tworzenia szkieletów. Mimo że aplikacja nie będzie używać SQL Server, narzędzie do tworzenia szkieletów musi być pakietem SQL Server.

* Utwórz folder *stron/uczniów* .

* Uruchom następujące polecenie, aby zainstalować narzędzie do tworzenia [szkieletu ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

* Uruchom następujące polecenie, aby uzyskać możliwość tworzenia szkieletu stron z uczniami.

  **W systemie Windows**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  **W systemie macOS lub Linux**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

Jeśli wystąpił problem z poprzednim krokiem, Skompiluj projekt i ponów próbę wykonania szkieletu.

Proces tworzenia szkieletu:

* Tworzy strony Razor w folderze *stron/uczniów* :
  * *Create. cshtml* i *Create.cshtml.cs*
  * *DELETE. cshtml* i *DELETE.cshtml.cs*
  * *Details. cshtml* i *details.cshtml.cs*
  * *Edit. cshtml* i *Edit.cshtml.cs*
  * *Index. cshtml* i *index.cshtml.cs*
* Tworzy *dane/SchoolContext. cs*.
* Dodaje kontekst do iniekcji zależności w *Startup.cs*.
* Dodaje parametry połączenia z bazą danych do pliku *appSettings. JSON*.

## <a name="database-connection-string"></a>Parametry połączenia z bazą danych

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Parametry połączenia określają [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych i jest przeznaczony do tworzenia aplikacji, a nie do użytku produkcyjnego. Domyślnie LocalDB tworzy pliki *MDF* w katalogu `C:/Users/<user>`.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Zmień parametry połączenia w taki sposób, aby wskazywały plik bazy danych programu SQLite o nazwie *cu. DB*:

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a>Aktualizowanie klasy kontekstu bazy danych

Klasa główna, która koordynuje funkcje EF Core dla danego modelu danych, jest klasą kontekstu bazy danych. Kontekst pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst określa, które jednostki są uwzględnione w modelu danych. W tym projekcie Klasa ma nazwę `SchoolContext`.

Zaktualizuj *SchoolContext.cs* przy użyciu następującego kodu:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

Wyróżniony kod tworzy właściwość [nieogólnymi\<>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla każdego zestawu jednostek. W terminologii programu EF Core:

* Zestaw jednostek zwykle odpowiada tabeli bazy danych.
* Jednostki odnosi się do wiersza w tabeli.

Ponieważ zestaw jednostek zawiera wiele jednostek, właściwości Nieogólnymi powinny mieć nazwy w liczbie mnogiej. Ponieważ narzędzie tworzenia szkieletów utworzyło`Student` Nieogólnymi, ten krok zmienia go na `Students`w liczbie mnogiej. 

Aby kod Razor Pages był zgodny z nową nazwą Nieogólnymi, wprowadź globalną zmianę w całym projekcie `_context.Student` do `_context.Students`.  Istnieją 8 wystąpień.

Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilatora.

## <a name="startupcs"></a>Startup.cs

ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst bazy danych EF Core) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora. Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych jest wyświetlany w dalszej części samouczka.

Narzędzie do tworzenia szkieletów automatycznie zarejestrowało klasę kontekstu z kontenerem iniekcji zależności.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W `ConfigureServices`wyróżnione wiersze zostały dodane przez szkieleter:

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* W `ConfigureServices`upewnij się, że kod dodany przez program tworzący szkielet jest wywoływany `UseSqlite`.

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) . W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .

## <a name="create-the-database"></a>Tworzenie bazy danych

Zaktualizuj *program.cs* , aby utworzyć bazę danych, jeśli nie istnieje:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

Jeśli istnieje baza danych dla kontekstu, Metoda [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) nie przyjmuje żadnej akcji. Jeśli baza danych nie istnieje, tworzy bazę danych i schemat. `EnsureCreated` umożliwia obsługę zmian modelu danych w poniższym przepływie pracy:

* Usuń bazę danych. Wszystkie istniejące dane zostaną utracone.
* Zmień model danych. Na przykład Dodaj pole `EmailAddress`.
* Uruchom aplikację.
* `EnsureCreated` tworzy bazę danych z nowym schematem.

Ten przepływ pracy działa dobrze na etapie opracowywania, gdy schemat jest gwałtownie zmieniany, o ile nie trzeba zachować danych. Sytuacja jest inna, gdy dane wprowadzone do bazy danych muszą zostać zachowane. W takim przypadku należy użyć migracji.

W dalszej części tego samouczka usuniesz bazę danych, która została utworzona przez `EnsureCreated` i zamiast tego użyj migracji. Bazy danych utworzonej przez `EnsureCreated` nie można zaktualizować za pomocą migracji.

### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację.
* Wybierz link **Students (studenci** ), a następnie **Utwórz nowy**.
* Przetestuj Edytuj, szczegóły i usuwać łącza.

## <a name="seed-the-database"></a>Inicjowanie bazy danych

Metoda `EnsureCreated` tworzy pustą bazę danych. Ta sekcja dodaje kod wypełniający bazę danych danymi testowymi.

Utwórz *dane/Dbinitializeer. cs* przy użyciu następującego kodu:

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  Kod sprawdza, czy w bazie danych istnieją uczniowie. Jeśli nie ma uczniów, dodaje dane testowe do bazy danych. Tworzy dane testowe w tablicach, a nie `List<T>` kolekcje w celu zoptymalizowania wydajności.

* W *program.cs*Zastąp wywołanie `EnsureCreated` przy użyciu `DbInitializer.Initialize` wywołania:

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie uruchom następujące polecenie w **konsoli Menedżera pakietów** (PMC):

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie usuń plik *cu. DB* .

---

* Uruchom ponownie aplikację.

* Wybierz stronę uczniów, aby zobaczyć dane z rozrzutu.

## <a name="view-the-database"></a>Wyświetlanie bazy danych

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Otwórz **Eksplorator obiektów SQL Server** (SSOX) z menu **Widok** w programie Visual Studio.
* W SSOX wybierz pozycję **(LocalDB) \MSSQLLocalDB > bazy danych > SchoolContext-{GUID}** . Nazwa bazy danych jest generowana na podstawie podanej wcześniej nazwy kontekstu oraz łącznika i identyfikatora GUID.
* Rozwiń węzeł **tabele** .
* Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Wyświetl dane** , aby wyświetlić utworzone kolumny i wiersze wstawione do tabeli.
* Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Wyświetl kod** , aby zobaczyć, jak model `Student` jest mapowany na schemat `Student` tabeli.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Użyj narzędzia SQLite, aby wyświetlić schemat bazy danych i dane referencyjne. Plik bazy danych ma nazwę *cu. DB* i znajduje się w folderze projektu.

---

## <a name="asynchronous-code"></a>Kod asynchroniczny

Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i programem EF Core.

Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte. Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane. Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć. Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań. W efekcie kod asynchroniczny umożliwia efektywniejsze wykorzystanie zasobów serwera, a serwer może obsłużyć więcej ruchu bez opóźnień.

Kod asynchroniczny wprowadzić niewielkiej ilości obciążenia w czasie wykonywania. W sytuacjach o niewielkim ruchu wpływający na wydajność jest nieistotny, podczas w sytuacjach dużego ruchu, potencjalna poprawa wydajności jest znacząca.

W poniższym kodzie słowo kluczowe [Async](/dotnet/csharp/language-reference/keywords/async) , `Task<T>` zwracanej wartości, `await` słowo kluczowe i Metoda `ToListAsync` sprawia, że kod jest wykonywany asynchronicznie.

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* Słowo kluczowe `async` instruuje kompilator, aby:
  * Generuj wywołania zwrotne dla części treści metody.
  * Utwórz obiekt [zadania](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) , który jest zwracany.
* Typ zwracany `Task<T>` reprezentuje bieżącą liczbę zadań.
* Słowo kluczowe `await` powoduje, że kompilator dzieli metodę na dwie części. Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie. Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.
* `ToListAsync` jest asynchroniczną wersją metody rozszerzenia `ToList`.

Kilka rzeczy, w których trzeba pamiętać podczas pisania kodu asynchronicznego, który używa programu EF Core:

* Tylko instrukcje, które powodują, że zapytania lub polecenia wysyłane do bazy danych są wykonywane asynchronicznie. Obejmuje `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`i `SaveChangesAsync`. Nie zawiera on instrukcji, które po prostu zmieniają `IQueryable`, takie jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* Z kontekstu programu EF Core nie jest bezpieczny dla wątków: nie należy próbować wykonać wiele operacji wykonywane równolegle.
* Aby skorzystać z zalet wydajności w kodzie asynchronicznym, należy sprawdzić, czy pakiety biblioteki (na przykład stronicowania) używają wartości asynchronicznej, jeśli są wywoływane EF Core metod wysyłających zapytania do bazy danych.

Aby uzyskać więcej informacji na temat programowania asynchronicznego w programie .NET, zobacz [Omówienie asynchroniczne](/dotnet/standard/async) i [programowanie asynchroniczne z Async i await](/dotnet/csharp/programming-guide/concepts/async/).

## <a name="next-steps"></a>Następne kroki

> [!div class="step-by-step"]
> [Następny samouczek](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Przykładową aplikację sieci web firmy Contoso University pokazuje, jak utworzyć aplikację stron Razor programu ASP.NET Core przy użyciu Core Entity Framework (EF).

Przykładowa aplikacja jest witryną sieci web dla uniwersytetu fikcyjnej firmy Contoso. Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora. Ta strona jest to pierwszy z serii samouczków, które opisują sposób tworzenia przykładowej aplikacji Contoso University.

[Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

Znajomość [Razor Pages](xref:razor-pages/index). Nowi programiści powinni zakończyć pracę [z Razor Pages](xref:tutorials/razor-pages/razor-pages-start) przed rozpoczęciem tej serii.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli wystąpi problem, którego nie można rozwiązać, można ogólnie znaleźć rozwiązanie, porównując kod z [ukończonym projektem](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Dobrym sposobem uzyskania pomocy jest zaksięgowanie pytania [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) na [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

## <a name="the-contoso-university-web-app"></a>Aplikacja sieci web firmy Contoso University

Aplikacja wbudowana w tych samouczkach to podstawowe university witryna sieci web.

Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów. Oto kilka ekranów, utworzone w tym samouczku.

![Strona indeksu uczniów](intro/_static/students-index.png)

![Strona edytowania uczniów](intro/_static/student-edit.png)

Styl interfejsu użytkownika w tej lokacji znajduje się w pobliżu co to jest generowany przez wbudowane szablony. Samouczek koncentruje się na programu EF Core ze stronami Razor, nie interfejs użytkownika.

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>Tworzenie aplikacji sieci web ContosoUniversity stron Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.
* Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core. Nazwij projekt **ContosoUniversity**. Ważne jest, aby nazwa projektu *ContosoUniversity* , tak aby przestrzenie nazw były zgodne, gdy kod jest kopiowany/wklejany.
* Wybierz pozycję **ASP.NET Core 2,1** na liście rozwijanej, a następnie wybierz pozycję **aplikacja sieci Web**.

Aby poznać obrazy powyższych kroków, zobacz [Tworzenie aplikacji sieci Web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).
Uruchom aplikację.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka zmian, skonfiguruj w menu witryny, układu i strony głównej. Aktualizowanie *stron/Shared/_Layout. cshtml* z następującymi zmianami:

* Należy zmienić każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso". Istnieją trzy wystąpienia.

* Dodaj pozycje menu dla **studentów**, **kursów**, **instruktorów**i **działów**, a następnie usuń wpis menu **kontakt** .

Zmiany są wyróżnione. (Wszystkie znaczniki *nie* są wyświetlane).

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

W obszarze *Pages/index. cshtml*Zastąp zawartość pliku następującym kodem, aby zamienić tekst na ASP.NET i MVC z tekstem dotyczącym tej aplikacji:

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Tworzenie klas jednostek dla aplikacji Contoso University. Uruchom przy użyciu następujących trzech jednostek:

![Diagram modelu danych kurs — rejestracja-ucznia](intro/_static/data-model-diagram.png)

Istnieje relacja jeden do wielu między jednostkami `Student` i `Enrollment`. Istnieje relacja jeden do wielu między jednostkami `Course` i `Enrollment`. Uczniem/uczennicą mogą rejestrować się w dowolnej liczby kursów. Kurs może mieć dowolną liczbę uczniów zarejestrowane w nim.

W poniższych sekcjach tworzenia klasy dla każdego z tych jednostek.

### <a name="the-student-entity"></a>Jednostki dla uczniów

![Diagram jednostek dla uczniów](intro/_static/student-entity.png)

Utwórz folder *models* . W folderze *modele* Utwórz plik klasy o nazwie *student.cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

Właściwość `ID` jest kolumną klucza podstawowego tabeli bazy danych (DB), która odnosi się do tej klasy. Domyślnie EF Core interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy. W `classnameID`, `classname` jest nazwą klasy. Alternatywny automatycznie rozpoznany klucz podstawowy jest `StudentID` w poprzednim przykładzie.

Właściwość `Enrollments` jest [właściwością nawigacji](/ef/core/modeling/relationships). Właściwości nawigacji link do innych jednostek, które są powiązane z tej jednostki. W takim przypadku Właściwość `Enrollments` `Student entity` zawiera wszystkie jednostki `Enrollment`, które są powiązane z tym `Student`em. Na przykład jeśli wiersz student w bazie danych ma dwa powiązane wiersze rejestracji, właściwość nawigacji `Enrollments` zawiera te dwa jednostki `Enrollment`. Powiązany `Enrollment` wiersz jest wierszem zawierającym wartość klucza podstawowego studenta w kolumnie `StudentID`. Załóżmy na przykład, że student o IDENTYFIKATORze 1 ma dwa wiersze w tabeli `Enrollment`. Tabela `Enrollment` ma dwa wiersze z `StudentID` = 1. `StudentID` jest kluczem obcym w tabeli `Enrollment`, który określa student w tabeli `Student`.

Jeśli właściwość nawigacji może zawierać wiele jednostek, właściwość nawigacji musi być typem listy, takim jak `ICollection<T>`. można określić `ICollection<T>` lub typ, taki jak `List<T>` lub `HashSet<T>`. Gdy `ICollection<T>` jest używany, EF Core domyślnie tworzy kolekcję `HashSet<T>`. Właściwości nawigacji, które zawierają wiele jednostek pochodzą z relacji wiele do wielu i jeden do wielu.

### <a name="the-enrollment-entity"></a>Jednostki rejestracji

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

W folderze *modele* Utwórz *Enrollment.cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

Właściwość `EnrollmentID` jest kluczem podstawowym. Ta jednostka używa wzorca `classnameID`, a nie `ID`, takiego jak `Student`. Zwykle programiści wybierz jednym ze wzorców i używać go w całym modelu danych. Przy użyciu Identyfikatora bez classname jest wyświetlany później w samouczku, aby ułatwić implementują dziedziczenie w modelu danych.

Właściwość `Grade` jest `enum`. Znak zapytania po deklaracji typu `Grade` wskazuje, że właściwość `Grade` ma wartość null. Ma wartość null, która różni się od zera klasy korporacyjnej — wartość null oznacza, że wartość nie jest znana lub nie została jeszcze przypisana.

Właściwość `StudentID` jest kluczem obcym, a odpowiednia właściwość nawigacji jest `Student`. Jednostka `Enrollment` jest skojarzona z jedną jednostką `Student`, więc Właściwość zawiera jedną jednostkę `Student`. Jednostka `Student` różni się od właściwości nawigacji `Student.Enrollments`, która zawiera wiele jednostek `Enrollment`.

Właściwość `CourseID` jest kluczem obcym, a odpowiednia właściwość nawigacji jest `Course`. Jednostka `Enrollment` jest skojarzona z jedną jednostką `Course`.

EF Core interpretuje właściwość jako klucz obcy, jeśli ma nazwę `<navigation property name><primary key property name>`. Na przykład`StudentID` właściwość nawigacji `Student`, ponieważ klucz podstawowy jednostki `Student` jest `ID`. Właściwości klucza obcego mogą być również nazwane `<primary key property name>`. Na przykład `CourseID`, ponieważ klucz podstawowy jednostki `Course` jest `CourseID`.

### <a name="the-course-entity"></a>Jednostki kursu

![Diagram jednostek kursu](intro/_static/course-entity.png)

W folderze *modele* Utwórz *Course.cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

Właściwość `Enrollments` jest właściwością nawigacji. Jednostka `Course` może być powiązana z dowolną liczbą `Enrollment` jednostek.

Atrybut `DatabaseGenerated` umożliwia aplikacji określenie klucza podstawowego zamiast generowania bazy danych.

## <a name="scaffold-the-student-model"></a>Tworzenie szkieletu modelu dla uczniów

W tej sekcji jest szkieletu modelu dla uczniów. Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) dla modelu dla uczniów.

* Skompiluj projekt.
* Utwórz folder *strony/uczniowie* .

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *strony/uczniowie* > **Dodaj** > **nowy element szkieletowy**.
* W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.

Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :

* Z listy rozwijanej **Klasa modelu** wybierz pozycję **student (ContosoUniversity. models)** .
* W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę na **ContosoUniversity. models. SchoolContext**.
* Z listy rozwijanej **Klasa kontekstu danych** wybierz pozycję **ContosoUniversity. models. SchoolContext**
* Wybierz pozycję **Dodaj**.

![Okno dialogowe CRUD](intro/_static/s1.png)

Zobacz Tworzenie [szkieletu modelu filmu w](xref:tutorials/razor-pages/model#scaffold-the-movie-model) przypadku problemu z poprzednim krokiem.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenia, aby utworzyć szkielet modelu dla uczniów.

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

Proces szkieletu tworzonych i zmienianych następujące pliki:

### <a name="files-created"></a>Utworzone pliki

* *Strony/uczniowie* Tworzenie, usuwanie, szczegóły, edytowanie, indeksowanie.
* *Data/SchoolContext. cs*

### <a name="file-updates"></a>Aktualizacje plików

* *Startup.cs* : szczegółowe zmiany w tym pliku opisano w następnej sekcji.
* *appSettings. JSON* : dodano parametry połączenia używane do nawiązywania połączenia z lokalną bazą danych.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności

ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora. Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.

Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.

Przejrzyj metodę `ConfigureServices` w *Startup.cs*. Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) . W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .

## <a name="update-main"></a>Zaktualizuj główne

W *program.cs*zmień metodę `Main`, aby wykonać następujące czynności:

* Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.
* Wywołaj [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).
* Usuń kontekst, gdy metoda `EnsureCreated` zostanie zakończona.

Poniższy kod przedstawia zaktualizowany plik *program.cs* .

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated` zapewnia, że baza danych dla kontekstu istnieje. Jeśli istnieje, nie podjęto żadnej akcji. Jeśli nie istnieje, bazy danych i jego schematu są tworzone. `EnsureCreated` nie korzysta z migracji, aby utworzyć bazę danych. Bazy danych utworzonej przy użyciu `EnsureCreated` nie można później zaktualizować za pomocą migracji.

`EnsureCreated` jest wywoływana podczas uruchamiania aplikacji, co pozwala na następujący przepływ pracy:

* Usuwanie bazy danych.
* Zmień schemat bazy danych (na przykład Dodaj pole `EmailAddress`).
* Uruchom aplikację.
* `EnsureCreated` tworzy bazę danych z kolumną`EmailAddress`.

`EnsureCreated` jest wygodnie wczesny podczas opracowywania, gdy schemat szybko się rozwija. Później w samouczku baza danych zostanie usunięty i migracje są używane.

### <a name="test-the-app"></a>Testowanie aplikacji

Uruchom aplikację i zaakceptować zasady plików cookie. Ta aplikacja nie zachowuje informacji osobistych. Informacje o zasadach dotyczących plików cookie można znaleźć na stronie [pomocy technicznej ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).

* Wybierz link **Students (studenci** ), a następnie **Utwórz nowy**.
* Przetestuj Edytuj, szczegóły i usuwać łącza.

## <a name="examine-the-schoolcontext-db-context"></a>Badanie kontekstu SchoolContext DB

Główna klasa, która służy do koordynowania funkcji EF Core dla modelu danych jest z klasy kontekstu bazy danych. Kontekst danych pochodzi od elementu [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst danych określa, które jednostki są uwzględnione w modelu danych. W tym projekcie Klasa ma nazwę `SchoolContext`.

Zaktualizuj *SchoolContext.cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

Wyróżniony kod tworzy właściwość [nieogólnymi\<>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla każdego zestawu jednostek. W terminologii programu EF Core:

* Zwykle zestaw jednostek odnosi się do tabeli bazy danych.
* Jednostki odnosi się do wiersza w tabeli.

nie można pominąć `DbSet<Enrollment>` i `DbSet<Course>`. EF Core obejmuje te niejawnie, ponieważ jednostka `Student` odwołuje się do jednostki `Enrollment`, a jednostka `Enrollment` odwołuje się do jednostki `Course`. W tym samouczku należy zachować `DbSet<Enrollment>` i `DbSet<Course>` w `SchoolContext`.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Parametry połączenia określają [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych i jest przeznaczony do tworzenia aplikacji, a nie do użytku produkcyjnego. LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji. Domyślnie LocalDB tworzy pliki *MDF* DB w katalogu `C:/Users/<user>`.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Dodaj kod, aby zainicjować bazy danych przy użyciu danych testowych

EF Core tworzy pustą bazy danych. W tej części Metoda `Initialize` jest zapisywana w celu wypełnienia jej danymi testowymi.

W folderze *dane* Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Dodaj następujący kod:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

Uwaga: Poprzedni kod używa `Models` dla przestrzeni nazw (`namespace ContosoUniversity.Models`), a nie `Data`. `Models` jest spójna z kodem generowanym przez program rusztowaer. Aby uzyskać więcej informacji, zobacz [ten problem z szkieletem usługi GitHub](https://github.com/aspnet/Scaffolding/issues/822).

Kod sprawdza, czy wszystkie studentów w bazie danych. W przypadku nie studentów w bazie danych, baza danych jest inicjowany z danych testowych. Ładuje dane testowe do tablic, a nie `List<T>` kolekcje w celu zoptymalizowania wydajności.

Metoda `EnsureCreated` automatycznie tworzy bazę danych dla kontekstu bazy danych. Jeśli baza danych istnieje, `EnsureCreated` zwraca bez modyfikowania bazy danych.

W *program.cs*zmień metodę `Main`, aby wywołać `Initialize`:

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie uruchom następujące polecenie w **konsoli Menedżera pakietów** (PMC):

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie usuń plik *cu. DB* .

---

## <a name="view-the-db"></a>Widok bazy danych

Nazwa bazy danych jest generowana na podstawie podanej wcześniej nazwy kontekstu oraz łącznika i identyfikatora GUID. W rezultacie nazwa bazy danych będzie "SchoolContext-{GUID}". Identyfikator GUID będzie różny dla każdego użytkownika.
Otwórz **Eksplorator obiektów SQL Server** (SSOX) z menu **Widok** w programie Visual Studio.
W SSOX kliknij pozycję **(LocalDB) \MSSQLLocalDB > bazy danych > SchoolContext-{GUID}** .

Rozwiń węzeł **tabele** .

Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Wyświetl dane** , aby wyświetlić utworzone kolumny i wiersze wstawione do tabeli.

## <a name="asynchronous-code"></a>Kod asynchroniczny

Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i programem EF Core.

Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte. Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane. Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć. Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań. W rezultacie kod asynchroniczny umożliwia bardziej efektywne wykorzystanie zasobów serwera i serwera jest włączona, aby obsłużyć większy ruch, bez opóźnień.

Kod asynchroniczny wprowadzić niewielkiej ilości obciążenia w czasie wykonywania. W sytuacjach o niewielkim ruchu wpływający na wydajność jest nieistotny, podczas w sytuacjach dużego ruchu, potencjalna poprawa wydajności jest znacząca.

W poniższym kodzie słowo kluczowe [Async](/dotnet/csharp/language-reference/keywords/async) , `Task<T>` zwracanej wartości, `await` słowo kluczowe i Metoda `ToListAsync` sprawia, że kod jest wykonywany asynchronicznie.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* Słowo kluczowe `async` instruuje kompilator, aby:
  * Generuj wywołania zwrotne dla części treści metody.
  * Automatycznie Utwórz obiekt [zadania](/dotnet/api/system.threading.tasks.task) , który jest zwracany. Aby uzyskać więcej informacji, zobacz [Typ zwracany zadania](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Niejawny typ zwracany `Task` reprezentuje trwającą prace.
* Słowo kluczowe `await` powoduje, że kompilator dzieli metodę na dwie części. Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie. Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.
* `ToListAsync` jest asynchroniczną wersją metody rozszerzenia `ToList`.

Kilka rzeczy, w których trzeba pamiętać podczas pisania kodu asynchronicznego, który używa programu EF Core:

* Tylko te instrukcje, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie. Obejmuje to, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`i `SaveChangesAsync`. Nie zawiera on instrukcji, które po prostu zmieniają `IQueryable`, takie jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* Z kontekstu programu EF Core nie jest bezpieczny dla wątków: nie należy próbować wykonać wiele operacji wykonywane równolegle.
* Aby móc korzystać z zalet wydajności kod asynchroniczny, sprawdź, czy biblioteka pakietów (np. stronicowania) używają asynchronicznej, jeśli wywołują metod programu EF Core, które wysyłają zapytania do bazy danych.

Aby uzyskać więcej informacji na temat programowania asynchronicznego w programie .NET, zobacz [Omówienie asynchroniczne](/dotnet/standard/async) i [programowanie asynchroniczne z Async i await](/dotnet/csharp/programming-guide/concepts/async/).

W następnym samouczku basic CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) są badane.



## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [Dalej](xref:data/ef-rp/crud)

::: moniker-end
