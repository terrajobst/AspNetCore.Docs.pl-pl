---
title: Strony razor za pomocą platformy Entity Framework Core w programie ASP.NET Core — samouczek 1 8
author: tdykstra
description: Pokazuje, jak utworzyć aplikację strony Razor za pomocą platformy Entity Framework Core
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/22/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 6b7d2ca1cea23efd195f1ae0e0a749c6d2d9b622
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186958"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Strony razor za pomocą platformy Entity Framework Core w programie ASP.NET Core — samouczek 1 8

Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Jest to pierwsza z serii samouczków, które pokazują, jak używać rdzenia Entity Framework (EF) w aplikacji ASP.NET Core Razor Pages. Samouczki budują witrynę sieci Web dla fikcyjnej uczelni firmy Contoso. Lokacja obejmuje funkcje, takie jak przyjmowanie uczniów, tworzenie kursu i przydziały instruktora.

[Pobieranie i wyświetlanie ukończonej aplikacji.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

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

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**.
* Nadaj projektowi nazwę *ContosoUniversity*. Ważne jest, aby użyć tej dokładnej nazwy, łącznie z wielką literą, więc przestrzenie nazw są zgodne, gdy kod jest kopiowany i wklejany.
* Na liście rozwijanej wybierz pozycję **.NET Core** i **ASP.NET Core 3,0** , a następnie wybierz pozycję **aplikacja sieci Web**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* W terminalu przejdź do folderu, w którym ma zostać utworzony folder projektu.

* Uruchom następujące polecenia, aby utworzyć projekt Razor Pages i `cd` w nowym folderze projektu:

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

`ID` Właściwość zostanie przestała kolumną klucza podstawowego tabeli bazy danych, która odnosi się do tej klasy. Domyślnie program EF Core interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy. W związku z tym alternatywną automatycznie rozpoznawaną nazwą `Student` klucza podstawowego klasy jest. `StudentID`

`Enrollments` Właściwość [właściwość nawigacji](/ef/core/modeling/relationships). Właściwości nawigacji zawierają inne jednostki, które są powiązane z tą jednostką. W takim przypadku `Enrollments` Właściwość `Student` jednostki zawiera wszystkie jednostki, które są powiązane z tym uczniem. `Enrollment` Na przykład jeśli wiersz student w bazie danych ma dwa powiązane wiersze rejestracji, `Enrollments` właściwość nawigacji zawiera te dwie jednostki rejestracji. 

W bazie danych wiersz rejestracji jest powiązany z wierszem studenta, jeśli jego kolumna StudentID zawiera wartość identyfikatora studenta. Załóżmy na przykład, że wiersz student ma identyfikator = 1. Powiązane wiersze rejestracji będą mieć StudentID = 1. StudentID jest *kluczem obcym* w tabeli rejestracji. 

Właściwość jest zdefiniowana tak, `ICollection<Enrollment>` ponieważ może istnieć wiele powiązanych jednostek rejestracji. `Enrollments` Można użyć innych typów kolekcji, takich jak `List<Enrollment>` lub. `HashSet<Enrollment>` Gdy `ICollection<Enrollment>` jest używany, tworzy programu EF Core `HashSet<Enrollment>` kolekcji domyślnie.

## <a name="the-enrollment-entity"></a>Jednostki rejestracji

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

Utwórz *modele/rejestrację. cs* przy użyciu następującego kodu:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

Właściwość jest kluczem podstawowym; ta jednostka `classnameID` używa wzorca zamiast `ID` samego siebie. `EnrollmentID` Dla modelu danych produkcyjnych wybierz jeden wzorzec i użyj go spójnie. Ten samouczek używa obu tych metod, aby zilustrować, że obie działają. Używanie `ID` bez`classname` ułatwi implementowanie niektórych rodzajów zmian modelu danych.

`Grade` Właściwość `enum`. Znak zapytania po `Grade` deklaracji typu wskazuje `Grade` , że właściwość [dopuszcza wartość null](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/). Klasa, która ma wartość null, różni się od zerowej klasy&mdash;zerowej oznacza, że Klasa nie jest znana lub nie została jeszcze przypisana.

`StudentID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Student`. `Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, więc ta właściwość zawiera pojedynczy `Student` jednostki.

`CourseID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Course`. `Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.

EF Core interpretuje właściwość jako klucz obcy, jeśli jest on nazwany `<navigation property name><primary key property name>`. Na przykład`StudentID` jest kluczem obcym `Student` dla właściwości `Student` nawigacji, ponieważ klucz podstawowy jednostki to `ID`. Może również mieć nazwę właściwości klucza obcego `<primary key property name>`. Na przykład `CourseID` ponieważ `Course` jest klucz podstawowy jednostki `CourseID`.

## <a name="the-course-entity"></a>Jednostki kursu

![Diagram jednostek kursu](intro/_static/course-entity.png)

Utwórz *modele/kurs. cs* przy użyciu następującego kodu:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

`Enrollments` Właściwość jest właściwością nawigacji. A `Course` jednostki mogą być one związane z dowolną liczbę `Enrollment` jednostek.

Ten `DatabaseGenerated` atrybut umożliwia aplikacji określenie klucza podstawowego zamiast wygenerowania go przez bazę danych.

Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilatora.

## <a name="scaffold-student-pages"></a>Strony uczniów tworzenia szkieletu

W tej sekcji użyjesz narzędzia do tworzenia szkieletów ASP.NET Core do wygenerowania:

* Klasa *kontekstu* EF Core. Kontekst jest klasą główną, która koordynuje Entity Framework funkcji dla danego modelu danych. Pochodzi od `Microsoft.EntityFrameworkCore.DbContext` klasy.
* Strony Razor obsługujące operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD) dla `Student` jednostki.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Utwórz folder *uczniów* w folderze *strony* .
* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *strony/uczniowie* i wybierz polecenie **Dodaj** > **nowy element szkieletowy**.
* W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **strony Razor za pomocą programu Entity Framework (CRUD)** > **Dodaj**.
* W oknie dialogowym **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :
  * W **klasa modelu** listę rozwijaną, wybierz opcję **uczniów (ContosoUniversity.Models)** .
  * W wierszu **klasy kontekstu danych** wybierz **+** znak (plus).
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

Parametry połączenia określają [programu SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych i jest przeznaczony do tworzenia aplikacji, a nie do użytku produkcyjnego. Domyślnie LocalDB tworzy pliki *MDF* w `C:/Users/<user>` katalogu.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Zmień parametry połączenia w taki sposób, aby wskazywały plik bazy danych programu SQLite o nazwie *cu. DB*:

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a>Aktualizowanie klasy kontekstu bazy danych

Klasa główna, która koordynuje funkcje EF Core dla danego modelu danych, jest klasą kontekstu bazy danych. Kontekst pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst określa, które jednostki są uwzględnione w modelu danych. W tym projekcie nosi nazwę klasy `SchoolContext`.

Aktualizacja *SchoolContext.cs* następującym kodem:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

Wyróżniony kod tworzy [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwości dla każdego zestawu jednostek. W terminologii programu EF Core:

* Zestaw jednostek zwykle odpowiada tabeli bazy danych.
* Jednostki odnosi się do wiersza w tabeli.

Ponieważ zestaw jednostek zawiera wiele jednostek, właściwości Nieogólnymi powinny mieć nazwy w liczbie mnogiej. Ponieważ narzędzie tworzenia szkieletów utworzyło`Student` nieogólnymi, ten krok zmienia go na plural `Students`. 

Aby kod Razor Pages był zgodny z nową nazwą nieogólnymi, wprowadź globalną zmianę w całym projekcie `_context.Student` do. `_context.Students`  Istnieją 8 wystąpień.

Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilatora.

## <a name="startupcs"></a>Startup.cs

Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst bazy danych EF Core) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora. Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych jest wyświetlany w dalszej części samouczka.

Narzędzie do tworzenia szkieletów automatycznie zarejestrowało klasę kontekstu z kontenerem iniekcji zależności.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W `ConfigureServices`programie wyróżnione wiersze zostały dodane przez program do tworzenia szkieletu:

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* W `ConfigureServices`programie upewnij się, że kod dodany przez program tworzący `UseSqlite`szkielet jest wywoływany.

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu. Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.

## <a name="create-the-database"></a>Tworzenie bazy danych

Zaktualizuj *program.cs* , aby utworzyć bazę danych, jeśli nie istnieje:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

Jeśli istnieje baza danych dla kontekstu, Metoda [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) nie przyjmuje żadnej akcji. Jeśli baza danych nie istnieje, tworzy bazę danych i schemat. `EnsureCreated`umożliwia korzystanie z następującego przepływu pracy w celu obsługi zmian modelu danych:

* Usuń bazę danych. Wszystkie istniejące dane zostaną utracone.
* Zmień model danych. Na przykład Dodaj `EmailAddress` pole.
* Uruchom aplikację.
* `EnsureCreated`tworzy bazę danych z nowym schematem.

Ten przepływ pracy działa dobrze na etapie opracowywania, gdy schemat jest gwałtownie zmieniany, o ile nie trzeba zachować danych. Sytuacja jest inna, gdy dane wprowadzone do bazy danych muszą zostać zachowane. W takim przypadku należy użyć migracji.

W dalszej części tego samouczka zostanie usunięta baza danych, która została `EnsureCreated` utworzona przez program, a zamiast tego zostanie użyta migracja. Baza danych utworzona przez `EnsureCreated` program nie może zostać zaktualizowana przy użyciu migracji.

### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację.
* Wybierz **studentów** link a następnie **Utwórz nowy**.
* Przetestuj Edytuj, szczegóły i usuwać łącza.

## <a name="seed-the-database"></a>Inicjowanie bazy danych

`EnsureCreated` Metoda tworzy pustą bazę danych. Ta sekcja dodaje kod wypełniający bazę danych danymi testowymi.

Utwórz *dane/Dbinitializeer. cs* przy użyciu następującego kodu:

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  Kod sprawdza, czy w bazie danych istnieją uczniowie. Jeśli nie ma uczniów, dodaje dane testowe do bazy danych. Tworzy dane testowe w tablicach, a nie `List<T>` w celu zoptymalizowania wydajności.

* W *program.cs*Zastąp `EnsureCreated` wywołanie `DbInitializer.Initialize` wywołaniem:

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ````

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

* Otwórz **Eksplorator obiektów SQL Server** (SSOX) z **widoku** menu w programie Visual Studio.
* W SSOX wybierz pozycję **(LocalDB) \MSSQLLocalDB > bazy danych > SchoolContext-{GUID}** . Nazwa bazy danych jest generowana na podstawie podanej wcześniej nazwy kontekstu oraz łącznika i identyfikatora GUID.
* Rozwiń **tabel** węzła.
* Kliknij prawym przyciskiem myszy **uczniów** tabeli, a następnie kliknij przycisk **dane widoku** kolumn utworzonych i wiersze wstawione do tabeli.
* Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Wyświetl kod** , aby `Student` zobaczyć, jak model `Student` mapuje się do schematu tabeli.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Użyj narzędzia SQLite, aby wyświetlić schemat bazy danych i dane referencyjne. Plik bazy danych ma nazwę *cu. DB* i znajduje się w folderze projektu.

---

## <a name="asynchronous-code"></a>Kod asynchroniczny

Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i programem EF Core.

Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte. Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane. Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć. Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań. W efekcie kod asynchroniczny umożliwia efektywniejsze wykorzystanie zasobów serwera, a serwer może obsłużyć więcej ruchu bez opóźnień.

Kod asynchroniczny wprowadzić niewielkiej ilości obciążenia w czasie wykonywania. W sytuacjach o niewielkim ruchu wpływający na wydajność jest nieistotny, podczas w sytuacjach dużego ruchu, potencjalna poprawa wydajności jest znacząca.

W poniższym kodzie [async](/dotnet/csharp/language-reference/keywords/async) — słowo kluczowe, `Task<T>` zwracają wartość, `await` — słowo kluczowe, i `ToListAsync` metoda powoduje, że kod, są wykonywane asynchronicznie.

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* `async` — Słowo kluczowe informuje kompilator, aby:
  * Generuj wywołania zwrotne dla części treści metody.
  * Utwórz obiekt [zadania](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) , który jest zwracany.
* Typ `Task<T>` zwracany reprezentuje bieżącą liczbę zadań.
* `await` — Słowo kluczowe powoduje, że kompilator podzielić metodę na dwie części. Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie. Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.
* `ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.

Kilka rzeczy, w których trzeba pamiętać podczas pisania kodu asynchronicznego, który używa programu EF Core:

* Tylko instrukcje, które powodują, że zapytania lub polecenia wysyłane do bazy danych są wykonywane asynchronicznie. Obejmuje `ToListAsync`, `SingleOrDefaultAsync`, i`FirstOrDefaultAsync`. `SaveChangesAsync` Nie zawiera instrukcji, które można zmienić `IQueryable`, takich jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* Z kontekstu programu EF Core nie jest bezpieczny dla wątków: nie należy próbować wykonać wiele operacji wykonywane równolegle.
* Aby skorzystać z zalet wydajności w kodzie asynchronicznym, należy sprawdzić, czy pakiety biblioteki (na przykład stronicowania) używają wartości asynchronicznej, jeśli są wywoływane EF Core metod wysyłających zapytania do bazy danych.

Aby uzyskać więcej informacji na temat programowania asynchronicznego w .NET, zobacz [Przegląd Async](/dotnet/standard/async) i [programowanie asynchroniczne z async i await](/dotnet/csharp/programming-guide/concepts/async/).

## <a name="next-steps"></a>Następne kroki

> [!div class="step-by-step"]
> [Następny samouczek](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Przykładową aplikację sieci web firmy Contoso University pokazuje, jak utworzyć aplikację stron Razor programu ASP.NET Core przy użyciu Core Entity Framework (EF).

Przykładowa aplikacja jest witryną sieci web dla uniwersytetu fikcyjnej firmy Contoso. Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora. Ta strona jest to pierwszy z serii samouczków, które opisują sposób tworzenia przykładowej aplikacji Contoso University.

[Pobieranie i wyświetlanie ukończonej aplikacji.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

Znajomość [stron Razor](xref:razor-pages/index). Należy wykonać początkujących programistów [Rozpoczynanie pracy ze stronami Razor](xref:tutorials/razor-pages/razor-pages-start) przed rozpoczęciem tej serii.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli napotkasz problem, nie można rozpoznać ogólnie można znaleźć rozwiązania, porównując swój kod, aby [projektu ukończona](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Dobrym sposobem, aby uzyskać pomoc polega na publikowanie pytań do [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) dla [platformy ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [programu EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

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

Obrazy te czynności, zobacz [tworzenie aplikacji sieci web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).
Uruchom aplikację.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

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

`Enrollments` Właściwość [właściwość nawigacji](/ef/core/modeling/relationships). Właściwości nawigacji link do innych jednostek, które są powiązane z tej jednostki. W tym przypadku `Enrollments` właściwość `Student entity` przechowuje wszystkie `Enrollment` jednostek, które są powiązane z tym, które `Student`. Na przykład, jeśli wiersz dla uczniów w bazy danych ma dwa powiązane wiersze rejestracji `Enrollments` właściwość nawigacji zawiera tych dwóch `Enrollment` jednostek. Powiązane `Enrollment` wiersz jest wierszem, który zawiera ten uczniów wartość klucza podstawowego w `StudentID` kolumny. Na przykład, załóżmy, że dla uczniów o identyfikatorze = 1 ma dwa wiersze `Enrollment` tabeli. `Enrollment` Tabela ma dwa wiersze z `StudentID` = 1. `StudentID` to klucz obcy w `Enrollment` tabela, która określa uczniów w `Student` tabeli.

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

* W **klasa modelu** listę rozwijaną, wybierz opcję **uczniów (ContosoUniversity.Models)** .
* W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i Zmień nazwę wygenerowanego na **ContosoUniversity.Models.SchoolContext**.
* W **klasa kontekstu danych** listę rozwijaną, wybierz opcję **ContosoUniversity.Models.SchoolContext**
* Wybierz pozycję **Dodaj**.

![Okno dialogowe CRUD](intro/_static/s1.png)

Zobacz [tworzenia szkieletu modelu movie](xref:tutorials/razor-pages/model#scaffold-the-movie-model) Jeśli masz problem z poprzedniego kroku.

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

* *Strony/uczniów* tworzenie, edytowanie usuwania, uzyskać szczegółowe informacje, indeksu.
* *Data/SchoolContext.cs*

### <a name="file-updates"></a>Aktualizacje plików

* *Startup.cs* : Zmiany w tym pliku opisano szczegółowo w następnej sekcji.
* *appSettings. JSON* : Dodano parametry połączenia używane do nawiązania połączenia z lokalną bazą danych.

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

Uwaga: Poprzedzający kod używa `Models` dla przestrzeni nazw (`namespace ContosoUniversity.Models`), a `Data`nie. `Models`jest spójny z kodem generowanym przez program rusztowaer. Aby uzyskać więcej informacji, zobacz [ten problem z szkieletem usługi GitHub](https://github.com/aspnet/Scaffolding/issues/822).

Kod sprawdza, czy wszystkie studentów w bazie danych. W przypadku nie studentów w bazie danych, baza danych jest inicjowany z danych testowych. Ładuje dane testowe do tablic zamiast `List<T>` kolekcje w celu zoptymalizowania wydajności.

`EnsureCreated` Metoda automatycznie tworzy bazy danych, aby uzyskać kontekst bazy danych. Jeśli baza danych istnieje, `EnsureCreated` zwraca bez modyfikowania bazy danych.

W *Program.cs*, zmodyfikuj `Main` metodę do wywołania `Initialize`:

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
Otwórz **Eksplorator obiektów SQL Server** (SSOX) z **widoku** menu w programie Visual Studio.
W SSOX kliknij pozycję **(LocalDB) \MSSQLLocalDB > bazy danych > SchoolContext-{GUID}** .

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

Aby uzyskać więcej informacji na temat programowania asynchronicznego w .NET, zobacz [Przegląd Async](/dotnet/standard/async) i [programowanie asynchroniczne z async i await](/dotnet/csharp/programming-guide/concepts/async/).

W następnym samouczku basic CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) są badane.



## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [Next](xref:data/ef-rp/crud)

::: moniker-end
