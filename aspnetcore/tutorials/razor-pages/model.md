---
title: Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak można dodać klas związanych z zarządzaniem filmów w bazie danych przy użyciu platformy Entity Framework Core (EF Core).
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: b7f77cfa51f8d86504939e31eade0dfda8a6b1c9
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371917"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="39ec8-103">Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39ec8-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="39ec8-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="39ec8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="39ec8-105">W tej sekcji klas są dodawane do zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="39ec8-106">Te klasy są używane wraz z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="39ec8-107">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="39ec8-108">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="39ec8-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="39ec8-109">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="39ec8-110">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="39ec8-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="39ec8-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39ec8-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="39ec8-112">Kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="39ec8-113">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="39ec8-113">Name the folder *Models*.</span></span>

<span data-ttu-id="39ec8-114">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="39ec8-114">Right click the *Models* folder.</span></span> <span data-ttu-id="39ec8-115">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="39ec8-116">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="39ec8-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39ec8-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="39ec8-118">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="39ec8-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="39ec8-119">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="39ec8-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="39ec8-120">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="39ec8-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="39ec8-121">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="39ec8-122">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="39ec8-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="39ec8-123">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="39ec8-124">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="39ec8-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="39ec8-125">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="39ec8-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="39ec8-126">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="39ec8-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="39ec8-127">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="39ec8-128">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="39ec8-129">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="39ec8-129">Scaffold the movie model</span></span>

<span data-ttu-id="39ec8-130">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="39ec8-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="39ec8-131">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="39ec8-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="39ec8-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39ec8-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="39ec8-133">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="39ec8-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="39ec8-134">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="39ec8-135">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="39ec8-135">Name the folder *Movies*</span></span>

<span data-ttu-id="39ec8-136">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="39ec8-138">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="39ec8-140">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="39ec8-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="39ec8-141">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="39ec8-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="39ec8-142">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus), a następnie zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="39ec8-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="39ec8-143">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="39ec8-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="39ec8-144">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="39ec8-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="39ec8-145">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-145">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

<span data-ttu-id="39ec8-147">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="39ec8-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39ec8-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="39ec8-149">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="39ec8-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="39ec8-150">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="39ec8-150">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="39ec8-151">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="39ec8-151">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="39ec8-152">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="39ec8-152">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="39ec8-153">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="39ec8-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="39ec8-154">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="39ec8-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="39ec8-155">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="39ec8-155">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="39ec8-156">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="39ec8-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="39ec8-157">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="39ec8-157">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="39ec8-158">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="39ec8-158">Files created</span></span>

* <span data-ttu-id="39ec8-159">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="39ec8-159">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="39ec8-160">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="39ec8-160">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="39ec8-161">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="39ec8-161">File updated</span></span>

* <span data-ttu-id="39ec8-162">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="39ec8-162">*Startup.cs*</span></span>

<span data-ttu-id="39ec8-163">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-163">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="39ec8-164">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="39ec8-164">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="39ec8-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39ec8-165">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="39ec8-166">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="39ec8-166">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="39ec8-167">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-167">Add an initial migration.</span></span>
* <span data-ttu-id="39ec8-168">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-168">Update the database with the initial migration.</span></span>

<span data-ttu-id="39ec8-169">W menu **Narzędzia** wybierz kolejno pozycje > **Menedżer pakietów NuGet** **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-169">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="39ec8-171">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="39ec8-171">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="39ec8-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39ec8-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="39ec8-173">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="39ec8-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="39ec8-174">Powyższe polecenia generują następujące ostrzeżenie: "Nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="39ec8-174">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="39ec8-175">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="39ec8-175">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="39ec8-176">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="39ec8-176">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="39ec8-177">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="39ec8-177">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="39ec8-178">`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-178">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="39ec8-179">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="39ec8-179">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="39ec8-180">`InitialCreate` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="39ec8-180">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="39ec8-181">Można dowolną nazwę, ale zgodnie z Konwencją, wybrane nazwę opisującą migracji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-181">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="39ec8-182">`ef database update` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="39ec8-182">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="39ec8-183">`Up` Metoda tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-183">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="39ec8-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39ec8-184">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="39ec8-185">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="39ec8-185">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="39ec8-186">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="39ec8-186">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="39ec8-187">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-187">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="39ec8-188">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="39ec8-188">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="39ec8-189">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="39ec8-189">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="39ec8-190">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="39ec8-190">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="39ec8-191">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="39ec8-191">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="39ec8-192">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="39ec8-192">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="39ec8-193">`RazorPagesMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="39ec8-193">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="39ec8-194">Kontekst danych (`RazorPagesMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="39ec8-194">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="39ec8-195">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-195">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="39ec8-196">Poprzedni kod tworzy [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) Właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="39ec8-196">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="39ec8-197">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-197">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="39ec8-198">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="39ec8-198">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="39ec8-199">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="39ec8-199">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="39ec8-200">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="39ec8-200">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="39ec8-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39ec8-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="39ec8-202">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="39ec8-202">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="39ec8-203">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="39ec8-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="39ec8-204">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="39ec8-204">Examine the `Up` method.</span></span>

---

<span data-ttu-id="39ec8-205">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-205">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="39ec8-206">Schemat jest oparta na modelu, określone w `RazorPagesMovieContext` (w *Data/RazorPagesMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="39ec8-206">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="39ec8-207">`Initial` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="39ec8-207">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="39ec8-208">Można dowolną nazwę, ale przez Konwencję na nazwę opisującą migracji jest używana.</span><span class="sxs-lookup"><span data-stu-id="39ec8-208">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="39ec8-209">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="39ec8-209">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="39ec8-210">`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-210">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="39ec8-211">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="39ec8-211">Test the app</span></span>

* <span data-ttu-id="39ec8-212">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="39ec8-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="39ec8-213">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="39ec8-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="39ec8-214">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="39ec8-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="39ec8-215">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="39ec8-215">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="39ec8-217">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="39ec8-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="39ec8-218">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="39ec8-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="39ec8-219">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="39ec8-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="39ec8-220">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="39ec8-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="39ec8-221">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="39ec8-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39ec8-222">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="39ec8-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="39ec8-223">[Ubiegł ](xref:tutorials/razor-pages/razor-pages-start)Rozpocznijdalej[:
>  Razor Pages szkieletowe](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="39ec8-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="39ec8-224">W tej sekcji klas są dodawane do zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-224">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="39ec8-225">Te klasy są używane wraz z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-225">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="39ec8-226">EF Core to platforma mapowania obiektowo relacyjny (ORM), która upraszcza kod dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-226">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="39ec8-227">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="39ec8-227">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="39ec8-228">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-228">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="39ec8-229">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="39ec8-229">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="39ec8-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39ec8-230">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="39ec8-231">Kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-231">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="39ec8-232">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="39ec8-232">Name the folder *Models*.</span></span>

<span data-ttu-id="39ec8-233">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="39ec8-233">Right click the *Models* folder.</span></span> <span data-ttu-id="39ec8-234">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-234">Select **Add** > **Class**.</span></span> <span data-ttu-id="39ec8-235">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-235">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="39ec8-236">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39ec8-236">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="39ec8-237">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="39ec8-237">Add a folder named *Models*.</span></span>
* <span data-ttu-id="39ec8-238">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="39ec8-238">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="39ec8-239">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="39ec8-239">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="39ec8-240">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-240">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="39ec8-241">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="39ec8-241">Name the folder *Models*.</span></span>
* <span data-ttu-id="39ec8-242">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-242">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="39ec8-243">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="39ec8-243">In the **New File** dialog:</span></span>

  * <span data-ttu-id="39ec8-244">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="39ec8-244">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="39ec8-245">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="39ec8-245">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="39ec8-246">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-246">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="39ec8-247">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-247">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="39ec8-248">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="39ec8-248">Scaffold the movie model</span></span>

<span data-ttu-id="39ec8-249">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="39ec8-249">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="39ec8-250">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="39ec8-250">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="39ec8-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39ec8-251">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="39ec8-252">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="39ec8-252">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="39ec8-253">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-253">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="39ec8-254">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="39ec8-254">Name the folder *Movies*</span></span>

<span data-ttu-id="39ec8-255">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-255">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="39ec8-257">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-257">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="39ec8-259">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="39ec8-259">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="39ec8-260">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="39ec8-260">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="39ec8-261">W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i zaakceptuj wygenerowaną nazwę **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-261">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="39ec8-262">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-262">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="39ec8-264">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-264">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="39ec8-265">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39ec8-265">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="39ec8-266">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="39ec8-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="39ec8-267">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="39ec8-267">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="39ec8-268">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="39ec8-268">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="39ec8-269">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="39ec8-269">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="39ec8-270">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="39ec8-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="39ec8-271">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="39ec8-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="39ec8-272">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="39ec8-272">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="39ec8-273">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="39ec8-273">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="39ec8-274">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="39ec8-274">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="39ec8-275">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="39ec8-275">Files created</span></span>

* <span data-ttu-id="39ec8-276">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="39ec8-276">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="39ec8-277">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="39ec8-277">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="39ec8-278">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="39ec8-278">File updated</span></span>

* <span data-ttu-id="39ec8-279">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="39ec8-279">*Startup.cs*</span></span>

<span data-ttu-id="39ec8-280">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-280">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="39ec8-281">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="39ec8-281">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="39ec8-282">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39ec8-282">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="39ec8-283">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="39ec8-283">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="39ec8-284">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-284">Add an initial migration.</span></span>
* <span data-ttu-id="39ec8-285">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-285">Update the database with the initial migration.</span></span>

<span data-ttu-id="39ec8-286">W menu **Narzędzia** wybierz kolejno pozycje > **Menedżer pakietów NuGet** **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="39ec8-286">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="39ec8-288">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="39ec8-288">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="39ec8-289">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39ec8-289">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="39ec8-290">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="39ec8-290">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="39ec8-291">Powyższe polecenia generują następujące ostrzeżenie: "Nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="39ec8-291">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="39ec8-292">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="39ec8-292">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="39ec8-293">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="39ec8-293">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="39ec8-294">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="39ec8-294">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="39ec8-295">`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-295">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="39ec8-296">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="39ec8-296">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="39ec8-297">`InitialCreate` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="39ec8-297">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="39ec8-298">Można dowolną nazwę, ale zgodnie z Konwencją, wybrane nazwę opisującą migracji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-298">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="39ec8-299">`ef database update` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="39ec8-299">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="39ec8-300">`Up` Metoda tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-300">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="39ec8-301">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39ec8-301">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="39ec8-302">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="39ec8-302">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="39ec8-303">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="39ec8-303">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="39ec8-304">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39ec8-304">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="39ec8-305">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="39ec8-305">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="39ec8-306">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="39ec8-306">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="39ec8-307">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="39ec8-307">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="39ec8-308">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="39ec8-308">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="39ec8-309">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="39ec8-309">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="39ec8-310">`RazorPagesMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="39ec8-310">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="39ec8-311">Kontekst danych (`RazorPagesMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="39ec8-311">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="39ec8-312">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-312">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="39ec8-313">Poprzedni kod tworzy [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) Właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="39ec8-313">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="39ec8-314">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-314">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="39ec8-315">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="39ec8-315">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="39ec8-316">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="39ec8-316">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="39ec8-317">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="39ec8-317">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="39ec8-318">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39ec8-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="39ec8-319">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="39ec8-319">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="39ec8-320">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="39ec8-320">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="39ec8-321">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="39ec8-321">Examine the `Up` method.</span></span>

---

<span data-ttu-id="39ec8-322">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-322">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="39ec8-323">Schemat jest oparta na modelu, określone w `RazorPagesMovieContext` (w *Data/RazorPagesMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="39ec8-323">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="39ec8-324">`Initial` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="39ec8-324">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="39ec8-325">Można dowolną nazwę, ale przez Konwencję na nazwę opisującą migracji jest używana.</span><span class="sxs-lookup"><span data-stu-id="39ec8-325">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="39ec8-326">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="39ec8-326">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="39ec8-327">`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="39ec8-327">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="39ec8-328">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="39ec8-328">Test the app</span></span>

* <span data-ttu-id="39ec8-329">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="39ec8-329">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="39ec8-330">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="39ec8-330">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="39ec8-331">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="39ec8-331">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="39ec8-332">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="39ec8-332">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="39ec8-334">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="39ec8-334">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="39ec8-335">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="39ec8-335">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="39ec8-336">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="39ec8-336">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="39ec8-337">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="39ec8-337">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="39ec8-338">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="39ec8-338">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39ec8-339">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="39ec8-339">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="39ec8-340">[Ubiegł ](xref:tutorials/razor-pages/razor-pages-start)Rozpocznijdalej[:
>  Razor Pages szkieletowe](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="39ec8-340">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end