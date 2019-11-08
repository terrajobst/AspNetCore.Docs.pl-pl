---
title: Dodawanie modelu do aplikacji Razor Pages w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać klasy do zarządzania filmami w bazie danych przy użyciu Entity Framework Core (EF Core).
ms.author: riande
ms.date: 11/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f2c9c2fc8112ef8a1a5afdbe448de6319c43521d
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2019
ms.locfileid: "73761223"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="191aa-103">Dodawanie modelu do aplikacji Razor Pages w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="191aa-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="191aa-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="191aa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="191aa-105">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="191aa-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="191aa-106">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="191aa-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="191aa-107">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="191aa-108">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="191aa-109">Klasy modelu są znane jako klasy POCO (z "zwykłych, starych obiektów CLR"), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="191aa-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="191aa-110">Definiują właściwości danych przechowywanych w bazie danych programu.</span><span class="sxs-lookup"><span data-stu-id="191aa-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="191aa-111">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="191aa-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="191aa-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="191aa-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="191aa-113">Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="191aa-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="191aa-114">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="191aa-114">Name the folder *Models*.</span></span>

<span data-ttu-id="191aa-115">Kliknij prawym przyciskiem myszy folder *modele* .</span><span class="sxs-lookup"><span data-stu-id="191aa-115">Right click the *Models* folder.</span></span> <span data-ttu-id="191aa-116">Wybierz pozycję **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="191aa-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="191aa-117">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="191aa-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="191aa-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="191aa-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="191aa-119">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="191aa-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="191aa-120">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="191aa-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="191aa-121">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="191aa-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="191aa-122">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="191aa-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="191aa-123">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="191aa-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="191aa-124">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="191aa-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="191aa-125">W oknie dialogowym **nowy plik** :</span><span class="sxs-lookup"><span data-stu-id="191aa-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="191aa-126">W lewym okienku wybierz pozycję **Ogólne** .</span><span class="sxs-lookup"><span data-stu-id="191aa-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="191aa-127">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="191aa-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="191aa-128">Nazwij **film** klasy i wybierz pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="191aa-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="191aa-129">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="191aa-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="191aa-130">Tworzenie szkieletu modelu filmu</span><span class="sxs-lookup"><span data-stu-id="191aa-130">Scaffold the movie model</span></span>

<span data-ttu-id="191aa-131">W tej sekcji model filmu jest szkieletem.</span><span class="sxs-lookup"><span data-stu-id="191aa-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="191aa-132">Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="191aa-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="191aa-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="191aa-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="191aa-134">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="191aa-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="191aa-135">Kliknij prawym przyciskiem myszy folder *strony* > **dodaj** **Nowy folder**>.</span><span class="sxs-lookup"><span data-stu-id="191aa-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="191aa-136">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="191aa-136">Name the folder *Movies*</span></span>

<span data-ttu-id="191aa-137">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** **nowy element szkieletowy**>.</span><span class="sxs-lookup"><span data-stu-id="191aa-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="191aa-139">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="191aa-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="191aa-141">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="191aa-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="191aa-142">Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="191aa-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="191aa-143">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="191aa-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="191aa-144">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="191aa-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="191aa-145">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="191aa-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="191aa-146">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="191aa-146">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

<span data-ttu-id="191aa-148">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="191aa-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="191aa-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="191aa-150">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="191aa-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="191aa-151">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="191aa-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="191aa-152">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="191aa-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="191aa-153">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="191aa-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="191aa-154">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="191aa-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="191aa-155">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="191aa-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="191aa-156">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="191aa-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="191aa-157">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="191aa-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---

### <a name="files-created"></a><span data-ttu-id="191aa-158">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="191aa-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="191aa-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="191aa-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="191aa-160">Proces tworzenia szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="191aa-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="191aa-161">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="191aa-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="191aa-162">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="191aa-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="191aa-163">Aktualny</span><span class="sxs-lookup"><span data-stu-id="191aa-163">Updated</span></span>

* <span data-ttu-id="191aa-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="191aa-164">*Startup.cs*</span></span>

<span data-ttu-id="191aa-165">Pliki utworzone i zaktualizowane zostały omówione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="191aa-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="191aa-166">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="191aa-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="191aa-167">Proces tworzenia szkieletu tworzy następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="191aa-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="191aa-168">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="191aa-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="191aa-169">Utworzone pliki zostały wyjaśnione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="191aa-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="191aa-170">Migracja początkowa</span><span class="sxs-lookup"><span data-stu-id="191aa-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="191aa-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="191aa-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="191aa-172">W tej sekcji konsola Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="191aa-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="191aa-173">Dodawanie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="191aa-173">Add an initial migration.</span></span>
* <span data-ttu-id="191aa-174">Zaktualizuj bazę danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="191aa-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="191aa-175">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="191aa-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="191aa-177">W obszarze PMC wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="191aa-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="191aa-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="191aa-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="191aa-179">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="191aa-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="191aa-180">Powyższe polecenia generują następujące ostrzeżenie: "nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="191aa-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="191aa-181">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="191aa-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="191aa-182">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="191aa-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="191aa-183">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="191aa-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="191aa-184">Polecenie migrations generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="191aa-185">Schemat jest oparty na modelu określonym w `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="191aa-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="191aa-186">Argument `InitialCreate` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="191aa-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="191aa-187">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="191aa-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="191aa-188">Polecenie `update` uruchamia metodę `Up` w migracjach, które nie zostały zastosowane.</span><span class="sxs-lookup"><span data-stu-id="191aa-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="191aa-189">W takim przypadku `update` uruchamia metodę `Up` w przypadku *migracji/\<sygnatury czasowej > pliku _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="191aa-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="191aa-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="191aa-191">Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="191aa-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="191aa-192">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="191aa-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="191aa-193">Usługi (takie jak kontekst EF Core DB) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="191aa-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="191aa-194">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="191aa-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="191aa-195">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="191aa-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="191aa-196">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="191aa-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="191aa-197">Przeanalizuj metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="191aa-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="191aa-198">Podświetlony wiersz został dodany przez program do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="191aa-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="191aa-199">`RazorPagesMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="191aa-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="191aa-200">Kontekst danych (`RazorPagesMovieContext`) pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="191aa-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="191aa-201">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="191aa-202">Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="191aa-202">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="191aa-203">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="191aa-204">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="191aa-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="191aa-205">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="191aa-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="191aa-206">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="191aa-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="191aa-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="191aa-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="191aa-208">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="191aa-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="191aa-209">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="191aa-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="191aa-210">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="191aa-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="191aa-211">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="191aa-211">Test the app</span></span>

* <span data-ttu-id="191aa-212">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="191aa-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="191aa-213">Jeśli wystąpi błąd:</span><span class="sxs-lookup"><span data-stu-id="191aa-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="191aa-214">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="191aa-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="191aa-215">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="191aa-215">Test the **Create** link.</span></span>

  ![Utwórz stronę](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="191aa-217">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="191aa-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="191aa-218">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="191aa-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="191aa-219">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="191aa-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="191aa-220">Przetestuj linki **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="191aa-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="191aa-221">W następnym samouczku objaśniono pliki utworzone przez tworzenie szkieletu.</span><span class="sxs-lookup"><span data-stu-id="191aa-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="191aa-222">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="191aa-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="191aa-223">[Poprzedni:](xref:tutorials/razor-pages/razor-pages-start)wprowadzenie 
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="191aa-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="191aa-224">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="191aa-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="191aa-225">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="191aa-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="191aa-226">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="191aa-227">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="191aa-228">Klasy modelu są znane jako klasy POCO (z "zwykłych, starych obiektów CLR"), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="191aa-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="191aa-229">Definiują właściwości danych przechowywanych w bazie danych programu.</span><span class="sxs-lookup"><span data-stu-id="191aa-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="191aa-230">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="191aa-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="191aa-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="191aa-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="191aa-232">Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="191aa-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="191aa-233">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="191aa-233">Name the folder *Models*.</span></span>

<span data-ttu-id="191aa-234">Kliknij prawym przyciskiem myszy folder *modele* .</span><span class="sxs-lookup"><span data-stu-id="191aa-234">Right click the *Models* folder.</span></span> <span data-ttu-id="191aa-235">Wybierz pozycję **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="191aa-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="191aa-236">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="191aa-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="191aa-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="191aa-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="191aa-238">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="191aa-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="191aa-239">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="191aa-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="191aa-240">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="191aa-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="191aa-241">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="191aa-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="191aa-242">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="191aa-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="191aa-243">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="191aa-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="191aa-244">W oknie dialogowym **nowy plik** :</span><span class="sxs-lookup"><span data-stu-id="191aa-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="191aa-245">W lewym okienku wybierz pozycję **Ogólne** .</span><span class="sxs-lookup"><span data-stu-id="191aa-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="191aa-246">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="191aa-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="191aa-247">Nazwij **film** klasy i wybierz pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="191aa-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="191aa-248">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="191aa-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="191aa-249">Tworzenie szkieletu modelu filmu</span><span class="sxs-lookup"><span data-stu-id="191aa-249">Scaffold the movie model</span></span>

<span data-ttu-id="191aa-250">W tej sekcji model filmu jest szkieletem.</span><span class="sxs-lookup"><span data-stu-id="191aa-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="191aa-251">Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="191aa-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="191aa-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="191aa-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="191aa-253">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="191aa-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="191aa-254">Kliknij prawym przyciskiem myszy folder *strony* > **dodaj** **Nowy folder**>.</span><span class="sxs-lookup"><span data-stu-id="191aa-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="191aa-255">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="191aa-255">Name the folder *Movies*</span></span>

<span data-ttu-id="191aa-256">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** **nowy element szkieletowy**>.</span><span class="sxs-lookup"><span data-stu-id="191aa-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="191aa-258">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="191aa-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="191aa-260">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="191aa-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="191aa-261">Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="191aa-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="191aa-262">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i zaakceptuj wygenerowaną nazwę **RazorPagesMovie. models. RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="191aa-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="191aa-263">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="191aa-263">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="191aa-265">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="191aa-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="191aa-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="191aa-267">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="191aa-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="191aa-268">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="191aa-268">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="191aa-269">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="191aa-269">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="191aa-270">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="191aa-270">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="191aa-271">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="191aa-271">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="191aa-272">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="191aa-272">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="191aa-273">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="191aa-273">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="191aa-274">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="191aa-274">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="191aa-275">Proces tworzenia szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="191aa-275">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="191aa-276">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="191aa-276">Files created</span></span>

* <span data-ttu-id="191aa-277">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="191aa-277">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="191aa-278">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="191aa-278">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="191aa-279">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="191aa-279">File updated</span></span>

* <span data-ttu-id="191aa-280">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="191aa-280">*Startup.cs*</span></span>

<span data-ttu-id="191aa-281">Pliki utworzone i zaktualizowane zostały omówione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="191aa-281">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="191aa-282">Migracja początkowa</span><span class="sxs-lookup"><span data-stu-id="191aa-282">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="191aa-283">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="191aa-283">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="191aa-284">W tej sekcji konsola Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="191aa-284">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="191aa-285">Dodawanie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="191aa-285">Add an initial migration.</span></span>
* <span data-ttu-id="191aa-286">Zaktualizuj bazę danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="191aa-286">Update the database with the initial migration.</span></span>

<span data-ttu-id="191aa-287">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="191aa-287">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="191aa-289">W obszarze PMC wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="191aa-289">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="191aa-290">Polecenie `Add-Migration` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-290">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="191aa-291">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="191aa-291">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="191aa-292">Do nazwy migracji użyto argumentu `InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="191aa-292">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="191aa-293">Można użyć dowolnej nazwy, ale według Konwencji Nazwa opisująca migrację jest używana.</span><span class="sxs-lookup"><span data-stu-id="191aa-293">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="191aa-294">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="191aa-294">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="191aa-295">`Update-Database` polecenie uruchamia metodę `Up` w *sygnaturze czasowej migracji/\<> pliku _InitialCreate. cs* .</span><span class="sxs-lookup"><span data-stu-id="191aa-295">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="191aa-296">Metoda `Up` tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-296">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="191aa-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="191aa-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="191aa-298">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="191aa-298">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="191aa-299">Powyższe polecenia generują następujące ostrzeżenie: "*nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".* Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="191aa-299">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="191aa-300">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="191aa-300">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="191aa-301">Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="191aa-301">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="191aa-302">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="191aa-302">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="191aa-303">Usługi (takie jak kontekst EF Core DB) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="191aa-303">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="191aa-304">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="191aa-304">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="191aa-305">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="191aa-305">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="191aa-306">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="191aa-306">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="191aa-307">Przeanalizuj metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="191aa-307">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="191aa-308">Podświetlony wiersz został dodany przez program do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="191aa-308">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="191aa-309">`RazorPagesMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="191aa-309">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="191aa-310">Kontekst danych (`RazorPagesMovieContext`) pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="191aa-310">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="191aa-311">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-311">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="191aa-312">Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="191aa-312">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="191aa-313">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="191aa-313">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="191aa-314">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="191aa-314">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="191aa-315">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="191aa-315">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="191aa-316">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="191aa-316">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="191aa-317">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="191aa-317">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="191aa-318">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="191aa-318">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="191aa-319">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="191aa-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="191aa-320">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="191aa-320">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="191aa-321">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="191aa-321">Test the app</span></span>

* <span data-ttu-id="191aa-322">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="191aa-322">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="191aa-323">Jeśli wystąpi błąd:</span><span class="sxs-lookup"><span data-stu-id="191aa-323">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="191aa-324">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="191aa-324">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="191aa-325">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="191aa-325">Test the **Create** link.</span></span>

  ![Utwórz stronę](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="191aa-327">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="191aa-327">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="191aa-328">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="191aa-328">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="191aa-329">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="191aa-329">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="191aa-330">Przetestuj linki **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="191aa-330">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="191aa-331">W następnym samouczku objaśniono pliki utworzone przez tworzenie szkieletu.</span><span class="sxs-lookup"><span data-stu-id="191aa-331">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="191aa-332">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="191aa-332">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="191aa-333">[Poprzedni:](xref:tutorials/razor-pages/razor-pages-start)wprowadzenie 
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="191aa-333">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
