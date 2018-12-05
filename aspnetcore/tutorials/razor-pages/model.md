---
title: Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak można dodać klas związanych z zarządzaniem filmów w bazie danych przy użyciu platformy Entity Framework Core (EF Core).
ms.author: riande
monikerRange: '>= aspnetcore-2.2'
ms.date: 12/3/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 3c9b96c8e56672a3c4268d8cd507f30127a812dd
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861475"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="48ef9-103">Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48ef9-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="48ef9-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="48ef9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="48ef9-105">W tej sekcji klas są dodawane do zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="48ef9-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="48ef9-106">Te klasy są używane wraz z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="48ef9-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="48ef9-107">EF Core to platforma mapowania obiektowo relacyjny (ORM), która upraszcza kod dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="48ef9-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="48ef9-108">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="48ef9-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="48ef9-109">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="48ef9-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="48ef9-110">[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages-start/sample/) próbki.</span><span class="sxs-lookup"><span data-stu-id="48ef9-110">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages-start/sample/) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="48ef9-111">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="48ef9-111">Add a data model</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48ef9-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ef9-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48ef9-113">Kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="48ef9-114">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="48ef9-114">Name the folder *Models*.</span></span>

<span data-ttu-id="48ef9-115">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="48ef9-115">Right click the *Models* folder.</span></span> <span data-ttu-id="48ef9-116">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="48ef9-117">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48ef9-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ef9-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="48ef9-119">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="48ef9-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="48ef9-120">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="48ef9-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48ef9-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="48ef9-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="48ef9-122">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="48ef9-123">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="48ef9-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="48ef9-124">Kliknij prawym przyciskiem myszy *modeli* folder, a następnie wybierz **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="48ef9-125">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="48ef9-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="48ef9-126">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="48ef9-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="48ef9-127">Wybierz **pustą klasę** w ból Centrum.</span><span class="sxs-lookup"><span data-stu-id="48ef9-127">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="48ef9-128">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

<span data-ttu-id="48ef9-129">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="48ef9-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="48ef9-130">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="48ef9-130">Scaffold the movie model</span></span>

<span data-ttu-id="48ef9-131">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="48ef9-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="48ef9-132">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="48ef9-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48ef9-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ef9-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48ef9-134">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="48ef9-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="48ef9-135">Kliknij prawym przyciskiem myszy *stron* folder > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="48ef9-136">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="48ef9-136">Name the folder *Movies*</span></span>

<span data-ttu-id="48ef9-137">Kliknij prawym przyciskiem myszy *stron/filmów* folder > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="48ef9-139">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **strony Razor za pomocą programu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="48ef9-141">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="48ef9-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="48ef9-142">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="48ef9-143">W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i zaakceptuj wygenerowaną nazwę **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="48ef9-144">Wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-144">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="48ef9-146">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48ef9-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48ef9-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ef9-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="48ef9-148">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="48ef9-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="48ef9-149">**Aby uzyskać Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="48ef9-149">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="48ef9-150">**Dla systemu macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="48ef9-150">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48ef9-151">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="48ef9-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="48ef9-152">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="48ef9-152">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="48ef9-153">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="48ef9-153">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="48ef9-154">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="48ef9-154">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="48ef9-155">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="48ef9-155">Files created</span></span>

* <span data-ttu-id="48ef9-156">*Strony/filmów*: tworzenie, usuwanie, uzyskać szczegółowe informacje, edytowanie i indeksu.</span><span class="sxs-lookup"><span data-stu-id="48ef9-156">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="48ef9-157">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="48ef9-157">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="48ef9-158">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="48ef9-158">File updated</span></span>

* <span data-ttu-id="48ef9-159">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="48ef9-159">*Startup.cs*</span></span>

<span data-ttu-id="48ef9-160">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="48ef9-160">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="48ef9-161">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="48ef9-161">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48ef9-162">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ef9-162">Visual Studio</span></span>](#tab/visual-studio)

<!-- VS -------------------------->

<span data-ttu-id="48ef9-163">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="48ef9-163">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="48ef9-164">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="48ef9-164">Add an initial migration.</span></span>
* <span data-ttu-id="48ef9-165">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="48ef9-165">Update the database with the initial migration.</span></span>

<span data-ttu-id="48ef9-166">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="48ef9-166">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="48ef9-168">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="48ef9-168">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48ef9-169">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ef9-169">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48ef9-170">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="48ef9-170">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="48ef9-171">`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48ef9-171">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="48ef9-172">Schemat jest oparta na modelu, określone w `DbContext` (w *Models/RazorPagesMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="48ef9-172">The schema is based on the model specified in the `DbContext` (In the *Models/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="48ef9-173">`InitialCreate` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="48ef9-173">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="48ef9-174">Można dowolną nazwę, ale zgodnie z Konwencją, wybrane nazwę opisującą migracji.</span><span class="sxs-lookup"><span data-stu-id="48ef9-174">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="48ef9-175">`ef database update` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="48ef9-175">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="48ef9-176">`Up` Metoda tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="48ef9-176">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48ef9-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ef9-177">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="48ef9-178">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="48ef9-178">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="48ef9-179">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="48ef9-179">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="48ef9-180">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48ef9-180">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="48ef9-181">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="48ef9-181">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="48ef9-182">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="48ef9-182">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="48ef9-183">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="48ef9-183">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="48ef9-184">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="48ef9-184">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="48ef9-185">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="48ef9-185">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="48ef9-186">`RazorPagesMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="48ef9-186">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="48ef9-187">Kontekst danych (`RazorPagesMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="48ef9-187">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="48ef9-188">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="48ef9-188">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="48ef9-189">Powyższy kod tworzy [DbSet /\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="48ef9-189">The preceding code creates a [DbSet/\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="48ef9-190">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48ef9-190">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="48ef9-191">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="48ef9-191">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="48ef9-192">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="48ef9-192">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="48ef9-193">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="48ef9-193">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48ef9-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ef9-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48ef9-195">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="48ef9-195">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

<span data-ttu-id="48ef9-196">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48ef9-196">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="48ef9-197">Schemat jest oparta na modelu, określone w `RazorPagesMovieContext` (w *Data/RazorPagesMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="48ef9-197">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="48ef9-198">`Initial` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="48ef9-198">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="48ef9-199">Można dowolną nazwę, ale przez Konwencję na nazwę opisującą migracji jest używana.</span><span class="sxs-lookup"><span data-stu-id="48ef9-199">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="48ef9-200">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="48ef9-200">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="48ef9-201">`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="48ef9-201">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="48ef9-202">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="48ef9-202">Test the app</span></span>

* <span data-ttu-id="48ef9-203">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="48ef9-203">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="48ef9-204">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="48ef9-204">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="48ef9-205">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="48ef9-205">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="48ef9-206">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="48ef9-206">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)
  
  > [!NOTE]
  > <span data-ttu-id="48ef9-208">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="48ef9-208">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="48ef9-209">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="48ef9-209">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="48ef9-210">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="48ef9-210">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="48ef9-211">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="48ef9-211">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="48ef9-212">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="48ef9-212">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48ef9-213">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="48ef9-213">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
