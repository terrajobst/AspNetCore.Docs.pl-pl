---
title: Dodawanie modelu do aplikacji Razor Pages w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać klasy do zarządzania filmami w bazie danych przy użyciu Entity Framework Core (EF Core).
ms.author: riande
ms.date: 11/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 312b3d4eb13eb04453bf0c3256fc362918157a45
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634171"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="cecea-103">Dodawanie modelu do aplikacji Razor Pages w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cecea-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="cecea-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cecea-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cecea-105">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="cecea-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="cecea-106">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="cecea-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="cecea-107">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="cecea-108">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="cecea-109">Klasy modelu są znane jako klasy POCO (z "zwykłych, starych obiektów CLR"), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="cecea-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="cecea-110">Definiują właściwości danych przechowywanych w bazie danych programu.</span><span class="sxs-lookup"><span data-stu-id="cecea-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="cecea-111">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="cecea-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cecea-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cecea-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cecea-113">Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="cecea-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="cecea-114">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="cecea-114">Name the folder *Models*.</span></span>

<span data-ttu-id="cecea-115">Kliknij prawym przyciskiem myszy folder *modele* .</span><span class="sxs-lookup"><span data-stu-id="cecea-115">Right click the *Models* folder.</span></span> <span data-ttu-id="cecea-116">Wybierz pozycję **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="cecea-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="cecea-117">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="cecea-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cecea-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cecea-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="cecea-119">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="cecea-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="cecea-120">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="cecea-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cecea-121">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cecea-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cecea-122">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="cecea-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="cecea-123">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="cecea-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="cecea-124">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="cecea-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="cecea-125">W oknie dialogowym **nowy plik** :</span><span class="sxs-lookup"><span data-stu-id="cecea-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="cecea-126">W lewym okienku wybierz pozycję **Ogólne** .</span><span class="sxs-lookup"><span data-stu-id="cecea-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="cecea-127">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="cecea-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="cecea-128">Nazwij **film** klasy i wybierz pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="cecea-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="cecea-129">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cecea-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="cecea-130">Tworzenie szkieletu modelu filmu</span><span class="sxs-lookup"><span data-stu-id="cecea-130">Scaffold the movie model</span></span>

<span data-ttu-id="cecea-131">W tej sekcji model filmu jest szkieletem.</span><span class="sxs-lookup"><span data-stu-id="cecea-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="cecea-132">Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="cecea-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cecea-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cecea-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cecea-134">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="cecea-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="cecea-135">Kliknij prawym przyciskiem myszy folder *strony* > **dodaj** **Nowy folder**>.</span><span class="sxs-lookup"><span data-stu-id="cecea-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="cecea-136">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="cecea-136">Name the folder *Movies*</span></span>

<span data-ttu-id="cecea-137">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** **nowy element szkieletowy**>.</span><span class="sxs-lookup"><span data-stu-id="cecea-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="cecea-139">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="cecea-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="cecea-141">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="cecea-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="cecea-142">Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="cecea-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="cecea-143">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="cecea-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="cecea-144">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="cecea-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="cecea-145">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="cecea-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="cecea-146">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="cecea-146">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

<span data-ttu-id="cecea-148">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cecea-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cecea-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="cecea-150">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="cecea-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="cecea-151">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="cecea-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="cecea-152">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="cecea-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="cecea-153">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="cecea-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cecea-154">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cecea-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cecea-155">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="cecea-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="cecea-156">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="cecea-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="cecea-157">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="cecea-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="cecea-158">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="cecea-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cecea-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cecea-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cecea-160">Proces tworzenia szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="cecea-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="cecea-161">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="cecea-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="cecea-162">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="cecea-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="cecea-163">Aktualny</span><span class="sxs-lookup"><span data-stu-id="cecea-163">Updated</span></span>

* <span data-ttu-id="cecea-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="cecea-164">*Startup.cs*</span></span>

<span data-ttu-id="cecea-165">Pliki utworzone i zaktualizowane zostały omówione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="cecea-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="cecea-166">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cecea-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="cecea-167">Proces tworzenia szkieletu tworzy następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="cecea-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="cecea-168">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="cecea-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="cecea-169">Utworzone pliki zostały wyjaśnione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="cecea-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="cecea-170">Migracja początkowa</span><span class="sxs-lookup"><span data-stu-id="cecea-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cecea-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cecea-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cecea-172">W tej sekcji konsola Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="cecea-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="cecea-173">Dodawanie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="cecea-173">Add an initial migration.</span></span>
* <span data-ttu-id="cecea-174">Zaktualizuj bazę danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="cecea-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="cecea-175">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="cecea-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="cecea-177">W obszarze PMC wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="cecea-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cecea-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cecea-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cecea-179">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cecea-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="cecea-180">Powyższe polecenia generują następujące ostrzeżenie: "nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="cecea-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="cecea-181">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="cecea-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="cecea-182">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="cecea-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="cecea-183">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="cecea-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="cecea-184">Polecenie migrations generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="cecea-185">Schemat jest oparty na modelu określonym w `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="cecea-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="cecea-186">Argument `InitialCreate` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="cecea-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="cecea-187">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="cecea-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="cecea-188">Polecenie `update` uruchamia metodę `Up` w migracjach, które nie zostały zastosowane.</span><span class="sxs-lookup"><span data-stu-id="cecea-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="cecea-189">W takim przypadku `update` uruchamia metodę `Up` w przypadku *migracji/\<sygnatury czasowej > pliku _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cecea-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cecea-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="cecea-191">Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="cecea-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="cecea-192">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cecea-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="cecea-193">Usługi (takie jak kontekst EF Core DB) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cecea-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="cecea-194">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="cecea-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="cecea-195">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="cecea-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="cecea-196">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="cecea-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="cecea-197">Przeanalizuj metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cecea-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="cecea-198">Podświetlony wiersz został dodany przez program do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="cecea-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="cecea-199">`RazorPagesMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="cecea-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="cecea-200">Kontekst danych (`RazorPagesMovieContext`) pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="cecea-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="cecea-201">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="cecea-202">Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="cecea-202">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="cecea-203">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="cecea-204">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="cecea-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="cecea-205">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="cecea-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="cecea-206">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="cecea-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cecea-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cecea-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="cecea-208">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="cecea-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cecea-209">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cecea-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="cecea-210">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="cecea-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="cecea-211">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="cecea-211">Test the app</span></span>

* <span data-ttu-id="cecea-212">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="cecea-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="cecea-213">Jeśli wystąpi błąd:</span><span class="sxs-lookup"><span data-stu-id="cecea-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="cecea-214">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="cecea-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="cecea-215">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="cecea-215">Test the **Create** link.</span></span>

  ![Utwórz stronę](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="cecea-217">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="cecea-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="cecea-218">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="cecea-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="cecea-219">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="cecea-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="cecea-220">Przetestuj linki **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="cecea-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="cecea-221">W następnym samouczku objaśniono pliki utworzone przez tworzenie szkieletu.</span><span class="sxs-lookup"><span data-stu-id="cecea-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cecea-222">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cecea-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cecea-223">[Poprzedni:](xref:tutorials/razor-pages/razor-pages-start)wprowadzenie 
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="cecea-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="cecea-224">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="cecea-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="cecea-225">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="cecea-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="cecea-226">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="cecea-227">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="cecea-228">Klasy modelu są znane jako klasy POCO (z "zwykłych, starych obiektów CLR"), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="cecea-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="cecea-229">Definiują właściwości danych przechowywanych w bazie danych programu.</span><span class="sxs-lookup"><span data-stu-id="cecea-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="cecea-230">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="cecea-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cecea-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cecea-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cecea-232">Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="cecea-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="cecea-233">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="cecea-233">Name the folder *Models*.</span></span>

<span data-ttu-id="cecea-234">Kliknij prawym przyciskiem myszy folder *modele* .</span><span class="sxs-lookup"><span data-stu-id="cecea-234">Right click the *Models* folder.</span></span> <span data-ttu-id="cecea-235">Wybierz pozycję **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="cecea-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="cecea-236">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="cecea-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cecea-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cecea-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="cecea-238">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="cecea-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="cecea-239">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="cecea-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cecea-240">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cecea-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cecea-241">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="cecea-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="cecea-242">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="cecea-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="cecea-243">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="cecea-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="cecea-244">W oknie dialogowym **nowy plik** :</span><span class="sxs-lookup"><span data-stu-id="cecea-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="cecea-245">W lewym okienku wybierz pozycję **Ogólne** .</span><span class="sxs-lookup"><span data-stu-id="cecea-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="cecea-246">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="cecea-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="cecea-247">Nazwij **film** klasy i wybierz pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="cecea-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="cecea-248">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cecea-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="cecea-249">Tworzenie szkieletu modelu filmu</span><span class="sxs-lookup"><span data-stu-id="cecea-249">Scaffold the movie model</span></span>

<span data-ttu-id="cecea-250">W tej sekcji model filmu jest szkieletem.</span><span class="sxs-lookup"><span data-stu-id="cecea-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="cecea-251">Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="cecea-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cecea-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cecea-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cecea-253">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="cecea-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="cecea-254">Kliknij prawym przyciskiem myszy folder *strony* > **dodaj** **Nowy folder**>.</span><span class="sxs-lookup"><span data-stu-id="cecea-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="cecea-255">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="cecea-255">Name the folder *Movies*</span></span>

<span data-ttu-id="cecea-256">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** **nowy element szkieletowy**>.</span><span class="sxs-lookup"><span data-stu-id="cecea-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="cecea-258">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="cecea-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="cecea-260">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="cecea-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="cecea-261">Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="cecea-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="cecea-262">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i zaakceptuj wygenerowaną nazwę **RazorPagesMovie. models. RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="cecea-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="cecea-263">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="cecea-263">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="cecea-265">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cecea-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cecea-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="cecea-267">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="cecea-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="cecea-268">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="cecea-268">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="cecea-269">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="cecea-269">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="cecea-270">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="cecea-270">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cecea-271">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cecea-271">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cecea-272">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="cecea-272">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="cecea-273">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="cecea-273">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="cecea-274">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="cecea-274">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="cecea-275">Proces tworzenia szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="cecea-275">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="cecea-276">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="cecea-276">Files created</span></span>

* <span data-ttu-id="cecea-277">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="cecea-277">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="cecea-278">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="cecea-278">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="cecea-279">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="cecea-279">File updated</span></span>

* <span data-ttu-id="cecea-280">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="cecea-280">*Startup.cs*</span></span>

<span data-ttu-id="cecea-281">Pliki utworzone i zaktualizowane zostały omówione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="cecea-281">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="cecea-282">Migracja początkowa</span><span class="sxs-lookup"><span data-stu-id="cecea-282">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cecea-283">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cecea-283">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cecea-284">W tej sekcji konsola Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="cecea-284">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="cecea-285">Dodawanie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="cecea-285">Add an initial migration.</span></span>
* <span data-ttu-id="cecea-286">Zaktualizuj bazę danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="cecea-286">Update the database with the initial migration.</span></span>

<span data-ttu-id="cecea-287">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="cecea-287">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="cecea-289">W obszarze PMC wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="cecea-289">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="cecea-290">Polecenie `Add-Migration` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-290">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="cecea-291">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="cecea-291">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="cecea-292">Do nazwy migracji użyto argumentu `InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="cecea-292">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="cecea-293">Można użyć dowolnej nazwy, ale według Konwencji Nazwa opisująca migrację jest używana.</span><span class="sxs-lookup"><span data-stu-id="cecea-293">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="cecea-294">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="cecea-294">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="cecea-295">`Update-Database` polecenie uruchamia metodę `Up` w *sygnaturze czasowej migracji/\<> pliku _InitialCreate. cs* .</span><span class="sxs-lookup"><span data-stu-id="cecea-295">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="cecea-296">Metoda `Up` tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-296">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cecea-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cecea-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cecea-298">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cecea-298">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="cecea-299">Powyższe polecenia generują następujące ostrzeżenie: "*nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".* Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="cecea-299">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cecea-300">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cecea-300">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="cecea-301">Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="cecea-301">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="cecea-302">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cecea-302">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="cecea-303">Usługi (takie jak kontekst EF Core DB) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cecea-303">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="cecea-304">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="cecea-304">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="cecea-305">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="cecea-305">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="cecea-306">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="cecea-306">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="cecea-307">Przeanalizuj metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cecea-307">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="cecea-308">Podświetlony wiersz został dodany przez program do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="cecea-308">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="cecea-309">`RazorPagesMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="cecea-309">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="cecea-310">Kontekst danych (`RazorPagesMovieContext`) pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="cecea-310">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="cecea-311">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-311">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="cecea-312">Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="cecea-312">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="cecea-313">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cecea-313">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="cecea-314">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="cecea-314">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="cecea-315">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="cecea-315">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="cecea-316">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="cecea-316">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cecea-317">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cecea-317">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="cecea-318">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="cecea-318">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cecea-319">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cecea-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="cecea-320">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="cecea-320">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="cecea-321">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="cecea-321">Test the app</span></span>

* <span data-ttu-id="cecea-322">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="cecea-322">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="cecea-323">Jeśli wystąpi błąd:</span><span class="sxs-lookup"><span data-stu-id="cecea-323">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="cecea-324">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="cecea-324">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="cecea-325">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="cecea-325">Test the **Create** link.</span></span>

  ![Utwórz stronę](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="cecea-327">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="cecea-327">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="cecea-328">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="cecea-328">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="cecea-329">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="cecea-329">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="cecea-330">Przetestuj linki **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="cecea-330">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="cecea-331">W następnym samouczku objaśniono pliki utworzone przez tworzenie szkieletu.</span><span class="sxs-lookup"><span data-stu-id="cecea-331">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cecea-332">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cecea-332">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cecea-333">[Poprzedni:](xref:tutorials/razor-pages/razor-pages-start)wprowadzenie 
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="cecea-333">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
