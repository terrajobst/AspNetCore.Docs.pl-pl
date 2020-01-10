---
title: Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak można dodać klas związanych z zarządzaniem filmów w bazie danych przy użyciu platformy Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: fa5be8f3a222a7c186409faa2f48e43347df637a
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829299"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="48afe-103">Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48afe-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="48afe-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="48afe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="48afe-105">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="48afe-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="48afe-106">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="48afe-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="48afe-107">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="48afe-108">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="48afe-109">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="48afe-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="48afe-110">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="48afe-111">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="48afe-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48afe-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48afe-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48afe-113">Kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="48afe-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="48afe-114">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="48afe-114">Name the folder *Models*.</span></span>

<span data-ttu-id="48afe-115">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="48afe-115">Right click the *Models* folder.</span></span> <span data-ttu-id="48afe-116">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="48afe-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="48afe-117">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="48afe-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48afe-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48afe-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="48afe-119">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="48afe-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="48afe-120">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="48afe-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48afe-121">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="48afe-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="48afe-122">W okienko rozwiązania kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder.** .. Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="48afe-122">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="48afe-123">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik.** ...</span><span class="sxs-lookup"><span data-stu-id="48afe-123">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="48afe-124">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="48afe-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="48afe-125">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="48afe-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="48afe-126">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="48afe-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="48afe-127">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="48afe-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="48afe-128">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="48afe-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="48afe-129">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="48afe-129">Scaffold the movie model</span></span>

<span data-ttu-id="48afe-130">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="48afe-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="48afe-131">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="48afe-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48afe-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48afe-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48afe-133">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="48afe-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="48afe-134">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="48afe-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="48afe-135">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="48afe-135">Name the folder *Movies*</span></span>

<span data-ttu-id="48afe-136">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="48afe-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="48afe-138">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="48afe-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="48afe-140">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="48afe-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="48afe-141">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="48afe-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="48afe-142">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="48afe-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="48afe-143">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="48afe-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="48afe-144">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="48afe-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="48afe-145">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="48afe-145">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

<span data-ttu-id="48afe-147">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48afe-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48afe-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="48afe-149">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="48afe-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="48afe-150">Zainstaluj narzędzia do tworzenia szkieletów:</span><span class="sxs-lookup"><span data-stu-id="48afe-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="48afe-151">**Aby uzyskać Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="48afe-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="48afe-152">**Dla systemu macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="48afe-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48afe-153">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="48afe-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48afe-154">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="48afe-154">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="48afe-155">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="48afe-155">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="48afe-156">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="48afe-156">Name the folder *Movies*</span></span>

<span data-ttu-id="48afe-157">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowe tworzenie szkieletu..** ..</span><span class="sxs-lookup"><span data-stu-id="48afe-157">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/scaMac.png)

<span data-ttu-id="48afe-159">W oknie dialogowym **Nowa rusztowania** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="48afe-159">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="48afe-161">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="48afe-161">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="48afe-162">W menu rozwijanym **Klasa modelu** wybierz lub wpisz **film (RazorPagesMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="48afe-162">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="48afe-163">W wierszu **klasy kontekstu danych** wpisz nazwę nowej klasy, RazorPagesMovie. **Dane**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="48afe-163">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="48afe-164">[Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="48afe-164">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="48afe-165">Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="48afe-165">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="48afe-166">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="48afe-166">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arpMac.png)

<span data-ttu-id="48afe-168">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-168">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

### <a name="add-ef-tools"></a><span data-ttu-id="48afe-169">Dodawanie narzędzi EF</span><span class="sxs-lookup"><span data-stu-id="48afe-169">Add EF tools</span></span>

<span data-ttu-id="48afe-170">Uruchom następujące interfejs wiersza polecenia platformy .NET Core polecenie:</span><span class="sxs-lookup"><span data-stu-id="48afe-170">Run the following .NET Core CLI command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="48afe-171">Poprzednie polecenie dodaje Entity Framework Core narzędzia dla interfejs wiersza polecenia platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="48afe-171">The preceding command adds the Entity Framework Core Tools for the .NET Core CLI.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="48afe-172">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="48afe-172">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48afe-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48afe-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48afe-174">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="48afe-174">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="48afe-175">*Strony/filmów*: tworzenie, usuwanie, uzyskać szczegółowe informacje, edytowanie i indeksu.</span><span class="sxs-lookup"><span data-stu-id="48afe-175">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="48afe-176">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="48afe-176">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="48afe-177">Zaktualizowano</span><span class="sxs-lookup"><span data-stu-id="48afe-177">Updated</span></span>

* <span data-ttu-id="48afe-178">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="48afe-178">*Startup.cs*</span></span>

<span data-ttu-id="48afe-179">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="48afe-179">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48afe-180">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="48afe-180">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48afe-181">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="48afe-181">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="48afe-182">*Strony/filmów*: tworzenie, usuwanie, uzyskać szczegółowe informacje, edytowanie i indeksu.</span><span class="sxs-lookup"><span data-stu-id="48afe-182">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="48afe-183">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="48afe-183">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="48afe-184">Zaktualizowano</span><span class="sxs-lookup"><span data-stu-id="48afe-184">Updated</span></span>

* <span data-ttu-id="48afe-185">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="48afe-185">*Startup.cs*</span></span>

<span data-ttu-id="48afe-186">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="48afe-186">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48afe-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48afe-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="48afe-188">Proces tworzenia szkieletu tworzy następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="48afe-188">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="48afe-189">*Strony/filmów*: tworzenie, usuwanie, uzyskać szczegółowe informacje, edytowanie i indeksu.</span><span class="sxs-lookup"><span data-stu-id="48afe-189">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="48afe-190">Utworzone pliki zostały wyjaśnione w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="48afe-190">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="48afe-191">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="48afe-191">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48afe-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48afe-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48afe-193">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="48afe-193">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="48afe-194">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="48afe-194">Add an initial migration.</span></span>
* <span data-ttu-id="48afe-195">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="48afe-195">Update the database with the initial migration.</span></span>

<span data-ttu-id="48afe-196">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="48afe-196">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="48afe-198">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="48afe-198">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48afe-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48afe-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48afe-200">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="48afe-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="48afe-201">Powyższe polecenia generują następujące ostrzeżenie: "nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ".</span><span class="sxs-lookup"><span data-stu-id="48afe-201">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="48afe-202">Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali.</span><span class="sxs-lookup"><span data-stu-id="48afe-202">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="48afe-203">Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".</span><span class="sxs-lookup"><span data-stu-id="48afe-203">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="48afe-204">Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="48afe-204">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="48afe-205">Polecenie migrations generuje kod, aby utworzyć początkowy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-205">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="48afe-206">Schemat jest oparty na modelu określonym w `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="48afe-206">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="48afe-207">`InitialCreate` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="48afe-207">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="48afe-208">Można dowolną nazwę, ale zgodnie z Konwencją, wybrane nazwę opisującą migracji.</span><span class="sxs-lookup"><span data-stu-id="48afe-208">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="48afe-209">`update` polecenie uruchamia metodę `Up` w przypadku migracji, które nie zostały zastosowane.</span><span class="sxs-lookup"><span data-stu-id="48afe-209">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="48afe-210">W takim przypadku `update` uruchamia metodę `Up` w plikach *migrations/\<sygnatura czasowa > _InitialCreate. cs* , który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-210">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48afe-211">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48afe-211">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="48afe-212">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="48afe-212">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="48afe-213">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="48afe-213">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="48afe-214">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48afe-214">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="48afe-215">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="48afe-215">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="48afe-216">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="48afe-216">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="48afe-217">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="48afe-217">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="48afe-218">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="48afe-218">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="48afe-219">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="48afe-219">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="48afe-220">`RazorPagesMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="48afe-220">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="48afe-221">Kontekst danych (`RazorPagesMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="48afe-221">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="48afe-222">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-222">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="48afe-223">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="48afe-223">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="48afe-224">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-224">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="48afe-225">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="48afe-225">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="48afe-226">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="48afe-226">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="48afe-227">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="48afe-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48afe-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48afe-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="48afe-229">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="48afe-229">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48afe-230">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="48afe-230">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48afe-231">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="48afe-231">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="48afe-232">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="48afe-232">Test the app</span></span>

* <span data-ttu-id="48afe-233">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="48afe-233">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="48afe-234">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="48afe-234">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="48afe-235">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="48afe-235">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="48afe-236">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="48afe-236">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="48afe-238">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="48afe-238">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="48afe-239">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="48afe-239">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="48afe-240">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="48afe-240">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="48afe-241">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="48afe-241">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="48afe-242">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="48afe-242">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48afe-243">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="48afe-243">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48afe-244">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="48afe-244">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="48afe-245">W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="48afe-245">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="48afe-246">Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite.</span><span class="sxs-lookup"><span data-stu-id="48afe-246">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="48afe-247">Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-247">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="48afe-248">EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-248">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="48afe-249">Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="48afe-249">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="48afe-250">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-250">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="48afe-251">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="48afe-251">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48afe-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48afe-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48afe-253">Kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="48afe-253">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="48afe-254">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="48afe-254">Name the folder *Models*.</span></span>

<span data-ttu-id="48afe-255">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="48afe-255">Right click the *Models* folder.</span></span> <span data-ttu-id="48afe-256">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="48afe-256">Select **Add** > **Class**.</span></span> <span data-ttu-id="48afe-257">Nazwa klasy **filmu**.</span><span class="sxs-lookup"><span data-stu-id="48afe-257">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48afe-258">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48afe-258">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="48afe-259">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="48afe-259">Add a folder named *Models*.</span></span>
* <span data-ttu-id="48afe-260">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="48afe-260">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48afe-261">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="48afe-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="48afe-262">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="48afe-262">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="48afe-263">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="48afe-263">Name the folder *Models*.</span></span>
* <span data-ttu-id="48afe-264">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="48afe-264">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="48afe-265">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="48afe-265">In the **New File** dialog:</span></span>

  * <span data-ttu-id="48afe-266">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="48afe-266">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="48afe-267">Wybierz **pustą klasę** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="48afe-267">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="48afe-268">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="48afe-268">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="48afe-269">Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="48afe-269">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="48afe-270">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="48afe-270">Scaffold the movie model</span></span>

<span data-ttu-id="48afe-271">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="48afe-271">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="48afe-272">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="48afe-272">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48afe-273">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48afe-273">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48afe-274">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="48afe-274">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="48afe-275">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="48afe-275">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="48afe-276">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="48afe-276">Name the folder *Movies*</span></span>

<span data-ttu-id="48afe-277">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="48afe-277">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="48afe-279">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="48afe-279">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="48afe-281">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="48afe-281">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="48afe-282">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="48afe-282">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="48afe-283">W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i zaakceptuj wygenerowaną nazwę **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="48afe-283">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="48afe-284">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="48afe-284">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="48afe-286">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-286">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48afe-287">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48afe-287">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="48afe-288">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="48afe-288">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="48afe-289">**Aby uzyskać Windows**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="48afe-289">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="48afe-290">**Dla systemu macOS i Linux**: Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="48afe-290">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48afe-291">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="48afe-291">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48afe-292">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="48afe-292">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="48afe-293">Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="48afe-293">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="48afe-294">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="48afe-294">Name the folder *Movies*</span></span>

<span data-ttu-id="48afe-295">Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="48afe-295">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/scaMac.png)

<span data-ttu-id="48afe-297">W oknie dialogowym **Dodawanie nowej szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="48afe-297">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="48afe-299">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="48afe-299">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="48afe-300">Z listy rozwijanej **Klasa modelu** wybierz lub wpisz **film**.</span><span class="sxs-lookup"><span data-stu-id="48afe-300">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="48afe-301">W wierszu **klasy kontekstu danych** wpisz wybierz **RazorPagesMovieContext** to spowoduje utworzenie nowej klasy kontekstu bazy danych z poprawną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="48afe-301">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="48afe-302">W takim przypadku będzie to **RazorPagesMovie. models. RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="48afe-302">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="48afe-303">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="48afe-303">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arpMac.png)

<span data-ttu-id="48afe-305">*Appsettings.json* plik został zaktualizowany o parametry połączenia używane do łączenia z lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-305">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="48afe-306">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="48afe-306">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="48afe-307">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="48afe-307">Files created</span></span>

* <span data-ttu-id="48afe-308">*Strony/filmów*: tworzenie, usuwanie, uzyskać szczegółowe informacje, edytowanie i indeksu.</span><span class="sxs-lookup"><span data-stu-id="48afe-308">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="48afe-309">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="48afe-309">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="48afe-310">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="48afe-310">File updated</span></span>

* <span data-ttu-id="48afe-311">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="48afe-311">*Startup.cs*</span></span>

<span data-ttu-id="48afe-312">Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="48afe-312">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="48afe-313">Początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="48afe-313">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48afe-314">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48afe-314">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48afe-315">W tej sekcji konsoli Menedżera pakietów (PMC) służy do:</span><span class="sxs-lookup"><span data-stu-id="48afe-315">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="48afe-316">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="48afe-316">Add an initial migration.</span></span>
* <span data-ttu-id="48afe-317">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="48afe-317">Update the database with the initial migration.</span></span>

<span data-ttu-id="48afe-318">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="48afe-318">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="48afe-320">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="48afe-320">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="48afe-321">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-321">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="48afe-322">Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ).</span><span class="sxs-lookup"><span data-stu-id="48afe-322">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="48afe-323">Argument `InitialCreate` jest używany do nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="48afe-323">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="48afe-324">Można dowolną nazwę, ale przez Konwencję na nazwę opisującą migracji jest używana.</span><span class="sxs-lookup"><span data-stu-id="48afe-324">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="48afe-325">Aby uzyskać więcej informacji, zobacz temat <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="48afe-325">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="48afe-326">`Update-Database` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="48afe-326">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="48afe-327">`Up` Metoda tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-327">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48afe-328">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48afe-328">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48afe-329">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="48afe-329">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="48afe-330">Powyższe polecenia generują następujące ostrzeżenie: "*nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".* Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="48afe-330">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48afe-331">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48afe-331">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="48afe-332">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="48afe-332">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="48afe-333">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="48afe-333">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="48afe-334">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48afe-334">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="48afe-335">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="48afe-335">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="48afe-336">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="48afe-336">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="48afe-337">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="48afe-337">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="48afe-338">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="48afe-338">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="48afe-339">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="48afe-339">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="48afe-340">`RazorPagesMovieContext` Współrzędne funkcji EF Core (tworzenia, odczytu, aktualizacji, usuwania, itp.) `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="48afe-340">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="48afe-341">Kontekst danych (`RazorPagesMovieContext`) jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="48afe-341">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="48afe-342">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-342">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="48afe-343">Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="48afe-343">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="48afe-344">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48afe-344">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="48afe-345">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="48afe-345">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="48afe-346">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="48afe-346">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="48afe-347">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="48afe-347">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48afe-348">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48afe-348">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="48afe-349">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="48afe-349">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48afe-350">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="48afe-350">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48afe-351">Sprawdź `Up` metody.</span><span class="sxs-lookup"><span data-stu-id="48afe-351">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="48afe-352">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="48afe-352">Test the app</span></span>

* <span data-ttu-id="48afe-353">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="48afe-353">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="48afe-354">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="48afe-354">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="48afe-355">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="48afe-355">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="48afe-356">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="48afe-356">Test the **Create** link.</span></span>

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="48afe-358">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="48afe-358">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="48afe-359">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") separator dziesiętny i formaty daty inne niż angielski, aplikacja musi globalizowana.</span><span class="sxs-lookup"><span data-stu-id="48afe-359">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="48afe-360">Globalizacja instrukcje można znaleźć [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="48afe-360">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="48afe-361">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="48afe-361">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="48afe-362">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="48afe-362">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48afe-363">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="48afe-363">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48afe-364">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="48afe-364">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
