---
title: Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak można dodać klas związanych z zarządzaniem filmów w bazie danych przy użyciu platformy Entity Framework Core (EF Core).
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
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="5aac8-103">Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5aac8-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="5aac8-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5aac8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5aac8-105">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="5aac8-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="5aac8-106">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="5aac8-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="5aac8-107">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="5aac8-108">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="5aac8-109">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="5aac8-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="5aac8-110">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="5aac8-111">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="5aac8-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5aac8-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5aac8-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5aac8-113">Kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="5aac8-114">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5aac8-114">Name the folder *Models*.</span></span>

<span data-ttu-id="5aac8-115">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="5aac8-115">Right click the *Models* folder.</span></span> <span data-ttu-id="5aac8-116">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="5aac8-117">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5aac8-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5aac8-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5aac8-119">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5aac8-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="5aac8-120">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="5aac8-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5aac8-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5aac8-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5aac8-122">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="5aac8-123">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5aac8-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="5aac8-124">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="5aac8-125">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="5aac8-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="5aac8-126">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="5aac8-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="5aac8-127">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="5aac8-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="5aac8-128">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="5aac8-129">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="5aac8-130">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="5aac8-130">Scaffold the movie model</span></span>

<span data-ttu-id="5aac8-131">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="5aac8-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="5aac8-132">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="5aac8-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5aac8-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5aac8-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5aac8-134">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="5aac8-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="5aac8-135">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="5aac8-136">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="5aac8-136">Name the folder *Movies*</span></span>

<span data-ttu-id="5aac8-137">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="5aac8-139">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="5aac8-141">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="5aac8-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="5aac8-142">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="5aac8-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="5aac8-143">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="5aac8-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="5aac8-144">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="5aac8-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="5aac8-145">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="5aac8-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="5aac8-146">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-146">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

<span data-ttu-id="5aac8-148">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5aac8-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5aac8-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="5aac8-150">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="5aac8-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5aac8-151">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="5aac8-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="5aac8-152">**Aby uzyskać Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5aac8-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="5aac8-153">**Dla systemu macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5aac8-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5aac8-154">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5aac8-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5aac8-155">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="5aac8-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5aac8-156">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="5aac8-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="5aac8-157">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5aac8-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---

### <a name="files-created"></a><span data-ttu-id="5aac8-158">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="5aac8-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5aac8-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5aac8-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5aac8-160">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="5aac8-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="5aac8-161">*Strony/filmów*: tworzenie, usuwanie, uzyskać szczegółowe informacje, edytowanie i indeksu.</span><span class="sxs-lookup"><span data-stu-id="5aac8-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="5aac8-162">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="5aac8-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="5aac8-163">Aktualny</span><span class="sxs-lookup"><span data-stu-id="5aac8-163">Updated</span></span>

* <span data-ttu-id="5aac8-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="5aac8-164">*Startup.cs*</span></span>

<span data-ttu-id="5aac8-165">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5aac8-166">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5aac8-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="5aac8-167">Proces tworzenia szkieletu tworzy następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="5aac8-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="5aac8-168">*Strony/filmów*: tworzenie, usuwanie, uzyskać szczegółowe informacje, edytowanie i indeksu.</span><span class="sxs-lookup"><span data-stu-id="5aac8-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="5aac8-169">Utworzone pliki zostały wyjaśnione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="5aac8-170">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="5aac8-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5aac8-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5aac8-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5aac8-172">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="5aac8-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="5aac8-173">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-173">Add an initial migration.</span></span>
* <span data-ttu-id="5aac8-174">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="5aac8-175">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="5aac8-177">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5aac8-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5aac8-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5aac8-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5aac8-179">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5aac8-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="5aac8-180">Powyższe polecenia generują następujące ostrzeżenie: "nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="5aac8-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="5aac8-181">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="5aac8-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="5aac8-182">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="5aac8-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="5aac8-183">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5aac8-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="5aac8-184">Polecenie migrations generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="5aac8-185">Schemat jest oparty na modelu określonym w `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="5aac8-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="5aac8-186">`InitialCreate` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="5aac8-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="5aac8-187">Można dowolną nazwę, ale zgodnie z Konwencją, wybrane nazwę opisującą migracji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="5aac8-188">`update` polecenie uruchamia metodę `Up` w przypadku migracji, które nie zostały zastosowane.</span><span class="sxs-lookup"><span data-stu-id="5aac8-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="5aac8-189">W takim przypadku `update` uruchamia metodę `Up` w plikach *migrations/\<sygnatura czasowa > _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5aac8-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5aac8-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="5aac8-191">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="5aac8-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="5aac8-192">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5aac8-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5aac8-193">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="5aac8-194">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="5aac8-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="5aac8-195">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="5aac8-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="5aac8-196">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="5aac8-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="5aac8-197">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="5aac8-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="5aac8-198">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="5aac8-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="5aac8-199">`RazorPagesMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="5aac8-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="5aac8-200">Kontekst danych (`RazorPagesMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="5aac8-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="5aac8-201">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="5aac8-202">Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="5aac8-202">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="5aac8-203">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="5aac8-204">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="5aac8-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="5aac8-205">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="5aac8-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="5aac8-206">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="5aac8-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5aac8-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5aac8-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5aac8-208">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="5aac8-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5aac8-209">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5aac8-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5aac8-210">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="5aac8-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="5aac8-211">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5aac8-211">Test the app</span></span>

* <span data-ttu-id="5aac8-212">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="5aac8-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="5aac8-213">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="5aac8-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="5aac8-214">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="5aac8-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="5aac8-215">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="5aac8-215">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="5aac8-217">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="5aac8-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="5aac8-218">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="5aac8-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="5aac8-219">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="5aac8-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="5aac8-220">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="5aac8-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="5aac8-221">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="5aac8-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5aac8-222">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5aac8-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5aac8-223">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="5aac8-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5aac8-224">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="5aac8-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="5aac8-225">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="5aac8-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="5aac8-226">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="5aac8-227">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="5aac8-228">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="5aac8-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="5aac8-229">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="5aac8-230">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="5aac8-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5aac8-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5aac8-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5aac8-232">Kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="5aac8-233">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5aac8-233">Name the folder *Models*.</span></span>

<span data-ttu-id="5aac8-234">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="5aac8-234">Right click the *Models* folder.</span></span> <span data-ttu-id="5aac8-235">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="5aac8-236">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5aac8-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5aac8-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5aac8-238">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5aac8-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="5aac8-239">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="5aac8-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5aac8-240">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5aac8-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5aac8-241">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="5aac8-242">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5aac8-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="5aac8-243">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="5aac8-244">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="5aac8-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="5aac8-245">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="5aac8-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="5aac8-246">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="5aac8-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="5aac8-247">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="5aac8-248">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="5aac8-249">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="5aac8-249">Scaffold the movie model</span></span>

<span data-ttu-id="5aac8-250">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="5aac8-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="5aac8-251">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="5aac8-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5aac8-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5aac8-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5aac8-253">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="5aac8-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="5aac8-254">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="5aac8-255">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="5aac8-255">Name the folder *Movies*</span></span>

<span data-ttu-id="5aac8-256">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="5aac8-258">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="5aac8-260">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="5aac8-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="5aac8-261">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="5aac8-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="5aac8-262">W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i zaakceptuj wygenerowaną nazwę **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="5aac8-263">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-263">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="5aac8-265">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5aac8-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5aac8-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="5aac8-267">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="5aac8-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5aac8-268">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="5aac8-268">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="5aac8-269">**Aby uzyskać Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5aac8-269">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="5aac8-270">**Dla systemu macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5aac8-270">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5aac8-271">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5aac8-271">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5aac8-272">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="5aac8-272">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5aac8-273">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="5aac8-273">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="5aac8-274">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5aac8-274">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="5aac8-275">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="5aac8-275">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="5aac8-276">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="5aac8-276">Files created</span></span>

* <span data-ttu-id="5aac8-277">*Strony/filmów*: tworzenie, usuwanie, uzyskać szczegółowe informacje, edytowanie i indeksu.</span><span class="sxs-lookup"><span data-stu-id="5aac8-277">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="5aac8-278">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="5aac8-278">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="5aac8-279">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="5aac8-279">File updated</span></span>

* <span data-ttu-id="5aac8-280">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="5aac8-280">*Startup.cs*</span></span>

<span data-ttu-id="5aac8-281">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-281">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="5aac8-282">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="5aac8-282">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5aac8-283">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5aac8-283">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5aac8-284">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="5aac8-284">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="5aac8-285">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-285">Add an initial migration.</span></span>
* <span data-ttu-id="5aac8-286">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-286">Update the database with the initial migration.</span></span>

<span data-ttu-id="5aac8-287">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="5aac8-287">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="5aac8-289">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5aac8-289">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="5aac8-290">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-290">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="5aac8-291">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="5aac8-291">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="5aac8-292">Argument `InitialCreate` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-292">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="5aac8-293">Można dowolną nazwę, ale przez Konwencję na nazwę opisującą migracji jest używana.</span><span class="sxs-lookup"><span data-stu-id="5aac8-293">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="5aac8-294">Aby uzyskać więcej informacji, zobacz temat <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="5aac8-294">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="5aac8-295">`Update-Database` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="5aac8-295">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="5aac8-296">`Up` Metoda tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-296">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5aac8-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5aac8-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5aac8-298">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5aac8-298">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="5aac8-299">Powyższe polecenia generują następujące ostrzeżenie: "*nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".* Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5aac8-299">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5aac8-300">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5aac8-300">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="5aac8-301">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="5aac8-301">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="5aac8-302">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5aac8-302">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5aac8-303">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5aac8-303">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="5aac8-304">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="5aac8-304">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="5aac8-305">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="5aac8-305">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="5aac8-306">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="5aac8-306">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="5aac8-307">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="5aac8-307">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="5aac8-308">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="5aac8-308">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="5aac8-309">`RazorPagesMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="5aac8-309">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="5aac8-310">Kontekst danych (`RazorPagesMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="5aac8-310">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="5aac8-311">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-311">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="5aac8-312">Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="5aac8-312">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="5aac8-313">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5aac8-313">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="5aac8-314">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="5aac8-314">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="5aac8-315">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="5aac8-315">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="5aac8-316">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="5aac8-316">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5aac8-317">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5aac8-317">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5aac8-318">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="5aac8-318">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5aac8-319">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5aac8-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5aac8-320">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="5aac8-320">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="5aac8-321">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5aac8-321">Test the app</span></span>

* <span data-ttu-id="5aac8-322">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="5aac8-322">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="5aac8-323">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="5aac8-323">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="5aac8-324">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="5aac8-324">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="5aac8-325">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="5aac8-325">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="5aac8-327">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="5aac8-327">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="5aac8-328">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="5aac8-328">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="5aac8-329">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="5aac8-329">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="5aac8-330">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="5aac8-330">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="5aac8-331">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="5aac8-331">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5aac8-332">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5aac8-332">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5aac8-333">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="5aac8-333">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
