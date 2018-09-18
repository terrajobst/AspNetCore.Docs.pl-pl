---
title: Dodaj nowe pole na stronę Razor programu ASP.NET Core
author: rick-anderson
description: Pokazuje, jak dodać nowe pole do strony Razor za pomocą platformy Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d6d59ff336095e2f1b8b2e9a0338b7791605ad7a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010900"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="9750f-103">Dodaj nowe pole na stronę Razor programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9750f-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="9750f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9750f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9750f-105">W tej sekcji użyjesz [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migracje Code First dodaje nowe pole do modelu i migracji, które zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9750f-105">In this section you use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="9750f-106">Jeśli przy użyciu programu EF Code First automatycznie utworzyć bazę danych, Code First:</span><span class="sxs-lookup"><span data-stu-id="9750f-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="9750f-107">Dodanie tabeli do sprawdzenia, czy schemat bazy danych jest zsynchronizowany z klasy modelu, który został wygenerowany z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9750f-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="9750f-108">Jeśli klasy modelu nie są zsynchronizowane z bazy danych, EF zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="9750f-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="9750f-109">Automatyczne weryfikacji/model schematu synchronizacji ułatwia znajdowanie problemów z niespójne bazy danych/code.</span><span class="sxs-lookup"><span data-stu-id="9750f-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="9750f-110">Dodawanie właściwości klasyfikacji do modelu Movie</span><span class="sxs-lookup"><span data-stu-id="9750f-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="9750f-111">Otwórz *Models/Movie.cs* pliku i Dodaj `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="9750f-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

<span data-ttu-id="9750f-112">Utwórz aplikację (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="9750f-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="9750f-113">Edytuj *Pages/Movies/Index.cshtml*i Dodaj `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="9750f-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="9750f-114">Dodaj `Rating` pola strony Delete i szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="9750f-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="9750f-115">Aktualizacja *Create.cshtml* z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="9750f-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="9750f-116">Możesz skopiować lub wkleić poprzedniego `<div>` elementu, dzięki czemu pomoc intelliSense, zaktualizuj pola.</span><span class="sxs-lookup"><span data-stu-id="9750f-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="9750f-117">Technologia IntelliSense działa z [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="9750f-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Deweloper wpisał literę R wartość atrybutu asp — dla w elemencie drugiego etykiety widoku.](new-field/_static/cr.png)

<span data-ttu-id="9750f-121">Poniższy kod przedstawia *Create.cshtml* z `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="9750f-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="9750f-122">Dodaj `Rating` pole edytowanie strony.</span><span class="sxs-lookup"><span data-stu-id="9750f-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="9750f-123">Aplikacja nie będzie działać, dopóki baza danych została zaktualizowana do nowego pola.</span><span class="sxs-lookup"><span data-stu-id="9750f-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="9750f-124">Jeśli teraz uruchomić zgłasza aplikacji `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="9750f-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="9750f-125">Ten błąd jest spowodowany przez zaktualizowane klasy modelu film jest inny niż schemat tabeli filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9750f-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="9750f-126">(Brak nie `Rating` kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="9750f-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="9750f-127">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="9750f-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="9750f-128">Ma automatycznie Porzuć i ponownie utworzyć bazę danych przy użyciu nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9750f-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="9750f-129">To podejście jest wygodne na wczesnym etapie cyklu tworzenia oprogramowania; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9750f-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="9750f-130">Minusem jest to utraty istniejących danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9750f-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="9750f-131">Nie chcesz użyć tej metody w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="9750f-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="9750f-132">Usunięcie bazy danych na zmiany schematu i automatycznie inicjowanie bazy danych z danymi za pomocą inicjatora jest często produktywny sposób do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9750f-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="9750f-133">Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="9750f-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="9750f-134">Zaletą tego podejścia jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="9750f-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="9750f-135">Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.</span><span class="sxs-lookup"><span data-stu-id="9750f-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="9750f-136">Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="9750f-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="9750f-137">W tym samouczku należy użyć migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="9750f-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="9750f-138">Aktualizacja `SeedData` klasy tak, aby go oferuje wartości dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="9750f-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="9750f-139">Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie` bloku.</span><span class="sxs-lookup"><span data-stu-id="9750f-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9750f-140">Zobacz [ukończone pliku SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="9750f-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9750f-141">Zobacz [ukończone pliku SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="9750f-141">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>

::: moniker-end

<span data-ttu-id="9750f-142">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="9750f-142">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="9750f-143">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="9750f-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="9750f-144">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="9750f-144">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="9750f-145">`Add-Migration` Polecenie informuje platformę, by:</span><span class="sxs-lookup"><span data-stu-id="9750f-145">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="9750f-146">Porównaj `Movie` modelu przy użyciu `Movie` schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9750f-146">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="9750f-147">Utwórz kod, aby migrować schemat bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="9750f-147">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="9750f-148">Nazwa "Ocena" dowolnej i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="9750f-148">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="9750f-149">Warto użyć znaczącą nazwę pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="9750f-149">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="9750f-150">Jeśli usuniesz wszystkie rekordy w bazie danych, inicjatora będzie obsługiwał bazy danych i obejmują `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="9750f-150">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="9750f-151">Można to zrobić za pomocą łącza delete w przeglądarce, albo z [Eksplorator obiektów Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="9750f-151">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="9750f-152">Aby usunąć bazy danych z SSOX:</span><span class="sxs-lookup"><span data-stu-id="9750f-152">To delete the database from SSOX:</span></span>

* <span data-ttu-id="9750f-153">Wybierz bazę danych w SSOX.</span><span class="sxs-lookup"><span data-stu-id="9750f-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="9750f-154">Kliknij prawym przyciskiem myszy w bazie danych, a następnie wybierz pozycję *Usuń*.</span><span class="sxs-lookup"><span data-stu-id="9750f-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="9750f-155">Sprawdź **Zamknij istniejące połączenia**.</span><span class="sxs-lookup"><span data-stu-id="9750f-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="9750f-156">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="9750f-156">Select **OK**.</span></span>
* <span data-ttu-id="9750f-157">W [PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizują bazę danych:</span><span class="sxs-lookup"><span data-stu-id="9750f-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="9750f-158">Uruchom aplikację i sprawdź, można tworzenia/edycji/wyświetlania filmów z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="9750f-158">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="9750f-159">Jeśli baza danych nie jest obsługiwany, Zatrzymaj usługi IIS Express, a następnie uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="9750f-159">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9750f-160">[Poprzedni: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
> [dalej: Dodawanie walidacji](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="9750f-160">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
