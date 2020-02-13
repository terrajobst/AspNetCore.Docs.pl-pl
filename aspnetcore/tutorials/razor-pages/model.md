---
title: Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak można dodać klas związanych z zarządzaniem filmów w bazie danych przy użyciu platformy Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: d2f9a64c77d76702004b94cdf36e558b33d7e19a
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172573"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="3c387-103">Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c387-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="3c387-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3c387-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="3c387-105">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="3c387-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="3c387-106">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="3c387-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="3c387-107">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="3c387-108">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="3c387-109">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="3c387-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="3c387-110">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="3c387-111">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="3c387-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c387-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c387-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c387-113">Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="3c387-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="3c387-114">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="3c387-114">Name the folder *Models*.</span></span>

<span data-ttu-id="3c387-115">Kliknij prawym przyciskiem myszy folder *modele* .</span><span class="sxs-lookup"><span data-stu-id="3c387-115">Right click the *Models* folder.</span></span> <span data-ttu-id="3c387-116">Wybierz pozycję dodaj **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="3c387-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="3c387-117">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="3c387-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c387-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c387-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3c387-119">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="3c387-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="3c387-120">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="3c387-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c387-121">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="3c387-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3c387-122">W okienko rozwiązania kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder.** .. Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="3c387-122">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="3c387-123">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik.** ...</span><span class="sxs-lookup"><span data-stu-id="3c387-123">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="3c387-124">W oknie dialogowym **nowy plik** :</span><span class="sxs-lookup"><span data-stu-id="3c387-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="3c387-125">W lewym okienku wybierz pozycję **Ogólne** .</span><span class="sxs-lookup"><span data-stu-id="3c387-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="3c387-126">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="3c387-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="3c387-127">Nazwij **film** klasy i wybierz pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="3c387-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="3c387-128">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="3c387-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="3c387-129">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="3c387-129">Scaffold the movie model</span></span>

<span data-ttu-id="3c387-130">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="3c387-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="3c387-131">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="3c387-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c387-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c387-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c387-133">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="3c387-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="3c387-134">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="3c387-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="3c387-135">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="3c387-135">Name the folder *Movies*</span></span>

<span data-ttu-id="3c387-136">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="3c387-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="3c387-138">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3c387-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="3c387-140">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="3c387-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="3c387-141">Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="3c387-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="3c387-142">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="3c387-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="3c387-143">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="3c387-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="3c387-144">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="3c387-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="3c387-145">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3c387-145">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

<span data-ttu-id="3c387-147">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c387-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c387-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="3c387-149">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="3c387-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="3c387-150">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="3c387-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="3c387-151">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3c387-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="3c387-152">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3c387-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c387-153">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="3c387-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c387-154">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="3c387-154">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="3c387-155">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="3c387-155">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="3c387-156">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="3c387-156">Name the folder *Movies*</span></span>

<span data-ttu-id="3c387-157">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowe tworzenie szkieletu..** ..</span><span class="sxs-lookup"><span data-stu-id="3c387-157">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/scaMac.png)

<span data-ttu-id="3c387-159">W oknie dialogowym **Nowa rusztowania** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="3c387-159">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="3c387-161">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="3c387-161">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="3c387-162">W menu rozwijanym **Klasa modelu** wybierz lub wpisz **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="3c387-162">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="3c387-163">W wierszu **klasy kontekstu danych** wpisz nazwę nowej klasy, RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="3c387-163">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="3c387-164">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="3c387-164">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="3c387-165">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="3c387-165">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="3c387-166">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3c387-166">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arpMac.png)

<span data-ttu-id="3c387-168">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-168">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

### <a name="add-ef-tools"></a><span data-ttu-id="3c387-169">Dodawanie narzędzi EF</span><span class="sxs-lookup"><span data-stu-id="3c387-169">Add EF tools</span></span>

<span data-ttu-id="3c387-170">Uruchom następujące interfejs wiersza polecenia platformy .NET Core polecenie:</span><span class="sxs-lookup"><span data-stu-id="3c387-170">Run the following .NET Core CLI command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="3c387-171">Poprzednie polecenie dodaje Entity Framework Core narzędzia dla interfejs wiersza polecenia platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c387-171">The preceding command adds the Entity Framework Core Tools for the .NET Core CLI.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="3c387-172">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="3c387-172">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c387-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c387-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c387-174">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="3c387-174">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="3c387-175">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="3c387-175">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="3c387-176">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="3c387-176">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="3c387-177">Aktualny</span><span class="sxs-lookup"><span data-stu-id="3c387-177">Updated</span></span>

* <span data-ttu-id="3c387-178">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="3c387-178">*Startup.cs*</span></span>

<span data-ttu-id="3c387-179">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3c387-179">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c387-180">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="3c387-180">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c387-181">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="3c387-181">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="3c387-182">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="3c387-182">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="3c387-183">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="3c387-183">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="3c387-184">Aktualny</span><span class="sxs-lookup"><span data-stu-id="3c387-184">Updated</span></span>

* <span data-ttu-id="3c387-185">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="3c387-185">*Startup.cs*</span></span>

<span data-ttu-id="3c387-186">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3c387-186">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c387-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c387-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3c387-188">Proces tworzenia szkieletu tworzy następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="3c387-188">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="3c387-189">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="3c387-189">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="3c387-190">Utworzone pliki zostały wyjaśnione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3c387-190">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="3c387-191">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="3c387-191">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c387-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c387-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c387-193">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="3c387-193">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="3c387-194">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="3c387-194">Add an initial migration.</span></span>
* <span data-ttu-id="3c387-195">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="3c387-195">Update the database with the initial migration.</span></span>

<span data-ttu-id="3c387-196">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="3c387-196">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="3c387-198">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c387-198">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c387-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c387-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c387-200">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="3c387-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="3c387-201">Powyższe polecenia generują następujące ostrzeżenie: "nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="3c387-201">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="3c387-202">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="3c387-202">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="3c387-203">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="3c387-203">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="3c387-204">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="3c387-204">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="3c387-205">Polecenie migrations generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-205">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="3c387-206">Schemat jest oparty na modelu określonym w `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3c387-206">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="3c387-207">Argument `InitialCreate` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="3c387-207">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="3c387-208">Można dowolną nazwę, ale zgodnie z Konwencją, wybrane nazwę opisującą migracji.</span><span class="sxs-lookup"><span data-stu-id="3c387-208">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="3c387-209">`update` polecenie uruchamia metodę `Up` w przypadku migracji, które nie zostały zastosowane.</span><span class="sxs-lookup"><span data-stu-id="3c387-209">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="3c387-210">W takim przypadku `update` uruchamia metodę `Up` w plikach *migrations/\<sygnatura czasowa > _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-210">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c387-211">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c387-211">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="3c387-212">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="3c387-212">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="3c387-213">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3c387-213">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3c387-214">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3c387-214">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="3c387-215">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="3c387-215">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="3c387-216">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="3c387-216">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="3c387-217">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="3c387-217">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="3c387-218">Przeanalizuj metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3c387-218">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="3c387-219">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="3c387-219">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="3c387-220">`RazorPagesMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="3c387-220">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="3c387-221">Kontekst danych (`RazorPagesMovieContext`) pochodzi od elementu [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="3c387-221">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="3c387-222">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-222">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="3c387-223">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="3c387-223">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="3c387-224">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-224">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="3c387-225">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="3c387-225">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="3c387-226">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="3c387-226">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="3c387-227">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="3c387-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c387-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c387-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3c387-229">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="3c387-229">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c387-230">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="3c387-230">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c387-231">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="3c387-231">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="3c387-232">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3c387-232">Test the app</span></span>

* <span data-ttu-id="3c387-233">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="3c387-233">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="3c387-234">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="3c387-234">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="3c387-235">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="3c387-235">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="3c387-236">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="3c387-236">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="3c387-238">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="3c387-238">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="3c387-239">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="3c387-239">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="3c387-240">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="3c387-240">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="3c387-241">Przetestuj linki **Edytuj**, **Szczegóły** i **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="3c387-241">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="3c387-242">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="3c387-242">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c387-243">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3c387-243">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3c387-244">[Poprzedni:](xref:tutorials/razor-pages/razor-pages-start) wprowadzenie
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="3c387-244">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3c387-245">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="3c387-245">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="3c387-246">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="3c387-246">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="3c387-247">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-247">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="3c387-248">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-248">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="3c387-249">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="3c387-249">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="3c387-250">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-250">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="3c387-251">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="3c387-251">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c387-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c387-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c387-253">Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="3c387-253">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="3c387-254">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="3c387-254">Name the folder *Models*.</span></span>

<span data-ttu-id="3c387-255">Kliknij prawym przyciskiem myszy folder *modele* .</span><span class="sxs-lookup"><span data-stu-id="3c387-255">Right click the *Models* folder.</span></span> <span data-ttu-id="3c387-256">Wybierz pozycję dodaj **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="3c387-256">Select **Add** > **Class**.</span></span> <span data-ttu-id="3c387-257">Nazwij **film**klasy.</span><span class="sxs-lookup"><span data-stu-id="3c387-257">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c387-258">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c387-258">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3c387-259">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="3c387-259">Add a folder named *Models*.</span></span>
* <span data-ttu-id="3c387-260">Dodaj klasę do folderu *models* o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="3c387-260">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c387-261">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="3c387-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3c387-262">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="3c387-262">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="3c387-263">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="3c387-263">Name the folder *Models*.</span></span>
* <span data-ttu-id="3c387-264">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="3c387-264">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="3c387-265">W oknie dialogowym **nowy plik** :</span><span class="sxs-lookup"><span data-stu-id="3c387-265">In the **New File** dialog:</span></span>

  * <span data-ttu-id="3c387-266">W lewym okienku wybierz pozycję **Ogólne** .</span><span class="sxs-lookup"><span data-stu-id="3c387-266">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="3c387-267">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="3c387-267">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="3c387-268">Nazwij **film** klasy i wybierz pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="3c387-268">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="3c387-269">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="3c387-269">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="3c387-270">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="3c387-270">Scaffold the movie model</span></span>

<span data-ttu-id="3c387-271">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="3c387-271">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="3c387-272">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="3c387-272">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c387-273">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c387-273">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c387-274">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="3c387-274">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="3c387-275">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="3c387-275">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="3c387-276">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="3c387-276">Name the folder *Movies*</span></span>

<span data-ttu-id="3c387-277">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="3c387-277">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="3c387-279">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3c387-279">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="3c387-281">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="3c387-281">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="3c387-282">Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="3c387-282">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="3c387-283">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i zaakceptuj wygenerowaną nazwę **RazorPagesMovie. models. RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="3c387-283">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="3c387-284">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3c387-284">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="3c387-286">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-286">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c387-287">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c387-287">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="3c387-288">Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="3c387-288">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="3c387-289">**Dla systemu Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3c387-289">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="3c387-290">**W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3c387-290">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c387-291">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="3c387-291">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c387-292">Utwórz folder *stron/filmów* :</span><span class="sxs-lookup"><span data-stu-id="3c387-292">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="3c387-293">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="3c387-293">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="3c387-294">Nazwij foldery *filmów*</span><span class="sxs-lookup"><span data-stu-id="3c387-294">Name the folder *Movies*</span></span>

<span data-ttu-id="3c387-295">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="3c387-295">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/scaMac.png)

<span data-ttu-id="3c387-297">W oknie dialogowym **Dodawanie nowej szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3c387-297">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="3c387-299">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="3c387-299">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="3c387-300">Z listy rozwijanej **Klasa modelu** wybierz lub wpisz **film**.</span><span class="sxs-lookup"><span data-stu-id="3c387-300">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="3c387-301">W wierszu **klasy kontekstu danych** wpisz wybierz **RazorPagesMovieContext** to spowoduje utworzenie nowej klasy kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="3c387-301">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="3c387-302">W takim przypadku będzie to **RazorPagesMovie. models. RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="3c387-302">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="3c387-303">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3c387-303">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arpMac.png)

<span data-ttu-id="3c387-305">Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-305">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="3c387-306">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="3c387-306">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="3c387-307">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="3c387-307">Files created</span></span>

* <span data-ttu-id="3c387-308">*Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="3c387-308">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="3c387-309">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="3c387-309">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="3c387-310">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="3c387-310">File updated</span></span>

* <span data-ttu-id="3c387-311">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="3c387-311">*Startup.cs*</span></span>

<span data-ttu-id="3c387-312">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3c387-312">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="3c387-313">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="3c387-313">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c387-314">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c387-314">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c387-315">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="3c387-315">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="3c387-316">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="3c387-316">Add an initial migration.</span></span>
* <span data-ttu-id="3c387-317">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="3c387-317">Update the database with the initial migration.</span></span>

<span data-ttu-id="3c387-318">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="3c387-318">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="3c387-320">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c387-320">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="3c387-321">Polecenie `Add-Migration` generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-321">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="3c387-322">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="3c387-322">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="3c387-323">Argument `InitialCreate` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="3c387-323">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="3c387-324">Można dowolną nazwę, ale przez Konwencję na nazwę opisującą migracji jest używana.</span><span class="sxs-lookup"><span data-stu-id="3c387-324">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="3c387-325">Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="3c387-325">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="3c387-326">`Update-Database` polecenie uruchamia metodę `Up` w *sygnaturze czasowej migracji/\<> _InitialCreate. cs* .</span><span class="sxs-lookup"><span data-stu-id="3c387-326">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="3c387-327">Metoda `Up` tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-327">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c387-328">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c387-328">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c387-329">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="3c387-329">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="3c387-330">Powyższe polecenia generują następujące ostrzeżenie: "*nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".* Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="3c387-330">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c387-331">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c387-331">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="3c387-332">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="3c387-332">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="3c387-333">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3c387-333">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3c387-334">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3c387-334">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="3c387-335">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="3c387-335">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="3c387-336">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="3c387-336">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="3c387-337">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="3c387-337">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="3c387-338">Przeanalizuj metodę `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3c387-338">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="3c387-339">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="3c387-339">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="3c387-340">`RazorPagesMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="3c387-340">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="3c387-341">Kontekst danych (`RazorPagesMovieContext`) pochodzi od elementu [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="3c387-341">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="3c387-342">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-342">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="3c387-343">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="3c387-343">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="3c387-344">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3c387-344">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="3c387-345">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="3c387-345">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="3c387-346">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="3c387-346">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="3c387-347">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="3c387-347">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c387-348">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c387-348">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3c387-349">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="3c387-349">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c387-350">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="3c387-350">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c387-351">Przeanalizuj metodę `Up`.</span><span class="sxs-lookup"><span data-stu-id="3c387-351">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="3c387-352">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3c387-352">Test the app</span></span>

* <span data-ttu-id="3c387-353">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="3c387-353">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="3c387-354">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="3c387-354">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="3c387-355">Pominięto [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="3c387-355">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="3c387-356">Przetestuj link **tworzenia** .</span><span class="sxs-lookup"><span data-stu-id="3c387-356">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="3c387-358">W polu `Price` nie można wprowadzać przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="3c387-358">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="3c387-359">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna.</span><span class="sxs-lookup"><span data-stu-id="3c387-359">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="3c387-360">Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="3c387-360">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="3c387-361">Przetestuj linki **Edytuj**, **Szczegóły** i **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="3c387-361">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="3c387-362">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="3c387-362">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c387-363">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3c387-363">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3c387-364">[Poprzedni:](xref:tutorials/razor-pages/razor-pages-start) wprowadzenie
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="3c387-364">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
