---
title: Platforma ASP.NET Core MVC z programem EF Core — zaawansowane — 10 10
author: rick-anderson
description: W tym samouczku przedstawiono przydatnych tematów dla wykraczających poza podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core, korzystających z platformy Entity Framework Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/advanced
ms.openlocfilehash: ba3834b29e78972bf914a5cba1a2cae3cc19a315
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "50090787"
---
# <a name="aspnet-core-mvc-with-ef-core---advanced---10-of-10"></a>Platforma ASP.NET Core MVC z programem EF Core — zaawansowane — 10 10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core MVC za pomocą platformy Entity Framework Core i Visual Studio. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](intro.md).

W poprzednim samouczku wdrożono Tabela wg hierarchii dziedziczenia. W tym samouczku przedstawiono kilka tematów, które są przydatne pod uwagę podczas czegoś podstawy tworzenia aplikacji sieci web platformy ASP.NET Core, które używają platformy Entity Framework Core.

## <a name="raw-sql-queries"></a>Pierwotne zapytania SQL

Jedną z zalet używający narzędzia Entity Framework jest, że takie rozwiązanie pomaga uniknąć wiązanie kodu zbyt ściśle do określonej metody przechowywania danych. Dzieje się tak, generując zapytań SQL i poleceń, która uwalnia użytkownika od konieczności pisania samodzielnie. Ale istnieją scenariusze wyjątkowe, gdy trzeba uruchomić określonego zapytania SQL, które zostały utworzone ręcznie. Dla tych scenariuszy interfejsu API z pierwszym kodu Entity Framework zawiera metody, które umożliwiają przekazywanie poleceń SQL bezpośrednio w bazie danych. W programu EF Core 1.0 są dostępne następujące opcje:

* Użyj `DbSet.FromSql` metodę dla zapytań zwracających typów jednostek. Zwracanych obiektów musi być typ oczekiwany przez `DbSet` obiektu, dlatego automatycznie są śledzone przez kontekst bazy danych o ile nie zostanie [wyłączyć śledzenie](crud.md#no-tracking-queries).

* Użyj `Database.ExecuteSqlCommand` poleceń niebędącą zapytaniem.

Jeśli potrzebujesz uruchomić zapytanie, które zwraca typy, które nie są jednostki, można użyć ADO.NET z dostarczonych przez EF połączenie z bazą danych. Zwrócone dane nie są śledzone przez kontekst bazy danych, nawet w przypadku używania tej metody można pobrać typów jednostek.

Jak jest zawsze wartość true, gdy wykonywanie poleceń SQL w aplikacji sieci web, należy podjąć środki ostrożności, aby chronić witrynę przed atakami polegającymi na iniekcji SQL. Jednym sposobem wykonania tego zadania jest użyć sparametryzowanych zapytań, aby upewnić się, że ciągi przesłane przez stronę sieci web nie może być interpretowany jako polecenia SQL. W tym samouczku użyjesz zapytaniach parametrycznych podczas integrowania danych wejściowych użytkownika na kwerendę.

## <a name="call-a-query-that-returns-entities"></a>Wywołania zapytania, które zwraca jednostek

`DbSet<TEntity>` Klasa dostarcza metody, która służy do wykonywania zapytania, które zwraca jednostki typu `TEntity`. Aby zobaczyć, jak to działa możesz poznasz, jak zmienić kod w `Details` metody kontrolera działu.

W *DepartmentsController.cs*w `Details` metody, Zastąp kod, który pobiera działowi `FromSql` wywołania metody, jak pokazano na następujący wyróżniony kod:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

Aby sprawdzić, czy nowy kod działa poprawnie, należy wybrać **działów** kartę i następnie **szczegóły** dla jednego z działów.

![Szczegóły działu](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a>Wywołania zapytania zwracającego innych typów

Wcześniej utworzono uczniów siatki statystyki dla strony informacje, które wykazało, że liczba studentów każdej daty rejestracji. Masz dane z zestawu jednostek uczniów (`_context.Students`) i używać programu LINQ do projektu wyniki w postaci listy `EnrollmentDateGroup` wyświetlić obiekty w modelu. Załóżmy, że chcesz napisać SQL sam, a nie za pomocą LINQ. Aby zrobić, należy uruchomić zapytanie SQL zwracającego coś innego niż obiekty jednostki. W programie EF Core 1.0 jednym ze sposobów, aby to zrobić jest pisanie kodu ADO.NET i uzyskać połączenia z bazą danych z programów EF.

W *HomeController.cs*, Zastąp `About` metoda następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

Dodaj instrukcję using instrukcji:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

Uruchom aplikację, a następnie przejdź do strony informacje. Wyświetla te same dane, które wcześniej.

![Informacje o stronie](advanced/_static/about.png)

## <a name="call-an-update-query"></a>Wywołaj zapytanie aktualizujące

Załóżmy, że administratorzy Contoso University chce wykonywać globalnych zmian w bazie danych, takich jak zmienianie liczby środki na korzystanie z każdego kursu. Jeśli uniwersytecie ma dużą liczbę kursów, będzie nieefektywne pobierać je wszystkie jako jednostki, a następnie zmianę ich osobno. W tej sekcji możesz wdrożyć strony sieci web, która umożliwia użytkownikowi określenie współczynnik za pomocą którego można zmienić ilość środków, aby uzyskać wszystkie kursy i wprowadzisz zmiany przez wykonanie instrukcji SQL UPDATE. Strony sieci web będzie wyglądał jak na poniższej ilustracji:

![Strona kurs środki na korzystanie z aktualizacji](advanced/_static/update-credits.png)

W *CoursesContoller.cs*, Dodaj metody UpdateCourseCredits HttpGet i HttpPost:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

Gdy kontroler przetworzy żądanie HttpGet, nic nie zostanie zwrócone w `ViewData["RowsAffected"]`, i wyświetla widok puste pole tekstowe i przycisk przesyłania, jak pokazano na poprzedniej ilustracji.

Gdy **aktualizacji** przycisku, wywoływana jest metoda HttpPost i mnożnik został wprowadzony w polu tekstowym. Kod wykonuje następnie SQL Server, który aktualizuje kursów i zwraca liczbę wierszy dotyczy do widoku `ViewData`. Jeśli widok pobiera `RowsAffected` wartości, wyświetla liczba zaktualizowanych wierszy.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *widoków/kursy* folder, a następnie kliknij **Dodaj > Nowy element**.

W **Dodaj nowy element** okno dialogowe, kliknij przycisk **ASP.NET** w obszarze **zainstalowane** w okienku po lewej stronie kliknij **strona widoku MVC**i Nazwij nowy widok  *UpdateCourseCredits.cshtml*.

W *Views/Courses/UpdateCourseCredits.cshtml*, Zastąp kod szablonu poniższym kodem:

[!code-html[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

Uruchom `UpdateCourseCredits` metody, wybierając **kursów** kartę, następnie dodając "/ UpdateCourseCredits" na końcu adresu URL w pasku adresu przeglądarki (na przykład: `http://localhost:5813/Courses/UpdateCourseCredits`). Wprowadź liczbę w polu tekstowym:

![Strona kurs środki na korzystanie z aktualizacji](advanced/_static/update-credits.png)

Kliknij przycisk **Aktualizuj**. Zobaczysz liczbę uwzględnionych wierszy:

![Zaktualizowano wierszy strony kurs środki na korzystanie z aktualizacji](advanced/_static/update-credits-rows-affected.png)

Kliknij przycisk **powrót do listy** Aby wyświetlić listę kursy z poprawione ilość środków.

Należy pamiętać, że kodu produkcyjnego będą upewnij się, że zawsze aktualizuje wynik na liście prawidłowych danych. Uproszczony kod przedstawiony tutaj pomnożyć liczbę środki na korzystanie z wystarczająco spowodować liczby większe niż 5. ( `Credits` Właściwość ma `[Range(0, 5)]` atrybutu.) Zapytanie update będzie działać, ale nieprawidłowych danych może spowodować nieoczekiwane wyniki w innych części systemu, które zakładają, że ilość środków jest 5 lub mniej.

Aby uzyskać więcej informacji na temat pierwotne zapytania SQL, zobacz [pierwotne zapytania SQL](/ef/core/querying/raw-sql).

## <a name="examine-sql-sent-to-the-database"></a>Sprawdź SQL wysyłane do bazy danych

Czasami warto będą mogli zobaczyć rzeczywiste zapytania SQL, które są wysyłane do bazy danych. Funkcje wbudowane funkcje rejestrowania dla platformy ASP.NET Core jest automatycznie używany przez programu EF Core na zapisywanie dzienników, które zawierają SQL dla zapytań i aktualizacji. W tej sekcji zobaczysz niektóre przykłady rejestrowania SQL.

Otwórz *StudentsController.cs* i `Details` Metoda Ustaw punkt przerwania na `if (student == null)` instrukcji.

Uruchom aplikację w trybie debugowania, a następnie przejdź do strony szczegółów dla uczniów lub studentów.

Przejdź do **dane wyjściowe** wyświetlana debugowania w oknie danych wyjściowych, a zobaczysz zapytanie:

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

W tym miejscu coś, co mogą Cię zaskoczyć, zauważysz: SQL wybiera wiersze do 2 (`TOP(2)`) z tabeli osoby. `SingleOrDefaultAsync` Metoda nie jest rozpoznawane jako 1 wiersz na serwerze. Oto Dlaczego:

* Jeśli zapytanie zwróci wiele wierszy, metoda zwraca wartość null.
* Aby ustalić, czy zapytanie zwróci wiele wierszy, EF ma pod, jeśli funkcja zwraca wartość co najmniej 2.

Należy pamiętać, że nie trzeba użyć trybu debugowania i zatrzymywanie w punkcie przerwania, aby uzyskać wyniki rejestracji w **dane wyjściowe** okna. Jest po prostu wygodny sposób zatrzymywanie rejestrowania w momencie, który chcesz Przyjrzyj się dane wyjściowe. Jeśli nie zrobisz, kontynuuje rejestrowanie i masz wróć do części, które interesują Cię znaleźć.

## <a name="repository-and-unit-of-work-patterns"></a>Repozytorium i jednostki pracy

Wielu programistów napisać kod, aby zaimplementować repozytorium i jednostki pracy jako otoka wokół kodu, który współdziała z platformą Entity Framework. Te wzorce są przeznaczone do tworzenia warstwę abstrakcji od warstwy dostępu do danych i warstwy logiki biznesowej aplikacji. Implementacji tych wzorców może pomóc ochronić aplikację przed zmianami w magazynie danych i może ułatwić automatyczne testy jednostkowe i Programowanie oparte na testach (TDD). Jednak tworzenia dodatkowego kodu, aby zaimplementować te wzorce nie zawsze jest najlepszym wyborem dla aplikacji, które używają programu EF, z kilku powodów:

* Samej klasy kontekstu EF powoduje, że kod w kodzie dotyczące magazynu danych.

* Klasy kontekstu EF może działać jako klasę jednostki pracy dla bazy danych aktualizacje, czy przy użyciu programu EF.

* EF zawiera funkcje implementowania TDD bez konieczności pisania kodu w repozytorium.

Aby uzyskać informacje o sposobie implementacji repozytorium i jednostki pracy, zobacz [Entity Framework 5 wersję tej serii samouczków](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).

Entity Framework Core implementuje dostawcy bazy danych w pamięci, który może służyć do testowania. Aby uzyskać więcej informacji, zobacz [testu za pomocą InMemory](/ef/core/miscellaneous/testing/in-memory).

## <a name="automatic-change-detection"></a>Automatyczna zmiana wykrywania

Entity Framework Określa, jak jednostki został zmieniony (i w związku z tym aktualizacje, które muszą być wysyłane do bazy danych) przez porównanie bieżącej wartości jednostki przy użyciu oryginalnych wartości. Oryginalne wartości są przechowywane, gdy jednostka jest badane lub dołączone. Niektóre metody, które powodują wykrywania automatycznego zmian są następujące:

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

Jeśli prześledzić dużą liczbę jednostek można wywołać jedną z następujących metod wiele razy w pętli, może uzyskać znaczne ulepszenia wydajności, tymczasowo wyłączając przy użyciu wykrywania automatyczna zmiana `ChangeTracker.AutoDetectChangesEnabled` właściwości. Na przykład:

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a>Entity Framework Core źródła kodu i tworzenia planów

Źródło platformy Entity Framework Core jest w [ https://github.com/aspnet/EntityFrameworkCore ](https://github.com/aspnet/EntityFrameworkCore). Repozytorium programu EF Core zawiera nocne kompilacje, śledzenia problemu, specyfikacji funkcji. zadaniem, notatek ze spotkań, projektowania i [plan do przyszłego rozwoju](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap). Możesz plików lub znajdowania usterek i współtworzyć.

Mimo że kod źródłowy jest otwarty, platformy Entity Framework Core jest w pełni obsługiwany jest produktem firmy Microsoft. Zespół Microsoft Entity Framework zachowuje kontroli nad tym, którzy są akceptowane wkładu i testy wszystkich zmian w kodzie w celu zapewnienia jakości każdej wersji.

## <a name="reverse-engineer-from-existing-database"></a>Odtwarzanie z istniejącej bazy danych

Aby odtworzyć modelu danych, w tym klas jednostek z istniejącej bazy danych, należy użyć [dbcontext szkieletu](/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) polecenia. Zobacz [samouczek ułatwiający rozpoczęcie](/ef/core/get-started/aspnetcore/existing-db).

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a>Korzystanie z dynamicznej LINQ w celu uproszczenia wybór rozliczeniowy

[Trzeci samouczek z tej serii](sort-filter-page.md) pokazuje, jak napisać kod LINQ, kodować nazwy kolumn w `switch` instrukcji. Dwie kolumny do wyboru to działa prawidłowo, ale jeśli masz wiele kolumn, kod można pobrać pełny. Aby rozwiązać ten problem, można użyć `EF.Property` metodę, aby określić nazwę właściwości jako ciąg. Aby wypróbować to podejście, Zastąp `Index` method in Class metoda `StudentsController` następującym kodem.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a>Następne kroki

Na tym kończy się w tej serii samouczków na temat korzystania z programu Entity Framework Core w aplikacji ASP.NET Core MVC.

Aby uzyskać więcej informacji na temat programu EF Core, zobacz [dokumentację programu Entity Framework Core](/ef/core). Książki jest również dostępna: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).

Aby uzyskać informacje na temat wdrażania aplikacji sieci web, zobacz <xref:host-and-deploy/index>.

Aby uzyskać informacje o innych tematów dotyczących platformy ASP.NET Core MVC, takie jak uwierzytelnianie i autoryzacja, zobacz <xref:index>.

## <a name="acknowledgments"></a>Potwierdzenia

Tom Dykstra i Rick Anderson (twitter @RickAndMSFT) napisany w tym samouczku. Rowan Miller, Diego Vega i inni członkowie zespołu programu Entity Framework korzystającej z przeglądy kodu i pomogła debugowanie problemów, które powstały podczas, gdy firma Microsoft była pisanie kodu dla samouczków.

## <a name="common-errors"></a>Typowe błędy

### <a name="contosouniversitydll-used-by-another-process"></a>ContosoUniversity.dll używany przez inny proces

Komunikat o błędzie:

> Nie można otworzyć "... bin\Debug\netcoreapp1.0\ContosoUniversity.dll" do zapisu — "proces nie może uzyskać dostępu do pliku"... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll ", ponieważ jest on używany przez inny proces.

Rozwiązanie:

Zatrzymaj witrynę w usługach IIS Express. Przejdź na pasku zadań systemu Windows, Znajdź usług IIS Express i kliknij prawym przyciskiem myszy jego ikonę, wybierz lokację University firmy Contoso, a następnie kliknij przycisk **zatrzymać witrynę**.

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>Działanie bez kodu w górę i w dół metod migracji

Możliwa przyczyna:

Polecenia interfejsu wiersza polecenia platformy EF nie automatycznie Zamknij i Zapisz pliki kodu. Jeśli istnieją niezapisane zmiany, po uruchomieniu `migrations add` polecenia EF nie odnajdzie zmiany.

Rozwiązanie:

Uruchom `migrations remove` polecenia, Zapisz zmiany kodu i ponownego przeprowadzania `migrations add` polecenia.

### <a name="errors-while-running-database-update"></a>Błędy podczas aktualizacji bazy danych uruchomione

Istnieje możliwość uzyskać inne błędy, podczas wprowadzania zmian schematu w bazie danych, która ma istniejące dane. Jeśli występują błędy migracji, których nie można rozwiązać, możesz zmienić nazwę bazy danych w parametrach połączenia lub usunąć bazy danych. Za pomocą nowej bazy danych nie ma żadnych danych do migracji, a polecenie update-database jest znacznie bardziej prawdopodobne zakończyć bez błędów.

Najprostszą metodą jest zmiana nazwy bazy danych w *appsettings.json*. Przy następnym uruchomieniu `database update`, będzie można utworzyć nową bazę danych.

Aby usunąć bazę danych w SSOX, kliknij prawym przyciskiem myszy bazę danych, kliknij przycisk **Usuń**, a następnie w polu **Usuń bazę danych** wybierz okno dialogowe **Zamknij istniejące połączenia** i kliknij przycisk  **OK**.

Aby usunąć bazę danych przy użyciu interfejsu wiersza polecenia, uruchom `database drop` interfejsu wiersza polecenia:

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>Błąd podczas lokalizowania wystąpienia programu SQL Server

Komunikat o błędzie:

> Wystąpił błąd związany z siecią lub wystąpieniem podczas nawiązywania połączenia z programem SQL Server. Serwer nie został znaleziony lub jest on niedostępny. Sprawdź, czy nazwa wystąpienia jest prawidłowa i że program SQL Server jest skonfigurowany do zezwalania na połączenia zdalne. (Dostawca: Interfejsy sieciowe programu SQL, błąd: 26 — błąd podczas znajdowania/podanego wystąpienia)

Rozwiązanie:

Sprawdź parametry połączenia. Jeśli został ręcznie usunięty plik bazy danych, Zmień nazwę bazy danych w ciągu konstrukcji, aby zacząć od nowa z nową bazę danych.

::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](inheritance.md)
