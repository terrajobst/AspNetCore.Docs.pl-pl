---
title: Dodawanie nowego pola do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak użyć migracje Code First Framework jednostki Dodawanie nowego pola do modelu, i przeprowadzić migrację tej zmiany do bazy danych.
ms.author: riande
ms.custom: mvc
ms.date: 12/25/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 3c29b01cc97dbfd93c21131c3d96ed309715da1a
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53382020"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="cf622-103">Dodawanie nowego pola do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="cf622-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="cf622-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cf622-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cf622-105">W tej sekcji [Entity Framework](/ef/core/get-started/aspnetcore/new-db) migracje Code First jest używana do:</span><span class="sxs-lookup"><span data-stu-id="cf622-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="cf622-106">Dodawanie nowego pola do modelu.</span><span class="sxs-lookup"><span data-stu-id="cf622-106">Add a new field to the model.</span></span>
* <span data-ttu-id="cf622-107">Przeprowadź migrację nowe pole do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cf622-107">Migrate the new field to the database.</span></span>

<span data-ttu-id="cf622-108">Gdy EF Code First służy do automatycznego tworzenia bazy danych, Code First:</span><span class="sxs-lookup"><span data-stu-id="cf622-108">When EF Code First is used to automatically create a database, Code First:</span></span>

* <span data-ttu-id="cf622-109">Dodaje tabelę w bazie danych do śledzenia schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cf622-109">Adds a table to the database to  track the schema of the database.</span></span>
* <span data-ttu-id="cf622-110">Sprawdź, czy baza danych jest zsynchronizowany z klasy modelu, który został wygenerowany z.</span><span class="sxs-lookup"><span data-stu-id="cf622-110">Verify the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="cf622-111">Jeśli nie są zsynchronizowane, EF zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="cf622-111">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="cf622-112">Ułatwia to znajdowanie problemów z niespójne bazy danych/kodu.</span><span class="sxs-lookup"><span data-stu-id="cf622-112">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="add-a-rating-property-to-the-movie-model"></a><span data-ttu-id="cf622-113">Dodawanie właściwości klasyfikacji do modelu Movie</span><span class="sxs-lookup"><span data-stu-id="cf622-113">Add a Rating Property to the Movie Model</span></span>

<span data-ttu-id="cf622-114">Dodaj `Rating` właściwości *Models/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="cf622-114">Add a `Rating` property to *Models/Movie.cs*:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="cf622-115">Utwórz aplikację (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="cf622-115">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="cf622-116">Ponieważ zostały dodane nowe pole do `Movie` klasy, należy zaktualizować powiązania białą listę dzięki tej nowej właściwości zostaną dołączone.</span><span class="sxs-lookup"><span data-stu-id="cf622-116">Because you've added a new field to the `Movie` class, you need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="cf622-117">W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metody akcji, aby uwzględnić `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="cf622-117">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="cf622-118">Aktualizowanie szablonów widoku, aby można było wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="cf622-118">Update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="cf622-119">Edytuj */Views/Movies/Index.cshtml* pliku i Dodaj `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="cf622-119">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="cf622-120">Aktualizacja */Views/Movies/Create.cshtml* z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="cf622-120">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> 

<!-- VS -------------------------->
# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[<span data-ttu-id="cf622-121">Program Visual Studio / Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cf622-121">Visual Studio / Visual Studio for Mac</span></span>](#tab/visual-studio+visual-studio-mac)

<span data-ttu-id="cf622-122">Można kopiowanie/wklejanie poprzedniego formularza grupy"" i umożliwić pomoc intelliSense, zaktualizuj pola.</span><span class="sxs-lookup"><span data-stu-id="cf622-122">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="cf622-123">Technologia IntelliSense działa z [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="cf622-123">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Deweloper wpisał literę R wartość atrybutu asp — dla w elemencie drugiego etykiety widoku.](new-field/_static/cr.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cf622-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cf622-127">Visual Studio Code</span></span>](#tab/visual-studio-code)
<!-- This tab intentionally left blank. -->
---  
<!-- End of VS tabs -->

<span data-ttu-id="cf622-128">Aktualizacja `SeedData` klasy tak, aby go oferuje wartości dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="cf622-128">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="cf622-129">Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="cf622-129">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="cf622-130">Aplikacja nie będzie działać, dopóki baza danych została zaktualizowana do nowego pola.</span><span class="sxs-lookup"><span data-stu-id="cf622-130">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="cf622-131">Jeśli ma ona Uruchom teraz następujące `SqlException` jest generowany:</span><span class="sxs-lookup"><span data-stu-id="cf622-131">If it's run now, the following `SqlException` is thrown:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="cf622-132">Ten błąd występuje, ponieważ zaktualizowane klasy modelu Movie różni się od schematu tabeli filmu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cf622-132">This error occurs because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="cf622-133">(Brak nie `Rating` kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="cf622-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="cf622-134">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="cf622-134">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="cf622-135">Ma automatycznie Porzuć i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cf622-135">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="cf622-136">To podejście jest bardzo wygodne na wczesnym etapie cyklu tworzenia oprogramowania, gdy robią active rozwoju w bazie danych testu; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych.</span><span class="sxs-lookup"><span data-stu-id="cf622-136">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="cf622-137">Wadą jednak jest utraty istniejących danych w bazie danych — dzięki czemu nie chcesz używać tej metody w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="cf622-137">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="cf622-138">Automatycznie zapełnić bazę danych przy użyciu danych testowych za pomocą inicjatora jest często produktywny sposób tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cf622-138">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="cf622-139">Jest to dobra metoda opracowywania wczesne i, gdy przy użyciu systemu SQLite.</span><span class="sxs-lookup"><span data-stu-id="cf622-139">This is a good approach for early development and when using SQLite.</span></span>

2. <span data-ttu-id="cf622-140">Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="cf622-140">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="cf622-141">Zaletą tego podejścia jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="cf622-141">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="cf622-142">Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.</span><span class="sxs-lookup"><span data-stu-id="cf622-142">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="cf622-143">Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="cf622-143">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="cf622-144">W tym samouczku jest używana migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="cf622-144">For this tutorial, Code First Migrations is used.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cf622-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cf622-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cf622-146">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="cf622-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](adding-model/_static/pmc.png)

<span data-ttu-id="cf622-148">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="cf622-148">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="cf622-149">`Add-Migration` Polecenie informuje platformę migracji, aby sprawdzić bieżące `Movie` modelu z bieżącymi `Movie` schematu bazy danych i utworzyć niezbędny kod, aby przeprowadzić migrację bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="cf622-149">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="cf622-150">Program Visual Studio Code / Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cf622-150">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="cf622-151">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="cf622-151">Run the following command:</span></span>

```cli
dotnet ef migrations add Rating
dotnet ef database update
```

---  
<!-- End of VS tabs -->

<span data-ttu-id="cf622-152">Nazwa "Ocena" dowolnej i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="cf622-152">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="cf622-153">Warto użyć znaczącą nazwę pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="cf622-153">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="cf622-154">Jeśli zostaną usunięte wszystkie rekordy w bazie danych, metoda inicjowania będzie obsługiwał bazy danych i zawiera `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="cf622-154">If all the records in the DB are deleted, the initialize method will seed the DB and include the `Rating` field.</span></span>

<span data-ttu-id="cf622-155">Uruchom aplikację i sprawdź, można tworzenia/edycji/wyświetlania filmów z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="cf622-155">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="cf622-156">Należy dodać `Rating` pole `Edit`, `Details`, i `Delete` wyświetlać szablony.</span><span class="sxs-lookup"><span data-stu-id="cf622-156">You should add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cf622-157">[Poprzednie](search.md)
> [dalej](validation.md)</span><span class="sxs-lookup"><span data-stu-id="cf622-157">[Previous](search.md)
[Next](validation.md)</span></span>  
