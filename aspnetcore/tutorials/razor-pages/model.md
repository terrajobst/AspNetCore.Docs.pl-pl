---
title: Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak można dodać klas związanych z zarządzaniem filmów w bazie danych przy użyciu platformy Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 1988877a552ba58841140a00b61bdcf003afd87d
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881341"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="27b55-103">Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="27b55-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="27b55-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="27b55-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="27b55-105">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="27b55-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="27b55-106">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="27b55-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="27b55-107">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="27b55-108">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="27b55-109">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="27b55-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="27b55-110">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="27b55-111">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="27b55-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27b55-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b55-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="27b55-113">Kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="27b55-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="27b55-114">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="27b55-114">Name the folder *Models*.</span></span>

<span data-ttu-id="27b55-115">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="27b55-115">Right click the *Models* folder.</span></span> <span data-ttu-id="27b55-116">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="27b55-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="27b55-117">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="27b55-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27b55-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27b55-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="27b55-119">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="27b55-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="27b55-120">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="27b55-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27b55-121">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="27b55-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="27b55-122">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="27b55-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="27b55-123">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="27b55-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="27b55-124">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="27b55-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="27b55-125">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="27b55-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="27b55-126">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="27b55-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="27b55-127">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="27b55-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="27b55-128">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="27b55-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="27b55-129">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="27b55-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="27b55-130">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="27b55-130">Scaffold the movie model</span></span>

<span data-ttu-id="27b55-131">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="27b55-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="27b55-132">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="27b55-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27b55-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b55-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="27b55-134">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="27b55-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="27b55-135">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="27b55-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="27b55-136">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="27b55-136">Name the folder *Movies*</span></span>

<span data-ttu-id="27b55-137">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="27b55-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="27b55-139">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="27b55-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="27b55-141">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="27b55-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="27b55-142">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="27b55-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="27b55-143">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="27b55-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="27b55-144">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="27b55-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="27b55-145">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="27b55-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="27b55-146">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="27b55-146">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

<span data-ttu-id="27b55-148">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27b55-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27b55-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="27b55-150">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="27b55-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="27b55-151">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="27b55-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="27b55-152">**Aby uzyskać Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="27b55-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="27b55-153">**Dla systemu macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="27b55-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27b55-154">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="27b55-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="27b55-155">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="27b55-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="27b55-156">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="27b55-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="27b55-157">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="27b55-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---

### <a name="files-created"></a><span data-ttu-id="27b55-158">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="27b55-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27b55-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b55-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="27b55-160">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="27b55-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="27b55-161">*Strony/filmów*: tworzenie, usuwanie, uzyskać szczegółowe informacje, edytowanie i indeksu.</span><span class="sxs-lookup"><span data-stu-id="27b55-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="27b55-162">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="27b55-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="27b55-163">Zaktualizowano</span><span class="sxs-lookup"><span data-stu-id="27b55-163">Updated</span></span>

* <span data-ttu-id="27b55-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="27b55-164">*Startup.cs*</span></span>

<span data-ttu-id="27b55-165">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="27b55-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="27b55-166">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="27b55-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="27b55-167">Proces tworzenia szkieletu tworzy następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="27b55-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="27b55-168">*Strony/filmów*: tworzenie, usuwanie, uzyskać szczegółowe informacje, edytowanie i indeksu.</span><span class="sxs-lookup"><span data-stu-id="27b55-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="27b55-169">Utworzone pliki zostały wyjaśnione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="27b55-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="27b55-170">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="27b55-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27b55-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b55-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="27b55-172">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="27b55-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="27b55-173">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="27b55-173">Add an initial migration.</span></span>
* <span data-ttu-id="27b55-174">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="27b55-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="27b55-175">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="27b55-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="27b55-177">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="27b55-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27b55-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27b55-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27b55-179">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="27b55-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="27b55-180">Powyższe polecenia generują następujące ostrzeżenie: "nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="27b55-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="27b55-181">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="27b55-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="27b55-182">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="27b55-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="27b55-183">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="27b55-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="27b55-184">Polecenie migrations generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="27b55-185">Schemat jest oparty na modelu określonym w `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="27b55-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="27b55-186">`InitialCreate` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="27b55-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="27b55-187">Można dowolną nazwę, ale zgodnie z Konwencją, wybrane nazwę opisującą migracji.</span><span class="sxs-lookup"><span data-stu-id="27b55-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="27b55-188">`update` polecenie uruchamia metodę `Up` w przypadku migracji, które nie zostały zastosowane.</span><span class="sxs-lookup"><span data-stu-id="27b55-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="27b55-189">W takim przypadku `update` uruchamia metodę `Up` w plikach *migrations/\<sygnatura czasowa > _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27b55-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b55-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="27b55-191">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="27b55-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="27b55-192">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="27b55-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="27b55-193">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27b55-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="27b55-194">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="27b55-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="27b55-195">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="27b55-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="27b55-196">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="27b55-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="27b55-197">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="27b55-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="27b55-198">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="27b55-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="27b55-199">`RazorPagesMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="27b55-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="27b55-200">Kontekst danych (`RazorPagesMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="27b55-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="27b55-201">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="27b55-202">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="27b55-202">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="27b55-203">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="27b55-204">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="27b55-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="27b55-205">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="27b55-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="27b55-206">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="27b55-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27b55-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27b55-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="27b55-208">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="27b55-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27b55-209">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="27b55-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="27b55-210">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="27b55-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="27b55-211">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="27b55-211">Test the app</span></span>

* <span data-ttu-id="27b55-212">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="27b55-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="27b55-213">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="27b55-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="27b55-214">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="27b55-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="27b55-215">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="27b55-215">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="27b55-217">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="27b55-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="27b55-218">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="27b55-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="27b55-219">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="27b55-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="27b55-220">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="27b55-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="27b55-221">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="27b55-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="27b55-222">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="27b55-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="27b55-223">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="27b55-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="27b55-224">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="27b55-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="27b55-225">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="27b55-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="27b55-226">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="27b55-227">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="27b55-228">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="27b55-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="27b55-229">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="27b55-230">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="27b55-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27b55-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b55-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="27b55-232">Kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="27b55-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="27b55-233">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="27b55-233">Name the folder *Models*.</span></span>

<span data-ttu-id="27b55-234">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="27b55-234">Right click the *Models* folder.</span></span> <span data-ttu-id="27b55-235">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="27b55-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="27b55-236">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="27b55-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27b55-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27b55-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="27b55-238">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="27b55-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="27b55-239">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="27b55-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27b55-240">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="27b55-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="27b55-241">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="27b55-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="27b55-242">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="27b55-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="27b55-243">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="27b55-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="27b55-244">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="27b55-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="27b55-245">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="27b55-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="27b55-246">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="27b55-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="27b55-247">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="27b55-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="27b55-248">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="27b55-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="27b55-249">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="27b55-249">Scaffold the movie model</span></span>

<span data-ttu-id="27b55-250">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="27b55-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="27b55-251">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="27b55-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27b55-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b55-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="27b55-253">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="27b55-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="27b55-254">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="27b55-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="27b55-255">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="27b55-255">Name the folder *Movies*</span></span>

<span data-ttu-id="27b55-256">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="27b55-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="27b55-258">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="27b55-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="27b55-260">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="27b55-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="27b55-261">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="27b55-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="27b55-262">W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i zaakceptuj wygenerowaną nazwę **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="27b55-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="27b55-263">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="27b55-263">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="27b55-265">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27b55-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27b55-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="27b55-267">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="27b55-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="27b55-268">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="27b55-268">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="27b55-269">**Aby uzyskać Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="27b55-269">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="27b55-270">**Dla systemu macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="27b55-270">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27b55-271">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="27b55-271">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="27b55-272">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="27b55-272">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="27b55-273">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="27b55-273">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="27b55-274">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="27b55-274">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="27b55-275">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="27b55-275">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="27b55-276">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="27b55-276">Files created</span></span>

* <span data-ttu-id="27b55-277">*Strony/filmów*: tworzenie, usuwanie, uzyskać szczegółowe informacje, edytowanie i indeksu.</span><span class="sxs-lookup"><span data-stu-id="27b55-277">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="27b55-278">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="27b55-278">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="27b55-279">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="27b55-279">File updated</span></span>

* <span data-ttu-id="27b55-280">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="27b55-280">*Startup.cs*</span></span>

<span data-ttu-id="27b55-281">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="27b55-281">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="27b55-282">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="27b55-282">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27b55-283">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b55-283">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="27b55-284">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="27b55-284">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="27b55-285">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="27b55-285">Add an initial migration.</span></span>
* <span data-ttu-id="27b55-286">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="27b55-286">Update the database with the initial migration.</span></span>

<span data-ttu-id="27b55-287">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="27b55-287">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="27b55-289">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="27b55-289">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="27b55-290">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-290">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="27b55-291">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="27b55-291">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="27b55-292">Argument `InitialCreate` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="27b55-292">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="27b55-293">Można dowolną nazwę, ale przez Konwencję na nazwę opisującą migracji jest używana.</span><span class="sxs-lookup"><span data-stu-id="27b55-293">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="27b55-294">Aby uzyskać więcej informacji, zobacz temat <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="27b55-294">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="27b55-295">`Update-Database` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="27b55-295">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="27b55-296">`Up` Metoda tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-296">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27b55-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27b55-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27b55-298">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="27b55-298">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="27b55-299">Powyższe polecenia generują następujące ostrzeżenie: "*nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".* Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="27b55-299">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27b55-300">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b55-300">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="27b55-301">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="27b55-301">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="27b55-302">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="27b55-302">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="27b55-303">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27b55-303">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="27b55-304">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="27b55-304">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="27b55-305">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="27b55-305">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="27b55-306">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="27b55-306">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="27b55-307">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="27b55-307">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="27b55-308">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="27b55-308">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="27b55-309">`RazorPagesMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="27b55-309">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="27b55-310">Kontekst danych (`RazorPagesMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="27b55-310">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="27b55-311">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-311">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="27b55-312">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="27b55-312">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="27b55-313">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="27b55-313">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="27b55-314">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="27b55-314">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="27b55-315">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="27b55-315">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="27b55-316">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="27b55-316">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27b55-317">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27b55-317">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="27b55-318">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="27b55-318">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27b55-319">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="27b55-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="27b55-320">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="27b55-320">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="27b55-321">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="27b55-321">Test the app</span></span>

* <span data-ttu-id="27b55-322">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="27b55-322">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="27b55-323">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="27b55-323">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="27b55-324">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="27b55-324">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="27b55-325">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="27b55-325">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="27b55-327">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="27b55-327">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="27b55-328">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="27b55-328">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="27b55-329">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="27b55-329">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="27b55-330">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="27b55-330">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="27b55-331">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="27b55-331">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="27b55-332">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="27b55-332">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="27b55-333">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="27b55-333">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
