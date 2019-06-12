---
title: Dodawanie modelu do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodawanie modelu do prostą aplikację platformy ASP.NET Core.
ms.author: riande
ms.date: 02/25/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: fcfe97e2228c21ffea8fe7ec5df8e2ea9c398828
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837326"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="b2d95-103">Dodawanie modelu do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="b2d95-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="b2d95-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b2d95-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="b2d95-105">W tej sekcji dodasz klasy zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b2d95-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="b2d95-106">Te klasy będzie "**M**odelu" wchodzi w skład **M**VC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2d95-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="b2d95-107">Użyj tych klas z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="b2d95-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="b2d95-108">EF Core to platforma mapowania obiektowo relacyjny (ORM), która upraszcza kod dostępu do danych, który trzeba napisać.</span><span class="sxs-lookup"><span data-stu-id="b2d95-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="b2d95-109">Klasy modeli, możesz utworzyć są nazywane klasami POCO (z **P**zwykły **O**ld **C**LR **O**biekty), ponieważ nie mają żadnych zależności EF Core.</span><span class="sxs-lookup"><span data-stu-id="b2d95-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="b2d95-110">Określają one po prostu właściwości danych, które będą przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b2d95-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="b2d95-111">W tym samouczku pisania klasy modeli i programem EF Core tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="b2d95-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="b2d95-112">Alternatywnym podejściu, nieuwzględnione w tym miejscu jest do generowania klasy modelu z istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b2d95-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="b2d95-113">Aby uzyskać informacji na temat tego podejścia, zobacz [ASP.NET Core — istniejąca baza danych](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="b2d95-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="b2d95-114">Dodaj klasę modelu danych</span><span class="sxs-lookup"><span data-stu-id="b2d95-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2d95-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2d95-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2d95-116">Kliknij prawym przyciskiem myszy *modeli* folder > **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="b2d95-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="b2d95-117">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="b2d95-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b2d95-118">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b2d95-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="b2d95-119">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="b2d95-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="b2d95-120">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="b2d95-120">Scaffold the movie model</span></span>

<span data-ttu-id="b2d95-121">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="b2d95-121">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="b2d95-122">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="b2d95-122">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2d95-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2d95-123">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2d95-124">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > Nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="b2d95-124">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

<span data-ttu-id="b2d95-126">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="b2d95-126">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Dodaj okno dialogowe Tworzenie szkieletu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="b2d95-128">Wykonaj **Dodaj kontroler** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="b2d95-128">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="b2d95-129">**Klasa modelu:** *Film (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="b2d95-129">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="b2d95-130">**Klasa kontekstu danych:** Wybierz **+** ikonę i Dodaj domyślny **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="b2d95-130">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Dodawanie kontekstu danych](adding-model/_static/dc.png)

* <span data-ttu-id="b2d95-132">**Widoki:** Zachowaj wartość domyślną każdego z zaznaczoną opcją</span><span class="sxs-lookup"><span data-stu-id="b2d95-132">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="b2d95-133">**Nazwa kontrolera:** Zachowaj wartość domyślną *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="b2d95-133">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="b2d95-134">Wybierz **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="b2d95-134">Select **Add**</span></span>

![Dodaj kontroler, okno dialogowe](adding-model/_static/add_controller2.png)

<span data-ttu-id="b2d95-136">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="b2d95-136">Visual Studio creates:</span></span>

* <span data-ttu-id="b2d95-137">Entity Framework Core [bazy danych klasy kontekstu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="b2d95-137">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="b2d95-138">Kontroler filmy (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="b2d95-138">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="b2d95-139">Pliki widoku razor dla stron Create, Delete, szczegółowe informacje, edycji i indeksu (*widoków/filmy/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="b2d95-139">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="b2d95-140">Automatyczne tworzenie kontekst bazy danych i [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki są określane jako *tworzenia szkieletów*.</span><span class="sxs-lookup"><span data-stu-id="b2d95-140">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b2d95-141">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b2d95-141">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="b2d95-142">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="b2d95-142">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="b2d95-143">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="b2d95-143">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="b2d95-144">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b2d95-144">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b2d95-145">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b2d95-145">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b2d95-146">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="b2d95-146">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="b2d95-147">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="b2d95-147">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="b2d95-148">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b2d95-148">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="b2d95-149">Jeśli Uruchom aplikację i kliknąć **filmu Mvc** link, zostanie wyświetlony komunikat o błędzie podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="b2d95-149">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

<span data-ttu-id="b2d95-150">Należy utworzyć bazę danych, a następnie użyj programu EF Core [migracje](xref:data/ef-mvc/migrations) funkcję, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="b2d95-150">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="b2d95-151">Migracje umożliwia tworzenie bazy danych, która pasuje do modelu danych i zaktualizować schemat bazy danych, gdy model danych, zmiany.</span><span class="sxs-lookup"><span data-stu-id="b2d95-151">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="b2d95-152">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="b2d95-152">Initial migration</span></span>

<span data-ttu-id="b2d95-153">W tej sekcji należy wykonać następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="b2d95-153">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="b2d95-154">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="b2d95-154">Add an initial migration.</span></span>
* <span data-ttu-id="b2d95-155">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="b2d95-155">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2d95-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2d95-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="b2d95-157">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="b2d95-157">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![Menu konsoli zarządzania Pakietami](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="b2d95-159">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b2d95-159">In the PMC, enter the following commands:</span></span>

   ```console
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="b2d95-160">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b2d95-160">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="b2d95-161">Schemat bazy danych zależy od określonego w modelu `MvcMovieContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="b2d95-161">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="b2d95-162">`Initial` Argument jest nazwą migracji.</span><span class="sxs-lookup"><span data-stu-id="b2d95-162">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="b2d95-163">Można dowolną nazwę, ale zgodnie z Konwencją, jest używany na nazwę opisującą migracji.</span><span class="sxs-lookup"><span data-stu-id="b2d95-163">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="b2d95-164">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="b2d95-164">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="b2d95-165">`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="b2d95-165">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b2d95-166">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b2d95-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="b2d95-167">`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b2d95-167">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="b2d95-168">Schemat bazy danych zależy od określonego w modelu `MvcMovieContext` klasy (w *Data/MvcMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="b2d95-168">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="b2d95-169">`InitialCreate` Argument jest nazwą migracji.</span><span class="sxs-lookup"><span data-stu-id="b2d95-169">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="b2d95-170">Można dowolną nazwę, ale zgodnie z Konwencją nazwa jest zaznaczona, opisujący migracji.</span><span class="sxs-lookup"><span data-stu-id="b2d95-170">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="b2d95-171">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="b2d95-171">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="b2d95-172">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b2d95-172">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b2d95-173">Usługi (takie jak kontekst bazy danych programu EF Core) są rejestrowane przy użyciu DI podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2d95-173">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="b2d95-174">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="b2d95-174">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="b2d95-175">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="b2d95-175">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2d95-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2d95-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2d95-177">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera DI.</span><span class="sxs-lookup"><span data-stu-id="b2d95-177">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="b2d95-178">Sprawdź następujące `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="b2d95-178">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="b2d95-179">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="b2d95-179">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="b2d95-180">`MvcMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="b2d95-180">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="b2d95-181">Kontekst danych (`MvcMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="b2d95-181">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="b2d95-182">Kontekst danych określa, które jednostki są uwzględnione w modelu danych:</span><span class="sxs-lookup"><span data-stu-id="b2d95-182">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="b2d95-183">Powyższy kod tworzy [DbSet\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="b2d95-183">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="b2d95-184">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b2d95-184">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="b2d95-185">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="b2d95-185">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="b2d95-186">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="b2d95-186">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="b2d95-187">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="b2d95-187">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b2d95-188">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b2d95-188">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="b2d95-189">Kontekst bazy danych utworzone i jest on zarejestrowany za pomocą kontenera DI.</span><span class="sxs-lookup"><span data-stu-id="b2d95-189">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="b2d95-190">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b2d95-190">Test the app</span></span>

* <span data-ttu-id="b2d95-191">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="b2d95-191">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="b2d95-192">Jeśli pojawi się wyjątek bazy danych podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="b2d95-192">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="b2d95-193">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="b2d95-193">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="b2d95-194">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="b2d95-194">Test the **Create** link.</span></span> <span data-ttu-id="b2d95-195">Wprowadź i przesyłania danych.</span><span class="sxs-lookup"><span data-stu-id="b2d95-195">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="b2d95-196">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="b2d95-196">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="b2d95-197">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="b2d95-197">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="b2d95-198">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="b2d95-198">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="b2d95-199">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="b2d95-199">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="b2d95-200">Sprawdź `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="b2d95-200">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="b2d95-201">Poprzedni kod wyróżniony pokazuje kontekst bazy danych filmów, które są dodawane do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera:</span><span class="sxs-lookup"><span data-stu-id="b2d95-201">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="b2d95-202">`services.AddDbContext<MvcMovieContext>(options =>` Określa bazę danych i parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="b2d95-202">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="b2d95-203">`=>` jest [operatora lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="b2d95-203">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="b2d95-204">Otwórz *Controllers/MoviesController.cs* plików i zbadaj konstruktora:</span><span class="sxs-lookup"><span data-stu-id="b2d95-204">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="b2d95-205">Używa konstruktora [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`MvcMovieContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b2d95-205">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="b2d95-206">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="b2d95-206">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="b2d95-207">Silnie typizowane modeli i @model — słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="b2d95-207">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="b2d95-208">Wcześniej w tym samouczku pokazano, jak kontroler można przekazać dane i obiekty za pomocą widoku `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="b2d95-208">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="b2d95-209">`ViewData` Słownik jest to obiekt dynamiczny, która zapewnia wygodny sposób z późnym wiązaniem do przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="b2d95-209">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="b2d95-210">MVC udostępnia również możliwość przekazywania silnie typizowanych obiektów modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="b2d95-210">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="b2d95-211">Silnie typizowane temu najlepszy czas kompilacji sprawdzania kodu.</span><span class="sxs-lookup"><span data-stu-id="b2d95-211">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="b2d95-212">Mechanizm tworzenia szkieletów używane takie podejście (który jest przekazując silnie typizowany model) z `MoviesController` klasy i widoki utworzenia metod i widoków.</span><span class="sxs-lookup"><span data-stu-id="b2d95-212">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="b2d95-213">Sprawdź wygenerowany `Details` method in Class metoda *Controllers/MoviesController.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="b2d95-213">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="b2d95-214">`id` Parametr ogólnie jest przekazywany jako dane trasy.</span><span class="sxs-lookup"><span data-stu-id="b2d95-214">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="b2d95-215">Na przykład `https://localhost:5001/movies/details/1` ustawia:</span><span class="sxs-lookup"><span data-stu-id="b2d95-215">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="b2d95-216">Kontroler do `movies` kontrolera (pierwszy segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="b2d95-216">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="b2d95-217">Działanie `details` (drugi segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="b2d95-217">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="b2d95-218">Identyfikator do 1 (ostatni segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="b2d95-218">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="b2d95-219">Możesz również przekazać `id` przy użyciu zapytania następujący ciąg:</span><span class="sxs-lookup"><span data-stu-id="b2d95-219">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="b2d95-220">`id` Parametr jest zdefiniowany jako [typu dopuszczającego wartość null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) w przypadku, gdy nie jest podana wartość Identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="b2d95-220">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="b2d95-221">A [wyrażenia lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przekazywany do `FirstOrDefaultAsync` do wybrania jednostki filmu, które odpowiada wartości ciągu danych lub zapytanie trasy.</span><span class="sxs-lookup"><span data-stu-id="b2d95-221">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="b2d95-222">Jeśli film zostanie znaleziony, wystąpienie `Movie` modelu jest przekazywany do `Details` widoku:</span><span class="sxs-lookup"><span data-stu-id="b2d95-222">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="b2d95-223">Sprawdź zawartość *Views/Movies/Details.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="b2d95-223">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="b2d95-224">Jeśli dołączysz `@model` instrukcji w górnej części pliku widoku, można określić typu obiektu, który oczekuje, że widok.</span><span class="sxs-lookup"><span data-stu-id="b2d95-224">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="b2d95-225">Podczas tworzenia kontrolera filmu, następujące `@model` instrukcja została automatycznie dołączane u góry *Details.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="b2d95-225">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="b2d95-226">To `@model` dyrektywy umożliwia dostęp do filmów, która kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="b2d95-226">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="b2d95-227">Na przykład w *Details.cshtml* widoku Kod przekazuje każdego pola film, aby `DisplayNameFor` i `DisplayFor` pomocników HTML za pomocą silnie typizowanej `Model` obiektu.</span><span class="sxs-lookup"><span data-stu-id="b2d95-227">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="b2d95-228">`Create` i `Edit` metody i widoki również przekazać `Movie` obiekt modelu.</span><span class="sxs-lookup"><span data-stu-id="b2d95-228">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="b2d95-229">Sprawdź *Index.cshtml* widoku i `Index` metody w kontrolerze filmów.</span><span class="sxs-lookup"><span data-stu-id="b2d95-229">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="b2d95-230">Zwróć uwagę, jak kod tworzy `List` obiektu, kiedy wywoływanych przez nią `View` metody.</span><span class="sxs-lookup"><span data-stu-id="b2d95-230">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="b2d95-231">Kod przekazuje to `Movies` listy z `Index` metody akcji do widoku:</span><span class="sxs-lookup"><span data-stu-id="b2d95-231">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="b2d95-232">Podczas tworzenia kontrolera filmy tworzenia szkieletów automatycznie uwzględnione następujące `@model` instrukcji na górze *Index.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="b2d95-232">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="b2d95-233">`@model` Dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="b2d95-233">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="b2d95-234">Na przykład w *Index.cshtml* wyświetlić kod pętlę filmów z `foreach` instrukcji na silnie typizowaną `Model` obiektu:</span><span class="sxs-lookup"><span data-stu-id="b2d95-234">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="b2d95-235">Ponieważ `Model` obiektu zdecydowanie jest wpisane (jako `IEnumerable<Movie>` obiektu), każdy element w pętli jest wpisana jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="b2d95-235">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="b2d95-236">Wśród innych korzyści oznacza to, uzyskasz czasie kompilacji sprawdzania kodu:</span><span class="sxs-lookup"><span data-stu-id="b2d95-236">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2d95-237">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b2d95-237">Additional resources</span></span>

* [<span data-ttu-id="b2d95-238">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="b2d95-238">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="b2d95-239">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="b2d95-239">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="b2d95-240">[Poprzednie, Dodawanie widoku](adding-view.md)
> [obok pracy przy użyciu języka SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="b2d95-240">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>
