---
title: Platformy ASP.NET Core MVC z Entity Framework Core - 1 samouczka 10
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/intro
ms.openlocfilehash: 3c418cc4e331ad19b0ec1be3207fa2cc44bef041
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275674"
---
# <a name="aspnet-core-mvc-with-entity-framework-core---tutorial-1-of-10"></a>Platformy ASP.NET Core MVC z Entity Framework Core - 1 samouczka 10

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [RP better than MVC](../../includes/RP-EF/rp-over-mvc.md)]

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core 2.0 MVC za pomocą Core Entity Framework (EF) 2.0 i programu Visual Studio 2017 r.

Przykładowa aplikacja jest witryną sieci web dla fikcyjnej uniwersytetu Contoso. Obejmuje funkcje, takie jak wprowadzenia studentów, tworzenie kursu i instruktora przypisania. Jest to pierwszy z serii samouczków, w których opisano sposób tworzenia przykładowej aplikacji Contoso University od początku.

[Pobrania lub wyświetlenia ukończona aplikacja.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

Podstawowe EF 2.0 jest najnowsza wersja EF jeszcze nie wszystkich funkcji EF 6.x. Informacje o tym, jak wybrać EF 6.x i podstawowe EF, zobacz [EF podstawowe programu vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/). Jeśli wybierzesz EF 6.x, zobacz [we wcześniejszej wersji tego samouczka serii](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

> [!NOTE]
> Wersja platformy ASP.NET Core 1.1 tego samouczka, dla [wersji programu VS 2017 Update 2 tego samouczka w formacie PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli napotkasz problem, nie można rozpoznać zwykle można znaleźć rozwiązania na podstawie porównania ilości kodu do [projektu zakończone](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final). Aby uzyskać listę typowych błędów i sposobów ich rozwiązania, zobacz [sekcji rozwiązywania problemów ostatniego samouczek z tej serii](advanced.md#common-errors). Nie można znaleźć, muszą, możesz wysłać zapytanie do StackOverflow.com dla [platformy ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Jest to szereg 10 samouczków, z których każdy opiera się na czynności wykonane w starszych samouczki. Warto zapisać kopię projektu po każdym pomyślnym ukończeniu samouczka. Następnie w razie problemów, możesz zacząć od nowa z poprzednich samouczek zamiast po powrocie do początku całej serii.

## <a name="the-contoso-university-web-application"></a>Aplikacja sieci web University firmy Contoso

Aplikację, która będzie układać w tych samouczkach jest witryną sieci web university proste.

Użytkownicy mogą wyświetlać i uczniów, kursu i informacje o wykładowcach aktualizacji. Poniżej przedstawiono niektóre ekrany, które zostaną utworzone.

![Strona indeksu uczniów lub studentów](intro/_static/students-index.png)

![Strona Edytuj uczniów lub studentów](intro/_static/student-edit.png)

Styl interfejsu użytkownika w tej lokacji została zachowana bliski co to jest generowany przez wbudowane szablony, dzięki czemu samouczka można skupić się głównie na temat korzystania z programu Entity Framework.

## <a name="create-an-aspnet-core-mvc-web-application"></a>Tworzenie aplikacji sieci web platformy ASP.NET Core MVC

Otwórz program Visual Studio i utworzyć nowy program ASP.NET Core C# projektu sieci web o nazwie "ContosoUniversity".

* Z **pliku** menu, wybierz opcję **nowy > Projekt**.

* W okienku po lewej stronie wybierz **zainstalowana > Visual C# > sieci Web**.

* Wybierz **aplikacji sieci Web platformy ASP.NET Core** szablonu projektu.

* Wprowadź **ContosoUniversity** jako nazwy i kliknij przycisk **OK**.

  ![Okno dialogowe nowego projektu](intro/_static/new-project.png)

* Poczekaj, aż **nowe podstawowe aplikacji sieci Web ASP.NET (.NET Core)** wyświetlać okno dialogowe

* Wybierz **ASP.NET Core 2.0** i **aplikacji sieci Web (Model-View-Controller)** szablonu.

  **Uwaga:** ten samouczek wymaga programu ASP.NET 2.0 Core i Core EF 2.0 lub nowszej — upewnij się, że **ASP.NET Core 1.1** nie jest zaznaczone.

* Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**.

* Kliknij przycisk **OK**

  ![Okno dialogowe nowego projektu platformy ASP.NET](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka prostych zmian zostaną skonfigurowane w lokacji menu, układu i strony głównej.

Otwórz *Views/Shared/_Layout.cshtml* i wprowadź następujące zmiany:

* Zmienić każde wystąpienie "ContosoUniversity" na "Contoso University". Istnieją trzy wystąpienia.

* Dodaj elementy menu dla **studentów**, **kursów**, **instruktorów**, i **działów**i Usuń **skontaktuj się z** wpisu menu.

Zmiany zostały wyróżnione.

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

W *Views/Home/Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zastąpić tekst o ASP.NET i MVC tekst o tej aplikacji:

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz **Debuguj > Uruchom bez debugowania** z menu. Zostanie wyświetlona strona główna z kartami dla stron, które zostaną utworzone w tych samouczkach.

![Strona główna University firmy Contoso](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a>Pakiety Framework Core NuGet jednostki

Aby dodać obsługę EF Core w projekcie, zainstalować dostawcę bazy danych, który ma być docelowa. Ten samouczek używa programu SQL Server, a pakiet dostawcy jest [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/). Ten pakiet jest uwzględniona w [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, dzięki czemu nie trzeba go zainstalować.

Ten pakiet i jego zależności (`Microsoft.EntityFrameworkCore` i `Microsoft.EntityFrameworkCore.Relational`) zapewniają obsługę środowiska uruchomieniowego dla EF. Dodasz pakiet narzędzi w dalszej [migracje](migrations.md) samouczka. 

Aby uzyskać informacje o innych dostawców bazy danych, które są dostępne dla programu Entity Framework Core, zobacz [bazy danych dostawców](https://docs.microsoft.com/ef/core/providers/).

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Następnie utworzysz klas jednostek dla aplikacji Contoso University. Będzie rozpoczynać następujące trzy jednostki.

![Diagram modelu danych kursu-rejestracji — dla użytkowników domowych](intro/_static/data-model-diagram.png)

Brak relacji jeden do wielu między `Student` i `Enrollment` jednostek, i ma relacji jeden do wielu między `Course` i `Enrollment` jednostek. Innymi słowy student mogą być rejestrowane w dowolnej liczbie kursy i kursu może mieć dowolną liczbę studentów zarejestrowane w nim.

W poniższych sekcjach zostaną utworzone klasy dla każdego z tych obiektów.

### <a name="the-student-entity"></a>Jednostki dla użytkowników domowych

![Diagram jednostek dla użytkowników domowych](intro/_static/student-entity.png)

W *modele* folderu, Utwórz plik klasy o nazwie *Student.cs* i Zastąp kod szablonu z następującym kodem.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` Właściwość staną się kolumna klucza podstawowego tabeli bazy danych, która odnosi się do tej klasy. Domyślnie program Entity Framework interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.

`Enrollments` Właściwość jest właściwością nawigacji. Właściwości nawigacji zawiera inne jednostki, które są powiązane z tą jednostką. W takim przypadku `Enrollments` właściwość `Student entity` będą przechowywane wszystkie `Enrollment` jednostek, które są powiązane z których `Student` jednostki. Innymi słowy, jeśli dany wiersz uczniów w bazie danych ma dwa powiązane rejestracji wierszy (wiersze, które zawierają Studenta dla tej wartości klucza podstawowego w kolumnie klucza obcego ich StudentID), który `Student` jednostki `Enrollments` właściwość nawigacji będzie zawierał te dwa `Enrollment` jednostek.

Jeśli właściwość nawigacji może zawierać wiele jednostek (jak relacje wiele do wielu lub jeden do wielu), w którym wpisów można można dodane, usunięte i zaktualizowane, takie jak listy musi być typu `ICollection<T>`. Można określić `ICollection<T>` lub typu, takich jak `List<T>` lub `HashSet<T>`. Jeśli określisz `ICollection<T>`, tworzy EF `HashSet<T>` kolekcji domyślnie.

### <a name="the-enrollment-entity"></a>Jednostka rejestracji

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

W *modele* folderu, Utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Właściwość będzie klucza podstawowego; używa tej jednostki `classnameID` wzorca zamiast `ID` sam jak opisany w `Student` jednostki. Zwykle będzie wybierz jeden wzorzec i używać go w modelu danych. W tym miejscu odmiany przedstawiono służy albo wzorzec. W [nowsze samouczek](inheritance.md), zobaczysz, jak za pomocą Identyfikatora bez classname ułatwia wdrażanie dziedziczenia w modelu danych.

`Grade` Jest właściwość `enum`. Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość dopuszcza wartość null. Ma wartość null, która różni się od zera klasy — wartość null oznacza, że klasa nie jest znana lub nie została jeszcze przypisana.

`StudentID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Student`. `Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, więc właściwości może zawierać tylko jeden `Student` jednostki (w przeciwieństwie do `Student.Enrollments` właściwość nawigacji był wyświetlany poprzednio, która zawiera wiele `Enrollment` jednostek).

`CourseID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Course`. `Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.

Entity Framework interpretuje właściwości jako właściwość klucza obcego, jeśli jest o nazwie `<navigation property name><primary key property name>` (na przykład `StudentID` dla `Student` właściwość nawigacji, ponieważ `Student` klucza podstawowego jednostki jest `ID`). Właściwości klucza obcego może także po prostu nazwę `<primary key property name>` (na przykład `CourseID` ponieważ `Course` klucza podstawowego jednostki jest `CourseID`).

### <a name="the-course-entity"></a>Jednostki ciągu

![Diagram jednostek kursu](intro/_static/course-entity.png)

W *modele* folderu, Utwórz *Course.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Właściwość jest właściwością nawigacji. A `Course` jednostka może być powiązane z dowolną liczbę `Enrollment` jednostek.

Będzie mówi się więcej na temat `DatabaseGenerated` atrybutu w [nowsze samouczek](complex-data-model.md) w tej serii. Zasadniczo ten atrybut umożliwia wprowadzenie klucza podstawowego dla porach zamiast generować go bazy danych.

## <a name="create-the-database-context"></a>Tworzenie kontekstu bazy danych

Klasy głównym, która koordynuje funkcji programu Entity Framework o dany model danych jest klasa kontekstu bazy danych. Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy. W kodzie należy określić jednostki, które znajdują się w modelu danych. Można również dostosować określone zachowanie programu Entity Framework. W tym projekcie klasy o nazwie `SchoolContext`.

W folderze projektu Utwórz folder o nazwie *danych*.

W *danych* folderze utwórz plik klasy o nazwie *SchoolContext.cs*i Zastąp kod szablonu z następującym kodem:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Ten kod tworzy `DbSet` właściwości dla każdego zestawu jednostek. W terminologii programu Entity Framework zwykle zestawu jednostek odnosi się do tabeli bazy danych, a jednostka odpowiada wiersza w tabeli.

Można już pominięto `DbSet<Enrollment>` i `DbSet<Course>` instrukcje i będzie działać w identyczny sposób. Entity Framework to ich niejawnie ponieważ `Student` odwołań do jednostek `Enrollment` jednostki i `Enrollment` odwołań do jednostek `Course` jednostki.

Po utworzeniu bazy danych EF tworzy tabel, które mają taki sam, jak nazwy `DbSet` nazwy właściwości. Zazwyczaj są to nazwy właściwości dla kolekcji (studentów zamiast uczniów) w liczbie mnogiej, ale deweloperzy nie zgadzają się o tego, czy należy pluralized nazwy tabeli lub nie. Te samouczki będzie zastąpienie zachowania domyślnego za pośrednictwem pojedynczej tabeli nazwy kontekstu DbContext. Aby to zrobić, Dodaj następujący kod wyróżnione po ostatnim właściwości DbSet.

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Zarejestruj kontekście iniekcji zależności

Implementuje platformy ASP.NET Core [iniekcji zależności](../../fundamentals/dependency-injection.md) domyślnie. Usługi (takie jak kontekst bazy danych EF) są zarejestrowane w usłudze iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (takich jak kontrolerów MVC) są udostępniane tych usług za pomocą parametrów konstruktora. Zobaczysz kontrolera kod konstruktora, który pobiera wystąpienia kontekstu w dalszej części tego samouczka.

Aby zarejestrować `SchoolContext` jako usługa, otwórz *Startup.cs*i Dodaj wyróżnione wiersze do `ConfigureServices` metody.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody `DbContextOptionsBuilder` obiektu. Dla wdrożenia lokalnego [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.

Dodaj `using` instrukcje dla `ContosoUniversity.Data` i `Microsoft.EntityFrameworkCore` przestrzeni nazw, a następnie skompilować projekt.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Otwórz *appsettings.json* i dodaj ciąg połączenia, jak pokazano w poniższym przykładzie.

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB.

Parametry połączenia określa bazę danych programu SQL Server LocalDB. LocalDB jest wersja programu SQL Server Express aparatu bazy danych i jest przeznaczony do tworzenia aplikacji, nie do użytku produkcyjnego. LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonych konfiguracji. Domyślnie tworzy LocalDB *.mdf* bazy danych plików w `C:/Users/<user>` katalogu.

## <a name="add-code-to-initialize-the-database-with-test-data"></a>Dodaj kod, aby zainicjować bazy danych z danych testowych

Entity Framework utworzy pustej bazy danych dla Ciebie. W tej sekcji napiszesz metodę, która jest wywoływana po utworzeniu bazy danych, aby wypełnić go danymi testu.

W tym miejscu użyjesz `EnsureCreated` metodę, aby automatycznie utworzyć bazę danych. W [nowsze samouczek](migrations.md) zobaczysz jak obsługiwać zmiany modelu przy użyciu migracje Code First, aby zmienić schemat bazy danych zamiast usunięcie i ponowne utworzenie bazy danych.

W *danych* folderu, Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Zastąp kod szablonu następujący kod, który powoduje, że bazy danych ma zostać utworzony, w razie potrzeby i danych do nowego testu obciążenia Baza danych.

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Kod sprawdza, czy istnieją wszystkie studentów w bazie danych jeśli nie, przyjęto założenie, baza danych jest nowa i musi zostać rozpoczęta z danych testowych. Ładuje dane testowe do tablic zamiast `List<T>` kolekcje w celu optymalizacji wydajności.

W *Program.cs*, zmodyfikuj `Main` metody wykonać następujące czynności podczas uruchamiania aplikacji:

* Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.
* Wywołaj metodę inicjatora, przekazanie jej w kontekście.
* Po zakończeniu metody inicjatora, należy dysponować kontekstu.

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

Dodaj `using` instrukcji:

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

W starszych samouczki, może wystąpić podobny kod w `Configure` metody w *Startup.cs*. Firma Microsoft zaleca użycie `Configure` metody tylko do konfigurowania potoku żądania. Kod uruchomienia aplikacji, należy w `Main` metody.

Teraz podczas pierwszego uruchomienia aplikacji, bazy danych zostanie utworzona i rozpoczęta z danych testowych. Przy każdej zmianie modelu danych, Usuń bazę danych, zaktualizuj metodę inicjatora i rozpoczynać się od nowa nową bazę danych tak samo. W kolejnych samouczkach zobaczysz sposobu modyfikowania bazy danych, gdy dane modelu zmiany, bez usunięcia i ponownego utworzenia go.

## <a name="create-a-controller-and-views"></a>Tworzenie kontrolera i widoków

Następnie użyjesz aparat szkieletów w programie Visual Studio, aby dodać kontroler MVC i widoki, które użyje EF w celu wykonywania zapytań i zapisać dane.

Automatyczne tworzenie widoków i metod akcji CRUD jest określany jako szkieletów. Funkcja szkieletów różni się od generowania kodu, w tym kod z utworzonym szkieletem stanowi punkt wyjścia, który można dostosować do swoich własnych wymagań należy zwykle nie należy zmodyfikować kod wygenerowany. Jeśli musisz dostosować wygenerowanego kodu, użyj klasy częściowe lub ponownego generowania kodu w przypadku zmian.

* Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj > Nowy element szkieletu**.

Jeśli **Dodaj zależności MVC** zostanie wyświetlone okno dialogowe:

* [Aktualizowanie do najnowszej wersji programu Visual Studio](https://www.visualstudio.com/downloads/). Visual Studio wersje poprzedzające 15,5 cala pokazuj tego okna dialogowego.
* Jeśli nie można zaktualizować, wybierz **Dodaj**, a następnie ponownie wykonaj kroki kontrolera Dodaj.

* W **Dodawanie szkieletu** okno dialogowe:

  * Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework**.

  * Kliknij przycisk **Dodaj**.

* W **Dodaj kontroler** okno dialogowe:

  * W **klasa modelu** wybierz **uczniowie**.

  * W **klasa kontekstu danych** wybierz **SchoolContext**.

  * Zaakceptuj wartość domyślną **StudentsController** jako nazwy.

  * Kliknij przycisk **Dodaj**.

  ![Uczniów szkieletu](intro/_static/scaffold-student.png)

  Po kliknięciu **Dodaj**, aparat szkieletów Visual Studio tworzy *StudentsController.cs* plików i zestaw widoków (*.cshtml* pliki) współpracujące z kontrolerem.

(Aparat szkieletów można również utworzyć kontekst bazy danych dla Ciebie nie utworzenie go ręcznie najpierw, tak jak wcześniej w tym samouczku. Można określić nową klasę w kontekście **Dodaj kontroler** pola, klikając znak plus z prawej strony **klasa kontekstu danych**.  Visual Studio spowoduje to utworzenie sieci `DbContext` klasy, a także kontrolera i widoków.)

Można zauważyć, że kontroler ma `SchoolContext` jako parametru konstruktora.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

Iniekcji zależności ASP.NET zajmie się przekazanie wystąpienia `SchoolContext` z kontrolerem. Skonfigurowane w *Startup.cs* wcześniej.

Zawiera kontroler `Index` metody akcji, która wyświetla wszystkie studentów w bazie danych. Metoda pobiera listę studentów z zestawu odczytując jednostek studentów `Students` właściwości wystąpienia kontekstu bazy danych:

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

Dowiesz się o elementach programowania asynchronicznego w tym kodzie później w samouczku.

*Views/Students/Index.cshtml* widoku tej listy są wyświetlane w tabeli:

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz **Debuguj > Uruchom bez debugowania** z menu.

Kliknij kartę studentów, aby wyświetlić dane testowe który `DbInitializer.Initialize` dodaje metody. W zależności od tego, jak wąskie okna przeglądarki jest, zobaczysz `Student` łącze kartę w górnej części strony lub będzie konieczne ikonę w prawym górnym rogu, aby wyświetlić łącza nawigacji.

![Strona główna contoso University wąskie](intro/_static/home-page-narrow.png)

![Strona indeksu uczniów lub studentów](intro/_static/students-index.png)

## <a name="view-the-database"></a>Widok bazy danych

Po uruchomieniu aplikacji, `DbInitializer.Initialize` wywołania metody `EnsureCreated`. EF był wyświetlany nie było żadnej bazy danych, a więc utworzyć jedną, a następnie w pozostałej części `Initialize` kod metody wypełnione w bazie danych. Można użyć **Eksplorator obiektów SQL Server** (SSOX), aby wyświetlić bazy danych w programie Visual Studio.

Zamknij przeglądarkę.

Jeśli okno SSOX nie jest jeszcze otwarty, wybierz go z **widoku** menu w programie Visual Studio.

W SSOX, kliknij przycisk **(localdb) \MSSQLLocalDB > baz danych**, a następnie kliknij polecenie wpisu dla nazwy bazy danych, który znajduje się w ciągu połączenia w Twojej *appsettings.json* pliku.

Rozwiń węzeł **tabel** węzeł, aby zobaczyć tabele w bazie danych.

![Tabele w SSOX](intro/_static/ssox-tables.png)

Kliknij prawym przyciskiem myszy **uczniowie** tabeli, a następnie kliknij przycisk **danych widoku** kolumn, które zostały utworzone i wiersze, które zostały wstawione do tabeli.

![Tabela uczniów w SSOX](intro/_static/ssox-student-table.png)

<em>.Mdf</em> i <em>ldf</em> pliki bazy danych znajdują się w <em>C:\Users\\ <yourusername> </em> folderu.

Ponieważ w przypadku wywoływania `EnsureCreated` w metodzie inicjatora uruchamiany podczas uruchamiania aplikacji, można teraz wprowadzić zmianę do `Student` klasy, Usuń bazę danych, uruchom ponownie aplikację i bazy danych będzie automatycznie utworzona ponownie, aby dopasować zmiany. Na przykład, jeśli dodasz `EmailAddress` właściwości `Student` klasy, zostanie wyświetlony nowy `EmailAddress` kolumny w tabeli odtworzony.

## <a name="conventions"></a>Konwencje

Ilość kodu, który miał do zapisu w kolejności Entity Framework można było utworzyć pełnej bazy danych dla Ciebie jest minimalny, z powodu użycia konwencje lub założenia, które wprowadza programu Entity Framework.

* Nazwy `DbSet` właściwości są używane jako nazwy tabeli. Dla jednostek nie odwołuje się `DbSet` właściwość klasy jednostka nazw są używane jako nazwy tabeli.

* Nazwy właściwości jednostki są używane dla nazw kolumn.

* Właściwości jednostki, które są nazywane Identyfikatora lub classnameID są rozpoznawane jako właściwości klucza podstawowego.

* Właściwość jest interpretowana jako właściwości klucza obcego, jeśli jest o nazwie *<navigation property name> <primary key property name>* (na przykład `StudentID` dla `Student` właściwość nawigacji, ponieważ `Student` jest klucza podstawowego jednostki `ID`). Właściwości klucza obcego może także po prostu nazwę *<primary key property name>* (na przykład `EnrollmentID` ponieważ `Enrollment` klucza podstawowego jednostki jest `EnrollmentID`).

Konwencjonalne zachowanie można przesłonić. Jawnie można na przykład określić nazwy tabeli, jak przedstawiono wcześniej w tym samouczku. I można ustawić nazwy kolumn i ustaw dowolną właściwość jako klucz podstawowy lub klucz obcy, jak można zauważyć w [nowsze samouczek](complex-data-model.md) w tej serii.

## <a name="asynchronous-code"></a>Kod asynchroniczny

Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i EF Core.

Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach wysokie obciążenie wszystkich dostępnych wątków może być używana. W takim przypadku serwer nie może przetworzyć nowe żądania aż wątki są zwalniane. Z kodem synchroniczne wiele wątków może powiązany, gdy nie są faktycznie wykonują pracę, ponieważ ich oczekiwania operacji We/Wy zakończyć. Asynchroniczne kodu podczas procesu jest oczekiwania operacji We/Wy zakończyć, jego wątku zostanie zwolniona dla serwera na potrzeby przetwarzania innych żądań. W związku z tym kod asynchroniczny umożliwia bardziej efektywne wykorzystanie zasobów serwera, a serwer jest skonfigurowany do obsługi ruchu więcej bez opóźnień.

Asynchroniczne kodu wprowadzenie niewielkiej liczby dodatkowych czynności w czasie wykonywania, ale w sytuacjach ruchu niskiej wydajności trafień jest niewielka, podczas w sytuacjach dużego natężenia ruchu sieciowego jest znacząca, potencjalne zwiększenie wydajności.

W poniższym kodzie `async` — słowo kluczowe, `Task<T>` zwrócić wartość, `await` — słowo kluczowe, i `ToListAsync` metoda powoduje, że kod asynchroniczne.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* `async` — Słowo kluczowe informuje kompilator, aby wygenerować wywołań zwrotnych dla części treści metody i do automatycznego tworzenia `Task<IActionResult>` obiekt, który jest zwracany.

* Zwracany typ `Task<IActionResult>` reprezentuje trwającej pracy z wyniku typu `IActionResult`.

* `await` — Słowo kluczowe powoduje, że kompilator podzielić na dwie części metodę. Pierwsza część kończy operację, który jest uruchamiany asynchronicznie. Druga część są umieszczane w metodę wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.

* `ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.

Należy pamiętać o podczas pisania kodu asynchroniczne, który korzysta z programu Entity Framework w kilku kwestiach:

* Tylko instrukcji, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie. Zawierającej, na przykład `ToListAsync`, `SingleOrDefaultAsync`, i `SaveChangesAsync`. Nie ma wśród nich, na przykład instrukcji, które można zmienić `IQueryable`, takich jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Kontekst EF nie jest bezpieczne dla wątków: nie należy próbować wykonać kilka operacji wykonywane równolegle. Po wywołaniu dowolnej metody EF async zawsze używaj `await` — słowo kluczowe.

* Jeśli chcesz skorzystać z zalet wydajności async kodu, upewnij się, że wszystkie biblioteki pakietami, które używasz (takie jak w przypadku stronicowania), również korzystają asynchronicznego, jeśli wywołują wszystkie metody powodują zapytania do bazy danych programu Entity Framework.

Aby uzyskać więcej informacji na temat programowania asynchronicznego w programie .NET, zobacz [omówienie Async](https://docs.microsoft.com/dotnet/articles/standard/async).

## <a name="summary"></a>Podsumowanie

Teraz utworzyć prostą aplikację, która używa do przechowywania i wyświetlania danych programu Entity Framework Core i SQL Server Express LocalDB. Następujące kroki samouczka, dowiesz się, jak wykonać podstawowe CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) operacji.

> [!div class="step-by-step"]
> [Next](crud.md)
