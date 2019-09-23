---
title: Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak można dodać klas związanych z zarządzaniem filmów w bazie danych przy użyciu platformy Entity Framework Core (EF Core).
ms.author: riande
ms.date: 9/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: dcbcf37dfd95d784ebe249ec6e9e4184a8853d3d
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187175"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="6cfd9-103">Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6cfd9-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="6cfd9-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6cfd9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6cfd9-105">W tej sekcji klas są dodawane do zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="6cfd9-106">Te klasy są używane wraz z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="6cfd9-107">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="6cfd9-108">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="6cfd9-109">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="6cfd9-110">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="6cfd9-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cfd9-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cfd9-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cfd9-112">Kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="6cfd9-113">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-113">Name the folder *Models*.</span></span>

<span data-ttu-id="6cfd9-114">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-114">Right click the *Models* folder.</span></span> <span data-ttu-id="6cfd9-115">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="6cfd9-116">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cfd9-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cfd9-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6cfd9-118">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="6cfd9-119">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cfd9-120">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6cfd9-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6cfd9-121">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="6cfd9-122">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="6cfd9-123">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="6cfd9-124">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="6cfd9-125">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="6cfd9-126">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="6cfd9-127">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="6cfd9-128">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="6cfd9-129">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="6cfd9-129">Scaffold the movie model</span></span>

<span data-ttu-id="6cfd9-130">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="6cfd9-131">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cfd9-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cfd9-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cfd9-133">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="6cfd9-134">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="6cfd9-135">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="6cfd9-135">Name the folder *Movies*</span></span>

<span data-ttu-id="6cfd9-136">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="6cfd9-138">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="6cfd9-140">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="6cfd9-141">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="6cfd9-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="6cfd9-142">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus), a następnie zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="6cfd9-143">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="6cfd9-144">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="6cfd9-145">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-145">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

<span data-ttu-id="6cfd9-147">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cfd9-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cfd9-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="6cfd9-149">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6cfd9-150">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6cfd9-151">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="6cfd9-152">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cfd9-153">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6cfd9-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6cfd9-154">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6cfd9-155">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-155">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6cfd9-156">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-156">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="6cfd9-157">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="6cfd9-157">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cfd9-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cfd9-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cfd9-159">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-159">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="6cfd9-160">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-160">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="6cfd9-161">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="6cfd9-161">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="6cfd9-162">aktualny</span><span class="sxs-lookup"><span data-stu-id="6cfd9-162">Updated</span></span>

* <span data-ttu-id="6cfd9-163">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="6cfd9-163">*Startup.cs*</span></span>

<span data-ttu-id="6cfd9-164">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-164">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6cfd9-165">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6cfd9-165">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="6cfd9-166">Proces tworzenia szkieletu tworzy następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-166">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="6cfd9-167">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-167">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="6cfd9-168">Utworzone pliki zostały wyjaśnione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-168">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="6cfd9-169">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="6cfd9-169">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cfd9-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cfd9-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cfd9-171">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-171">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="6cfd9-172">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-172">Add an initial migration.</span></span>
* <span data-ttu-id="6cfd9-173">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-173">Update the database with the initial migration.</span></span>

<span data-ttu-id="6cfd9-174">W menu **Narzędzia** wybierz kolejno pozycje > **Menedżer pakietów NuGet** **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-174">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="6cfd9-176">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-176">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cfd9-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cfd9-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cfd9-178">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6cfd9-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="6cfd9-179">Powyższe polecenia generują następujące ostrzeżenie: "Nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="6cfd9-179">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="6cfd9-180">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="6cfd9-181">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="6cfd9-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="6cfd9-182">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-182">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="6cfd9-183">`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-183">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="6cfd9-184">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-184">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="6cfd9-185">`InitialCreate` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-185">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="6cfd9-186">Można dowolną nazwę, ale zgodnie z Konwencją, wybrane nazwę opisującą migracji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-186">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="6cfd9-187">`ef database update` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-187">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="6cfd9-188">`Up` Metoda tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-188">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cfd9-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cfd9-189">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="6cfd9-190">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="6cfd9-190">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="6cfd9-191">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-191">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6cfd9-192">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-192">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="6cfd9-193">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-193">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="6cfd9-194">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-194">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="6cfd9-195">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-195">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="6cfd9-196">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-196">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6cfd9-197">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-197">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="6cfd9-198">`RazorPagesMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-198">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="6cfd9-199">Kontekst danych (`RazorPagesMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-199">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="6cfd9-200">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-200">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="6cfd9-201">Poprzedni kod tworzy [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) Właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-201">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="6cfd9-202">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-202">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="6cfd9-203">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-203">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="6cfd9-204">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-204">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="6cfd9-205">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-205">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cfd9-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cfd9-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6cfd9-207">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-207">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cfd9-208">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6cfd9-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6cfd9-209">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-209">Examine the `Up` method.</span></span>

---

<span data-ttu-id="6cfd9-210">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-210">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="6cfd9-211">Schemat jest oparta na modelu, określone w `RazorPagesMovieContext` (w *Data/RazorPagesMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-211">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="6cfd9-212">`Initial` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-212">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="6cfd9-213">Można dowolną nazwę, ale przez Konwencję na nazwę opisującą migracji jest używana.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-213">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="6cfd9-214">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-214">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="6cfd9-215">`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-215">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="6cfd9-216">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="6cfd9-216">Test the app</span></span>

* <span data-ttu-id="6cfd9-217">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-217">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="6cfd9-218">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-218">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="6cfd9-219">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-219">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="6cfd9-220">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-220">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="6cfd9-222">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-222">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="6cfd9-223">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-223">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="6cfd9-224">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-224">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="6cfd9-225">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-225">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="6cfd9-226">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-226">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6cfd9-227">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6cfd9-227">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6cfd9-228">[Ubiegł ](xref:tutorials/razor-pages/razor-pages-start)Rozpocznijdalej[:
>  Razor Pages szkieletowe](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="6cfd9-228">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6cfd9-229">W tej sekcji klas są dodawane do zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-229">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="6cfd9-230">Te klasy są używane wraz z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-230">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="6cfd9-231">EF Core to platforma mapowania obiektowo relacyjny (ORM), która upraszcza kod dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-231">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="6cfd9-232">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-232">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="6cfd9-233">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-233">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="6cfd9-234">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="6cfd9-234">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cfd9-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cfd9-235">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cfd9-236">Kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-236">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="6cfd9-237">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-237">Name the folder *Models*.</span></span>

<span data-ttu-id="6cfd9-238">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-238">Right click the *Models* folder.</span></span> <span data-ttu-id="6cfd9-239">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-239">Select **Add** > **Class**.</span></span> <span data-ttu-id="6cfd9-240">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-240">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cfd9-241">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cfd9-241">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6cfd9-242">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-242">Add a folder named *Models*.</span></span>
* <span data-ttu-id="6cfd9-243">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-243">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cfd9-244">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6cfd9-244">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6cfd9-245">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-245">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="6cfd9-246">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-246">Name the folder *Models*.</span></span>
* <span data-ttu-id="6cfd9-247">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-247">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="6cfd9-248">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-248">In the **New File** dialog:</span></span>

  * <span data-ttu-id="6cfd9-249">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-249">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="6cfd9-250">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-250">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="6cfd9-251">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-251">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="6cfd9-252">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-252">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="6cfd9-253">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="6cfd9-253">Scaffold the movie model</span></span>

<span data-ttu-id="6cfd9-254">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-254">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="6cfd9-255">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-255">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cfd9-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cfd9-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cfd9-257">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-257">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="6cfd9-258">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-258">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="6cfd9-259">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="6cfd9-259">Name the folder *Movies*</span></span>

<span data-ttu-id="6cfd9-260">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-260">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="6cfd9-262">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-262">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="6cfd9-264">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-264">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="6cfd9-265">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="6cfd9-265">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="6cfd9-266">W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i zaakceptuj wygenerowaną nazwę **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-266">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="6cfd9-267">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-267">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="6cfd9-269">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-269">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cfd9-270">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cfd9-270">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="6cfd9-271">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6cfd9-272">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-272">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6cfd9-273">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-273">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="6cfd9-274">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-274">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cfd9-275">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6cfd9-275">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6cfd9-276">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-276">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6cfd9-277">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-277">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6cfd9-278">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-278">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="6cfd9-279">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-279">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="6cfd9-280">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="6cfd9-280">Files created</span></span>

* <span data-ttu-id="6cfd9-281">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-281">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="6cfd9-282">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="6cfd9-282">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="6cfd9-283">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="6cfd9-283">File updated</span></span>

* <span data-ttu-id="6cfd9-284">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="6cfd9-284">*Startup.cs*</span></span>

<span data-ttu-id="6cfd9-285">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-285">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="6cfd9-286">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="6cfd9-286">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cfd9-287">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cfd9-287">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cfd9-288">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-288">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="6cfd9-289">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-289">Add an initial migration.</span></span>
* <span data-ttu-id="6cfd9-290">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-290">Update the database with the initial migration.</span></span>

<span data-ttu-id="6cfd9-291">W menu **Narzędzia** wybierz kolejno pozycje > **Menedżer pakietów NuGet** **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-291">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="6cfd9-293">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-293">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cfd9-294">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cfd9-294">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cfd9-295">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6cfd9-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="6cfd9-296">Powyższe polecenia generują następujące ostrzeżenie: "Nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="6cfd9-296">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="6cfd9-297">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-297">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="6cfd9-298">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="6cfd9-298">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="6cfd9-299">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-299">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="6cfd9-300">`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-300">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="6cfd9-301">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-301">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="6cfd9-302">`InitialCreate` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-302">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="6cfd9-303">Można dowolną nazwę, ale zgodnie z Konwencją, wybrane nazwę opisującą migracji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-303">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="6cfd9-304">`ef database update` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-304">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="6cfd9-305">`Up` Metoda tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-305">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cfd9-306">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cfd9-306">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="6cfd9-307">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="6cfd9-307">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="6cfd9-308">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-308">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6cfd9-309">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-309">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="6cfd9-310">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-310">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="6cfd9-311">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-311">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="6cfd9-312">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-312">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="6cfd9-313">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-313">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6cfd9-314">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-314">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="6cfd9-315">`RazorPagesMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-315">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="6cfd9-316">Kontekst danych (`RazorPagesMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-316">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="6cfd9-317">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-317">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="6cfd9-318">Poprzedni kod tworzy [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) Właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-318">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="6cfd9-319">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-319">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="6cfd9-320">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-320">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="6cfd9-321">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-321">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="6cfd9-322">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-322">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cfd9-323">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cfd9-323">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6cfd9-324">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-324">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cfd9-325">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6cfd9-325">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6cfd9-326">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-326">Examine the `Up` method.</span></span>

---

<span data-ttu-id="6cfd9-327">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-327">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="6cfd9-328">Schemat jest oparta na modelu, określone w `RazorPagesMovieContext` (w *Data/RazorPagesMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-328">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="6cfd9-329">`Initial` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-329">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="6cfd9-330">Można dowolną nazwę, ale przez Konwencję na nazwę opisującą migracji jest używana.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-330">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="6cfd9-331">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-331">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="6cfd9-332">`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-332">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="6cfd9-333">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="6cfd9-333">Test the app</span></span>

* <span data-ttu-id="6cfd9-334">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-334">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="6cfd9-335">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-335">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="6cfd9-336">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-336">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="6cfd9-337">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-337">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="6cfd9-339">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-339">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="6cfd9-340">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-340">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="6cfd9-341">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-341">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="6cfd9-342">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-342">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="6cfd9-343">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-343">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6cfd9-344">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6cfd9-344">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6cfd9-345">[Ubiegł ](xref:tutorials/razor-pages/razor-pages-start)Rozpocznijdalej[:
>  Razor Pages szkieletowe](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="6cfd9-345">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
