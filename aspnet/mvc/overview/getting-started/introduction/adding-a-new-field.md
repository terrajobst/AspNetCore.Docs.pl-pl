---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Dodanie nowego pola | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 453fbf68aa2f3a1d9ea708355c06c53d4f1eabd0
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
<a name="adding-a-new-field"></a><span data-ttu-id="2c2e0-102">Dodanie nowego pola</span><span class="sxs-lookup"><span data-stu-id="2c2e0-102">Adding a New Field</span></span>
====================
<span data-ttu-id="2c2e0-103">przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2c2e0-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="2c2e0-104">W tej sekcji użyjesz migracje Code First Framework jednostki migrację pewne zmiany do klasy modeli, aby zmiana została zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="2c2e0-105">Domyślnie korzystając z programu Entity Framework Code First automatycznie utworzyć bazę danych, tak jak wcześniej w tym samouczku Code First dodaje tabeli do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="2c2e0-106">Jeśli nie są zsynchronizowane, Entity Framework zgłasza błąd.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="2c2e0-107">Ułatwia to śledzenie problemów w czasie opracowywania, znajdującego się w przeciwnym razie tylko (przez zasłoniętej błędy) w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="2c2e0-108">Konfigurowanie migracje Code First zmiany modelu</span><span class="sxs-lookup"><span data-stu-id="2c2e0-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="2c2e0-109">Przejdź do Eksploratora rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="2c2e0-110">Kliknij prawym przyciskiem myszy *Movies.mdf* plik i wybierz **usunąć** usuwania filmy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="2c2e0-111">Jeśli nie widzisz *Movies.mdf* plików, kliknij na **Pokaż wszystkie pliki** ikona przedstawionym poniżej na czerwone obramowanie.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="2c2e0-112">Tworzenie aplikacji, aby upewnić się, że nie ma żadnych błędów.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="2c2e0-113">Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet** , a następnie **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Dodaj pakiet Man](adding-a-new-field/_static/image2.png)

<span data-ttu-id="2c2e0-115">W **Konsola Menedżera pakietów** okno w `PM>` wierszu wprowadź</span><span class="sxs-lookup"><span data-stu-id="2c2e0-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="2c2e0-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="2c2e0-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="2c2e0-117">**Enable-Migrations** polecenie (pokazanym powyżej) tworzy *Configuration.cs* plik w nowym *migracje* folderu.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="2c2e0-118">Visual Studio otworzy *Configuration.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="2c2e0-119">Zastąp `Seed` metody w *Configuration.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2c2e0-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="2c2e0-120">Umieść kursor nad dowolnym kształcie czerwoną linię w obszarze `Movie` i kliknij przycisk `Show Potential Fixes` , a następnie kliknij przycisk **przy użyciu** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="2c2e0-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="2c2e0-121">Dzięki temu dodaje następujące instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="2c2e0-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="2c2e0-122">Kod wywoła pierwszy migracji `Seed` metody po każdym migracji (oznacza to, że wywołanie **update-database** w konsoli Menedżera pakietów), i ta metoda aktualizacji wierszy, które już zostały wstawione lub wstawia je, jeśli ich jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="2c2e0-123">[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody w poniższym kodzie wykonuje operację "upsert":</span><span class="sxs-lookup"><span data-stu-id="2c2e0-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="2c2e0-124">Ponieważ [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metody uruchamiany przy każdej migracji, po prostu nie można wstawić danych, ponieważ wierszy chcesz dodać już będą dostępne po pierwszej migracji, które utworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="2c2e0-125">"[Upsert](http://en.wikipedia.org/wiki/Upsert)" operację zapobiega błędom, które może się zdarzyć, jeśli podczas próby wstawienia wiersza, który już istnieje, ale zastępuje wszelkie zmiany danych, które mogły zostać wprowadzone podczas testowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="2c2e0-126">Z danych testowych w niektórych tabel nie można się zdarzyć, że: w niektórych przypadkach po zmianie danych podczas testowania ma zmiany po aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="2c2e0-127">W takim przypadku chcesz wykonać operację wstawiania warunkowe: Wstaw wiersz tylko wtedy, gdy jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
>   
> <span data-ttu-id="2c2e0-128">Pierwszy parametr przekazany do [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody Określa właściwość, można użyć do sprawdzenia, czy wiersz już istnieje.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="2c2e0-129">Dla danych filmu testowych, które udostępniasz `Title` właściwość może być używana w tym celu, ponieważ jest unikatowa w ramach tytułów poszczególnych na liście:</span><span class="sxs-lookup"><span data-stu-id="2c2e0-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="2c2e0-130">Ten kod przyjęto założenie, że tytuły są unikatowe.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="2c2e0-131">Jeśli ręcznie dodać zduplikowany tytuł, zostanie wyświetlony następujący wyjątek podczas następnego przeprowadzić migrację.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
>   
>  <span data-ttu-id="2c2e0-132">*Sekwencja zawiera więcej niż jeden element*</span><span class="sxs-lookup"><span data-stu-id="2c2e0-132">*Sequence contains more than one element*</span></span>  
>   
> <span data-ttu-id="2c2e0-133">Aby uzyskać więcej informacji na temat [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody, zobacz [zajmie się za pomocą metody AddOrUpdate EF 4.3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...</span><span class="sxs-lookup"><span data-stu-id="2c2e0-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>


<span data-ttu-id="2c2e0-134">**Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.** (Następujące kroki zakończy się niepowodzeniem w przypadku tworzenia nie w tym momencie.)</span><span class="sxs-lookup"><span data-stu-id="2c2e0-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="2c2e0-135">Następnym krokiem jest utworzenie `DbMigration` klasy początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="2c2e0-136">Ta migracja tworzy nową bazę danych, dlatego należy usunąć *movie.mdf* pliku w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="2c2e0-137">W **Konsola Menedżera pakietów** okna, wprowadź polecenie `add-migration Initial` do utworzenia początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="2c2e0-138">Nazwa "Początkowego" dowolnej i jest używany do nazywania plików migracji utworzony.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="2c2e0-139">Migracje Code First tworzy innego pliku klasy w *migracje* folderze (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="2c2e0-140">Aby pomóc w kolejności z sygnaturą czasową wstępnie ustala się filename migracji.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="2c2e0-141">Sprawdź *{DateStamp}\_Initial.cs* plik zawiera instrukcje dotyczące tworzenia `Movies` tabeli bazy danych filmu.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="2c2e0-142">Po zaktualizowaniu bazy danych w instrukcji poniżej, *{DateStamp}\_Initial.cs* pliku zostaną uruchomione i Utwórz schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="2c2e0-143">Następnie przy użyciu **inicjatora** metoda zostaną uruchomione, aby wypełnić bazę danych z danych testowych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="2c2e0-144">W **Konsola Menedżera pakietów**, wprowadź polecenie `update-database` do tworzenia bazy danych i uruchamiania `Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="2c2e0-145">Jeśli zostanie wyświetlony komunikat o błędzie wskazujący tabela już istnieje i nie można utworzyć, prawdopodobnie uruchomienia aplikacji, po usunięciu bazy danych i należy wykonać `update-database`.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="2c2e0-146">W takim przypadku usuń *Movies.mdf* ponownie, a następnie spróbuj ponownie `update-database` polecenia.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="2c2e0-147">Jeśli nadal występuje błąd, Usuń migrations folder i zawartość, a następnie uruchomić z instrukcjami w górnej części strony (który jest delete *Movies.mdf* pliku, a następnie przejdź do Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="2c2e0-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="2c2e0-148">Jeśli nadal eror, Otwórz Eksplorator obiektów SQL Server i usunąć bazę danych z listy.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-148">If you still get an eror, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="2c2e0-149">Uruchom aplikację i przejdź do */Movies* adresu URL.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="2c2e0-150">Są wyświetlane dane inicjatora.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="2c2e0-151">Dodawanie właściwości klasyfikacji do modelu film</span><span class="sxs-lookup"><span data-stu-id="2c2e0-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="2c2e0-152">Rozpocznij od dodania nowej `Rating` właściwości do istniejącej `Movie` klasy.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="2c2e0-153">Otwórz *Models\Movie.cs* plik i dodać `Rating` właściwości podobne do następującego:</span><span class="sxs-lookup"><span data-stu-id="2c2e0-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="2c2e0-154">Pełną `Movie` klasy teraz wygląda podobnie do następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2c2e0-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="2c2e0-155">Tworzenie aplikacji (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="2c2e0-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="2c2e0-156">Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania *białą listę* dzięki tej nowej właściwości zostaną uwzględnione.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="2c2e0-157">Aktualizacja `bind` atrybutu dla `Create` i `Edit` metod akcji, aby uwzględnić `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="2c2e0-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="2c2e0-158">Należy również zaktualizować szablony widok, aby wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="2c2e0-159">Otwórz *\Views\Movies\Index.cshtml* plik i dodać `<th>Rating</th>` tuż po nagłówek kolumny **cen** kolumny.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="2c2e0-160">Następnie dodaj `<td>` kolumny zbliża się koniec szablon do renderowania `@item.Rating` wartość.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="2c2e0-161">Poniżej znajduje się jakie zaktualizowane *Index.cshtml* Wyświetl szablon wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="2c2e0-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="2c2e0-162">Następnie otwórz folder *\Views\Movies\Create.cshtml* plik i dodać `Rating` pole z następujących znaczników highlighed.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighed markup.</span></span> <span data-ttu-id="2c2e0-163">Renderuje pola tekstowego, tak aby po utworzeniu nowego movie, można określić klasyfikację.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="2c2e0-164">Użytkownik zaktualizował teraz kodu aplikacji do obsługi nowej `Rating` właściwości.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="2c2e0-165">Uruchom aplikację i przejdź do */Movies* adresu URL.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="2c2e0-166">Po wykonaniu tej czynności, zostanie wyświetlony jeden z następujących błędów:</span><span class="sxs-lookup"><span data-stu-id="2c2e0-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="2c2e0-167">Model kopii kontekstu "MovieDBContext" została zmieniona od czasu utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="2c2e0-168">Należy rozważyć użycie migracje Code First można zaktualizować bazy danych (https://go.microsoft.com/fwlink/?LinkId=238269).</span><span class="sxs-lookup"><span data-stu-id="2c2e0-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="2c2e0-169">Ten błąd jest wyświetlane, ponieważ zaktualizowanego `Movie` klasy modelu w aplikacji teraz różni się od schematu `Movie` tabeli istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="2c2e0-170">(Brak nie `Rating` kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="2c2e0-170">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="2c2e0-171">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="2c2e0-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="2c2e0-172">Ma automatycznie porzucenia i ponownego utworzenia bazy danych na podstawie nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="2c2e0-173">Ta metoda jest bardzo wygodny wczesnym etapie cyklu programowanie podczas wprowadzania active Programowanie w testowej bazy danych; pozwala na szybkie razem rozwijać schematu modelu i bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="2c2e0-174">Wadą interfejsu, jest jednak, że utracić istniejące dane w bazie danych — dlatego możesz *nie* chcesz użyć tej metody w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="2c2e0-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="2c2e0-175">Automatycznie inicjatora bazy danych z danych testowych za pomocą inicjatora jest często produktywności możliwości opracowywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="2c2e0-176">Aby uzyskać więcej informacji o inicjatorach bazy danych programu Entity Framework, zobacz [samouczek platformy ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="2c2e0-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="2c2e0-177">Jawnie modyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="2c2e0-178">Zaletą tej metody jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="2c2e0-179">Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="2c2e0-180">Użyj migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-180">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="2c2e0-181">W tym samouczku użyjemy migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="2c2e0-182">Seed — metoda aktualizacji, dzięki czemu zapewnia wartość dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="2c2e0-183">Otwórz plik Migrations\Configuration.cs i Dodaj pole klasyfikacji do każdego obiektu filmu.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="2c2e0-184">Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2c2e0-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="2c2e0-185">`add-migration` Polecenie informuje framework migracji do badania bieżącego modelu film z bieżącego schematu filmu bazy danych i utworzyć niezbędne kod, aby przeprowadzić migrację bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="2c2e0-186">Nazwa *klasyfikacji* dowolnej i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="2c2e0-187">Warto użyć opisową nazwę krok migracji.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="2c2e0-188">Po zakończeniu działania tego polecenia programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMigration` klasy, a następnie w `Up` — metoda zostanie wyświetlony kod, który tworzy nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="2c2e0-189">Skompiluj rozwiązanie, a następnie wprowadź `update-database` w **Konsola Menedżera pakietów** okna.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="2c2e0-190">Na poniższej ilustracji przedstawiono dane wyjściowe w **Konsola Menedżera pakietów** okna (dołączanie sygnaturę daty *klasyfikacji* będą inne.)</span><span class="sxs-lookup"><span data-stu-id="2c2e0-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="2c2e0-191">Ponownie uruchom aplikację i przejdź do adresu URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="2c2e0-192">Widać nowe pole klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="2c2e0-193">Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy filmu.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="2c2e0-194">Należy pamiętać, że można dodać ocenę.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="2c2e0-196">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-196">Click **Create**.</span></span> <span data-ttu-id="2c2e0-197">Nowe film, w tym ocenę, teraz wyświetlone w filmy wyświetlania:</span><span class="sxs-lookup"><span data-stu-id="2c2e0-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="2c2e0-199">Teraz, gdy projekt używa migracji, nie musisz porzucić bazy danych podczas dodawania nowego pola lub w przeciwnym razie aktualizacji schematu.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="2c2e0-200">W następnej sekcji możemy wprowadzić dodatkowe zmiany schematu i użyj migracji do aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="2c2e0-201">Należy również dodać `Rating` pole do widoku szablonów edycji, szczegóły i Delete.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="2c2e0-202">Można wprowadzić polecenie "update-database" w **Konsola Menedżera pakietów** ponownie okno i żaden kod migracji może działać, ponieważ schemat zgodny z modelem.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="2c2e0-203">Jednak uruchomiona "update-database" zostanie uruchomiony `Seed` metody ponownie, i jeśli zmieniono żadnych danych inicjatora, zmiany zostaną utracone, ponieważ `Seed` metody upserts danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="2c2e0-204">Możesz przeczytać dodatkowe informacje `Seed` metody w Dykstra Tomasz popularnych [samouczek platformy ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="2c2e0-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="2c2e0-205">W tej sekcji przedstawiono sposób modyfikowania modelu obiektów i zachować synchronizację ze zmianami bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="2c2e0-206">Przedstawiono również sposób wypełnienia nowo utworzonej bazy danych z przykładowymi danymi, więc można wypróbować scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="2c2e0-207">To właśnie szybkie wprowadzenie do Code First, zobacz [tworzenia modelu danych struktury jednostek dla aplikacji platformy ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) bardziej szczegółowy samouczek na temat.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="2c2e0-208">Następnie Oto jak można dodać bardziej rozbudowane logikę weryfikacji dla klasy modelu i włączyć niektóre reguły biznesowe są wymuszane.</span><span class="sxs-lookup"><span data-stu-id="2c2e0-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2c2e0-209">[Poprzednie](adding-search.md)
[dalej](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="2c2e0-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
