---
title: Korzystanie z języka SQL w aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się więcej o używaniu SQL Server LocalDB lub oprogramowania SQLite w aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 8/16/2019
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: cb356bca50540d7c471cf625a26bfe2dd155b627
ms.sourcegitcommit: 3ffcd8cbff8b49128733842f72270bc58279de70
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2019
ms.locfileid: "71955919"
---
# <a name="work-with-sql-in-aspnet-core"></a><span data-ttu-id="997c3-103">Współpraca z SQL w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="997c3-103">Work with SQL in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="997c3-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="997c3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="997c3-105">Obiekt `MvcMovieContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="997c3-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="997c3-106">Kontekst bazy danych jest zarejestrowany z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w metodzie `ConfigureServices` w pliku *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="997c3-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="997c3-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="997c3-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

<span data-ttu-id="997c3-108">System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="997c3-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="997c3-109">W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="997c3-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="997c3-110">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="997c3-110">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

<span data-ttu-id="997c3-111">System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="997c3-111">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="997c3-112">W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="997c3-112">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

<span data-ttu-id="997c3-113">Gdy aplikacja jest wdrażana na serwerze testowym lub produkcyjnym, zmienna środowiskowa może służyć do ustawiania parametrów połączenia do SQL Server produkcyjnej.</span><span class="sxs-lookup"><span data-stu-id="997c3-113">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a production SQL Server.</span></span> <span data-ttu-id="997c3-114">Aby uzyskać więcej informacji, zobacz [Konfiguracja](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="997c3-114">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="997c3-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="997c3-115">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="997c3-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="997c3-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="997c3-117">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express, która jest przeznaczona do tworzenia programów.</span><span class="sxs-lookup"><span data-stu-id="997c3-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="997c3-118">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="997c3-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="997c3-119">Domyślnie baza danych LocalDB tworzy pliki *MDF* w katalogu *C:/Users/{User}* .</span><span class="sxs-lookup"><span data-stu-id="997c3-119">By default, LocalDB database creates *.mdf* files in the *C:/Users/{user}* directory.</span></span>

* <span data-ttu-id="997c3-120">Z menu **Widok** Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="997c3-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](working-with-sql/_static/ssox.png)

* <span data-ttu-id="997c3-122">Kliknij prawym przyciskiem myszy tabelę `Movie` **> widok projektanta**</span><span class="sxs-lookup"><span data-stu-id="997c3-122">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmów](working-with-sql/_static/design.png)

  ![Tabela filmów otwarta w projektancie](working-with-sql/_static/dv.png)

<span data-ttu-id="997c3-125">Zwróć uwagę na ikonę klucza obok `ID`.</span><span class="sxs-lookup"><span data-stu-id="997c3-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="997c3-126">Domyślnie, EF wprowadzi właściwość o nazwie `ID` klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="997c3-126">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="997c3-127">Kliknij prawym przyciskiem myszy tabelę `Movie` **> Wyświetl dane**</span><span class="sxs-lookup"><span data-stu-id="997c3-127">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmów](working-with-sql/_static/ssox2.png)

  ![Otwórz tabelę filmów pokazującą dane tabeli](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="997c3-130">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="997c3-130">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="997c3-131">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="997c3-131">Seed the database</span></span>

<span data-ttu-id="997c3-132">Utwórz nową klasę o nazwie `SeedData` w folderze *modele* .</span><span class="sxs-lookup"><span data-stu-id="997c3-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="997c3-133">Zastąp wygenerowany kod następującym:</span><span class="sxs-lookup"><span data-stu-id="997c3-133">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="997c3-134">Jeśli w bazie danych znajdują się jakiekolwiek filmy, inicjatora inicjatora zwraca i nie dodano żadnych filmów.</span><span class="sxs-lookup"><span data-stu-id="997c3-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="997c3-135">Dodawanie inicjatora inicjatora</span><span class="sxs-lookup"><span data-stu-id="997c3-135">Add the seed initializer</span></span>

<span data-ttu-id="997c3-136">Zastąp zawartość *program.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="997c3-136">Replace the contents of *Program.cs* with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Program.cs)]

<span data-ttu-id="997c3-137">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="997c3-137">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="997c3-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="997c3-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="997c3-139">Usuń wszystkie rekordy z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="997c3-139">Delete all the records in the DB.</span></span> <span data-ttu-id="997c3-140">Można to zrobić za pomocą linków usuwania w przeglądarce lub z SSOX.</span><span class="sxs-lookup"><span data-stu-id="997c3-140">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="997c3-141">Wymuś inicjalizację aplikacji (wywołaj metody z klasy `Startup`), aby Metoda inicjatora była uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="997c3-141">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="997c3-142">Aby wymusić inicjalizację, IIS Express należy zatrzymać i uruchomić ponownie.</span><span class="sxs-lookup"><span data-stu-id="997c3-142">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="997c3-143">Można to zrobić przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="997c3-143">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="997c3-144">Kliknij prawym przyciskiem myszy ikonę IIS Express pasku zadań w obszarze powiadomień i naciśnij pozycję **Zakończ** lub **Zatrzymaj witrynę**</span><span class="sxs-lookup"><span data-stu-id="997c3-144">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Ikona paska zadań IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="997c3-147">W przypadku uruchamiania programu VS w trybie innym niż debugowanie naciśnij klawisz F5, aby uruchomić w trybie debugowania</span><span class="sxs-lookup"><span data-stu-id="997c3-147">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="997c3-148">W przypadku uruchamiania programu VS w trybie debugowania Zatrzymaj debuger i naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="997c3-148">If you were running VS in debug mode, stop the debugger and press F5</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="997c3-149">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="997c3-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="997c3-150">Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora).</span><span class="sxs-lookup"><span data-stu-id="997c3-150">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="997c3-151">Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="997c3-151">Stop and start the app to seed the database.</span></span>

---

<span data-ttu-id="997c3-152">Aplikacja pokazuje dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="997c3-152">The app shows the seeded data.</span></span>

![Aplikacja filmowa MVC otwarta w przeglądarce Microsoft Edge pokazująca dane filmu](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="997c3-154">[Poprzedni](adding-model.md)
> [Następny](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="997c3-154">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="997c3-155">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="997c3-155">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="997c3-156">Obiekt `MvcMovieContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="997c3-156">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="997c3-157">Kontekst bazy danych jest zarejestrowany z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w metodzie `ConfigureServices` w pliku *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="997c3-157">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="997c3-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="997c3-158">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="997c3-159">System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="997c3-159">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="997c3-160">W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="997c3-160">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="997c3-161">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="997c3-161">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="997c3-162">System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="997c3-162">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="997c3-163">W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="997c3-163">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

<span data-ttu-id="997c3-164">Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym można użyć zmiennej środowiskowej lub innego podejścia do ustawiania parametrów połączenia dla rzeczywistych SQL Server.</span><span class="sxs-lookup"><span data-stu-id="997c3-164">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="997c3-165">Aby uzyskać więcej informacji, zobacz [Konfiguracja](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="997c3-165">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="997c3-166">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="997c3-166">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="997c3-167">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="997c3-167">SQL Server Express LocalDB</span></span>

<span data-ttu-id="997c3-168">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express, która jest przeznaczona do tworzenia programów.</span><span class="sxs-lookup"><span data-stu-id="997c3-168">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="997c3-169">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="997c3-169">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="997c3-170">Domyślnie baza danych LocalDB tworzy pliki *MDF* w katalogu *C:/Users/{User}* .</span><span class="sxs-lookup"><span data-stu-id="997c3-170">By default, LocalDB database creates *.mdf* files in the *C:/Users/{user}* directory.</span></span>

* <span data-ttu-id="997c3-171">Z menu **Widok** Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="997c3-171">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](working-with-sql/_static/ssox.png)

* <span data-ttu-id="997c3-173">Kliknij prawym przyciskiem myszy tabelę `Movie` **> widok projektanta**</span><span class="sxs-lookup"><span data-stu-id="997c3-173">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmów](working-with-sql/_static/design.png)

  ![Tabela filmów otwarta w projektancie](working-with-sql/_static/dv.png)

<span data-ttu-id="997c3-176">Zwróć uwagę na ikonę klucza obok `ID`.</span><span class="sxs-lookup"><span data-stu-id="997c3-176">Note the key icon next to `ID`.</span></span> <span data-ttu-id="997c3-177">Domyślnie, EF wprowadzi właściwość o nazwie `ID` klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="997c3-177">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="997c3-178">Kliknij prawym przyciskiem myszy tabelę `Movie` **> Wyświetl dane**</span><span class="sxs-lookup"><span data-stu-id="997c3-178">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmów](working-with-sql/_static/ssox2.png)

  ![Otwórz tabelę filmów pokazującą dane tabeli](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="997c3-181">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="997c3-181">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="997c3-182">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="997c3-182">Seed the database</span></span>

<span data-ttu-id="997c3-183">Utwórz nową klasę o nazwie `SeedData` w folderze *modele* .</span><span class="sxs-lookup"><span data-stu-id="997c3-183">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="997c3-184">Zastąp wygenerowany kod następującym:</span><span class="sxs-lookup"><span data-stu-id="997c3-184">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="997c3-185">Jeśli w bazie danych znajdują się jakiekolwiek filmy, inicjatora inicjatora zwraca i nie dodano żadnych filmów.</span><span class="sxs-lookup"><span data-stu-id="997c3-185">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="997c3-186">Dodawanie inicjatora inicjatora</span><span class="sxs-lookup"><span data-stu-id="997c3-186">Add the seed initializer</span></span>

<span data-ttu-id="997c3-187">Zastąp zawartość *program.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="997c3-187">Replace the contents of *Program.cs* with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

<span data-ttu-id="997c3-188">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="997c3-188">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="997c3-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="997c3-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="997c3-190">Usuń wszystkie rekordy z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="997c3-190">Delete all the records in the DB.</span></span> <span data-ttu-id="997c3-191">Można to zrobić za pomocą linków usuwania w przeglądarce lub z SSOX.</span><span class="sxs-lookup"><span data-stu-id="997c3-191">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="997c3-192">Wymuś inicjalizację aplikacji (wywołaj metody z klasy `Startup`), aby Metoda inicjatora była uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="997c3-192">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="997c3-193">Aby wymusić inicjalizację, IIS Express należy zatrzymać i uruchomić ponownie.</span><span class="sxs-lookup"><span data-stu-id="997c3-193">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="997c3-194">Można to zrobić przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="997c3-194">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="997c3-195">Kliknij prawym przyciskiem myszy ikonę IIS Express pasku zadań w obszarze powiadomień i naciśnij pozycję **Zakończ** lub **Zatrzymaj witrynę**</span><span class="sxs-lookup"><span data-stu-id="997c3-195">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Ikona paska zadań IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="997c3-198">W przypadku uruchamiania programu VS w trybie innym niż debugowanie naciśnij klawisz F5, aby uruchomić w trybie debugowania</span><span class="sxs-lookup"><span data-stu-id="997c3-198">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="997c3-199">W przypadku uruchamiania programu VS w trybie debugowania Zatrzymaj debuger i naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="997c3-199">If you were running VS in debug mode, stop the debugger and press F5</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="997c3-200">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="997c3-200">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="997c3-201">Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora).</span><span class="sxs-lookup"><span data-stu-id="997c3-201">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="997c3-202">Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="997c3-202">Stop and start the app to seed the database.</span></span>

---

<span data-ttu-id="997c3-203">Aplikacja pokazuje dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="997c3-203">The app shows the seeded data.</span></span>

![Aplikacja filmowa MVC otwarta w przeglądarce Microsoft Edge pokazująca dane filmu](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="997c3-205">[Poprzedni](adding-model.md)
> [Następny](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="997c3-205">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>

::: moniker-end
