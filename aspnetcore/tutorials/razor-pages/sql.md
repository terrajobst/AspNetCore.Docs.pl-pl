---
title: Praca z programu SQL Server LocalDB i ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono, pracy z programu SQL Server LocalDB i ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 255faf12064aa424d51fb6faa801884c474bd288
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889486"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="3ce8c-103">Praca z programu SQL Server LocalDB i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ce8c-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="3ce8c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="3ce8c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="3ce8c-105">`MovieContext` Obiektu obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="3ce8c-106">Kontekst bazy danych jest zarejestrowany w [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="3ce8c-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="3ce8c-107">Aby uzyskać więcej informacji na temat metod używanych w `ConfigureServices`, zobacz:</span><span class="sxs-lookup"><span data-stu-id="3ce8c-107">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="3ce8c-108">[Obsługa Unii Europejskiej ogólnego danych (GDPR Protection Regulation) w programie ASP.NET Core](xref:security/gdpr) dla `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-108">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="3ce8c-109">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="3ce8c-109">SetCompatibilityVersion</span></span>](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

<span data-ttu-id="3ce8c-110">ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty system `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-110">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="3ce8c-111">Dla wdrożenia lokalnego, pobiera parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-111">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="3ce8c-112">Wartość nazwy bazy danych (`Database={Database name}`) będzie różna dla wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="3ce8c-113">Wartość nazwy jest dowolnego.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="3ce8c-114">Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym, można użyć zmiennej środowiskowej lub innej metody, które można ustawić parametrów połączenia do rzeczywistego programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-114">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="3ce8c-115">Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-115">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="3ce8c-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="3ce8c-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="3ce8c-117">LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych, która jest przeznaczona do tworzenia programu.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="3ce8c-118">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="3ce8c-119">Domyślnie tworzy bazy danych LocalDB "\*.mdf" pliki *C:/Users/\<użytkownika\>*  katalogu.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-119">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="3ce8c-120">Z **widoku** menu Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="3ce8c-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](sql/_static/ssox.png)

* <span data-ttu-id="3ce8c-122">Kliknij prawym przyciskiem myszy `Movie` tabeli, a następnie wybierz pozycję **Projektant widoków**:</span><span class="sxs-lookup"><span data-stu-id="3ce8c-122">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmu](sql/_static/design.png)

  ![Otwórz w Projektancie tabel filmu](sql/_static/dv.png)

<span data-ttu-id="3ce8c-125">Należy zauważyć ikonę klucza, obok `ID`.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="3ce8c-126">Domyślnie program EF tworzy właściwość o nazwie `ID` dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-126">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="3ce8c-127">Kliknij prawym przyciskiem myszy `Movie` tabeli, a następnie wybierz pozycję **dane widoku**:</span><span class="sxs-lookup"><span data-stu-id="3ce8c-127">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tabela filmu otwarte, wyświetlanie tabeli danych](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="3ce8c-129">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="3ce8c-129">Seed the database</span></span>

<span data-ttu-id="3ce8c-130">Utwórz nową klasę o nazwie `SeedData` w *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-130">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="3ce8c-131">Zastąp wygenerowany kod następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="3ce8c-131">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

<span data-ttu-id="3ce8c-132">Czy istnieją wszystkie filmy w bazie danych, zwraca inicjatora inicjatora i filmy nie zostaną dodane.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-132">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="3ce8c-133">Dodaj inicjator inicjatora</span><span class="sxs-lookup"><span data-stu-id="3ce8c-133">Add the seed initializer</span></span>

<span data-ttu-id="3ce8c-134">W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="3ce8c-134">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="3ce8c-135">Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-135">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="3ce8c-136">Wywołaj metodę inicjatora, przekazując mu kontekst.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-136">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="3ce8c-137">Po zakończeniu seed — metoda, należy dysponować kontekstu.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-137">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="3ce8c-138">Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-138">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

<span data-ttu-id="3ce8c-139">Nie może wywołać aplikacji produkcyjnej `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-139">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="3ce8c-140">Jest ona dodawana do poprzedniego kodu, aby zapobiec następujący wyjątek podczas `Update-Database` nie zostały uruchomione:</span><span class="sxs-lookup"><span data-stu-id="3ce8c-140">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="3ce8c-141">Sqlexception —: Nie można otworzyć bazy danych "RazorPagesMovieContext-21" żądanego podczas logowania.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-141">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="3ce8c-142">Logowanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-142">The login failed.</span></span>
<span data-ttu-id="3ce8c-143">Nie można zalogować użytkownika "Nazwa użytkownika".</span><span class="sxs-lookup"><span data-stu-id="3ce8c-143">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="3ce8c-144">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3ce8c-144">Test the app</span></span>

* <span data-ttu-id="3ce8c-145">Usuń wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-145">Delete all the records in the DB.</span></span> <span data-ttu-id="3ce8c-146">Można to zrobić za pomocą łącza delete w przeglądarce, albo z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="3ce8c-146">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="3ce8c-147">Wymusić na aplikacji, aby zainicjować (wywoływanie metody w `Startup` klasy), uruchamia seed — metoda.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-147">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="3ce8c-148">Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-148">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="3ce8c-149">To zrobić przy użyciu dowolnej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="3ce8c-149">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="3ce8c-150">Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie naciśnij pozycję **zakończenia** lub **zatrzymać witrynę**:</span><span class="sxs-lookup"><span data-stu-id="3ce8c-150">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Usługi IIS Express ikony na pasku zadań](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * <span data-ttu-id="3ce8c-153">Podczas uruchamiania w trybie bez debugowania programu VS, naciśnij klawisz F5, aby uruchomić w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-153">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="3ce8c-154">Podczas uruchamiania programu VS w trybie debugowania, Zatrzymaj debuger, a następnie naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-154">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="3ce8c-155">Aplikacja przedstawiono wprowadzonych danych:</span><span class="sxs-lookup"><span data-stu-id="3ce8c-155">The app shows the seeded data:</span></span>

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji filmów](sql/_static/m55.png)

<span data-ttu-id="3ce8c-157">Następny samouczek spowoduje oczyszczenie prezentacji danych.</span><span class="sxs-lookup"><span data-stu-id="3ce8c-157">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3ce8c-158">[Poprzedni: Działanie stron Razor](xref:tutorials/razor-pages/page)
> [dalej: aktualizowanie stron](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="3ce8c-158">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
