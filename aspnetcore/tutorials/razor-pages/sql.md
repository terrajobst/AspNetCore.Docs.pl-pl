---
title: Praca z bazy danych LocalDB programu SQL Server i platformy ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono pracy z bazy danych LocalDB programu SQL Server i ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 1fd86fb61b7f5ddb760992ac10f9bee43ab831cf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272146"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="01514-103">Praca z bazy danych LocalDB programu SQL Server i platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01514-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="01514-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="01514-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="01514-105">`MovieContext` Obiektu obsługuje zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="01514-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="01514-106">Kontekst bazy danych został zarejestrowany za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="01514-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="01514-107">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]</span><span class="sxs-lookup"><span data-stu-id="01514-107">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="01514-108">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="01514-108">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]</span></span>

<span data-ttu-id="01514-109">Aby uzyskać więcej informacji na temat metody używane w `ConfigureServices`, zobacz:</span><span class="sxs-lookup"><span data-stu-id="01514-109">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="01514-110">[Obsługa interfejsów UE ogólne dane ochrony rozporządzenia (GDPR) w ASP.NET Core](xref:security/gdpr) dla `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="01514-110">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="01514-111">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="01514-111">SetCompatibilityVersion</span></span>](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

<span data-ttu-id="01514-112">Platformy ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty systemu `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="01514-112">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="01514-113">Lokalne działania projektowe, pobiera parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="01514-113">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="01514-114">Wartość Nazwa bazy danych (`Database={Database name}`) mogą być inne dla wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="01514-114">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="01514-115">Wartość nazwy jest dowolnego.</span><span class="sxs-lookup"><span data-stu-id="01514-115">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="01514-116">Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym, można użyć zmiennej środowiskowej lub innej metody, aby ustawić parametry połączenia do rzeczywistego programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="01514-116">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="01514-117">Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="01514-117">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="01514-118">SQL Server Express LocalDB.</span><span class="sxs-lookup"><span data-stu-id="01514-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="01514-119">To Zubożona wersja programu SQL Server Express aparat bazy danych jest przeznaczony do tworzenia aplikacji programu.</span><span class="sxs-lookup"><span data-stu-id="01514-119">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="01514-120">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="01514-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="01514-121">Domyślnie tworzy bazy danych LocalDB "\*.mdf" plików *C:/Users/\<użytkownika\>*  katalogu.</span><span class="sxs-lookup"><span data-stu-id="01514-121">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="01514-122">Z **widoku** menu, otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="01514-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](sql/_static/ssox.png)

* <span data-ttu-id="01514-124">Kliknij prawym przyciskiem myszy `Movie` tabeli i wybierz **Widok projektanta**:</span><span class="sxs-lookup"><span data-stu-id="01514-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu kontekstowe otwarty na tabeli film](sql/_static/design.png)

  ![Otwórz w projektancie tabeli film](sql/_static/dv.png)

<span data-ttu-id="01514-127">Należy pamiętać ikonę klucza, obok pozycji `ID`.</span><span class="sxs-lookup"><span data-stu-id="01514-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="01514-128">Domyślnie EF tworzy właściwość o nazwie `ID` klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="01514-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="01514-129">Kliknij prawym przyciskiem myszy `Movie` tabeli i wybierz **danych widoku**:</span><span class="sxs-lookup"><span data-stu-id="01514-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Otwórz tabelę dane są wyświetlane tabeli film](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="01514-131">Inicjatora bazy danych</span><span class="sxs-lookup"><span data-stu-id="01514-131">Seed the database</span></span>

<span data-ttu-id="01514-132">Utwórz nową klasę o nazwie `SeedData` w *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="01514-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="01514-133">Zastąp wygenerowany kod poniżej:</span><span class="sxs-lookup"><span data-stu-id="01514-133">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="01514-134">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="01514-134">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="01514-135">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="01514-135">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]</span></span>

::: moniker-end

<span data-ttu-id="01514-136">Jeśli w bazie danych są wszystkie filmy, zwraca inicjatora inicjatora i filmy nie zostaną dodane.</span><span class="sxs-lookup"><span data-stu-id="01514-136">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="01514-137">Dodaj inicjatora inicjatora</span><span class="sxs-lookup"><span data-stu-id="01514-137">Add the seed initializer</span></span>

<span data-ttu-id="01514-138">W *Program.cs*, zmodyfikuj `Main` metody wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="01514-138">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="01514-139">Pobierz wystąpienia kontekstu DB z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="01514-139">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="01514-140">Wywołaj metodę inicjatora, przekazanie jej w kontekście.</span><span class="sxs-lookup"><span data-stu-id="01514-140">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="01514-141">Po zakończeniu metody inicjatora, należy dysponować kontekstu.</span><span class="sxs-lookup"><span data-stu-id="01514-141">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="01514-142">Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="01514-142">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="01514-143">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="01514-143">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="01514-144">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="01514-144">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]</span></span>

::: moniker-end

<span data-ttu-id="01514-145">Nie wywołać aplikacji produkcyjnej `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="01514-145">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="01514-146">Jest ona dodawana do kodu poprzedzających, aby zapobiec następujący wyjątek podczas `Update-Database` nie zostało uruchomione:</span><span class="sxs-lookup"><span data-stu-id="01514-146">It's added to the preceeding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="01514-147">SqlException: Nie można otworzyć bazy danych "RazorPagesMovieContext-21" żądanego podczas logowania.</span><span class="sxs-lookup"><span data-stu-id="01514-147">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="01514-148">Logowanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="01514-148">The login failed.</span></span>
<span data-ttu-id="01514-149">Użytkownik "Nazwa użytkownika" Logowanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="01514-149">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="01514-150">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="01514-150">Test the app</span></span>

* <span data-ttu-id="01514-151">Usuń wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="01514-151">Delete all the records in the DB.</span></span> <span data-ttu-id="01514-152">Można to zrobić z łączami Usuń w przeglądarce lub z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="01514-152">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="01514-153">Wymusić na aplikacji, aby zainicjować (wywołanie metody w `Startup` klasy), uruchamia seed — metoda.</span><span class="sxs-lookup"><span data-stu-id="01514-153">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="01514-154">Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie.</span><span class="sxs-lookup"><span data-stu-id="01514-154">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="01514-155">Można to zrobić za pomocą dowolnego z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="01514-155">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="01514-156">Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie wybierz polecenie **zakończenia** lub **Zatrzymaj witrynę**:</span><span class="sxs-lookup"><span data-stu-id="01514-156">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Usługi IIS Express ikony paska zadań](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * <span data-ttu-id="01514-159">Podczas uruchamiania programu VS w trybie bez debugowania, naciśnij klawisz F5, aby uruchomić w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="01514-159">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="01514-160">W przypadku korzystania z wersji programu VS w trybie debugowania, zatrzymaniu debugera i naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="01514-160">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="01514-161">Aplikacja zawiera wprowadzonych danych:</span><span class="sxs-lookup"><span data-stu-id="01514-161">The app shows the seeded data:</span></span>

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji film](sql/_static/m55.png)

<span data-ttu-id="01514-163">Następny samouczek spowoduje oczyszczenie prezentacji danych.</span><span class="sxs-lookup"><span data-stu-id="01514-163">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="01514-164">[Poprzedni: Tworzenie szkieletu stron Razor](xref:tutorials/razor-pages/page)
> [dalej: aktualizowanie stron](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="01514-164">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
