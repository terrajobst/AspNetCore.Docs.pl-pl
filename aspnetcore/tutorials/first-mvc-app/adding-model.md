---
title: Dodawanie modelu do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodaj model do prostej aplikacji ASP.NET Core.
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 40f26c8c2bf8d8aaec1da4ca2ff96cb45830914e
ms.sourcegitcommit: 57b85708f4cded99b8f008a69830cb104cd8e879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/13/2020
ms.locfileid: "75914175"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="5ade2-103">Dodawanie modelu do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="5ade2-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="5ade2-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5ade2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5ade2-105">W tej sekcji dodasz klasy do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="5ade2-106">Te klasy będą częścią "**m**odelu" aplikacji **m**VC.</span><span class="sxs-lookup"><span data-stu-id="5ade2-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="5ade2-107">Te klasy są używane z [Entity Framework Core](/ef/core) (Ef Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="5ade2-108">EF Core to struktura obiektu mapowania relacyjnego (ORM), która upraszcza kod dostępu do danych, który trzeba napisać.</span><span class="sxs-lookup"><span data-stu-id="5ade2-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="5ade2-109">Klasy modelu, które tworzysz, są nazywane klasami POCO ( **z Lain** **P**LR **o**biekty), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="5ade2-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="5ade2-110">Po prostu definiują właściwości danych, które będą przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="5ade2-111">W tym samouczku najpierw napiszesz klasy modelu, a EF Core tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="5ade2-112">Alternatywnym podejściem nieopisanym w tym miejscu jest wygenerowanie klas modelu z istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="5ade2-113">Aby uzyskać informacje na temat tego podejścia, zobacz [ASP.NET Core — istniejąca baza danych](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="5ade2-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="5ade2-114">Dodaj klasę modelu danych</span><span class="sxs-lookup"><span data-stu-id="5ade2-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5ade2-116">Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="5ade2-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="5ade2-117">Nazwij plik *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="5ade2-117">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5ade2-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5ade2-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5ade2-119">Dodaj plik o nazwie *Movie.cs* do folderu *models* .</span><span class="sxs-lookup"><span data-stu-id="5ade2-119">Add a file named *Movie.cs* to the *Models* folder.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5ade2-120">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5ade2-121">Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** > **nową klasę** > **pustą klasę**.</span><span class="sxs-lookup"><span data-stu-id="5ade2-121">Right-click the *Models* folder > **Add** > **New Class** > **Empty Class**.</span></span> <span data-ttu-id="5ade2-122">Nazwij plik *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="5ade2-122">Name the file *Movie.cs*.</span></span>

---

<span data-ttu-id="5ade2-123">Zaktualizuj plik *Movie.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="5ade2-123">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="5ade2-124">Klasa `Movie` zawiera pole `Id`, które jest wymagane przez bazę danych klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="5ade2-124">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="5ade2-125">Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) w `ReleaseDate` określa typ danych (`Date`).</span><span class="sxs-lookup"><span data-stu-id="5ade2-125">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="5ade2-126">Z tym atrybutem:</span><span class="sxs-lookup"><span data-stu-id="5ade2-126">With this attribute:</span></span>

  * <span data-ttu-id="5ade2-127">Użytkownik nie musi wprowadzać informacji o czasie w polu Data.</span><span class="sxs-lookup"><span data-stu-id="5ade2-127">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="5ade2-128">Tylko data jest wyświetlana, a nie informacje o czasie.</span><span class="sxs-lookup"><span data-stu-id="5ade2-128">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="5ade2-129">[Adnotacje DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) są omówione w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5ade2-129">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="5ade2-130">Dodaj pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="5ade2-130">Add NuGet packages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-131">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5ade2-132">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet** > **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="5ade2-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![Menu konsoli zarządzania Pakietami](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="5ade2-134">W obszarze PMC Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5ade2-134">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="5ade2-135">Poprzednie polecenie dodaje dostawcę SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="5ade2-135">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="5ade2-136">Pakiet dostawcy instaluje pakiet EF Core jako zależność.</span><span class="sxs-lookup"><span data-stu-id="5ade2-136">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="5ade2-137">Dodatkowe pakiety są instalowane automatycznie w kroku tworzenia szkieletu w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="5ade2-137">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5ade2-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5ade2-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5ade2-139">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5ade2-140">W menu **projekt** wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5ade2-140">From the **Project** menu, select **Manage Nuget Packages**.</span></span>

<span data-ttu-id="5ade2-141">W polu **wyszukiwania** w prawym górnym rogu wprowadź `Microsoft.EntityFrameworkCore.SQLite` i naciśnij klawisz **Return** , aby wyszukać.</span><span class="sxs-lookup"><span data-stu-id="5ade2-141">In the **Search** field in the upper right, enter `Microsoft.EntityFrameworkCore.SQLite` and press the **Return** key to search.</span></span> <span data-ttu-id="5ade2-142">Wybierz pasujący pakiet NuGet i naciśnij przycisk **Dodaj pakiet** .</span><span class="sxs-lookup"><span data-stu-id="5ade2-142">Select the matching NuGet package and press the **Add Package** button.</span></span>

![Dodaj Entity Framework Core pakiet NuGet](~/tutorials/first-mvc-app-mac/adding-model/_static/add-nuget-packages.png)

<span data-ttu-id="5ade2-144">Zostanie wyświetlone okno dialogowe **Wybieranie projektów** z wybranym projektem `MvcMovie`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-144">The **Select Projects** dialog will be displayed, with the `MvcMovie` project selected.</span></span> <span data-ttu-id="5ade2-145">Naciśnij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="5ade2-145">Press the **Ok** button.</span></span>

<span data-ttu-id="5ade2-146">Zostanie wyświetlone okno dialogowe **akceptacji licencji** .</span><span class="sxs-lookup"><span data-stu-id="5ade2-146">A **License Acceptance** dialog will be displayed.</span></span> <span data-ttu-id="5ade2-147">Przejrzyj odpowiednie licencje, a następnie kliknij przycisk **Akceptuj** .</span><span class="sxs-lookup"><span data-stu-id="5ade2-147">Review the licenses as desired, then click the **Accept** button.</span></span>

<span data-ttu-id="5ade2-148">Powtórz powyższe kroki, aby zainstalować następujące pakiety NuGet:</span><span class="sxs-lookup"><span data-stu-id="5ade2-148">Repeat the above steps to install the following NuGet packages:</span></span>
 * `Microsoft.VisualStudio.Web.CodeGeneration.Design`
 * `Microsoft.EntityFrameworkCore.SqlServer`
 * `Microsoft.EntityFrameworkCore.Design`

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="5ade2-149">Tworzenie klasy kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="5ade2-149">Create a database context class</span></span>

<span data-ttu-id="5ade2-150">Klasa kontekstu bazy danych jest wymagana do koordynowania funkcji EF Core (tworzenie, odczytywanie, aktualizowanie, usuwanie) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-150">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="5ade2-151">Kontekst bazy danych pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) i określa jednostki, które mają zostać uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-151">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="5ade2-152">Utwórz folder *danych* .</span><span class="sxs-lookup"><span data-stu-id="5ade2-152">Create a *Data* folder.</span></span>

<span data-ttu-id="5ade2-153">Dodaj plik *Data/MvcMovieContext. cs* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="5ade2-153">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="5ade2-154">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="5ade2-154">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="5ade2-155">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-155">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="5ade2-156">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="5ade2-156">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="5ade2-157">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="5ade2-157">Register the database context</span></span>

<span data-ttu-id="5ade2-158">ASP.NET Core jest skompilowany przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5ade2-158">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5ade2-159">Usługi (takie jak kontekst EF Core DB) muszą być zarejestrowane przy użyciu funkcji "DI" podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-159">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="5ade2-160">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="5ade2-160">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="5ade2-161">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="5ade2-161">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="5ade2-162">W tej sekcji rejestrujesz kontekst bazy danych przy użyciu funkcji DI Container.</span><span class="sxs-lookup"><span data-stu-id="5ade2-162">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="5ade2-163">Dodaj następujące instrukcje `using` w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ade2-163">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="5ade2-164">Dodaj następujący wyróżniony kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5ade2-164">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-165">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5ade2-166">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="5ade2-167">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="5ade2-167">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="5ade2-168">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="5ade2-168">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="5ade2-169">Dodaj parametry połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="5ade2-169">Add a database connection string</span></span>

<span data-ttu-id="5ade2-170">Dodaj parametry połączenia do pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="5ade2-170">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-171">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5ade2-172">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-172">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="5ade2-173">Kompiluj projekt jako sprawdzenie błędów kompilatora.</span><span class="sxs-lookup"><span data-stu-id="5ade2-173">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="5ade2-174">Strony z filmem szkieletu</span><span class="sxs-lookup"><span data-stu-id="5ade2-174">Scaffold movie pages</span></span>

<span data-ttu-id="5ade2-175">Użyj narzędzia do tworzenia szkieletu, aby utworzyć strony z przykładem tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="5ade2-175">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5ade2-177">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , **> Dodaj > nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="5ade2-177">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

<span data-ttu-id="5ade2-179">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler MVC z widokami przy użyciu Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5ade2-179">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Okno dialogowe Dodawanie szkieletu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="5ade2-181">Ukończ okno dialogowe **Dodawanie kontrolera** :</span><span class="sxs-lookup"><span data-stu-id="5ade2-181">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="5ade2-182">**Model Class:** *Movie (MvcMovie. models)*</span><span class="sxs-lookup"><span data-stu-id="5ade2-182">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="5ade2-183">**Klasa kontekstu danych:** *MvcMovieContext (MvcMovie. Data)*</span><span class="sxs-lookup"><span data-stu-id="5ade2-183">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![Dodaj kontekst danych](adding-model/_static/dc3.png)

* <span data-ttu-id="5ade2-185">**Widoki:** Zachowaj wartość domyślną dla każdej zaznaczonej opcji</span><span class="sxs-lookup"><span data-stu-id="5ade2-185">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="5ade2-186">**Nazwa kontrolera:** Zachowaj domyślną *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="5ade2-186">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="5ade2-187">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5ade2-187">Select **Add**</span></span>

<span data-ttu-id="5ade2-188">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="5ade2-188">Visual Studio creates:</span></span>

* <span data-ttu-id="5ade2-189">Kontroler filmów (*controllers/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="5ade2-189">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="5ade2-190">Pliki widoku Razor na potrzeby tworzenia, usuwania, szczegółów, edytowania i indeksowania stron (*widoki/filmy/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="5ade2-190">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="5ade2-191">Automatyczne tworzenie tych plików jest znane jako *rusztowania*.</span><span class="sxs-lookup"><span data-stu-id="5ade2-191">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5ade2-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5ade2-192">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="5ade2-193">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="5ade2-193">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="5ade2-194">W systemie Linux wyeksportuj ścieżkę narzędzia do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="5ade2-194">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="5ade2-195">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5ade2-195">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5ade2-196">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5ade2-197">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="5ade2-197">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="5ade2-198">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5ade2-198">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="5ade2-199">Nie można jeszcze użyć stron szkieletowych, ponieważ baza danych nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="5ade2-199">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="5ade2-200">Jeśli uruchomisz aplikację i klikniesz link **aplikacji filmowej** , uzyskasz dostęp do *otwartej bazy danych* lub *nie ma takiej tabeli:* komunikat o błędzie filmu.</span><span class="sxs-lookup"><span data-stu-id="5ade2-200">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="5ade2-201">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="5ade2-201">Initial migration</span></span>

<span data-ttu-id="5ade2-202">Użyj funkcji [migracji](xref:data/ef-mvc/migrations) EF Core, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-202">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="5ade2-203">Migracje to zestaw narzędzi umożliwiających tworzenie i aktualizowanie bazy danych w celu dopasowania jej do modelu danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-203">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-204">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5ade2-205">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet** > **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="5ade2-205">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="5ade2-206">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5ade2-206">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="5ade2-207">`Add-Migration InitialCreate`: generuje *migrację/{timestamp} _InitialCreate. cs* pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-207">`Add-Migration InitialCreate`: Generates a *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="5ade2-208">`InitialCreate` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-208">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="5ade2-209">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="5ade2-209">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="5ade2-210">Ponieważ jest to pierwsza migracja, wygenerowana Klasa zawiera kod, aby utworzyć schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-210">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="5ade2-211">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-211">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="5ade2-212">`Update-Database`: aktualizuje bazę danych do najnowszej migracji, która została utworzona przez poprzednie polecenie.</span><span class="sxs-lookup"><span data-stu-id="5ade2-212">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="5ade2-213">To polecenie uruchamia metodę `Up` w pliku *migrations/{sygnatura czasowa} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-213">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="5ade2-214">Polecenie aktualizacji bazy danych generuje następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="5ade2-214">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="5ade2-215">Nie określono typu dla kolumny dziesiętnej "price" w typie jednostki "Movie".</span><span class="sxs-lookup"><span data-stu-id="5ade2-215">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="5ade2-216">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="5ade2-216">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="5ade2-217">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="5ade2-217">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="5ade2-218">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5ade2-218">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5ade2-219">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-219">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="5ade2-220">Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="5ade2-220">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="5ade2-221">`ef migrations add InitialCreate`: generuje *migrację/{timestamp} _InitialCreate. cs* pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-221">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="5ade2-222">`InitialCreate` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-222">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="5ade2-223">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="5ade2-223">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="5ade2-224">Ponieważ jest to pierwsza migracja, wygenerowana Klasa zawiera kod, aby utworzyć schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-224">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="5ade2-225">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext` (w pliku *Data/MvcMovieContext. cs* ).</span><span class="sxs-lookup"><span data-stu-id="5ade2-225">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="5ade2-226">`ef database update`: aktualizuje bazę danych do najnowszej migracji, która została utworzona przez poprzednie polecenie.</span><span class="sxs-lookup"><span data-stu-id="5ade2-226">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="5ade2-227">To polecenie uruchamia metodę `Up` w pliku *migrations/{sygnatura czasowa} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-227">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="5ade2-228">Klasa InitialCreate</span><span class="sxs-lookup"><span data-stu-id="5ade2-228">The InitialCreate class</span></span>

<span data-ttu-id="5ade2-229">Przejrzyj *migracje/{timestamp} _InitialCreate* pliku migracji CS:</span><span class="sxs-lookup"><span data-stu-id="5ade2-229">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 <span data-ttu-id="5ade2-230">Metoda `Up` tworzy tabelę filmów i konfiguruje `Id` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="5ade2-230">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="5ade2-231">Metoda `Down` przywraca zmiany schematu dokonane przez `Up` migracji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-231">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="5ade2-232">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5ade2-232">Test the app</span></span>

* <span data-ttu-id="5ade2-233">Uruchom aplikację i kliknij link **aplikacji filmowej** .</span><span class="sxs-lookup"><span data-stu-id="5ade2-233">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="5ade2-234">Jeśli zostanie wyświetlony wyjątek podobny do jednego z następujących:</span><span class="sxs-lookup"><span data-stu-id="5ade2-234">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-235">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5ade2-236">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-236">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="5ade2-237">Prawdopodobnie pominięto [krok migracji](#migration).</span><span class="sxs-lookup"><span data-stu-id="5ade2-237">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="5ade2-238">Przetestuj stronę **Tworzenie** .</span><span class="sxs-lookup"><span data-stu-id="5ade2-238">Test the **Create** page.</span></span> <span data-ttu-id="5ade2-239">Wprowadź i prześlij dane.</span><span class="sxs-lookup"><span data-stu-id="5ade2-239">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="5ade2-240">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="5ade2-240">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="5ade2-241">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="5ade2-241">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="5ade2-242">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="5ade2-242">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="5ade2-243">Przetestuj strony **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="5ade2-243">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="5ade2-244">Wstrzykiwanie zależności w kontrolerze</span><span class="sxs-lookup"><span data-stu-id="5ade2-244">Dependency injection in the controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-245">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5ade2-246">Otwórz plik *controllers/MoviesController. cs* i zapoznaj się z konstruktorem:</span><span class="sxs-lookup"><span data-stu-id="5ade2-246">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="5ade2-247">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych (`MvcMovieContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5ade2-247">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="5ade2-248">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="5ade2-248">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5ade2-249">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-249">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="5ade2-250">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych (`MvcMovieContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5ade2-250">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="5ade2-251">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="5ade2-251">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---
<!-- end of tabs --->

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="5ade2-252">Modele silnie wpisane i @model słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="5ade2-252">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="5ade2-253">Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do widoku przy użyciu słownika `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-253">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="5ade2-254">Słownik `ViewData` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="5ade2-254">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="5ade2-255">MVC oferuje również możliwość przekazywania obiektów modelu silnie typu do widoku.</span><span class="sxs-lookup"><span data-stu-id="5ade2-255">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="5ade2-256">Takie silnie wpisane podejście umożliwia sprawdzanie kodu czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-256">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="5ade2-257">Mechanizm tworzenia szkieletu używa tego podejścia (czyli przekazania silnie określonego modelu) z klasą `MoviesController` i widokami.</span><span class="sxs-lookup"><span data-stu-id="5ade2-257">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="5ade2-258">Przejrzyj wygenerowaną metodę `Details` w pliku *controllers/MoviesController. cs* :</span><span class="sxs-lookup"><span data-stu-id="5ade2-258">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="5ade2-259">Parametr `id` jest zwykle przenoszona jako dane trasy.</span><span class="sxs-lookup"><span data-stu-id="5ade2-259">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="5ade2-260">Na przykład zestawy `https://localhost:5001/movies/details/1`:</span><span class="sxs-lookup"><span data-stu-id="5ade2-260">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="5ade2-261">Kontroler do kontrolera `movies` (pierwszy segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="5ade2-261">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="5ade2-262">Akcja do `details` (drugi segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="5ade2-262">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="5ade2-263">Identyfikator 1 (ostatni segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="5ade2-263">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="5ade2-264">Możesz również przekazać `id` za pomocą ciągu zapytania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="5ade2-264">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="5ade2-265">Parametr `id` jest zdefiniowany jako [typ dopuszczający wartość null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) w przypadku, gdy nie podano wartości identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="5ade2-265">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="5ade2-266">[Wyrażenie lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przesyłane do `FirstOrDefaultAsync`, aby wybrać jednostki filmu, które pasują do wartości danych trasy lub ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="5ade2-266">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="5ade2-267">Jeśli film zostanie znaleziony, wystąpienie modelu `Movie` jest przesyłane do widoku `Details`:</span><span class="sxs-lookup"><span data-stu-id="5ade2-267">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="5ade2-268">Zapoznaj się z zawartością pliku *widoki/filmy/szczegóły. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="5ade2-268">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="5ade2-269">Instrukcja `@model` w górnej części pliku widoku określa typ obiektu, który jest oczekiwany przez widok.</span><span class="sxs-lookup"><span data-stu-id="5ade2-269">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="5ade2-270">Gdy kontroler filmu został utworzony, dołączono następującą `@model` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="5ade2-270">When the movie controller was created, the following `@model` statement was included:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="5ade2-271">Ta `@model` dyrektywa zezwala na dostęp do filmu, który kontroler przeszedł do widoku.</span><span class="sxs-lookup"><span data-stu-id="5ade2-271">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="5ade2-272">Obiekt `Model` jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="5ade2-272">The `Model` object is strongly typed.</span></span> <span data-ttu-id="5ade2-273">Na przykład w widoku *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` i `DisplayFor` pomocników HTML z silnie wpisaną `Model` obiektem.</span><span class="sxs-lookup"><span data-stu-id="5ade2-273">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="5ade2-274">Metody `Create` i `Edit` i widoki również przekazują obiekt modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-274">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="5ade2-275">Sprawdź widok *index. cshtml* i metodę `Index` w kontrolerze filmów.</span><span class="sxs-lookup"><span data-stu-id="5ade2-275">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="5ade2-276">Zwróć uwagę, jak kod tworzy obiekt `List`, gdy wywołuje metodę `View`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-276">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="5ade2-277">Kod przekazuje tę `Movies` listę z metody akcji `Index` do widoku:</span><span class="sxs-lookup"><span data-stu-id="5ade2-277">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="5ade2-278">Gdy kontroler filmów został utworzony, funkcja tworzenia szkieletów zawiera następującą instrukcję `@model` w górnej części pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="5ade2-278">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="5ade2-279">Dyrektywa `@model` pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="5ade2-279">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="5ade2-280">Na przykład, w widoku *index. cshtml* , kod przechodzi przez filmy z instrukcją `foreach` na obiekt `Model` o jednoznacznie określonym typie:</span><span class="sxs-lookup"><span data-stu-id="5ade2-280">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="5ade2-281">Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy element w pętli jest wpisywany jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-281">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="5ade2-282">Dzięki temu możesz uzyskać kontrolę czasu kompilowania kodu.</span><span class="sxs-lookup"><span data-stu-id="5ade2-282">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5ade2-283">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5ade2-283">Additional resources</span></span>

* [<span data-ttu-id="5ade2-284">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="5ade2-284">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="5ade2-285">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="5ade2-285">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="5ade2-286">[Poprzednie dodanie widoku](adding-view.md)
> [następnym DZIAŁAniem z SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="5ade2-286">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="5ade2-287">Dodaj klasę modelu danych</span><span class="sxs-lookup"><span data-stu-id="5ade2-287">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-288">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-288">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5ade2-289">Kliknij prawym przyciskiem myszy folder *modele* > **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="5ade2-289">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="5ade2-290">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="5ade2-290">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5ade2-291">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-291">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="5ade2-292">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="5ade2-292">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="5ade2-293">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="5ade2-293">Scaffold the movie model</span></span>

<span data-ttu-id="5ade2-294">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="5ade2-294">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="5ade2-295">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="5ade2-295">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-296">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-296">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5ade2-297">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , **> Dodaj > nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="5ade2-297">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

<span data-ttu-id="5ade2-299">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler MVC z widokami przy użyciu Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5ade2-299">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Okno dialogowe Dodawanie szkieletu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="5ade2-301">Ukończ okno dialogowe **Dodawanie kontrolera** :</span><span class="sxs-lookup"><span data-stu-id="5ade2-301">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="5ade2-302">**Model Class:** *Movie (MvcMovie. models)*</span><span class="sxs-lookup"><span data-stu-id="5ade2-302">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="5ade2-303">**Klasa kontekstu danych:** Wybierz ikonę **+** i Dodaj domyślną **MvcMovie. models. MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="5ade2-303">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Dodaj kontekst danych](adding-model/_static/dc.png)

* <span data-ttu-id="5ade2-305">**Widoki:** Zachowaj wartość domyślną dla każdej zaznaczonej opcji</span><span class="sxs-lookup"><span data-stu-id="5ade2-305">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="5ade2-306">**Nazwa kontrolera:** Zachowaj domyślną *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="5ade2-306">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="5ade2-307">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5ade2-307">Select **Add**</span></span>

![Okno dialogowe Dodawanie kontrolera](adding-model/_static/add_controller2.png)

<span data-ttu-id="5ade2-309">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="5ade2-309">Visual Studio creates:</span></span>

* <span data-ttu-id="5ade2-310">[Klasa kontekstu bazy danych](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext. cs*)</span><span class="sxs-lookup"><span data-stu-id="5ade2-310">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="5ade2-311">Kontroler filmów (*controllers/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="5ade2-311">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="5ade2-312">Pliki widoku Razor na potrzeby tworzenia, usuwania, szczegółów, edytowania i indeksowania stron (*widoki/filmy/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="5ade2-312">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="5ade2-313">Automatyczne tworzenie kontekstu bazy danych i metod akcji [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenie, odczytywanie, aktualizowanie i usuwanie) jest znane jako *rusztowanie*.</span><span class="sxs-lookup"><span data-stu-id="5ade2-313">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5ade2-314">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5ade2-314">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="5ade2-315">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="5ade2-315">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5ade2-316">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="5ade2-316">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="5ade2-317">W systemie Linux wyeksportuj ścieżkę narzędzia do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="5ade2-317">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="5ade2-318">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5ade2-318">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5ade2-319">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5ade2-320">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="5ade2-320">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5ade2-321">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="5ade2-321">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="5ade2-322">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5ade2-322">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="5ade2-323">Jeśli uruchomisz aplikację i klikniesz link do **filmu MVC** , zostanie wyświetlony komunikat o błędzie podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="5ade2-323">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-324">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-324">Visual Studio</span></span>](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5ade2-325">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-325">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="5ade2-326">Należy utworzyć bazę danych i użyć funkcji [migracji](xref:data/ef-mvc/migrations) EF Core, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="5ade2-326">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="5ade2-327">Migracja umożliwia utworzenie bazy danych zgodnej z modelem danych i zaktualizowanie schematu bazy danych, gdy zmieni się model danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-327">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="5ade2-328">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="5ade2-328">Initial migration</span></span>

<span data-ttu-id="5ade2-329">W tej sekcji zostały wykonane następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="5ade2-329">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="5ade2-330">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-330">Add an initial migration.</span></span>
* <span data-ttu-id="5ade2-331">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-331">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-332">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-332">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="5ade2-333">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet** > **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="5ade2-333">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![Menu konsoli zarządzania Pakietami](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="5ade2-335">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5ade2-335">In the PMC, enter the following commands:</span></span>

   ```PMC
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="5ade2-336">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-336">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="5ade2-337">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-337">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="5ade2-338">`Initial` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-338">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="5ade2-339">Można użyć dowolnej nazwy, ale według Konwencji, nazwy opisującej migrację.</span><span class="sxs-lookup"><span data-stu-id="5ade2-339">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="5ade2-340">Aby uzyskać więcej informacji, zobacz temat <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="5ade2-340">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="5ade2-341">`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-341">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5ade2-342">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-342">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="5ade2-343">`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-343">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="5ade2-344">Schemat bazy danych jest oparty na modelu określonym w klasie `MvcMovieContext` (w pliku *Data/MvcMovieContext. cs* ).</span><span class="sxs-lookup"><span data-stu-id="5ade2-344">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="5ade2-345">`InitialCreate` argumentem jest nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-345">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="5ade2-346">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="5ade2-346">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="5ade2-347">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="5ade2-347">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="5ade2-348">ASP.NET Core jest skompilowany przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5ade2-348">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5ade2-349">Usługi (takie jak kontekst EF Core DB) są rejestrowane przy użyciu funkcji "DI" podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5ade2-349">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="5ade2-350">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="5ade2-350">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="5ade2-351">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="5ade2-351">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ade2-352">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ade2-352">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5ade2-353">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu DI kontenera.</span><span class="sxs-lookup"><span data-stu-id="5ade2-353">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="5ade2-354">Przeanalizuj poniższą metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-354">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="5ade2-355">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="5ade2-355">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="5ade2-356">`MvcMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="5ade2-356">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="5ade2-357">Kontekst danych (`MvcMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="5ade2-357">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="5ade2-358">Kontekst danych określa, które jednostki są uwzględnione w modelu danych:</span><span class="sxs-lookup"><span data-stu-id="5ade2-358">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="5ade2-359">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="5ade2-359">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="5ade2-360">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5ade2-360">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="5ade2-361">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="5ade2-361">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="5ade2-362">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="5ade2-362">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="5ade2-363">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="5ade2-363">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5ade2-364">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5ade2-364">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="5ade2-365">Utworzono kontekst bazy danych i zarejestrowano go przy użyciu DI Container.</span><span class="sxs-lookup"><span data-stu-id="5ade2-365">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="5ade2-366">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5ade2-366">Test the app</span></span>

* <span data-ttu-id="5ade2-367">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="5ade2-367">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="5ade2-368">Jeśli zostanie wyświetlony wyjątek bazy danych podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="5ade2-368">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="5ade2-369">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="5ade2-369">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="5ade2-370">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="5ade2-370">Test the **Create** link.</span></span> <span data-ttu-id="5ade2-371">Wprowadź i prześlij dane.</span><span class="sxs-lookup"><span data-stu-id="5ade2-371">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="5ade2-372">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="5ade2-372">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="5ade2-373">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="5ade2-373">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="5ade2-374">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="5ade2-374">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="5ade2-375">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="5ade2-375">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="5ade2-376">Zapoznaj się z klasą `Startup`:</span><span class="sxs-lookup"><span data-stu-id="5ade2-376">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="5ade2-377">Poprzedni wyróżniony kod pokazuje kontekst bazy danych filmu dodawany do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="5ade2-377">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="5ade2-378">`services.AddDbContext<MvcMovieContext>(options =>` określa bazę danych, która ma być używana, oraz parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="5ade2-378">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="5ade2-379">`=>` jest [operatorem lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="5ade2-379">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="5ade2-380">Otwórz plik *controllers/MoviesController. cs* i zapoznaj się z konstruktorem:</span><span class="sxs-lookup"><span data-stu-id="5ade2-380">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="5ade2-381">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) do iniekcji kontekstu bazy danych (`MvcMovieContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5ade2-381">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="5ade2-382">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="5ade2-382">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="5ade2-383">Modele silnie wpisane i @model słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="5ade2-383">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="5ade2-384">Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do widoku przy użyciu słownika `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-384">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="5ade2-385">Słownik `ViewData` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="5ade2-385">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="5ade2-386">MVC oferuje również możliwość przekazywania obiektów modelu silnie typu do widoku.</span><span class="sxs-lookup"><span data-stu-id="5ade2-386">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="5ade2-387">Takie silnie wpisane podejście umożliwia lepsze sprawdzanie czasu kompilowania kodu.</span><span class="sxs-lookup"><span data-stu-id="5ade2-387">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="5ade2-388">Mechanizm tworzenia szkieletu używa tego podejścia (czyli przekazywania modelu silnie określonego typu) z klasą `MoviesController` i widokami podczas tworzenia metod i widoków.</span><span class="sxs-lookup"><span data-stu-id="5ade2-388">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="5ade2-389">Przejrzyj wygenerowaną metodę `Details` w pliku *controllers/MoviesController. cs* :</span><span class="sxs-lookup"><span data-stu-id="5ade2-389">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="5ade2-390">Parametr `id` jest zwykle przenoszona jako dane trasy.</span><span class="sxs-lookup"><span data-stu-id="5ade2-390">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="5ade2-391">Na przykład zestawy `https://localhost:5001/movies/details/1`:</span><span class="sxs-lookup"><span data-stu-id="5ade2-391">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="5ade2-392">Kontroler do kontrolera `movies` (pierwszy segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="5ade2-392">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="5ade2-393">Akcja do `details` (drugi segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="5ade2-393">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="5ade2-394">Identyfikator 1 (ostatni segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="5ade2-394">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="5ade2-395">Możesz również przekazać `id` za pomocą ciągu zapytania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="5ade2-395">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="5ade2-396">Parametr `id` jest zdefiniowany jako [typ dopuszczający wartość null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) w przypadku, gdy nie podano wartości identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="5ade2-396">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="5ade2-397">[Wyrażenie lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przesyłane do `FirstOrDefaultAsync`, aby wybrać jednostki filmu, które pasują do wartości danych trasy lub ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="5ade2-397">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="5ade2-398">Jeśli film zostanie znaleziony, wystąpienie modelu `Movie` jest przesyłane do widoku `Details`:</span><span class="sxs-lookup"><span data-stu-id="5ade2-398">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="5ade2-399">Zapoznaj się z zawartością pliku *widoki/filmy/szczegóły. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="5ade2-399">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="5ade2-400">Dołączając instrukcję `@model` w górnej części pliku widoku, można określić typ obiektu, którego oczekuje widok.</span><span class="sxs-lookup"><span data-stu-id="5ade2-400">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="5ade2-401">Po utworzeniu kontrolera filmu Poniższa instrukcja `@model` została automatycznie uwzględniona w górnej części pliku *details. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="5ade2-401">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="5ade2-402">Ta `@model` dyrektywa pozwala uzyskać dostęp do filmu, który kontroler przeszedł do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="5ade2-402">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="5ade2-403">Na przykład w widoku *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` i `DisplayFor` pomocników HTML z silnie wpisaną `Model` obiektem.</span><span class="sxs-lookup"><span data-stu-id="5ade2-403">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="5ade2-404">Metody `Create` i `Edit` i widoki również przekazują obiekt modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-404">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="5ade2-405">Sprawdź widok *index. cshtml* i metodę `Index` w kontrolerze filmów.</span><span class="sxs-lookup"><span data-stu-id="5ade2-405">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="5ade2-406">Zwróć uwagę, jak kod tworzy obiekt `List`, gdy wywołuje metodę `View`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-406">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="5ade2-407">Kod przekazuje tę `Movies` listę z metody akcji `Index` do widoku:</span><span class="sxs-lookup"><span data-stu-id="5ade2-407">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="5ade2-408">Podczas tworzenia kontrolera filmów zostanie automatycznie uwzględniona następująca instrukcja `@model` w górnej części pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="5ade2-408">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="5ade2-409">Dyrektywa `@model` pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="5ade2-409">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="5ade2-410">Na przykład, w widoku *index. cshtml* , kod przechodzi przez filmy z instrukcją `foreach` na obiekt `Model` o jednoznacznie określonym typie:</span><span class="sxs-lookup"><span data-stu-id="5ade2-410">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="5ade2-411">Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy element w pętli jest wpisywany jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="5ade2-411">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="5ade2-412">Dzięki temu możesz uzyskać kontrolę czasu kompilowania kodu:</span><span class="sxs-lookup"><span data-stu-id="5ade2-412">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5ade2-413">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5ade2-413">Additional resources</span></span>

* [<span data-ttu-id="5ade2-414">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="5ade2-414">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="5ade2-415">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="5ade2-415">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="5ade2-416">[Poprzednie dodanie widoku](adding-view.md)
> [następnym działaniem z bazą danych](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="5ade2-416">[Previous Adding a View](adding-view.md)
[Next Working with a database](working-with-sql.md)</span></span>

::: moniker-end
