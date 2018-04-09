---
title: Dodaj nowe pole do Razor strony platformy ASP.NET Core
author: rick-anderson
description: Pokazuje, jak dodać nowe pole do stron Razor z programu Entity Framework Core
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 2652aea2bc587074da5101e3de2bdf55d032214a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="d1354-103">Dodaj nowe pole do Razor strony platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d1354-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="d1354-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d1354-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d1354-105">W tej sekcji użyjesz [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migracje Code First, aby dodać nowe pole do modelu i migracji, które zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d1354-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="d1354-106">Gdy używasz EF Code First automatycznie utworzyć bazę danych, Code First dodaje tabeli do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="d1354-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="d1354-107">Jeśli nie są zsynchronizowane, EF zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="d1354-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="d1354-108">Dzięki temu można łatwiej znaleźć problemy niespójne bazy danych/kod.</span><span class="sxs-lookup"><span data-stu-id="d1354-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="d1354-109">Dodawanie właściwości klasyfikacji do modelu film</span><span class="sxs-lookup"><span data-stu-id="d1354-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="d1354-110">Otwórz *Models/Movie.cs* plik i dodać `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="d1354-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="d1354-111">Tworzenie aplikacji (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="d1354-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="d1354-112">Edytuj *Pages/Movies/Index.cshtml*i Dodaj `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="d1354-112">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="d1354-113">Dodaj `Rating` pola do usunięcia i szczegóły stron.</span><span class="sxs-lookup"><span data-stu-id="d1354-113">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="d1354-114">Aktualizacja *Create.cshtml* z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="d1354-114">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="d1354-115">Użytkownik może kopiowania/wklejania poprzedniej `<div>` element i umożliwiają pomoc intelliSense, zaktualizuj pola.</span><span class="sxs-lookup"><span data-stu-id="d1354-115">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="d1354-116">IntelliSense współpracuje z [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="d1354-116">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Deweloper wpisał litera R dla wartości atrybutu asp — dla w drugi element label w widoku.](new-field/_static/cr.png)

<span data-ttu-id="d1354-120">Poniższy kod przedstawia *Create.cshtml* z `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="d1354-120">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="d1354-121">Dodaj `Rating` pole do edycji strony.</span><span class="sxs-lookup"><span data-stu-id="d1354-121">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="d1354-122">Aplikacja nie będzie działać, dopóki bazy danych zostanie zaktualizowana, aby uwzględnić nowe pole.</span><span class="sxs-lookup"><span data-stu-id="d1354-122">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="d1354-123">Jeśli teraz uruchomić zgłasza aplikacji `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="d1354-123">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="d1354-124">Ten błąd jest spowodowany przez zaktualizowane klasy modelu film jest inny niż schemat tabeli filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d1354-124">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="d1354-125">(Brak nie `Rating` kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="d1354-125">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="d1354-126">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="d1354-126">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="d1354-127">Ma automatycznie Porzuć i ponownie utworzyć bazę danych przy użyciu nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d1354-127">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="d1354-128">Ta metoda jest wygodne wczesnym etapie cyklu programowanie; pozwala na szybkie razem rozwijać schematu modelu i bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d1354-128">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="d1354-129">Wadą podwyższonego jest, że utracić istniejące dane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d1354-129">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="d1354-130">Nie chcesz użyć tej metody w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="d1354-130">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="d1354-131">Usunięcie bazy danych na zmiany schematu i automatycznie inicjatora bazy danych z danych testowych za pomocą inicjatora jest często produktywności możliwości opracowywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d1354-131">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="d1354-132">Jawnie modyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="d1354-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="d1354-133">Zaletą tej metody jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="d1354-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="d1354-134">Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.</span><span class="sxs-lookup"><span data-stu-id="d1354-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="d1354-135">Użyj migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d1354-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="d1354-136">W tym samouczku Użyj migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="d1354-136">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="d1354-137">Aktualizacja `SeedData` klasy, dzięki czemu zapewnia wartość dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="d1354-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="d1354-138">Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie` bloku.</span><span class="sxs-lookup"><span data-stu-id="d1354-138">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="d1354-139">Zobacz [ukończone pliku SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="d1354-139">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="d1354-140">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="d1354-140">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="d1354-141">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > konsoli Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="d1354-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="d1354-142">W kryterium wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d1354-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="d1354-143">`Add-Migration` Polecenie informuje platformę, by:</span><span class="sxs-lookup"><span data-stu-id="d1354-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="d1354-144">Porównaj `Movie` modelu z `Movie` schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d1354-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="d1354-145">Utwórz kod, aby przeprowadzić migrację schemat bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="d1354-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="d1354-146">Nazwa "Klasyfikacja" jest dowolnego i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="d1354-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="d1354-147">Warto użyć opisową nazwę pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="d1354-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="d1354-148">Jeśli usuniesz wszystkie rekordy w bazie danych, inicjator będzie inicjatora bazy danych i obejmują `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="d1354-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="d1354-149">Można to zrobić z łączami Usuń w przeglądarce lub z [Eksplorator obiektów Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="d1354-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="d1354-150">Aby usunąć bazę danych z SSOX:</span><span class="sxs-lookup"><span data-stu-id="d1354-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="d1354-151">Wybierz bazę danych w SSOX.</span><span class="sxs-lookup"><span data-stu-id="d1354-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="d1354-152">Kliknij prawym przyciskiem myszy w bazie danych, a następnie wybierz *usunąć*.</span><span class="sxs-lookup"><span data-stu-id="d1354-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="d1354-153">Sprawdź **Zamknij istniejące połączenia**.</span><span class="sxs-lookup"><span data-stu-id="d1354-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="d1354-154">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="d1354-154">Select **OK**.</span></span>
* <span data-ttu-id="d1354-155">W [PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="d1354-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="d1354-156">Uruchom aplikację i sprawdzić, można utworzyć/edycji/wyświetlania filmów `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="d1354-156">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="d1354-157">Jeśli bazy danych nie jest obsługiwany, Zatrzymaj usługi IIS Express, a następnie uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d1354-157">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d1354-158">[Poprzedni: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
> [dalej: Dodawanie walidacji](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="d1354-158">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
