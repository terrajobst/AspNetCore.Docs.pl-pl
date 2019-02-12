---
title: 'Samouczek: Rozpoczynanie pracy z programem EF Core w aplikacji sieci web platformy ASP.NET MVC'
description: Jest to pierwszy z serii samouczków, które wyjaśniają, jak tworzyć Contoso University przykładowej aplikacji od podstaw.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/06/2019
ms.topic: tutorial
uid: data/ef-mvc/intro
ms.openlocfilehash: f7b557c8e560393ae886c46fad95c48ccbcc65b4
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102971"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a>Samouczek: Rozpoczynanie pracy z programem EF Core w aplikacji sieci web platformy ASP.NET MVC

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core 2.2 MVC przy użyciu programu Entity Framework (EF) Core 2.0 oraz program Visual Studio 2017.

Przykładowa aplikacja jest witryną sieci web dla uniwersytetu fikcyjnej firmy Contoso. Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora. Jest to pierwszy z serii samouczków, które wyjaśniają, jak tworzyć Contoso University przykładowej aplikacji od podstaw.

EF Core 2.0 jest najnowsza wersja programu EF jeszcze nie wszystkie funkcje programu EF 6.x. Aby uzyskać informacje o tym, jak dokonać wyboru między EF 6.x i programem EF Core, zobacz [vs programu EF Core. EF6.x](/ef/efcore-and-ef6/). Jeśli wybierzesz EF w wersji 6.x, zobacz [poprzedniej wersji tej serii samouczków](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

> [!NOTE]
> Wersja platformy ASP.NET Core 1.1 po ukończeniu tego samouczka, zobacz [wersji programu VS 2017 Update 2 po ukończeniu tego samouczka w formacie PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/efmvc1.1.pdf).

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Tworzenie aplikacji sieci web platformy ASP.NET Core MVC
> * Ustawianie stylów lokacji
> * Dowiedz się więcej o pakietach EF Core NuGet
> * Tworzenie modelu danych
> * Utwórz kontekst bazy danych
> * Zarejestruj SchoolContext
> * Zainicjuj kontekst bazy danych przy użyciu danych testowych
> * Tworzenie widoków i kontrolerów
> * Widok bazy danych

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli napotkasz problem, nie można rozpoznać ogólnie można znaleźć rozwiązania, porównując swój kod, aby [projektu ukończona](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final). Aby uzyskać listę typowych błędów i sposobów ich rozwiązania, zobacz [sekcję Rozwiązywanie problemów w ostatni samouczek z tej serii](advanced.md#common-errors). Jeśli nie możesz znaleźć, potrzebujesz, możesz zadać pytanie na StackOverflow.com dla [platformy ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [programu EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Jest to szereg 10 samouczków, z których każdy jest oparta na czynności wykonane w starszych samouczków. Warto zapisać kopię projektu po każdym pomyślnym ukończeniu samouczka. Następnie, jeśli napotkasz problemy, można uruchomić za pośrednictwem z poprzedniego samouczka zamiast po powrocie do początku całej serii.

## <a name="contoso-university-web-app"></a>Aplikacja sieci web firmy Contoso University

Aplikacja, której można tworzyć w tych samouczkach znajduje university prostą witrynę sieci web.

Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów. Oto kilka ekranów, które zostaną utworzone.

![Strona indeksu uczniów](intro/_static/students-index.png)

![Strona edytowania uczniów](intro/_static/student-edit.png)

Styl interfejsu użytkownika w tej lokacji została zachowana blisko co to jest generowany przez wbudowane szablony, dzięki czemu samouczka można skupić się głównie na temat korzystania z programu Entity Framework.

## <a name="create-aspnet-core-mvc-web-app"></a>Tworzenie aplikacji sieci web platformy ASP.NET Core MVC

Otwórz program Visual Studio i Utwórz nowe platformy ASP.NET Core C# projektu sieci web o nazwie "ContosoUniversity".

* Z **pliku** menu, wybierz opcję **nowy > Projekt**.

* W okienku po lewej stronie wybierz **zainstalowane > Visual C# > sieci Web**.

* Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu projektu.

* Wprowadź **ContosoUniversity** jako nazwę i kliknij przycisk **OK**.

  ![Okno dialogowe nowego projektu](intro/_static/new-project2.png)

* Poczekaj, aż **nowa Core aplikacja internetowa ASP.NET (.NET Core)** wyświetlać okno dialogowe

  ![Okno dialogowe nowego projektu programu ASP.NET Core](intro/_static/new-aspnet2.png)

* Wybierz **platformy ASP.NET Core 2.2** i **aplikacji sieci Web (Model-View-Controller)** szablonu.

  **Uwaga:** Ten samouczek wymaga platformy ASP.NET Core 2.2 i programem EF Core 2.0 lub nowszej.

* Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**.

* Kliknij przycisk **OK**

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka prostych zmian zostanie skonfigurowany w menu witryny, układu i strony głównej.

Otwórz *Views/Shared/_Layout.cshtml* i wprowadź następujące zmiany:

* Należy zmienić każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso". Istnieją trzy wystąpienia.

* Dodaj elementy menu dla **o**, **studentów**, **kursów**, **Instruktorzy**, i **działów**, i Usuń **zachowania** wpis menu.

Zmiany są wyróżnione.

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,32-36,51)]

W *Views/Home/Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zamienić tekst o platformie ASP.NET i MVC z tekstem o tej aplikacji:

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz **Debuguj > Uruchom bez debugowania** z menu. Zostanie wyświetlona strona główna z kartami dla stron, które zostaną utworzone w tych samouczkach.

![Strona główna University firmy Contoso](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a>Temat pakietów programu EF Core NuGet

Aby dodać obsługę programu EF Core do projektu, należy zainstalować dostawcy bazy danych, który ma pod kątem. Ten samouczek używa programu SQL Server i pakiet dostawcy jest [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/). Ten pakiet jest objęta [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), więc nie trzeba tworzą odwołanie do pakietu, jeśli aplikacja ma odwołania do pakietu dla `Microsoft.AspNetCore.App` pakietu.

Ten pakiet i jego zależności (`Microsoft.EntityFrameworkCore` i `Microsoft.EntityFrameworkCore.Relational`) zapewnia obsługę środowiska uruchomieniowego dla platformy EF. Należy dodać pakiet narzędzi w dalszej części [migracje](migrations.md) samouczka.

Aby uzyskać informacje o innych dostawców bazy danych, które są dostępne dla platformy Entity Framework Core, zobacz [bazy danych dostawcy](/ef/core/providers/).

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Następnie utworzysz klas jednostek dla aplikacji Contoso University. Będzie rozpoczynać następujących trzech jednostek.

![Diagram modelu danych kurs — rejestracja-ucznia](intro/_static/data-model-diagram.png)

Istnieje relacja jeden do wielu między `Student` i `Enrollment` jednostek i relacji jeden do wielu między `Course` i `Enrollment` jednostek. Innymi słowy uczniem/uczennicą mogą być rejestrowane w dowolnej liczbie kursów i kursu mogą mieć dowolną liczbę uczniów zarejestrowane w nim.

W poniższych sekcjach utworzysz klasy dla każdego z tych jednostek.

### <a name="the-student-entity"></a>Jednostki dla uczniów

![Diagram jednostek dla uczniów](intro/_static/student-entity.png)

W *modeli* folderze utwórz plik klasy o nazwie *Student.cs* i Zastąp kod szablonu poniższym kodem.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` Właściwości staną się kolumna klucza podstawowego w tabeli bazy danych, która odnosi się do tej klasy. Domyślnie platforma Entity Framework interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.

`Enrollments` Właściwość [właściwość nawigacji](/ef/core/modeling/relationships). Właściwości nawigacji przechowywania innych jednostek, które są powiązane z tej jednostki. W tym przypadku `Enrollments` właściwość `Student entity` pomieścić wszystkie `Enrollment` jednostek, które są powiązane z tym, które `Student` jednostki. Innymi słowy, jeśli w danym wierszu dla uczniów w bazie danych ma dwa powiązane wiersze rejestracji (wiersze, które zawierają ten uczniów wartość klucza podstawowego w kolumnie klucza obcego ich StudentID) który `Student` jednostki `Enrollments` właściwość nawigacji będzie zawierać tych dwa `Enrollment` jednostek.

Jeśli właściwość nawigacji może zawierać wiele jednostek (tak jak w relacji wiele do wielu lub jeden do wielu), jego typ musi być listy, w którym wpisy mogą być dodawane, usuwane lub zaktualizowane, takich jak `ICollection<T>`. Można określić `ICollection<T>` lub typu, takie jak `List<T>` lub `HashSet<T>`. Jeśli określisz `ICollection<T>`, tworzy EF `HashSet<T>` kolekcji domyślnie.

### <a name="the-enrollment-entity"></a>Jednostki rejestracji

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

W *modeli* folderze utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Właściwość będzie miała klucz podstawowy; używa tej jednostki `classnameID` wzorca zamiast `ID` sam jak opisany w `Student` jednostki. Zazwyczaj będzie wybrać jeden wzorzec i używać go w całym modelu danych. W tym miejscu odmiany ilustruje, czy można używać obu wzorca. W [dalszych samouczków](inheritance.md), zobaczysz, jak przy użyciu Identyfikatora bez classname ułatwia implementują dziedziczenie w modelu danych.

`Grade` Właściwość `enum`. Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość ma wartość null. Ma wartość null, która różni się od zera klasy korporacyjnej — wartość null oznacza, że wartość nie jest znana lub nie została jeszcze przypisana.

`StudentID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Student`. `Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, dzięki czemu właściwość może zawierać tylko jeden `Student` jednostki (w przeciwieństwie do `Student.Enrollments` właściwość nawigacji był wyświetlany poprzednio, która zawiera wiele `Enrollment` jednostek).

`CourseID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Course`. `Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.

Platforma Entity Framework interpretuje właściwość jako właściwość klucza obcego, jeśli jest on nazwany `<navigation property name><primary key property name>` (na przykład `StudentID` dla `Student` właściwość nawigacji od `Student` jest klucz podstawowy jednostki `ID`). Właściwości klucza obcego może też po prostu nazwę `<primary key property name>` (na przykład `CourseID` ponieważ `Course` jest klucz podstawowy jednostki `CourseID`).

### <a name="the-course-entity"></a>Jednostki kursu

![Diagram jednostek kursu](intro/_static/course-entity.png)

W *modeli* folderze utwórz *Course.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Właściwość jest właściwością nawigacji. A `Course` jednostki mogą być one związane z dowolną liczbę `Enrollment` jednostek.

Przycisk więcej na temat `DatabaseGenerated` atrybutu w [dalszych samouczków](complex-data-model.md) w tej serii. Zasadniczo ten atrybut umożliwia wprowadzenie klucza podstawowego dla kurs zamiast bazy danych, do jego wygenerowania.

## <a name="create-the-database-context"></a>Utwórz kontekst bazy danych

Główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework jest klasy kontekstu bazy danych. Tworzenie tej klasy, wynikające z `Microsoft.EntityFrameworkCore.DbContext` klasy. W kodzie należy określić, które jednostki są uwzględnione w modelu danych. Można również dostosować określone zachowanie programu Entity Framework. W tym projekcie nosi nazwę klasy `SchoolContext`.

W folderze projektu, Utwórz folder o nazwie *danych*.

W *danych* folderze utwórz nowy plik klasy o nazwie *SchoolContext.cs*i Zastąp kod szablonu poniższym kodem:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Ten kod tworzy `DbSet` właściwości dla każdego zestawu jednostek. W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych, a jednostka odnosi się do wiersza w tabeli.

Można już pominięto `DbSet<Enrollment>` i `DbSet<Course>` instrukcji i będzie działać tak samo. Entity Framework będzie je uwzględnić niejawnie, ponieważ `Student` odwołań do jednostek `Enrollment` jednostki i `Enrollment` odwołań do jednostek `Course` jednostki.

Po utworzeniu bazy danych EF tworzy tabel, które mają takie same jak nazwy `DbSet` nazwy właściwości. Zazwyczaj są to nazwy właściwości dla kolekcji (uczniów zamiast ucznia) w liczbie mnogiej, ale deweloperzy nie zgadzają się dotyczące tego, czy nazwy tabel należy pluralized czy nie. Te samouczki będzie zastąpić domyślne zachowanie przez określenie nazwy w liczbie pojedynczej tabeli w kontekstu DbContext. Aby to zrobić, Dodaj następujący wyróżniony kod po ostatniej właściwości DbSet.

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a>Zarejestruj SchoolContext

Implementuje platformy ASP.NET Core [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) domyślnie. Usługi (takie jak EF kontekst bazy danych) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług, (na przykład kontrolerów MVC) znajdują się tych usług za pomocą parametry konstruktora. Zobaczysz kod konstruktora kontrolera, pobierające wystąpienie kontekstu w dalszej części tego samouczka.

Aby zarejestrować `SchoolContext` jako usługi, otwórz *Startup.cs*i Dodaj wyróżnione wiersze w celu `ConfigureServices` metody.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na `DbContextOptionsBuilder` obiektu. Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.

Dodaj `using` instrukcji dla `ContosoUniversity.Data` i `Microsoft.EntityFrameworkCore` przestrzeni nazw, a następnie Skompiluj projekt.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Otwórz *appsettings.json* pliku i dodaj ciąg połączenia, jak pokazano w poniższym przykładzie.

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Parametry połączenia określają bazę danych programu SQL Server LocalDB. LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych i jest przeznaczony do tworzenia aplikacji, nie do użytku produkcyjnego. LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji. Domyślnie tworzy LocalDB *.mdf* bazy danych plików w `C:/Users/<user>` katalogu.

## <a name="initialize-db-with-test-data"></a>Zainicjuj kontekst bazy danych przy użyciu danych testowych

Entity Framework utworzy pustą bazę danych dla Ciebie. W tej sekcji możesz napisanie metody, która jest wywoływana po utworzeniu bazy danych, aby wypełnić je danymi testu.

W tym miejscu użyjemy `EnsureCreated` metodę, aby automatycznie utworzyć bazę danych. W [dalszych samouczków](migrations.md) zobaczysz jak obsługiwać zmiany modelu, używając migracje Code First do zmiany schematu bazy danych zamiast porzucenie i ponowne utworzenie bazy danych.

W *danych* folderu, Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Zastąp kod szablonu poniższym kodem, co powoduje, że bazy danych ma zostać utworzony, w razie i ładowania testów danych do nowego Baza danych.

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Kod sprawdza, czy istnieją wszystkie studentów w bazie danych, a jeśli nie, zakłada się bazy danych jest nowa i musi zostać rozpoczęta z danymi. Ładuje dane testowe do tablic zamiast `List<T>` kolekcje w celu zoptymalizowania wydajności.

W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności podczas uruchamiania aplikacji:

* Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.
* Wywołaj metodę inicjatora, przekazując mu kontekst.
* Po zakończeniu seed — metoda, należy dysponować kontekstu.

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

Dodaj `using` instrukcji:

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

W samouczkach starsze, może zostać wyświetlony podobny kod w `Configure` method in Class metoda *Startup.cs*. Firma Microsoft zaleca użycie `Configure` metody tylko po to, aby skonfigurować Potok żądań. Należy do kodu uruchamiania aplikacji `Main` metody.

Teraz podczas pierwszego uruchomienia aplikacji, baza danych zostanie utworzona i zasilany z danymi. Zawsze wtedy, gdy zmienisz swój model danych, można usunąć bazę danych, zaktualizować seed — metoda i rozpoczynać się od nowa nową bazę danych tak samo. W kolejnych samouczkach pokazano, jak zmodyfikować bazy danych, gdy dane modelu zmiany, bez wcześniejszego usunięcia i ponownego utworzenia go.

## <a name="create-controller-and-views"></a>Tworzenie widoków i kontrolerów

Następnie użyjemy aparatu tworzenia szkieletów w programie Visual Studio można dodać kontroler MVC i widoki, które użyje EF w celu wykonywania zapytań i zapisywanie danych.

Automatyczne tworzenie widoków i CRUD metody akcji jest określana jako tworzenie szkieletów. Tworzenie szkieletu różni się od generowania kodu, w tym utworzony szkielet kodu stanowi punkt wyjścia, który można dostosować do własnych własnych wymagań, dlatego należy zwykle nie należy modyfikować wygenerowanego kodu. Należy dostosować wygenerowany kod, użyj klasy częściowe lub ponownego generowania kodu w przypadku zmian.

* Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj > Nowy element szkieletu**.

Jeśli **Dodaj zależności MVC** zostanie wyświetlone okno dialogowe:

* [Aktualizacja programu Visual Studio do najnowszej wersji](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Wersje serwera Visual Studio przed 15.5 pokazuj tego okna dialogowego.
* Jeśli nie można zaktualizować wybierz **Dodaj**, a następnie ponownie wykonaj kroki kontrolera Dodaj.

* W **Dodawanie szkieletu** okno dialogowe:

  * Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework**.

  * Kliknij przycisk **Dodaj**. **Dodaj kontroler MVC z widokami używający narzędzia Entity Framework** pojawi się okno dialogowe.

    ![Tworzenie szkieletu ucznia](intro/_static/scaffold-student2.png)

  * W **klasa modelu** wybierz **uczniów**.

  * W **klasa kontekstu danych** wybierz **SchoolContext**.

  * Zaakceptuj wartość domyślną **StudentsController** jako nazwę.

  * Kliknij przycisk **Dodaj**.

  Po kliknięciu **Dodaj**, aparat tworzenia szkieletów programu Visual Studio tworzy *StudentsController.cs* plików i zestaw widoków (*.cshtml* plików), pracować z kontrolerem.

(Aparat tworzenia szkieletów można również utworzyć kontekst bazy danych dla Ciebie nie utworzenie go ręcznie najpierw miało to miejsce wcześniej w tym samouczku. Można określić nową klasę kontekstu w **Dodaj kontroler** pola, klikając znak plus, aby po prawej stronie **klasa kontekstu danych**.  Visual Studio utworzy swoje `DbContext` klasy, a także kontrolera i widoki.)

Można zauważyć, że kontroler trwa `SchoolContext` jako parametr konstruktora.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

Wstrzykiwanie zależności platformy ASP.NET Core dba o przekazanie wystąpienia `SchoolContext` do kontrolera. Skonfigurowano w *Startup.cs* pliku.

Zawiera kontroler `Index` metody akcji, który wyświetla wszystkich studentów w bazie danych. Metoda pobiera listę uczniów z zestawu, czytając jednostek studentów `Students` właściwości wystąpienia kontekstu bazy danych:

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

W dalszej części tego samouczka, dowiesz się o asynchronicznych elementów programowania, w tym kodzie.

*Views/Students/Index.cshtml* widoku tej liście są wyświetlane w tabeli:

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz **Debuguj > Uruchom bez debugowania** z menu.

Kliknij kartę studentów, aby wyświetlić dane z badań, `DbInitializer.Initialize` metoda wstawiony. W zależności od tego, jak wąskie okno przeglądarki jest, zobaczysz `Student` należy łącze kartę w górnej części strony lub kliknij ikonę nawigacji w prawym górnym rogu, aby zobaczyć łącza.

![Strona główna firmy Contoso University wąskie](intro/_static/home-page-narrow.png)

![Strona indeksu uczniów](intro/_static/students-index.png)

## <a name="view-the-database"></a>Widok bazy danych

Po uruchomieniu aplikacji, `DbInitializer.Initialize` wywołania metody `EnsureCreated`. EF pokazano, że wystąpił brak bazy danych, a więc utworzyć jeden, a następnie w pozostałej części `Initialize` kod metody wypełnione w bazie danych. Możesz użyć **Eksplorator obiektów SQL Server** (SSOX), aby wyświetlić bazy danych w programie Visual Studio.

Zamknij przeglądarkę.

Jeśli okno SSOX nie jest jeszcze otwarty, wybierz go z **widoku** menu w programie Visual Studio.

SSOX, kliknij **(localdb) \MSSQLLocalDB > bazy danych**, a następnie kliknij wpis dla nazwy bazy danych, która znajduje się w ciągu połączenia w Twojej *appsettings.json* pliku.

Rozwiń **tabel** węzeł, aby zobaczyć tabele w bazie danych.

![Tabele w SSOX](intro/_static/ssox-tables.png)

Kliknij prawym przyciskiem myszy **uczniów** tabeli, a następnie kliknij przycisk **dane widoku** kolumn, które zostały utworzone i wierszy, które zostały wstawione do tabeli.

![Tabela student w SSOX](intro/_static/ssox-student-table.png)

<em>.Mdf</em> i <em>ldf</em> pliki bazy danych znajdują się w <em>C:\Users\\ <yourusername> </em> folderu.

Ponieważ w przypadku wywoływania `EnsureCreated` w metodzie inicjatora, która działa na uruchomienie aplikacji, można teraz wprowadzić zmianę do `Student` klasy, Usuń bazę danych, uruchom ponownie aplikację i bazy danych będzie automatycznie ponownie tworzone, aby dopasować zmiany. Na przykład jeśli dodasz `EmailAddress` właściwości `Student` klasy, zostanie wyświetlony nowy `EmailAddress` kolumny w tabeli odtworzony.

## <a name="conventions"></a>Konwencje

Ilość kodu, który trzeba było pisać w kolejności programu Entity Framework można było utworzyć pełną bazę danych dla Ciebie jest minimalny, ze względu na użycie Konwencji lub założenia, które czynią Entity Framework.

* Nazwy `DbSet` właściwości są używane jako nazwy tabeli. W przypadku jednostek, które nie odwołują się `DbSet` właściwość, klasa jednostki, które nazwy są używane jako nazwy tabeli.

* Nazwy właściwości jednostki są używane dla nazw kolumn.

* Właściwości jednostki, które noszą nazwy lub classnameID są rozpoznawane jako właściwości klucza podstawowego.

* Właściwość jest interpretowany jako właściwość klucza obcego, jeśli jest on nazwany * <navigation property name> <primary key property name> * (na przykład `StudentID` dla `Student` właściwość nawigacji od `Student` jest klucz podstawowy jednostki `ID`). Właściwości klucza obcego może też po prostu nazwę * <primary key property name> * (na przykład `EnrollmentID` ponieważ `Enrollment` jest klucz podstawowy jednostki `EnrollmentID`).

Konwencjonalne zachowanie można przesłonić. Na przykład można jawnie określasz nazwy tabel, jak przedstawiono wcześniej w tym samouczku. I można ustawić nazwy kolumn i ustaw dowolną właściwość jako klucz podstawowy lub klucz obcy, jak można zauważyć w [dalszych samouczków](complex-data-model.md) w tej serii.

## <a name="asynchronous-code"></a>Kod asynchroniczny

Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i programem EF Core.

Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte. Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane. Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć. Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań. W rezultacie kod asynchroniczny umożliwia bardziej efektywne wykorzystanie zasobów serwera i serwera jest włączona, aby obsłużyć większy ruch, bez opóźnień.

Kod asynchroniczny wprowadzają niewielkiej ilości obciążenia w czasie wykonywania, ale wpływający na wydajność jest nieistotny, podczas w sytuacjach dużego ruchu sytuacjach o niewielkim ruchu, istotne jest potencjalna poprawa wydajności.

W poniższym kodzie `async` — słowo kluczowe, `Task<T>` zwracają wartość, `await` — słowo kluczowe, i `ToListAsync` metoda powoduje, że kod, są wykonywane asynchronicznie.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* `async` — Słowo kluczowe informuje kompilator, aby wygenerować wywołania zwrotne dla części treści metody, a także automatyczne tworzenie `Task<IActionResult>` obiekt, który jest zwracany.

* Zwracany typ `Task<IActionResult>` reprezentuje pracę w toku z wyniku typu `IActionResult`.

* `await` — Słowo kluczowe powoduje, że kompilator podzielić metodę na dwie części. Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie. Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.

* `ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.

Niektóre elementy pod uwagę podczas pisania kodu asynchronicznego, który używa programu Entity Framework:

* Tylko te instrukcje, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie. Zawierającej, na przykład `ToListAsync`, `SingleOrDefaultAsync`, i `SaveChangesAsync`. Nie zawiera, na przykład instrukcji, które można zmienić `IQueryable`, takich jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Kontekst EF nie jest bezpieczny dla wątków: nie należy próbować wykonać wiele operacji wykonywane równolegle. Po wywołaniu każda metoda asynchroniczna, EF, należy zawsze używać `await` — słowo kluczowe.

* Jeśli chcesz skorzystać z zalet wydajności kod asynchroniczny, upewnij się, wszystkie biblioteki pakietami, które używasz (takie jak w przypadku stronicowania), jeśli wywołują wszystkie metody, że zapytania wysyłane do bazy danych programu Entity Framework użyć async.

Aby uzyskać więcej informacji na temat programowania asynchronicznego w .NET, zobacz [Przegląd Async](/dotnet/articles/standard/async).

## <a name="get-the-code"></a>Pobierz kod

[Pobieranie i wyświetlanie ukończonej aplikacji.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Utworzoną aplikację sieci web platformy ASP.NET Core MVC
> * Ustawianie stylów lokacji
> * Przedstawia informacje na temat pakietów programu EF Core NuGet
> * Utworzony model danych
> * Utworzone kontekst bazy danych
> * Zarejestrowany SchoolContext
> * Zainicjowana klasa bazy danych przy użyciu danych testowych
> * Utworzony kontroler i widoków
> * Wyświetlać bazy danych

W następującego samouczka, dowiesz się, jak do wykonywania podstawowych operacji CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) operacji.

Przejdź do następnego artykułu, aby dowiedzieć się, jak i wykonywania podstawowych operacji CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) operacji.
> [!div class="nextstepaction"]
> [Implementowanie podstawowych funkcji CRUD](crud.md)
