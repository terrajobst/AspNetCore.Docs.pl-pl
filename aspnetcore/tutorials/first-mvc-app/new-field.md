---
title: Dodawanie nowego pola do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak użyć migracje Code First Framework jednostki Dodawanie nowego pola do modelu, i przeprowadzić migrację tej zmiany do bazy danych.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: e58d5af90b997c66cb749ab8f1b2f8049b8f7303
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089689"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="e1408-103">Dodawanie nowego pola do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e1408-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="e1408-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e1408-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e1408-105">W tej sekcji użyjesz [Entity Framework](/ef/core/get-started/aspnetcore/new-db) migracje Code First dodaje nowe pole do modelu i migracji, które zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e1408-105">In this section you'll use [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="e1408-106">Korzystając z programów EF Code First automatycznie utworzyć bazę danych, rozwiązanie Code First dodaje tabelę do bazy danych, aby śledzić, czy schemat bazy danych jest zsynchronizowany z klasy modelu, który został wygenerowany z.</span><span class="sxs-lookup"><span data-stu-id="e1408-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="e1408-107">Jeśli nie są zsynchronizowane, EF zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="e1408-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="e1408-108">Ułatwia to znajdowanie problemów z niespójne bazy danych/kodu.</span><span class="sxs-lookup"><span data-stu-id="e1408-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="e1408-109">Dodawanie właściwości klasyfikacji do modelu Movie</span><span class="sxs-lookup"><span data-stu-id="e1408-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="e1408-110">Otwórz *Models/Movie.cs* pliku i Dodaj `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="e1408-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

<span data-ttu-id="e1408-111">Utwórz aplikację (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="e1408-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="e1408-112">Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania białą listę dzięki tej nowej właściwości zostaną dołączone.</span><span class="sxs-lookup"><span data-stu-id="e1408-112">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="e1408-113">W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metody akcji, aby uwzględnić `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="e1408-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="e1408-114">Należy również zaktualizować Przeglądanie szablonów, aby można było wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e1408-114">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="e1408-115">Edytuj */Views/Movies/Index.cshtml* pliku i Dodaj `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="e1408-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="e1408-116">Aktualizacja */Views/Movies/Create.cshtml* z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="e1408-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="e1408-117">Można kopiowanie/wklejanie poprzedniego formularza grupy"" i umożliwić pomoc intelliSense, zaktualizuj pola.</span><span class="sxs-lookup"><span data-stu-id="e1408-117">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="e1408-118">Technologia IntelliSense działa z [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="e1408-118">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="e1408-119">Uwaga: W wersji RTM programu Visual Studio 2017 musisz zainstalować [usługi języka Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) Razor funkcji IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e1408-119">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="e1408-120">Ten problem zostanie rozwiązany w następnej wersji.</span><span class="sxs-lookup"><span data-stu-id="e1408-120">This will be fixed in the next release.</span></span>

![Deweloper wpisał literę R wartość atrybutu asp — dla w elemencie drugiego etykiety widoku.](new-field/_static/cr.png)

<span data-ttu-id="e1408-124">Aplikacja nie będzie działać, dopóki nie możemy zaktualizować bazy danych, aby uwzględnić nowe pole.</span><span class="sxs-lookup"><span data-stu-id="e1408-124">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="e1408-125">Jeśli uruchamiasz go teraz, uzyskasz następujące `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="e1408-125">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="e1408-126">Widzisz ten błąd, ponieważ zaktualizowane klasy modelu Movie różni się od schematu tabeli filmu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e1408-126">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="e1408-127">(Brak jest żadnej kolumny ocenę w tabeli bazy danych).</span><span class="sxs-lookup"><span data-stu-id="e1408-127">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="e1408-128">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="e1408-128">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="e1408-129">Ma automatycznie Porzuć i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e1408-129">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="e1408-130">To podejście jest bardzo wygodne na wczesnym etapie cyklu tworzenia oprogramowania, gdy robią active rozwoju w bazie danych testu; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e1408-130">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="e1408-131">Wadą jednak jest utraty istniejących danych w bazie danych — dzięki czemu nie chcesz używać tej metody w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="e1408-131">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="e1408-132">Automatycznie zapełnić bazę danych przy użyciu danych testowych za pomocą inicjatora jest często produktywny sposób tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e1408-132">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="e1408-133">Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="e1408-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="e1408-134">Zaletą tego podejścia jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="e1408-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="e1408-135">Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.</span><span class="sxs-lookup"><span data-stu-id="e1408-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="e1408-136">Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="e1408-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="e1408-137">W tym samouczku użyjemy migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="e1408-137">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="e1408-138">Aktualizacja `SeedData` klasy tak, aby go oferuje wartości dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="e1408-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="e1408-139">Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="e1408-139">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="e1408-140">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="e1408-140">Build the solution.</span></span>

<span data-ttu-id="e1408-141">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="e1408-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Menu konsoli zarządzania Pakietami](adding-model/_static/pmc.png)

<span data-ttu-id="e1408-143">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e1408-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="e1408-144">`Add-Migration` Polecenie informuje platformę migracji, aby sprawdzić bieżące `Movie` modelu z bieżącymi `Movie` schematu bazy danych i utworzyć niezbędny kod, aby przeprowadzić migrację bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="e1408-144">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="e1408-145">Nazwa "Ocena" dowolnej i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="e1408-145">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="e1408-146">Warto użyć znaczącą nazwę pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="e1408-146">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="e1408-147">Jeśli usuniesz wszystkie rekordy w bazie danych, inicjowania będzie obsługiwał bazy danych i obejmują `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="e1408-147">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="e1408-148">Można to zrobić, wraz z łączami delete w przeglądarce lub SSOX.</span><span class="sxs-lookup"><span data-stu-id="e1408-148">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="e1408-149">Uruchom aplikację i sprawdź, można tworzenia/edycji/wyświetlania filmów z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="e1408-149">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="e1408-150">Należy również dodać `Rating` pole `Edit`, `Details`, i `Delete` wyświetlać szablony.</span><span class="sxs-lookup"><span data-stu-id="e1408-150">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e1408-151">[Poprzednie](search.md)
> [dalej](validation.md)</span><span class="sxs-lookup"><span data-stu-id="e1408-151">[Previous](search.md)
[Next](validation.md)</span></span>  
