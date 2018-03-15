---
title: Stron razor z Entity Framework Core w platformy ASP.NET Core - 1 samouczka 8
author: rick-anderson
description: "Pokazuje, jak utworzyć aplikację stron Razor przy użyciu programu Entity Framework Core"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 1b0fdb9be83530323f2dc7e3bcb26df26c597c1b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Stron razor z Entity Framework Core w platformy ASP.NET Core - 1 samouczka 8

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core 2.0 MVC za pomocą Core Entity Framework (EF) 2.0 i programu Visual Studio 2017 r.

Przykładowa aplikacja jest witryną sieci web dla fikcyjnej uniwersytetu Contoso. Obejmuje funkcje, takie jak wprowadzenia studentów, tworzenie kursu i instruktora przypisania. Ta strona jest to pierwszy z serii samouczków, które opisują sposób kompilowania aplikacji przykładowej Contoso University.

[Pobrania lub wyświetlenia ukończonej aplikacji.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

Znajomość [stron Razor](xref:mvc/razor-pages/index). Nowe programistów powinno zakończyć się [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start) przed uruchomieniem tej serii.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli napotkasz problem, nie można rozpoznać zwykle można znaleźć rozwiązania na podstawie porównania ilości kodu do [ukończone etap](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots). Aby uzyskać listę typowych błędów i sposobów ich rozwiązania, zobacz [sekcji rozwiązywania problemów ostatniego samouczek z tej serii](xref:data/ef-mvc/advanced#common-errors). Jeśli nie możesz znaleźć, muszą, możesz wysłać zapytanie do [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) dla [platformy ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Tej serii samouczków opiera się na czynności wykonane w starszych samouczki. Warto zapisać kopię projektu po każdym pomyślnym ukończeniu samouczka. Jeśli wystąpiły problemy, możesz zacząć od nowa z poprzednich samouczek zamiast po powrocie do początku. Alternatywnie możesz pobrać [ukończone etap](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) i zacząć od nowa przy użyciu etap ukończone.

## <a name="the-contoso-university-web-app"></a>Aplikacja sieci web firmy Contoso University

Aplikacji utworzony w tych samouczkach jest witryną sieci web university podstawowe.

Użytkownicy mogą wyświetlać i uczniów, kursu i informacje o wykładowcach aktualizacji. Poniżej przedstawiono kilka ekranów utworzone w samouczku.

![Strona indeksu uczniów lub studentów](intro/_static/students-index.png)

![Strona Edytuj uczniów lub studentów](intro/_static/student-edit.png)

Styl interfejsu użytkownika w tej lokacji jest bliski co to jest generowany przez wbudowane szablony. Samouczek koncentruje się na podstawowych EF Razor strony, nie interfejsu użytkownika.

## <a name="create-a-razor-pages-web-app"></a>Tworzenie aplikacji sieci web Razor strony

* W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.
* Utwórz nową aplikację sieci Web platformy ASP.NET Core. Nazwij projekt **ContosoUniversity**. Ważne jest, aby nazwa projektu *ContosoUniversity* tak dopasuj przestrzenie nazw, jeśli kod jest skopiowane i wklejone.
 ![nową aplikację sieci Web Core ASP.NET](intro/_static/np.png)
* Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.
 ![Aplikacja sieci Web (Razor strony)](../../mvc/razor-pages/index/_static/np2.png)

Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** działać bez dołączanie debugera

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka zmian ustawienia lokacji menu, układu i strony głównej.

Otwórz *Pages/_Layout.cshtml* i wprowadź następujące zmiany:

* Zmienić każde wystąpienie "ContosoUniversity" na "University firmy Contoso". Istnieją trzy wystąpienia.

* Dodaj elementy menu dla **studentów**, **kursów**, **instruktorów**, i **działów**i Usuń **skontaktuj się z** wpisu menu.

Zmiany zostały wyróżnione. (Kod znaczników jest *nie* wyświetlane.)

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

W *Pages/Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zastąpić tekst o ASP.NET i MVC tekst o tej aplikacji:

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Naciśnij klawisze CTRL + F5, aby uruchomić projekt. Strona główna jest wyświetlana z kartami utworzone w następujące samouczki:

![Strona główna University firmy Contoso](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Tworzenie klas jednostek dla aplikacji Contoso University. Rozpocząć następujące trzy jednostki:

![Diagram modelu danych kursu-rejestracji — dla użytkowników domowych](intro/_static/data-model-diagram.png)

Brak relacji jeden do wielu między `Student` i `Enrollment` jednostek. Brak relacji jeden do wielu między `Course` i `Enrollment` jednostek. Gdy uczniowie mogą rejestrować się w dowolnej liczby kursów. Kursu może mieć dowolną liczbę studentów zarejestrowane w nim.

W poniższych sekcjach utworzeniu klasy dla każdego z tych obiektów.

### <a name="the-student-entity"></a>Jednostki dla użytkowników domowych

![Diagram jednostek dla użytkowników domowych](intro/_static/student-entity.png)

Utwórz *modele* folderu. W *modele* folderu, Utwórz plik klasy o nazwie *Student.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` Właściwości staje się kolumna klucza podstawowego tabeli bazy danych (bazy danych), która odnosi się do tej klasy. Domyślnie EF rdzenie będą interpretowane przez właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.

`Enrollments` Właściwość jest właściwością nawigacji. Właściwości nawigacji, połącz się z innymi obiektami, które są powiązane z tą jednostką. W takim przypadku `Enrollments` właściwość `Student entity` przechowuje wszystkie `Enrollment` jednostek, które są powiązane z których `Student`. Na przykład, jeśli wiersz uczniów w bazie danych ma dwa powiązane wiersze rejestracji `Enrollments` właściwość nawigacji zawiera tych dwóch `Enrollment` jednostek. Powiązane `Enrollment` wiersz jest wierszem zawiera Studenta dla tej wartości klucza podstawowego w `StudentID` kolumny. Na przykład, załóżmy, że uczniów o identyfikatorze = 1 ma dwa wiersze `Enrollment` tabeli. `Enrollment` Tabela zawiera dwa wiersze z `StudentID` = 1. `StudentID` jest to kolumna klucza obcego w `Enrollment` tabeli określający student w `Student` tabeli.

Jeśli właściwość nawigacji może zawierać wiele jednostek, właściwość nawigacji musi być typem listy, takich jak `ICollection<T>`. `ICollection<T>` można określić, takie jak typ lub `List<T>` lub `HashSet<T>`. Gdy `ICollection<T>` jest używana, tworzy EF Core `HashSet<T>` kolekcji domyślnie. Właściwości nawigacji, które zawierają wiele jednostek pochodzą z relacji wiele do wielu oraz jeden do wielu.

### <a name="the-enrollment-entity"></a>Jednostka rejestracji

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

W *modele* folderu, Utwórz *Enrollment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Właściwość jest kluczem podstawowym. Używa tej jednostki `classnameID` wzorca zamiast `ID` jak `Student` jednostki. Zwykle programiści wybierz jeden wzorzec i używać go w modelu danych. W samouczku nowsze za pomocą Identyfikatora bez classname przedstawiono ułatwiające wdrażanie dziedziczenia w modelu danych.

`Grade` Jest właściwość `enum`. Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość dopuszcza wartość null. Ma wartość null, która różni się od zera klasy — wartość null oznacza, że klasa nie jest znana lub nie została jeszcze przypisana.

`StudentID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Student`. `Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, więc ta właściwość zawiera jeden `Student` jednostki. `Student` Jednostki różni się od `Student.Enrollments` właściwość nawigacji, która zawiera wiele `Enrollment` jednostek.

`CourseID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Course`. `Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.

EF Core interpretuje właściwość jako klucz obcy, jeśli jest o nazwie `<navigation property name><primary key property name>`. Na przykład`StudentID` dla `Student` właściwość nawigacji, ponieważ `Student` klucza podstawowego jednostki jest `ID`. Właściwości klucza obcego może również być nazwany `<primary key property name>`. Na przykład `CourseID` ponieważ `Course` klucza podstawowego jednostki jest `CourseID`.

### <a name="the-course-entity"></a>Jednostki ciągu

![Diagram jednostek kursu](intro/_static/course-entity.png)

W *modele* folderu, Utwórz *Course.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Właściwość jest właściwością nawigacji. A `Course` jednostka może być powiązane z dowolną liczbę `Enrollment` jednostek.

`DatabaseGenerated` Atrybutu umożliwia aplikacji określenie klucza podstawowego, a nie bazy danych o jego wygenerowaniu.

## <a name="create-the-schoolcontext-db-context"></a>Tworzenie kontekstu SchoolContext DB

Klasy głównym, która koordynuje EF podstawową funkcjonalność o dany model danych jest klasa kontekstu bazy danych. Kontekst danych jest określana na podstawie `Microsoft.EntityFrameworkCore.DbContext`. Kontekst danych określa jednostki, które znajdują się w modelu danych. W tym projekcie klasy o nazwie `SchoolContext`.

W folderze projektu Utwórz folder o nazwie *danych*.

W *danych* Utwórz folder *SchoolContext.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Ten kod tworzy `DbSet` właściwości dla każdego zestawu jednostek. W terminologii EF rdzeni:

* Zwykle zestawu jednostek odnosi się do tabeli bazy danych.
* Jednostka odpowiada wiersza w tabeli.

`DbSet<Enrollment>` i `DbSet<Course>` można pominąć. Podstawowe EF są uwzględniane niejawnie ponieważ `Student` odwołań do jednostek `Enrollment` jednostki i `Enrollment` odwołań do jednostek `Course` jednostki. W tym samouczku, Zachowaj `DbSet<Enrollment>` i `DbSet<Course>` w `SchoolContext`.

Po utworzeniu bazy danych EF Core tworzy tabele, które mają taki sam, jak nazwy `DbSet` nazwy właściwości. Nazwy właściwości w kolekcji są zwykle w liczbie mnogiej (studentów zamiast uczniów). Deweloperzy nie zgadzają się o czy nazwy tabeli powinien być w liczbie mnogiej. Te samouczki domyślne zachowanie jest zastępowany przez określenie nazw pojedynczej tabeli w DbContext. Do określenia nazwy w liczbie pojedynczej tabeli, Dodaj następujący wyróżniony kod:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Zarejestruj kontekście iniekcji zależności

Obejmuje platformy ASP.NET Core [iniekcji zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst danych podstawowych EF) są zarejestrowane w usłudze iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (takich jak strony Razor) są udostępniane tych usług za pomocą parametrów konstruktora. Kod konstruktora, który pobiera wystąpienia kontekstu db przedstawiono w dalszej części tego samouczka.

Aby zarejestrować `SchoolContext` jako usługa, otwórz *Startup.cs*i Dodaj wyróżnione wiersze do `ConfigureServices` metody.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody `DbContextOptionsBuilder` obiektu. Dla wdrożenia lokalnego [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.

Dodaj `using` instrukcje dla `ContosoUniversity.Data` i `Microsoft.EntityFrameworkCore` przestrzeni nazw. Skompiluj projekt.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Otwórz *appsettings.json* i dodaj ciąg połączenia, jak pokazano w poniższym kodzie:

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

Poprzednie parametry połączenia używane `ConnectRetryCount=0` zapobiegające [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) z wiszące.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB.

Parametry połączenia wskazują bazy danych LocalDB programu SQL Server. LocalDB jest wersja programu SQL Server Express aparatu bazy danych i jest przeznaczony do tworzenia aplikacji, a nie do użytku produkcyjnego. LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonych konfiguracji. Domyślnie tworzy LocalDB *.mdf* pliki bazy danych w `C:/Users/<user>` katalogu.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Dodaj kod, aby zainicjować bazy danych z danych testowych

Podstawowe EF tworzy puste bazy danych. W tej sekcji *inicjatora* zapisywana jest metoda wypełnić je danymi testu.

W *danych* folderu, Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Dodaj następujący kod:

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Kod sprawdza, czy wszystkie studentów w bazie danych. Jeśli w bazie danych nie nie studentów, bazy danych jest obsługiwany z danych testowych. Ładuje dane testowe do tablic zamiast `List<T>` kolekcje w celu optymalizacji wydajności.

`EnsureCreated` Metoda automatycznie tworzy bazy danych dla kontekst bazy danych. Jeśli baza danych istnieje, `EnsureCreated` zwraca bez modyfikowania bazy danych.

W *Program.cs*, zmodyfikuj `Main` metody wykonać następujące czynności:

* Pobierz wystąpienia kontekstu DB z kontenera iniekcji zależności.
* Wywołaj metodę inicjatora, przekazanie jej w kontekście.
* Po zakończeniu metody inicjatora, należy dysponować kontekstu.

Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

Podczas pierwszego uruchomienia aplikacji bazy danych jest utworzony i rozpoczęta z danych testowych. Po zaktualizowaniu modelu danych:
* Usuń bazę danych.
* Metoda inicjatora aktualizacji.
* Uruchom aplikację i utworzeniu nowego wprowadzonych bazy danych. 

W kolejnych samouczkach bazy danych jest aktualizowany, gdy dane modelu zmiany, bez usunięcia i ponownego utworzenia bazy danych.

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>Dodawanie szkieletu narzędzi

W tej sekcji konsoli Menedżera pakietów (PMC) służy do dodawania pakietu generowania kodu sieci web programu Visual Studio. Ten pakiet jest wymagana do uruchamiania aparatu szkieletów.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenia:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

Poprzednie polecenie dodaje pakietów NuGet do pliku *.csproj:

[!code-csharp[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>Tworzenie szkieletu modelu

* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* Uruchom następujące polecenia:


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

Jeśli zostanie wyświetlony błąd:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).


Skompiluj projekt. Kompilacja generuje błędy podobne do następujących:

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 Globalnie zmienić `_context.Student` do `_context.Students` (to znaczy dodania "s" do `Student`). 7 wystąpienia są odnaleźć i zaktualizować. Mamy nadzieję naprawić [tej usterki](https://github.com/aspnet/Scaffolding/issues/633) w następnej wersji.

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>Testowanie aplikacji

Uruchom aplikację i wybierz **studentów** łącza. W zależności od szerokości przeglądarki **studentów** łącza wyświetlany w górnej części strony. Jeśli **studentów** łącze nie jest widoczny, kliknij ikonę nawigacji w prawym górnym rogu.

![Strona główna contoso University wąskie](intro/_static/home-page-narrow.png)

Test **Utwórz**, **Edytuj**, i **szczegóły** łącza.

## <a name="view-the-db"></a>Widok bazy danych

Po uruchomieniu aplikacji `DbInitializer.Initialize` wywołania `EnsureCreated`. `EnsureCreated` wykrywa, czy istnieje bazę danych i tworzy jeden, jeśli to konieczne. Jeśli w bazie danych, nie nie studentów `Initialize` studentów dodaje metody.

Otwórz **Eksplorator obiektów SQL Server** (SSOX) z **widoku** menu w programie Visual Studio.
W SSOX, kliknij przycisk **(localdb) \MSSQLLocalDB > baz danych > ContosoUniversity1**.

Rozwiń węzeł **tabel** węzła.

Kliknij prawym przyciskiem myszy **uczniowie** tabeli, a następnie kliknij przycisk **danych widoku** kolumn utworzona i wiersze wstawione do tabeli.

*.Mdf* i *ldf* pliki bazy danych znajdują się w *C:\Users\\ <yourusername>*  folderu.

`EnsureCreated` jest wywoływana po uruchomieniu aplikacji, dzięki czemu następującego przepływu pracy:

* Usuń bazę danych.
* Zmień schemat bazy danych (na przykład dodać `EmailAddress` pól).
* Uruchom aplikację.

`EnsureCreated` Tworzy baza danych o`EmailAddress` kolumny.

## <a name="conventions"></a>Konwencje

Ilość kodu napisanego w kolejności EF podstawowych utworzyć pełną bazy danych jest minimalny, z powodu użycia konwencje lub założenia, które wprowadza EF Core.

* Nazwy `DbSet` właściwości są używane jako nazwy tabeli. Dla jednostek nie odwołuje się `DbSet` właściwość klasy jednostka nazw są używane jako nazwy tabeli.

* Nazwy właściwości jednostki są używane dla nazw kolumn.

* Właściwości jednostki, które są nazywane Identyfikatora lub classnameID są rozpoznawane jako właściwości klucza podstawowego.

* Właściwość jest interpretowana jako właściwości klucza obcego, jeśli jest o nazwie  *<navigation property name> <primary key property name>*  (na przykład `StudentID` dla `Student` właściwość nawigacji, ponieważ `Student` jest klucza podstawowego jednostki `ID`). Właściwości klucza obcego może mieć nazwę  *<primary key property name>*  (na przykład `EnrollmentID` ponieważ `Enrollment` klucza podstawowego jednostki jest `EnrollmentID`).

Konwencjonalne zachowanie można przesłonić. Na przykład nazwy tabeli można jawnie określić, jak pokazano wcześniej w tym samouczku. Nazwy kolumn można ustawić jawnie. Klucze podstawowe i klucze obce można ustawić jawnie.

## <a name="asynchronous-code"></a>Kod asynchroniczny

Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i EF Core.

Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach wysokie obciążenie wszystkich dostępnych wątków może być używana. W takim przypadku serwer nie może przetworzyć nowe żądania aż wątki są zwalniane. Z kodem synchroniczne wiele wątków może powiązany, gdy nie są faktycznie wykonują pracę, ponieważ ich oczekiwania operacji We/Wy zakończyć. Asynchroniczne kodu podczas procesu jest oczekiwania operacji We/Wy zakończyć, jego wątku zostanie zwolniona dla serwera na potrzeby przetwarzania innych żądań. W związku z tym kod asynchroniczny umożliwia bardziej efektywne wykorzystanie zasobów serwera, a serwer jest skonfigurowany do obsługi ruchu więcej bez opóźnień.

Asynchroniczne kodu wprowadzenie niewielkiej liczby dodatkowych czynności w czasie wykonywania. W sytuacjach niski ruchu trafień wydajności jest niewielka, podczas w sytuacjach dużego natężenia ruchu sieciowego, potencjalne zwiększenie wydajności jest znacząca.

W poniższym kodzie `async` — słowo kluczowe, `Task<T>` zwrócić wartość, `await` — słowo kluczowe, i `ToListAsync` metoda powoduje, że kod asynchroniczne.

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` — Słowo kluczowe informuje kompilator, aby:

  * Generuj wywołania zwrotne dla części treści metody.
  * Automatyczne tworzenie [zadań](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) obiekt, który jest zwracany. Aby uzyskać więcej informacji, zobacz [typu zwracanego zadań](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Typ zwracany niejawne `Task` reprezentuje pracy w toku.

* `await` — Słowo kluczowe powoduje, że kompilator podzielić na dwie części metodę. Pierwsza część kończy operację, który jest uruchamiany asynchronicznie. Druga część są umieszczane w metodę wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.

* `ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.

Należy pamiętać o podczas pisania kodu asynchroniczne EF Core używa w kilku kwestiach:

* Tylko te instrukcje, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie. Zawierającej, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, i `SaveChangesAsync`. Nie zawiera instrukcji, które można zmienić `IQueryable`, takich jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Kontekst EF Core nie jest bezpieczne dla wątków: nie należy próbować wykonać kilka operacji wykonywane równolegle. 

* Można wykorzystać zalety wydajności async kodu, sprawdź, czy pakiety bibliotekę (takie jak w przypadku stronicowania) używają async Jeśli wywołują metody EF podstawowych, które wysyłanie zapytań do bazy danych.

Aby uzyskać więcej informacji na temat programowania asynchronicznego w programie .NET, zobacz [omówienie Async](https://docs.microsoft.com/dotnet/articles/standard/async).

W następnym samouczku basic CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) operacje są sprawdzane.

>[!div class="step-by-step"]
[Next](xref:data/ef-rp/crud)
