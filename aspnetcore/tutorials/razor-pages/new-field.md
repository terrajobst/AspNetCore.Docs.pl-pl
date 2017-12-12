---
title: Dodanie nowego pola do strony Razor
author: rick-anderson
description: "Pokazuje, jak dodać nowe pole do stron Razor z programu Entity Framework Core"
keywords: Migracje platformy ASP.NET Core Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 128b69513976a56104524bb803f2b8cb1daf1967
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="0ed3b-104">Dodanie nowego pola do strony Razor</span><span class="sxs-lookup"><span data-stu-id="0ed3b-104">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="0ed3b-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0ed3b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0ed3b-106">W tej sekcji użyjesz [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migracje Code First, aby dodać nowe pole do modelu i migracji, które zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-106">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="0ed3b-107">Gdy używasz EF Code First automatycznie utworzyć bazę danych, Code First dodaje tabeli do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-107">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="0ed3b-108">Jeśli nie są zsynchronizowane, EF zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-108">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="0ed3b-109">Dzięki temu można łatwiej znaleźć problemy niespójne bazy danych/kod.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-109">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="0ed3b-110">Dodawanie właściwości klasyfikacji do modelu film</span><span class="sxs-lookup"><span data-stu-id="0ed3b-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="0ed3b-111">Otwórz *Models/Movie.cs* plik i dodać `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="0ed3b-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="0ed3b-112">Tworzenie aplikacji (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="0ed3b-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="0ed3b-113">Edytuj *Pages/Movies/Index.cshtml*i Dodaj `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="0ed3b-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="0ed3b-114">Dodaj `Rating` pola do usunięcia i szczegóły stron.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="0ed3b-115">Aktualizacja *Create.cshtml* z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="0ed3b-116">Użytkownik może kopiowania/wklejania poprzedniej `<div>` element i umożliwiają pomoc intelliSense, zaktualizuj pola.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="0ed3b-117">IntelliSense współpracuje z [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="0ed3b-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Deweloper wpisał litera R dla wartości atrybutu asp — dla w drugi element label w widoku.](new-field/_static/cr.png)

<span data-ttu-id="0ed3b-121">Poniższy kod przedstawia *Create.cshtml* z `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="0ed3b-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="0ed3b-122">Dodaj `Rating` pole do edycji strony.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="0ed3b-123">Aplikacja nie będzie działać, dopóki bazy danych zostanie zaktualizowana, aby uwzględnić nowe pole.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="0ed3b-124">Jeśli teraz uruchomić zgłasza aplikacji `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="0ed3b-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="0ed3b-125">Ten błąd jest spowodowany przez zaktualizowane klasy modelu film jest inny niż schemat tabeli filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="0ed3b-126">(Brak nie `Rating` kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="0ed3b-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="0ed3b-127">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="0ed3b-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="0ed3b-128">Ma automatycznie Porzuć i ponownie utworzyć bazę danych przy użyciu nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="0ed3b-129">Ta metoda jest wygodne wczesnym etapie cyklu programowanie; pozwala na szybkie razem rozwijać schematu modelu i bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="0ed3b-130">Wadą podwyższonego jest, że utracić istniejące dane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="0ed3b-131">Nie chcesz użyć tej metody w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="0ed3b-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="0ed3b-132">Usunięcie bazy danych na zmiany schematu i automatycznie inicjatora bazy danych z danych testowych za pomocą inicjatora jest często produktywności możliwości opracowywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="0ed3b-133">Jawnie modyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="0ed3b-134">Zaletą tej metody jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="0ed3b-135">Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="0ed3b-136">Użyj migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="0ed3b-137">W tym samouczku Użyj migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="0ed3b-138">Aktualizacja `SeedData` klasy, dzięki czemu zapewnia wartość dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="0ed3b-139">Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie` bloku.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="0ed3b-140">Zobacz [ukończone pliku SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="0ed3b-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="0ed3b-141">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-141">Build the solution.</span></span>

<a name="pmc"></a><span data-ttu-id="0ed3b-142">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > konsoli Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-142">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="0ed3b-143">W kryterium wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="0ed3b-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="0ed3b-144">`Add-Migration` Polecenie informuje platformę, by:</span><span class="sxs-lookup"><span data-stu-id="0ed3b-144">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="0ed3b-145">Porównaj `Movie` modelu z `Movie` schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-145">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="0ed3b-146">Utwórz kod, aby przeprowadzić migrację schemat bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-146">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="0ed3b-147">Nazwa "Klasyfikacja" jest dowolnego i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-147">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="0ed3b-148">Warto użyć opisową nazwę pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-148">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a><span data-ttu-id="0ed3b-149">Jeśli usuniesz wszystkie rekordy w bazie danych, inicjator będzie inicjatora bazy danych i obejmują `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="0ed3b-150">Można to zrobić z łączami Usuń w przeglądarce lub z [Eksplorator obiektów Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="0ed3b-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="0ed3b-151">Aby usunąć bazę danych z SSOX:</span><span class="sxs-lookup"><span data-stu-id="0ed3b-151">To delete the database from SSOX:</span></span>

* <span data-ttu-id="0ed3b-152">Wybierz bazę danych w SSOX.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-152">Select the database in SSOX.</span></span>
* <span data-ttu-id="0ed3b-153">Kliknij prawym przyciskiem myszy w bazie danych, a następnie wybierz *usunąć*.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-153">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="0ed3b-154">Sprawdź **Zamknij istniejące połączenia**.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-154">Check **Close existing connections**.</span></span>
* <span data-ttu-id="0ed3b-155">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-155">Select **OK**.</span></span>
* <span data-ttu-id="0ed3b-156">W [PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="0ed3b-156">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="0ed3b-157">Uruchom aplikację i sprawdzić, można utworzyć/edycji/wyświetlania filmów `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-157">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="0ed3b-158">Jeśli nie jest inicjowana bazy danych, Zatrzymaj usługi IIS Express, a następnie uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="0ed3b-158">If the database is not seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0ed3b-159">[Poprzedni: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
[dalej: Dodawanie walidacji](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="0ed3b-159">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
