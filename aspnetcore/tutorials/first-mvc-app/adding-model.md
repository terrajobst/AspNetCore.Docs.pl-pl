---
title: Dodawanie modelu do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodaj model do prostej aplikacji ASP.NET Core.
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 5ad31a2536ad70590eaa767cf20068512241f36b
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2019
ms.locfileid: "71295472"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Dodawanie modelu do aplikacji ASP.NET Core MVC

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Tomasz Dykstra](https://github.com/tdykstra)

W tej sekcji dodasz klasy do zarządzania filmami w bazie danych. Te klasy będą częścią "**m**odelu" aplikacji **m**VC.

Te klasy są używane z [Entity Framework Core](/ef/core) (Ef Core) do pracy z bazą danych. EF Core to struktura obiektu mapowania relacyjnego (ORM), która upraszcza kod dostępu do danych, który trzeba napisać.

Klasy modelu, które tworzysz, są nazywane klasami POCO ( **z Lain** **P**LR **o**biekty), ponieważ nie mają żadnej zależności od EF Core. Po prostu definiują właściwości danych, które będą przechowywane w bazie danych.

W tym samouczku najpierw napiszesz klasy modelu, a EF Core tworzy bazę danych. Alternatywnym podejściem nieopisanym w tym miejscu jest wygenerowanie klas modelu z istniejącej bazy danych. Aby uzyskać informacje na temat tego podejścia, zobacz [ASP.NET Core — istniejąca baza danych](/ef/core/get-started/aspnetcore/existing-db).

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a>Dodaj klasę modelu danych

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** > **klasę**. Nazwij plik *Movie.cs*.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

Dodaj plik o nazwie *Movie.cs* do folderu *models* .

---

Zaktualizuj plik *Movie.cs* przy użyciu następującego kodu:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

`Movie` Klasa`Id` zawiera pole, które jest wymagane przez bazę danych klucza podstawowego.

Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) on `ReleaseDate` określa typ danych (`Date`). Z tym atrybutem:

  * Użytkownik nie musi wprowadzać informacji o czasie w polu Data.
  * Tylko data jest wyświetlana, a nie informacje o czasie.

[Adnotacje DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) są omówione w kolejnym samouczku.

## <a name="add-nuget-packages"></a>Dodaj pakiety NuGet

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W menu **Narzędzia** wybierz kolejno pozycje > **Menedżer pakietów NuGet** **konsola Menedżera pakietów** (PMC).

![Menu konsoli zarządzania Pakietami](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

W obszarze PMC Uruchom następujące polecenie:

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

Poprzednie polecenie dodaje dostawcę SQL Server EF Core. Pakiet dostawcy instaluje pakiet EF Core jako zależność. Dodatkowe pakiety są instalowane automatycznie w kroku tworzenia szkieletu w dalszej części tego samouczka.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a>Tworzenie klasy kontekstu bazy danych

Klasa kontekstu bazy danych jest wymagana do koordynowania funkcji EF Core (tworzenie, odczytywanie, aktualizowanie, usuwanie) `Movie` dla modelu. Kontekst bazy danych pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) i określa jednostki, które mają zostać uwzględnione w modelu danych.

Utwórz folder *danych* .

Dodaj plik *Data/MvcMovieContext. cs* o następującym kodzie: 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

Poprzedni kod tworzy [\<nieogólnymi > film](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek. W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych. Jednostki odnosi się do wiersza w tabeli.

<a name="reg"></a>

## <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

ASP.NET Core jest skompilowany przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst EF Core DB) muszą być zarejestrowane przy użyciu funkcji "DI" podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora. Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka. W tej sekcji rejestrujesz kontekst bazy danych przy użyciu funkcji DI Container.

Dodaj następujące `using` instrukcje w górnej części *Startup.cs*:

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

Dodaj następujący wyróżniony kod w `Startup.ConfigureServices`:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu. Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a>Dodaj parametry połączenia z bazą danych

Dodaj parametry połączenia do pliku *appSettings. JSON* :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

Kompiluj projekt jako sprawdzenie błędów kompilatora.

## <a name="scaffold-movie-pages"></a>Strony z filmem szkieletu

Użyj narzędzia do tworzenia szkieletu, aby utworzyć strony z przykładem tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD) dla modelu filmu.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , **> Dodaj > nowy element szkieletowy**.

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler MVC z widokami przy użyciu Entity Framework > Dodaj**.

![Okno dialogowe Dodawanie szkieletu](adding-model/_static/add_scaffold21.png)

Ukończ okno dialogowe **Dodawanie kontrolera** :

* **Klasa modelu:** *Film (MvcMovie. models)*
* **Klasa kontekstu danych:** *MvcMovieContext (MvcMovie. Data)*

![Dodaj kontekst danych](adding-model/_static/dc3.png)

* **Widoki:** Zachowaj wartość domyślną dla każdej zaznaczonej opcji
* **Nazwa kontrolera:** Zachowaj domyślną *MoviesController*
* Wybierz pozycję **Dodaj**

Program Visual Studio tworzy:

* Kontroler filmów (*controllers/MoviesController. cs*)
* Pliki widoku Razor na potrzeby tworzenia, usuwania, szczegółów, edytowania i indeksowania stron (*widoki/filmy/\*. cshtml*)

Automatyczne tworzenie tych plików jest znane jako *rusztowania*.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).

* W systemie Linux wyeksportuj ścieżkę narzędzia do tworzenia szkieletu:

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* Uruchom następujące polecenie:

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).

* Uruchom następujące polecenie:

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

Nie można jeszcze użyć stron szkieletowych, ponieważ baza danych nie istnieje. Jeśli uruchomisz aplikację i klikniesz link **aplikacji filmowej** , uzyskasz dostęp do *otwartej bazy danych* lub *nie ma takiej tabeli: Komunikat* o błędzie filmu.

<a name="migration"></a>

## <a name="initial-migration"></a>Początkowej migracji

Użyj funkcji [migracji](xref:data/ef-mvc/migrations) EF Core, aby utworzyć bazę danych. Migracje to zestaw narzędzi umożliwiających tworzenie i aktualizowanie bazy danych w celu dopasowania jej do modelu danych.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W menu **Narzędzia** wybierz kolejno pozycje > **Menedżer pakietów NuGet** **konsola Menedżera pakietów** (PMC).

W konsoli zarządzania Pakietami wprowadź następujące polecenia:

```console
Add-Migration InitialCreate
Update-Database
```

* `Add-Migration InitialCreate`: Generuje plik migracji *_InitialCreate. cs migracji/{timestamp}* . `InitialCreate` Argument jest nazwą migracji. Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację. Ponieważ jest to pierwsza migracja, wygenerowana Klasa zawiera kod, aby utworzyć schemat bazy danych. Schemat bazy danych jest oparty na modelu określonym w `MvcMovieContext` klasie.

* `Update-Database`: Aktualizuje bazę danych do najnowszej migracji, która została utworzona przez poprzednie polecenie. To polecenie uruchamia `Up` metodę w pliku *migrations/{Time-sygnatura} _InitialCreate. cs* , który tworzy bazę danych.

  Polecenie aktualizacji bazy danych generuje następujące ostrzeżenie: 

  > Nie określono typu dla kolumny dziesiętnej "price" w typie jednostki "Movie". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".

  Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core:

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* `ef migrations add InitialCreate`: Generuje plik migracji *_InitialCreate. cs migracji/{timestamp}* . `InitialCreate` Argument jest nazwą migracji. Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację. Ponieważ jest to pierwsza migracja, wygenerowana Klasa zawiera kod, aby utworzyć schemat bazy danych. Schemat bazy danych jest oparty na modelu określonym w `MvcMovieContext` klasie (w pliku *Data/MvcMovieContext. cs* ).

* `ef database update`: Aktualizuje bazę danych do najnowszej migracji, która została utworzona przez poprzednie polecenie. To polecenie uruchamia `Up` metodę w pliku *migrations/{Time-sygnatura} _InitialCreate. cs* , który tworzy bazę danych.

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a>Klasa InitialCreate

Zapoznaj się z plikiem migracji *_InitialCreate. cs migracji/{timestamp}* :

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 Metoda tworzy tabelę filmów i konfiguruje `Id` jako klucz podstawowy. `Up` Metoda przywraca zmiany schematu wykonane `Up` podczas migracji. `Down`

<a name="test"></a>

## <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i kliknij link **aplikacji filmowej** .

  Jeśli zostanie wyświetlony wyjątek podobny do jednego z następujących:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  Prawdopodobnie pominięto [krok migracji](#migration).

* Przetestuj stronę **Tworzenie** . Wprowadź i prześlij dane.

  > [!NOTE]
  > Nie można wprowadzić dziesiętna przecinkami w `Price` pola. Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana. Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).

* Przetestuj strony **Edytuj**, **szczegóły**i **Usuń** .

## <a name="dependency-injection-in-the-controller"></a>Wstrzykiwanie zależności w kontrolerze

Otwórz plik *controllers/MoviesController. cs* i zapoznaj się z konstruktorem:

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych`MvcMovieContext`() do kontrolera. Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modele silnie wpisane i @model słowo kluczowe

Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do widoku przy użyciu `ViewData` słownika. `ViewData` Słownik jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.

MVC oferuje również możliwość przekazywania obiektów modelu silnie typu do widoku. Takie silnie wpisane podejście umożliwia sprawdzanie kodu czasu kompilacji. Mechanizm tworzenia szkieletu używa tego podejścia (czyli przekazania silnie określonego modelu) z `MoviesController` klasą i widokami.

Przejrzyj wygenerowaną `Details` metodę w pliku *controllers/MoviesController. cs* :

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

`id` Parametr jest zazwyczaj przesyłany jako dane trasy. Na przykład `https://localhost:5001/movies/details/1` zestawy:

* Kontroler do `movies` kontrolera (pierwszy segment adresu URL).
* Akcja do `details` (drugi segment adresu URL).
* Identyfikator 1 (ostatni segment adresu URL).

Można również przekazać `id` za pomocą ciągu zapytania w następujący sposób:

`https://localhost:5001/movies/details?id=1`

Parametr jest zdefiniowany jako [typ dopuszczający](/dotnet/csharp/programming-guide/nullable-types/index) wartość`int?`null () w przypadku, gdy nie podano wartości identyfikatora. `id`

[Wyrażenie lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przesyłane do `FirstOrDefaultAsync` , aby wybrać jednostki filmu, które pasują do wartości danych trasy lub ciągu zapytania.

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

Jeśli film zostanie znaleziony, wystąpienie `Movie` modelu jest przesyłane `Details` do widoku:

```csharp
return View(movie);
   ```

Zapoznaj się z zawartością pliku *widoki/filmy/szczegóły. cshtml* :

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

`@model` Instrukcja w górnej części pliku widoku określa typ obiektu, który jest oczekiwany przez widok. Po utworzeniu kontrolera filmu uwzględniono następujące `@model` instrukcje:

```HTML
@model MvcMovie.Models.Movie
   ```

Ta `@model` dyrektywa zezwala na dostęp do filmu, który kontroler przeszedł do widoku. `Model` Obiekt ma silną wartość. Na przykład w widoku *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` `DisplayFor` pomocników HTML z obiektem o jednoznacznie określonym typie `Model` . Metody `Create` i `Edit` iwidokirównieżprzekazują`Movie` obiekt modelu.

Sprawdź widok *index. cshtml* i `Index` metodę w kontrolerze filmów. Zwróć uwagę, jak kod tworzy `List` obiekt, gdy `View` wywołuje metodę. Kod przekazuje tę `Movies` listę `Index` z metody akcji do widoku:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

Gdy kontroler filmów został utworzony, szkielet zawiera następujące `@model` instrukcje w górnej części pliku *index. cshtml* :

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

Dyrektywa pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy `Model` użyciu jednoznacznie określonego obiektu. `@model` Na przykład w widoku *index. cshtml* kod przechodzi przez filmy z `foreach` instrukcją względem obiektu silnie wpisanego: `Model`

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

Ponieważ obiekt jest silnie określony ( `IEnumerable<Movie>` jako obiekt), każdy element w pętli jest wpisywany jako `Movie`. `Model` Dzięki temu możesz uzyskać kontrolę czasu kompilowania kodu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Poprzednie dodanie widoku](adding-view.md)
> [następnym razem z programem SQL](working-with-sql.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a>Dodaj klasę modelu danych

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** > **klasę**. Nazwa klasy **filmu**.

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

* Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu movie

W tej sekcji modelu movie jest szkielet. Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , **> Dodaj > nowy element szkieletowy**.

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler MVC z widokami przy użyciu Entity Framework > Dodaj**.

![Okno dialogowe Dodawanie szkieletu](adding-model/_static/add_scaffold21.png)

Ukończ okno dialogowe **Dodawanie kontrolera** :

* **Klasa modelu:** *Film (MvcMovie. models)*
* **Klasa kontekstu danych:** Wybierz ikonę i Dodaj domyślną **MvcMovie. models. MvcMovieContext** **+**

![Dodaj kontekst danych](adding-model/_static/dc.png)

* **Widoki:** Zachowaj wartość domyślną dla każdej zaznaczonej opcji
* **Nazwa kontrolera:** Zachowaj domyślną *MoviesController*
* Wybierz pozycję **Dodaj**

![Okno dialogowe Dodawanie kontrolera](adding-model/_static/add_controller2.png)

Program Visual Studio tworzy:

* [Klasa kontekstu bazy danych](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext. cs*)
* Kontroler filmów (*controllers/MoviesController. cs*)
* Pliki widoku Razor na potrzeby tworzenia, usuwania, szczegółów, edytowania i indeksowania stron (*widoki/filmy/\*. cshtml*)

Automatyczne tworzenie kontekstu bazy danych i metod akcji [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenie, odczytywanie, aktualizowanie i usuwanie) jest znane jako *rusztowanie*.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* Zainstaluj narzędzia do tworzenia szkieletów:

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* W systemie Linux wyeksportuj ścieżkę narzędzia do tworzenia szkieletu:

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* Uruchom następujące polecenie:

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* Zainstaluj narzędzia do tworzenia szkieletów:

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* Uruchom następujące polecenie:

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

Jeśli uruchomisz aplikację i klikniesz link do **filmu MVC** , zostanie wyświetlony komunikat o błędzie podobny do następującego:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

Należy utworzyć bazę danych i użyć funkcji [migracji](xref:data/ef-mvc/migrations) EF Core, aby to zrobić. Migracja umożliwia utworzenie bazy danych zgodnej z modelem danych i zaktualizowanie schematu bazy danych, gdy zmieni się model danych.

<a name="pmc"></a>

## <a name="initial-migration"></a>Początkowej migracji

W tej sekcji zostały wykonane następujące zadania:

* Dodaj początkowej migracji.
* Zaktualizuj bazy danych przy użyciu początkowej migracji.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. W menu **Narzędzia** wybierz kolejno pozycje > **Menedżer pakietów NuGet** **konsola Menedżera pakietów** (PMC).

   ![Menu konsoli zarządzania Pakietami](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. W konsoli zarządzania Pakietami wprowadź następujące polecenia:

   ```console
   Add-Migration Initial
   Update-Database
   ```

   `Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.

   Schemat bazy danych jest oparty na modelu określonym w `MvcMovieContext` klasie. `Initial` Argument jest nazwą migracji. Można użyć dowolnej nazwy, ale według Konwencji, nazwy opisującej migrację. Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.

   `Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.

Schemat bazy danych jest oparty na modelu określonym w `MvcMovieContext` klasie (w pliku *Data/MvcMovieContext. cs* ). `InitialCreate` Argument jest nazwą migracji. Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.

---

## <a name="examine-the-context-registered-with-dependency-injection"></a>Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności

ASP.NET Core jest skompilowany przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst EF Core DB) są rejestrowane przy użyciu funkcji "DI" podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora. Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu DI kontenera.

Przeanalizuj poniższą `Startup.ConfigureServices` metodę. Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

`MvcMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu. Kontekst danych (`MvcMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst danych określa, które jednostki są uwzględnione w modelu danych:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

Poprzedni kod tworzy [\<nieogólnymi > film](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek. W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych. Jednostki odnosi się do wiersza w tabeli.

Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu. Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

Utworzono kontekst bazy danych i zarejestrowano go przy użyciu DI Container.

---

<a name="test"></a>

### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).

Jeśli zostanie wyświetlony wyjątek bazy danych podobny do poniższego:

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Możesz pominąć [krok migracji](#pmc).

* Test **Utwórz** łącza. Wprowadź i prześlij dane.

  > [!NOTE]
  > Nie można wprowadzić dziesiętna przecinkami w `Price` pola. Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana. Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).

* Test **Edytuj**, **szczegóły**, i **Usuń** łącza.

Zapoznaj `Startup` się z klasą:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

Poprzedni wyróżniony kod pokazuje kontekst bazy danych filmu dodawany do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) :

* `services.AddDbContext<MvcMovieContext>(options =>`Określa bazę danych, która ma być używana, oraz parametry połączenia.
* `=>`jest [operatorem lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator)

Otwórz plik *controllers/MoviesController. cs* i zapoznaj się z konstruktorem:

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych`MvcMovieContext`() do kontrolera. Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modele silnie wpisane i @model słowo kluczowe

Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do widoku przy użyciu `ViewData` słownika. `ViewData` Słownik jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.

MVC oferuje również możliwość przekazywania obiektów modelu silnie typu do widoku. Takie silnie wpisane podejście umożliwia lepsze sprawdzanie czasu kompilowania kodu. Mechanizm tworzenia szkieletów korzysta z tego podejścia (czyli przekazywania modelu silnie określonego typu) z klasą i widokami, `MoviesController` gdy utworzyły metody i widoki.

Przejrzyj wygenerowaną `Details` metodę w pliku *controllers/MoviesController. cs* :

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

`id` Parametr jest zazwyczaj przesyłany jako dane trasy. Na przykład `https://localhost:5001/movies/details/1` zestawy:

* Kontroler do `movies` kontrolera (pierwszy segment adresu URL).
* Akcja do `details` (drugi segment adresu URL).
* Identyfikator 1 (ostatni segment adresu URL).

Można również przekazać `id` za pomocą ciągu zapytania w następujący sposób:

`https://localhost:5001/movies/details?id=1`

Parametr jest zdefiniowany jako [typ dopuszczający](/dotnet/csharp/programming-guide/nullable-types/index) wartość`int?`null () w przypadku, gdy nie podano wartości identyfikatora. `id`

[Wyrażenie lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przesyłane do `FirstOrDefaultAsync` , aby wybrać jednostki filmu, które pasują do wartości danych trasy lub ciągu zapytania.

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

Jeśli film zostanie znaleziony, wystąpienie `Movie` modelu jest przesyłane `Details` do widoku:

```csharp
return View(movie);
   ```

Zapoznaj się z zawartością pliku *widoki/filmy/szczegóły. cshtml* :

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

Dołączając `@model` instrukcję w górnej części pliku widoku, można określić typ obiektu, którego oczekuje widok. Po utworzeniu kontrolera filmu Poniższa `@model` instrukcja została automatycznie uwzględniona w górnej części pliku *details. cshtml* :

```HTML
@model MvcMovie.Models.Movie
   ```

Ta `@model` dyrektywa pozwala uzyskać dostęp do filmu, który kontroler przeszedł do widoku przy `Model` użyciu jednoznacznie określonego obiektu. Na przykład w widoku *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` `DisplayFor` pomocników HTML z obiektem o jednoznacznie określonym typie `Model` . Metody `Create` i `Edit` iwidokirównieżprzekazują`Movie` obiekt modelu.

Sprawdź widok *index. cshtml* i `Index` metodę w kontrolerze filmów. Zwróć uwagę, jak kod tworzy `List` obiekt, gdy `View` wywołuje metodę. Kod przekazuje tę `Movies` listę `Index` z metody akcji do widoku:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

Podczas tworzenia kontrolera filmów zostanie automatycznie uwzględniona następująca `@model` instrukcja w górnej części pliku *index. cshtml* :

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

Dyrektywa pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy `Model` użyciu jednoznacznie określonego obiektu. `@model` Na przykład w widoku *index. cshtml* kod przechodzi przez filmy z `foreach` instrukcją względem obiektu silnie wpisanego: `Model`

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

Ponieważ obiekt jest silnie określony ( `IEnumerable<Movie>` jako obiekt), każdy element w pętli jest wpisywany jako `Movie`. `Model` Dzięki temu możesz uzyskać kontrolę czasu kompilowania kodu:

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Poprzednie dodanie widoku](adding-view.md)
> [następnym razem z programem SQL](working-with-sql.md)

::: moniker-end
