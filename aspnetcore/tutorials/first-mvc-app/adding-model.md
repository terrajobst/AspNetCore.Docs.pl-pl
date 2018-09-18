---
title: Dodawanie modelu do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodawanie modelu do prostą aplikację platformy ASP.NET Core.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 5a820789ee3a761025d09aa78f3c42e59fc5fa38
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011381"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="f1796-103">Kliknij prawym przyciskiem myszy *modeli* folder > **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="f1796-103">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="f1796-104">Nazwa klasy **filmu** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="f1796-104">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="f1796-105">`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="f1796-105">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="f1796-106">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="f1796-106">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="f1796-107">Masz teraz **M**odelu w swojej **M**VC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1796-107">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="f1796-108">Tworzenia szkieletów kontrolera</span><span class="sxs-lookup"><span data-stu-id="f1796-108">Scaffolding a controller</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f1796-109">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > Nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="f1796-109">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

<span data-ttu-id="f1796-111">W **Dodawanie szkieletu** okno dialogowe, naciśnij **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="f1796-111">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Dodaj okno dialogowe Tworzenie szkieletu](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="f1796-113">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="f1796-113">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Widok powyżej kroku](adding-model/_static/add_controller.png)

<span data-ttu-id="f1796-115">Jeśli **Dodaj zależności MVC** zostanie wyświetlone okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="f1796-115">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="f1796-116">[Aktualizacja programu Visual Studio do najnowszej wersji](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f1796-116">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="f1796-117">Wersje serwera Visual Studio przed 15.5 pokazuj tego okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="f1796-117">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="f1796-118">Jeśli nie można zaktualizować wybierz **Dodaj**, a następnie ponownie wykonaj kroki kontrolera Dodaj.</span><span class="sxs-lookup"><span data-stu-id="f1796-118">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="f1796-119">W **Dodawanie szkieletu** okno dialogowe, naciśnij **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="f1796-119">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Dodaj okno dialogowe Tworzenie szkieletu](adding-model/_static/add_scaffold2.png)

::: moniker-end

<span data-ttu-id="f1796-121">Wykonaj **Dodaj kontroler** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="f1796-121">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="f1796-122">**Klasa modelu:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="f1796-122">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="f1796-123">**Klasa kontekstu danych:** wybierz **+** ikonę i Dodaj domyślny **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="f1796-123">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Dodawanie kontekstu danych](adding-model/_static/dc.png)

* <span data-ttu-id="f1796-125">**Widoki:** Zachowaj domyślne poszczególnych opcji zaznaczone</span><span class="sxs-lookup"><span data-stu-id="f1796-125">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="f1796-126">**Nazwa kontrolera:** Zachowaj ustawienie domyślne *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="f1796-126">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="f1796-127">Naciśnij pozycję **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="f1796-127">Tap **Add**</span></span>

![Dodaj kontroler, okno dialogowe](adding-model/_static/add_controller2.png)

<span data-ttu-id="f1796-129">Program Visual Studio tworzy:</span><span class="sxs-lookup"><span data-stu-id="f1796-129">Visual Studio creates:</span></span>

* <span data-ttu-id="f1796-130">Entity Framework Core [bazy danych klasy kontekstu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="f1796-130">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="f1796-131">Kontroler filmy (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="f1796-131">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="f1796-132">Pliki widoku razor dla stron Create, Delete, szczegółowe informacje, edycji i indeksu (<em>widoków/filmy/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="f1796-132">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="f1796-133">Automatyczne tworzenie kontekst bazy danych i [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki są określane jako *tworzenia szkieletów*.</span><span class="sxs-lookup"><span data-stu-id="f1796-133">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="f1796-134">Wkrótce będziesz mieć aplikację internetową w pełni funkcjonalne, która umożliwia zarządzanie filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f1796-134">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="f1796-135">Jeśli Uruchom aplikację i kliknąć **filmu Mvc** link, zostanie wyświetlony komunikat o błędzie podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="f1796-135">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="f1796-136">Musisz utworzyć bazę danych i użyjesz programu EF Core [migracje](xref:data/ef-mvc/migrations) funkcję, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="f1796-136">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="f1796-137">Migracje umożliwia tworzenie bazy danych, która pasuje do modelu danych i zaktualizować schemat bazy danych, gdy model danych, zmiany.</span><span class="sxs-lookup"><span data-stu-id="f1796-137">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="f1796-138">Dodaj EF narzędzi i wykonywania początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="f1796-138">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="f1796-139">W tej sekcji użyjesz konsoli Menedżera pakietów (PMC) do:</span><span class="sxs-lookup"><span data-stu-id="f1796-139">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="f1796-140">Dodaj pakiet narzędzi Entity Framework Core Tools.</span><span class="sxs-lookup"><span data-stu-id="f1796-140">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="f1796-141">Ten pakiet jest wymagany do dodawania migracje i aktualizują bazę danych.</span><span class="sxs-lookup"><span data-stu-id="f1796-141">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="f1796-142">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="f1796-142">Add an initial migration.</span></span>
* <span data-ttu-id="f1796-143">Zaktualizuj bazy danych przy użyciu początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="f1796-143">Update the database with the initial migration.</span></span>

<span data-ttu-id="f1796-144">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="f1796-144">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<span data-ttu-id="f1796-145"><!-- following image shared with uid: tutorials/razor-pages/model --> ![Menu konsoli zarządzania Pakietami](adding-model/_static/pmc.png)</span><span class="sxs-lookup"><span data-stu-id="f1796-145"><!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC menu](adding-model/_static/pmc.png)</span></span>

<span data-ttu-id="f1796-146">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="f1796-146">In the PMC, enter the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

``` PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="f1796-147">Ignoruj następujący komunikat o błędzie, możemy naprawić w następnym samouczku:</span><span class="sxs-lookup"><span data-stu-id="f1796-147">Ignore the following error message, we fix it in the next tutorial:</span></span>

<span data-ttu-id="f1796-148">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span><span class="sxs-lookup"><span data-stu-id="f1796-148">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span></span>  
      <span data-ttu-id="f1796-149">*Brak typu została określona dla dziesiętną kolumny "Cena" jednostki typu "Filmu". To spowoduje, że wartości, aby dyskretnie obcięty, jeśli nie mieszczą się w domyślnej dokładności i skali. Jawnie określić typ kolumny serwera SQL, która może pomieścić wszystkie wartości przy użyciu "ForHasColumnType()".*</span><span class="sxs-lookup"><span data-stu-id="f1796-149">*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="f1796-150">**Uwaga:** Jeśli otrzymasz komunikat o błędzie z `Install-Package` polecenia Otwórz Menedżera pakietów NuGet i wyszukiwania oraz `Microsoft.EntityFrameworkCore.Tools` pakietu.</span><span class="sxs-lookup"><span data-stu-id="f1796-150">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="f1796-151">Dzięki temu można zainstalować pakietu, lub sprawdź, czy jest on już zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="f1796-151">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="f1796-152">Możesz również zapoznać się [podejście interfejsu wiersza polecenia](#cli) Jeśli masz problemy z konsoli zarządzania Pakietami.</span><span class="sxs-lookup"><span data-stu-id="f1796-152">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

::: moniker-end

<span data-ttu-id="f1796-153">`Add-Migration` Polecenie tworzy kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f1796-153">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="f1796-154">Schemat jest oparta na modelu, określone w `DbContext`(w *Data/MvcMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="f1796-154">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="f1796-155">`Initial` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="f1796-155">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="f1796-156">Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="f1796-156">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="f1796-157">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="f1796-157">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="f1796-158">`Update-Database` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _Initial.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="f1796-158">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="f1796-159">Zamiast konsoli zarządzania Pakietami, należy wykonać czynności poprzedzających przy użyciu interfejsu wiersza polecenia (CLI):</span><span class="sxs-lookup"><span data-stu-id="f1796-159">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="f1796-160">Dodaj [narzędzi programu EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) do *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="f1796-160">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="f1796-161">Uruchom następujące polecenia z poziomu konsoli (w katalogu projektu):</span><span class="sxs-lookup"><span data-stu-id="f1796-161">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  <span data-ttu-id="f1796-162">Jeśli uruchamianie aplikacji, a komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="f1796-162">If you run the app and get the error:</span></span>

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="f1796-163">Prawdopodobnie nie uruchomiono `dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="f1796-163">You probably have not run `dotnet ef database update`.</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Menu kontekstowe funkcji IntelliSense dla elementu modelu, wyświetlanie listy dostępnych właściwości dla Identyfikatora, ceny, Data wydania i tytuł](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="f1796-165">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f1796-165">Additional resources</span></span>

* [<span data-ttu-id="f1796-166">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="f1796-166">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="f1796-167">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="f1796-167">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="f1796-168">[Poprzednie, Dodawanie widoku](adding-view.md)
> [obok pracy przy użyciu języka SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="f1796-168">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
