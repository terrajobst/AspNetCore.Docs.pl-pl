---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Dodawanie nowego pola | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: a4eb759646fc77bbba687d8b4914465d58cd3aaa
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38151460"
---
<a name="adding-a-new-field"></a><span data-ttu-id="3a247-102">Dodawanie nowego pola</span><span class="sxs-lookup"><span data-stu-id="3a247-102">Adding a New Field</span></span>
====================
<span data-ttu-id="3a247-103">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="3a247-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="3a247-104">W tej sekcji użyjesz migracje Code First Framework jednostki migracji pewne zmiany do klasy modelu, aby zmiana została zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3a247-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="3a247-105">Domyślnie gdy używasz programu Entity Framework Code First automatycznie utworzyć bazę danych, tak jak wcześniej w tym samouczku rozwiązanie Code First dodaje tabelę do bazy danych, aby śledzić, czy schemat bazy danych jest zsynchronizowany z klasy modelu, który został wygenerowany z.</span><span class="sxs-lookup"><span data-stu-id="3a247-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="3a247-106">Jeśli nie są zsynchronizowane, platformy Entity Framework zgłasza błąd.</span><span class="sxs-lookup"><span data-stu-id="3a247-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="3a247-107">Ta funkcja ułatwia śledzenie problemów w czasie programowania, które mogą w przeciwnym razie tylko się okazać (przez zasłoniętej błędy) w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="3a247-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="3a247-108">Konfigurowanie funkcji migracje Code First dla zmiany modelu</span><span class="sxs-lookup"><span data-stu-id="3a247-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="3a247-109">Przejdź do Eksploratora rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="3a247-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="3a247-110">Kliknij prawym przyciskiem myszy *Movies.mdf* plik i wybierz **Usuń** można usunąć bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="3a247-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="3a247-111">Jeśli nie widzisz *Movies.mdf* plików, kliknij na **Pokaż wszystkie pliki** ikonę przedstawionym poniżej na czerwone obramowanie.</span><span class="sxs-lookup"><span data-stu-id="3a247-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="3a247-112">Utwórz aplikację, aby upewnić się, że nie ma żadnych błędów.</span><span class="sxs-lookup"><span data-stu-id="3a247-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="3a247-113">Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet** i następnie **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="3a247-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Dodaj pakiet Man](adding-a-new-field/_static/image2.png)

<span data-ttu-id="3a247-115">W **Konsola Menedżera pakietów** okna w `PM>` wierszu wprowadź</span><span class="sxs-lookup"><span data-stu-id="3a247-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="3a247-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="3a247-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="3a247-117">**Enable-Migrations** polecenie (jak pokazano powyżej) umożliwia utworzenie *Configuration.cs* plik w nowym *migracje* folderu.</span><span class="sxs-lookup"><span data-stu-id="3a247-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="3a247-118">Zostanie otwarty program Visual Studio *Configuration.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="3a247-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="3a247-119">Zastąp `Seed` method in Class metoda *Configuration.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="3a247-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="3a247-120">Umieść kursor nad czerwona linia falista pod `Movie` i kliknij przycisk `Show Potential Fixes` a następnie kliknij przycisk **przy użyciu** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="3a247-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="3a247-121">Spowoduje to dodanie następujących instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="3a247-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="3a247-122">Kod wywołuje metodę migracji First `Seed` metody po każdej migracji (oznacza to, że wywołanie **update-database** w konsoli Menedżera pakietów), i ta metoda aktualizuje wierszy, które już zostały wstawione lub wstawia je, jeśli ich jeszcze nie istnieją.</span><span class="sxs-lookup"><span data-stu-id="3a247-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="3a247-123">[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metoda poniższy kod wykonuje operację "upsert":</span><span class="sxs-lookup"><span data-stu-id="3a247-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="3a247-124">Ponieważ [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda uruchamia się za pomocą każdej migracji, po prostu nie można wstawić danych, ponieważ wierszy próbujesz dodać będą już dostępne po pierwszej migracji, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="3a247-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="3a247-125">"[Upsert](http://en.wikipedia.org/wiki/Upsert)" operacja zapobiega błędom, które może się zdarzyć, jeśli podczas próby wstawienia wiersza, który już istnieje, lecz zastępuje ona wszelkie zmiany w danych, które mogły zostać wprowadzone podczas testowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3a247-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="3a247-126">Zawierającej dane testowe w niektórych tabel nie można było to możliwe: w niektórych przypadkach po zmianie danych podczas testowania mają zmiany po aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3a247-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="3a247-127">W takim przypadku chcesz zrobić to operacja wstawiania warunkowego: Wstaw wiersz, tylko wtedy, gdy jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="3a247-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="3a247-128">Pierwszy parametr przekazany do [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody Określa właściwość, można użyć, aby sprawdzić, czy wiersz już istnieje.</span><span class="sxs-lookup"><span data-stu-id="3a247-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="3a247-129">Dla danych filmu testów, które są podawane `Title` właściwość może być używana w tym celu, ponieważ w ramach tytułów poszczególnych na liście jest unikatowa:</span><span class="sxs-lookup"><span data-stu-id="3a247-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="3a247-130">Ten kod zakłada, że tytuły są unikatowe.</span><span class="sxs-lookup"><span data-stu-id="3a247-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="3a247-131">Jeśli ręcznie dodasz zduplikowane tytuł, otrzymasz następujący wyjątek podczas następnego przeprowadzić migrację.</span><span class="sxs-lookup"><span data-stu-id="3a247-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
>  <span data-ttu-id="3a247-132">*Sekwencja zawiera więcej niż jeden element*</span><span class="sxs-lookup"><span data-stu-id="3a247-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="3a247-133">Aby uzyskać więcej informacji na temat [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody, zobacz [powinien zachować ostrożność przy użyciu metody AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...</span><span class="sxs-lookup"><span data-stu-id="3a247-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>


<span data-ttu-id="3a247-134">**Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.** (Następujących kroków zakończy się niepowodzeniem, jeśli nie utworzysz w tym momencie.)</span><span class="sxs-lookup"><span data-stu-id="3a247-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="3a247-135">Następnym krokiem jest utworzenie `DbMigration` klasy początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="3a247-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="3a247-136">Tej migracji tworzy nową bazę danych, dlatego możesz usunąć *movie.mdf* pliku w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="3a247-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="3a247-137">W **Konsola Menedżera pakietów** okna, wprowadź polecenie `add-migration Initial` do utworzenia początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="3a247-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="3a247-138">Nazwa "Początkowego" dowolnej i jest używany do nazywania plików migracji utworzone.</span><span class="sxs-lookup"><span data-stu-id="3a247-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="3a247-139">Migracje Code First tworzy inny plik klasy w *migracje* folderze (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3a247-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="3a247-140">Nazwa pliku migracji jest wstępnie stała z sygnaturą czasową pomagające w kolejności.</span><span class="sxs-lookup"><span data-stu-id="3a247-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="3a247-141">Sprawdź *{DateStamp}\_Initial.cs* plik zawiera instrukcje dotyczące tworzenia `Movies` tabeli bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="3a247-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="3a247-142">Kiedy aktualizować bazę danych w instrukcjach poniżej to *{DateStamp}\_Initial.cs* pliku zostaną uruchomione i Utwórz schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3a247-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="3a247-143">A następnie **inicjatora** metoda zostanie uruchomiony do wypełniania bazy danych z danymi.</span><span class="sxs-lookup"><span data-stu-id="3a247-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="3a247-144">W **Konsola Menedżera pakietów**, wprowadź polecenie `update-database` do tworzenia bazy danych i uruchamiania `Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="3a247-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="3a247-145">Jeśli otrzymasz komunikat o błędzie wskazujący tabela już istnieje i nie można utworzyć, prawdopodobnie uruchomienia aplikacji, po usunięciu bazy danych oraz zanim zostanie wykonany `update-database`.</span><span class="sxs-lookup"><span data-stu-id="3a247-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="3a247-146">W takim przypadku usuń *Movies.mdf* ponownie plik, a następnie spróbuj ponownie `update-database` polecenia.</span><span class="sxs-lookup"><span data-stu-id="3a247-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="3a247-147">Jeśli nadal wystąpi błąd, usuń folder migracje i zawartość, a następnie uruchomić za pomocą instrukcji u góry tej strony (to delete *Movies.mdf* pliku, a następnie przejść do Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="3a247-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="3a247-148">Jeśli nadal występują Twoja nazwavstutorialfactory, Otwórz Eksplorator obiektów SQL Server, a następnie usunąć bazę danych z listy.</span><span class="sxs-lookup"><span data-stu-id="3a247-148">If you still get an eror, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="3a247-149">Uruchom aplikację, a następnie przejdź do */Movies* adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3a247-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="3a247-150">Dane inicjatora są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="3a247-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="3a247-151">Dodawanie właściwości klasyfikacji do modelu Movie</span><span class="sxs-lookup"><span data-stu-id="3a247-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="3a247-152">Rozpocznij, dodając nowe `Rating` właściwości do istniejących `Movie` klasy.</span><span class="sxs-lookup"><span data-stu-id="3a247-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="3a247-153">Otwórz *Models\Movie.cs* pliku i Dodaj `Rating` właściwość podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="3a247-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="3a247-154">Pełne `Movie` klasy teraz wygląda podobnie do poniższego kodu:</span><span class="sxs-lookup"><span data-stu-id="3a247-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="3a247-155">Utwórz aplikację (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="3a247-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="3a247-156">Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania *białą listę* dzięki tej nowej właściwości zostaną dołączone.</span><span class="sxs-lookup"><span data-stu-id="3a247-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="3a247-157">Aktualizacja `bind` atrybutu dla `Create` i `Edit` metody akcji, aby uwzględnić `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="3a247-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="3a247-158">Należy również zaktualizować Przeglądanie szablonów, aby można było wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3a247-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="3a247-159">Otwórz *\Views\Movies\Index.cshtml* pliku i Dodaj `<th>Rating</th>` nagłówek kolumny, tuż za **cena** kolumny.</span><span class="sxs-lookup"><span data-stu-id="3a247-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="3a247-160">Następnie dodaj `<td>` kolumny w końcowej części szablonu do renderowania `@item.Rating` wartość.</span><span class="sxs-lookup"><span data-stu-id="3a247-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="3a247-161">Poniżej przedstawiono, jakie zaktualizowane *Index.cshtml* Wyświetl szablon wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="3a247-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="3a247-162">Następnie otwórz *\Views\Movies\Create.cshtml* pliku i Dodaj `Rating` pole za pomocą następujących znaczników highlighed.</span><span class="sxs-lookup"><span data-stu-id="3a247-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighed markup.</span></span> <span data-ttu-id="3a247-163">Renderuje pola tekstowego, tak, aby można było określić klasyfikację, gdy zostanie utworzony nowy film.</span><span class="sxs-lookup"><span data-stu-id="3a247-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="3a247-164">Użytkownik zaktualizował teraz kod aplikacji do obsługi nowej `Rating` właściwości.</span><span class="sxs-lookup"><span data-stu-id="3a247-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="3a247-165">Uruchom aplikację, a następnie przejdź do */Movies* adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3a247-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="3a247-166">Gdy to zrobisz, zobaczysz jedną z następujących błędów:</span><span class="sxs-lookup"><span data-stu-id="3a247-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="3a247-167">Model kopii kontekstu "MovieDBContext" została zmieniona od czasu utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3a247-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="3a247-168">Należy rozważyć użycie migracje Code First w aktualizacji bazy danych (https://go.microsoft.com/fwlink/?LinkId=238269).</span><span class="sxs-lookup"><span data-stu-id="3a247-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="3a247-169">Widzisz ten błąd, ponieważ zaktualizowanego `Movie` klasy modelu w aplikacji teraz różni się od schematu `Movie` tabeli istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3a247-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="3a247-170">(Brak nie `Rating` kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="3a247-170">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="3a247-171">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="3a247-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="3a247-172">Ma automatycznie Porzuć i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3a247-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="3a247-173">To podejście jest bardzo wygodne na wczesnym etapie cyklu tworzenia oprogramowania, gdy robią active rozwoju w bazie danych testu; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3a247-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="3a247-174">Wadą jednak jest utraty istniejących danych w bazie danych — dzięki czemu możesz *nie* chcesz użyć tej metody w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="3a247-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="3a247-175">Automatycznie zapełnić bazę danych przy użyciu danych testowych za pomocą inicjatora jest często produktywny sposób tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3a247-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="3a247-176">Aby uzyskać więcej informacji na temat inicjatory bazy danych programu Entity Framework, zobacz [samouczek platformy ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="3a247-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="3a247-177">Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="3a247-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="3a247-178">Zaletą tego podejścia jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="3a247-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="3a247-179">Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.</span><span class="sxs-lookup"><span data-stu-id="3a247-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="3a247-180">Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="3a247-180">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="3a247-181">W tym samouczku użyjemy migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="3a247-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="3a247-182">Seed — metoda należy zaktualizować tak, aby go oferuje wartości dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="3a247-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="3a247-183">Otwórz plik Migrations\Configuration.cs i dodać pole klasyfikacji do każdego obiektu filmu.</span><span class="sxs-lookup"><span data-stu-id="3a247-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="3a247-184">Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3a247-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="3a247-185">`add-migration` Polecenie informuje platformę migracji, aby zbadać bieżącej modelu movie z bieżącym schemacie filmu bazy danych i utworzyć niezbędny kod, aby przeprowadzić migrację bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="3a247-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="3a247-186">Nazwa *ocena* dowolnej i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="3a247-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="3a247-187">Warto użyć znaczącą nazwę krok migracji.</span><span class="sxs-lookup"><span data-stu-id="3a247-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="3a247-188">Po zakończeniu tego polecenia, programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMigration` klasy, a następnie w `Up` metody zostanie wyświetlony kod, który tworzy nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="3a247-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="3a247-189">Skompiluj rozwiązanie, a następnie wprowadź `update-database` polecenia w pliku **Konsola Menedżera pakietów** okna.</span><span class="sxs-lookup"><span data-stu-id="3a247-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="3a247-190">Na poniższej ilustracji przedstawiono dane wyjściowe w **Konsola Menedżera pakietów** okna (dołączenie sygnatura daty *ocena* będzie się różnił.)</span><span class="sxs-lookup"><span data-stu-id="3a247-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="3a247-191">Uruchom ponownie aplikację, a następnie przejdź do adresu URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="3a247-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="3a247-192">Możesz zobaczyć nowe pole klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="3a247-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="3a247-193">Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy film.</span><span class="sxs-lookup"><span data-stu-id="3a247-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="3a247-194">Należy pamiętać, że można dodawać ocenę.</span><span class="sxs-lookup"><span data-stu-id="3a247-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="3a247-196">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="3a247-196">Click **Create**.</span></span> <span data-ttu-id="3a247-197">Ten nowy film, w tym klasyfikacji, wyświetlane w filmach, wyświetlanie listy:</span><span class="sxs-lookup"><span data-stu-id="3a247-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="3a247-199">Teraz, gdy projekt korzysta z migracji, nie musisz porzucić bazy danych, podczas dodawania nowego pola, lub w przeciwnym razie zaktualizowania schematu.</span><span class="sxs-lookup"><span data-stu-id="3a247-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="3a247-200">W następnej sekcji możemy wprowadzić więcej zmian schematu i użyć migracje aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3a247-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="3a247-201">Należy również dodać `Rating` pole Przeglądanie szablonów Edytuj, szczegóły i Delete.</span><span class="sxs-lookup"><span data-stu-id="3a247-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="3a247-202">Można wprowadzić polecenie "update-database" w **Konsola Menedżera pakietów** ponownie okno i nie ma kodu migracji uruchomić, ponieważ schemat jest zgodna z modelem.</span><span class="sxs-lookup"><span data-stu-id="3a247-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="3a247-203">Jednak uruchomiona "Aktualizacja bazy danych" zostanie uruchomiony `Seed` metoda ponownie, i jeśli zmienisz jakiekolwiek danych inicjatora, zmiany zostaną utracone, ponieważ `Seed` danych wykonuje operację UPSERT metody.</span><span class="sxs-lookup"><span data-stu-id="3a247-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="3a247-204">Przeczytaj więcej o `Seed` method in Class metoda Tom Dykstra popularnych [samouczek platformy ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="3a247-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="3a247-205">W tej sekcji pokazano, jak można modyfikować obiekty modelu i synchronizację bazy danych przy użyciu zmian.</span><span class="sxs-lookup"><span data-stu-id="3a247-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="3a247-206">Przedstawiono również sposób wypełnić nowo utworzoną bazę danych z przykładowymi danymi, dzięki czemu możesz wypróbować scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="3a247-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="3a247-207">Jest to tylko krótkie wprowadzenie do Code First, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) bardziej szczegółowy samouczek na temat.</span><span class="sxs-lookup"><span data-stu-id="3a247-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="3a247-208">Następnie Przyjrzyjmy się jak dodać bogatsze logikę walidacji do klas modelu i włączyć niektóre reguły biznesowe zostaną wymuszone.</span><span class="sxs-lookup"><span data-stu-id="3a247-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3a247-209">[Poprzednie](adding-search.md)
> [dalej](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="3a247-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
