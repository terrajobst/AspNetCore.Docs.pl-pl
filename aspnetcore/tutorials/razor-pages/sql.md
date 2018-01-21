---
title: Praca z bazy danych LocalDB programu SQL Server i platformy ASP.NET Core
author: rick-anderson
description: "W tym artykule wyjaśniono pracy z bazy danych LocalDB programu SQL Server i ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 90aa194eda1c52afb1f299a0b95c7040e32a02fc
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="77f2c-103">Praca z bazy danych LocalDB programu SQL Server i platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77f2c-103">Working with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="77f2c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="77f2c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="77f2c-105">`MovieContext` Obiektu obsługuje zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="77f2c-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="77f2c-106">Kontekst bazy danych został zarejestrowany za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="77f2c-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

<span data-ttu-id="77f2c-107">Platformy ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty systemu `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="77f2c-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="77f2c-108">Lokalne działania projektowe, pobiera parametry połączenia z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="77f2c-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="77f2c-109">Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym, można użyć zmiennej środowiskowej lub innej metody, aby ustawić parametry połączenia do rzeczywistego programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="77f2c-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="77f2c-110">Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="77f2c-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="77f2c-111">SQL Server Express LocalDB.</span><span class="sxs-lookup"><span data-stu-id="77f2c-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="77f2c-112">To Zubożona wersja programu SQL Server Express aparat bazy danych jest przeznaczony do tworzenia aplikacji programu.</span><span class="sxs-lookup"><span data-stu-id="77f2c-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="77f2c-113">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="77f2c-113">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="77f2c-114">Domyślnie tworzy bazy danych LocalDB "\*.mdf" plików *C:/Users/\<użytkownika\>*  katalogu.</span><span class="sxs-lookup"><span data-stu-id="77f2c-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="77f2c-115">Z **widoku** menu, otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="77f2c-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](sql/_static/ssox.png)

* <span data-ttu-id="77f2c-117">Kliknij prawym przyciskiem myszy `Movie` tabeli i wybierz **Widok projektanta**:</span><span class="sxs-lookup"><span data-stu-id="77f2c-117">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu kontekstowe otwarty na tabeli film](sql/_static/design.png)

  ![Otwórz w projektancie tabeli film](sql/_static/dv.png)

<span data-ttu-id="77f2c-120">Należy pamiętać ikonę klucza, obok pozycji `ID`.</span><span class="sxs-lookup"><span data-stu-id="77f2c-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="77f2c-121">Domyślnie EF tworzy właściwość o nazwie `ID` klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="77f2c-121">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="77f2c-122">Kliknij prawym przyciskiem myszy `Movie` tabeli i wybierz **danych widoku**:</span><span class="sxs-lookup"><span data-stu-id="77f2c-122">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Otwórz tabelę dane są wyświetlane tabeli film](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="77f2c-124">Inicjatora bazy danych</span><span class="sxs-lookup"><span data-stu-id="77f2c-124">Seed the database</span></span>

<span data-ttu-id="77f2c-125">Utwórz nową klasę o nazwie `SeedData` w *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="77f2c-125">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="77f2c-126">Zastąp wygenerowany kod poniżej:</span><span class="sxs-lookup"><span data-stu-id="77f2c-126">Replace the generated code with the following:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="77f2c-127">Jeśli w bazie danych są wszystkie filmy, zwraca inicjatora inicjatora i filmy nie zostaną dodane.</span><span class="sxs-lookup"><span data-stu-id="77f2c-127">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="77f2c-128">Dodaj inicjatora inicjatora</span><span class="sxs-lookup"><span data-stu-id="77f2c-128">Add the seed initializer</span></span>

<span data-ttu-id="77f2c-129">Dodaj inicjatora inicjatora na końcu `Main` metody w *Program.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="77f2c-129">Add the seed initializer to the end of the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

<span data-ttu-id="77f2c-130">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="77f2c-130">Test the app</span></span>

* <span data-ttu-id="77f2c-131">Usuń wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="77f2c-131">Delete all the records in the DB.</span></span> <span data-ttu-id="77f2c-132">Można to zrobić z łączami Usuń w przeglądarce lub z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="77f2c-132">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="77f2c-133">Wymusić na aplikacji, aby zainicjować (wywołanie metody w `Startup` klasy), uruchamia seed — metoda.</span><span class="sxs-lookup"><span data-stu-id="77f2c-133">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="77f2c-134">Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie.</span><span class="sxs-lookup"><span data-stu-id="77f2c-134">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="77f2c-135">Można to zrobić za pomocą dowolnego z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="77f2c-135">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="77f2c-136">Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie wybierz polecenie **zakończenia** lub **Zatrzymaj witrynę**:</span><span class="sxs-lookup"><span data-stu-id="77f2c-136">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Usługi IIS Express ikony paska zadań](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

   * <span data-ttu-id="77f2c-139">Podczas uruchamiania programu VS w trybie bez debugowania, naciśnij klawisz F5, aby uruchomić w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="77f2c-139">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
   * <span data-ttu-id="77f2c-140">W przypadku korzystania z wersji programu VS w trybie debugowania, zatrzymaniu debugera i naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="77f2c-140">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="77f2c-141">Aplikacja zawiera wprowadzonych danych:</span><span class="sxs-lookup"><span data-stu-id="77f2c-141">The app shows the seeded data:</span></span>

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji film](sql/_static/m55.png)

<span data-ttu-id="77f2c-143">Następny samouczek spowoduje oczyszczenie prezentacji danych.</span><span class="sxs-lookup"><span data-stu-id="77f2c-143">The next tutorial will clean up the presentation of the data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="77f2c-144">[Poprzedni: Tworzenie szkieletu stron Razor](xref:tutorials/razor-pages/page)
[dalej: aktualizowanie stron](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="77f2c-144">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
