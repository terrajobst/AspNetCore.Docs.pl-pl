---
title: Praca z bazy danych LocalDB programu SQL Server w platformy ASP.NET Core
author: rick-anderson
description: Informacje o używaniu bazy danych LocalDB programu SQL Server w prostej aplikacji ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: b00d709f3009f63431becf24797269ad5988a20b
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34688377"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a><span data-ttu-id="92c90-103">Praca z bazy danych LocalDB programu SQL Server w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92c90-103">Work with SQL Server LocalDB in ASP.NET Core</span></span>

<span data-ttu-id="92c90-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="92c90-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="92c90-105">`MvcMovieContext` Obiektu obsługuje zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="92c90-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="92c90-106">Kontekst bazy danych został zarejestrowany za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="92c90-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="92c90-107">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span><span class="sxs-lookup"><span data-stu-id="92c90-107">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="92c90-108">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="92c90-108">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>
::: moniker-end

<span data-ttu-id="92c90-109">Platformy ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty systemu `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="92c90-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="92c90-110">Lokalne działania projektowe, pobiera parametry połączenia z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="92c90-110">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="92c90-111">Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym, można użyć zmiennej środowiskowej lub innej metody, aby ustawić parametry połączenia do rzeczywistego programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="92c90-111">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="92c90-112">Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="92c90-112">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="92c90-113">SQL Server Express LocalDB.</span><span class="sxs-lookup"><span data-stu-id="92c90-113">SQL Server Express LocalDB</span></span>

<span data-ttu-id="92c90-114">To Zubożona wersja programu SQL Server Express aparat bazy danych jest przeznaczony do tworzenia aplikacji programu.</span><span class="sxs-lookup"><span data-stu-id="92c90-114">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="92c90-115">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="92c90-115">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="92c90-116">Domyślnie tworzy bazy danych LocalDB "\*.mdf" plików *C:/Users/\<użytkownika\>*  katalogu.</span><span class="sxs-lookup"><span data-stu-id="92c90-116">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="92c90-117">Z **widoku** menu, otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="92c90-117">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](working-with-sql/_static/ssox.png)

* <span data-ttu-id="92c90-119">Kliknij prawym przyciskiem myszy `Movie` tabeli **> Widok projektanta**</span><span class="sxs-lookup"><span data-stu-id="92c90-119">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu kontekstowe otwarty na tabeli film](working-with-sql/_static/design.png)

  ![Otwórz w projektancie tabeli film](working-with-sql/_static/dv.png)

<span data-ttu-id="92c90-122">Należy pamiętać ikonę klucza, obok pozycji `ID`.</span><span class="sxs-lookup"><span data-stu-id="92c90-122">Note the key icon next to `ID`.</span></span> <span data-ttu-id="92c90-123">Domyślnie EF spowoduje, że właściwość o nazwie `ID` klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="92c90-123">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="92c90-124">Kliknij prawym przyciskiem myszy `Movie` tabeli **> Wyświetlanie danych**</span><span class="sxs-lookup"><span data-stu-id="92c90-124">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu kontekstowe otwarty na tabeli film](working-with-sql/_static/ssox2.png)

  ![Otwórz tabelę dane są wyświetlane tabeli film](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="92c90-127">Inicjatora bazy danych</span><span class="sxs-lookup"><span data-stu-id="92c90-127">Seed the database</span></span>

<span data-ttu-id="92c90-128">Utwórz nową klasę o nazwie `SeedData` w *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="92c90-128">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="92c90-129">Zastąp wygenerowany kod poniżej:</span><span class="sxs-lookup"><span data-stu-id="92c90-129">Replace the generated code with the following:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="92c90-130">Jeśli w bazie danych są wszystkie filmy, zwraca inicjatora inicjatora i filmy nie zostaną dodane.</span><span class="sxs-lookup"><span data-stu-id="92c90-130">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="92c90-131">Dodaj inicjatora inicjatora</span><span class="sxs-lookup"><span data-stu-id="92c90-131">Add the seed initializer</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="92c90-132">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="92c90-132">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92c90-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92c90-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="92c90-134">Inicjator inicjatora, aby dodać `Main` metody w *Program.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="92c90-134">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

<span data-ttu-id="92c90-135">[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]</span><span class="sxs-lookup"><span data-stu-id="92c90-135">[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92c90-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92c90-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="92c90-137">Dodaj inicjatora inicjatora na końcu `Configure` metody w *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="92c90-137">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

<span data-ttu-id="92c90-138">[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]</span><span class="sxs-lookup"><span data-stu-id="92c90-138">[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]</span></span>

---
::: moniker-end

<span data-ttu-id="92c90-139">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="92c90-139">Test the app</span></span>

* <span data-ttu-id="92c90-140">Usuń wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="92c90-140">Delete all the records in the DB.</span></span> <span data-ttu-id="92c90-141">Można to zrobić z łączami Usuń w przeglądarce lub SSOX.</span><span class="sxs-lookup"><span data-stu-id="92c90-141">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="92c90-142">Wymusić na aplikacji, aby zainicjować (wywołanie metody w `Startup` klasy), uruchamia seed — metoda.</span><span class="sxs-lookup"><span data-stu-id="92c90-142">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="92c90-143">Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie.</span><span class="sxs-lookup"><span data-stu-id="92c90-143">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="92c90-144">Można to zrobić za pomocą dowolnego z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="92c90-144">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="92c90-145">Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie wybierz polecenie **zakończenia** lub **zatrzymania lokacji**</span><span class="sxs-lookup"><span data-stu-id="92c90-145">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Usługi IIS Express ikony paska zadań](working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="92c90-148">Podczas uruchamiania programu VS w trybie bez debugowania, naciśnij klawisz F5, aby uruchomić w trybie debugowania</span><span class="sxs-lookup"><span data-stu-id="92c90-148">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="92c90-149">W przypadku korzystania z wersji programu VS w trybie debugowania, zatrzymaniu debugera i naciśnij klawisz F5</span><span class="sxs-lookup"><span data-stu-id="92c90-149">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<span data-ttu-id="92c90-150">Aplikacja zawiera wprowadzonych danych.</span><span class="sxs-lookup"><span data-stu-id="92c90-150">The app shows the seeded data.</span></span>

![Aplikacja MVC Movie otworzyć w programie Microsoft Edge danych Film przedstawiający](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="92c90-152">[Poprzednie](adding-model.md)
> [dalej](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="92c90-152">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
