---
title: Dodawanie nowego pola do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak za pomocą migracje Code First platformy Entity Framework dodać nowe pole do modelu i zmigrować tę zmianę do bazy danych.
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 6a2a2ca45f793ab95d45281ebb23180ac64761ec
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082314"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="48e31-103">Dodawanie nowego pola do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="48e31-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="48e31-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="48e31-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="48e31-105">W tej sekcji [Entity Framework](/ef/core/get-started/aspnetcore/new-db) migracje Code First służy do:</span><span class="sxs-lookup"><span data-stu-id="48e31-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="48e31-106">Dodaj nowe pole do modelu.</span><span class="sxs-lookup"><span data-stu-id="48e31-106">Add a new field to the model.</span></span>
* <span data-ttu-id="48e31-107">Migruj nowe pole do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48e31-107">Migrate the new field to the database.</span></span>

<span data-ttu-id="48e31-108">Gdy Code First EF jest używany do automatycznego tworzenia bazy danych, Code First:</span><span class="sxs-lookup"><span data-stu-id="48e31-108">When EF Code First is used to automatically create a database, Code First:</span></span>

* <span data-ttu-id="48e31-109">Dodaje tabelę do bazy danych w celu śledzenia schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48e31-109">Adds a table to the database to  track the schema of the database.</span></span>
* <span data-ttu-id="48e31-110">Weryfikuje, czy baza danych jest zsynchronizowana z klasami modelu, z których została wygenerowana.</span><span class="sxs-lookup"><span data-stu-id="48e31-110">Verifies the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="48e31-111">Jeśli nie są zsynchronizowane, EF zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="48e31-111">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="48e31-112">Ułatwia to znalezienie niespójnych problemów z bazą danych i kodem.</span><span class="sxs-lookup"><span data-stu-id="48e31-112">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="add-a-rating-property-to-the-movie-model"></a><span data-ttu-id="48e31-113">Dodawanie właściwości oceny do modelu filmu</span><span class="sxs-lookup"><span data-stu-id="48e31-113">Add a Rating Property to the Movie Model</span></span>

<span data-ttu-id="48e31-114">Dodawanie właściwości `Rating` do *modeli/filmów. cs*:</span><span class="sxs-lookup"><span data-stu-id="48e31-114">Add a `Rating` property to *Models/Movie.cs*:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="48e31-115">Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="48e31-115">Build the app</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48e31-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48e31-116">Visual Studio</span></span>](#tab/visual-studio)

 <span data-ttu-id="48e31-117">Ctrl+Shift+B</span><span class="sxs-lookup"><span data-stu-id="48e31-117">Ctrl+Shift+B</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48e31-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48e31-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet build
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48e31-119">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="48e31-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48e31-120">Polecenie ⌘ + B</span><span class="sxs-lookup"><span data-stu-id="48e31-120">Command ⌘ + B</span></span>

------

<span data-ttu-id="48e31-121">Ponieważ dodano nowe pole do klasy `Movie`, należy zaktualizować białe listy powiązań, aby ta nowa właściwość została uwzględniona.</span><span class="sxs-lookup"><span data-stu-id="48e31-121">Because you've added a new field to the `Movie` class, you need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="48e31-122">W *MoviesController.cs*, zaktualizuj atrybut `[Bind]` dla `Create` i `Edit` metody akcji, aby uwzględnić Właściwość `Rating`:</span><span class="sxs-lookup"><span data-stu-id="48e31-122">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("Id,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="48e31-123">Aktualizowanie szablonów widoku w celu wyświetlania, tworzenia i edytowania nowej właściwości `Rating` w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="48e31-123">Update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="48e31-124">Edytuj plik */views/Movies/index.cshtml* i dodaj pole `Rating`:</span><span class="sxs-lookup"><span data-stu-id="48e31-124">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=16,38&range=24-64)]

<span data-ttu-id="48e31-125">Zaktualizuj */views/Movies/Create.cshtml* za pomocą pola `Rating`.</span><span class="sxs-lookup"><span data-stu-id="48e31-125">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[<span data-ttu-id="48e31-126">Visual Studio/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="48e31-126">Visual Studio / Visual Studio for Mac</span></span>](#tab/visual-studio+visual-studio-mac)

<span data-ttu-id="48e31-127">Możesz skopiować/wkleić poprzednią "grupę formularzy" i pozwól, aby funkcja intelliSense mogła zaktualizować pola.</span><span class="sxs-lookup"><span data-stu-id="48e31-127">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="48e31-128">Technologia IntelliSense współpracuje z [pomocnikami tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="48e31-128">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Deweloper wpisze literę R dla wartości atrybutu ASP-for w drugim elemencie Label widoku.](new-field/_static/cr.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48e31-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48e31-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- This tab intentionally left blank. -->

---

<span data-ttu-id="48e31-133">Zaktualizuj pozostałe szablony.</span><span class="sxs-lookup"><span data-stu-id="48e31-133">Update the remaining templates.</span></span>

<span data-ttu-id="48e31-134">Zaktualizuj klasę `SeedData`, aby zapewnić wartość nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="48e31-134">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="48e31-135">Poniżej przedstawiono przykładową zmianę, ale trzeba wprowadzić tę zmianę dla każdego `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="48e31-135">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="48e31-136">Aplikacja nie będzie działała, dopóki baza danych nie zostanie zaktualizowana w celu uwzględnienia nowego pola.</span><span class="sxs-lookup"><span data-stu-id="48e31-136">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="48e31-137">Jeśli jest teraz uruchomiona, zostanie zgłoszony następujący `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="48e31-137">If it's run now, the following `SqlException` is thrown:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="48e31-138">Ten błąd występuje, ponieważ zaktualizowana Klasa modelu filmu jest inna niż schemat tabeli filmów istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48e31-138">This error occurs because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="48e31-139">(Brak kolumny `Rating` w tabeli bazy danych).</span><span class="sxs-lookup"><span data-stu-id="48e31-139">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="48e31-140">Istnieje kilka metod rozpoznawania błędu:</span><span class="sxs-lookup"><span data-stu-id="48e31-140">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="48e31-141">Entity Framework automatycznie porzucić i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="48e31-141">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="48e31-142">To podejście jest bardzo wygodne w cyklu rozwoju, gdy chodzi o aktywne programowanie w testowej bazie danych. pozwala ona szybko rozwijać model i schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48e31-142">This approach is very convenient early in the development cycle when you're doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="48e31-143">Minusem, mimo że utracisz istniejące dane w bazie danych, więc nie chcesz używać tego podejścia w produkcyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="48e31-143">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="48e31-144">Użycie inicjatora do automatycznego umieszczania bazy danych z danymi testowymi jest często wydajnym sposobem na tworzenie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48e31-144">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="48e31-145">Jest to dobre podejście do wczesnego programowania i korzystania z oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="48e31-145">This is a good approach for early development and when using SQLite.</span></span>

2. <span data-ttu-id="48e31-146">Jawnie zmodyfikuj schemat istniejącej bazy danych, tak aby pasował do klas modelu.</span><span class="sxs-lookup"><span data-stu-id="48e31-146">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="48e31-147">Zaletą tego podejścia jest utrzymywanie danych.</span><span class="sxs-lookup"><span data-stu-id="48e31-147">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="48e31-148">Tę zmianę można wprowadzić ręcznie lub przez utworzenie skryptu zmiany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48e31-148">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="48e31-149">Użyj Migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48e31-149">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="48e31-150">W tym samouczku zostanie użyta Migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="48e31-150">For this tutorial, Code First Migrations is used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48e31-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48e31-151">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48e31-152">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet > konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="48e31-152">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](adding-model/_static/pmc.png)

<span data-ttu-id="48e31-154">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="48e31-154">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="48e31-155">`Add-Migration` polecenie instruuje platformę migracji, aby przeanalizować bieżący model `Movie` z bieżącym schematem `Movie` DB i utworzyć wymagany kod w celu przeprowadzenia migracji bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="48e31-155">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span>

<span data-ttu-id="48e31-156">Nazwa "Rating" jest arbitralna i jest używana do nazwy pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="48e31-156">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="48e31-157">Warto użyć zrozumiałej nazwy dla pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="48e31-157">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="48e31-158">Jeśli wszystkie rekordy w bazie danych zostaną usunięte, metoda Initialize będzie wypełniać bazę danych i zawierać pole `Rating`.</span><span class="sxs-lookup"><span data-stu-id="48e31-158">If all the records in the DB are deleted, the initialize method will seed the DB and include the `Rating` field.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="48e31-159">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="48e31-159">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="48e31-160">Usuń bazę danych i użyj migracji, aby ponownie utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="48e31-160">Delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="48e31-161">Aby usunąć bazę danych, usuń plik bazy danych (*MvcMovie. DB*).</span><span class="sxs-lookup"><span data-stu-id="48e31-161">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="48e31-162">Następnie uruchom `ef database update` polecenie:</span><span class="sxs-lookup"><span data-stu-id="48e31-162">Then run the `ef database update` command:</span></span>

```dotnetcli
dotnet ef database update
```

---
<!-- End of VS tabs -->

<span data-ttu-id="48e31-163">Uruchom aplikację i sprawdź, czy można tworzyć/edytować/wyświetlać filmy z polem `Rating`.</span><span class="sxs-lookup"><span data-stu-id="48e31-163">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="48e31-164">Należy dodać pole `Rating` do szablonów widoków `Edit`, `Details`i `Delete`.</span><span class="sxs-lookup"><span data-stu-id="48e31-164">You should add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48e31-165">[Poprzedni](search.md)
> [Następny](validation.md)</span><span class="sxs-lookup"><span data-stu-id="48e31-165">[Previous](search.md)
[Next](validation.md)</span></span>
