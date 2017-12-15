---
title: Dodawanie modelu dla aplikacji platformy ASP.NET Core MVC
author: rick-anderson
description: Dodawanie modelu do prostej aplikacji platformy ASP.NET Core.
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/8/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 03c16e523fe2f91cae5c71357835684d813e3a1f
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="e905b-104">Uwaga: Szablony ASP.NET Core 2.0 zawierają *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="e905b-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="e905b-105">Kliknij prawym przyciskiem myszy *modele* folder > **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="e905b-105">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="e905b-106">Nazwa klasy **film** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="e905b-106">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="e905b-107">`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="e905b-107">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="e905b-108">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="e905b-108">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="e905b-109">Masz teraz **M**odelu w Twojej **M**VC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e905b-109">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="e905b-110">Tworzenia szkieletu kontrolera</span><span class="sxs-lookup"><span data-stu-id="e905b-110">Scaffolding a controller</span></span>

<span data-ttu-id="e905b-111">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="e905b-111">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller.png)

<span data-ttu-id="e905b-113">Jeśli **Dodaj zależności MVC** zostanie wyświetlone okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="e905b-113">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="e905b-114">[Aktualizowanie do najnowszej wersji programu Visual Studio](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="e905b-114">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="e905b-115">Visual Studio wersje poprzedzające 15,5 cala pokazuj tego okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="e905b-115">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="e905b-116">Jeśli nie można zaktualizować, wybierz **Dodaj**, a następnie ponownie wykonaj kroki kontrolera Dodaj.</span><span class="sxs-lookup"><span data-stu-id="e905b-116">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="e905b-117">W **Dodawanie szkieletu** okno dialogowe, naciśnij **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="e905b-117">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Dodawanie szkieletu okna dialogowego](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="e905b-119">Zakończenie **Dodaj kontroler** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="e905b-119">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="e905b-120">**Klasa modelu:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="e905b-120">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="e905b-121">**Klasa kontekstu danych:** wybierz  **+**  ikonę i Dodaj domyślny **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="e905b-121">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Dodaj kontekst danych](adding-model/_static/dc.png)

* <span data-ttu-id="e905b-123">**Widoki:** zachować zaznaczone domyślne każdej z nich</span><span class="sxs-lookup"><span data-stu-id="e905b-123">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="e905b-124">**Nazwa kontrolera:** Zachowaj ustawienie domyślne *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="e905b-124">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="e905b-125">Wybierz **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="e905b-125">Tap **Add**</span></span>

![Dodawanie kontrolera okna dialogowego](adding-model/_static/add_controller2.png)

<span data-ttu-id="e905b-127">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="e905b-127">Visual Studio creates:</span></span>

* <span data-ttu-id="e905b-128">Entity Framework Core [bazy danych klasy kontekstu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="e905b-128">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="e905b-129">Kontroler filmów (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="e905b-129">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="e905b-130">Pliki widoku razor dla stron Create, Delete, szczegóły, edycji i indeksu (*widoków/filmów/&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="e905b-130">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="e905b-131">Automatyczne tworzenie kontekstu bazy danych i [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki nosi nazwę *szkieletów*.</span><span class="sxs-lookup"><span data-stu-id="e905b-131">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="e905b-132">Konieczne będzie wkrótce aplikacji funkcjonalnej sieci web, która umożliwia zarządzanie filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e905b-132">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="e905b-133">Uruchom aplikację i kliknięcie na **Mvc Movie** łącza, wystąpi błąd podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="e905b-133">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="e905b-134">Należy utworzyć bazę danych i użyjesz EF Core [migracje](xref:data/ef-mvc/migrations) funkcji w tym celu.</span><span class="sxs-lookup"><span data-stu-id="e905b-134">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="e905b-135">Migracje umożliwia tworzenie bazy danych, która jest zgodna z modelem danych i zaktualizować schemat bazy danych, gdy model danych, zmiany.</span><span class="sxs-lookup"><span data-stu-id="e905b-135">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="e905b-136">Dodaj EF narzędzi i przeprowadzić migrację początkowej</span><span class="sxs-lookup"><span data-stu-id="e905b-136">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="e905b-137">W tej sekcji służą do konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="e905b-137">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="e905b-138">Dodaj pakiet Entity Framework podstawowe narzędzia.</span><span class="sxs-lookup"><span data-stu-id="e905b-138">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="e905b-139">Ten pakiet jest wymagany do dodawania migracji i aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e905b-139">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="e905b-140">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="e905b-140">Add an initial migration.</span></span>
* <span data-ttu-id="e905b-141">Aktualizacji bazy danych z początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="e905b-141">Update the database with the initial migration.</span></span>

<span data-ttu-id="e905b-142">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > konsoli Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="e905b-142">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC menu](adding-model/_static/pmc.png)

<span data-ttu-id="e905b-144">W kryterium wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e905b-144">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="e905b-145">**Uwaga:** Jeśli wystąpi błąd z `Install-Package` polecenia, otwórz Menedżera pakietów NuGet i wyszukaj `Microsoft.EntityFrameworkCore.Tools` pakietu.</span><span class="sxs-lookup"><span data-stu-id="e905b-145">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="e905b-146">Dzięki temu można zainstalować pakiet lub sprawdź, czy jest on już zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="e905b-146">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="e905b-147">Zobacz też [podejście CLI](#cli) Jeśli masz problemy z kryterium.</span><span class="sxs-lookup"><span data-stu-id="e905b-147">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="e905b-148">`Add-Migration` Polecenie tworzy kod w celu utworzenia schematu początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e905b-148">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="e905b-149">Schemat jest oparta na modelu określone w `DbContext`(w *Data/MvcMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="e905b-149">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="e905b-150">`Initial` Argument jest używany do nazywania migracji.</span><span class="sxs-lookup"><span data-stu-id="e905b-150">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="e905b-151">Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="e905b-151">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="e905b-152">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="e905b-152">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="e905b-153">`Update-Database` Polecenia `Up` metody w *migracje /\<sygnatury czasowej > _Initial.cs* pliku, który utworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e905b-153">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a><span data-ttu-id="e905b-154">Należy wykonać czynności poprzedzających przy użyciu interfejsu wiersza polecenia (CLI) zamiast PMC:</span><span class="sxs-lookup"><span data-stu-id="e905b-154">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="e905b-155">Dodaj [EF podstawowych narzędzi](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) do *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="e905b-155">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="e905b-156">Uruchom następujące polecenia z poziomu konsoli (w katalogu projektu):</span><span class="sxs-lookup"><span data-stu-id="e905b-156">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Menu kontekstowe IntelliSense w elemencie modelu listę dostępnych właściwości dla Identyfikatora, Price Data wydania i tytuł](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="e905b-158">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e905b-158">Additional resources</span></span>

* [<span data-ttu-id="e905b-159">Pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="e905b-159">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="e905b-160">Lokalizacja i globalizacja</span><span class="sxs-lookup"><span data-stu-id="e905b-160">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="e905b-161">[Dodawanie widoku poprzedniej](adding-view.md)
[obok Praca z SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="e905b-161">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
