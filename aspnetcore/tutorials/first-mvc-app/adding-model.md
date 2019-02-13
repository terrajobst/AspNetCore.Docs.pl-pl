---
title: Dodawanie modelu do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodawanie modelu do prostą aplikację platformy ASP.NET Core.
ms.author: riande
ms.date: 02/12/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: da30c1c97cbf40a89d163b2116c8d5f9ad422b25
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159243"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Dodawanie modelu do aplikacji ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Tom Dykstra](https://github.com/tdykstra)

W tej sekcji dodasz klasy zarządzania filmów w bazie danych. Te klasy będzie "**M**odelu" wchodzi w skład **M**VC aplikacji.

Użyj tych klas z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych. EF Core to platforma mapowania obiektowo relacyjny (ORM), która upraszcza kod dostępu do danych, który trzeba napisać.

Klasy modeli, możesz utworzyć są nazywane klasami POCO (z **P**zwykły **O**ld **C**LR **O**biekty), ponieważ nie mają żadnych zależności EF Core. Określają one po prostu właściwości danych, które będą przechowywane w bazie danych.

W tym samouczku pisania klasy modeli i programem EF Core tworzy bazę danych. Alternatywnym podejściu, nieuwzględnione w tym miejscu jest do generowania klasy modelu z istniejącej bazy danych. Aby uzyskać informacji na temat tego podejścia, zobacz [ASP.NET Core — istniejąca baza danych](/ef/core/get-started/aspnetcore/existing-db).

## <a name="add-a-data-model-class"></a>Dodaj klasę modelu danych

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Kliknij prawym przyciskiem myszy *modeli* folder > **Dodaj** > **klasy**. Nazwa klasy **filmu**.

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---  
<!-- End of VS tabs -->

## <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu movie

W tej sekcji modelu movie jest szkielet. Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > Nowy element szkieletu**.

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.

![Dodaj okno dialogowe Tworzenie szkieletu](adding-model/_static/add_scaffold21.png)

Wykonaj **Dodaj kontroler** okno dialogowe:

* **Klasa modelu:** *Film (MvcMovie.Models)*
* **Klasa kontekstu danych:** Wybierz **+** ikonę i Dodaj domyślny **MvcMovie.Models.MvcMovieContext**

![Dodawanie kontekstu danych](adding-model/_static/dc.png)

* **Widoki:** Zachowaj wartość domyślną każdego z zaznaczoną opcją
* **Nazwa kontrolera:** Zachowaj wartość domyślną *MoviesController*
* Wybierz **Dodaj**

![Dodaj kontroler, okno dialogowe](adding-model/_static/add_controller2.png)

Program Visual Studio tworzy:

* Entity Framework Core [bazy danych klasy kontekstu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* Kontroler filmy (*Controllers/MoviesController.cs*)
* Pliki widoku razor dla stron Create, Delete, szczegółowe informacje, edycji i indeksu (<em>widoków/filmy/&ast;.cshtml</em>)

Automatyczne tworzenie kontekst bazy danych i [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki są określane jako *tworzenia szkieletów*.

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* Zainstaluj narzędzia do tworzenia szkieletów:

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* Uruchom następujące polecenie:

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* Zainstaluj narzędzia do tworzenia szkieletów:

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* Uruchom następujące polecenie:

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

Jeśli Uruchom aplikację i kliknąć **filmu Mvc** link, zostanie wyświetlony komunikat o błędzie podobny do następującego:

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

Należy utworzyć bazę danych, a następnie użyj programu EF Core [migracje](xref:data/ef-mvc/migrations) funkcję, aby to zrobić. Migracje umożliwia tworzenie bazy danych, która pasuje do modelu danych i zaktualizować schemat bazy danych, gdy model danych, zmiany.

<a name="pmc"></a>

## <a name="initial-migration"></a>Początkowej migracji

W tej sekcji należy wykonać następujące zadania:

* Dodaj początkowej migracji.
* Zaktualizuj bazy danych przy użyciu początkowej migracji.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów** (PMC).

   ![Menu konsoli zarządzania Pakietami](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. W konsoli zarządzania Pakietami wprowadź następujące polecenia:

   ```console
   Add-Migration Initial
   Update-Database
   ```

   `Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.

   Schemat bazy danych zależy od określonego w modelu `MvcMovieContext` klasy (w *Data/MvcMovieContext.cs* pliku). `Initial` Argument jest nazwą migracji. Można dowolną nazwę, ale zgodnie z Konwencją, jest używany na nazwę opisującą migracji. Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.

   `Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.

Schemat bazy danych zależy od określonego w modelu `MvcMovieContext` klasy (w *Data/MvcMovieContext.cs* pliku). `InitialCreate` Argument jest nazwą migracji. Można dowolną nazwę, ale zgodnie z Konwencją nazwa jest zaznaczona, opisujący migracji.

---  

Poprzednich poleceniach generuje następujące ostrzeżenie:

```text
No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.
```

Możesz zignorować ostrzeżenie o tym. Zostanie on rozwiązany później w samouczku.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności

Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst bazy danych programu EF Core) są rejestrowane przy użyciu DI podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora. Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera DI.

Sprawdź następujące `Startup.ConfigureServices` metody. Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

`MvcMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu. Kontekst danych (`MvcMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst danych określa, które jednostki są uwzględnione w modelu danych:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

Powyższy kod tworzy [DbSet\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwość zestawu jednostek. W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych. Jednostki odnosi się do wiersza w tabeli.

Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu. Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

Kontekst bazy danych utworzone i jest on zarejestrowany za pomocą kontenera DI.

---

<a name="test"></a>

### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).

Jeśli pojawi się wyjątek bazy danych podobny do następującego:

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Możesz pominąć [krok migracji](#pmc).

* Test **Utwórz** łącza.

  > [!NOTE]
  > Nie można wprowadzić dziesiętna przecinkami w `Price` pola. Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana. Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).

* Test **Edytuj**, **szczegóły**, i **Usuń** łącza.

Sprawdź `Startup` klasy:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

Poprzedni kod wyróżniony pokazuje kontekst bazy danych filmów, które są dodawane do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera:

* `services.AddDbContext<MvcMovieContext>(options =>` Określa bazę danych i parametry połączenia.
* `=>` jest [operatora lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator)

Otwórz *Controllers/MoviesController.cs* plików i zbadaj konstruktora:

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

Używa konstruktora [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`MvcMovieContext `) do kontrolera. Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silnie typizowane modeli i @model — słowo kluczowe

Wcześniej w tym samouczku pokazano, jak kontroler można przekazać dane i obiekty za pomocą widoku `ViewData` słownika. `ViewData` Słownik jest to obiekt dynamiczny, która zapewnia wygodny sposób z późnym wiązaniem do przekazywania informacji do widoku.

MVC udostępnia również możliwość przekazywania silnie typizowanych obiektów modelu widoku. Silnie typizowane temu najlepszy czas kompilacji sprawdzania kodu. Mechanizm tworzenia szkieletów używane takie podejście (który jest przekazując silnie typizowany model) z `MoviesController` klasy i widoki utworzenia metod i widoków.

Sprawdź wygenerowany `Details` method in Class metoda *Controllers/MoviesController.cs* pliku:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

`id` Parametr ogólnie jest przekazywany jako dane trasy. Na przykład `https://localhost:5001/movies/details/1` ustawia:

* Kontroler do `movies` kontrolera (pierwszy segment adresu URL).
* Działanie `details` (drugi segment adresu URL).
* Identyfikator do 1 (ostatni segment adresu URL).

Możesz również przekazać `id` przy użyciu zapytania następujący ciąg:

`https://localhost:5001/movies/details?id=1`

`id` Parametr jest zdefiniowany jako [typu dopuszczającego wartość null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) w przypadku, gdy nie jest podana wartość Identyfikatora.

A [wyrażenia lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przekazywany do `FirstOrDefaultAsync` do wybrania jednostki filmu, które odpowiada wartości ciągu danych lub zapytanie trasy.

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

Jeśli film zostanie znaleziony, wystąpienie `Movie` modelu jest przekazywany do `Details` widoku:

```csharp
return View(movie);
   ```

Sprawdź zawartość *Views/Movies/Details.cshtml* pliku:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

Jeśli dołączysz `@model` instrukcji w górnej części pliku widoku, można określić typu obiektu, który oczekuje, że widok. Podczas tworzenia kontrolera filmu, następujące `@model` instrukcja została automatycznie dołączane u góry *Details.cshtml* pliku:

```HTML
@model MvcMovie.Models.Movie
   ```

To `@model` dyrektywy umożliwia dostęp do filmów, która kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Details.cshtml* widoku Kod przekazuje każdego pola film, aby `DisplayNameFor` i `DisplayFor` pomocników HTML za pomocą silnie typizowanej `Model` obiektu. `Create` i `Edit` metody i widoki również przekazać `Movie` obiekt modelu.

Sprawdź *Index.cshtml* widoku i `Index` metody w kontrolerze filmów. Zwróć uwagę, jak kod tworzy `List` obiektu, kiedy wywoływanych przez nią `View` metody. Kod przekazuje to `Movies` listy z `Index` metody akcji do widoku:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

Podczas tworzenia kontrolera filmy tworzenia szkieletów automatycznie uwzględnione następujące `@model` instrukcji na górze *Index.cshtml* pliku:

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

`@model` Dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Index.cshtml* wyświetlić kod pętlę filmów z `foreach` instrukcji na silnie typizowaną `Model` obiektu:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

Ponieważ `Model` obiektu zdecydowanie jest wpisane (jako `IEnumerable<Movie>` obiektu), każdy element w pętli jest wpisana jako `Movie`. Wśród innych korzyści oznacza to, uzyskasz czasie kompilacji sprawdzania kodu:

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Poprzednie, Dodawanie widoku](adding-view.md)
> [obok pracy przy użyciu języka SQL](working-with-sql.md)  
