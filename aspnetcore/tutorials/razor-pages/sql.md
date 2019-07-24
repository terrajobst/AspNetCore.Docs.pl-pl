---
title: Pracuj z bazą danych i ASP.NET Core
author: rick-anderson
description: Wyjaśnia, jak pracować z bazą danych i ASP.NET Core.
ms.author: riande
ms.date: 7/22/2019
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 197697f28e9faa45c1ac2b7f993bde15994957e5
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440394"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="9b9f3-103">Pracuj z bazą danych i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b9f3-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="9b9f3-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="9b9f3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="9b9f3-105">Obiekt obsługuje zadanie łączenia się z bazą danych i mapowania `Movie` obiektów do rekordów bazy danych. `RazorPagesMovieContext`</span><span class="sxs-lookup"><span data-stu-id="9b9f3-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="9b9f3-106">Kontekst bazy danych jest zarejestrowany z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w `ConfigureServices` metodzie w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9b9f3-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b9f3-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9b9f3-108">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="9b9f3-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="9b9f3-109">System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="9b9f3-110">W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="9b9f3-110">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9b9f3-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b9f3-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9b9f3-112">Wartość nazwy dla bazy danych (`Database={Database name}`) będzie różna dla wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="9b9f3-113">Wartość nazwy jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie30/appsettings.json)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9b9f3-114">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="9b9f3-114">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="9b9f3-115">Gdy aplikacja jest wdrażana na serwerze testowym lub produkcyjnym, zmienna środowiskowa może być używana do ustawiania parametrów połączenia na rzeczywistym serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-115">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="9b9f3-116">Aby uzyskać więcej informacji, zobacz [Konfiguracja](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="9b9f3-116">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9b9f3-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b9f3-117">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="9b9f3-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="9b9f3-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="9b9f3-119">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express, która jest przeznaczona do tworzenia programów.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-119">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="9b9f3-120">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="9b9f3-121">Domyślnie baza danych LocalDB tworzy `*.mdf` pliki `C:/Users/<user/>` w katalogu.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-121">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="9b9f3-122">Z menu **Widok** Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="9b9f3-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](sql/_static/ssox.png)

* <span data-ttu-id="9b9f3-124">Kliknij prawym przyciskiem `Movie` myszy tabelę i wybierz polecenie **Projektant widoków**:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu kontekstowe są otwierane w tabeli filmów](sql/_static/design.png)

  ![Tabele filmów otwierane w projektancie](sql/_static/dv.png)

<span data-ttu-id="9b9f3-127">Zanotuj ikonę klucza obok pozycji `ID`.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="9b9f3-128">Domyślnie EF tworzy właściwość o nazwie `ID` dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="9b9f3-129">Kliknij prawym przyciskiem `Movie` myszy tabelę i wybierz polecenie **Wyświetl dane**:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Otwórz tabelę filmów pokazującą dane tabeli](sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9b9f3-131">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="9b9f3-131">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="9b9f3-132">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="9b9f3-132">Seed the database</span></span>

<span data-ttu-id="9b9f3-133">Utwórz nową klasę o nazwie `SeedData` w folderze *models* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-133">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="9b9f3-134">Jeśli w bazie danych znajdują się jakiekolwiek filmy, inicjatora inicjatora zwraca i nie dodano żadnych filmów.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="9b9f3-135">Dodawanie inicjatora inicjatora</span><span class="sxs-lookup"><span data-stu-id="9b9f3-135">Add the seed initializer</span></span>

<span data-ttu-id="9b9f3-136">W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-136">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="9b9f3-137">Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-137">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="9b9f3-138">Wywołaj metodę inicjatora, przekazując ją do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-138">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="9b9f3-139">Usuń kontekst, gdy metoda inicjatora zostanie zakończona.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-139">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="9b9f3-140">Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-140">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Program.cs)]

<span data-ttu-id="9b9f3-141">Aplikacja produkcyjna nie będzie wywoływana `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-141">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="9b9f3-142">Jest on dodawany do poprzedniego kodu, aby zapobiec następującemu wyjątku, `Update-Database` gdy nie został uruchomiony:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-142">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="9b9f3-143">SqlException: Nie można otworzyć bazy danych "RazorPagesMovieContext-21" żądanej przez nazwę logowania.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-143">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="9b9f3-144">Logowanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-144">The login failed.</span></span>
<span data-ttu-id="9b9f3-145">Logowanie użytkownika "Nazwa użytkownika" nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-145">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="9b9f3-146">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9b9f3-146">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9b9f3-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b9f3-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9b9f3-148">Usuń wszystkie rekordy z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-148">Delete all the records in the DB.</span></span> <span data-ttu-id="9b9f3-149">Można to zrobić za pomocą linków usuwania w przeglądarce lub z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="9b9f3-149">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="9b9f3-150">Wymuś inicjalizację aplikacji (wywołaj metody z `Startup` klasy), aby była uruchamiana Metoda inicjatora.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-150">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="9b9f3-151">Aby wymusić inicjalizację, IIS Express należy zatrzymać i uruchomić ponownie.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-151">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="9b9f3-152">Można to zrobić przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-152">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="9b9f3-153">Kliknij prawym przyciskiem myszy ikonę IIS Express pasku zadań w obszarze powiadomień i naciśnij pozycję **Zakończ** lub **Zatrzymaj witrynę**:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-153">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Ikona paska zadań IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * <span data-ttu-id="9b9f3-156">W przypadku uruchamiania programu VS w trybie innym niż debugowanie naciśnij klawisz F5, aby uruchomić polecenie w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-156">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="9b9f3-157">W przypadku uruchamiania programu VS w trybie debugowania Zatrzymaj debuger i naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-157">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9b9f3-158">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="9b9f3-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="9b9f3-159">Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora).</span><span class="sxs-lookup"><span data-stu-id="9b9f3-159">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="9b9f3-160">Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-160">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="9b9f3-161">Aplikacja pokazuje dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-161">The app shows the seeded data.</span></span>

---

<span data-ttu-id="9b9f3-162">Następny samouczek poprawi prezentację danych.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-162">The next tutorial will improve the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b9f3-163">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9b9f3-163">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9b9f3-164">[Ubiegł Razor Pages](xref:tutorials/razor-pages/page)zszkieletem[:
>  Aktualizowanie stron](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="9b9f3-164">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="9b9f3-165">Obiekt obsługuje zadanie łączenia się z bazą danych i mapowania `Movie` obiektów do rekordów bazy danych. `RazorPagesMovieContext`</span><span class="sxs-lookup"><span data-stu-id="9b9f3-165">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="9b9f3-166">Kontekst bazy danych jest zarejestrowany z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w `ConfigureServices` metodzie w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-166">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9b9f3-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b9f3-167">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9b9f3-168">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="9b9f3-168">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="9b9f3-169">Aby uzyskać więcej informacji na temat metod używanych `ConfigureServices`w programie, zobacz:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-169">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="9b9f3-170">[Obsługa ogólne rozporządzenie o ochronie danych UE (Rodo) w ASP.NET Core](xref:security/gdpr) dla `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-170">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="9b9f3-171">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="9b9f3-171">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="9b9f3-172">System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-172">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="9b9f3-173">W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="9b9f3-173">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9b9f3-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b9f3-174">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9b9f3-175">Wartość nazwy dla bazy danych (`Database={Database name}`) będzie różna dla wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-175">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="9b9f3-176">Wartość nazwy jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-176">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9b9f3-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9b9f3-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9b9f3-178">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9b9f3-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="9b9f3-179">Gdy aplikacja jest wdrażana na serwerze testowym lub produkcyjnym, zmienna środowiskowa może być używana do ustawiania parametrów połączenia na rzeczywistym serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-179">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="9b9f3-180">Aby uzyskać więcej informacji, zobacz [Konfiguracja](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="9b9f3-180">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9b9f3-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b9f3-181">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="9b9f3-182">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="9b9f3-182">SQL Server Express LocalDB</span></span>

<span data-ttu-id="9b9f3-183">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express, która jest przeznaczona do tworzenia programów.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-183">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="9b9f3-184">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-184">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="9b9f3-185">Domyślnie baza danych LocalDB tworzy `*.mdf` pliki `C:/Users/<user/>` w katalogu.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-185">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="9b9f3-186">Z menu **Widok** Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="9b9f3-186">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](sql/_static/ssox.png)

* <span data-ttu-id="9b9f3-188">Kliknij prawym przyciskiem `Movie` myszy tabelę i wybierz polecenie **Projektant widoków**:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-188">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmów](sql/_static/design.png)

  ![Tabela filmów otwarta w projektancie](sql/_static/dv.png)

<span data-ttu-id="9b9f3-191">Zanotuj ikonę klucza obok pozycji `ID`.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-191">Note the key icon next to `ID`.</span></span> <span data-ttu-id="9b9f3-192">Domyślnie EF tworzy właściwość o nazwie `ID` dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-192">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="9b9f3-193">Kliknij prawym przyciskiem `Movie` myszy tabelę i wybierz polecenie **Wyświetl dane**:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-193">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Otwórz tabelę filmów pokazującą dane tabeli](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9b9f3-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9b9f3-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9b9f3-196">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9b9f3-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="9b9f3-197">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="9b9f3-197">Seed the database</span></span>

<span data-ttu-id="9b9f3-198">Utwórz nową klasę o nazwie `SeedData` w folderze *models* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-198">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="9b9f3-199">Jeśli w bazie danych znajdują się jakiekolwiek filmy, inicjatora inicjatora zwraca i nie dodano żadnych filmów.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-199">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="9b9f3-200">Dodawanie inicjatora inicjatora</span><span class="sxs-lookup"><span data-stu-id="9b9f3-200">Add the seed initializer</span></span>

<span data-ttu-id="9b9f3-201">W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-201">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="9b9f3-202">Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-202">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="9b9f3-203">Wywołaj metodę inicjatora, przekazując ją do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-203">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="9b9f3-204">Usuń kontekst, gdy metoda inicjatora zostanie zakończona.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-204">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="9b9f3-205">Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-205">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="9b9f3-206">Aplikacja produkcyjna nie będzie wywoływana `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-206">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="9b9f3-207">Jest on dodawany do poprzedniego kodu, aby zapobiec następującemu wyjątku, `Update-Database` gdy nie został uruchomiony:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-207">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="9b9f3-208">SqlException: Nie można otworzyć bazy danych "RazorPagesMovieContext-21" żądanej przez nazwę logowania.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-208">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="9b9f3-209">Logowanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-209">The login failed.</span></span>
<span data-ttu-id="9b9f3-210">Logowanie użytkownika "Nazwa użytkownika" nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-210">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="9b9f3-211">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9b9f3-211">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9b9f3-212">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b9f3-212">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9b9f3-213">Usuń wszystkie rekordy z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-213">Delete all the records in the DB.</span></span> <span data-ttu-id="9b9f3-214">Można to zrobić za pomocą linków usuwania w przeglądarce lub z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="9b9f3-214">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="9b9f3-215">Wymuś inicjalizację aplikacji (wywołaj metody z `Startup` klasy), aby była uruchamiana Metoda inicjatora.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-215">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="9b9f3-216">Aby wymusić inicjalizację, IIS Express należy zatrzymać i uruchomić ponownie.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-216">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="9b9f3-217">Można to zrobić przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-217">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="9b9f3-218">Kliknij prawym przyciskiem myszy ikonę IIS Express pasku zadań w obszarze powiadomień i naciśnij pozycję **Zakończ** lub **Zatrzymaj witrynę**:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-218">Right-click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Ikona paska zadań IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * <span data-ttu-id="9b9f3-221">W przypadku uruchamiania programu VS w trybie innym niż debugowanie naciśnij klawisz F5, aby uruchomić polecenie w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-221">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="9b9f3-222">W przypadku uruchamiania programu VS w trybie debugowania Zatrzymaj debuger i naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-222">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9b9f3-223">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9b9f3-223">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9b9f3-224">Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora).</span><span class="sxs-lookup"><span data-stu-id="9b9f3-224">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="9b9f3-225">Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-225">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="9b9f3-226">Aplikacja pokazuje dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-226">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9b9f3-227">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9b9f3-227">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9b9f3-228">Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora).</span><span class="sxs-lookup"><span data-stu-id="9b9f3-228">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="9b9f3-229">Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-229">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="9b9f3-230">Aplikacja pokazuje dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-230">The app shows the seeded data.</span></span>

---

<span data-ttu-id="9b9f3-231">Aplikacja pokazuje dane z rozrzutu:</span><span class="sxs-lookup"><span data-stu-id="9b9f3-231">The app shows the seeded data:</span></span>

![Aplikacja filmowa otwarta w programie Chrome pokazująca dane filmu](sql/_static/m55.png)

<span data-ttu-id="9b9f3-233">Następny samouczek czyści prezentację danych.</span><span class="sxs-lookup"><span data-stu-id="9b9f3-233">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b9f3-234">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9b9f3-234">Additional resources</span></span>

* [<span data-ttu-id="9b9f3-235">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="9b9f3-235">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="9b9f3-236">[Ubiegł Razor Pages](xref:tutorials/razor-pages/page)zszkieletem[:
>  Aktualizowanie stron](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="9b9f3-236">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end