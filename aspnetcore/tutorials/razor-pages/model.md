---
title: Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak można dodać klas związanych z zarządzaniem filmów w bazie danych przy użyciu platformy Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: c4b23f75da298e4ee804f649219c2ce466b6d6ea
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299446"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="651ad-103">Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="651ad-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="651ad-104">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="651ad-104">Add a data model</span></span>

<span data-ttu-id="651ad-105">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="651ad-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="651ad-106">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="651ad-106">Name the folder *Models*.</span></span>

<span data-ttu-id="651ad-107">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="651ad-107">Right click the *Models* folder.</span></span> <span data-ttu-id="651ad-108">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="651ad-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="651ad-109">Nazwa klasy **filmu** i Zastąp zawartość `Movie` klasy z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="651ad-109">Name the class **Movie** and replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="651ad-110">Tworzenie szkieletu modelu movie</span><span class="sxs-lookup"><span data-stu-id="651ad-110">Scaffold the movie model</span></span>

<span data-ttu-id="651ad-111">W tej sekcji modelu movie jest szkielet.</span><span class="sxs-lookup"><span data-stu-id="651ad-111">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="651ad-112">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.</span><span class="sxs-lookup"><span data-stu-id="651ad-112">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="651ad-113">Tworzenie *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="651ad-113">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="651ad-114">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron* folder > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="651ad-114">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="651ad-115">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="651ad-115">Name the folder *Movies*</span></span>

<span data-ttu-id="651ad-116">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron/filmów* folder > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="651ad-116">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="651ad-118">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **strony Razor za pomocą programu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="651ad-118">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="651ad-120">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="651ad-120">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="651ad-121">W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="651ad-121">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="651ad-122">W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i zaakceptuj wygenerowaną nazwę **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="651ad-122">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="651ad-123">Wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="651ad-123">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<span data-ttu-id="651ad-125">Proces szkieletu tworzy i aktualizuje następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="651ad-125">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="651ad-126">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="651ad-126">Files created</span></span>

* <span data-ttu-id="651ad-127">*Strony/filmów*: tworzenie, edytowanie usuwania, uzyskać szczegółowe informacje, indeksu.</span><span class="sxs-lookup"><span data-stu-id="651ad-127">*Pages/Movies*: Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="651ad-128">Te strony są szczegółowo opisane w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="651ad-128">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="651ad-129">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="651ad-129">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="651ad-130">Zaktualizowano plik</span><span class="sxs-lookup"><span data-stu-id="651ad-130">File updated</span></span>

* <span data-ttu-id="651ad-131">*Startup.cs*: zmiany w tym pliku opisano szczegółowo w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="651ad-131">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="651ad-132">*appSettings.JSON*: parametry połączenia używane do łączenia z lokalnej bazy danych zostanie dodany.</span><span class="sxs-lookup"><span data-stu-id="651ad-132">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="651ad-133">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="651ad-133">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="651ad-134">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="651ad-134">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="651ad-135">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="651ad-135">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="651ad-136">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="651ad-136">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="651ad-137">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="651ad-137">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="651ad-138">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="651ad-138">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="651ad-139">Sprawdź `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="651ad-139">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="651ad-140">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="651ad-140">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="651ad-141">Główna klasa, która służy do koordynowania funkcji EF Core dla modelu danych jest z klasy kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="651ad-141">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="651ad-142">Kontekst danych jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="651ad-142">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="651ad-143">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="651ad-143">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="651ad-144">W tym projekcie nosi nazwę klasy `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="651ad-144">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="651ad-145">Powyższy kod tworzy [DbSet\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="651ad-145">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="651ad-146">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="651ad-146">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="651ad-147">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="651ad-147">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="651ad-148">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="651ad-148">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="651ad-149">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="651ad-149">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="651ad-150">Przeprowadzenie początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="651ad-150">Perform initial migration</span></span>

<span data-ttu-id="651ad-151">W tej sekcji użyjesz konsoli Menedżera pakietów (PMC) do:</span><span class="sxs-lookup"><span data-stu-id="651ad-151">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="651ad-152">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="651ad-152">Add an initial migration.</span></span>
* <span data-ttu-id="651ad-153">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="651ad-153">Update the database with the initial migration.</span></span>

<span data-ttu-id="651ad-154">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="651ad-154">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="651ad-156">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="651ad-156">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="651ad-157">Alternatywnie można następujące polecenia interfejsu wiersza polecenia platformy .NET Core z folderu projektu:</span><span class="sxs-lookup"><span data-stu-id="651ad-157">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="651ad-158">Ignoruj następujący komunikat ostrzegawczy możesz rozwiązać później w samouczku:</span><span class="sxs-lookup"><span data-stu-id="651ad-158">Ignore the following warning message, which you fix in a later tutorial:</span></span>

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.
```

<span data-ttu-id="651ad-159">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="651ad-159">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="651ad-160">Schemat jest oparta na modelu, określone w `RazorPagesMovieContext` (w *Data/RazorPagesMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="651ad-160">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="651ad-161">`Initial` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="651ad-161">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="651ad-162">Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="651ad-162">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="651ad-163">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="651ad-163">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="651ad-164">`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="651ad-164">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="651ad-165">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="651ad-165">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="651ad-166">Możesz pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="651ad-166">You missed the [migrations step](#pmc).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="651ad-167">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="651ad-167">Add a data model</span></span>

<span data-ttu-id="651ad-168">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="651ad-168">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="651ad-169">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="651ad-169">Name the folder *Models*.</span></span>

<span data-ttu-id="651ad-170">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="651ad-170">Right click the *Models* folder.</span></span> <span data-ttu-id="651ad-171">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="651ad-171">Select **Add** > **Class**.</span></span> <span data-ttu-id="651ad-172">Nazwa klasy **filmu** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="651ad-172">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="651ad-173">Dodaj parametry połączenia bazy danych</span><span class="sxs-lookup"><span data-stu-id="651ad-173">Add a database connection string</span></span>

<span data-ttu-id="651ad-174">Dodaj parametry połączenia, aby *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="651ad-174">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="651ad-175">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="651ad-175">Register the database context</span></span>

<span data-ttu-id="651ad-176">Zarejestruj kontekst bazy danych za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w [ConfigureServices metody klasy uruchamiania](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="651ad-176">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="651ad-177">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="651ad-177">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="651ad-178">Dodawanie szkieletu narzędzi i wykonywania początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="651ad-178">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="651ad-179">W tej sekcji użyjesz konsoli Menedżera pakietów (PMC) do:</span><span class="sxs-lookup"><span data-stu-id="651ad-179">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="651ad-180">Dodaj pakiet generowania kodu sieci web programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="651ad-180">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="651ad-181">Ten pakiet jest wymagana do uruchamiania aparatu tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="651ad-181">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="651ad-182">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="651ad-182">Add an initial migration.</span></span>
* <span data-ttu-id="651ad-183">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="651ad-183">Update the database with the initial migration.</span></span>

<span data-ttu-id="651ad-184">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="651ad-184">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="651ad-186">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="651ad-186">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="651ad-187">Alternatywnie można skorzystać z następujących poleceń interfejsu wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="651ad-187">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="651ad-188">Ignoruj następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="651ad-188">Ignore the following message:</span></span>

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'
```

<span data-ttu-id="651ad-189">Które naprawić w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="651ad-189">You fix that in the next tutorial.</span></span>

<span data-ttu-id="651ad-190">`Install-Package` Polecenie instaluje narzędzia wymagane do uruchamiania aparatu tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="651ad-190">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="651ad-191">`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="651ad-191">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="651ad-192">Schemat jest oparta na modelu, określone w `DbContext` (w *Models/MovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="651ad-192">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="651ad-193">`Initial` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="651ad-193">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="651ad-194">Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="651ad-194">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="651ad-195">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="651ad-195">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="651ad-196">`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="651ad-196">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="651ad-197">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="651ad-197">Test the app</span></span>

* <span data-ttu-id="651ad-198">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="651ad-198">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="651ad-199">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="651ad-199">Test the **Create** link.</span></span>

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="651ad-201">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="651ad-201">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="651ad-202">Jeśli wystąpi wyjątek SQL, należy sprawdzić, masz Uruchom migracje i aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="651ad-202">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="651ad-203">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="651ad-203">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="651ad-204">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="651ad-204">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
