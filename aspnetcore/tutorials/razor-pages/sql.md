---
title: Pracuj z bazą danych i ASP.NET Core
author: rick-anderson
description: Wyjaśnia, jak pracować z bazą danych i ASP.NET Core.
ms.author: riande
ms.date: 7/22/2019
uid: tutorials/razor-pages/sql
ms.openlocfilehash: b5acb573f8fa39e5300ecdb359113d8697d78934
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334227"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="60a88-103">Pracuj z bazą danych i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60a88-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="60a88-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="60a88-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="60a88-105">Obiekt `RazorPagesMovieContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60a88-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="60a88-106">Kontekst bazy danych jest zarejestrowany z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w metodzie `ConfigureServices` w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="60a88-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60a88-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60a88-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="60a88-108">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="60a88-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="60a88-109">System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="60a88-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="60a88-110">W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="60a88-110">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60a88-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60a88-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="60a88-112">Wartość nazwy dla bazy danych (`Database={Database name}`) będzie różna dla wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="60a88-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="60a88-113">Wartość nazwy jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="60a88-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie30/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="60a88-114">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="60a88-114">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="60a88-115">Gdy aplikacja jest wdrażana na serwerze testowym lub produkcyjnym, zmienna środowiskowa może być używana do ustawiania parametrów połączenia na rzeczywistym serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60a88-115">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="60a88-116">Aby uzyskać więcej informacji, zobacz [Konfiguracja](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="60a88-116">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60a88-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60a88-117">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="60a88-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="60a88-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="60a88-119">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express, która jest przeznaczona do tworzenia programów.</span><span class="sxs-lookup"><span data-stu-id="60a88-119">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="60a88-120">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="60a88-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="60a88-121">Domyślnie baza danych LocalDB tworzy pliki `*.mdf` w katalogu `C:\Users\<user>\`.</span><span class="sxs-lookup"><span data-stu-id="60a88-121">By default, LocalDB database creates `*.mdf` files in the `C:\Users\<user>\` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="60a88-122">Z menu **Widok** Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="60a88-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](sql/_static/ssox.png)

* <span data-ttu-id="60a88-124">Kliknij prawym przyciskiem myszy tabelę `Movie` i wybierz polecenie **Projektant widoków**:</span><span class="sxs-lookup"><span data-stu-id="60a88-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu kontekstowe są otwierane w tabeli filmów](sql/_static/design.png)

  ![Tabele filmów otwierane w projektancie](sql/_static/dv.png)

<span data-ttu-id="60a88-127">Zwróć uwagę na ikonę klucza obok `ID`.</span><span class="sxs-lookup"><span data-stu-id="60a88-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="60a88-128">Domyślnie EF tworzy właściwość o nazwie `ID` dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="60a88-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="60a88-129">Kliknij prawym przyciskiem myszy tabelę `Movie` i wybierz polecenie **Wyświetl dane**:</span><span class="sxs-lookup"><span data-stu-id="60a88-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Otwórz tabelę filmów pokazującą dane tabeli](sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="60a88-131">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="60a88-131">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="60a88-132">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="60a88-132">Seed the database</span></span>

<span data-ttu-id="60a88-133">Utwórz nową klasę o nazwie `SeedData` w folderze *modele* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="60a88-133">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="60a88-134">Jeśli w bazie danych znajdują się jakiekolwiek filmy, inicjatora inicjatora zwraca i nie dodano żadnych filmów.</span><span class="sxs-lookup"><span data-stu-id="60a88-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="60a88-135">Dodawanie inicjatora inicjatora</span><span class="sxs-lookup"><span data-stu-id="60a88-135">Add the seed initializer</span></span>

<span data-ttu-id="60a88-136">W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="60a88-136">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="60a88-137">Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="60a88-137">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="60a88-138">Wywołaj metodę inicjatora, przekazując ją do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="60a88-138">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="60a88-139">Usuń kontekst, gdy metoda inicjatora zostanie zakończona.</span><span class="sxs-lookup"><span data-stu-id="60a88-139">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="60a88-140">Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="60a88-140">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Program.cs)]

<span data-ttu-id="60a88-141">Następujący wyjątek występuje, gdy `Update-Database` nie został uruchomiony:</span><span class="sxs-lookup"><span data-stu-id="60a88-141">The following exception occurs when `Update-Database` has not been run:</span></span>

> `SqlException: Cannot open database "RazorPagesMovieContext-" requested by the login. The login failed.`
> `Login failed for user 'user name'.`

### <a name="test-the-app"></a><span data-ttu-id="60a88-142">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="60a88-142">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60a88-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60a88-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="60a88-144">Usuń wszystkie rekordy z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60a88-144">Delete all the records in the DB.</span></span> <span data-ttu-id="60a88-145">Można to zrobić za pomocą linków usuwania w przeglądarce lub z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="60a88-145">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="60a88-146">Wymuś inicjalizację aplikacji (wywołaj metody z klasy `Startup`), aby była uruchamiana Metoda inicjatora.</span><span class="sxs-lookup"><span data-stu-id="60a88-146">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="60a88-147">Aby wymusić inicjalizację, IIS Express należy zatrzymać i uruchomić ponownie.</span><span class="sxs-lookup"><span data-stu-id="60a88-147">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="60a88-148">Można to zrobić przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="60a88-148">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="60a88-149">Kliknij prawym przyciskiem myszy ikonę IIS Express pasku zadań w obszarze powiadomień i naciśnij pozycję **Zakończ** lub **Zatrzymaj witrynę**:</span><span class="sxs-lookup"><span data-stu-id="60a88-149">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Ikona paska zadań IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * <span data-ttu-id="60a88-152">W przypadku uruchamiania programu VS w trybie innym niż debugowanie naciśnij klawisz F5, aby uruchomić polecenie w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="60a88-152">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="60a88-153">W przypadku uruchamiania programu VS w trybie debugowania Zatrzymaj debuger i naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="60a88-153">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="60a88-154">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="60a88-154">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="60a88-155">Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora).</span><span class="sxs-lookup"><span data-stu-id="60a88-155">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="60a88-156">Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="60a88-156">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="60a88-157">Aplikacja pokazuje dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="60a88-157">The app shows the seeded data.</span></span>

---

<span data-ttu-id="60a88-158">Następny samouczek poprawi prezentację danych.</span><span class="sxs-lookup"><span data-stu-id="60a88-158">The next tutorial will improve the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60a88-159">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="60a88-159">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="60a88-160">[Poprzedni: Razor Pages szkieletowe](xref:tutorials/razor-pages/page)
> [Dalej: aktualizowanie stron](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="60a88-160">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="60a88-161">Obiekt `RazorPagesMovieContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60a88-161">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="60a88-162">Kontekst bazy danych jest zarejestrowany z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w metodzie `ConfigureServices` w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="60a88-162">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60a88-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60a88-163">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="60a88-164">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="60a88-164">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="60a88-165">Aby uzyskać więcej informacji na temat metod używanych w `ConfigureServices`, zobacz:</span><span class="sxs-lookup"><span data-stu-id="60a88-165">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="60a88-166">[Obsługa ogólne rozporządzenie o ochronie danych UE (Rodo) w ASP.NET Core](xref:security/gdpr) dla `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="60a88-166">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="60a88-167">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="60a88-167">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="60a88-168">System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="60a88-168">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="60a88-169">W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="60a88-169">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60a88-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60a88-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="60a88-171">Wartość nazwy dla bazy danych (`Database={Database name}`) będzie różna dla wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="60a88-171">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="60a88-172">Wartość nazwy jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="60a88-172">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="60a88-173">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="60a88-173">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="60a88-174">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="60a88-174">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="60a88-175">Gdy aplikacja jest wdrażana na serwerze testowym lub produkcyjnym, zmienna środowiskowa może być używana do ustawiania parametrów połączenia na rzeczywistym serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60a88-175">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="60a88-176">Aby uzyskać więcej informacji, zobacz [Konfiguracja](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="60a88-176">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60a88-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60a88-177">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="60a88-178">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="60a88-178">SQL Server Express LocalDB</span></span>

<span data-ttu-id="60a88-179">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express, która jest przeznaczona do tworzenia programów.</span><span class="sxs-lookup"><span data-stu-id="60a88-179">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="60a88-180">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="60a88-180">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="60a88-181">Domyślnie baza danych LocalDB tworzy pliki `*.mdf` w katalogu `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="60a88-181">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="60a88-182">Z menu **Widok** Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="60a88-182">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](sql/_static/ssox.png)

* <span data-ttu-id="60a88-184">Kliknij prawym przyciskiem myszy tabelę `Movie` i wybierz polecenie **Projektant widoków**:</span><span class="sxs-lookup"><span data-stu-id="60a88-184">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmów](sql/_static/design.png)

  ![Tabela filmów otwarta w projektancie](sql/_static/dv.png)

<span data-ttu-id="60a88-187">Zwróć uwagę na ikonę klucza obok `ID`.</span><span class="sxs-lookup"><span data-stu-id="60a88-187">Note the key icon next to `ID`.</span></span> <span data-ttu-id="60a88-188">Domyślnie EF tworzy właściwość o nazwie `ID` dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="60a88-188">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="60a88-189">Kliknij prawym przyciskiem myszy tabelę `Movie` i wybierz polecenie **Wyświetl dane**:</span><span class="sxs-lookup"><span data-stu-id="60a88-189">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Otwórz tabelę filmów pokazującą dane tabeli](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="60a88-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="60a88-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="60a88-192">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="60a88-192">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="60a88-193">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="60a88-193">Seed the database</span></span>

<span data-ttu-id="60a88-194">Utwórz nową klasę o nazwie `SeedData` w folderze *modele* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="60a88-194">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="60a88-195">Jeśli w bazie danych znajdują się jakiekolwiek filmy, inicjatora inicjatora zwraca i nie dodano żadnych filmów.</span><span class="sxs-lookup"><span data-stu-id="60a88-195">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="60a88-196">Dodawanie inicjatora inicjatora</span><span class="sxs-lookup"><span data-stu-id="60a88-196">Add the seed initializer</span></span>

<span data-ttu-id="60a88-197">W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="60a88-197">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="60a88-198">Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="60a88-198">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="60a88-199">Wywołaj metodę inicjatora, przekazując ją do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="60a88-199">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="60a88-200">Usuń kontekst, gdy metoda inicjatora zostanie zakończona.</span><span class="sxs-lookup"><span data-stu-id="60a88-200">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="60a88-201">Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="60a88-201">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="60a88-202">Aplikacja produkcyjna nie wywoła `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="60a88-202">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="60a88-203">Jest on dodawany do poprzedniego kodu, aby zapobiec następującym wyjątkom, gdy `Update-Database` nie został uruchomiony:</span><span class="sxs-lookup"><span data-stu-id="60a88-203">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="60a88-204">SqlException: nie można otworzyć bazy danych "RazorPagesMovieContext-21" żądanej przez nazwę logowania.</span><span class="sxs-lookup"><span data-stu-id="60a88-204">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="60a88-205">Logowanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="60a88-205">The login failed.</span></span>
<span data-ttu-id="60a88-206">Logowanie użytkownika "Nazwa użytkownika" nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="60a88-206">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="60a88-207">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="60a88-207">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60a88-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60a88-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="60a88-209">Usuń wszystkie rekordy z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60a88-209">Delete all the records in the DB.</span></span> <span data-ttu-id="60a88-210">Można to zrobić za pomocą linków usuwania w przeglądarce lub z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="60a88-210">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="60a88-211">Wymuś inicjalizację aplikacji (wywołaj metody z klasy `Startup`), aby była uruchamiana Metoda inicjatora.</span><span class="sxs-lookup"><span data-stu-id="60a88-211">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="60a88-212">Aby wymusić inicjalizację, IIS Express należy zatrzymać i uruchomić ponownie.</span><span class="sxs-lookup"><span data-stu-id="60a88-212">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="60a88-213">Można to zrobić przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="60a88-213">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="60a88-214">Kliknij prawym przyciskiem myszy ikonę IIS Express pasku zadań w obszarze powiadomień i naciśnij pozycję **Zakończ** lub **Zatrzymaj witrynę**:</span><span class="sxs-lookup"><span data-stu-id="60a88-214">Right-click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Ikona paska zadań IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * <span data-ttu-id="60a88-217">W przypadku uruchamiania programu VS w trybie innym niż debugowanie naciśnij klawisz F5, aby uruchomić polecenie w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="60a88-217">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="60a88-218">W przypadku uruchamiania programu VS w trybie debugowania Zatrzymaj debuger i naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="60a88-218">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="60a88-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="60a88-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="60a88-220">Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora).</span><span class="sxs-lookup"><span data-stu-id="60a88-220">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="60a88-221">Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="60a88-221">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="60a88-222">Aplikacja pokazuje dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="60a88-222">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="60a88-223">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="60a88-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="60a88-224">Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora).</span><span class="sxs-lookup"><span data-stu-id="60a88-224">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="60a88-225">Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="60a88-225">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="60a88-226">Aplikacja pokazuje dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="60a88-226">The app shows the seeded data.</span></span>

---

<span data-ttu-id="60a88-227">Aplikacja pokazuje dane z rozrzutu:</span><span class="sxs-lookup"><span data-stu-id="60a88-227">The app shows the seeded data:</span></span>

![Aplikacja filmowa otwarta w programie Chrome pokazująca dane filmu](sql/_static/m55.png)

<span data-ttu-id="60a88-229">Następny samouczek czyści prezentację danych.</span><span class="sxs-lookup"><span data-stu-id="60a88-229">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60a88-230">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="60a88-230">Additional resources</span></span>

* [<span data-ttu-id="60a88-231">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="60a88-231">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="60a88-232">[Poprzedni: Razor Pages szkieletowe](xref:tutorials/razor-pages/page)
> [Dalej: aktualizowanie stron](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="60a88-232">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end
