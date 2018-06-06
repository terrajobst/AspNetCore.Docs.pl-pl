---
title: Dodaj model do aplikacji platformy ASP.NET Core MVC
author: rick-anderson
description: Dodawanie modelu do prostej aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/8/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 802cb458cb05579b97256022b56d6f97a03d2f1a
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687795"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="597be-103">Dodaj model do aplikacji platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="597be-103">Add a model to an ASP.NET Core MVC app</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="597be-104">Kliknij prawym przyciskiem myszy *modele* folder > **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="597be-104">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="597be-105">Nazwa klasy **film** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="597be-105">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="597be-106">`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="597be-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="597be-107">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="597be-107">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="597be-108">Masz teraz **M**odelu w Twojej **M**VC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="597be-108">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="597be-109">Tworzenia szkieletu kontrolera</span><span class="sxs-lookup"><span data-stu-id="597be-109">Scaffolding a controller</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="597be-110">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > Nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="597be-110">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

<span data-ttu-id="597be-112">W **Dodawanie szkieletu** okno dialogowe, naciśnij **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="597be-112">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Dodawanie szkieletu okna dialogowego](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="597be-114">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="597be-114">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller.png)

<span data-ttu-id="597be-116">Jeśli **Dodaj zależności MVC** zostanie wyświetlone okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="597be-116">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="597be-117">[Aktualizowanie do najnowszej wersji programu Visual Studio](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="597be-117">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="597be-118">Visual Studio wersje poprzedzające 15,5 cala pokazuj tego okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="597be-118">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="597be-119">Jeśli nie można zaktualizować, wybierz **Dodaj**, a następnie ponownie wykonaj kroki kontrolera Dodaj.</span><span class="sxs-lookup"><span data-stu-id="597be-119">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="597be-120">W **Dodawanie szkieletu** okno dialogowe, naciśnij **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="597be-120">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Dodawanie szkieletu okna dialogowego](adding-model/_static/add_scaffold2.png)

::: moniker-end

<span data-ttu-id="597be-122">Zakończenie **Dodaj kontroler** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="597be-122">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="597be-123">**Klasa modelu:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="597be-123">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="597be-124">**Klasa kontekstu danych:** wybierz **+** ikonę i Dodaj domyślny **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="597be-124">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Dodaj kontekst danych](adding-model/_static/dc.png)

* <span data-ttu-id="597be-126">**Widoki:** zachować zaznaczone domyślne każdej z nich</span><span class="sxs-lookup"><span data-stu-id="597be-126">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="597be-127">**Nazwa kontrolera:** Zachowaj ustawienie domyślne *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="597be-127">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="597be-128">Wybierz **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="597be-128">Tap **Add**</span></span>

![Dodawanie kontrolera okna dialogowego](adding-model/_static/add_controller2.png)

<span data-ttu-id="597be-130">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="597be-130">Visual Studio creates:</span></span>

* <span data-ttu-id="597be-131">Entity Framework Core [bazy danych klasy kontekstu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="597be-131">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="597be-132">Kontroler filmów (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="597be-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="597be-133">Pliki widoku razor dla stron Create, Delete, szczegóły, edycji i indeksu (<em>widoków/filmów/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="597be-133">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="597be-134">Automatyczne tworzenie kontekstu bazy danych i [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki nosi nazwę *szkieletów*.</span><span class="sxs-lookup"><span data-stu-id="597be-134">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="597be-135">Konieczne będzie wkrótce aplikacji funkcjonalnej sieci web, która umożliwia zarządzanie filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="597be-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="597be-136">Uruchom aplikację i kliknięcie na **Mvc Movie** łącza, wystąpi błąd podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="597be-136">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="597be-137">Należy utworzyć bazę danych i użyjesz EF Core [migracje](xref:data/ef-mvc/migrations) funkcji w tym celu.</span><span class="sxs-lookup"><span data-stu-id="597be-137">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="597be-138">Migracje umożliwia tworzenie bazy danych, która jest zgodna z modelem danych i zaktualizować schemat bazy danych, gdy model danych, zmiany.</span><span class="sxs-lookup"><span data-stu-id="597be-138">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="597be-139">Dodaj EF narzędzi i przeprowadzić migrację początkowej</span><span class="sxs-lookup"><span data-stu-id="597be-139">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="597be-140">W tej sekcji służą do konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="597be-140">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="597be-141">Dodaj pakiet Entity Framework podstawowe narzędzia.</span><span class="sxs-lookup"><span data-stu-id="597be-141">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="597be-142">Ten pakiet jest wymagany do dodawania migracji i aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="597be-142">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="597be-143">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="597be-143">Add an initial migration.</span></span>
* <span data-ttu-id="597be-144">Aktualizacji bazy danych z początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="597be-144">Update the database with the initial migration.</span></span>

<span data-ttu-id="597be-145">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > konsoli Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="597be-145">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC menu](adding-model/_static/pmc.png)

<span data-ttu-id="597be-147">W kryterium wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="597be-147">In the PMC, enter the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="597be-148">Ignoruj następujący komunikat o błędzie, możemy naprawić w następnym samouczku:</span><span class="sxs-lookup"><span data-stu-id="597be-148">Ignore the following error message, we fix it in the next tutorial:</span></span>

<span data-ttu-id="597be-149">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span><span class="sxs-lookup"><span data-stu-id="597be-149">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span></span>  
      <span data-ttu-id="597be-150">*Typ nie został określony dla kolumny dziesiętną "Cena" na typ jednostki "Filmu". Spowoduje to wartości, aby dyskretnie obcięty, jeśli nie mieszczą się w domyślnej precyzję i skalę. Jawnie określić typ kolumny serwera SQL, która może obsłużyć wszystkie wartości przy użyciu "ForHasColumnType()".*</span><span class="sxs-lookup"><span data-stu-id="597be-150">*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="597be-151">**Uwaga:** Jeśli wystąpi błąd z `Install-Package` polecenia, otwórz Menedżera pakietów NuGet i wyszukaj `Microsoft.EntityFrameworkCore.Tools` pakietu.</span><span class="sxs-lookup"><span data-stu-id="597be-151">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="597be-152">Dzięki temu można zainstalować pakiet lub sprawdź, czy jest on już zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="597be-152">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="597be-153">Zobacz też [podejście CLI](#cli) Jeśli masz problemy z kryterium.</span><span class="sxs-lookup"><span data-stu-id="597be-153">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

::: moniker-end

<span data-ttu-id="597be-154">`Add-Migration` Polecenie tworzy kod w celu utworzenia schematu początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="597be-154">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="597be-155">Schemat jest oparta na modelu określone w `DbContext`(w *Data/MvcMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="597be-155">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="597be-156">`Initial` Argument jest używany do nazywania migracji.</span><span class="sxs-lookup"><span data-stu-id="597be-156">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="597be-157">Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="597be-157">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="597be-158">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="597be-158">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="597be-159">`Update-Database` Polecenia `Up` metody w *migracje /\<sygnatury czasowej > _Initial.cs* pliku, który utworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="597be-159">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="597be-160">Należy wykonać czynności poprzedzających przy użyciu interfejsu wiersza polecenia (CLI) zamiast PMC:</span><span class="sxs-lookup"><span data-stu-id="597be-160">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="597be-161">Dodaj [EF podstawowych narzędzi](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) do *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="597be-161">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="597be-162">Uruchom następujące polecenia z poziomu konsoli (w katalogu projektu):</span><span class="sxs-lookup"><span data-stu-id="597be-162">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  <span data-ttu-id="597be-163">Jeżeli możesz uruchomić aplikację i komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="597be-163">If you run the app and get the error:</span></span>

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="597be-164">Prawdopodobnie nie uruchomiono `dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="597be-164">You probably have not run `dotnet ef database update`.</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="597be-165">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span><span class="sxs-lookup"><span data-stu-id="597be-165">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="597be-166">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="597be-166">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Menu kontekstowe IntelliSense w elemencie modelu listę dostępnych właściwości dla Identyfikatora, Price Data wydania i tytuł](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="597be-168">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="597be-168">Additional resources</span></span>

* [<span data-ttu-id="597be-169">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="597be-169">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="597be-170">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="597be-170">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="597be-171">[Dodawanie widoku poprzedniej](adding-view.md)
> [obok Praca z SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="597be-171">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
