---
title: Dodawanie modelu do aplikacji Razor Pages w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać klasy do zarządzania filmami w bazie danych przy użyciu Entity Framework Core (EF Core).
ms.author: riande
ms.date: 9/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 4f8b80cb51bd10eb3b136a780dc123c41d61c0a5
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519076"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="77cd0-103">Dodawanie modelu do aplikacji Razor Pages w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77cd0-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="77cd0-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="77cd0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="77cd0-105">W tej sekcji są dodawane klasy do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="77cd0-106">Te klasy są używane z [Entity Framework Core](/ef/core) (Ef Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="77cd0-107">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="77cd0-108">Klasy modelu są znane jako klasy POCO (z "zwykłych, starych obiektów CLR"), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="77cd0-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="77cd0-109">Definiują właściwości danych przechowywanych w bazie danych programu.</span><span class="sxs-lookup"><span data-stu-id="77cd0-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="77cd0-110">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="77cd0-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="77cd0-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77cd0-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="77cd0-112">Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="77cd0-113">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="77cd0-113">Name the folder *Models*.</span></span>

<span data-ttu-id="77cd0-114">Kliknij prawym przyciskiem myszy folder *modele* .</span><span class="sxs-lookup"><span data-stu-id="77cd0-114">Right click the *Models* folder.</span></span> <span data-ttu-id="77cd0-115">Wybierz pozycję **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="77cd0-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="77cd0-116">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="77cd0-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="77cd0-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77cd0-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="77cd0-118">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="77cd0-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="77cd0-119">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="77cd0-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="77cd0-120">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="77cd0-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="77cd0-121">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="77cd0-122">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="77cd0-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="77cd0-123">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="77cd0-124">W oknie dialogowym **nowy plik** :</span><span class="sxs-lookup"><span data-stu-id="77cd0-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="77cd0-125">W lewym okienku wybierz pozycję **Ogólne** .</span><span class="sxs-lookup"><span data-stu-id="77cd0-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="77cd0-126">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="77cd0-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="77cd0-127">Nazwij **film** klasy i wybierz pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="77cd0-128">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="77cd0-129">Tworzenie szkieletu modelu filmu</span><span class="sxs-lookup"><span data-stu-id="77cd0-129">Scaffold the movie model</span></span>

<span data-ttu-id="77cd0-130">W tej sekcji model filmu jest szkieletem.</span><span class="sxs-lookup"><span data-stu-id="77cd0-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="77cd0-131">Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="77cd0-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="77cd0-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77cd0-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="77cd0-133">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="77cd0-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="77cd0-134">Kliknij prawym przyciskiem myszy folder *strony* > **dodaj** **Nowy folder**>.</span><span class="sxs-lookup"><span data-stu-id="77cd0-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="77cd0-135">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="77cd0-135">Name the folder *Movies*</span></span>

<span data-ttu-id="77cd0-136">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** **nowy element szkieletowy**>.</span><span class="sxs-lookup"><span data-stu-id="77cd0-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="77cd0-138">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="77cd0-140">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="77cd0-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="77cd0-141">Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="77cd0-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="77cd0-142">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="77cd0-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="77cd0-143">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="77cd0-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="77cd0-144">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="77cd0-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="77cd0-145">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-145">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

<span data-ttu-id="77cd0-147">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="77cd0-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77cd0-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="77cd0-149">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="77cd0-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="77cd0-150">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="77cd0-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="77cd0-151">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="77cd0-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="77cd0-152">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="77cd0-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="77cd0-153">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="77cd0-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="77cd0-154">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="77cd0-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="77cd0-155">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="77cd0-155">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="77cd0-156">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="77cd0-156">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="77cd0-157">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="77cd0-157">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="77cd0-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77cd0-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="77cd0-159">Proces tworzenia szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="77cd0-159">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="77cd0-160">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="77cd0-160">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="77cd0-161">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="77cd0-161">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="77cd0-162">Aktualny</span><span class="sxs-lookup"><span data-stu-id="77cd0-162">Updated</span></span>

* <span data-ttu-id="77cd0-163">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="77cd0-163">*Startup.cs*</span></span>

<span data-ttu-id="77cd0-164">Pliki utworzone i zaktualizowane zostały omówione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-164">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="77cd0-165">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="77cd0-165">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="77cd0-166">Proces tworzenia szkieletu tworzy następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="77cd0-166">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="77cd0-167">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="77cd0-167">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="77cd0-168">Utworzone pliki zostały wyjaśnione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-168">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="77cd0-169">Migracja początkowa</span><span class="sxs-lookup"><span data-stu-id="77cd0-169">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="77cd0-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77cd0-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="77cd0-171">W tej sekcji konsola Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="77cd0-171">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="77cd0-172">Dodawanie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-172">Add an initial migration.</span></span>
* <span data-ttu-id="77cd0-173">Zaktualizuj bazę danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-173">Update the database with the initial migration.</span></span>

<span data-ttu-id="77cd0-174">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-174">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="77cd0-176">W obszarze PMC wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="77cd0-176">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="77cd0-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77cd0-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="77cd0-178">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="77cd0-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="77cd0-179">Powyższe polecenia generują następujące ostrzeżenie: "nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="77cd0-179">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="77cd0-180">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="77cd0-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="77cd0-181">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="77cd0-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="77cd0-182">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="77cd0-182">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="77cd0-183">Polecenie migrations generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-183">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="77cd0-184">Schemat jest oparty na modelu określonym w `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="77cd0-184">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="77cd0-185">Argument `InitialCreate` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-185">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="77cd0-186">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="77cd0-186">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="77cd0-187">Polecenie `update` uruchamia metodę `Up` w migracjach, które nie zostały zastosowane.</span><span class="sxs-lookup"><span data-stu-id="77cd0-187">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="77cd0-188">W takim przypadku `update` uruchamia metodę `Up` w *migracji/\<time-sygnaturze > _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-188">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="77cd0-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77cd0-189">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="77cd0-190">Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="77cd0-190">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="77cd0-191">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="77cd0-191">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="77cd0-192">Usługi (takie jak kontekst EF Core DB) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-192">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="77cd0-193">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="77cd0-193">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="77cd0-194">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="77cd0-194">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="77cd0-195">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="77cd0-195">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="77cd0-196">Przeanalizuj metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="77cd0-196">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="77cd0-197">Podświetlony wiersz został dodany przez program do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="77cd0-197">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="77cd0-198">@No__t-0 koordynuje EF Core funkcjonalności (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="77cd0-198">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="77cd0-199">Kontekst danych (`RazorPagesMovieContext`) pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="77cd0-199">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="77cd0-200">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-200">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="77cd0-201">Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="77cd0-201">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="77cd0-202">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-202">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="77cd0-203">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="77cd0-203">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="77cd0-204">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="77cd0-204">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="77cd0-205">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="77cd0-205">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="77cd0-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77cd0-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="77cd0-207">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="77cd0-207">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="77cd0-208">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="77cd0-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="77cd0-209">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="77cd0-209">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="77cd0-210">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="77cd0-210">Test the app</span></span>

* <span data-ttu-id="77cd0-211">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="77cd0-211">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="77cd0-212">Jeśli wystąpi błąd:</span><span class="sxs-lookup"><span data-stu-id="77cd0-212">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="77cd0-213">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="77cd0-213">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="77cd0-214">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="77cd0-214">Test the **Create** link.</span></span>

  ![Utwórz stronę](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="77cd0-216">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-216">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="77cd0-217">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="77cd0-217">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="77cd0-218">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="77cd0-218">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="77cd0-219">Przetestuj linki **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="77cd0-219">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="77cd0-220">W następnym samouczku objaśniono pliki utworzone przez tworzenie szkieletu.</span><span class="sxs-lookup"><span data-stu-id="77cd0-220">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77cd0-221">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="77cd0-221">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="77cd0-222">[Poprzedni:](xref:tutorials/razor-pages/razor-pages-start)wprowadzenie 
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="77cd0-222">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="77cd0-223">W tej sekcji są dodawane klasy do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-223">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="77cd0-224">Te klasy są używane z [Entity Framework Core](/ef/core) (Ef Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-224">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="77cd0-225">EF Core to struktura obiektu mapowania relacyjnego (ORM), która upraszcza kod dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-225">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="77cd0-226">Klasy modelu są znane jako klasy POCO (z "zwykłych, starych obiektów CLR"), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="77cd0-226">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="77cd0-227">Definiują właściwości danych przechowywanych w bazie danych programu.</span><span class="sxs-lookup"><span data-stu-id="77cd0-227">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="77cd0-228">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="77cd0-228">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="77cd0-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77cd0-229">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="77cd0-230">Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-230">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="77cd0-231">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="77cd0-231">Name the folder *Models*.</span></span>

<span data-ttu-id="77cd0-232">Kliknij prawym przyciskiem myszy folder *modele* .</span><span class="sxs-lookup"><span data-stu-id="77cd0-232">Right click the *Models* folder.</span></span> <span data-ttu-id="77cd0-233">Wybierz pozycję **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="77cd0-233">Select **Add** > **Class**.</span></span> <span data-ttu-id="77cd0-234">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="77cd0-234">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="77cd0-235">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77cd0-235">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="77cd0-236">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="77cd0-236">Add a folder named *Models*.</span></span>
* <span data-ttu-id="77cd0-237">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="77cd0-237">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="77cd0-238">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="77cd0-238">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="77cd0-239">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-239">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="77cd0-240">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="77cd0-240">Name the folder *Models*.</span></span>
* <span data-ttu-id="77cd0-241">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-241">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="77cd0-242">W oknie dialogowym **nowy plik** :</span><span class="sxs-lookup"><span data-stu-id="77cd0-242">In the **New File** dialog:</span></span>

  * <span data-ttu-id="77cd0-243">W lewym okienku wybierz pozycję **Ogólne** .</span><span class="sxs-lookup"><span data-stu-id="77cd0-243">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="77cd0-244">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="77cd0-244">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="77cd0-245">Nazwij **film** klasy i wybierz pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-245">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="77cd0-246">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-246">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="77cd0-247">Tworzenie szkieletu modelu filmu</span><span class="sxs-lookup"><span data-stu-id="77cd0-247">Scaffold the movie model</span></span>

<span data-ttu-id="77cd0-248">W tej sekcji model filmu jest szkieletem.</span><span class="sxs-lookup"><span data-stu-id="77cd0-248">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="77cd0-249">Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="77cd0-249">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="77cd0-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77cd0-250">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="77cd0-251">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="77cd0-251">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="77cd0-252">Kliknij prawym przyciskiem myszy folder *strony* > **dodaj** **Nowy folder**>.</span><span class="sxs-lookup"><span data-stu-id="77cd0-252">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="77cd0-253">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="77cd0-253">Name the folder *Movies*</span></span>

<span data-ttu-id="77cd0-254">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** **nowy element szkieletowy**>.</span><span class="sxs-lookup"><span data-stu-id="77cd0-254">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="77cd0-256">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-256">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="77cd0-258">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="77cd0-258">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="77cd0-259">Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="77cd0-259">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="77cd0-260">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i zaakceptuj wygenerowaną nazwę **RazorPagesMovie. models. RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-260">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="77cd0-261">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-261">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="77cd0-263">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-263">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="77cd0-264">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77cd0-264">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="77cd0-265">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="77cd0-265">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="77cd0-266">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="77cd0-266">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="77cd0-267">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="77cd0-267">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="77cd0-268">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="77cd0-268">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="77cd0-269">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="77cd0-269">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="77cd0-270">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="77cd0-270">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="77cd0-271">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="77cd0-271">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="77cd0-272">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="77cd0-272">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="77cd0-273">Proces tworzenia szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="77cd0-273">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="77cd0-274">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="77cd0-274">Files created</span></span>

* <span data-ttu-id="77cd0-275">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="77cd0-275">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="77cd0-276">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="77cd0-276">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="77cd0-277">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="77cd0-277">File updated</span></span>

* <span data-ttu-id="77cd0-278">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="77cd0-278">*Startup.cs*</span></span>

<span data-ttu-id="77cd0-279">Pliki utworzone i zaktualizowane zostały omówione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-279">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="77cd0-280">Migracja początkowa</span><span class="sxs-lookup"><span data-stu-id="77cd0-280">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="77cd0-281">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77cd0-281">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="77cd0-282">W tej sekcji konsola Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="77cd0-282">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="77cd0-283">Dodawanie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-283">Add an initial migration.</span></span>
* <span data-ttu-id="77cd0-284">Zaktualizuj bazę danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-284">Update the database with the initial migration.</span></span>

<span data-ttu-id="77cd0-285">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="77cd0-285">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="77cd0-287">W obszarze PMC wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="77cd0-287">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="77cd0-288">Polecenie `Add-Migration` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-288">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="77cd0-289">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="77cd0-289">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="77cd0-290">Do nazwy migracji użyto argumentu `InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="77cd0-290">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="77cd0-291">Można użyć dowolnej nazwy, ale według Konwencji Nazwa opisująca migrację jest używana.</span><span class="sxs-lookup"><span data-stu-id="77cd0-291">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="77cd0-292">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="77cd0-292">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="77cd0-293">Polecenie `Update-Database` uruchamia metodę `Up` w pliku *migrations/\<time-sygnatura > _InitialCreate. cs* .</span><span class="sxs-lookup"><span data-stu-id="77cd0-293">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="77cd0-294">Metoda `Up` tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-294">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="77cd0-295">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77cd0-295">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="77cd0-296">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="77cd0-296">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="77cd0-297">Powyższe polecenia generują następujące ostrzeżenie: "*nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".* Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="77cd0-297">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="77cd0-298">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77cd0-298">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="77cd0-299">Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="77cd0-299">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="77cd0-300">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="77cd0-300">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="77cd0-301">Usługi (takie jak kontekst EF Core DB) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77cd0-301">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="77cd0-302">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="77cd0-302">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="77cd0-303">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="77cd0-303">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="77cd0-304">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="77cd0-304">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="77cd0-305">Przeanalizuj metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="77cd0-305">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="77cd0-306">Podświetlony wiersz został dodany przez program do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="77cd0-306">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="77cd0-307">@No__t-0 koordynuje EF Core funkcjonalności (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="77cd0-307">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="77cd0-308">Kontekst danych (`RazorPagesMovieContext`) pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="77cd0-308">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="77cd0-309">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-309">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="77cd0-310">Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="77cd0-310">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="77cd0-311">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-311">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="77cd0-312">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="77cd0-312">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="77cd0-313">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="77cd0-313">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="77cd0-314">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="77cd0-314">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="77cd0-315">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77cd0-315">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="77cd0-316">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="77cd0-316">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="77cd0-317">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="77cd0-317">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="77cd0-318">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="77cd0-318">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="77cd0-319">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="77cd0-319">Test the app</span></span>

* <span data-ttu-id="77cd0-320">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="77cd0-320">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="77cd0-321">Jeśli wystąpi błąd:</span><span class="sxs-lookup"><span data-stu-id="77cd0-321">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="77cd0-322">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="77cd0-322">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="77cd0-323">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="77cd0-323">Test the **Create** link.</span></span>

  ![Utwórz stronę](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="77cd0-325">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="77cd0-325">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="77cd0-326">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="77cd0-326">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="77cd0-327">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="77cd0-327">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="77cd0-328">Przetestuj linki **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="77cd0-328">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="77cd0-329">W następnym samouczku objaśniono pliki utworzone przez tworzenie szkieletu.</span><span class="sxs-lookup"><span data-stu-id="77cd0-329">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77cd0-330">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="77cd0-330">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="77cd0-331">[Poprzedni:](xref:tutorials/razor-pages/razor-pages-start)wprowadzenie 
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="77cd0-331">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
