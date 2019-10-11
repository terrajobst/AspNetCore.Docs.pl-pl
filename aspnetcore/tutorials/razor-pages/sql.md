---
title: Pracuj z bazą danych i ASP.NET Core
author: rick-anderson
description: Wyjaśnia, jak pracować z bazą danych i ASP.NET Core.
ms.author: riande
ms.date: 7/22/2019
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 87d27b60940826e21b060f2e07d344b30ff75b27
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259783"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="40cf0-103">Pracuj z bazą danych i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40cf0-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="40cf0-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i Jan [Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="40cf0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="40cf0-105">Obiekt `RazorPagesMovieContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="40cf0-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="40cf0-106">Kontekst bazy danych jest zarejestrowany z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w metodzie `ConfigureServices` w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="40cf0-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40cf0-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40cf0-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="40cf0-108">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="40cf0-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="40cf0-109">System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="40cf0-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="40cf0-110">W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="40cf0-110">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40cf0-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40cf0-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="40cf0-112">Wartość nazwy dla bazy danych (`Database={Database name}`) będzie różna dla wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="40cf0-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="40cf0-113">Wartość nazwy jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="40cf0-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie30/appsettings.json)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="40cf0-114">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="40cf0-114">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="40cf0-115">Gdy aplikacja jest wdrażana na serwerze testowym lub produkcyjnym, zmienna środowiskowa może być używana do ustawiania parametrów połączenia na rzeczywistym serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="40cf0-115">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="40cf0-116">Aby uzyskać więcej informacji, zobacz [Konfiguracja](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="40cf0-116">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40cf0-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40cf0-117">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="40cf0-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="40cf0-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="40cf0-119">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express, która jest przeznaczona do tworzenia programów.</span><span class="sxs-lookup"><span data-stu-id="40cf0-119">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="40cf0-120">LocalDB uruchamia się na żądanie i działa w trybie użytkownika, więc nie istnieje złożona konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="40cf0-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="40cf0-121">Domyślnie baza danych LocalDB tworzy pliki `*.mdf` w katalogu `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="40cf0-121">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="40cf0-122">Z menu **Widok** Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="40cf0-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](sql/_static/ssox.png)

* <span data-ttu-id="40cf0-124">Kliknij prawym przyciskiem myszy tabelę `Movie` i wybierz polecenie **Projektant widoków**:</span><span class="sxs-lookup"><span data-stu-id="40cf0-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu kontekstowe są otwierane w tabeli filmów](sql/_static/design.png)

  ![Tabele filmów otwierane w projektancie](sql/_static/dv.png)

<span data-ttu-id="40cf0-127">Zwróć uwagę na ikonę klucza obok `ID`.</span><span class="sxs-lookup"><span data-stu-id="40cf0-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="40cf0-128">Domyślnie EF tworzy właściwość o nazwie `ID` dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="40cf0-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="40cf0-129">Kliknij prawym przyciskiem myszy tabelę `Movie` i wybierz pozycję **Wyświetl dane**:</span><span class="sxs-lookup"><span data-stu-id="40cf0-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Otwórz tabelę filmów pokazującą dane tabeli](sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="40cf0-131">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="40cf0-131">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="40cf0-132">Wypełnianie bazy danych</span><span class="sxs-lookup"><span data-stu-id="40cf0-132">Seed the database</span></span>

<span data-ttu-id="40cf0-133">Utwórz nową klasę o nazwie `SeedData` w folderze *modele* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="40cf0-133">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="40cf0-134">Jeśli w bazie danych znajdują się jakiekolwiek filmy, inicjatora inicjatora zwraca i nie dodano żadnych filmów.</span><span class="sxs-lookup"><span data-stu-id="40cf0-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="40cf0-135">Dodawanie inicjatora inicjatora</span><span class="sxs-lookup"><span data-stu-id="40cf0-135">Add the seed initializer</span></span>

<span data-ttu-id="40cf0-136">W *program.cs*zmień metodę `Main`, aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="40cf0-136">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="40cf0-137">Pobierz wystąpienie kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="40cf0-137">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="40cf0-138">Wywołaj metodę inicjatora, przekazując ją do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="40cf0-138">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="40cf0-139">Usuń kontekst, gdy metoda inicjatora zostanie zakończona.</span><span class="sxs-lookup"><span data-stu-id="40cf0-139">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="40cf0-140">Poniższy kod przedstawia zaktualizowany plik *program.cs* .</span><span class="sxs-lookup"><span data-stu-id="40cf0-140">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Program.cs)]

<span data-ttu-id="40cf0-141">Następujący wyjątek występuje, gdy nie uruchomiono `Update-Database`:</span><span class="sxs-lookup"><span data-stu-id="40cf0-141">The following exception occurs when `Update-Database` has not been run:</span></span>

`SqlException: Cannot open database "RazorPagesMovieContext-" requested by the login. The login failed.`
`Login failed for user 'user name'.`

### <a name="test-the-app"></a><span data-ttu-id="40cf0-142">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="40cf0-142">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40cf0-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40cf0-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="40cf0-144">Usuń wszystkie rekordy z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="40cf0-144">Delete all the records in the DB.</span></span> <span data-ttu-id="40cf0-145">Można to zrobić za pomocą linków usuwania w przeglądarce lub z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="40cf0-145">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="40cf0-146">Wymuś inicjalizację aplikacji (wywołaj metody z klasy `Startup`), aby Metoda inicjatora była uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="40cf0-146">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="40cf0-147">Aby wymusić inicjalizację, IIS Express należy zatrzymać i uruchomić ponownie.</span><span class="sxs-lookup"><span data-stu-id="40cf0-147">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="40cf0-148">Można to zrobić przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="40cf0-148">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="40cf0-149">Kliknij prawym przyciskiem myszy ikonę IIS Express pasku zadań w obszarze powiadomień i naciśnij pozycję **Zakończ** lub **Zatrzymaj witrynę**:</span><span class="sxs-lookup"><span data-stu-id="40cf0-149">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Ikona paska zadań IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * <span data-ttu-id="40cf0-152">W przypadku uruchamiania programu VS w trybie innym niż debugowanie naciśnij klawisz F5, aby uruchomić polecenie w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="40cf0-152">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="40cf0-153">W przypadku uruchamiania programu VS w trybie debugowania Zatrzymaj debuger i naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="40cf0-153">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="40cf0-154">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="40cf0-154">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="40cf0-155">Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora).</span><span class="sxs-lookup"><span data-stu-id="40cf0-155">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="40cf0-156">Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="40cf0-156">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="40cf0-157">Aplikacja pokazuje dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="40cf0-157">The app shows the seeded data.</span></span>

---

<span data-ttu-id="40cf0-158">Następny samouczek poprawi prezentację danych.</span><span class="sxs-lookup"><span data-stu-id="40cf0-158">The next tutorial will improve the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40cf0-159">Zasoby dodatkowe</span><span class="sxs-lookup"><span data-stu-id="40cf0-159">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="40cf0-160">[Poprzedni: Razor Pages szkieletowej](xref:tutorials/razor-pages/page)
> [Dalej: aktualizowanie stron](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="40cf0-160">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="40cf0-161">Obiekt `RazorPagesMovieContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="40cf0-161">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="40cf0-162">Kontekst bazy danych jest zarejestrowany z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w metodzie `ConfigureServices` w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="40cf0-162">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40cf0-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40cf0-163">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="40cf0-164">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="40cf0-164">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="40cf0-165">Aby uzyskać więcej informacji na temat metod używanych w `ConfigureServices`, zobacz:</span><span class="sxs-lookup"><span data-stu-id="40cf0-165">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="40cf0-166">[Obsługa ogólne rozporządzenie o ochronie danych UE (Rodo) w ASP.NET Core](xref:security/gdpr) dla `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="40cf0-166">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="40cf0-167">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="40cf0-167">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="40cf0-168">System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="40cf0-168">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="40cf0-169">W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="40cf0-169">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40cf0-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40cf0-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="40cf0-171">Wartość nazwy dla bazy danych (`Database={Database name}`) będzie różna dla wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="40cf0-171">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="40cf0-172">Wartość nazwy jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="40cf0-172">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="40cf0-173">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="40cf0-173">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="40cf0-174">program Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="40cf0-174">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="40cf0-175">Gdy aplikacja jest wdrażana na serwerze testowym lub produkcyjnym, zmienna środowiskowa może być używana do ustawiania parametrów połączenia na rzeczywistym serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="40cf0-175">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="40cf0-176">Aby uzyskać więcej informacji, zobacz [Konfiguracja](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="40cf0-176">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40cf0-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40cf0-177">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="40cf0-178">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="40cf0-178">SQL Server Express LocalDB</span></span>

<span data-ttu-id="40cf0-179">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express, która jest przeznaczona do tworzenia programów.</span><span class="sxs-lookup"><span data-stu-id="40cf0-179">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="40cf0-180">LocalDB uruchamia się na żądanie i działa w trybie użytkownika, więc nie istnieje złożona konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="40cf0-180">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="40cf0-181">Domyślnie baza danych LocalDB tworzy pliki `*.mdf` w katalogu `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="40cf0-181">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="40cf0-182">Z menu **Widok** Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="40cf0-182">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](sql/_static/ssox.png)

* <span data-ttu-id="40cf0-184">Kliknij prawym przyciskiem myszy tabelę `Movie` i wybierz polecenie **Projektant widoków**:</span><span class="sxs-lookup"><span data-stu-id="40cf0-184">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmów](sql/_static/design.png)

  ![Tabela filmów otwarta w projektancie](sql/_static/dv.png)

<span data-ttu-id="40cf0-187">Zwróć uwagę na ikonę klucza obok `ID`.</span><span class="sxs-lookup"><span data-stu-id="40cf0-187">Note the key icon next to `ID`.</span></span> <span data-ttu-id="40cf0-188">Domyślnie EF tworzy właściwość o nazwie `ID` dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="40cf0-188">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="40cf0-189">Kliknij prawym przyciskiem myszy tabelę `Movie` i wybierz pozycję **Wyświetl dane**:</span><span class="sxs-lookup"><span data-stu-id="40cf0-189">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Otwórz tabelę filmów pokazującą dane tabeli](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="40cf0-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="40cf0-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="40cf0-192">program Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="40cf0-192">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="40cf0-193">Wypełnianie bazy danych</span><span class="sxs-lookup"><span data-stu-id="40cf0-193">Seed the database</span></span>

<span data-ttu-id="40cf0-194">Utwórz nową klasę o nazwie `SeedData` w folderze *modele* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="40cf0-194">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="40cf0-195">Jeśli w bazie danych znajdują się jakiekolwiek filmy, inicjatora inicjatora zwraca i nie dodano żadnych filmów.</span><span class="sxs-lookup"><span data-stu-id="40cf0-195">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="40cf0-196">Dodawanie inicjatora inicjatora</span><span class="sxs-lookup"><span data-stu-id="40cf0-196">Add the seed initializer</span></span>

<span data-ttu-id="40cf0-197">W *program.cs*zmień metodę `Main`, aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="40cf0-197">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="40cf0-198">Pobierz wystąpienie kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="40cf0-198">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="40cf0-199">Wywołaj metodę inicjatora, przekazując ją do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="40cf0-199">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="40cf0-200">Usuń kontekst, gdy metoda inicjatora zostanie zakończona.</span><span class="sxs-lookup"><span data-stu-id="40cf0-200">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="40cf0-201">Poniższy kod przedstawia zaktualizowany plik *program.cs* .</span><span class="sxs-lookup"><span data-stu-id="40cf0-201">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="40cf0-202">Aplikacja produkcyjna nie wywoła `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="40cf0-202">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="40cf0-203">Jest on dodawany do powyższego kodu, aby zapobiec następującym wyjątkom, gdy nie uruchomiono `Update-Database`:</span><span class="sxs-lookup"><span data-stu-id="40cf0-203">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="40cf0-204">SqlException: nie można otworzyć bazy danych "RazorPagesMovieContext-21" żądanej przez nazwę logowania.</span><span class="sxs-lookup"><span data-stu-id="40cf0-204">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="40cf0-205">Logowanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="40cf0-205">The login failed.</span></span>
<span data-ttu-id="40cf0-206">Logowanie użytkownika "Nazwa użytkownika" nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="40cf0-206">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="40cf0-207">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="40cf0-207">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40cf0-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40cf0-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="40cf0-209">Usuń wszystkie rekordy z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="40cf0-209">Delete all the records in the DB.</span></span> <span data-ttu-id="40cf0-210">Można to zrobić za pomocą linków usuwania w przeglądarce lub z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="40cf0-210">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="40cf0-211">Wymuś inicjalizację aplikacji (wywołaj metody z klasy `Startup`), aby Metoda inicjatora była uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="40cf0-211">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="40cf0-212">Aby wymusić inicjalizację, IIS Express należy zatrzymać i uruchomić ponownie.</span><span class="sxs-lookup"><span data-stu-id="40cf0-212">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="40cf0-213">Można to zrobić przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="40cf0-213">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="40cf0-214">Kliknij prawym przyciskiem myszy ikonę IIS Express pasku zadań w obszarze powiadomień i naciśnij pozycję **Zakończ** lub **Zatrzymaj witrynę**:</span><span class="sxs-lookup"><span data-stu-id="40cf0-214">Right-click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Ikona paska zadań IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * <span data-ttu-id="40cf0-217">W przypadku uruchamiania programu VS w trybie innym niż debugowanie naciśnij klawisz F5, aby uruchomić polecenie w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="40cf0-217">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="40cf0-218">W przypadku uruchamiania programu VS w trybie debugowania Zatrzymaj debuger i naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="40cf0-218">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="40cf0-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="40cf0-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="40cf0-220">Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora).</span><span class="sxs-lookup"><span data-stu-id="40cf0-220">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="40cf0-221">Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="40cf0-221">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="40cf0-222">Aplikacja pokazuje dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="40cf0-222">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="40cf0-223">program Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="40cf0-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="40cf0-224">Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora).</span><span class="sxs-lookup"><span data-stu-id="40cf0-224">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="40cf0-225">Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="40cf0-225">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="40cf0-226">Aplikacja pokazuje dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="40cf0-226">The app shows the seeded data.</span></span>

---

<span data-ttu-id="40cf0-227">Aplikacja pokazuje dane z rozrzutu:</span><span class="sxs-lookup"><span data-stu-id="40cf0-227">The app shows the seeded data:</span></span>

![Aplikacja filmowa otwarta w programie Chrome pokazująca dane filmu](sql/_static/m55.png)

<span data-ttu-id="40cf0-229">Następny samouczek czyści prezentację danych.</span><span class="sxs-lookup"><span data-stu-id="40cf0-229">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40cf0-230">Zasoby dodatkowe</span><span class="sxs-lookup"><span data-stu-id="40cf0-230">Additional resources</span></span>

* [<span data-ttu-id="40cf0-231">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="40cf0-231">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="40cf0-232">[Poprzedni: Razor Pages szkieletowej](xref:tutorials/razor-pages/page)
> [Dalej: aktualizowanie stron](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="40cf0-232">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end
