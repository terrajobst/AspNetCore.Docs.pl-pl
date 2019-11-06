---
title: Dodawanie modelu do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodaj model do prostej aplikacji ASP.NET Core.
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: d6d75bcbab875c08bfff532d968013dca323beed
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634105"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="671ae-103">Dodawanie modelu do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="671ae-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="671ae-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="671ae-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="671ae-105">W tej sekcji dodasz klasy do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="671ae-106">Te klasy będą częścią "**m**odelu" aplikacji **m**VC.</span><span class="sxs-lookup"><span data-stu-id="671ae-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="671ae-107">Te klasy są używane z [Entity Framework Core](/ef/core) (Ef Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="671ae-108">EF Core to struktura obiektu mapowania relacyjnego (ORM), która upraszcza kod dostępu do danych, który trzeba napisać.</span><span class="sxs-lookup"><span data-stu-id="671ae-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="671ae-109">Klasy modelu, które tworzysz, są nazywane klasami POCO ( **z Lain** **P**LR **o**biekty), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="671ae-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="671ae-110">Po prostu definiują właściwości danych, które będą przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="671ae-111">W tym samouczku najpierw napiszesz klasy modelu, a EF Core tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="671ae-112">Alternatywnym podejściem nieopisanym w tym miejscu jest wygenerowanie klas modelu z istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="671ae-113">Aby uzyskać informacje na temat tego podejścia, zobacz [ASP.NET Core — istniejąca baza danych](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="671ae-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="671ae-114">Dodaj klasę modelu danych</span><span class="sxs-lookup"><span data-stu-id="671ae-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="671ae-116">Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="671ae-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="671ae-117">Nazwij plik *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="671ae-117">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="671ae-118">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="671ae-119">Dodaj plik o nazwie *Movie.cs* do folderu *models* .</span><span class="sxs-lookup"><span data-stu-id="671ae-119">Add a file named *Movie.cs* to the *Models* folder.</span></span>

---

<span data-ttu-id="671ae-120">Zaktualizuj plik *Movie.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="671ae-120">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="671ae-121">Klasa `Movie` zawiera pole `Id`, które jest wymagane przez bazę danych klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="671ae-121">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="671ae-122">Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) w `ReleaseDate` określa typ danych (`Date`).</span><span class="sxs-lookup"><span data-stu-id="671ae-122">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="671ae-123">Z tym atrybutem:</span><span class="sxs-lookup"><span data-stu-id="671ae-123">With this attribute:</span></span>

  * <span data-ttu-id="671ae-124">Użytkownik nie musi wprowadzać informacji o czasie w polu Data.</span><span class="sxs-lookup"><span data-stu-id="671ae-124">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="671ae-125">Tylko data jest wyświetlana, a nie informacje o czasie.</span><span class="sxs-lookup"><span data-stu-id="671ae-125">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="671ae-126">[Adnotacje DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) są omówione w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="671ae-126">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="671ae-127">Dodaj pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="671ae-127">Add NuGet packages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-128">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="671ae-129">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet** > **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="671ae-129">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![Menu PMC](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="671ae-131">W obszarze PMC Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="671ae-131">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="671ae-132">Poprzednie polecenie dodaje dostawcę SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="671ae-132">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="671ae-133">Pakiet dostawcy instaluje pakiet EF Core jako zależność.</span><span class="sxs-lookup"><span data-stu-id="671ae-133">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="671ae-134">Dodatkowe pakiety są instalowane automatycznie w kroku tworzenia szkieletu w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="671ae-134">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="671ae-135">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-135">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="671ae-136">Tworzenie klasy kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="671ae-136">Create a database context class</span></span>

<span data-ttu-id="671ae-137">Klasa kontekstu bazy danych jest wymagana do koordynowania funkcji EF Core (tworzenie, odczytywanie, aktualizowanie, usuwanie) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="671ae-137">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="671ae-138">Kontekst bazy danych pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) i określa jednostki, które mają zostać uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-138">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="671ae-139">Utwórz folder *danych* .</span><span class="sxs-lookup"><span data-stu-id="671ae-139">Create a *Data* folder.</span></span>

<span data-ttu-id="671ae-140">Dodaj plik *Data/MvcMovieContext. cs* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="671ae-140">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="671ae-141">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="671ae-141">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="671ae-142">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-142">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="671ae-143">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="671ae-143">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="671ae-144">Rejestrowanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="671ae-144">Register the database context</span></span>

<span data-ttu-id="671ae-145">ASP.NET Core jest skompilowany przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="671ae-145">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="671ae-146">Usługi (takie jak kontekst EF Core DB) muszą być zarejestrowane przy użyciu funkcji "DI" podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="671ae-146">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="671ae-147">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="671ae-147">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="671ae-148">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="671ae-148">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="671ae-149">W tej sekcji rejestrujesz kontekst bazy danych przy użyciu funkcji DI Container.</span><span class="sxs-lookup"><span data-stu-id="671ae-149">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="671ae-150">Dodaj następujące instrukcje `using` w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="671ae-150">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="671ae-151">Dodaj następujący wyróżniony kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="671ae-151">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-152">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-152">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="671ae-153">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-153">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="671ae-154">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="671ae-154">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="671ae-155">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="671ae-155">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="671ae-156">Dodaj parametry połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="671ae-156">Add a database connection string</span></span>

<span data-ttu-id="671ae-157">Dodaj parametry połączenia do pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="671ae-157">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-158">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="671ae-159">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-159">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="671ae-160">Kompiluj projekt jako sprawdzenie błędów kompilatora.</span><span class="sxs-lookup"><span data-stu-id="671ae-160">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="671ae-161">Strony z filmem szkieletu</span><span class="sxs-lookup"><span data-stu-id="671ae-161">Scaffold movie pages</span></span>

<span data-ttu-id="671ae-162">Użyj narzędzia do tworzenia szkieletu, aby utworzyć strony z przykładem tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="671ae-162">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-163">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="671ae-164">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , **> Dodaj > nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="671ae-164">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

<span data-ttu-id="671ae-166">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler MVC z widokami przy użyciu Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="671ae-166">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Okno dialogowe Dodawanie szkieletu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="671ae-168">Ukończ okno dialogowe **Dodawanie kontrolera** :</span><span class="sxs-lookup"><span data-stu-id="671ae-168">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="671ae-169">**Model Class:** *Movie (MvcMovie. models)*</span><span class="sxs-lookup"><span data-stu-id="671ae-169">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="671ae-170">**Klasa kontekstu danych:** *MvcMovieContext (MvcMovie. Data)*</span><span class="sxs-lookup"><span data-stu-id="671ae-170">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![Dodaj kontekst danych](adding-model/_static/dc3.png)

* <span data-ttu-id="671ae-172">**Widoki:** Zachowaj wartość domyślną dla każdej zaznaczonej opcji</span><span class="sxs-lookup"><span data-stu-id="671ae-172">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="671ae-173">**Nazwa kontrolera:** Zachowaj domyślną *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="671ae-173">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="671ae-174">Wybierz pozycję **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="671ae-174">Select **Add**</span></span>

<span data-ttu-id="671ae-175">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="671ae-175">Visual Studio creates:</span></span>

* <span data-ttu-id="671ae-176">Kontroler filmów (*controllers/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="671ae-176">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="671ae-177">Pliki widoku Razor na potrzeby tworzenia, usuwania, szczegółów, edytowania i indeksowania stron (*widoki/filmy/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="671ae-177">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="671ae-178">Automatyczne tworzenie tych plików jest znane jako *rusztowania*.</span><span class="sxs-lookup"><span data-stu-id="671ae-178">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="671ae-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="671ae-179">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="671ae-180">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="671ae-180">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="671ae-181">W systemie Linux wyeksportuj ścieżkę narzędzia do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="671ae-181">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="671ae-182">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="671ae-182">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="671ae-183">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="671ae-184">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="671ae-184">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="671ae-185">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="671ae-185">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="671ae-186">Nie można jeszcze użyć stron szkieletowych, ponieważ baza danych nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="671ae-186">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="671ae-187">Jeśli uruchomisz aplikację i klikniesz link **aplikacji filmowej** , uzyskasz dostęp do *otwartej bazy danych* lub *nie ma takiej tabeli:* komunikat o błędzie filmu.</span><span class="sxs-lookup"><span data-stu-id="671ae-187">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="671ae-188">Migracja początkowa</span><span class="sxs-lookup"><span data-stu-id="671ae-188">Initial migration</span></span>

<span data-ttu-id="671ae-189">Użyj funkcji [migracji](xref:data/ef-mvc/migrations) EF Core, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-189">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="671ae-190">Migracje to zestaw narzędzi umożliwiających tworzenie i aktualizowanie bazy danych w celu dopasowania jej do modelu danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-190">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-191">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="671ae-192">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet** > **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="671ae-192">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="671ae-193">W obszarze PMC wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="671ae-193">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="671ae-194">`Add-Migration InitialCreate`: generuje plik migracji *_InitialCreate. cs migracji/{timestamp}* .</span><span class="sxs-lookup"><span data-stu-id="671ae-194">`Add-Migration InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="671ae-195">`InitialCreate` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="671ae-195">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="671ae-196">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="671ae-196">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="671ae-197">Ponieważ jest to pierwsza migracja, wygenerowana Klasa zawiera kod, aby utworzyć schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-197">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="671ae-198">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="671ae-198">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="671ae-199">`Update-Database`: aktualizuje bazę danych do najnowszej migracji, która została utworzona przez poprzednie polecenie.</span><span class="sxs-lookup"><span data-stu-id="671ae-199">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="671ae-200">To polecenie uruchamia metodę `Up` w pliku *migrations/{Time-sygnatura} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-200">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="671ae-201">Polecenie aktualizacji bazy danych generuje następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="671ae-201">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="671ae-202">Nie określono typu dla kolumny dziesiętnej "price" w typie jednostki "Movie".</span><span class="sxs-lookup"><span data-stu-id="671ae-202">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="671ae-203">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="671ae-203">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="671ae-204">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="671ae-204">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="671ae-205">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="671ae-205">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="671ae-206">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-206">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="671ae-207">Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="671ae-207">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="671ae-208">`ef migrations add InitialCreate`: generuje plik migracji *_InitialCreate. cs migracji/{timestamp}* .</span><span class="sxs-lookup"><span data-stu-id="671ae-208">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="671ae-209">`InitialCreate` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="671ae-209">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="671ae-210">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="671ae-210">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="671ae-211">Ponieważ jest to pierwsza migracja, wygenerowana Klasa zawiera kod, aby utworzyć schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-211">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="671ae-212">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext` (w pliku *Data/MvcMovieContext. cs* ).</span><span class="sxs-lookup"><span data-stu-id="671ae-212">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="671ae-213">`ef database update`: aktualizuje bazę danych do najnowszej migracji, która została utworzona przez poprzednie polecenie.</span><span class="sxs-lookup"><span data-stu-id="671ae-213">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="671ae-214">To polecenie uruchamia metodę `Up` w pliku *migrations/{Time-sygnatura} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-214">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="671ae-215">Klasa InitialCreate</span><span class="sxs-lookup"><span data-stu-id="671ae-215">The InitialCreate class</span></span>

<span data-ttu-id="671ae-216">Zapoznaj się z plikiem migracji *_InitialCreate. cs migracji/{timestamp}* :</span><span class="sxs-lookup"><span data-stu-id="671ae-216">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 <span data-ttu-id="671ae-217">Metoda `Up` tworzy tabelę filmów i konfiguruje `Id` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="671ae-217">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="671ae-218">Metoda `Down` przywraca zmiany schematu dokonane przez `Up` migracji.</span><span class="sxs-lookup"><span data-stu-id="671ae-218">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="671ae-219">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="671ae-219">Test the app</span></span>

* <span data-ttu-id="671ae-220">Uruchom aplikację i kliknij link **aplikacji filmowej** .</span><span class="sxs-lookup"><span data-stu-id="671ae-220">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="671ae-221">Jeśli zostanie wyświetlony wyjątek podobny do jednego z następujących:</span><span class="sxs-lookup"><span data-stu-id="671ae-221">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-222">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="671ae-223">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-223">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="671ae-224">Prawdopodobnie pominięto [krok migracji](#migration).</span><span class="sxs-lookup"><span data-stu-id="671ae-224">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="671ae-225">Przetestuj stronę **Tworzenie** .</span><span class="sxs-lookup"><span data-stu-id="671ae-225">Test the **Create** page.</span></span> <span data-ttu-id="671ae-226">Wprowadź i prześlij dane.</span><span class="sxs-lookup"><span data-stu-id="671ae-226">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="671ae-227">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="671ae-227">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="671ae-228">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="671ae-228">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="671ae-229">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="671ae-229">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="671ae-230">Przetestuj strony **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="671ae-230">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="671ae-231">Wstrzykiwanie zależności w kontrolerze</span><span class="sxs-lookup"><span data-stu-id="671ae-231">Dependency injection in the controller</span></span>

<span data-ttu-id="671ae-232">Otwórz plik *controllers/MoviesController. cs* i zapoznaj się z konstruktorem:</span><span class="sxs-lookup"><span data-stu-id="671ae-232">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="671ae-233">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych (`MvcMovieContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="671ae-233">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="671ae-234">Kontekst bazy danych jest używany w każdej z metod [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="671ae-234">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="671ae-235">Modele silnie wpisane i @model słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="671ae-235">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="671ae-236">Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do widoku przy użyciu słownika `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="671ae-236">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="671ae-237">Słownik `ViewData` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="671ae-237">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="671ae-238">MVC oferuje również możliwość przekazywania obiektów modelu silnie typu do widoku.</span><span class="sxs-lookup"><span data-stu-id="671ae-238">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="671ae-239">Takie silnie wpisane podejście umożliwia sprawdzanie kodu czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="671ae-239">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="671ae-240">Mechanizm tworzenia szkieletu używa tego podejścia (czyli przekazania silnie określonego modelu) z klasą `MoviesController` i widokami.</span><span class="sxs-lookup"><span data-stu-id="671ae-240">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="671ae-241">Przejrzyj wygenerowaną metodę `Details` w pliku *controllers/MoviesController. cs* :</span><span class="sxs-lookup"><span data-stu-id="671ae-241">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="671ae-242">Parametr `id` jest zwykle przenoszona jako dane trasy.</span><span class="sxs-lookup"><span data-stu-id="671ae-242">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="671ae-243">Na przykład zestawy `https://localhost:5001/movies/details/1`:</span><span class="sxs-lookup"><span data-stu-id="671ae-243">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="671ae-244">Kontroler do kontrolera `movies` (pierwszy segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="671ae-244">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="671ae-245">Akcja do `details` (drugi segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="671ae-245">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="671ae-246">Identyfikator 1 (ostatni segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="671ae-246">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="671ae-247">Możesz również przekazać `id` za pomocą ciągu zapytania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="671ae-247">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="671ae-248">Parametr `id` jest zdefiniowany jako [typ dopuszczający wartość null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) w przypadku, gdy nie podano wartości identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="671ae-248">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="671ae-249">[Wyrażenie lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przesyłane do `FirstOrDefaultAsync`, aby wybrać jednostki filmu, które pasują do wartości danych trasy lub ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="671ae-249">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="671ae-250">Jeśli film zostanie znaleziony, wystąpienie modelu `Movie` jest przesyłane do widoku `Details`:</span><span class="sxs-lookup"><span data-stu-id="671ae-250">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="671ae-251">Zapoznaj się z zawartością pliku *widoki/filmy/szczegóły. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="671ae-251">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="671ae-252">Instrukcja `@model` w górnej części pliku widoku określa typ obiektu, który jest oczekiwany przez widok.</span><span class="sxs-lookup"><span data-stu-id="671ae-252">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="671ae-253">Gdy kontroler filmu został utworzony, dołączono następującą `@model` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="671ae-253">When the movie controller was created, the following `@model` statement was included:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="671ae-254">Ta `@model` dyrektywa zezwala na dostęp do filmu, który kontroler przeszedł do widoku.</span><span class="sxs-lookup"><span data-stu-id="671ae-254">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="671ae-255">Obiekt `Model` jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="671ae-255">The `Model` object is strongly typed.</span></span> <span data-ttu-id="671ae-256">Na przykład w widoku *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` i `DisplayFor` pomocników HTML z silnie wpisaną `Model` obiektem.</span><span class="sxs-lookup"><span data-stu-id="671ae-256">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="671ae-257">Metody `Create` i `Edit` i widoki również przekazują obiekt modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="671ae-257">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="671ae-258">Sprawdź widok *index. cshtml* i metodę `Index` w kontrolerze filmów.</span><span class="sxs-lookup"><span data-stu-id="671ae-258">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="671ae-259">Zwróć uwagę, jak kod tworzy obiekt `List`, gdy wywołuje metodę `View`.</span><span class="sxs-lookup"><span data-stu-id="671ae-259">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="671ae-260">Kod przekazuje tę `Movies` listę z metody akcji `Index` do widoku:</span><span class="sxs-lookup"><span data-stu-id="671ae-260">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="671ae-261">Gdy kontroler filmów został utworzony, funkcja tworzenia szkieletów zawiera następującą instrukcję `@model` w górnej części pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="671ae-261">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="671ae-262">Dyrektywa `@model` pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="671ae-262">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="671ae-263">Na przykład, w widoku *index. cshtml* , kod przechodzi przez filmy z instrukcją `foreach` na obiekt `Model` o jednoznacznie określonym typie:</span><span class="sxs-lookup"><span data-stu-id="671ae-263">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="671ae-264">Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy element w pętli jest wpisywany jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="671ae-264">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="671ae-265">Dzięki temu możesz uzyskać kontrolę czasu kompilowania kodu.</span><span class="sxs-lookup"><span data-stu-id="671ae-265">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="671ae-266">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="671ae-266">Additional resources</span></span>

* [<span data-ttu-id="671ae-267">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="671ae-267">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="671ae-268">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="671ae-268">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="671ae-269">[Poprzednie dodanie widoku](adding-view.md)
> [następnym DZIAŁAniem z SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="671ae-269">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="671ae-270">Dodaj klasę modelu danych</span><span class="sxs-lookup"><span data-stu-id="671ae-270">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-271">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-271">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="671ae-272">Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="671ae-272">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="671ae-273">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="671ae-273">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="671ae-274">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-274">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="671ae-275">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="671ae-275">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="671ae-276">Tworzenie szkieletu modelu filmu</span><span class="sxs-lookup"><span data-stu-id="671ae-276">Scaffold the movie model</span></span>

<span data-ttu-id="671ae-277">W tej sekcji model filmu jest szkieletem.</span><span class="sxs-lookup"><span data-stu-id="671ae-277">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="671ae-278">Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="671ae-278">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-279">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-279">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="671ae-280">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , **> Dodaj > nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="671ae-280">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

<span data-ttu-id="671ae-282">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler MVC z widokami przy użyciu Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="671ae-282">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Okno dialogowe Dodawanie szkieletu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="671ae-284">Ukończ okno dialogowe **Dodawanie kontrolera** :</span><span class="sxs-lookup"><span data-stu-id="671ae-284">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="671ae-285">**Model Class:** *Movie (MvcMovie. models)*</span><span class="sxs-lookup"><span data-stu-id="671ae-285">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="671ae-286">**Klasa kontekstu danych:** Wybierz ikonę **+** i Dodaj domyślną **MvcMovie. models. MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="671ae-286">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Dodaj kontekst danych](adding-model/_static/dc.png)

* <span data-ttu-id="671ae-288">**Widoki:** Zachowaj wartość domyślną dla każdej zaznaczonej opcji</span><span class="sxs-lookup"><span data-stu-id="671ae-288">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="671ae-289">**Nazwa kontrolera:** Zachowaj domyślną *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="671ae-289">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="671ae-290">Wybierz pozycję **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="671ae-290">Select **Add**</span></span>

![Okno dialogowe Dodawanie kontrolera](adding-model/_static/add_controller2.png)

<span data-ttu-id="671ae-292">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="671ae-292">Visual Studio creates:</span></span>

* <span data-ttu-id="671ae-293">[Klasa kontekstu bazy danych](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext. cs*)</span><span class="sxs-lookup"><span data-stu-id="671ae-293">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="671ae-294">Kontroler filmów (*controllers/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="671ae-294">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="671ae-295">Pliki widoku Razor na potrzeby tworzenia, usuwania, szczegółów, edytowania i indeksowania stron (*widoki/filmy/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="671ae-295">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="671ae-296">Automatyczne tworzenie kontekstu bazy danych i metod akcji [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenie, odczytywanie, aktualizowanie i usuwanie) jest znane jako *rusztowanie*.</span><span class="sxs-lookup"><span data-stu-id="671ae-296">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="671ae-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="671ae-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="671ae-298">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="671ae-298">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="671ae-299">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="671ae-299">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="671ae-300">W systemie Linux wyeksportuj ścieżkę narzędzia do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="671ae-300">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="671ae-301">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="671ae-301">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="671ae-302">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-302">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="671ae-303">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="671ae-303">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="671ae-304">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="671ae-304">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="671ae-305">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="671ae-305">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="671ae-306">Jeśli uruchomisz aplikację i klikniesz link do **filmu MVC** , zostanie wyświetlony komunikat o błędzie podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="671ae-306">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-307">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-307">Visual Studio</span></span>](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="671ae-308">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-308">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="671ae-309">Należy utworzyć bazę danych i użyć funkcji [migracji](xref:data/ef-mvc/migrations) EF Core, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="671ae-309">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="671ae-310">Migracja umożliwia utworzenie bazy danych zgodnej z modelem danych i zaktualizowanie schematu bazy danych, gdy zmieni się model danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-310">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="671ae-311">Migracja początkowa</span><span class="sxs-lookup"><span data-stu-id="671ae-311">Initial migration</span></span>

<span data-ttu-id="671ae-312">W tej sekcji zostały wykonane następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="671ae-312">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="671ae-313">Dodawanie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="671ae-313">Add an initial migration.</span></span>
* <span data-ttu-id="671ae-314">Zaktualizuj bazę danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="671ae-314">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-315">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-315">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="671ae-316">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet** > **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="671ae-316">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![Menu PMC](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="671ae-318">W obszarze PMC wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="671ae-318">In the PMC, enter the following commands:</span></span>

   ```PMC
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="671ae-319">Polecenie `Add-Migration` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-319">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="671ae-320">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="671ae-320">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="671ae-321">`Initial` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="671ae-321">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="671ae-322">Można użyć dowolnej nazwy, ale według Konwencji, nazwy opisującej migrację.</span><span class="sxs-lookup"><span data-stu-id="671ae-322">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="671ae-323">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="671ae-323">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="671ae-324">`Update-Database` polecenie uruchamia metodę `Up` w pliku *migrations/{Time-sygnatura} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-324">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="671ae-325">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-325">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="671ae-326">Polecenie `ef migrations add InitialCreate` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-326">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="671ae-327">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext` (w pliku *Data/MvcMovieContext. cs* ).</span><span class="sxs-lookup"><span data-stu-id="671ae-327">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="671ae-328">`InitialCreate` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="671ae-328">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="671ae-329">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="671ae-329">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="671ae-330">Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="671ae-330">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="671ae-331">ASP.NET Core jest skompilowany przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="671ae-331">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="671ae-332">Usługi (takie jak kontekst EF Core DB) są rejestrowane przy użyciu funkcji "DI" podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="671ae-332">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="671ae-333">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="671ae-333">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="671ae-334">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="671ae-334">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="671ae-335">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="671ae-335">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="671ae-336">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu DI kontenera.</span><span class="sxs-lookup"><span data-stu-id="671ae-336">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="671ae-337">Przeanalizuj poniższą metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="671ae-337">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="671ae-338">Podświetlony wiersz został dodany przez program do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="671ae-338">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="671ae-339">`MvcMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="671ae-339">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="671ae-340">Kontekst danych (`MvcMovieContext`) pochodzi od elementu [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="671ae-340">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="671ae-341">Kontekst danych określa, które jednostki są uwzględnione w modelu danych:</span><span class="sxs-lookup"><span data-stu-id="671ae-341">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="671ae-342">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="671ae-342">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="671ae-343">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="671ae-343">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="671ae-344">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="671ae-344">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="671ae-345">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="671ae-345">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="671ae-346">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="671ae-346">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="671ae-347">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="671ae-347">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="671ae-348">Utworzono kontekst bazy danych i zarejestrowano go przy użyciu DI Container.</span><span class="sxs-lookup"><span data-stu-id="671ae-348">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="671ae-349">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="671ae-349">Test the app</span></span>

* <span data-ttu-id="671ae-350">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="671ae-350">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="671ae-351">Jeśli zostanie wyświetlony wyjątek bazy danych podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="671ae-351">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="671ae-352">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="671ae-352">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="671ae-353">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="671ae-353">Test the **Create** link.</span></span> <span data-ttu-id="671ae-354">Wprowadź i prześlij dane.</span><span class="sxs-lookup"><span data-stu-id="671ae-354">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="671ae-355">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="671ae-355">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="671ae-356">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="671ae-356">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="671ae-357">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="671ae-357">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="671ae-358">Przetestuj linki **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="671ae-358">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="671ae-359">Zapoznaj się z klasą `Startup`:</span><span class="sxs-lookup"><span data-stu-id="671ae-359">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="671ae-360">Poprzedni wyróżniony kod pokazuje kontekst bazy danych filmu dodawany do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="671ae-360">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="671ae-361">`services.AddDbContext<MvcMovieContext>(options =>` określa bazę danych, która ma być używana, oraz parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="671ae-361">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="671ae-362">`=>` jest [operatorem lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="671ae-362">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="671ae-363">Otwórz plik *controllers/MoviesController. cs* i zapoznaj się z konstruktorem:</span><span class="sxs-lookup"><span data-stu-id="671ae-363">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="671ae-364">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych (`MvcMovieContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="671ae-364">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="671ae-365">Kontekst bazy danych jest używany w każdej z metod [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="671ae-365">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="671ae-366">Modele silnie wpisane i @model słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="671ae-366">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="671ae-367">Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do widoku przy użyciu słownika `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="671ae-367">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="671ae-368">Słownik `ViewData` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="671ae-368">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="671ae-369">MVC oferuje również możliwość przekazywania obiektów modelu silnie typu do widoku.</span><span class="sxs-lookup"><span data-stu-id="671ae-369">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="671ae-370">Takie silnie wpisane podejście umożliwia lepsze sprawdzanie czasu kompilowania kodu.</span><span class="sxs-lookup"><span data-stu-id="671ae-370">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="671ae-371">Mechanizm tworzenia szkieletu używa tego podejścia (czyli przekazywania modelu silnie określonego typu) z klasą `MoviesController` i widokami podczas tworzenia metod i widoków.</span><span class="sxs-lookup"><span data-stu-id="671ae-371">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="671ae-372">Przejrzyj wygenerowaną metodę `Details` w pliku *controllers/MoviesController. cs* :</span><span class="sxs-lookup"><span data-stu-id="671ae-372">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="671ae-373">Parametr `id` jest zwykle przenoszona jako dane trasy.</span><span class="sxs-lookup"><span data-stu-id="671ae-373">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="671ae-374">Na przykład zestawy `https://localhost:5001/movies/details/1`:</span><span class="sxs-lookup"><span data-stu-id="671ae-374">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="671ae-375">Kontroler do kontrolera `movies` (pierwszy segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="671ae-375">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="671ae-376">Akcja do `details` (drugi segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="671ae-376">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="671ae-377">Identyfikator 1 (ostatni segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="671ae-377">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="671ae-378">Możesz również przekazać `id` za pomocą ciągu zapytania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="671ae-378">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="671ae-379">Parametr `id` jest zdefiniowany jako [typ dopuszczający wartość null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) w przypadku, gdy nie podano wartości identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="671ae-379">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="671ae-380">[Wyrażenie lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przesyłane do `FirstOrDefaultAsync`, aby wybrać jednostki filmu, które pasują do wartości danych trasy lub ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="671ae-380">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="671ae-381">Jeśli film zostanie znaleziony, wystąpienie modelu `Movie` jest przesyłane do widoku `Details`:</span><span class="sxs-lookup"><span data-stu-id="671ae-381">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="671ae-382">Zapoznaj się z zawartością pliku *widoki/filmy/szczegóły. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="671ae-382">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="671ae-383">Dołączając instrukcję `@model` w górnej części pliku widoku, można określić typ obiektu, którego oczekuje widok.</span><span class="sxs-lookup"><span data-stu-id="671ae-383">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="671ae-384">Po utworzeniu kontrolera filmu Poniższa instrukcja `@model` została automatycznie uwzględniona w górnej części pliku *details. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="671ae-384">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="671ae-385">Ta `@model` dyrektywa pozwala uzyskać dostęp do filmu, który kontroler przeszedł do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="671ae-385">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="671ae-386">Na przykład w widoku *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` i `DisplayFor` pomocników HTML z silnie wpisaną `Model` obiektem.</span><span class="sxs-lookup"><span data-stu-id="671ae-386">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="671ae-387">Metody `Create` i `Edit` i widoki również przekazują obiekt modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="671ae-387">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="671ae-388">Sprawdź widok *index. cshtml* i metodę `Index` w kontrolerze filmów.</span><span class="sxs-lookup"><span data-stu-id="671ae-388">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="671ae-389">Zwróć uwagę, jak kod tworzy obiekt `List`, gdy wywołuje metodę `View`.</span><span class="sxs-lookup"><span data-stu-id="671ae-389">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="671ae-390">Kod przekazuje tę `Movies` listę z metody akcji `Index` do widoku:</span><span class="sxs-lookup"><span data-stu-id="671ae-390">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="671ae-391">Podczas tworzenia kontrolera filmów zostanie automatycznie uwzględniona następująca instrukcja `@model` w górnej części pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="671ae-391">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="671ae-392">Dyrektywa `@model` pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="671ae-392">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="671ae-393">Na przykład, w widoku *index. cshtml* , kod przechodzi przez filmy z instrukcją `foreach` na obiekt `Model` o jednoznacznie określonym typie:</span><span class="sxs-lookup"><span data-stu-id="671ae-393">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="671ae-394">Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy element w pętli jest wpisywany jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="671ae-394">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="671ae-395">Dzięki temu możesz uzyskać kontrolę czasu kompilowania kodu:</span><span class="sxs-lookup"><span data-stu-id="671ae-395">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="671ae-396">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="671ae-396">Additional resources</span></span>

* [<span data-ttu-id="671ae-397">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="671ae-397">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="671ae-398">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="671ae-398">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="671ae-399">[Poprzednie dodanie widoku](adding-view.md)
> [następnym DZIAŁAniem z SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="671ae-399">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end
