---
title: Dodawanie modelu dla aplikacji platformy ASP.NET Core MVC
author: rick-anderson
description: Dodawanie modelu do prostej aplikacji platformy ASP.NET Core.
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 7469546494ec54bfe36bc5bd2f5f9702889ddf4a
ms.sourcegitcommit: 2e61e287e220eddd5f3f4cd9147aa6417cfd9236
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="9f049-104">Uwaga: Szablony ASP.NET Core 2.0 zawierają *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="9f049-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="9f049-105">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **MvcMovie** projektu > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="9f049-105">In Solution Explorer, right click the **MvcMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="9f049-106">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="9f049-106">Name the folder *Models*.</span></span>

<span data-ttu-id="9f049-107">Kliknij prawym przyciskiem myszy *modele* folder > **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="9f049-107">Right click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="9f049-108">Nazwa klasy **film** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="9f049-108">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="9f049-109">`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="9f049-109">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="9f049-110">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="9f049-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="9f049-111">Masz teraz **M**odelu w Twojej **M**VC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f049-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="9f049-112">Tworzenia szkieletu kontrolera</span><span class="sxs-lookup"><span data-stu-id="9f049-112">Scaffolding a controller</span></span>

<span data-ttu-id="9f049-113">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="9f049-113">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller.png)

<span data-ttu-id="9f049-115">W **Dodaj zależności MVC** okno dialogowe, wybierz opcję **minimalnego zależności**i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="9f049-115">In the **Add MVC Dependencies** dialog, select **Minimal Dependencies**, and select **Add**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_depend.png)

<span data-ttu-id="9f049-117">Visual Studio dodaje zależności niezbędne do tworzenia szkieletu z kontrolerem, ale sam kontroler nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="9f049-117">Visual Studio adds the dependencies needed to scaffold a controller, but the controller itself is not created.</span></span> <span data-ttu-id="9f049-118">Następny invoke z **> Dodaj > kontrolera** tworzy kontroler.</span><span class="sxs-lookup"><span data-stu-id="9f049-118">The next invoke of **> Add > Controller** creates the controller.</span></span> 

<span data-ttu-id="9f049-119">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="9f049-119">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller.png)

<span data-ttu-id="9f049-121">W **Dodawanie szkieletu** okno dialogowe, naciśnij **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="9f049-121">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Dodawanie szkieletu okna dialogowego](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="9f049-123">Zakończenie **Dodaj kontroler** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="9f049-123">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="9f049-124">**Klasa modelu:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="9f049-124">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="9f049-125">**Klasa kontekstu danych:** wybierz  **+**  ikonę i Dodaj domyślny **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="9f049-125">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Dodaj kontekst danych](adding-model/_static/dc.png)

* <span data-ttu-id="9f049-127">**Widoki:** zachować zaznaczone domyślne każdej z nich</span><span class="sxs-lookup"><span data-stu-id="9f049-127">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="9f049-128">**Nazwa kontrolera:** Zachowaj ustawienie domyślne *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="9f049-128">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="9f049-129">Wybierz **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="9f049-129">Tap **Add**</span></span>

![Dodawanie kontrolera okna dialogowego](adding-model/_static/add_controller2.png)

<span data-ttu-id="9f049-131">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="9f049-131">Visual Studio creates:</span></span>

* <span data-ttu-id="9f049-132">Entity Framework Core [bazy danych klasy kontekstu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="9f049-132">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="9f049-133">Kontroler filmów (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="9f049-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="9f049-134">Pliki widoku razor dla stron Create, Delete, szczegóły, edycji i indeksu (*widoków/filmów/&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="9f049-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="9f049-135">Automatyczne tworzenie kontekstu bazy danych i [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki nosi nazwę *szkieletów*.</span><span class="sxs-lookup"><span data-stu-id="9f049-135">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="9f049-136">Konieczne będzie wkrótce aplikacji funkcjonalnej sieci web, która umożliwia zarządzanie filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9f049-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="9f049-137">Uruchom aplikację i kliknięcie na **Mvc Movie** link, zostanie wyświetlony błąd podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="9f049-137">If you run the app and click on the **Mvc Movie** link, you'll get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="9f049-138">Należy utworzyć bazę danych i użyjesz EF Core [migracje](xref:data/ef-mvc/migrations) funkcji w tym celu.</span><span class="sxs-lookup"><span data-stu-id="9f049-138">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="9f049-139">Migracje umożliwia tworzenie bazy danych, która jest zgodna z modelem danych i zaktualizować schemat bazy danych, gdy model danych, zmiany.</span><span class="sxs-lookup"><span data-stu-id="9f049-139">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="9f049-140">Dodaj EF narzędzi i przeprowadzić migrację początkowej</span><span class="sxs-lookup"><span data-stu-id="9f049-140">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="9f049-141">W tej sekcji służą do konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="9f049-141">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="9f049-142">Dodaj pakiet Entity Framework podstawowe narzędzia.</span><span class="sxs-lookup"><span data-stu-id="9f049-142">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="9f049-143">Ten pakiet jest wymagany do dodawania migracji i aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9f049-143">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="9f049-144">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="9f049-144">Add an initial migration.</span></span>
* <span data-ttu-id="9f049-145">Aktualizacji bazy danych z początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="9f049-145">Update the database with the initial migration.</span></span>

<span data-ttu-id="9f049-146">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > konsoli Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="9f049-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC menu](adding-model/_static/pmc.png)

<span data-ttu-id="9f049-148">W kryterium wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="9f049-148">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="9f049-149">**Uwaga:** Jeśli wystąpi błąd z `Install-Package` polecenia, otwórz Menedżera pakietów NuGet i wyszukaj `Microsoft.EntityFrameworkCore.Tools` pakietu.</span><span class="sxs-lookup"><span data-stu-id="9f049-149">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="9f049-150">Dzięki temu można zainstalować pakiet lub sprawdź, czy jest on już zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="9f049-150">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="9f049-151">Zobacz też [podejście CLI](#cli) Jeśli masz problemy z kryterium.</span><span class="sxs-lookup"><span data-stu-id="9f049-151">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="9f049-152">`Add-Migration` Polecenie tworzy kod w celu utworzenia schematu początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9f049-152">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="9f049-153">Schemat jest oparta na modelu określone w `DbContext`(w *Data/MvcMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="9f049-153">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="9f049-154">`Initial` Argument jest używany do nazywania migracji.</span><span class="sxs-lookup"><span data-stu-id="9f049-154">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="9f049-155">Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="9f049-155">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="9f049-156">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="9f049-156">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="9f049-157">`Update-Database` Polecenia `Up` metody w *migracje /\<sygnatury czasowej > _InitialCreate.cs* pliku, który utworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9f049-157">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="9f049-158"><a name="cli"></a>Należy wykonać czynności poprzedzających przy użyciu interfejsu wiersza polecenia (CLI) zamiast PMC:</span><span class="sxs-lookup"><span data-stu-id="9f049-158"><a name="cli"></a> You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="9f049-159">Dodaj [EF podstawowych narzędzi](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) do *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="9f049-159">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="9f049-160">Uruchom następujące polecenia z poziomu konsoli (w katalogu projektu):</span><span class="sxs-lookup"><span data-stu-id="9f049-160">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Menu kontekstowe IntelliSense w elemencie modelu listę dostępnych właściwości dla Identyfikatora, Price Data wydania i tytuł](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="9f049-162">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9f049-162">Additional resources</span></span>

* [<span data-ttu-id="9f049-163">Pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="9f049-163">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="9f049-164">Lokalizacja i globalizacja</span><span class="sxs-lookup"><span data-stu-id="9f049-164">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="9f049-165">[Dodawanie widoku poprzedniej](adding-view.md)
[obok Praca z SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="9f049-165">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
