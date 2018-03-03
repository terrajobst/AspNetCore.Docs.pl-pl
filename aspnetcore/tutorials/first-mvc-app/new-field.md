---
title: Dodanie nowego pola
author: rick-anderson
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 7b92d1b50311cedbae2bdf30a2dc19204c5b81d1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="adding-a-new-field"></a><span data-ttu-id="1eee0-102">Dodanie nowego pola</span><span class="sxs-lookup"><span data-stu-id="1eee0-102">Adding a New Field</span></span>

<span data-ttu-id="1eee0-103">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1eee0-103">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1eee0-104">W tej sekcji użyjesz [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migracje Code First, aby dodać nowe pole do modelu i migracji, które zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1eee0-104">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="1eee0-105">Gdy używasz EF Code First automatycznie utworzyć bazę danych, Code First dodaje tabeli do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="1eee0-105">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="1eee0-106">Jeśli nie są zsynchronizowane, EF zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="1eee0-106">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="1eee0-107">Dzięki temu można łatwiej znaleźć problemy niespójne bazy danych/kod.</span><span class="sxs-lookup"><span data-stu-id="1eee0-107">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="1eee0-108">Dodawanie właściwości klasyfikacji do modelu film</span><span class="sxs-lookup"><span data-stu-id="1eee0-108">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="1eee0-109">Otwórz *Models/Movie.cs* plik i dodać `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="1eee0-109">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="1eee0-110">Tworzenie aplikacji (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="1eee0-110">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="1eee0-111">Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania białą listę dzięki tej nowej właściwości zostaną uwzględnione.</span><span class="sxs-lookup"><span data-stu-id="1eee0-111">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="1eee0-112">W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metod akcji, aby uwzględnić `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="1eee0-112">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="1eee0-113">Należy również zaktualizować szablony widok, aby wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1eee0-113">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="1eee0-114">Edytuj */Views/Movies/Index.cshtml* plik i dodać `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="1eee0-114">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="1eee0-115">Aktualizacja */Views/Movies/Create.cshtml* z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="1eee0-115">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="1eee0-116">Można kopiowania/wklejania poprzednią "formularza grupę" i umożliwić pomoc intelliSense, zaktualizuj pola.</span><span class="sxs-lookup"><span data-stu-id="1eee0-116">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="1eee0-117">IntelliSense współpracuje z [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="1eee0-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="1eee0-118">Uwaga: W wersji RTM programu Visual Studio 2017 należy zainstalować [usługi języka Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) dla Razor intelliSense.</span><span class="sxs-lookup"><span data-stu-id="1eee0-118">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="1eee0-119">Ten problem zostanie rozwiązany w następnej wersji.</span><span class="sxs-lookup"><span data-stu-id="1eee0-119">This will be fixed in the next release.</span></span>

![Deweloper wpisał litera R dla wartości atrybutu asp — dla w drugi element label w widoku.](new-field/_static/cr.png)

<span data-ttu-id="1eee0-123">Aplikacja nie będzie działać, dopóki nie możemy zaktualizować bazę danych, aby uwzględnić nowe pole.</span><span class="sxs-lookup"><span data-stu-id="1eee0-123">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="1eee0-124">Jeżeli możesz uruchomić go teraz, uzyskasz następujące `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="1eee0-124">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="1eee0-125">Ten błąd jest wyświetlane, ponieważ zaktualizowane klasy modelu film jest inny niż schemat tabeli filmu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1eee0-125">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="1eee0-126">(Brak żadnej klasyfikacji kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="1eee0-126">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="1eee0-127">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="1eee0-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="1eee0-128">Ma automatycznie porzucenia i ponownego utworzenia bazy danych na podstawie nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1eee0-128">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="1eee0-129">Ta metoda jest bardzo wygodny wczesnym etapie cyklu programowanie podczas wprowadzania active Programowanie w testowej bazy danych; pozwala na szybkie razem rozwijać schematu modelu i bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1eee0-129">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="1eee0-130">Wadą interfejsu, jest jednak, że utracić istniejące dane w bazie danych, więc nie chcesz użyć tej metody w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="1eee0-130">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="1eee0-131">Automatycznie inicjatora bazy danych z danych testowych za pomocą inicjatora jest często produktywności możliwości opracowywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1eee0-131">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="1eee0-132">Jawnie modyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="1eee0-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="1eee0-133">Zaletą tej metody jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="1eee0-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="1eee0-134">Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.</span><span class="sxs-lookup"><span data-stu-id="1eee0-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="1eee0-135">Użyj migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1eee0-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="1eee0-136">W tym samouczku użyjemy migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="1eee0-136">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="1eee0-137">Aktualizacja `SeedData` klasy, dzięki czemu zapewnia wartość dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="1eee0-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="1eee0-138">Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="1eee0-138">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="1eee0-139">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="1eee0-139">Build the solution.</span></span>

<span data-ttu-id="1eee0-140">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > konsoli Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="1eee0-140">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC menu](adding-model/_static/pmc.png)

<span data-ttu-id="1eee0-142">W kryterium wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1eee0-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="1eee0-143">`Add-Migration` Polecenie informuje framework migracji, aby sprawdzić bieżące `Movie` modelu z bieżącym `Movie` schemat bazy danych i utworzyć niezbędne kod, aby przeprowadzić migrację bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="1eee0-143">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="1eee0-144">Nazwa "Klasyfikacja" jest dowolnego i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="1eee0-144">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="1eee0-145">Warto użyć opisową nazwę pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="1eee0-145">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="1eee0-146">Po usunięciu wszystkich rekordów w bazie danych będzie inicjatora bazy danych i obejmują `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="1eee0-146">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="1eee0-147">Można to zrobić z łączami Usuń w przeglądarce lub SSOX.</span><span class="sxs-lookup"><span data-stu-id="1eee0-147">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="1eee0-148">Uruchom aplikację i sprawdzić, można utworzyć/edycji/wyświetlania filmów `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="1eee0-148">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="1eee0-149">Należy również dodać `Rating` do `Edit`, `Details`, i `Delete` wyświetlania szablonów.</span><span class="sxs-lookup"><span data-stu-id="1eee0-149">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1eee0-150">[Poprzednie](search.md)
[dalej](validation.md)</span><span class="sxs-lookup"><span data-stu-id="1eee0-150">[Previous](search.md)
[Next](validation.md)</span></span>  
