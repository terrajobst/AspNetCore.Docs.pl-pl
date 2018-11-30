---
title: Praca z programu SQL Server LocalDB w aplikacji MVC platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej o korzystaniu z programu SQL Server LocalDB w prostej aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 49615c25d51cfa671157c2e56b8e0753719c678a
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710104"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a><span data-ttu-id="7ec12-103">Praca z bazą danych LocalDB programu SQL Server w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ec12-103">Work with SQL Server LocalDB in ASP.NET Core</span></span>

<span data-ttu-id="7ec12-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7ec12-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7ec12-105">`MvcMovieContext` Obiektu obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7ec12-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="7ec12-106">Kontekst bazy danych jest zarejestrowany w [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="7ec12-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

::: moniker-end

<span data-ttu-id="7ec12-107">ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty system `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="7ec12-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="7ec12-108">Dla wdrożenia lokalnego, pobiera parametry połączenia z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="7ec12-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="7ec12-109">Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym, można użyć zmiennej środowiskowej lub innej metody, które można ustawić parametrów połączenia do rzeczywistego programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7ec12-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="7ec12-110">Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="7ec12-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="7ec12-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="7ec12-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="7ec12-112">LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych, która jest przeznaczona do tworzenia programu.</span><span class="sxs-lookup"><span data-stu-id="7ec12-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="7ec12-113">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7ec12-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="7ec12-114">Domyślnie tworzy bazy danych LocalDB "\*.mdf" pliki *C:/Users/\<użytkownika\>*  katalogu.</span><span class="sxs-lookup"><span data-stu-id="7ec12-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="7ec12-115">Z **widoku** menu Otwórz **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="7ec12-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu Widok](working-with-sql/_static/ssox.png)

* <span data-ttu-id="7ec12-117">Kliknij prawym przyciskiem myszy `Movie` tabeli **> Projektant widoków**</span><span class="sxs-lookup"><span data-stu-id="7ec12-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmu](working-with-sql/_static/design.png)

  ![Otwórz w Projektancie tabel filmu](working-with-sql/_static/dv.png)

<span data-ttu-id="7ec12-120">Należy zauważyć ikonę klucza, obok `ID`.</span><span class="sxs-lookup"><span data-stu-id="7ec12-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="7ec12-121">Domyślnie program EF spowoduje, że właściwość o nazwie `ID` klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="7ec12-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="7ec12-122">Kliknij prawym przyciskiem myszy `Movie` tabeli **> Dane widoku**</span><span class="sxs-lookup"><span data-stu-id="7ec12-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu kontekstowe jest otwarte w tabeli filmu](working-with-sql/_static/ssox2.png)

  ![Tabela filmu otwarte, wyświetlanie tabeli danych](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="7ec12-125">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="7ec12-125">Seed the database</span></span>

<span data-ttu-id="7ec12-126">Utwórz nową klasę o nazwie `SeedData` w *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="7ec12-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="7ec12-127">Zastąp wygenerowany kod następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="7ec12-127">Replace the generated code with the following:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="7ec12-128">Czy istnieją wszystkie filmy w bazie danych, zwraca inicjatora inicjatora i filmy nie zostaną dodane.</span><span class="sxs-lookup"><span data-stu-id="7ec12-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="7ec12-129">Dodaj inicjator inicjatora</span><span class="sxs-lookup"><span data-stu-id="7ec12-129">Add the seed initializer</span></span>

<span data-ttu-id="7ec12-130">Zastąp zawartość *Program.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7ec12-130">Replace the contents of *Program.cs* with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="7ec12-131">Dodaj inicjator inicjator, tak aby `Main` method in Class metoda *Program.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="7ec12-131">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

::: moniker-end

<span data-ttu-id="7ec12-132">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7ec12-132">Test the app</span></span>

* <span data-ttu-id="7ec12-133">Usuń wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7ec12-133">Delete all the records in the DB.</span></span> <span data-ttu-id="7ec12-134">Można to zrobić, wraz z łączami delete w przeglądarce lub SSOX.</span><span class="sxs-lookup"><span data-stu-id="7ec12-134">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="7ec12-135">Wymusić na aplikacji, aby zainicjować (wywoływanie metody w `Startup` klasy), uruchamia seed — metoda.</span><span class="sxs-lookup"><span data-stu-id="7ec12-135">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="7ec12-136">Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie.</span><span class="sxs-lookup"><span data-stu-id="7ec12-136">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="7ec12-137">To zrobić przy użyciu dowolnej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="7ec12-137">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="7ec12-138">Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie naciśnij pozycję **zakończenia** lub **zatrzymanie witryny**</span><span class="sxs-lookup"><span data-stu-id="7ec12-138">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Usługi IIS Express ikony na pasku zadań](working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="7ec12-141">Podczas uruchamiania w trybie bez debugowania programu VS, naciśnij klawisz F5, aby uruchomić tryb debugowania</span><span class="sxs-lookup"><span data-stu-id="7ec12-141">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="7ec12-142">W przypadku uruchomienia programu VS w trybie debugowania, Zatrzymaj debuger, a następnie naciśnij klawisz F5</span><span class="sxs-lookup"><span data-stu-id="7ec12-142">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<span data-ttu-id="7ec12-143">Aplikacja zawiera wprowadzonych danych.</span><span class="sxs-lookup"><span data-stu-id="7ec12-143">The app shows the seeded data.</span></span>

![Aplikacja filmu MVC otworzyć w programie Microsoft Edge wyświetlanie danych filmu](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="7ec12-145">[Poprzednie](adding-model.md)
> [dalej](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="7ec12-145">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
