---
title: Dodaj nowe pole do strony Razor w ASP.NET Core
author: rick-anderson
description: Pokazuje, jak dodać nowe pole do strony Razor przy użyciu Entity Framework Core
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d34b938dbd1b512ddb167cac0c035837889cd38f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657816"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="145c2-103">Dodaj nowe pole do strony Razor w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="145c2-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="145c2-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="145c2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="145c2-105">W tej sekcji [Entity Framework](/ef/core/get-started/aspnetcore/new-db) migracje Code First służy do:</span><span class="sxs-lookup"><span data-stu-id="145c2-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="145c2-106">Dodaj nowe pole do modelu.</span><span class="sxs-lookup"><span data-stu-id="145c2-106">Add a new field to the model.</span></span>
* <span data-ttu-id="145c2-107">Migruj nową zmianę schematu pola do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="145c2-108">W przypadku automatycznego tworzenia bazy danych przy użyciu narzędzia EF Code First Code First:</span><span class="sxs-lookup"><span data-stu-id="145c2-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="145c2-109">Dodaje tabelę `__EFMigrationsHistory` do bazy danych w celu śledzenia, czy schemat bazy danych jest zsynchronizowany z klasami modelu, z których została wygenerowana.</span><span class="sxs-lookup"><span data-stu-id="145c2-109">Adds an `__EFMigrationsHistory` table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="145c2-110">Jeśli klasy modelu nie są zsynchronizowane z bazą danych, EF zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="145c2-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="145c2-111">Automatyczna weryfikacja schematu/modelu w ramach synchronizacji ułatwia znalezienie niespójnych problemów z bazą danych i kodem.</span><span class="sxs-lookup"><span data-stu-id="145c2-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="145c2-112">Dodawanie właściwości oceny do modelu filmu</span><span class="sxs-lookup"><span data-stu-id="145c2-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="145c2-113">Otwórz plik *models/Movie. cs* i Dodaj `Rating` Właściwość:</span><span class="sxs-lookup"><span data-stu-id="145c2-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="145c2-114">Skompiluj aplikację.</span><span class="sxs-lookup"><span data-stu-id="145c2-114">Build the app.</span></span>

<span data-ttu-id="145c2-115">Edycja *stron/filmów/index. cshtml*i dodawanie pola `Rating`:</span><span class="sxs-lookup"><span data-stu-id="145c2-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

<a name="addrat"></a>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexRating.cshtml?highlight=40-42,62-64)]

<span data-ttu-id="145c2-116">Aktualizowanie następujących stron:</span><span class="sxs-lookup"><span data-stu-id="145c2-116">Update the following pages:</span></span>

* <span data-ttu-id="145c2-117">Dodaj pole `Rating` do stron usuwanie i szczegóły.</span><span class="sxs-lookup"><span data-stu-id="145c2-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="145c2-118">Zaktualizuj element [Create. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml) przy użyciu pola `Rating`.</span><span class="sxs-lookup"><span data-stu-id="145c2-118">Update [Create.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="145c2-119">Dodaj pole `Rating` do strony Edycja.</span><span class="sxs-lookup"><span data-stu-id="145c2-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="145c2-120">Aplikacja nie będzie działała, dopóki baza danych nie zostanie zaktualizowana w celu uwzględnienia nowego pola.</span><span class="sxs-lookup"><span data-stu-id="145c2-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="145c2-121">Uruchomienie aplikacji bez aktualizowania bazy danych zgłasza `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="145c2-121">Running the app without updating the database throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="145c2-122">Wyjątek `SqlException` jest spowodowany przez zaktualizowaną klasę filmu inną niż schemat tabeli filmów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-122">The `SqlException` exception is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="145c2-123">(Brak kolumny `Rating` w tabeli bazy danych).</span><span class="sxs-lookup"><span data-stu-id="145c2-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="145c2-124">Istnieje kilka metod rozpoznawania błędu:</span><span class="sxs-lookup"><span data-stu-id="145c2-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="145c2-125">Entity Framework automatycznie porzucić i ponownie utworzyć bazę danych przy użyciu nowego schematu klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="145c2-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="145c2-126">Takie podejście jest wygodnie wczesne w cyklu rozwoju; pozwala ona szybko rozwijać model i schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="145c2-127">Minusem polega na utracie istniejących danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="145c2-128">Nie używaj tego podejścia w produkcyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="145c2-129">Porzucenie bazy danych w ramach zmian schematu i użycie inicjatora do automatycznego wypełniania bazy danych za pomocą danych testowych jest często wydajnym sposobem na tworzenie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="145c2-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="145c2-130">Jawnie zmodyfikuj schemat istniejącej bazy danych, tak aby pasował do klas modelu.</span><span class="sxs-lookup"><span data-stu-id="145c2-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="145c2-131">Zaletą tego podejścia jest utrzymywanie danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="145c2-132">Tę zmianę można wprowadzić ręcznie lub przez utworzenie skryptu zmiany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="145c2-133">Użyj Migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="145c2-134">Na potrzeby tego samouczka Użyj Migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="145c2-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="145c2-135">Zaktualizuj klasę `SeedData`, aby zapewnić wartość nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="145c2-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="145c2-136">Poniżej przedstawiono przykładową zmianę, ale trzeba wprowadzić tę zmianę dla każdego bloku `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="145c2-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="145c2-137">Zobacz [ukończony plik SeedData.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="145c2-137">See the [completed SeedData.cs file](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="145c2-138">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="145c2-138">Build the solution.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="145c2-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="145c2-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="145c2-140">Dodawanie migracji dla pola oceny</span><span class="sxs-lookup"><span data-stu-id="145c2-140">Add a migration for the rating field</span></span>

<span data-ttu-id="145c2-141">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet > konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="145c2-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="145c2-142">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="145c2-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="145c2-143">`Add-Migration` polecenie instruuje platformę, aby:</span><span class="sxs-lookup"><span data-stu-id="145c2-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="145c2-144">Porównaj model `Movie` ze schematem `Movie` DB.</span><span class="sxs-lookup"><span data-stu-id="145c2-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="145c2-145">Utwórz kod, aby zmigrować schemat bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="145c2-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="145c2-146">Nazwa "Rating" jest arbitralna i jest używana do nazwy pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="145c2-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="145c2-147">Warto użyć zrozumiałej nazwy dla pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="145c2-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="145c2-148">Polecenie `Update-Database` informuje platformę, aby zastosować zmiany schematu do bazy danych i zachować istniejące dane.</span><span class="sxs-lookup"><span data-stu-id="145c2-148">The `Update-Database` command tells the framework to apply the schema changes to the database and to preserve existing data.</span></span>

<a name="ssox"></a>

<span data-ttu-id="145c2-149">W przypadku usunięcia wszystkich rekordów w bazie danych inicjator będzie wypełniać bazę danych i zawierać pole `Rating`.</span><span class="sxs-lookup"><span data-stu-id="145c2-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="145c2-150">Można to zrobić za pomocą linków usuwania w przeglądarce lub z [programu SQL Server Eksplorator obiektów](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="145c2-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="145c2-151">Innym rozwiązaniem jest usunięcie bazy danych i użycie migracji w celu ponownego utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="145c2-152">Aby usunąć bazę danych w programie SSOX:</span><span class="sxs-lookup"><span data-stu-id="145c2-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="145c2-153">Wybierz bazę danych w SSOX.</span><span class="sxs-lookup"><span data-stu-id="145c2-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="145c2-154">Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję *Usuń*.</span><span class="sxs-lookup"><span data-stu-id="145c2-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="145c2-155">Zaznacz pole wyboru **Zamknij istniejące połączenia**.</span><span class="sxs-lookup"><span data-stu-id="145c2-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="145c2-156">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="145c2-156">Select **OK**.</span></span>
* <span data-ttu-id="145c2-157">W obszarze [PMC](xref:tutorials/razor-pages/new-field#pmc)zaktualizuj bazę danych:</span><span class="sxs-lookup"><span data-stu-id="145c2-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="145c2-158">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="145c2-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="145c2-159">Porzuć i ponownie utwórz bazę danych</span><span class="sxs-lookup"><span data-stu-id="145c2-159">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="145c2-160">Usuń folder migracji.</span><span class="sxs-lookup"><span data-stu-id="145c2-160">Delete the migration folder.</span></span>  <span data-ttu-id="145c2-161">Użyj następujących poleceń, aby ponownie utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-161">Use the following commands to recreate the database.</span></span>

```dotnetcli
dotnet ef database drop
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

<span data-ttu-id="145c2-162">Uruchom aplikację i sprawdź, czy można tworzyć/edytować/wyświetlać filmy z polem `Rating`.</span><span class="sxs-lookup"><span data-stu-id="145c2-162">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="145c2-163">Jeśli baza danych nie jest zainicjowana, ustaw punkt przerwania w metodzie `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="145c2-163">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="145c2-164">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="145c2-164">Additional resources</span></span>

* [<span data-ttu-id="145c2-165">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="145c2-165">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="145c2-166">[Poprzedni: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
> [Dalej: Dodawanie walidacji](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="145c2-166">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="145c2-167">W tej sekcji [Entity Framework](/ef/core/get-started/aspnetcore/new-db) migracje Code First służy do:</span><span class="sxs-lookup"><span data-stu-id="145c2-167">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="145c2-168">Dodaj nowe pole do modelu.</span><span class="sxs-lookup"><span data-stu-id="145c2-168">Add a new field to the model.</span></span>
* <span data-ttu-id="145c2-169">Migruj nową zmianę schematu pola do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-169">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="145c2-170">W przypadku automatycznego tworzenia bazy danych przy użyciu narzędzia EF Code First Code First:</span><span class="sxs-lookup"><span data-stu-id="145c2-170">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="145c2-171">Dodaje tabelę do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowany z klasami modelu, z których została wygenerowana.</span><span class="sxs-lookup"><span data-stu-id="145c2-171">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="145c2-172">Jeśli klasy modelu nie są zsynchronizowane z bazą danych, EF zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="145c2-172">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="145c2-173">Automatyczna weryfikacja schematu/modelu w ramach synchronizacji ułatwia znalezienie niespójnych problemów z bazą danych i kodem.</span><span class="sxs-lookup"><span data-stu-id="145c2-173">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="145c2-174">Dodawanie właściwości oceny do modelu filmu</span><span class="sxs-lookup"><span data-stu-id="145c2-174">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="145c2-175">Otwórz plik *models/Movie. cs* i Dodaj `Rating` Właściwość:</span><span class="sxs-lookup"><span data-stu-id="145c2-175">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="145c2-176">Skompiluj aplikację.</span><span class="sxs-lookup"><span data-stu-id="145c2-176">Build the app.</span></span>

<span data-ttu-id="145c2-177">Edycja *stron/filmów/index. cshtml*i dodawanie pola `Rating`:</span><span class="sxs-lookup"><span data-stu-id="145c2-177">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="145c2-178">Aktualizowanie następujących stron:</span><span class="sxs-lookup"><span data-stu-id="145c2-178">Update the following pages:</span></span>

* <span data-ttu-id="145c2-179">Dodaj pole `Rating` do stron usuwanie i szczegóły.</span><span class="sxs-lookup"><span data-stu-id="145c2-179">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="145c2-180">Zaktualizuj element [Create. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) przy użyciu pola `Rating`.</span><span class="sxs-lookup"><span data-stu-id="145c2-180">Update [Create.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="145c2-181">Dodaj pole `Rating` do strony Edycja.</span><span class="sxs-lookup"><span data-stu-id="145c2-181">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="145c2-182">Aplikacja nie będzie działała, dopóki baza danych nie zostanie zaktualizowana w celu uwzględnienia nowego pola.</span><span class="sxs-lookup"><span data-stu-id="145c2-182">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="145c2-183">Jeśli uruchomisz teraz, aplikacja zgłosi `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="145c2-183">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="145c2-184">Ten błąd jest spowodowany przez zaktualizowaną klasę filmu inną niż schemat tabeli filmów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-184">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="145c2-185">(Brak kolumny `Rating` w tabeli bazy danych).</span><span class="sxs-lookup"><span data-stu-id="145c2-185">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="145c2-186">Istnieje kilka metod rozpoznawania błędu:</span><span class="sxs-lookup"><span data-stu-id="145c2-186">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="145c2-187">Entity Framework automatycznie porzucić i ponownie utworzyć bazę danych przy użyciu nowego schematu klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="145c2-187">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="145c2-188">Takie podejście jest wygodnie wczesne w cyklu rozwoju; pozwala ona szybko rozwijać model i schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-188">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="145c2-189">Minusem polega na utracie istniejących danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-189">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="145c2-190">Nie używaj tego podejścia w produkcyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-190">Don't use this approach on a production database!</span></span> <span data-ttu-id="145c2-191">Porzucenie bazy danych w ramach zmian schematu i użycie inicjatora do automatycznego wypełniania bazy danych za pomocą danych testowych jest często wydajnym sposobem na tworzenie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="145c2-191">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="145c2-192">Jawnie zmodyfikuj schemat istniejącej bazy danych, tak aby pasował do klas modelu.</span><span class="sxs-lookup"><span data-stu-id="145c2-192">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="145c2-193">Zaletą tego podejścia jest utrzymywanie danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-193">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="145c2-194">Tę zmianę można wprowadzić ręcznie lub przez utworzenie skryptu zmiany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-194">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="145c2-195">Użyj Migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-195">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="145c2-196">Na potrzeby tego samouczka Użyj Migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="145c2-196">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="145c2-197">Zaktualizuj klasę `SeedData`, aby zapewnić wartość nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="145c2-197">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="145c2-198">Poniżej przedstawiono przykładową zmianę, ale trzeba wprowadzić tę zmianę dla każdego bloku `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="145c2-198">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="145c2-199">Zobacz [ukończony plik SeedData.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="145c2-199">See the [completed SeedData.cs file](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="145c2-200">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="145c2-200">Build the solution.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="145c2-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="145c2-201">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="145c2-202">Dodawanie migracji dla pola oceny</span><span class="sxs-lookup"><span data-stu-id="145c2-202">Add a migration for the rating field</span></span>

<span data-ttu-id="145c2-203">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet > konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="145c2-203">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="145c2-204">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="145c2-204">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="145c2-205">`Add-Migration` polecenie instruuje platformę, aby:</span><span class="sxs-lookup"><span data-stu-id="145c2-205">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="145c2-206">Porównaj model `Movie` ze schematem `Movie` DB.</span><span class="sxs-lookup"><span data-stu-id="145c2-206">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="145c2-207">Utwórz kod, aby zmigrować schemat bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="145c2-207">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="145c2-208">Nazwa "Rating" jest arbitralna i jest używana do nazwy pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="145c2-208">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="145c2-209">Warto użyć zrozumiałej nazwy dla pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="145c2-209">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="145c2-210">Polecenie `Update-Database` informuje platformę, aby zastosować zmiany schematu do bazy danych programu.</span><span class="sxs-lookup"><span data-stu-id="145c2-210">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="145c2-211">W przypadku usunięcia wszystkich rekordów w bazie danych inicjator będzie wypełniać bazę danych i zawierać pole `Rating`.</span><span class="sxs-lookup"><span data-stu-id="145c2-211">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="145c2-212">Można to zrobić za pomocą linków usuwania w przeglądarce lub z [programu SQL Server Eksplorator obiektów](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="145c2-212">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="145c2-213">Innym rozwiązaniem jest usunięcie bazy danych i użycie migracji w celu ponownego utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-213">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="145c2-214">Aby usunąć bazę danych w programie SSOX:</span><span class="sxs-lookup"><span data-stu-id="145c2-214">To delete the database in SSOX:</span></span>

* <span data-ttu-id="145c2-215">Wybierz bazę danych w SSOX.</span><span class="sxs-lookup"><span data-stu-id="145c2-215">Select the database in SSOX.</span></span>
* <span data-ttu-id="145c2-216">Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję *Usuń*.</span><span class="sxs-lookup"><span data-stu-id="145c2-216">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="145c2-217">Zaznacz pole wyboru **Zamknij istniejące połączenia**.</span><span class="sxs-lookup"><span data-stu-id="145c2-217">Check **Close existing connections**.</span></span>
* <span data-ttu-id="145c2-218">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="145c2-218">Select **OK**.</span></span>
* <span data-ttu-id="145c2-219">W obszarze [PMC](xref:tutorials/razor-pages/new-field#pmc)zaktualizuj bazę danych:</span><span class="sxs-lookup"><span data-stu-id="145c2-219">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="145c2-220">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="145c2-220">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="145c2-221">Porzuć i ponownie utwórz bazę danych</span><span class="sxs-lookup"><span data-stu-id="145c2-221">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="145c2-222">Usuń bazę danych i użyj migracji, aby ponownie utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="145c2-222">Delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="145c2-223">Aby usunąć bazę danych, usuń plik bazy danych (*MvcMovie. DB*).</span><span class="sxs-lookup"><span data-stu-id="145c2-223">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="145c2-224">Następnie uruchom `ef database update` polecenie:</span><span class="sxs-lookup"><span data-stu-id="145c2-224">Then run the `ef database update` command:</span></span>

```dotnetcli
dotnet ef database update
```

---

<span data-ttu-id="145c2-225">Uruchom aplikację i sprawdź, czy można tworzyć/edytować/wyświetlać filmy z polem `Rating`.</span><span class="sxs-lookup"><span data-stu-id="145c2-225">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="145c2-226">Jeśli baza danych nie jest zainicjowana, ustaw punkt przerwania w metodzie `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="145c2-226">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="145c2-227">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="145c2-227">Additional resources</span></span>

* [<span data-ttu-id="145c2-228">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="145c2-228">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="145c2-229">[Poprzedni: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
> [Dalej: Dodawanie walidacji](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="145c2-229">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end
