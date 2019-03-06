---
title: Praca z bazy danych i ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono, pracy z bazą danych i ASP.NET Core.
ms.author: riande
ms.date: 12/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 9f01f5c9fcf9c423ccc2af8ae6ca55c2f427c9c1
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346031"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="8d9c3-103">Praca z bazy danych i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d9c3-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="8d9c3-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="8d9c3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="8d9c3-105">`RazorPagesMovieContext` Obiektu obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="8d9c3-106">Kontekst bazy danych jest zarejestrowany w [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` method in Class metoda *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8d9c3-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8d9c3-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d9c3-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8d9c3-108">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d9c3-108">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8d9c3-109">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8d9c3-109">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="8d9c3-110">Aby uzyskać więcej informacji na temat metod używanych w `ConfigureServices`, zobacz:</span><span class="sxs-lookup"><span data-stu-id="8d9c3-110">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="8d9c3-111">[Obsługa Unii Europejskiej ogólnego danych (GDPR Protection Regulation) w programie ASP.NET Core](xref:security/gdpr) dla `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-111">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="8d9c3-112">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="8d9c3-112">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="8d9c3-113">ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty system `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-113">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="8d9c3-114">Dla wdrożenia lokalnego, pobiera parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-114">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8d9c3-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d9c3-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8d9c3-116">Wartość nazwy bazy danych (`Database={Database name}`) będzie różna dla wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-116">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="8d9c3-117">Wartość nazwy jest dowolnego.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-117">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8d9c3-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d9c3-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8d9c3-119">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8d9c3-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="8d9c3-120">Po wdrożeniu aplikacji na serwerze testowym lub produkcyjnym zmiennej środowiskowej można ustawić parametrów połączenia z serwerem bazy danych rzeczywistych.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-120">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="8d9c3-121">Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-121">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8d9c3-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d9c3-122">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="8d9c3-123">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="8d9c3-123">SQL Server Express LocalDB</span></span>

<span data-ttu-id="8d9c3-124">LocalDB to Uproszczona wersja programu SQL Server Express aparatu bazy danych, która jest przeznaczona do tworzenia programu.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-124">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="8d9c3-125">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-125">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="8d9c3-126">Domyślnie tworzy bazy danych LocalDB `*.mdf` pliki `C:/Users/<user/>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-126">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="8d9c3-127">Z **widoku** menu Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="8d9c3-127">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](sql/_static/ssox.png)

* <span data-ttu-id="8d9c3-129">Kliknij prawym przyciskiem myszy `Movie` tabeli, a następnie wybierz pozycję **Projektant widoków**:</span><span class="sxs-lookup"><span data-stu-id="8d9c3-129">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmu](sql/_static/design.png)

  ![Otwórz w Projektancie tabel filmu](sql/_static/dv.png)

<span data-ttu-id="8d9c3-132">Należy zauważyć ikonę klucza, obok `ID`.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-132">Note the key icon next to `ID`.</span></span> <span data-ttu-id="8d9c3-133">Domyślnie program EF tworzy właściwość o nazwie `ID` dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-133">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="8d9c3-134">Kliknij prawym przyciskiem myszy `Movie` tabeli, a następnie wybierz pozycję **dane widoku**:</span><span class="sxs-lookup"><span data-stu-id="8d9c3-134">Right click on the `Movie` table and select **View Data**:</span></span>

  <span data-ttu-id="8d9c3-135">![Tabela filmu otwarte, wyświetlanie tabeli danych](sql/_static/vd22.png)
<!-- Code --------------------------></span><span class="sxs-lookup"><span data-stu-id="8d9c3-135">![Movie table open showing table data](sql/_static/vd22.png)
<!-- Code --------------------------></span></span>
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8d9c3-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d9c3-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8d9c3-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8d9c3-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="8d9c3-138">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="8d9c3-138">Seed the database</span></span>

<span data-ttu-id="8d9c3-139">Utwórz nową klasę o nazwie `SeedData` w *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8d9c3-139">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="8d9c3-140">Czy istnieją wszystkie filmy w bazie danych, zwraca inicjatora inicjatora i filmy nie zostaną dodane.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-140">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="8d9c3-141">Dodaj inicjator inicjatora</span><span class="sxs-lookup"><span data-stu-id="8d9c3-141">Add the seed initializer</span></span>

<span data-ttu-id="8d9c3-142">W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="8d9c3-142">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="8d9c3-143">Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-143">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="8d9c3-144">Wywołaj metodę inicjatora, przekazując mu kontekst.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-144">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="8d9c3-145">Po zakończeniu seed — metoda, należy dysponować kontekstu.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-145">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="8d9c3-146">Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-146">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="8d9c3-147">Nie może wywołać aplikacji produkcyjnej `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-147">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="8d9c3-148">Jest ona dodawana do poprzedniego kodu, aby zapobiec następujący wyjątek podczas `Update-Database` nie zostały uruchomione:</span><span class="sxs-lookup"><span data-stu-id="8d9c3-148">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="8d9c3-149">SqlException: Nie można otworzyć bazy danych "RazorPagesMovieContext-21" żądanego podczas logowania.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-149">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="8d9c3-150">Logowanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-150">The login failed.</span></span>
<span data-ttu-id="8d9c3-151">Nie można zalogować użytkownika "Nazwa użytkownika".</span><span class="sxs-lookup"><span data-stu-id="8d9c3-151">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="8d9c3-152">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8d9c3-152">Test the app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8d9c3-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d9c3-153">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8d9c3-154">Usuń wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-154">Delete all the records in the DB.</span></span> <span data-ttu-id="8d9c3-155">Można to zrobić za pomocą łącza delete w przeglądarce, albo z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="8d9c3-155">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="8d9c3-156">Wymusić na aplikacji, aby zainicjować (wywoływanie metody w `Startup` klasy), uruchamia seed — metoda.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-156">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="8d9c3-157">Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-157">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="8d9c3-158">To zrobić przy użyciu dowolnej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="8d9c3-158">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="8d9c3-159">Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie naciśnij pozycję **zakończenia** lub **zatrzymać witrynę**:</span><span class="sxs-lookup"><span data-stu-id="8d9c3-159">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Usługi IIS Express ikony na pasku zadań](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * <span data-ttu-id="8d9c3-162">Podczas uruchamiania w trybie bez debugowania programu VS, naciśnij klawisz F5, aby uruchomić w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-162">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="8d9c3-163">Podczas uruchamiania programu VS w trybie debugowania, Zatrzymaj debuger, a następnie naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-163">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8d9c3-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d9c3-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8d9c3-165">(Aby uruchomi seed — metoda), należy usunąć wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-165">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="8d9c3-166">Zatrzymywanie i uruchamianie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-166">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="8d9c3-167">Aplikacja zawiera wprowadzonych danych.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-167">The app shows the seeded data.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8d9c3-168">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8d9c3-168">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="8d9c3-169">(Aby uruchomi seed — metoda), należy usunąć wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-169">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="8d9c3-170">Zatrzymywanie i uruchamianie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-170">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="8d9c3-171">Aplikacja zawiera wprowadzonych danych.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-171">The app shows the seeded data.</span></span>

---  
<!-- End of VS tabs -->


   
<span data-ttu-id="8d9c3-172">Aplikacja przedstawiono wprowadzonych danych:</span><span class="sxs-lookup"><span data-stu-id="8d9c3-172">The app shows the seeded data:</span></span>

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji filmów](sql/_static/m55.png)

<span data-ttu-id="8d9c3-174">Następny samouczek spowoduje oczyszczenie prezentacji danych.</span><span class="sxs-lookup"><span data-stu-id="8d9c3-174">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8d9c3-175">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8d9c3-175">Additional resources</span></span>

* [<span data-ttu-id="8d9c3-176">Wersja usługi YouTube w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="8d9c3-176">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="8d9c3-177">[Poprzednie: Działanie stron Razor](xref:tutorials/razor-pages/page)
> [dalej: Aktualizowanie stron](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="8d9c3-177">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
