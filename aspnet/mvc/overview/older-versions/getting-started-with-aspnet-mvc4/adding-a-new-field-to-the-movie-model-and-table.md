---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Dodanie nowego pola filmu modelu i tabeli | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d8a42e9acdce687ab6e9742071dd2949f244622f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="8efac-104">Dodanie nowego pola filmu modelu i tabeli</span><span class="sxs-lookup"><span data-stu-id="8efac-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="8efac-105">przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8efac-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="8efac-106">Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="8efac-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="8efac-107">Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="8efac-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="8efac-108">W tej sekcji użyjesz migracje Code First Framework jednostki migrację pewne zmiany do klasy modeli, aby zmiana została zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8efac-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="8efac-109">Domyślnie korzystając z programu Entity Framework Code First automatycznie utworzyć bazę danych, tak jak wcześniej w tym samouczku Code First dodaje tabeli do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="8efac-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="8efac-110">Jeśli nie są zsynchronizowane, Entity Framework zgłasza błąd.</span><span class="sxs-lookup"><span data-stu-id="8efac-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="8efac-111">Ułatwia to śledzenie problemów w czasie opracowywania, znajdującego się w przeciwnym razie tylko (przez zasłoniętej błędy) w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8efac-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="8efac-112">Konfigurowanie migracje Code First zmiany modelu</span><span class="sxs-lookup"><span data-stu-id="8efac-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="8efac-113">Jeśli używasz programu Visual Studio 2012, kliknij dwukrotnie *Movies.mdf* plików w Eksploratorze rozwiązań, aby otworzyć narzędzie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8efac-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="8efac-114">Visual Studio Express for Web wyświetli Eksploratora bazy danych programu Visual Studio 2012 wyświetli Eksploratora serwera.</span><span class="sxs-lookup"><span data-stu-id="8efac-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="8efac-115">Jeśli używasz programu Visual Studio 2010, należy użyć Eksplorator obiektów SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8efac-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="8efac-116">W narzędziu bazy danych (Eksploratora bazy danych, w Eksploratorze serwera lub Eksploratorze obiektów SQL Server), kliknij prawym przyciskiem myszy `MovieDBContext` i wybierz **usunąć** można usunąć bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="8efac-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="8efac-117">Przejdź z powrotem do Eksploratora rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="8efac-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="8efac-118">Kliknij prawym przyciskiem myszy *Movies.mdf* plik i wybierz **usunąć** usuwania filmy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8efac-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="8efac-119">Tworzenie aplikacji, aby upewnić się, że nie ma żadnych błędów.</span><span class="sxs-lookup"><span data-stu-id="8efac-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="8efac-120">Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki** , a następnie **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="8efac-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Dodaj pakiet Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="8efac-122">W **Konsola Menedżera pakietów** okno w `PM>` wierszu wprowadź "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="8efac-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="8efac-123">**Enable-Migrations** polecenie (pokazanym powyżej) tworzy *Configuration.cs* plik w nowym *migracje* folderu.</span><span class="sxs-lookup"><span data-stu-id="8efac-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="8efac-124">Visual Studio otworzy *Configuration.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="8efac-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="8efac-125">Zastąp `Seed` metody w *Configuration.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8efac-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="8efac-126">Kliknij prawym przyciskiem myszy w dowolnym kształcie wierszu red w obszarze `Movie` i wybierz **rozwiązać** następnie **przy użyciu** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="8efac-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="8efac-127">Dzięki temu dodaje następujące instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="8efac-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="8efac-128">Kod wywoła pierwszy migracji `Seed` metody po każdym migracji (oznacza to, że wywołanie **update-database** w konsoli Menedżera pakietów), i ta metoda aktualizacji wierszy, które już zostały wstawione lub wstawia je, jeśli ich jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="8efac-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="8efac-129">**Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.** (Następujące kroki zakończy się niepowodzeniem, jeśli Twoje nie kompilacji w tym momencie.)</span><span class="sxs-lookup"><span data-stu-id="8efac-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="8efac-130">Następnym krokiem jest utworzenie `DbMigration` klasy początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="8efac-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="8efac-131">Migracja do tworzy nową bazę danych, dlatego należy usunąć *movie.mdf* pliku w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="8efac-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="8efac-132">W **Konsola Menedżera pakietów** okna, wprowadź polecenie "Dodaj migracji początkowego" do utworzenia początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="8efac-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="8efac-133">Nazwa "Początkowego" dowolnej i jest używany do nazywania plików migracji utworzony.</span><span class="sxs-lookup"><span data-stu-id="8efac-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="8efac-134">Migracje Code First tworzy innego pliku klasy w *migracje* folderze (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8efac-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="8efac-135">Aby pomóc w kolejności z sygnaturą czasową wstępnie ustala się filename migracji.</span><span class="sxs-lookup"><span data-stu-id="8efac-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="8efac-136">Sprawdź *{DateStamp}\_Initial.cs* plik zawiera instrukcje tworzenia tabeli filmy dla filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8efac-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="8efac-137">Po zaktualizowaniu bazy danych w instrukcji poniżej, *{DateStamp}\_Initial.cs* pliku zostaną uruchomione i Utwórz schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8efac-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="8efac-138">Następnie przy użyciu **inicjatora** metoda zostaną uruchomione, aby wypełnić bazę danych z danych testowych.</span><span class="sxs-lookup"><span data-stu-id="8efac-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="8efac-139">W **Konsola Menedżera pakietów**, wprowadź polecenie "update-database" Aby utworzyć bazę danych i uruchomić **inicjatora** metody.</span><span class="sxs-lookup"><span data-stu-id="8efac-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="8efac-140">Jeśli zostanie wyświetlony komunikat o błędzie wskazujący tabela już istnieje i nie można utworzyć, prawdopodobnie uruchomienia aplikacji, po usunięciu bazy danych i należy wykonać `update-database`.</span><span class="sxs-lookup"><span data-stu-id="8efac-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="8efac-141">W takim przypadku usuń *Movies.mdf* ponownie, a następnie spróbuj ponownie `update-database` polecenia.</span><span class="sxs-lookup"><span data-stu-id="8efac-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="8efac-142">Jeśli nadal występuje błąd, Usuń migrations folder i zawartość, a następnie uruchomić z instrukcjami w górnej części strony (który jest delete *Movies.mdf* pliku, a następnie przejdź do Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="8efac-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="8efac-143">Uruchom aplikację i przejdź do */Movies* adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8efac-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="8efac-144">Są wyświetlane dane inicjatora.</span><span class="sxs-lookup"><span data-stu-id="8efac-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="8efac-145">Dodawanie właściwości klasyfikacji do modelu film</span><span class="sxs-lookup"><span data-stu-id="8efac-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="8efac-146">Rozpocznij od dodania nowej `Rating` właściwości do istniejącej `Movie` klasy.</span><span class="sxs-lookup"><span data-stu-id="8efac-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="8efac-147">Otwórz *Models\Movie.cs* plik i dodać `Rating` właściwości podobne do następującego:</span><span class="sxs-lookup"><span data-stu-id="8efac-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="8efac-148">Pełną `Movie` klasy teraz wygląda podobnie do następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="8efac-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="8efac-149">Tworzenie aplikacji przy użyciu **kompilacji** &gt; **kompilacji filmu** menu polecenie lub naciskając klawisz CTRL-SHIFT-B.</span><span class="sxs-lookup"><span data-stu-id="8efac-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="8efac-150">Teraz, gdy użytkownik zaktualizował `Model` klasy, należy również zaktualizować *\Views\Movies\Index.cshtml* i *\Views\Movies\Create.cshtml* wyświetlania szablonów, aby wyświetlić nowy `Rating`właściwości w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8efac-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="8efac-151">Otwórz<em>\Views\Movies\Index.cshtml</em> plik i dodać `<th>Rating</th>` tuż po nagłówek kolumny <strong>cen</strong> kolumny.</span><span class="sxs-lookup"><span data-stu-id="8efac-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="8efac-152">Następnie dodaj `<td>` kolumny zbliża się koniec szablon do renderowania `@item.Rating` wartość.</span><span class="sxs-lookup"><span data-stu-id="8efac-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="8efac-153">Poniżej znajduje się jakie zaktualizowane <em>Index.cshtml</em> Wyświetl szablon wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="8efac-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="8efac-154">Następnie otwórz folder *\Views\Movies\Create.cshtml* i Dodaj następujący kod pod koniec formularza.</span><span class="sxs-lookup"><span data-stu-id="8efac-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="8efac-155">Renderuje pola tekstowego, tak aby po utworzeniu nowego movie, można określić klasyfikację.</span><span class="sxs-lookup"><span data-stu-id="8efac-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="8efac-156">Użytkownik zaktualizował teraz kodu aplikacji do obsługi nowej `Rating` właściwości.</span><span class="sxs-lookup"><span data-stu-id="8efac-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="8efac-157">Teraz uruchom aplikację i przejdź do */Movies* adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8efac-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="8efac-158">Po wykonaniu tej czynności, zostanie wyświetlony jeden z następujących błędów:</span><span class="sxs-lookup"><span data-stu-id="8efac-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="8efac-159">Ten błąd jest wyświetlane, ponieważ zaktualizowanego `Movie` klasy modelu w aplikacji teraz różni się od schematu `Movie` tabeli istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8efac-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="8efac-160">(Brak nie `Rating` kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="8efac-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="8efac-161">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="8efac-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="8efac-162">Ma automatycznie porzucenia i ponownego utworzenia bazy danych na podstawie nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8efac-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="8efac-163">Ta metoda jest bardzo wygodny w trakcie rozwoju active w testowej bazy danych; pozwala na szybkie razem rozwijać schematu modelu i bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8efac-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="8efac-164">Wadą interfejsu, jest jednak, że utracić istniejące dane w bazie danych — dlatego możesz *nie* chcesz użyć tej metody w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="8efac-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="8efac-165">Automatycznie inicjatora bazy danych z danych testowych za pomocą inicjatora jest często produktywności możliwości opracowywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8efac-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="8efac-166">Aby uzyskać więcej informacji na inicjatory bazy danych programu Entity Framework, zobacz Tomasz Dykstra [samouczek platformy ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="8efac-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="8efac-167">Jawnie modyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="8efac-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="8efac-168">Zaletą tej metody jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="8efac-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="8efac-169">Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.</span><span class="sxs-lookup"><span data-stu-id="8efac-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="8efac-170">Użyj migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8efac-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="8efac-171">W tym samouczku użyjemy migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="8efac-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="8efac-172">Seed — metoda aktualizacji, dzięki czemu zapewnia wartość dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="8efac-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="8efac-173">Otwórz plik Migrations\Configuration.cs i Dodaj pole klasyfikacji do każdego obiektu filmu.</span><span class="sxs-lookup"><span data-stu-id="8efac-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="8efac-174">Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8efac-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="8efac-175">`add-migration` Polecenie informuje framework migracji do badania bieżącego modelu film z bieżącego schematu filmu bazy danych i utworzyć niezbędne kod, aby przeprowadzić migrację bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="8efac-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="8efac-176">AddRatingMig dowolnej i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="8efac-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="8efac-177">Warto użyć opisową nazwę krok migracji.</span><span class="sxs-lookup"><span data-stu-id="8efac-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="8efac-178">Po zakończeniu działania tego polecenia programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` klasy, a następnie w `Up` — metoda zostanie wyświetlony kod, który tworzy nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="8efac-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="8efac-179">Skompiluj rozwiązanie, a następnie wprowadź polecenie "update-database" w **Konsola Menedżera pakietów** okna.</span><span class="sxs-lookup"><span data-stu-id="8efac-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="8efac-180">Na poniższej ilustracji przedstawiono dane wyjściowe w **Konsola Menedżera pakietów** okna (sygnaturę daty dołączanie AddRatingMig może się różnić).</span><span class="sxs-lookup"><span data-stu-id="8efac-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="8efac-181">Ponownie uruchom aplikację i przejdź do adresu URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="8efac-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="8efac-182">Widać nowe pole klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="8efac-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="8efac-183">Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy filmu.</span><span class="sxs-lookup"><span data-stu-id="8efac-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="8efac-184">Należy pamiętać, że można dodać ocenę.</span><span class="sxs-lookup"><span data-stu-id="8efac-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="8efac-186">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="8efac-186">Click **Create**.</span></span> <span data-ttu-id="8efac-187">Nowe film, w tym ocenę, teraz wyświetlone w filmy wyświetlania:</span><span class="sxs-lookup"><span data-stu-id="8efac-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="8efac-189">Należy również dodać `Rating` pola edycji, szczegóły i SearchIndex Wyświetl szablony.</span><span class="sxs-lookup"><span data-stu-id="8efac-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="8efac-190">Można wprowadzić polecenie "update-database" w **Konsola Menedżera pakietów** ponownie okno i żadne zmiany nie nastąpi, ponieważ schemat zgodny z modelem.</span><span class="sxs-lookup"><span data-stu-id="8efac-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="8efac-191">W tej sekcji przedstawiono sposób modyfikowania modelu obiektów i zachować synchronizację ze zmianami bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8efac-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="8efac-192">Przedstawiono również sposób wypełnienia nowo utworzonej bazy danych z przykładowymi danymi, więc można wypróbować scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="8efac-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="8efac-193">Następnie Oto jak można dodać bardziej rozbudowane logikę weryfikacji dla klasy modelu i włączyć niektóre reguły biznesowe są wymuszane.</span><span class="sxs-lookup"><span data-stu-id="8efac-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8efac-194">[Poprzednie](examining-the-edit-methods-and-edit-view.md)
> [dalej](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="8efac-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
