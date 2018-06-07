---
title: Dodawanie do aplikacji stron Razor w ASP.NET Core modelu
author: rick-anderson
description: Dowiedzieć się, jak można dodać klasy do zrządzania filmów w bazie danych przy użyciu Entity Framework Core (EF Core).
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 50b1b01448ad870a2889db7dfe8367ab9e661840
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729966"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="48822-103">Dodawanie do aplikacji stron Razor w ASP.NET Core modelu</span><span class="sxs-lookup"><span data-stu-id="48822-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="48822-104">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="48822-104">Add a data model</span></span>

<span data-ttu-id="48822-105">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="48822-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="48822-106">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="48822-106">Name the folder *Models*.</span></span>

<span data-ttu-id="48822-107">Kliknij prawym przyciskiem myszy *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="48822-107">Right click the *Models* folder.</span></span> <span data-ttu-id="48822-108">Wybierz **dodać** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="48822-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="48822-109">Nazwa klasy **film** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="48822-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="48822-110">Zastąp zawartość `Movie` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="48822-110">Replace the contents of the `Movie` class with the following code:</span></span>

<span data-ttu-id="48822-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="48822-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="48822-112">Tworzenie szkieletu modelu film</span><span class="sxs-lookup"><span data-stu-id="48822-112">Scaffold the movie model</span></span>

<span data-ttu-id="48822-113">W tej sekcji jest szkieletu modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="48822-113">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="48822-114">Oznacza to, że narzędzie szkieletów zawiera strony dla operacji tworzenia, odczytu, aktualizacji i usuwania (CRUD) dla modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="48822-114">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="48822-115">Utwórz *stron/filmów* folderu:</span><span class="sxs-lookup"><span data-stu-id="48822-115">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="48822-116">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron* folder > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="48822-116">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="48822-117">Nazwa folderu *filmy*</span><span class="sxs-lookup"><span data-stu-id="48822-117">Name the folder *Movies*</span></span>

<span data-ttu-id="48822-118">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron/filmów* folder > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="48822-118">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

<span data-ttu-id="48822-120">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **stron Razor przy użyciu programu Entity Framework (CRUD)** > **dodać**.</span><span class="sxs-lookup"><span data-stu-id="48822-120">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

<span data-ttu-id="48822-122">Zakończenie **Dodaj Razor strony przy użyciu programu Entity Framework (CRUD)** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="48822-122">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="48822-123">W **klasa modelu** listy rozwijanej, wybierz **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="48822-123">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="48822-124">W **klasa kontekstu danych** wierszu, wybierz opcję **+** (oraz) podpisywania i zaakceptuj nazwę wygenerowanego **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="48822-124">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="48822-125">W **klasa kontekstu danych** listy rozwijanej, wybierz **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="48822-125">In the **Data context class** drop down,  select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="48822-126">Wybierz **dodać**.</span><span class="sxs-lookup"><span data-stu-id="48822-126">Select **Add**.</span></span>

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="48822-128">Przeprowadź migrację początkowej</span><span class="sxs-lookup"><span data-stu-id="48822-128">Perform initial migration</span></span>

<span data-ttu-id="48822-129">W tej sekcji można użyć do konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="48822-129">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="48822-130">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="48822-130">Add an initial migration.</span></span>
* <span data-ttu-id="48822-131">Aktualizacji bazy danych z początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="48822-131">Update the database with the initial migration.</span></span>

<span data-ttu-id="48822-132">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="48822-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menu](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="48822-134">W kryterium wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="48822-134">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="48822-135">Alternatywnie można użyć następujących poleceń .NET Core interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="48822-135">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="48822-136">Ignoruj następujący komunikat ostrzegawczy, możesz rozwiązać ten problem, w następnym samouczku:</span><span class="sxs-lookup"><span data-stu-id="48822-136">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="48822-137">`Add-Migration` Polecenie generuje kod w celu utworzenia schematu początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48822-137">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="48822-138">Schemat jest oparta na modelu określone w `DbContext` (w *Models/MovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="48822-138">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="48822-139">`Initial` Argument jest używany do nazywania migracji.</span><span class="sxs-lookup"><span data-stu-id="48822-139">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="48822-140">Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="48822-140">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="48822-141">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="48822-141">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="48822-142">`Update-Database` Polecenia `Up` metody w *migracje / {sygnatury czasowej} _InitialCreate.cs* pliku, który utworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="48822-142">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="48822-143">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="48822-143">If you get the error:</span></span>

<span data-ttu-id="48822-144">SqlException: Nie można otworzyć bazy danych "RazorPagesMovieContext GUID" żądanego podczas logowania.</span><span class="sxs-lookup"><span data-stu-id="48822-144">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="48822-145">Logowanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="48822-145">The login failed.</span></span>
<span data-ttu-id="48822-146">Użytkownik "Nazwa użytkownika" Logowanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="48822-146">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="48822-147">Można pominąć [krok migracji](#pmc).</span><span class="sxs-lookup"><span data-stu-id="48822-147">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="48822-148">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="48822-148">Add a data model</span></span>

<span data-ttu-id="48822-149">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="48822-149">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="48822-150">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="48822-150">Name the folder *Models*.</span></span>

<span data-ttu-id="48822-151">Kliknij prawym przyciskiem myszy *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="48822-151">Right click the *Models* folder.</span></span> <span data-ttu-id="48822-152">Wybierz **dodać** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="48822-152">Select **Add** > **Class**.</span></span> <span data-ttu-id="48822-153">Nazwa klasy **film** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="48822-153">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="48822-154">Dodaj parametry połączenia bazy danych</span><span class="sxs-lookup"><span data-stu-id="48822-154">Add a database connection string</span></span>

<span data-ttu-id="48822-155">Dodaj parametry połączenia do *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="48822-155">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="48822-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="48822-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="48822-157">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="48822-157">Register the database context</span></span>

<span data-ttu-id="48822-158">Zarejestruj kontekst bazy danych z [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w [ConfigureServices metody klasy uruchamiania](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="48822-158">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

<span data-ttu-id="48822-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span><span class="sxs-lookup"><span data-stu-id="48822-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span></span>

<span data-ttu-id="48822-160">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="48822-160">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="48822-161">Dodawanie szkieletu narzędzi i przeprowadzić migrację początkowej</span><span class="sxs-lookup"><span data-stu-id="48822-161">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="48822-162">W tej sekcji można użyć do konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="48822-162">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="48822-163">Dodaj pakiet generowania kodu sieci web programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="48822-163">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="48822-164">Ten pakiet jest wymagana do uruchamiania aparatu szkieletów.</span><span class="sxs-lookup"><span data-stu-id="48822-164">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="48822-165">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="48822-165">Add an initial migration.</span></span>
* <span data-ttu-id="48822-166">Aktualizacji bazy danych z początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="48822-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="48822-167">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="48822-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menu](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="48822-169">W kryterium wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="48822-169">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="48822-170">Alternatywnie można użyć następujących poleceń .NET Core interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="48822-170">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="48822-171">Ignoruj następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="48822-171">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="48822-172">Napraw który w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="48822-172">You fix that in the next tutorial.</span></span>

<span data-ttu-id="48822-173">`Install-Package` Polecenie powoduje zainstalowanie narzędzi wymagane do uruchamiania aparatu szkieletów.</span><span class="sxs-lookup"><span data-stu-id="48822-173">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="48822-174">`Add-Migration` Polecenie generuje kod w celu utworzenia schematu początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48822-174">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="48822-175">Schemat jest oparta na modelu określone w `DbContext` (w *Models/MovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="48822-175">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="48822-176">`Initial` Argument jest używany do nazywania migracji.</span><span class="sxs-lookup"><span data-stu-id="48822-176">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="48822-177">Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="48822-177">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="48822-178">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="48822-178">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="48822-179">`Update-Database` Polecenia `Up` metody w *migracje / {sygnatury czasowej} _InitialCreate.cs* pliku, który utworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="48822-179">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="48822-180">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="48822-180">Test the app</span></span>

* <span data-ttu-id="48822-181">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="48822-181">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="48822-182">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="48822-182">Test the **Create** link.</span></span>

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="48822-184">Test **Edytuj**, **szczegóły**, i **usunąć** łącza.</span><span class="sxs-lookup"><span data-stu-id="48822-184">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="48822-185">Jeśli otrzymasz wyjątek SQL, sprawdź zostało uruchomione migracji i aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48822-185">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="48822-186">Następny samouczek wyjaśnia plików utworzonych przez funkcję szkieletów.</span><span class="sxs-lookup"><span data-stu-id="48822-186">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48822-187">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: szkieletu stron Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="48822-187">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
