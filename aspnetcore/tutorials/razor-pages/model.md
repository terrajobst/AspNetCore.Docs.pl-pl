---
title: Dodawanie modelu do aplikacji Razor Pages w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać klasy do zarządzania filmami w bazie danych przy użyciu Entity Framework Core (EF Core).
ms.author: riande
ms.date: 9/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: c101fe4aee9a1fbf28d66a8527e3c199194d73fe
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334182"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="73040-103">Dodawanie modelu do aplikacji Razor Pages w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73040-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="73040-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="73040-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="73040-105">W tej sekcji są dodawane klasy do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="73040-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="73040-106">Te klasy są używane z [Entity Framework Core](/ef/core) (Ef Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="73040-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="73040-107">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="73040-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="73040-108">Klasy modelu są znane jako klasy POCO (z "zwykłych, starych obiektów CLR"), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="73040-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="73040-109">Definiują właściwości danych przechowywanych w bazie danych programu.</span><span class="sxs-lookup"><span data-stu-id="73040-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="73040-110">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="73040-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73040-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73040-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="73040-112">Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="73040-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="73040-113">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="73040-113">Name the folder *Models*.</span></span>

<span data-ttu-id="73040-114">Kliknij prawym przyciskiem myszy folder *modele* .</span><span class="sxs-lookup"><span data-stu-id="73040-114">Right click the *Models* folder.</span></span> <span data-ttu-id="73040-115">Wybierz pozycję **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="73040-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="73040-116">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="73040-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73040-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73040-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="73040-118">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="73040-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="73040-119">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="73040-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73040-120">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="73040-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="73040-121">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="73040-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="73040-122">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="73040-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="73040-123">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="73040-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="73040-124">W oknie dialogowym **nowy plik** :</span><span class="sxs-lookup"><span data-stu-id="73040-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="73040-125">W lewym okienku wybierz pozycję **Ogólne** .</span><span class="sxs-lookup"><span data-stu-id="73040-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="73040-126">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="73040-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="73040-127">Nazwij **film** klasy i wybierz pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="73040-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="73040-128">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="73040-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="73040-129">Tworzenie szkieletu modelu filmu</span><span class="sxs-lookup"><span data-stu-id="73040-129">Scaffold the movie model</span></span>

<span data-ttu-id="73040-130">W tej sekcji model filmu jest szkieletem.</span><span class="sxs-lookup"><span data-stu-id="73040-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="73040-131">Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="73040-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73040-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73040-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="73040-133">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="73040-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="73040-134">Kliknij prawym przyciskiem myszy folder *strony* > **dodaj** **Nowy folder**>.</span><span class="sxs-lookup"><span data-stu-id="73040-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="73040-135">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="73040-135">Name the folder *Movies*</span></span>

<span data-ttu-id="73040-136">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** **nowy element szkieletowy**>.</span><span class="sxs-lookup"><span data-stu-id="73040-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="73040-138">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="73040-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="73040-140">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="73040-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="73040-141">Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="73040-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="73040-142">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="73040-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="73040-143">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="73040-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="73040-144">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="73040-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="73040-145">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="73040-145">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

<span data-ttu-id="73040-147">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="73040-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73040-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73040-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="73040-149">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="73040-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="73040-150">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="73040-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="73040-151">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="73040-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="73040-152">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="73040-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73040-153">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="73040-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="73040-154">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="73040-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="73040-155">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="73040-155">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="73040-156">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="73040-156">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="73040-157">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="73040-157">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73040-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73040-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="73040-159">Proces tworzenia szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="73040-159">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="73040-160">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="73040-160">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="73040-161">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="73040-161">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="73040-162">Aktualny</span><span class="sxs-lookup"><span data-stu-id="73040-162">Updated</span></span>

* <span data-ttu-id="73040-163">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="73040-163">*Startup.cs*</span></span>

<span data-ttu-id="73040-164">Pliki utworzone i zaktualizowane zostały omówione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="73040-164">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="73040-165">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="73040-165">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="73040-166">Proces tworzenia szkieletu tworzy następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="73040-166">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="73040-167">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="73040-167">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="73040-168">Utworzone pliki zostały wyjaśnione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="73040-168">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="73040-169">Migracja początkowa</span><span class="sxs-lookup"><span data-stu-id="73040-169">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73040-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73040-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="73040-171">W tej sekcji konsola Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="73040-171">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="73040-172">Dodawanie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="73040-172">Add an initial migration.</span></span>
* <span data-ttu-id="73040-173">Zaktualizuj bazę danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="73040-173">Update the database with the initial migration.</span></span>

<span data-ttu-id="73040-174">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="73040-174">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="73040-176">W obszarze PMC wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="73040-176">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73040-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73040-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73040-178">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="73040-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="73040-179">Powyższe polecenia generują następujące ostrzeżenie: "nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="73040-179">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="73040-180">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="73040-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="73040-181">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="73040-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="73040-182">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="73040-182">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="73040-183">Polecenie `ef migrations add InitialCreate` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="73040-183">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="73040-184">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="73040-184">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="73040-185">Argument `InitialCreate` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="73040-185">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="73040-186">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="73040-186">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="73040-187">Polecenie `ef database update` uruchamia metodę `Up` w pliku *migrations/\<time-sygnatura > _InitialCreate. cs* .</span><span class="sxs-lookup"><span data-stu-id="73040-187">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="73040-188">Metoda `Up` tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="73040-188">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73040-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73040-189">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="73040-190">Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="73040-190">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="73040-191">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="73040-191">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="73040-192">Usługi (takie jak kontekst EF Core DB) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="73040-192">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="73040-193">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="73040-193">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="73040-194">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="73040-194">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="73040-195">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="73040-195">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="73040-196">Przeanalizuj metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="73040-196">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="73040-197">Podświetlony wiersz został dodany przez program do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="73040-197">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="73040-198">@No__t-0 koordynuje EF Core funkcjonalności (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="73040-198">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="73040-199">Kontekst danych (`RazorPagesMovieContext`) pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="73040-199">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="73040-200">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="73040-200">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="73040-201">Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="73040-201">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="73040-202">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="73040-202">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="73040-203">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="73040-203">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="73040-204">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="73040-204">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="73040-205">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="73040-205">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73040-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73040-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="73040-207">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="73040-207">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73040-208">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="73040-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="73040-209">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="73040-209">Examine the `Up` method.</span></span>

---

<span data-ttu-id="73040-210">Polecenie `Add-Migration` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="73040-210">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="73040-211">Schemat jest oparty na modelu określonym w `RazorPagesMovieContext` (w pliku *Data/RazorPagesMovieContext. cs* ).</span><span class="sxs-lookup"><span data-stu-id="73040-211">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="73040-212">Argument `Initial` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="73040-212">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="73040-213">Można użyć dowolnej nazwy, ale według Konwencji Nazwa opisująca migrację jest używana.</span><span class="sxs-lookup"><span data-stu-id="73040-213">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="73040-214">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="73040-214">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="73040-215">Polecenie `Update-Database` uruchamia metodę `Up` w pliku *migrations/{Time-sygnatura} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="73040-215">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="73040-216">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="73040-216">Test the app</span></span>

* <span data-ttu-id="73040-217">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="73040-217">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="73040-218">Jeśli wystąpi błąd:</span><span class="sxs-lookup"><span data-stu-id="73040-218">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="73040-219">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="73040-219">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="73040-220">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="73040-220">Test the **Create** link.</span></span>

  ![Utwórz stronę](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="73040-222">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="73040-222">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="73040-223">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="73040-223">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="73040-224">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="73040-224">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="73040-225">Przetestuj linki **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="73040-225">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="73040-226">W następnym samouczku objaśniono pliki utworzone przez tworzenie szkieletu.</span><span class="sxs-lookup"><span data-stu-id="73040-226">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73040-227">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="73040-227">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="73040-228">[Poprzedni:](xref:tutorials/razor-pages/razor-pages-start)wprowadzenie 
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="73040-228">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="73040-229">W tej sekcji są dodawane klasy do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="73040-229">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="73040-230">Te klasy są używane z [Entity Framework Core](/ef/core) (Ef Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="73040-230">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="73040-231">EF Core to struktura obiektu mapowania relacyjnego (ORM), która upraszcza kod dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="73040-231">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="73040-232">Klasy modelu są znane jako klasy POCO (z "zwykłych, starych obiektów CLR"), ponieważ nie mają żadnej zależności od EF Core.</span><span class="sxs-lookup"><span data-stu-id="73040-232">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="73040-233">Definiują właściwości danych przechowywanych w bazie danych programu.</span><span class="sxs-lookup"><span data-stu-id="73040-233">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="73040-234">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="73040-234">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73040-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73040-235">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="73040-236">Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="73040-236">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="73040-237">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="73040-237">Name the folder *Models*.</span></span>

<span data-ttu-id="73040-238">Kliknij prawym przyciskiem myszy folder *modele* .</span><span class="sxs-lookup"><span data-stu-id="73040-238">Right click the *Models* folder.</span></span> <span data-ttu-id="73040-239">Wybierz pozycję **Dodaj** **klasy** > .</span><span class="sxs-lookup"><span data-stu-id="73040-239">Select **Add** > **Class**.</span></span> <span data-ttu-id="73040-240">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="73040-240">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73040-241">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73040-241">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="73040-242">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="73040-242">Add a folder named *Models*.</span></span>
* <span data-ttu-id="73040-243">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="73040-243">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73040-244">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="73040-244">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="73040-245">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="73040-245">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="73040-246">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="73040-246">Name the folder *Models*.</span></span>
* <span data-ttu-id="73040-247">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="73040-247">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="73040-248">W oknie dialogowym **nowy plik** :</span><span class="sxs-lookup"><span data-stu-id="73040-248">In the **New File** dialog:</span></span>

  * <span data-ttu-id="73040-249">W lewym okienku wybierz pozycję **Ogólne** .</span><span class="sxs-lookup"><span data-stu-id="73040-249">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="73040-250">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="73040-250">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="73040-251">Nazwij **film** klasy i wybierz pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="73040-251">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="73040-252">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="73040-252">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="73040-253">Tworzenie szkieletu modelu filmu</span><span class="sxs-lookup"><span data-stu-id="73040-253">Scaffold the movie model</span></span>

<span data-ttu-id="73040-254">W tej sekcji model filmu jest szkieletem.</span><span class="sxs-lookup"><span data-stu-id="73040-254">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="73040-255">Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="73040-255">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73040-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73040-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="73040-257">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="73040-257">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="73040-258">Kliknij prawym przyciskiem myszy folder *strony* > **dodaj** **Nowy folder**>.</span><span class="sxs-lookup"><span data-stu-id="73040-258">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="73040-259">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="73040-259">Name the folder *Movies*</span></span>

<span data-ttu-id="73040-260">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** **nowy element szkieletowy**>.</span><span class="sxs-lookup"><span data-stu-id="73040-260">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="73040-262">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="73040-262">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="73040-264">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="73040-264">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="73040-265">Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="73040-265">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="73040-266">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i zaakceptuj wygenerowaną nazwę **RazorPagesMovie. models. RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="73040-266">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="73040-267">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="73040-267">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="73040-269">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="73040-269">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73040-270">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73040-270">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="73040-271">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="73040-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="73040-272">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="73040-272">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="73040-273">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="73040-273">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="73040-274">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="73040-274">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73040-275">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="73040-275">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="73040-276">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="73040-276">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="73040-277">Zainstaluj narzędzie do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="73040-277">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="73040-278">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="73040-278">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="73040-279">Proces tworzenia szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="73040-279">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="73040-280">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="73040-280">Files created</span></span>

* <span data-ttu-id="73040-281">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="73040-281">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="73040-282">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="73040-282">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="73040-283">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="73040-283">File updated</span></span>

* <span data-ttu-id="73040-284">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="73040-284">*Startup.cs*</span></span>

<span data-ttu-id="73040-285">Pliki utworzone i zaktualizowane zostały omówione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="73040-285">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="73040-286">Migracja początkowa</span><span class="sxs-lookup"><span data-stu-id="73040-286">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73040-287">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73040-287">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="73040-288">W tej sekcji konsola Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="73040-288">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="73040-289">Dodawanie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="73040-289">Add an initial migration.</span></span>
* <span data-ttu-id="73040-290">Zaktualizuj bazę danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="73040-290">Update the database with the initial migration.</span></span>

<span data-ttu-id="73040-291">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="73040-291">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="73040-293">W obszarze PMC wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="73040-293">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73040-294">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73040-294">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73040-295">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="73040-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="73040-296">Powyższe polecenia generują następujące ostrzeżenie: "nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="73040-296">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="73040-297">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="73040-297">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="73040-298">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="73040-298">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="73040-299">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="73040-299">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="73040-300">Polecenie `ef migrations add InitialCreate` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="73040-300">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="73040-301">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="73040-301">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="73040-302">Argument `InitialCreate` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="73040-302">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="73040-303">Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.</span><span class="sxs-lookup"><span data-stu-id="73040-303">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="73040-304">Polecenie `ef database update` uruchamia metodę `Up` w pliku *migrations/\<time-sygnatura > _InitialCreate. cs* .</span><span class="sxs-lookup"><span data-stu-id="73040-304">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="73040-305">Metoda `Up` tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="73040-305">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73040-306">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73040-306">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="73040-307">Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="73040-307">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="73040-308">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="73040-308">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="73040-309">Usługi (takie jak kontekst EF Core DB) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="73040-309">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="73040-310">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="73040-310">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="73040-311">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="73040-311">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="73040-312">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="73040-312">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="73040-313">Przeanalizuj metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="73040-313">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="73040-314">Podświetlony wiersz został dodany przez program do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="73040-314">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="73040-315">@No__t-0 koordynuje EF Core funkcjonalności (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="73040-315">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="73040-316">Kontekst danych (`RazorPagesMovieContext`) pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="73040-316">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="73040-317">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="73040-317">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="73040-318">Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="73040-318">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="73040-319">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="73040-319">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="73040-320">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="73040-320">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="73040-321">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="73040-321">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="73040-322">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="73040-322">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73040-323">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73040-323">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="73040-324">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="73040-324">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73040-325">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="73040-325">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="73040-326">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="73040-326">Examine the `Up` method.</span></span>

---

<span data-ttu-id="73040-327">Polecenie `Add-Migration` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="73040-327">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="73040-328">Schemat jest oparty na modelu określonym w `RazorPagesMovieContext` (w pliku *Data/RazorPagesMovieContext. cs* ).</span><span class="sxs-lookup"><span data-stu-id="73040-328">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="73040-329">Argument `Initial` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="73040-329">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="73040-330">Można użyć dowolnej nazwy, ale według Konwencji Nazwa opisująca migrację jest używana.</span><span class="sxs-lookup"><span data-stu-id="73040-330">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="73040-331">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="73040-331">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="73040-332">Polecenie `Update-Database` uruchamia metodę `Up` w pliku *migrations/{Time-sygnatura} _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="73040-332">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="73040-333">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="73040-333">Test the app</span></span>

* <span data-ttu-id="73040-334">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="73040-334">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="73040-335">Jeśli wystąpi błąd:</span><span class="sxs-lookup"><span data-stu-id="73040-335">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="73040-336">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="73040-336">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="73040-337">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="73040-337">Test the **Create** link.</span></span>

  ![Utwórz stronę](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="73040-339">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="73040-339">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="73040-340">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="73040-340">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="73040-341">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="73040-341">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="73040-342">Przetestuj linki **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="73040-342">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="73040-343">W następnym samouczku objaśniono pliki utworzone przez tworzenie szkieletu.</span><span class="sxs-lookup"><span data-stu-id="73040-343">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73040-344">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="73040-344">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="73040-345">[Poprzedni:](xref:tutorials/razor-pages/razor-pages-start)wprowadzenie 
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="73040-345">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
