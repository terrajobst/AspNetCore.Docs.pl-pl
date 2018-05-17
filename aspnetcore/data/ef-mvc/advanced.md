---
title: Zaawansowane platformy ASP.NET Core MVC podstawowych EF - - 10, 10
author: rick-anderson
description: Ten samouczek przedstawia przydatne tematy dotyczące wykraczających poza podstawy tworzenia aplikacji sieci web platformy ASP.NET Core, które używają programu Entity Framework Core.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/advanced
ms.openlocfilehash: 95500686052a3f75dd71244bc9da500300416dec
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---advanced---10-of-10"></a>Zaawansowane platformy ASP.NET Core MVC podstawowych EF - - 10, 10

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).

W samouczku poprzedniej zaimplementowano tabeli na hierarchię dziedziczenia. W tym samouczku przedstawiono kilka tematów, które są przydatne pod uwagę przed przejściem do innych niż podstawowe informacje dotyczące projektowania aplikacji sieci web platformy ASP.NET Core, które używają programu Entity Framework Core.

## <a name="raw-sql-queries"></a>Zapytania SQL pierwotnych

Jedną z zalet funkcji programu Entity Framework jest, że unika wiązanie kodu zbytnio do określonej metody przechowywania danych. Robi to przez generowanie zapytań SQL i poleceń, które zwalnia z konieczności pisania samodzielnie. Ale w przypadku większości scenariuszy, w należy uruchomić określonego zapytania SQL, które zostały utworzone ręcznie. W tych sytuacjach interfejsu API z pierwszego kodu Entity Framework zawiera metody, które umożliwiają przekazywania poleceń SQL bezpośrednio do bazy danych. W wersji 1.0 Core EF są następujące opcje:

* Użyj `DbSet.FromSql` metodę dla zapytań zwracających typów jednostek. Zwracane obiekty muszą mieć typ oczekiwany przez `DbSet` obiektów i ich automatycznie są śledzone przez kontekst bazy danych o ile nie zostanie [wyłączyć śledzenie](crud.md#no-tracking-queries).

* Użyj `Database.ExecuteSqlCommand` -query poleceń.

Jeśli musisz uruchomić zapytanie zwracające typy, które nie są jednostek, można użyć ADO.NET z dostarczonych przez EF połączenie z bazą danych. Zwrócone dane nie jest śledzony przez kontekst bazy danych, nawet w przypadku użycia tej metody można pobrać typów jednostek.

Podobnie jak zawsze podczas wykonywania polecenia SQL w aplikacji sieci web, należy wykonać środki ostrożności, aby chronić witrynę przed atakami opartymi na wstrzyknięciu kodu SQL. Jest jednym ze sposobów to zrobić na potrzeby zapytań sparametryzowanych upewnij się, że ciągi przesłane przez stronę sieci web nie można zinterpretować jako polecenia SQL. W tym samouczku użyjesz zapytań sparametryzowanych składników podczas integrowania danych wejściowych użytkownika na kwerendę.

## <a name="call-a-query-that-returns-entities"></a>Wywołanie kwerendy zwracającej jednostek

`DbSet<TEntity>` Klasa udostępnia metodę, która służy do wykonywania zapytania, które zwraca jednostki typu `TEntity`. Aby zobaczyć, jak to działa użytkownik będzie Zmień kod w `Details` kontrolera działu.

W *DepartmentsController.cs*w `Details` metody, Zastąp kod, który pobiera działu z `FromSql` wywołania metody, jak pokazano w poniższym kodzie wyróżnione:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

Aby sprawdzić, czy nowy kod działa poprawnie, wybierz **działów** kartę, a następnie **szczegóły** dla jednego z działów.

![Szczegóły działu](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a>Wywołanie kwerendy zwracającej innych typów

Wcześniej utworzono siatka uczniów statystyki dla strony informacje wskazujące, liczba studentów dla każdego dnia rejestracji. Pobrano dane z zestawu jednostek studentów (`_context.Students`) i używać LINQ do projektu wyniki w postaci listy `EnrollmentDateGroup` wyświetlić obiekty modelu. Załóżmy, że chcesz zapisać SQL samej siebie, a nie za pomocą LINQ. Aby zrobić, należy uruchomić kwerendę SQL która zwraca wartość inną niż obiektów jednostek. W programie EF Core 1.0 jeden sposób jest napisać kod ADO.NET i uzyskać połączenia z bazą danych z EF.

W *HomeController.cs*, Zastąp `About` metodę z następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

Dodaj using instrukcji:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

Uruchom aplikację i przejdź do strony informacje. Wyświetla dane, które jak poprzednio.

![Strona — informacje](advanced/_static/about.png)

## <a name="call-an-update-query"></a>Wywołaj zapytanie update

Załóżmy, że chcesz wykonać globalnych zmian w bazie danych, takich jak zmiana liczby środków dla porach co pracowników firmy Contoso administracyjnych. Jeśli uniwersyteckie ma dużą liczbę kursów, byłoby nieefektywne je odzyskać wszystkie jako jednostek i zmień je pojedynczo. W tej sekcji będziesz zaimplementować stronę sieci web, która umożliwia użytkownikowi określenie czynnikiem, o którą należy zmienić numer środków dla wszystkich kursów i wprowadzić zmiany, będzie wykonywania instrukcji SQL UPDATE. Strony sieci web będzie wyglądać jak na poniższej ilustracji:

![Strona środków w trakcie przebiegu aktualizacji](advanced/_static/update-credits.png)

W *CoursesContoller.cs*, Dodaj metody UpdateCourseCredits HttpGet i HttpPost:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

Gdy kontroler przetworzy żądanie HttpGet, nic nie zostanie zwrócone w `ViewData["RowsAffected"]`, i wyświetla widok puste pole tekstowe i przycisk Prześlij, jak pokazano na powyższej ilustracji.

Gdy **aktualizacji** przycisku, wywoływana jest metoda HttpPost i mnożnik ma wartość wprowadzona w polu tekstowym. Kod wykonywany następnie SQL Server, który aktualizuje kursy i zwraca liczbę wierszy wykorzystywanych do widoku w `ViewData`. Jeśli widok pobiera `RowsAffected` wartość wyświetlana liczba zaktualizowanych.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *widoków/kursów* folder, a następnie kliknij przycisk **Dodaj > Nowy element**.

W **Dodaj nowy element** okna dialogowego, kliknij przycisk **ASP.NET** w obszarze **zainstalowana** w okienku po lewej stronie kliknij **strona widoku MVC**oraz nazwę nowego widoku  *UpdateCourseCredits.cshtml*.

W *Views/Courses/UpdateCourseCredits.cshtml*, Zastąp kod szablonu z następującym kodem:

[!code-html[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

Uruchom `UpdateCourseCredits` metody wybierając **kursów** kartę, następnie dodając "/ UpdateCourseCredits" na końcu adresu URL na pasku adresu przeglądarki (na przykład: `http://localhost:5813/Courses/UpdateCourseCredits`). Wprowadź liczbę w polu tekstowym:

![Strona środków w trakcie przebiegu aktualizacji](advanced/_static/update-credits.png)

Kliknij przycisk **aktualizacji**. Zostanie wyświetlony liczba zmodyfikowanych wierszy:

![Uwzględnionych wierszy strony środków w trakcie przebiegu aktualizacji](advanced/_static/update-credits-rows-affected.png)

Kliknij przycisk **powrót do listy** Lista kursów poprawione numer środków.

Należy pamiętać, że kodzie produkcyjnym czy upewnij się, że zawsze aktualizuje wynikiem są prawidłowe dane. Kodu uproszczonego pokazane pomnożyć liczbę środków wystarczająco spowodować liczby większe niż 5. ( `Credits` Ma właściwość `[Range(0, 5)]` atrybutu.) Zapytanie update będzie działać, ale nieprawidłowych danych może spowodować nieoczekiwane wyniki w innych częściach systemu, które przyjęto założenie, że liczba kredytów jest 5 lub mniej.

Aby uzyskać więcej informacji o raw zapytania SQL, zobacz [Raw zapytania SQL](https://docs.microsoft.com/ef/core/querying/raw-sql).

## <a name="examine-sql-sent-to-the-database"></a>Sprawdź wysłanych do bazy danych SQL

Czasami jest przydatne można było wyświetlić rzeczywiste zapytania SQL, które są wysyłane do bazy danych. Funkcje wbudowane rejestrowanie dla platformy ASP.NET Core jest automatycznie używany przez podstawowe EF zapisywanie dzienników zawierające SQL zapytań i aktualizacji. W tej sekcji zobaczysz przykłady rejestrowania SQL.

Otwórz *StudentsController.cs* i `Details` metody Ustaw punkt przerwania na `if (student == null)` instrukcji.

Uruchom aplikację w trybie debugowania, a następnie przejdź do strony szczegółów dla studenta.

Przejdź do **dane wyjściowe** wyświetlana debugowania w oknie danych wyjściowych i Zobacz kwerendy:

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

Można zauważyć coś w tym miejscu, które mogą Cię zaskoczył: SQL wybiera wiersze do 2 (`TOP(2)`) z tabeli osoby. `SingleOrDefaultAsync` — Metoda nie jest rozpoznawane jako 1 wiersz na serwerze. Poniżej przedstawiono przyczyny:

* Jeśli zapytanie zwróci wiele wierszy, metoda zwraca wartość null.
* Aby ustalić, czy zapytanie zwróci wiele wierszy, EF musi sprawdzić, czy zwraca co najmniej 2.

Należy pamiętać, że nie trzeba używać trybu debugowania i zatrzyma się w punkt przerwania, aby uzyskać dane wyjściowe rejestrowania w **dane wyjściowe** okna. Jest po prostu wygodny sposób zatrzymania rejestrowania w momencie, które mają zostać znalezione w danych wyjściowych. Jeśli użytkownik nie należy kontynuuje rejestrowanie i konieczne przewinięcie do znalezienia interesujących Cię w części.

## <a name="repository-and-unit-of-work-patterns"></a>Repozytorium i jednostki pracy

Wielu deweloperów napisać kod do implementacji repozytorium i jednostki pracy jako otokę kod, który działa z programu Entity Framework. Te wzorce są przeznaczone do tworzenia warstwy abstrakcji między Warstwa dostępu do danych i warstwy logiki biznesowej aplikacji. Implementacja tych wzorców może pomóc zabezpieczyć aplikacji od zmian w magazynie danych i może ułatwić automatyczne testy jednostkowe lub projektowanie oparte na (testach TDD). Jednak zapisywania dodatkowy kod w celu wdrożenia tych wzorców nie zawsze jest najlepszym wyborem aplikacje używające funkcji EF, z kilku powodów:

* Ta sama klasa kontekstu EF powoduje kodu z kodu określonych w przypadku magazynu danych.

* Klasy kontekstu EF może działać jako klasę jednostki pracy dla bazy danych aktualizacje, czy przy użyciu EF.

* EF obejmuje funkcje implementowania TDD bez pisania kodu repozytorium.

Informacje dotyczące sposobu wdrażania repozytorium i jednostki pracy, zobacz [wersji programu Entity Framework 5 tego samouczka serii](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).

Entity Framework Core implementuje dostawcę bazy danych w pamięci, który może służyć do testowania. Aby uzyskać więcej informacji, zobacz [testu z InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).

## <a name="automatic-change-detection"></a>Zmiana automatycznego wykrywania

Entity Framework określa zmian jednostki (i w związku z tym aktualizacje, które muszą być wysyłane do bazy danych), porównując bieżące wartości jednostki z oryginalnych wartości. Oryginalne wartości są przechowywane po jednostek jest poddawany kwerendzie lub dołączony. Niektóre metody, które powodują zmiany automatycznego wykrywania są następujące:

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

Jeśli podczas śledzenia dużą liczbę jednostek i wywołania jednej z tych metod wiele razy w pętlę, można otrzymać znaczną poprawę wydajności wyłączając tymczasowo zmień automatycznego wykrywania przy użyciu `ChangeTracker.AutoDetectChangesEnabled` właściwości. Na przykład:

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a>Entity Framework Core źródła kodu i rozwoju planów

Źródło programu Entity Framework Core jest na [ https://github.com/aspnet/EntityFrameworkCore ](https://github.com/aspnet/EntityFrameworkCore). Repozytorium EF Core zawiera kompilacje co noc, problem śledzenia, specyfikacji funkcji, spotkania protokołowane, projektowania i [przewodnik przyszłego rozwoju](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap). Może plików lub znalezienia usterek i współtworzenia.

Mimo że kod źródłowy jest otwarty, Entity Framework Core jest w pełni obsługiwany jako produkt firmy Microsoft. Zespół Microsoft Entity Framework temu formantu, w którym są akceptowane udziały i sprawdza wszystkie zmiany kodu w celu zapewnienia jakości każdej wersji.

## <a name="reverse-engineer-from-existing-database"></a>Odtwarzanie z istniejącej bazy danych

Aby odtworzyć modelu danych, w tym klas jednostek z istniejącej bazy danych, należy użyć [dbcontext szkieletu](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) polecenia. Zobacz [samouczka wprowadzającego](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a>Upraszczanie rozliczeniowy wyboru przy użyciu dynamicznych LINQ

[Trzeci samouczku tej serii](sort-filter-page.md) pokazano, jak napisać kod LINQ przez kodować nazwy kolumn w `switch` instrukcji. Dwie kolumny do wyboru to działa prawidłowo, ale jeśli masz wiele kolumn kodu można pobrać informacji pełnej. Aby rozwiązać ten problem, można użyć `EF.Property` metodę, aby określić nazwę właściwości jako ciąg. Aby wypróbować takie podejście, należy zastąpić `Index` metoda `StudentsController` następującym kodem.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a>Następne kroki

Na tym kończy się tej serii samouczków przy użyciu programu Entity Framework Core w aplikacji platformy ASP.NET MVC.

Aby uzyskać więcej informacji na temat EF Core, zobacz [dokumentację programu Entity Framework Core](https://docs.microsoft.com/ef/core). Jest również dostępna w książce: [Entity Framework Core w akcji](https://www.manning.com/books/entity-framework-core-in-action).

Aby uzyskać informacje na temat wdrażania aplikacji sieci web, zobacz [hosta i wdrażanie](xref:host-and-deploy/index).

Informacje o innych tematów dotyczących podstawowych ASP.NET MVC, takie jak uwierzytelniania i autoryzacji, zobacz [dokumentacji platformy ASP.NET Core](xref:index).

## <a name="acknowledgments"></a>Potwierdzenia

Tomasz Dykstra i Rick Anderson (twitter @RickAndMSFT) został napisany w tym samouczku. Tomaszewski rowan, Diego Vega i innych członków zespołu programu Entity Framework wspierana z przeglądy kodu i pomogła debugowanie problemów, które powstały podczas możemy zostały pisanie kodu dla samouczków.

## <a name="common-errors"></a>Typowe błędy  

### <a name="contosouniversitydll-used-by-another-process"></a>ContosoUniversity.dll używany przez inny proces

Komunikat o błędzie:

> Nie można otworzyć '... bin\Debug\netcoreapp1.0\ContosoUniversity.dll "do zapisu--" proces nie może uzyskać dostępu do pliku "... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll", ponieważ jest on używany przez inny proces.

Rozwiązanie:

Zatrzymaj witrynę w usługach IIS Express. Przejdź do paska zadań systemu Windows, Znajdź usług IIS Express i kliknij prawym przyciskiem myszy ikonę, wybierz witrynę University firmy Contoso, a następnie kliknij przycisk **Zatrzymaj witrynę**.

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>Tworzenie szkieletu z żadnego kodu w górę i w dół metody migracji

Możliwa przyczyna:

Polecenia interfejsu wiersza polecenia EF nie automatycznie Zamknij i Zapisz pliki kodu. Jeśli masz niezapisane zmiany po uruchomieniu `migrations add` polecenia EF nie odnajdzie zmiany.

Rozwiązanie:

Uruchom `migrations remove` poleceń, Zapisz zmiany kodu i uruchom ponownie `migrations add` polecenia.

### <a name="errors-while-running-database-update"></a>Wystąpiły błędy podczas uruchomionej aktualizacji bazy danych

Istnieje możliwość pobrania inne błędy podczas wprowadzania zmian schematu w bazie danych istniejących danych. Jeśli występują błędy migracji, których nie można rozwiązać, można zmienić nazwę bazy danych w parametrach połączenia lub Usuń bazę danych. Z nową bazę danych nie ma żadnych danych do migracji, a polecenia update-database jest znacznie bardziej prawdopodobne zakończyć bez błędów.

Najprostsza metoda jest można zmienić nazwy bazy danych w *appsettings.json*. Przy następnym uruchomieniu `database update`, zostanie utworzona nowa baza danych.

Aby usunąć bazę danych w SSOX, kliknij prawym przyciskiem myszy bazę danych, kliknij przycisk **usunąć**, a następnie w **usunąć bazy danych** wybierz okno dialogowe **Zamknij istniejące połączenia** i kliknij przycisk  **OK**.

Aby usunąć bazę danych za pomocą interfejsu wiersza polecenia, uruchom `database drop` polecenia interfejsu wiersza polecenia:

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>Błąd podczas lokalizowania wystąpienia programu SQL Server

Komunikat o błędzie:

> Wystąpił błąd związany z siecią lub wystąpieniem podczas ustanawiania połączenia z programem SQL Server. Serwer nie został znaleziony lub był niedostępny. Sprawdź, czy nazwa wystąpienia jest poprawna i czy programu SQL Server jest skonfigurowany do zezwalania na połączenia zdalne. (Dostawca: interfejsy sieciowe programu SQL, błąd: 26 - błąd podczas lokalizowania określonego serwera/wystąpienia)

Rozwiązanie:

Sprawdź parametry połączenia. Jeśli plik bazy danych został ręcznie usunięty, Zmień nazwę bazy danych w ciągu konstrukcji rozpocząć nową bazę danych.

> [!div class="step-by-step"]
> [Poprzednie](inheritance.md)
