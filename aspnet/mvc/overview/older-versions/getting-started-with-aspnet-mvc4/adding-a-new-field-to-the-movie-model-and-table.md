---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Dodawanie nowego pola do modelu Movie i tabeli | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna w tym miejscu używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 45e6de410266af9d7909cea6d03258d2db7cb240
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753613"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="f5432-104">Dodawanie nowego pola do modelu Movie i tabeli</span><span class="sxs-lookup"><span data-stu-id="f5432-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="f5432-105">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="f5432-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="f5432-106">Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="f5432-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="f5432-107">Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="f5432-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="f5432-108">W tej sekcji użyjesz migracje Code First Framework jednostki migracji pewne zmiany do klasy modelu, aby zmiana została zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f5432-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="f5432-109">Domyślnie gdy używasz programu Entity Framework Code First automatycznie utworzyć bazę danych, tak jak wcześniej w tym samouczku rozwiązanie Code First dodaje tabelę do bazy danych, aby śledzić, czy schemat bazy danych jest zsynchronizowany z klasy modelu, który został wygenerowany z.</span><span class="sxs-lookup"><span data-stu-id="f5432-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="f5432-110">Jeśli nie są zsynchronizowane, platformy Entity Framework zgłasza błąd.</span><span class="sxs-lookup"><span data-stu-id="f5432-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="f5432-111">Ta funkcja ułatwia śledzenie problemów w czasie programowania, które mogą w przeciwnym razie tylko się okazać (przez zasłoniętej błędy) w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="f5432-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="f5432-112">Konfigurowanie funkcji migracje Code First dla zmiany modelu</span><span class="sxs-lookup"><span data-stu-id="f5432-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="f5432-113">Jeśli używasz programu Visual Studio 2012, kliknij dwukrotnie *Movies.mdf* plik z Eksploratora rozwiązań, aby otworzyć narzędzie do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f5432-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="f5432-114">Visual Studio Express for Web pokaże Eksplorator bazy danych programu Visual Studio 2012 pokaże Eksploratora serwera.</span><span class="sxs-lookup"><span data-stu-id="f5432-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="f5432-115">Jeśli używasz programu Visual Studio 2010, należy użyć Eksplorator obiektów SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f5432-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="f5432-116">W narzędziu bazy danych (Eksplorator bazy danych, Eksplorator serwera lub Eksploratorze obiektów SQL Server), kliknij prawym przyciskiem myszy `MovieDBContext` i wybierz **Usuń** celu porzucenia bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="f5432-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="f5432-117">Przejdź z powrotem do Eksploratora rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="f5432-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="f5432-118">Kliknij prawym przyciskiem myszy *Movies.mdf* plik i wybierz **Usuń** można usunąć bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="f5432-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="f5432-119">Utwórz aplikację, aby upewnić się, że nie ma żadnych błędów.</span><span class="sxs-lookup"><span data-stu-id="f5432-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="f5432-120">Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki** i następnie **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="f5432-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Dodaj pakiet Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="f5432-122">W **Konsola Menedżera pakietów** okna w `PM>` wierszu wprowadź "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="f5432-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="f5432-123">**Enable-Migrations** polecenie (jak pokazano powyżej) umożliwia utworzenie *Configuration.cs* plik w nowym *migracje* folderu.</span><span class="sxs-lookup"><span data-stu-id="f5432-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="f5432-124">Zostanie otwarty program Visual Studio *Configuration.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="f5432-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="f5432-125">Zastąp `Seed` method in Class metoda *Configuration.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f5432-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="f5432-126">Kliknij prawym przyciskiem myszy czerwona linia falista pod `Movie` i wybierz **rozwiązać** następnie **przy użyciu** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="f5432-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="f5432-127">Spowoduje to dodanie następujących instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="f5432-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="f5432-128">Kod wywołuje metodę migracji First `Seed` metody po każdej migracji (oznacza to, że wywołanie **update-database** w konsoli Menedżera pakietów), i ta metoda aktualizuje wierszy, które już zostały wstawione lub wstawia je, jeśli ich jeszcze nie istnieją.</span><span class="sxs-lookup"><span data-stu-id="f5432-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="f5432-129">**Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.** (Następujących kroków zakończy się niepowodzeniem, jeśli usługi nie kompilacji na tym etapie.)</span><span class="sxs-lookup"><span data-stu-id="f5432-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="f5432-130">Następnym krokiem jest utworzenie `DbMigration` klasy początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="f5432-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="f5432-131">Ta migracja do tworzy nową bazę danych, dlatego możesz usunąć *movie.mdf* pliku w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="f5432-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="f5432-132">W **Konsola Menedżera pakietów** okna, wprowadź polecenie "Dodaj migracji początkowego" do utworzenia początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="f5432-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="f5432-133">Nazwa "Początkowego" dowolnej i jest używany do nazywania plików migracji utworzone.</span><span class="sxs-lookup"><span data-stu-id="f5432-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="f5432-134">Migracje Code First tworzy inny plik klasy w *migracje* folderze (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f5432-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="f5432-135">Nazwa pliku migracji jest wstępnie stała z sygnaturą czasową pomagające w kolejności.</span><span class="sxs-lookup"><span data-stu-id="f5432-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="f5432-136">Sprawdź *{DateStamp}\_Initial.cs* plik zawiera instrukcje tworzenia tabeli filmy dla bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="f5432-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="f5432-137">Kiedy aktualizować bazę danych w instrukcjach poniżej to *{DateStamp}\_Initial.cs* pliku zostaną uruchomione i Utwórz schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f5432-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="f5432-138">A następnie **inicjatora** metoda zostanie uruchomiony do wypełniania bazy danych z danymi.</span><span class="sxs-lookup"><span data-stu-id="f5432-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="f5432-139">W **Konsola Menedżera pakietów**, wprowadź polecenie "update-database" Aby utworzyć bazę danych i uruchomić **inicjatora** metody.</span><span class="sxs-lookup"><span data-stu-id="f5432-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="f5432-140">Jeśli otrzymasz komunikat o błędzie wskazujący tabela już istnieje i nie można utworzyć, prawdopodobnie uruchomienia aplikacji, po usunięciu bazy danych oraz zanim zostanie wykonany `update-database`.</span><span class="sxs-lookup"><span data-stu-id="f5432-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="f5432-141">W takim przypadku usuń *Movies.mdf* ponownie plik, a następnie spróbuj ponownie `update-database` polecenia.</span><span class="sxs-lookup"><span data-stu-id="f5432-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="f5432-142">Jeśli nadal wystąpi błąd, usuń folder migracje i zawartość, a następnie uruchomić za pomocą instrukcji u góry tej strony (to delete *Movies.mdf* pliku, a następnie przejść do Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="f5432-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="f5432-143">Uruchom aplikację, a następnie przejdź do */Movies* adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f5432-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="f5432-144">Dane inicjatora są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="f5432-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="f5432-145">Dodawanie właściwości klasyfikacji do modelu Movie</span><span class="sxs-lookup"><span data-stu-id="f5432-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="f5432-146">Rozpocznij, dodając nowe `Rating` właściwości do istniejących `Movie` klasy.</span><span class="sxs-lookup"><span data-stu-id="f5432-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="f5432-147">Otwórz *Models\Movie.cs* pliku i Dodaj `Rating` właściwość podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="f5432-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="f5432-148">Pełne `Movie` klasy teraz wygląda podobnie do poniższego kodu:</span><span class="sxs-lookup"><span data-stu-id="f5432-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="f5432-149">Tworzenie aplikacji przy użyciu **kompilacji** &gt; **kompilacji filmu** menu polecenia lub przez naciśnięcie klawiszy CTRL-SHIFT-B.</span><span class="sxs-lookup"><span data-stu-id="f5432-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="f5432-150">Teraz, gdy użytkownik zaktualizował `Model` klasy, należy również zaktualizować *\Views\Movies\Index.cshtml* i *\Views\Movies\Create.cshtml* wyświetlać szablony, aby wyświetlić nowy `Rating`właściwości w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="f5432-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="f5432-151">Otwórz<em>\Views\Movies\Index.cshtml</em> pliku i Dodaj `<th>Rating</th>` nagłówek kolumny, tuż za <strong>cena</strong> kolumny.</span><span class="sxs-lookup"><span data-stu-id="f5432-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="f5432-152">Następnie dodaj `<td>` kolumny w końcowej części szablonu do renderowania `@item.Rating` wartość.</span><span class="sxs-lookup"><span data-stu-id="f5432-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="f5432-153">Poniżej przedstawiono, jakie zaktualizowane <em>Index.cshtml</em> Wyświetl szablon wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="f5432-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="f5432-154">Następnie otwórz *\Views\Movies\Create.cshtml* pliku i Dodaj następujący kod w końcowej części formularza.</span><span class="sxs-lookup"><span data-stu-id="f5432-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="f5432-155">Renderuje pola tekstowego, tak, aby można było określić klasyfikację, gdy zostanie utworzony nowy film.</span><span class="sxs-lookup"><span data-stu-id="f5432-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="f5432-156">Użytkownik zaktualizował teraz kod aplikacji do obsługi nowej `Rating` właściwości.</span><span class="sxs-lookup"><span data-stu-id="f5432-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="f5432-157">Teraz uruchom aplikację, a następnie przejdź do */Movies* adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f5432-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="f5432-158">Gdy to zrobisz, zobaczysz jedną z następujących błędów:</span><span class="sxs-lookup"><span data-stu-id="f5432-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="f5432-159">Widzisz ten błąd, ponieważ zaktualizowanego `Movie` klasy modelu w aplikacji teraz różni się od schematu `Movie` tabeli istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f5432-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="f5432-160">(Brak nie `Rating` kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="f5432-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="f5432-161">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="f5432-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="f5432-162">Ma automatycznie Porzuć i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f5432-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="f5432-163">To podejście jest bardzo wygodne w przypadku, gdy aktywne tworzenie aplikacji w bazie danych testu; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f5432-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="f5432-164">Wadą jednak jest utraty istniejących danych w bazie danych — dzięki czemu możesz *nie* chcesz użyć tej metody w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="f5432-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="f5432-165">Automatycznie zapełnić bazę danych przy użyciu danych testowych za pomocą inicjatora jest często produktywny sposób tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f5432-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="f5432-166">Aby uzyskać więcej informacji na temat inicjatory bazy danych programu Entity Framework, zobacz Tom Dykstra [samouczek platformy ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="f5432-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="f5432-167">Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="f5432-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="f5432-168">Zaletą tego podejścia jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="f5432-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="f5432-169">Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.</span><span class="sxs-lookup"><span data-stu-id="f5432-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="f5432-170">Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="f5432-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="f5432-171">W tym samouczku użyjemy migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="f5432-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="f5432-172">Seed — metoda należy zaktualizować tak, aby go oferuje wartości dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="f5432-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="f5432-173">Otwórz plik Migrations\Configuration.cs i dodać pole klasyfikacji do każdego obiektu filmu.</span><span class="sxs-lookup"><span data-stu-id="f5432-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="f5432-174">Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f5432-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="f5432-175">`add-migration` Polecenie informuje platformę migracji, aby zbadać bieżącej modelu movie z bieżącym schemacie filmu bazy danych i utworzyć niezbędny kod, aby przeprowadzić migrację bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="f5432-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="f5432-176">AddRatingMig jest dowolnego i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="f5432-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="f5432-177">Warto użyć znaczącą nazwę krok migracji.</span><span class="sxs-lookup"><span data-stu-id="f5432-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="f5432-178">Po zakończeniu tego polecenia, programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` klasy, a następnie w `Up` metody zostanie wyświetlony kod, który tworzy nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="f5432-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="f5432-179">Skompiluj rozwiązanie, a następnie wprowadź polecenie "update-database" w **Konsola Menedżera pakietów** okna.</span><span class="sxs-lookup"><span data-stu-id="f5432-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="f5432-180">Na poniższej ilustracji przedstawiono dane wyjściowe w **Konsola Menedżera pakietów** okna (sygnatura daty AS AddRatingMig będzie się różnić).</span><span class="sxs-lookup"><span data-stu-id="f5432-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="f5432-181">Uruchom ponownie aplikację, a następnie przejdź do adresu URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="f5432-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="f5432-182">Możesz zobaczyć nowe pole klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="f5432-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="f5432-183">Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy film.</span><span class="sxs-lookup"><span data-stu-id="f5432-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="f5432-184">Należy pamiętać, że można dodawać ocenę.</span><span class="sxs-lookup"><span data-stu-id="f5432-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="f5432-186">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="f5432-186">Click **Create**.</span></span> <span data-ttu-id="f5432-187">Ten nowy film, w tym klasyfikacji, wyświetlane w filmach, wyświetlanie listy:</span><span class="sxs-lookup"><span data-stu-id="f5432-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="f5432-189">Należy również dodać `Rating` pola edycji, szczegóły i SearchIndex Przeglądanie szablonów.</span><span class="sxs-lookup"><span data-stu-id="f5432-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="f5432-190">Można wprowadzić polecenie "update-database" w **Konsola Menedżera pakietów** ponownie okno i żadne zmiany nie nastąpi, ponieważ schemat jest zgodna z modelem.</span><span class="sxs-lookup"><span data-stu-id="f5432-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="f5432-191">W tej sekcji pokazano, jak można modyfikować obiekty modelu i synchronizację bazy danych przy użyciu zmian.</span><span class="sxs-lookup"><span data-stu-id="f5432-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="f5432-192">Przedstawiono również sposób wypełnić nowo utworzoną bazę danych z przykładowymi danymi, dzięki czemu możesz wypróbować scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="f5432-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="f5432-193">Następnie Przyjrzyjmy się jak dodać bogatsze logikę walidacji do klas modelu i włączyć niektóre reguły biznesowe zostaną wymuszone.</span><span class="sxs-lookup"><span data-stu-id="f5432-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f5432-194">[Poprzednie](examining-the-edit-methods-and-edit-view.md)
> [dalej](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="f5432-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
