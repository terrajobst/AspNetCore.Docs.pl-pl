---
title: Dodaj nowe pole do aplikacji platformy ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak używać migracje Code First Framework jednostki Dodaj nowe pole do modelu i przeprowadzić migrację tej zmiany do bazy danych.
ms.author: riande
ms.date: 10/14/2016
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: eb98ebcde1086ad605127dddc055a18d4874c722
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961038"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="cc060-103">Dodaj nowe pole do aplikacji platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="cc060-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="cc060-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cc060-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cc060-105">W tej sekcji użyjesz [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migracje Code First, aby dodać nowe pole do modelu i migracji, które zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="cc060-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="cc060-106">Gdy używasz EF Code First automatycznie utworzyć bazę danych, Code First dodaje tabeli do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="cc060-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="cc060-107">Jeśli nie są zsynchronizowane, EF zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="cc060-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="cc060-108">Dzięki temu można łatwiej znaleźć problemy niespójne bazy danych/kod.</span><span class="sxs-lookup"><span data-stu-id="cc060-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="cc060-109">Dodawanie właściwości klasyfikacji do modelu film</span><span class="sxs-lookup"><span data-stu-id="cc060-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="cc060-110">Otwórz *Models/Movie.cs* plik i dodać `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="cc060-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]
::: moniker-end

<span data-ttu-id="cc060-111">Tworzenie aplikacji (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="cc060-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="cc060-112">Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania białą listę dzięki tej nowej właściwości zostaną uwzględnione.</span><span class="sxs-lookup"><span data-stu-id="cc060-112">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="cc060-113">W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metod akcji, aby uwzględnić `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="cc060-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="cc060-114">Należy również zaktualizować szablony widok, aby wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="cc060-114">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="cc060-115">Edytuj */Views/Movies/Index.cshtml* plik i dodać `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="cc060-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="cc060-116">Aktualizacja */Views/Movies/Create.cshtml* z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="cc060-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="cc060-117">Można kopiowania/wklejania poprzednią "formularza grupę" i umożliwić pomoc intelliSense, zaktualizuj pola.</span><span class="sxs-lookup"><span data-stu-id="cc060-117">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="cc060-118">IntelliSense współpracuje z [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="cc060-118">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="cc060-119">Uwaga: W wersji RTM programu Visual Studio 2017 należy zainstalować [usługi języka Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) dla Razor intelliSense.</span><span class="sxs-lookup"><span data-stu-id="cc060-119">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="cc060-120">Ten problem zostanie rozwiązany w następnej wersji.</span><span class="sxs-lookup"><span data-stu-id="cc060-120">This will be fixed in the next release.</span></span>

![Deweloper wpisał litera R dla wartości atrybutu asp — dla w drugi element label w widoku.](new-field/_static/cr.png)

<span data-ttu-id="cc060-124">Aplikacja nie będzie działać, dopóki nie możemy zaktualizować bazę danych, aby uwzględnić nowe pole.</span><span class="sxs-lookup"><span data-stu-id="cc060-124">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="cc060-125">Jeżeli możesz uruchomić go teraz, uzyskasz następujące `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="cc060-125">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="cc060-126">Ten błąd jest wyświetlane, ponieważ zaktualizowane klasy modelu film jest inny niż schemat tabeli filmu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc060-126">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="cc060-127">(Brak żadnej klasyfikacji kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="cc060-127">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="cc060-128">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="cc060-128">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="cc060-129">Ma automatycznie porzucenia i ponownego utworzenia bazy danych na podstawie nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cc060-129">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="cc060-130">Ta metoda jest bardzo wygodny wczesnym etapie cyklu programowanie podczas wprowadzania active Programowanie w testowej bazy danych; pozwala na szybkie razem rozwijać schematu modelu i bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc060-130">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="cc060-131">Wadą interfejsu, jest jednak, że utracić istniejące dane w bazie danych, więc nie chcesz użyć tej metody w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="cc060-131">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="cc060-132">Automatycznie inicjatora bazy danych z danych testowych za pomocą inicjatora jest często produktywności możliwości opracowywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc060-132">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="cc060-133">Jawnie modyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="cc060-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="cc060-134">Zaletą tej metody jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="cc060-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="cc060-135">Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.</span><span class="sxs-lookup"><span data-stu-id="cc060-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="cc060-136">Użyj migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc060-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="cc060-137">W tym samouczku użyjemy migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="cc060-137">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="cc060-138">Aktualizacja `SeedData` klasy, dzięki czemu zapewnia wartość dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="cc060-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="cc060-139">Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="cc060-139">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="cc060-140">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="cc060-140">Build the solution.</span></span>

<span data-ttu-id="cc060-141">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > konsoli Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="cc060-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC menu](adding-model/_static/pmc.png)

<span data-ttu-id="cc060-143">W kryterium wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="cc060-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="cc060-144">`Add-Migration` Polecenie informuje framework migracji, aby sprawdzić bieżące `Movie` modelu z bieżącym `Movie` schemat bazy danych i utworzyć niezbędne kod, aby przeprowadzić migrację bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="cc060-144">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="cc060-145">Nazwa "Klasyfikacja" jest dowolnego i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="cc060-145">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="cc060-146">Warto użyć opisową nazwę pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="cc060-146">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="cc060-147">Po usunięciu wszystkich rekordów w bazie danych będzie inicjatora bazy danych i obejmują `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="cc060-147">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="cc060-148">Można to zrobić z łączami Usuń w przeglądarce lub SSOX.</span><span class="sxs-lookup"><span data-stu-id="cc060-148">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="cc060-149">Uruchom aplikację i sprawdzić, można utworzyć/edycji/wyświetlania filmów `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="cc060-149">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="cc060-150">Należy również dodać `Rating` do `Edit`, `Details`, i `Delete` wyświetlania szablonów.</span><span class="sxs-lookup"><span data-stu-id="cc060-150">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cc060-151">[Poprzednie](search.md)
> [dalej](validation.md)</span><span class="sxs-lookup"><span data-stu-id="cc060-151">[Previous](search.md)
[Next](validation.md)</span></span>  
