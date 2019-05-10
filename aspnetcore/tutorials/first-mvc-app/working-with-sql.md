---
title: Praca z SQL w aplikacji MVC platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej o korzystaniu z programu SQL Server LocalDB lub bazy danych SQLite w aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 983742276f3519b540cd62e4ada6eb5189650aa8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899230"
---
# <a name="work-with-sql-in-aspnet-core"></a><span data-ttu-id="e2d9d-103">Praca z SQL w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2d9d-103">Work with SQL in ASP.NET Core</span></span>

<span data-ttu-id="e2d9d-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e2d9d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e2d9d-105">`MvcMovieContext` Obiektu obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e2d9d-106">Kontekst bazy danych jest zarejestrowany w [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="e2d9d-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2d9d-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2d9d-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="e2d9d-108">ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty system `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="e2d9d-109">Dla wdrożenia lokalnego, pobiera parametry połączenia z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="e2d9d-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e2d9d-110">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e2d9d-110">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="e2d9d-111">ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty system `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-111">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="e2d9d-112">Dla wdrożenia lokalnego, pobiera parametry połączenia z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="e2d9d-112">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

<span data-ttu-id="e2d9d-113">Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym, można użyć zmiennej środowiskowej lub innej metody, które można ustawić parametrów połączenia do rzeczywistego programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-113">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="e2d9d-114">Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-114">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2d9d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2d9d-115">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="e2d9d-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="e2d9d-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e2d9d-117">LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych, która jest przeznaczona do tworzenia programu.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="e2d9d-118">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="e2d9d-119">Domyślnie tworzy bazy danych LocalDB *.mdf* pliki *C:/Users / {user}* katalogu.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-119">By default, LocalDB database creates *.mdf* files in the *C:/Users/{user}* directory.</span></span>

* <span data-ttu-id="e2d9d-120">Z **widoku** menu Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="e2d9d-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](working-with-sql/_static/ssox.png)

* <span data-ttu-id="e2d9d-122">Kliknij prawym przyciskiem myszy `Movie` tabeli **> Projektant widoków**</span><span class="sxs-lookup"><span data-stu-id="e2d9d-122">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmu](working-with-sql/_static/design.png)

  ![Otwórz w Projektancie tabel filmu](working-with-sql/_static/dv.png)

<span data-ttu-id="e2d9d-125">Należy zauważyć ikonę klucza, obok `ID`.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="e2d9d-126">Domyślnie program EF spowoduje, że właściwość o nazwie `ID` klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-126">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="e2d9d-127">Kliknij prawym przyciskiem myszy `Movie` tabeli **> Dane widoku**</span><span class="sxs-lookup"><span data-stu-id="e2d9d-127">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmu](working-with-sql/_static/ssox2.png)

  ![Tabela filmu otwarte, wyświetlanie tabeli danych](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e2d9d-130">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e2d9d-130">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="e2d9d-131">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="e2d9d-131">Seed the database</span></span>

<span data-ttu-id="e2d9d-132">Utwórz nową klasę o nazwie `SeedData` w *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="e2d9d-133">Zastąp wygenerowany kod następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="e2d9d-133">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="e2d9d-134">Czy istnieją wszystkie filmy w bazie danych, zwraca inicjatora inicjatora i filmy nie zostaną dodane.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="e2d9d-135">Dodaj inicjator inicjatora</span><span class="sxs-lookup"><span data-stu-id="e2d9d-135">Add the seed initializer</span></span>

<span data-ttu-id="e2d9d-136">Zastąp zawartość *Program.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="e2d9d-136">Replace the contents of *Program.cs* with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

<span data-ttu-id="e2d9d-137">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e2d9d-137">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2d9d-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2d9d-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2d9d-139">Usuń wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-139">Delete all the records in the DB.</span></span> <span data-ttu-id="e2d9d-140">Można to zrobić, wraz z łączami delete w przeglądarce lub SSOX.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-140">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="e2d9d-141">Wymusić na aplikacji, aby zainicjować (wywoływanie metody w `Startup` klasy), uruchamia seed — metoda.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-141">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="e2d9d-142">Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-142">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="e2d9d-143">To zrobić przy użyciu dowolnej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="e2d9d-143">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="e2d9d-144">Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie naciśnij pozycję **zakończenia** lub **zatrzymanie witryny**</span><span class="sxs-lookup"><span data-stu-id="e2d9d-144">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Usługi IIS Express ikony na pasku zadań](working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="e2d9d-147">Podczas uruchamiania w trybie bez debugowania programu VS, naciśnij klawisz F5, aby uruchomić tryb debugowania</span><span class="sxs-lookup"><span data-stu-id="e2d9d-147">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="e2d9d-148">W przypadku uruchomienia programu VS w trybie debugowania, Zatrzymaj debuger, a następnie naciśnij klawisz F5</span><span class="sxs-lookup"><span data-stu-id="e2d9d-148">If you were running VS in debug mode, stop the debugger and press F5</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e2d9d-149">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e2d9d-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e2d9d-150">(Aby uruchomi seed — metoda), należy usunąć wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-150">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="e2d9d-151">Zatrzymywanie i uruchamianie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-151">Stop and start the app to seed the database.</span></span>

---

<span data-ttu-id="e2d9d-152">Aplikacja zawiera wprowadzonych danych.</span><span class="sxs-lookup"><span data-stu-id="e2d9d-152">The app shows the seeded data.</span></span>

![Aplikacja filmu MVC otworzyć w programie Microsoft Edge wyświetlanie danych filmu](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="e2d9d-154">[Poprzednie](adding-model.md)
> [dalej](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="e2d9d-154">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>
