---
title: Dodawanie modelu do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodaj model do prostej aplikacji ASP.NET Core.
ms.author: riande
ms.date: 01/13/2020
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 3fe22511b4d887177d86013d080f307e16361d5b
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172169"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="383c9-103">Dodawanie modelu do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="383c9-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="383c9-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="383c9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="383c9-105">W tej sekcji dodasz klasy do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="383c9-106">Te klasy będą częścią "**m**odelu" aplikacji **m**VC.</span><span class="sxs-lookup"><span data-stu-id="383c9-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="383c9-107">Te klasy są używane z [Entity Framework Core](/ef/core) (Ef Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="383c9-108">EF Core to struktura obiektu mapowania relacyjnego (ORM), która upraszcza kod dostępu do danych, który trzeba napisać.</span><span class="sxs-lookup"><span data-stu-id="383c9-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="383c9-109">Klasy modelu, które tworzysz, są nazywane klasami POCO ( **z Lain** **P**LR **o**biekty), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="383c9-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="383c9-110">Po prostu definiują właściwości danych, które będą przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="383c9-111">W tym samouczku najpierw napiszesz klasy modelu, a EF Core tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="383c9-112">Dodaj klasę modelu danych</span><span class="sxs-lookup"><span data-stu-id="383c9-112">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="383c9-114">Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="383c9-114">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="383c9-115">Nazwij plik *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="383c9-115">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="383c9-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="383c9-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="383c9-117">Dodaj plik o nazwie *Movie.cs* do folderu *models* .</span><span class="sxs-lookup"><span data-stu-id="383c9-117">Add a file named *Movie.cs* to the *Models* folder.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="383c9-118">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="383c9-119">Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** > **nową klasę** > **pustą klasę**.</span><span class="sxs-lookup"><span data-stu-id="383c9-119">Right-click the *Models* folder > **Add** > **New Class** > **Empty Class**.</span></span> <span data-ttu-id="383c9-120">Nazwij plik *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="383c9-120">Name the file *Movie.cs*.</span></span>

---

<span data-ttu-id="383c9-121">Zaktualizuj plik *Movie.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="383c9-121">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="383c9-122">Klasa `Movie` zawiera pole `Id`, które jest wymagane przez bazę danych klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="383c9-122">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="383c9-123">Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) w `ReleaseDate` określa typ danych (`Date`).</span><span class="sxs-lookup"><span data-stu-id="383c9-123">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="383c9-124">Z tym atrybutem:</span><span class="sxs-lookup"><span data-stu-id="383c9-124">With this attribute:</span></span>

* <span data-ttu-id="383c9-125">Użytkownik nie musi wprowadzać informacji o czasie w polu Data.</span><span class="sxs-lookup"><span data-stu-id="383c9-125">The user is not required to enter time information in the date field.</span></span>
* <span data-ttu-id="383c9-126">Tylko data jest wyświetlana, a nie informacje o czasie.</span><span class="sxs-lookup"><span data-stu-id="383c9-126">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="383c9-127">[Adnotacje DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) są omówione w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="383c9-127">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="383c9-128">Dodaj pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="383c9-128">Add NuGet packages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-129">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="383c9-130">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet** > **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="383c9-130">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![Menu konsoli zarządzania Pakietami](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="383c9-132">W obszarze PMC Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="383c9-132">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="383c9-133">Poprzednie polecenie dodaje dostawcę SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="383c9-133">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="383c9-134">Pakiet dostawcy instaluje pakiet EF Core jako zależność.</span><span class="sxs-lookup"><span data-stu-id="383c9-134">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="383c9-135">Dodatkowe pakiety są instalowane automatycznie w kroku tworzenia szkieletu w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="383c9-135">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="383c9-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="383c9-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="383c9-137">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="383c9-138">W menu **projekt** wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="383c9-138">From the **Project** menu, select **Manage NuGet Packages**.</span></span>

<span data-ttu-id="383c9-139">W polu **wyszukiwania** w prawym górnym rogu wprowadź `Microsoft.EntityFrameworkCore.SQLite` i naciśnij klawisz **Return** , aby wyszukać.</span><span class="sxs-lookup"><span data-stu-id="383c9-139">In the **Search** field in the upper right, enter `Microsoft.EntityFrameworkCore.SQLite` and press the **Return** key to search.</span></span> <span data-ttu-id="383c9-140">Wybierz pasujący pakiet NuGet i naciśnij przycisk **Dodaj pakiet** .</span><span class="sxs-lookup"><span data-stu-id="383c9-140">Select the matching NuGet package and press the **Add Package** button.</span></span>

![Dodaj Entity Framework Core pakiet NuGet](~/tutorials/first-mvc-app-mac/adding-model/_static/add-nuget-packages.png)

<span data-ttu-id="383c9-142">Zostanie wyświetlone okno dialogowe **Wybieranie projektów** z wybranym projektem `MvcMovie`.</span><span class="sxs-lookup"><span data-stu-id="383c9-142">The **Select Projects** dialog will be displayed, with the `MvcMovie` project selected.</span></span> <span data-ttu-id="383c9-143">Naciśnij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="383c9-143">Press the **Ok** button.</span></span>

<span data-ttu-id="383c9-144">Zostanie wyświetlone okno dialogowe **akceptacji licencji** .</span><span class="sxs-lookup"><span data-stu-id="383c9-144">A **License Acceptance** dialog will be displayed.</span></span> <span data-ttu-id="383c9-145">Przejrzyj odpowiednie licencje, a następnie kliknij przycisk **Akceptuj** .</span><span class="sxs-lookup"><span data-stu-id="383c9-145">Review the licenses as desired, then click the **Accept** button.</span></span>

<span data-ttu-id="383c9-146">Powtórz powyższe kroki, aby zainstalować następujące pakiety NuGet:</span><span class="sxs-lookup"><span data-stu-id="383c9-146">Repeat the above steps to install the following NuGet packages:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.EntityFrameworkCore.Design`

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="383c9-147">Tworzenie klasy kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="383c9-147">Create a database context class</span></span>

<span data-ttu-id="383c9-148">Klasa kontekstu bazy danych jest wymagana do koordynowania funkcji EF Core (tworzenie, odczytywanie, aktualizowanie, usuwanie) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="383c9-148">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="383c9-149">Kontekst bazy danych pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) i określa jednostki, które mają zostać uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-149">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="383c9-150">Utwórz folder *danych* .</span><span class="sxs-lookup"><span data-stu-id="383c9-150">Create a *Data* folder.</span></span>

<span data-ttu-id="383c9-151">Dodaj plik *Data/MvcMovieContext. cs* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="383c9-151">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="383c9-152">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="383c9-152">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="383c9-153">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-153">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="383c9-154">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="383c9-154">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="383c9-155">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="383c9-155">Register the database context</span></span>

<span data-ttu-id="383c9-156">ASP.NET Core jest skompilowany przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="383c9-156">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="383c9-157">Usługi (takie jak kontekst EF Core DB) muszą być zarejestrowane przy użyciu funkcji "DI" podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="383c9-157">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="383c9-158">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="383c9-158">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="383c9-159">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="383c9-159">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="383c9-160">W tej sekcji rejestrujesz kontekst bazy danych przy użyciu funkcji DI Container.</span><span class="sxs-lookup"><span data-stu-id="383c9-160">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="383c9-161">Dodaj następujące instrukcje `using` w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="383c9-161">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="383c9-162">Dodaj następujący wyróżniony kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="383c9-162">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-163">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="383c9-164">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-164">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="383c9-165">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="383c9-165">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="383c9-166">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="383c9-166">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="383c9-167">Dodaj parametry połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="383c9-167">Add a database connection string</span></span>

<span data-ttu-id="383c9-168">Dodaj parametry połączenia do pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="383c9-168">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-169">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="383c9-170">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-170">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="383c9-171">Kompiluj projekt jako sprawdzenie błędów kompilatora.</span><span class="sxs-lookup"><span data-stu-id="383c9-171">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="383c9-172">Strony z filmem szkieletu</span><span class="sxs-lookup"><span data-stu-id="383c9-172">Scaffold movie pages</span></span>

<span data-ttu-id="383c9-173">Użyj narzędzia do tworzenia szkieletu, aby utworzyć strony z przykładem tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="383c9-173">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-174">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="383c9-175">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , **> Dodaj > nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="383c9-175">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

<span data-ttu-id="383c9-177">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler MVC z widokami przy użyciu Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="383c9-177">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Okno dialogowe Dodawanie szkieletu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="383c9-179">Ukończ okno dialogowe **Dodawanie kontrolera** :</span><span class="sxs-lookup"><span data-stu-id="383c9-179">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="383c9-180">**Model Class:** *Movie (MvcMovie. models)*</span><span class="sxs-lookup"><span data-stu-id="383c9-180">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="383c9-181">**Klasa kontekstu danych:** *MvcMovieContext (MvcMovie. Data)*</span><span class="sxs-lookup"><span data-stu-id="383c9-181">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![Dodaj kontekst danych](adding-model/_static/dc3.png)

* <span data-ttu-id="383c9-183">**Widoki:** Zachowaj wartość domyślną dla każdej zaznaczonej opcji</span><span class="sxs-lookup"><span data-stu-id="383c9-183">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="383c9-184">**Nazwa kontrolera:** Zachowaj domyślną *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="383c9-184">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="383c9-185">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="383c9-185">Select **Add**</span></span>

<span data-ttu-id="383c9-186">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="383c9-186">Visual Studio creates:</span></span>

* <span data-ttu-id="383c9-187">Kontroler filmów (*controllers/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="383c9-187">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="383c9-188">Pliki widoku Razor na potrzeby tworzenia, usuwania, szczegółów, edytowania i indeksowania stron (*widoki/filmy/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="383c9-188">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="383c9-189">Automatyczne tworzenie tych plików jest znane jako *rusztowania*.</span><span class="sxs-lookup"><span data-stu-id="383c9-189">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="383c9-190">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="383c9-190">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="383c9-191">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="383c9-191">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="383c9-192">W systemie Linux wyeksportuj ścieżkę narzędzia do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="383c9-192">On Linux, export the scaffold tool path:</span></span>

  ```console
  export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="383c9-193">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="383c9-193">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="383c9-194">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="383c9-195">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="383c9-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="383c9-196">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="383c9-196">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="383c9-197">Nie można jeszcze użyć stron szkieletowych, ponieważ baza danych nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="383c9-197">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="383c9-198">Jeśli uruchomisz aplikację i klikniesz link **aplikacji filmowej** , uzyskasz dostęp do *otwartej bazy danych* lub *nie ma takiej tabeli:* komunikat o błędzie filmu.</span><span class="sxs-lookup"><span data-stu-id="383c9-198">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="383c9-199">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="383c9-199">Initial migration</span></span>

<span data-ttu-id="383c9-200">Użyj funkcji [migracji](xref:data/ef-mvc/migrations) EF Core, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-200">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="383c9-201">Migracje to zestaw narzędzi umożliwiających tworzenie i aktualizowanie bazy danych w celu dopasowania jej do modelu danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-201">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-202">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="383c9-203">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet** > **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="383c9-203">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="383c9-204">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="383c9-204">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="383c9-205">`Add-Migration InitialCreate`: generuje *migrację/{timestamp} _InitialCreate. cs* pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="383c9-205">`Add-Migration InitialCreate`: Generates a *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="383c9-206">`InitialCreate` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="383c9-206">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="383c9-207">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="383c9-207">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="383c9-208">Ponieważ jest to pierwsza migracja, wygenerowana Klasa zawiera kod, aby utworzyć schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-208">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="383c9-209">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="383c9-209">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="383c9-210">`Update-Database`: aktualizuje bazę danych do najnowszej migracji, która została utworzona przez poprzednie polecenie.</span><span class="sxs-lookup"><span data-stu-id="383c9-210">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="383c9-211">To polecenie uruchamia metodę `Up` w pliku *migrations/{sygnatura czasowa} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-211">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="383c9-212">Polecenie aktualizacji bazy danych generuje następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="383c9-212">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="383c9-213">Nie określono typu dla kolumny dziesiętnej "price" w typie jednostki "Movie".</span><span class="sxs-lookup"><span data-stu-id="383c9-213">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="383c9-214">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="383c9-214">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="383c9-215">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="383c9-215">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="383c9-216">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="383c9-216">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="383c9-217">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-217">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="383c9-218">Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="383c9-218">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="383c9-219">`ef migrations add InitialCreate`: generuje *migrację/{timestamp} _InitialCreate. cs* pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="383c9-219">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="383c9-220">`InitialCreate` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="383c9-220">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="383c9-221">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="383c9-221">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="383c9-222">Ponieważ jest to pierwsza migracja, wygenerowana Klasa zawiera kod, aby utworzyć schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-222">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="383c9-223">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext` (w pliku *Data/MvcMovieContext. cs* ).</span><span class="sxs-lookup"><span data-stu-id="383c9-223">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="383c9-224">`ef database update`: aktualizuje bazę danych do najnowszej migracji, która została utworzona przez poprzednie polecenie.</span><span class="sxs-lookup"><span data-stu-id="383c9-224">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="383c9-225">To polecenie uruchamia metodę `Up` w pliku *migrations/{sygnatura czasowa} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-225">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [more information on the CLI for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="383c9-226">Klasa InitialCreate</span><span class="sxs-lookup"><span data-stu-id="383c9-226">The InitialCreate class</span></span>

<span data-ttu-id="383c9-227">Przejrzyj *migracje/{timestamp} _InitialCreate* pliku migracji CS:</span><span class="sxs-lookup"><span data-stu-id="383c9-227">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

<span data-ttu-id="383c9-228">Metoda `Up` tworzy tabelę filmów i konfiguruje `Id` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="383c9-228">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="383c9-229">Metoda `Down` przywraca zmiany schematu dokonane przez `Up` migracji.</span><span class="sxs-lookup"><span data-stu-id="383c9-229">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="383c9-230">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="383c9-230">Test the app</span></span>

* <span data-ttu-id="383c9-231">Uruchom aplikację i kliknij link **aplikacji filmowej** .</span><span class="sxs-lookup"><span data-stu-id="383c9-231">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="383c9-232">Jeśli zostanie wyświetlony wyjątek podobny do jednego z następujących:</span><span class="sxs-lookup"><span data-stu-id="383c9-232">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-233">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-233">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="383c9-234">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-234">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="383c9-235">Prawdopodobnie pominięto [krok migracji](#migration).</span><span class="sxs-lookup"><span data-stu-id="383c9-235">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="383c9-236">Przetestuj stronę **Tworzenie** .</span><span class="sxs-lookup"><span data-stu-id="383c9-236">Test the **Create** page.</span></span> <span data-ttu-id="383c9-237">Wprowadź i prześlij dane.</span><span class="sxs-lookup"><span data-stu-id="383c9-237">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="383c9-238">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="383c9-238">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="383c9-239">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="383c9-239">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="383c9-240">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="383c9-240">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="383c9-241">Przetestuj strony **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="383c9-241">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="383c9-242">Wstrzykiwanie zależności w kontrolerze</span><span class="sxs-lookup"><span data-stu-id="383c9-242">Dependency injection in the controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-243">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="383c9-244">Otwórz plik *controllers/MoviesController. cs* i zapoznaj się z konstruktorem:</span><span class="sxs-lookup"><span data-stu-id="383c9-244">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="383c9-245">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych (`MvcMovieContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="383c9-245">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="383c9-246">Kontekst bazy danych jest używany w każdej z metod [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="383c9-246">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="383c9-247">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-247">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="383c9-248">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych (`MvcMovieContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="383c9-248">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="383c9-249">Kontekst bazy danych jest używany w każdej z metod [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="383c9-249">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

### <a name="use-sqlite-for-development-sql-server-for-production"></a><span data-ttu-id="383c9-250">Używanie oprogramowania SQLite do programowania, SQL Server do produkcji</span><span class="sxs-lookup"><span data-stu-id="383c9-250">Use SQLite for development, SQL Server for production</span></span>

<span data-ttu-id="383c9-251">Po wybraniu oprogramowania SQLite kod wygenerowany przez szablon jest gotowy do programowania.</span><span class="sxs-lookup"><span data-stu-id="383c9-251">When SQLite is selected, the template generated code is ready for development.</span></span> <span data-ttu-id="383c9-252">Poniższy kod pokazuje, jak wstrzyknąć <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="383c9-252">The following code shows how to inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into Startup.</span></span> <span data-ttu-id="383c9-253">`IWebHostEnvironment` jest wprowadzany, więc `ConfigureServices` mogą używać oprogramowania SQLite w programowaniu i SQL Server w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="383c9-253">`IWebHostEnvironment` is injected so `ConfigureServices` can use SQLite in development and SQL Server in production.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/StartupDevProd.cs?name=snippet_StartupClass&highlight=5,10,16-28)]

---
<!-- end of tabs --->

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="383c9-254">Modele silnie wpisane i @model słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="383c9-254">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="383c9-255">Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do widoku przy użyciu słownika `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="383c9-255">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="383c9-256">Słownik `ViewData` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="383c9-256">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="383c9-257">MVC oferuje również możliwość przekazywania obiektów modelu silnie typu do widoku.</span><span class="sxs-lookup"><span data-stu-id="383c9-257">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="383c9-258">Takie silnie wpisane podejście umożliwia sprawdzanie kodu czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="383c9-258">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="383c9-259">Mechanizm tworzenia szkieletu używa tego podejścia (czyli przekazania silnie określonego modelu) z klasą `MoviesController` i widokami.</span><span class="sxs-lookup"><span data-stu-id="383c9-259">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="383c9-260">Przejrzyj wygenerowaną metodę `Details` w pliku *controllers/MoviesController. cs* :</span><span class="sxs-lookup"><span data-stu-id="383c9-260">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="383c9-261">Parametr `id` jest zwykle przenoszona jako dane trasy.</span><span class="sxs-lookup"><span data-stu-id="383c9-261">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="383c9-262">Na przykład zestawy `https://localhost:5001/movies/details/1`:</span><span class="sxs-lookup"><span data-stu-id="383c9-262">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="383c9-263">Kontroler do kontrolera `movies` (pierwszy segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="383c9-263">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="383c9-264">Akcja do `details` (drugi segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="383c9-264">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="383c9-265">Identyfikator 1 (ostatni segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="383c9-265">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="383c9-266">Możesz również przekazać `id` za pomocą ciągu zapytania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="383c9-266">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="383c9-267">Parametr `id` jest zdefiniowany jako [typ dopuszczający wartość null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) w przypadku, gdy nie podano wartości identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="383c9-267">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="383c9-268">[Wyrażenie lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przesyłane do `FirstOrDefaultAsync`, aby wybrać jednostki filmu, które pasują do wartości danych trasy lub ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="383c9-268">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="383c9-269">Jeśli film zostanie znaleziony, wystąpienie modelu `Movie` jest przesyłane do widoku `Details`:</span><span class="sxs-lookup"><span data-stu-id="383c9-269">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
```

<span data-ttu-id="383c9-270">Zapoznaj się z zawartością pliku *widoki/filmy/szczegóły. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="383c9-270">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="383c9-271">Instrukcja `@model` w górnej części pliku widoku określa typ obiektu, który jest oczekiwany przez widok.</span><span class="sxs-lookup"><span data-stu-id="383c9-271">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="383c9-272">Gdy kontroler filmu został utworzony, dołączono następującą `@model` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="383c9-272">When the movie controller was created, the following `@model` statement was included:</span></span>

```cshtml
@model MvcMovie.Models.Movie
```

<span data-ttu-id="383c9-273">Ta `@model` dyrektywa zezwala na dostęp do filmu, który kontroler przeszedł do widoku.</span><span class="sxs-lookup"><span data-stu-id="383c9-273">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="383c9-274">Obiekt `Model` jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="383c9-274">The `Model` object is strongly typed.</span></span> <span data-ttu-id="383c9-275">Na przykład w widoku *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` i `DisplayFor` pomocników HTML z silnie wpisaną `Model` obiektem.</span><span class="sxs-lookup"><span data-stu-id="383c9-275">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="383c9-276">Metody `Create` i `Edit` i widoki również przekazują obiekt modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="383c9-276">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="383c9-277">Sprawdź widok *index. cshtml* i metodę `Index` w kontrolerze filmów.</span><span class="sxs-lookup"><span data-stu-id="383c9-277">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="383c9-278">Zwróć uwagę, jak kod tworzy obiekt `List`, gdy wywołuje metodę `View`.</span><span class="sxs-lookup"><span data-stu-id="383c9-278">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="383c9-279">Kod przekazuje tę `Movies` listę z metody akcji `Index` do widoku:</span><span class="sxs-lookup"><span data-stu-id="383c9-279">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="383c9-280">Gdy kontroler filmów został utworzony, funkcja tworzenia szkieletów zawiera następującą instrukcję `@model` w górnej części pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="383c9-280">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="383c9-281">Dyrektywa `@model` pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="383c9-281">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="383c9-282">Na przykład, w widoku *index. cshtml* , kod przechodzi przez filmy z instrukcją `foreach` na obiekt `Model` o jednoznacznie określonym typie:</span><span class="sxs-lookup"><span data-stu-id="383c9-282">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="383c9-283">Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy element w pętli jest wpisywany jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="383c9-283">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="383c9-284">Dzięki temu możesz uzyskać kontrolę czasu kompilowania kodu.</span><span class="sxs-lookup"><span data-stu-id="383c9-284">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="383c9-285">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="383c9-285">Additional resources</span></span>

* [<span data-ttu-id="383c9-286">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="383c9-286">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="383c9-287">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="383c9-287">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="383c9-288">[Poprzednie dodanie widoku](adding-view.md)
> [następnym DZIAŁAniem z SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="383c9-288">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="383c9-289">Dodaj klasę modelu danych</span><span class="sxs-lookup"><span data-stu-id="383c9-289">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-290">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-290">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="383c9-291">Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="383c9-291">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="383c9-292">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="383c9-292">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="383c9-293">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-293">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="383c9-294">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="383c9-294">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="383c9-295">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="383c9-295">Scaffold the movie model</span></span>

<span data-ttu-id="383c9-296">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="383c9-296">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="383c9-297">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="383c9-297">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-298">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-298">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="383c9-299">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , **> Dodaj > nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="383c9-299">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

<span data-ttu-id="383c9-301">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler MVC z widokami przy użyciu Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="383c9-301">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Okno dialogowe Dodawanie szkieletu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="383c9-303">Ukończ okno dialogowe **Dodawanie kontrolera** :</span><span class="sxs-lookup"><span data-stu-id="383c9-303">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="383c9-304">**Model Class:** *Movie (MvcMovie. models)*</span><span class="sxs-lookup"><span data-stu-id="383c9-304">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="383c9-305">**Klasa kontekstu danych:** Wybierz ikonę **+** i Dodaj domyślną **MvcMovie. models. MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="383c9-305">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Dodaj kontekst danych](adding-model/_static/dc.png)

* <span data-ttu-id="383c9-307">**Widoki:** Zachowaj wartość domyślną dla każdej zaznaczonej opcji</span><span class="sxs-lookup"><span data-stu-id="383c9-307">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="383c9-308">**Nazwa kontrolera:** Zachowaj domyślną *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="383c9-308">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="383c9-309">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="383c9-309">Select **Add**</span></span>

![Okno dialogowe Dodawanie kontrolera](adding-model/_static/add_controller2.png)

<span data-ttu-id="383c9-311">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="383c9-311">Visual Studio creates:</span></span>

* <span data-ttu-id="383c9-312">[Klasa kontekstu bazy danych](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext. cs*)</span><span class="sxs-lookup"><span data-stu-id="383c9-312">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="383c9-313">Kontroler filmów (*controllers/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="383c9-313">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="383c9-314">Pliki widoku Razor na potrzeby tworzenia, usuwania, szczegółów, edytowania i indeksowania stron (*widoki/filmy/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="383c9-314">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="383c9-315">Automatyczne tworzenie kontekstu bazy danych i metod akcji [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenie, odczytywanie, aktualizowanie i usuwanie) jest znane jako *rusztowanie*.</span><span class="sxs-lookup"><span data-stu-id="383c9-315">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="383c9-316">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="383c9-316">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="383c9-317">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="383c9-317">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="383c9-318">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="383c9-318">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="383c9-319">W systemie Linux wyeksportuj ścieżkę narzędzia do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="383c9-319">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="383c9-320">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="383c9-320">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="383c9-321">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-321">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="383c9-322">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="383c9-322">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="383c9-323">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="383c9-323">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="383c9-324">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="383c9-324">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="383c9-325">Jeśli uruchomisz aplikację i klikniesz link do **filmu MVC** , zostanie wyświetlony komunikat o błędzie podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="383c9-325">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-326">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-326">Visual Studio</span></span>](#tab/visual-studio)

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="383c9-327">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-327">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="383c9-328">Należy utworzyć bazę danych i użyć funkcji [migracji](xref:data/ef-mvc/migrations) EF Core, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="383c9-328">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="383c9-329">Migracja umożliwia utworzenie bazy danych zgodnej z modelem danych i zaktualizowanie schematu bazy danych, gdy zmieni się model danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-329">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="383c9-330">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="383c9-330">Initial migration</span></span>

<span data-ttu-id="383c9-331">W tej sekcji zostały wykonane następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="383c9-331">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="383c9-332">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="383c9-332">Add an initial migration.</span></span>
* <span data-ttu-id="383c9-333">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="383c9-333">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-334">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-334">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="383c9-335">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet** > **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="383c9-335">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![Menu konsoli zarządzania Pakietami](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="383c9-337">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="383c9-337">In the PMC, enter the following commands:</span></span>

   ```powershell
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="383c9-338">Polecenie `Add-Migration` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-338">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="383c9-339">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="383c9-339">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="383c9-340">`Initial` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="383c9-340">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="383c9-341">Można użyć dowolnej nazwy, ale według Konwencji, nazwy opisującej migrację.</span><span class="sxs-lookup"><span data-stu-id="383c9-341">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="383c9-342">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="383c9-342">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="383c9-343">`Update-Database` polecenie uruchamia metodę `Up` w pliku *migracji/{sygnatura czasowa} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-343">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="383c9-344">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-344">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="383c9-345">Polecenie `ef migrations add InitialCreate` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-345">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="383c9-346">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext` (w pliku *Data/MvcMovieContext. cs* ).</span><span class="sxs-lookup"><span data-stu-id="383c9-346">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="383c9-347">`InitialCreate` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="383c9-347">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="383c9-348">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="383c9-348">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="383c9-349">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="383c9-349">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="383c9-350">ASP.NET Core jest skompilowany przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="383c9-350">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="383c9-351">Usługi (takie jak kontekst EF Core DB) są rejestrowane przy użyciu funkcji "DI" podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="383c9-351">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="383c9-352">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="383c9-352">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="383c9-353">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="383c9-353">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383c9-354">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383c9-354">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="383c9-355">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu DI kontenera.</span><span class="sxs-lookup"><span data-stu-id="383c9-355">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="383c9-356">Przeanalizuj poniższą metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="383c9-356">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="383c9-357">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="383c9-357">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="383c9-358">`MvcMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="383c9-358">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="383c9-359">Kontekst danych (`MvcMovieContext`) pochodzi od elementu [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="383c9-359">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="383c9-360">Kontekst danych określa, które jednostki są uwzględnione w modelu danych:</span><span class="sxs-lookup"><span data-stu-id="383c9-360">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="383c9-361">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="383c9-361">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="383c9-362">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="383c9-362">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="383c9-363">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="383c9-363">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="383c9-364">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="383c9-364">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="383c9-365">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="383c9-365">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="383c9-366">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="383c9-366">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="383c9-367">Utworzono kontekst bazy danych i zarejestrowano go przy użyciu DI Container.</span><span class="sxs-lookup"><span data-stu-id="383c9-367">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="383c9-368">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="383c9-368">Test the app</span></span>

* <span data-ttu-id="383c9-369">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="383c9-369">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="383c9-370">Jeśli zostanie wyświetlony wyjątek bazy danych podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="383c9-370">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="383c9-371">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="383c9-371">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="383c9-372">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="383c9-372">Test the **Create** link.</span></span> <span data-ttu-id="383c9-373">Wprowadź i prześlij dane.</span><span class="sxs-lookup"><span data-stu-id="383c9-373">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="383c9-374">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="383c9-374">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="383c9-375">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="383c9-375">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="383c9-376">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="383c9-376">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="383c9-377">Przetestuj linki **Edytuj**, **Szczegóły** i **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="383c9-377">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="383c9-378">Zapoznaj się z klasą `Startup`:</span><span class="sxs-lookup"><span data-stu-id="383c9-378">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="383c9-379">Poprzedni wyróżniony kod pokazuje kontekst bazy danych filmu dodawany do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="383c9-379">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="383c9-380">`services.AddDbContext<MvcMovieContext>(options =>` określa bazę danych, która ma być używana, oraz parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="383c9-380">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="383c9-381">`=>` jest [operatorem lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="383c9-381">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="383c9-382">Otwórz plik *controllers/MoviesController. cs* i zapoznaj się z konstruktorem:</span><span class="sxs-lookup"><span data-stu-id="383c9-382">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="383c9-383">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych (`MvcMovieContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="383c9-383">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="383c9-384">Kontekst bazy danych jest używany w każdej z metod [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="383c9-384">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="383c9-385">Modele silnie wpisane i @model słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="383c9-385">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="383c9-386">Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do widoku przy użyciu słownika `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="383c9-386">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="383c9-387">Słownik `ViewData` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="383c9-387">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="383c9-388">MVC oferuje również możliwość przekazywania obiektów modelu silnie typu do widoku.</span><span class="sxs-lookup"><span data-stu-id="383c9-388">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="383c9-389">Takie silnie wpisane podejście umożliwia lepsze sprawdzanie czasu kompilowania kodu.</span><span class="sxs-lookup"><span data-stu-id="383c9-389">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="383c9-390">Mechanizm tworzenia szkieletu używa tego podejścia (czyli przekazywania modelu silnie określonego typu) z klasą `MoviesController` i widokami podczas tworzenia metod i widoków.</span><span class="sxs-lookup"><span data-stu-id="383c9-390">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="383c9-391">Przejrzyj wygenerowaną metodę `Details` w pliku *controllers/MoviesController. cs* :</span><span class="sxs-lookup"><span data-stu-id="383c9-391">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="383c9-392">Parametr `id` jest zwykle przenoszona jako dane trasy.</span><span class="sxs-lookup"><span data-stu-id="383c9-392">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="383c9-393">Na przykład zestawy `https://localhost:5001/movies/details/1`:</span><span class="sxs-lookup"><span data-stu-id="383c9-393">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="383c9-394">Kontroler do kontrolera `movies` (pierwszy segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="383c9-394">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="383c9-395">Akcja do `details` (drugi segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="383c9-395">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="383c9-396">Identyfikator 1 (ostatni segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="383c9-396">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="383c9-397">Możesz również przekazać `id` za pomocą ciągu zapytania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="383c9-397">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="383c9-398">Parametr `id` jest zdefiniowany jako [typ dopuszczający wartość null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) w przypadku, gdy nie podano wartości identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="383c9-398">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="383c9-399">[Wyrażenie lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przesyłane do `FirstOrDefaultAsync`, aby wybrać jednostki filmu, które pasują do wartości danych trasy lub ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="383c9-399">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="383c9-400">Jeśli film zostanie znaleziony, wystąpienie modelu `Movie` jest przesyłane do widoku `Details`:</span><span class="sxs-lookup"><span data-stu-id="383c9-400">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="383c9-401">Zapoznaj się z zawartością pliku *widoki/filmy/szczegóły. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="383c9-401">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="383c9-402">Dołączając instrukcję `@model` w górnej części pliku widoku, można określić typ obiektu, którego oczekuje widok.</span><span class="sxs-lookup"><span data-stu-id="383c9-402">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="383c9-403">Po utworzeniu kontrolera filmu Poniższa instrukcja `@model` została automatycznie uwzględniona w górnej części pliku *details. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="383c9-403">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```cshtml
@model MvcMovie.Models.Movie
```

<span data-ttu-id="383c9-404">Ta `@model` dyrektywa pozwala uzyskać dostęp do filmu, który kontroler przeszedł do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="383c9-404">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="383c9-405">Na przykład w widoku *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` i `DisplayFor` pomocników HTML z silnie wpisaną `Model` obiektem.</span><span class="sxs-lookup"><span data-stu-id="383c9-405">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="383c9-406">Metody `Create` i `Edit` i widoki również przekazują obiekt modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="383c9-406">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="383c9-407">Sprawdź widok *index. cshtml* i metodę `Index` w kontrolerze filmów.</span><span class="sxs-lookup"><span data-stu-id="383c9-407">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="383c9-408">Zwróć uwagę, jak kod tworzy obiekt `List`, gdy wywołuje metodę `View`.</span><span class="sxs-lookup"><span data-stu-id="383c9-408">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="383c9-409">Kod przekazuje tę `Movies` listę z metody akcji `Index` do widoku:</span><span class="sxs-lookup"><span data-stu-id="383c9-409">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="383c9-410">Podczas tworzenia kontrolera filmów zostanie automatycznie uwzględniona następująca instrukcja `@model` w górnej części pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="383c9-410">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="383c9-411">Dyrektywa `@model` pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="383c9-411">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="383c9-412">Na przykład, w widoku *index. cshtml* , kod przechodzi przez filmy z instrukcją `foreach` na obiekt `Model` o jednoznacznie określonym typie:</span><span class="sxs-lookup"><span data-stu-id="383c9-412">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="383c9-413">Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy element w pętli jest wpisywany jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="383c9-413">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="383c9-414">Dzięki temu możesz uzyskać kontrolę czasu kompilowania kodu:</span><span class="sxs-lookup"><span data-stu-id="383c9-414">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="383c9-415">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="383c9-415">Additional resources</span></span>

* [<span data-ttu-id="383c9-416">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="383c9-416">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="383c9-417">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="383c9-417">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="383c9-418">[Poprzednie dodanie widoku](adding-view.md)
> [następnym działaniem z bazą danych](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="383c9-418">[Previous Adding a View](adding-view.md)
[Next Working with a database](working-with-sql.md)</span></span>

::: moniker-end
