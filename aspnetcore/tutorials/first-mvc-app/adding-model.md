---
title: Dodawanie modelu do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodaj model do prostej aplikacji ASP.NET Core.
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 038ea8cf7c72e4aaca6e06c0208d3dd1d5597577
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022461"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="e4911-103">Dodawanie modelu do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e4911-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="e4911-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e4911-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="e4911-105">W tej sekcji dodasz klasy do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="e4911-106">Te klasy będą częścią "**m**odelu" aplikacji **m**VC.</span><span class="sxs-lookup"><span data-stu-id="e4911-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="e4911-107">Te klasy są używane z [Entity Framework Core](/ef/core) (Ef Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="e4911-108">EF Core to struktura obiektu mapowania relacyjnego (ORM), która upraszcza kod dostępu do danych, który trzeba napisać.</span><span class="sxs-lookup"><span data-stu-id="e4911-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="e4911-109">Klasy modelu, które tworzysz, są nazywane klasami POCO (z Lain **P**LR **o**biekty), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="e4911-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="e4911-110">Po prostu definiują właściwości danych, które będą przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="e4911-111">W tym samouczku najpierw napiszesz klasy modelu, a EF Core tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="e4911-112">Alternatywnym podejściem nieopisanym w tym miejscu jest wygenerowanie klas modelu z istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="e4911-113">Aby uzyskać informacje na temat tego podejścia, zobacz [ASP.NET Core — istniejąca baza danych](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="e4911-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="e4911-114">Dodaj klasę modelu danych</span><span class="sxs-lookup"><span data-stu-id="e4911-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4911-116">Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** > **klasę**.</span><span class="sxs-lookup"><span data-stu-id="e4911-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="e4911-117">Nazwij plik *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="e4911-117">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4911-118">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e4911-119">Dodaj plik o nazwie *Movie.cs* do folderu *models* .</span><span class="sxs-lookup"><span data-stu-id="e4911-119">Add a file named *Movie.cs* to the *Models* folder.</span></span>

---

<span data-ttu-id="e4911-120">Zaktualizuj plik *Movie.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="e4911-120">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="e4911-121">`Movie` Klasa`Id` zawiera pole, które jest wymagane przez bazę danych klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="e4911-121">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="e4911-122">Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) on `ReleaseDate` określa typ danych (`Date`).</span><span class="sxs-lookup"><span data-stu-id="e4911-122">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="e4911-123">Z tym atrybutem:</span><span class="sxs-lookup"><span data-stu-id="e4911-123">With this attribute:</span></span>

  * <span data-ttu-id="e4911-124">Użytkownik nie musi wprowadzać informacji o czasie w polu Data.</span><span class="sxs-lookup"><span data-stu-id="e4911-124">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="e4911-125">Tylko data jest wyświetlana, a nie informacje o czasie.</span><span class="sxs-lookup"><span data-stu-id="e4911-125">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="e4911-126">[Adnotacje](/dotnet/api/system.componentmodel.dataannotations) DataAnnotations są omówione w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="e4911-126">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="e4911-127">Dodaj pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="e4911-127">Add NuGet packages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-128">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4911-129">W menu **Narzędzia** wybierz kolejno pozycje > **Menedżer pakietów NuGet** **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="e4911-129">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![Menu konsoli zarządzania Pakietami](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="e4911-131">W obszarze PMC Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e4911-131">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer -IncludePrerelease
```

<span data-ttu-id="e4911-132">Poprzednie polecenie dodaje dostawcę SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="e4911-132">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="e4911-133">Pakiet dostawcy instaluje pakiet EF Core jako zależność.</span><span class="sxs-lookup"><span data-stu-id="e4911-133">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="e4911-134">Dodatkowe pakiety są instalowane automatycznie w kroku tworzenia szkieletu w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="e4911-134">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4911-135">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-135">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e4911-136">Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="e4911-136">Run the following .NET Core CLI commands:</span></span>

```console
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="e4911-137">Powyższe polecenia powodują dodanie:</span><span class="sxs-lookup"><span data-stu-id="e4911-137">The preceding commands add:</span></span>

* <span data-ttu-id="e4911-138">Entity Framework Core narzędzia dla interfejsu wiersza polecenia platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="e4911-138">The Entity Framework Core Tools for the .NET CLI.</span></span>
* <span data-ttu-id="e4911-139">EF Core dostawca oprogramowania SQLite, który instaluje pakiet EF Core jako zależność.</span><span class="sxs-lookup"><span data-stu-id="e4911-139">The EF Core SQLite provider, which installs the EF Core package as a dependency.</span></span>
* <span data-ttu-id="e4911-140">Pakiety wymagające tworzenia szkieletów `Microsoft.VisualStudio.Web.CodeGeneration.Design` : `Microsoft.EntityFrameworkCore.SqlServer`i.</span><span class="sxs-lookup"><span data-stu-id="e4911-140">Packages needed for scaffolding: `Microsoft.VisualStudio.Web.CodeGeneration.Design` and `Microsoft.EntityFrameworkCore.SqlServer`.</span></span>

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="e4911-141">Tworzenie klasy kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="e4911-141">Create a database context class</span></span>

<span data-ttu-id="e4911-142">Klasa kontekstu bazy danych jest wymagana do koordynowania funkcji EF Core (tworzenie, odczytywanie, aktualizowanie, usuwanie) `Movie` dla modelu.</span><span class="sxs-lookup"><span data-stu-id="e4911-142">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="e4911-143">Kontekst bazy danych pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) i określa jednostki, które mają zostać uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-143">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="e4911-144">Utwórz folder *danych* .</span><span class="sxs-lookup"><span data-stu-id="e4911-144">Create a *Data* folder.</span></span>

<span data-ttu-id="e4911-145">Dodaj plik *Data/MvcMovieContext. cs* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="e4911-145">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="e4911-146">Poprzedni kod tworzy [\<nieogólnymi > film](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="e4911-146">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="e4911-147">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-147">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="e4911-148">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="e4911-148">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="e4911-149">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="e4911-149">Register the database context</span></span>

<span data-ttu-id="e4911-150">ASP.NET Core jest skompilowany przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e4911-150">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e4911-151">Usługi (takie jak kontekst EF Core DB) muszą być zarejestrowane przy użyciu funkcji "DI" podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4911-151">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="e4911-152">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e4911-152">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="e4911-153">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="e4911-153">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="e4911-154">W tej sekcji rejestrujesz kontekst bazy danych przy użyciu funkcji DI Container.</span><span class="sxs-lookup"><span data-stu-id="e4911-154">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="e4911-155">Dodaj następujące `using` instrukcje w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4911-155">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="e4911-156">Dodaj następujący wyróżniony kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e4911-156">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-157">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4911-158">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="e4911-159">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="e4911-159">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="e4911-160">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="e4911-160">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="e4911-161">Dodaj parametry połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="e4911-161">Add a database connection string</span></span>

<span data-ttu-id="e4911-162">Dodaj parametry połączenia do pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="e4911-162">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-163">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4911-164">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-164">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="e4911-165">Kompiluj projekt jako sprawdzenie błędów kompilatora.</span><span class="sxs-lookup"><span data-stu-id="e4911-165">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="e4911-166">Strony z filmem szkieletu</span><span class="sxs-lookup"><span data-stu-id="e4911-166">Scaffold movie pages</span></span>

<span data-ttu-id="e4911-167">Użyj narzędzia do tworzenia szkieletu, aby utworzyć strony z przykładem tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="e4911-167">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4911-169">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder controllers, **> Dodaj > nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="e4911-169">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

<span data-ttu-id="e4911-171">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler MVC z widokami przy użyciu Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="e4911-171">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Okno dialogowe Dodawanie szkieletu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="e4911-173">Ukończ okno dialogowe **Dodawanie kontrolera** :</span><span class="sxs-lookup"><span data-stu-id="e4911-173">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="e4911-174">**Klasa modelu:** *Film (MvcMovie. models)*</span><span class="sxs-lookup"><span data-stu-id="e4911-174">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="e4911-175">**Klasa kontekstu danych:** *MvcMovieContext (MvcMovie. Data)*</span><span class="sxs-lookup"><span data-stu-id="e4911-175">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![Dodaj kontekst danych](adding-model/_static/dc3.png)

* <span data-ttu-id="e4911-177">**Widoki:** Zachowaj wartość domyślną dla każdej zaznaczonej opcji</span><span class="sxs-lookup"><span data-stu-id="e4911-177">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="e4911-178">**Nazwa kontrolera:** Zachowaj domyślną *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="e4911-178">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="e4911-179">Wybierz pozycję **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="e4911-179">Select **Add**</span></span>

<span data-ttu-id="e4911-180">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="e4911-180">Visual Studio creates:</span></span>

* <span data-ttu-id="e4911-181">Kontroler filmów (*controllers/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="e4911-181">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="e4911-182">Pliki widoku Razor na potrzeby tworzenia, usuwania, szczegółów, edytowania i indeksowania stron (*widoki/filmy/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="e4911-182">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="e4911-183">Automatyczne tworzenie tych plików jest znane jako rusztowania.</span><span class="sxs-lookup"><span data-stu-id="e4911-183">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e4911-184">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4911-184">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="e4911-185">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="e4911-185">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="e4911-186">W systemie Linux wyeksportuj ścieżkę narzędzia do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="e4911-186">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="e4911-187">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e4911-187">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e4911-188">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-188">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e4911-189">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="e4911-189">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="e4911-190">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e4911-190">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="e4911-191">Nie można jeszcze użyć stron szkieletowych, ponieważ baza danych nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="e4911-191">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="e4911-192">Jeśli uruchomisz aplikację i klikniesz link **aplikacji filmowej** , uzyskasz dostęp do *otwartej bazy danych* lub *nie ma takiej tabeli: Komunikat* o błędzie filmu.</span><span class="sxs-lookup"><span data-stu-id="e4911-192">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="e4911-193">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="e4911-193">Initial migration</span></span>

<span data-ttu-id="e4911-194">Użyj funkcji [migracji](xref:data/ef-mvc/migrations) EF Core, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-194">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="e4911-195">Migracje to zestaw narzędzi umożliwiających tworzenie i aktualizowanie bazy danych w celu dopasowania jej do modelu danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-195">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-196">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4911-197">W menu **Narzędzia** wybierz kolejno pozycje > **Menedżer pakietów NuGet** **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="e4911-197">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="e4911-198">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e4911-198">In the PMC, enter the following commands:</span></span>

```console
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="e4911-199">`Add-Migration InitialCreate`: Generuje plik migracji *_InitialCreate. cs migracji/{timestamp}* .</span><span class="sxs-lookup"><span data-stu-id="e4911-199">`Add-Migration InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="e4911-200">`InitialCreate` Argument jest nazwą migracji.</span><span class="sxs-lookup"><span data-stu-id="e4911-200">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="e4911-201">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="e4911-201">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="e4911-202">Ponieważ jest to pierwsza migracja, wygenerowana Klasa zawiera kod, aby utworzyć schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-202">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="e4911-203">Schemat bazy danych jest oparty na modelu określonym w `MvcMovieContext` klasie.</span><span class="sxs-lookup"><span data-stu-id="e4911-203">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="e4911-204">`Update-Database`: Aktualizuje bazę danych do najnowszej migracji, która została utworzona przez poprzednie polecenie.</span><span class="sxs-lookup"><span data-stu-id="e4911-204">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="e4911-205">To polecenie uruchamia `Up` metodę w pliku *migrations/{Time-sygnatura} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-205">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="e4911-206">Polecenie aktualizacji bazy danych generuje następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="e4911-206">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="e4911-207">Nie określono typu dla kolumny dziesiętnej "price" w typie jednostki "Movie".</span><span class="sxs-lookup"><span data-stu-id="e4911-207">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="e4911-208">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="e4911-208">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="e4911-209">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="e4911-209">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="e4911-210">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="e4911-210">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4911-211">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-211">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e4911-212">Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="e4911-212">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="e4911-213">`ef migrations add InitialCreate`: Generuje plik migracji *_InitialCreate. cs migracji/{timestamp}* .</span><span class="sxs-lookup"><span data-stu-id="e4911-213">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="e4911-214">`InitialCreate` Argument jest nazwą migracji.</span><span class="sxs-lookup"><span data-stu-id="e4911-214">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="e4911-215">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="e4911-215">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="e4911-216">Ponieważ jest to pierwsza migracja, wygenerowana Klasa zawiera kod, aby utworzyć schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-216">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="e4911-217">Schemat bazy danych jest oparty na modelu określonym w `MvcMovieContext` klasie (w pliku *Data/MvcMovieContext. cs* ).</span><span class="sxs-lookup"><span data-stu-id="e4911-217">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="e4911-218">`ef database update`: Aktualizuje bazę danych do najnowszej migracji, która została utworzona przez poprzednie polecenie.</span><span class="sxs-lookup"><span data-stu-id="e4911-218">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="e4911-219">To polecenie uruchamia `Up` metodę w pliku *migrations/{Time-sygnatura} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-219">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="e4911-220">Klasa InitialCreate</span><span class="sxs-lookup"><span data-stu-id="e4911-220">The InitialCreate class</span></span>

<span data-ttu-id="e4911-221">Zapoznaj się z plikiem migracji *_InitialCreate. cs migracji/{timestamp}* :</span><span class="sxs-lookup"><span data-stu-id="e4911-221">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 <span data-ttu-id="e4911-222">Metoda tworzy tabelę filmów i konfiguruje `Id` jako klucz podstawowy. `Up`</span><span class="sxs-lookup"><span data-stu-id="e4911-222">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="e4911-223">Metoda przywraca zmiany schematu wykonane `Up` podczas migracji. `Down`</span><span class="sxs-lookup"><span data-stu-id="e4911-223">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="e4911-224">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e4911-224">Test the app</span></span>

* <span data-ttu-id="e4911-225">Uruchom aplikację i kliknij link **aplikacji filmowej** .</span><span class="sxs-lookup"><span data-stu-id="e4911-225">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="e4911-226">Jeśli zostanie wyświetlony wyjątek podobny do jednego z następujących:</span><span class="sxs-lookup"><span data-stu-id="e4911-226">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-227">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4911-228">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="e4911-229">Prawdopodobnie pominięto [krok migracji](#migration).</span><span class="sxs-lookup"><span data-stu-id="e4911-229">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="e4911-230">Przetestuj stronę **Tworzenie** .</span><span class="sxs-lookup"><span data-stu-id="e4911-230">Test the **Create** page.</span></span> <span data-ttu-id="e4911-231">Wprowadź i prześlij dane.</span><span class="sxs-lookup"><span data-stu-id="e4911-231">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="e4911-232">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="e4911-232">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="e4911-233">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="e4911-233">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="e4911-234">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="e4911-234">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="e4911-235">Przetestuj strony **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="e4911-235">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="e4911-236">Wstrzykiwanie zależności w kontrolerze</span><span class="sxs-lookup"><span data-stu-id="e4911-236">Dependency injection in the controller</span></span>

<span data-ttu-id="e4911-237">Otwórz plik *controllers/MoviesController. cs* i zapoznaj się z konstruktorem:</span><span class="sxs-lookup"><span data-stu-id="e4911-237">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="e4911-238">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych`MvcMovieContext`() do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e4911-238">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="e4911-239">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="e4911-239">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="e4911-240">Modele silnie wpisane i @model słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="e4911-240">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="e4911-241">Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do widoku przy użyciu `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="e4911-241">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="e4911-242">`ViewData` Słownik jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="e4911-242">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="e4911-243">MVC oferuje również możliwość przekazywania obiektów modelu silnie typu do widoku.</span><span class="sxs-lookup"><span data-stu-id="e4911-243">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="e4911-244">Takie silnie wpisane podejście umożliwia sprawdzanie kodu czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e4911-244">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="e4911-245">Mechanizm tworzenia szkieletu używa tego podejścia (czyli przekazania silnie określonego modelu) z `MoviesController` klasą i widokami.</span><span class="sxs-lookup"><span data-stu-id="e4911-245">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="e4911-246">Przejrzyj wygenerowaną `Details` metodę w pliku *controllers/MoviesController. cs* :</span><span class="sxs-lookup"><span data-stu-id="e4911-246">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="e4911-247">`id` Parametr jest zazwyczaj przesyłany jako dane trasy.</span><span class="sxs-lookup"><span data-stu-id="e4911-247">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="e4911-248">Na przykład `https://localhost:5001/movies/details/1` zestawy:</span><span class="sxs-lookup"><span data-stu-id="e4911-248">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="e4911-249">Kontroler do `movies` kontrolera (pierwszy segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="e4911-249">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="e4911-250">Akcja do `details` (drugi segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="e4911-250">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="e4911-251">Identyfikator 1 (ostatni segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="e4911-251">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="e4911-252">Można również przekazać `id` za pomocą ciągu zapytania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e4911-252">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="e4911-253">Parametr jest zdefiniowany jako typ dopuszczający wartość`int?`null () w przypadku, gdy nie podano wartości identyfikatora. [](/dotnet/csharp/programming-guide/nullable-types/index) `id`</span><span class="sxs-lookup"><span data-stu-id="e4911-253">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="e4911-254">[Wyrażenie lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przesyłane do `FirstOrDefaultAsync` , aby wybrać jednostki filmu, które pasują do wartości danych trasy lub ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="e4911-254">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="e4911-255">Jeśli film zostanie znaleziony, wystąpienie `Movie` modelu jest przesyłane `Details` do widoku:</span><span class="sxs-lookup"><span data-stu-id="e4911-255">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="e4911-256">Zapoznaj się z zawartością pliku *widoki/filmy/szczegóły. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e4911-256">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="e4911-257">`@model` Instrukcja w górnej części pliku widoku określa typ obiektu, który jest oczekiwany przez widok.</span><span class="sxs-lookup"><span data-stu-id="e4911-257">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="e4911-258">Po utworzeniu kontrolera filmu uwzględniono następujące `@model` instrukcje:</span><span class="sxs-lookup"><span data-stu-id="e4911-258">When the movie controller was created, the following `@model` statement was included:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="e4911-259">Ta `@model` dyrektywa zezwala na dostęp do filmu, który kontroler przeszedł do widoku.</span><span class="sxs-lookup"><span data-stu-id="e4911-259">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="e4911-260">`Model` Obiekt ma silną wartość.</span><span class="sxs-lookup"><span data-stu-id="e4911-260">The `Model` object is strongly typed.</span></span> <span data-ttu-id="e4911-261">Na przykład w widoku Details *. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` `DisplayFor` pomocników HTML z obiektem o jednoznacznie określonym typie `Model` .</span><span class="sxs-lookup"><span data-stu-id="e4911-261">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="e4911-262">Metody `Create` i `Edit` iwidokirównieżprzekazują`Movie` obiekt modelu.</span><span class="sxs-lookup"><span data-stu-id="e4911-262">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="e4911-263">Sprawdź widok *index. cshtml* i `Index` metodę w kontrolerze filmów.</span><span class="sxs-lookup"><span data-stu-id="e4911-263">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="e4911-264">Zwróć uwagę, jak kod tworzy `List` obiekt, gdy `View` wywołuje metodę.</span><span class="sxs-lookup"><span data-stu-id="e4911-264">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="e4911-265">Kod przekazuje tę `Movies` listę `Index` z metody akcji do widoku:</span><span class="sxs-lookup"><span data-stu-id="e4911-265">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="e4911-266">Gdy kontroler filmów został utworzony, szkielet zawiera następujące `@model` instrukcje w górnej części pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e4911-266">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="e4911-267">Dyrektywa pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy `Model` użyciu jednoznacznie określonego obiektu. `@model`</span><span class="sxs-lookup"><span data-stu-id="e4911-267">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="e4911-268">Na przykład w widoku *index. cshtml* kod przechodzi przez filmy z `foreach` instrukcją względem obiektu silnie wpisanego: `Model`</span><span class="sxs-lookup"><span data-stu-id="e4911-268">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="e4911-269">Ponieważ obiekt jest silnie określony ( `IEnumerable<Movie>` jako obiekt), każdy element w pętli jest wpisywany jako `Movie`. `Model`</span><span class="sxs-lookup"><span data-stu-id="e4911-269">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="e4911-270">Dzięki temu możesz uzyskać kontrolę czasu kompilowania kodu.</span><span class="sxs-lookup"><span data-stu-id="e4911-270">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4911-271">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e4911-271">Additional resources</span></span>

* [<span data-ttu-id="e4911-272">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="e4911-272">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="e4911-273">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="e4911-273">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="e4911-274">[Poprzednie dodanie widoku](adding-view.md)
> [następnym razem z programem SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="e4911-274">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="e4911-275">Dodaj klasę modelu danych</span><span class="sxs-lookup"><span data-stu-id="e4911-275">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-276">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-276">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4911-277">Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** > **klasę**.</span><span class="sxs-lookup"><span data-stu-id="e4911-277">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="e4911-278">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="e4911-278">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4911-279">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-279">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e4911-280">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="e4911-280">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="e4911-281">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="e4911-281">Scaffold the movie model</span></span>

<span data-ttu-id="e4911-282">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="e4911-282">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="e4911-283">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="e4911-283">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-284">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-284">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4911-285">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder controllers, **> Dodaj > nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="e4911-285">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

<span data-ttu-id="e4911-287">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler MVC z widokami przy użyciu Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="e4911-287">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Okno dialogowe Dodawanie szkieletu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="e4911-289">Ukończ okno dialogowe **Dodawanie kontrolera** :</span><span class="sxs-lookup"><span data-stu-id="e4911-289">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="e4911-290">**Klasa modelu:** *Film (MvcMovie. models)*</span><span class="sxs-lookup"><span data-stu-id="e4911-290">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="e4911-291">**Klasa kontekstu danych:** Wybierz ikonę i Dodaj domyślną **MvcMovie. models. MvcMovieContext** **+**</span><span class="sxs-lookup"><span data-stu-id="e4911-291">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Dodaj kontekst danych](adding-model/_static/dc.png)

* <span data-ttu-id="e4911-293">**Widoki:** Zachowaj wartość domyślną dla każdej zaznaczonej opcji</span><span class="sxs-lookup"><span data-stu-id="e4911-293">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="e4911-294">**Nazwa kontrolera:** Zachowaj domyślną *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="e4911-294">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="e4911-295">Wybierz pozycję **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="e4911-295">Select **Add**</span></span>

![Okno dialogowe Dodawanie kontrolera](adding-model/_static/add_controller2.png)

<span data-ttu-id="e4911-297">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="e4911-297">Visual Studio creates:</span></span>

* <span data-ttu-id="e4911-298">[Klasa kontekstu bazy danych](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext. cs*)</span><span class="sxs-lookup"><span data-stu-id="e4911-298">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="e4911-299">Kontroler filmów (*controllers/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="e4911-299">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="e4911-300">Pliki widoku Razor na potrzeby tworzenia, usuwania, szczegółów, edytowania i indeksowania stron (*widoki/filmy/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="e4911-300">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="e4911-301">Automatyczne tworzenie kontekstu bazy danych i metod akcji [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenie, odczytywanie, aktualizowanie i usuwanie) jest znane jako *rusztowanie*.</span><span class="sxs-lookup"><span data-stu-id="e4911-301">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e4911-302">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4911-302">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="e4911-303">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="e4911-303">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e4911-304">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="e4911-304">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="e4911-305">W systemie Linux wyeksportuj ścieżkę narzędzia do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="e4911-305">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="e4911-306">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e4911-306">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e4911-307">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-307">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e4911-308">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="e4911-308">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e4911-309">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="e4911-309">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="e4911-310">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e4911-310">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="e4911-311">Jeśli uruchomisz aplikację i klikniesz link do **filmu MVC** , zostanie wyświetlony komunikat o błędzie podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="e4911-311">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-312">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-312">Visual Studio</span></span>](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4911-313">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-313">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="e4911-314">Należy utworzyć bazę danych i użyć funkcji [migracji](xref:data/ef-mvc/migrations) EF Core, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="e4911-314">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="e4911-315">Migracja umożliwia utworzenie bazy danych zgodnej z modelem danych i zaktualizowanie schematu bazy danych, gdy zmieni się model danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-315">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="e4911-316">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="e4911-316">Initial migration</span></span>

<span data-ttu-id="e4911-317">W tej sekcji zostały wykonane następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="e4911-317">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="e4911-318">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="e4911-318">Add an initial migration.</span></span>
* <span data-ttu-id="e4911-319">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="e4911-319">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-320">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-320">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e4911-321">W menu **Narzędzia** wybierz kolejno pozycje > **Menedżer pakietów NuGet** **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="e4911-321">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![Menu konsoli zarządzania Pakietami](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="e4911-323">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e4911-323">In the PMC, enter the following commands:</span></span>

   ```console
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="e4911-324">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-324">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="e4911-325">Schemat bazy danych jest oparty na modelu określonym w `MvcMovieContext` klasie.</span><span class="sxs-lookup"><span data-stu-id="e4911-325">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="e4911-326">`Initial` Argument jest nazwą migracji.</span><span class="sxs-lookup"><span data-stu-id="e4911-326">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="e4911-327">Można użyć dowolnej nazwy, ale według Konwencji, nazwy opisującej migrację.</span><span class="sxs-lookup"><span data-stu-id="e4911-327">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="e4911-328">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="e4911-328">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="e4911-329">`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-329">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4911-330">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-330">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="e4911-331">`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-331">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="e4911-332">Schemat bazy danych jest oparty na modelu określonym w `MvcMovieContext` klasie (w pliku *Data/MvcMovieContext. cs* ).</span><span class="sxs-lookup"><span data-stu-id="e4911-332">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="e4911-333">`InitialCreate` Argument jest nazwą migracji.</span><span class="sxs-lookup"><span data-stu-id="e4911-333">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="e4911-334">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="e4911-334">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="e4911-335">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="e4911-335">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="e4911-336">ASP.NET Core jest skompilowany przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e4911-336">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e4911-337">Usługi (takie jak kontekst EF Core DB) są rejestrowane przy użyciu funkcji "DI" podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4911-337">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="e4911-338">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e4911-338">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="e4911-339">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="e4911-339">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4911-340">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4911-340">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4911-341">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu DI kontenera.</span><span class="sxs-lookup"><span data-stu-id="e4911-341">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="e4911-342">Przeanalizuj poniższą `Startup.ConfigureServices` metodę.</span><span class="sxs-lookup"><span data-stu-id="e4911-342">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="e4911-343">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="e4911-343">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="e4911-344">`MvcMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="e4911-344">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="e4911-345">Kontekst danych (`MvcMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="e4911-345">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="e4911-346">Kontekst danych określa, które jednostki są uwzględnione w modelu danych:</span><span class="sxs-lookup"><span data-stu-id="e4911-346">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="e4911-347">Poprzedni kod tworzy [\<nieogólnymi > film](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="e4911-347">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="e4911-348">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e4911-348">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="e4911-349">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="e4911-349">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="e4911-350">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="e4911-350">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="e4911-351">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="e4911-351">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4911-352">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="e4911-352">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e4911-353">Utworzono kontekst bazy danych i zarejestrowano go przy użyciu DI Container.</span><span class="sxs-lookup"><span data-stu-id="e4911-353">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="e4911-354">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e4911-354">Test the app</span></span>

* <span data-ttu-id="e4911-355">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="e4911-355">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="e4911-356">Jeśli zostanie wyświetlony wyjątek bazy danych podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="e4911-356">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="e4911-357">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="e4911-357">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="e4911-358">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="e4911-358">Test the **Create** link.</span></span> <span data-ttu-id="e4911-359">Wprowadź i prześlij dane.</span><span class="sxs-lookup"><span data-stu-id="e4911-359">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="e4911-360">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="e4911-360">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="e4911-361">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="e4911-361">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="e4911-362">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="e4911-362">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="e4911-363">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="e4911-363">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e4911-364">Zapoznaj `Startup` się z klasą:</span><span class="sxs-lookup"><span data-stu-id="e4911-364">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="e4911-365">Poprzedni wyróżniony kod pokazuje kontekst bazy danych filmu dodawany do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="e4911-365">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="e4911-366">`services.AddDbContext<MvcMovieContext>(options =>`Określa bazę danych, która ma być używana, oraz parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="e4911-366">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="e4911-367">`=>`jest [operatorem lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="e4911-367">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="e4911-368">Otwórz plik *controllers/MoviesController. cs* i zapoznaj się z konstruktorem:</span><span class="sxs-lookup"><span data-stu-id="e4911-368">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="e4911-369">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych`MvcMovieContext`() do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e4911-369">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="e4911-370">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="e4911-370">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="e4911-371">Modele silnie wpisane i @model słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="e4911-371">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="e4911-372">Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do widoku przy użyciu `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="e4911-372">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="e4911-373">`ViewData` Słownik jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="e4911-373">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="e4911-374">MVC oferuje również możliwość przekazywania obiektów modelu silnie typu do widoku.</span><span class="sxs-lookup"><span data-stu-id="e4911-374">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="e4911-375">Takie silnie wpisane podejście umożliwia lepsze sprawdzanie czasu kompilowania kodu.</span><span class="sxs-lookup"><span data-stu-id="e4911-375">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="e4911-376">Mechanizm tworzenia szkieletów korzysta z tego podejścia (czyli przekazywania modelu silnie określonego typu) z klasą i widokami, `MoviesController` gdy utworzyły metody i widoki.</span><span class="sxs-lookup"><span data-stu-id="e4911-376">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="e4911-377">Przejrzyj wygenerowaną `Details` metodę w pliku *controllers/MoviesController. cs* :</span><span class="sxs-lookup"><span data-stu-id="e4911-377">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="e4911-378">`id` Parametr jest zazwyczaj przesyłany jako dane trasy.</span><span class="sxs-lookup"><span data-stu-id="e4911-378">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="e4911-379">Na przykład `https://localhost:5001/movies/details/1` zestawy:</span><span class="sxs-lookup"><span data-stu-id="e4911-379">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="e4911-380">Kontroler do `movies` kontrolera (pierwszy segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="e4911-380">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="e4911-381">Akcja do `details` (drugi segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="e4911-381">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="e4911-382">Identyfikator 1 (ostatni segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="e4911-382">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="e4911-383">Można również przekazać `id` za pomocą ciągu zapytania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e4911-383">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="e4911-384">Parametr jest zdefiniowany jako typ dopuszczający wartość`int?`null () w przypadku, gdy nie podano wartości identyfikatora. [](/dotnet/csharp/programming-guide/nullable-types/index) `id`</span><span class="sxs-lookup"><span data-stu-id="e4911-384">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="e4911-385">[Wyrażenie lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przesyłane do `FirstOrDefaultAsync` , aby wybrać jednostki filmu, które pasują do wartości danych trasy lub ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="e4911-385">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="e4911-386">Jeśli film zostanie znaleziony, wystąpienie `Movie` modelu jest przesyłane `Details` do widoku:</span><span class="sxs-lookup"><span data-stu-id="e4911-386">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="e4911-387">Zapoznaj się z zawartością pliku *widoki/filmy/szczegóły. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e4911-387">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="e4911-388">Dołączając `@model` instrukcję w górnej części pliku widoku, można określić typ obiektu, którego oczekuje widok.</span><span class="sxs-lookup"><span data-stu-id="e4911-388">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="e4911-389">Po utworzeniu kontrolera filmu Poniższa `@model` instrukcja została automatycznie uwzględniona w górnej części pliku Details *. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e4911-389">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="e4911-390">Ta `@model` dyrektywa pozwala uzyskać dostęp do filmu, który kontroler przeszedł do widoku przy `Model` użyciu jednoznacznie określonego obiektu.</span><span class="sxs-lookup"><span data-stu-id="e4911-390">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="e4911-391">Na przykład w widoku Details *. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` `DisplayFor` pomocników HTML z obiektem o jednoznacznie określonym typie `Model` .</span><span class="sxs-lookup"><span data-stu-id="e4911-391">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="e4911-392">Metody `Create` i `Edit` iwidokirównieżprzekazują`Movie` obiekt modelu.</span><span class="sxs-lookup"><span data-stu-id="e4911-392">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="e4911-393">Sprawdź widok *index. cshtml* i `Index` metodę w kontrolerze filmów.</span><span class="sxs-lookup"><span data-stu-id="e4911-393">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="e4911-394">Zwróć uwagę, jak kod tworzy `List` obiekt, gdy `View` wywołuje metodę.</span><span class="sxs-lookup"><span data-stu-id="e4911-394">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="e4911-395">Kod przekazuje tę `Movies` listę `Index` z metody akcji do widoku:</span><span class="sxs-lookup"><span data-stu-id="e4911-395">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="e4911-396">Podczas tworzenia kontrolera filmów zostanie automatycznie uwzględniona następująca `@model` instrukcja w górnej części pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e4911-396">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="e4911-397">Dyrektywa pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy `Model` użyciu jednoznacznie określonego obiektu. `@model`</span><span class="sxs-lookup"><span data-stu-id="e4911-397">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="e4911-398">Na przykład w widoku *index. cshtml* kod przechodzi przez filmy z `foreach` instrukcją względem obiektu silnie wpisanego: `Model`</span><span class="sxs-lookup"><span data-stu-id="e4911-398">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="e4911-399">Ponieważ obiekt jest silnie określony ( `IEnumerable<Movie>` jako obiekt), każdy element w pętli jest wpisywany jako `Movie`. `Model`</span><span class="sxs-lookup"><span data-stu-id="e4911-399">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="e4911-400">Dzięki temu możesz uzyskać kontrolę czasu kompilowania kodu:</span><span class="sxs-lookup"><span data-stu-id="e4911-400">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4911-401">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e4911-401">Additional resources</span></span>

* [<span data-ttu-id="e4911-402">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="e4911-402">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="e4911-403">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="e4911-403">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="e4911-404">[Poprzednie dodanie widoku](adding-view.md)
> [następnym razem z programem SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="e4911-404">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end
