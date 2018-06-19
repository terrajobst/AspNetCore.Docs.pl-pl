---
title: Dodawanie modelu do aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac
author: rick-anderson
description: Dodawanie modelu do prostej aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.devlang: csharp
ms.prod: .net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 6792dbc7c9ab063d85c0c4145481b8fd6b40da63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899528"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app-with-visual-studio-for-mac"></a><span data-ttu-id="fab28-103">Dodawanie modelu do aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="fab28-103">Add a model to an ASP.NET Core MVC app with Visual Studio for Mac</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="fab28-104">Kliknij prawym przyciskiem myszy *modele* folder, a następnie wybierz **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="fab28-104">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="fab28-105">W **nowy plik** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="fab28-105">In the **New File** dialog:</span></span>

  * <span data-ttu-id="fab28-106">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="fab28-106">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="fab28-107">Wybierz **pustą klasę** w słabe center.</span><span class="sxs-lookup"><span data-stu-id="fab28-107">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="fab28-108">Nazwa klasy **film** i wybierz **nowy**.</span><span class="sxs-lookup"><span data-stu-id="fab28-108">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="fab28-109">Dodaj następujące właściwości `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="fab28-109">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="fab28-110">`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="fab28-110">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="fab28-111">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="fab28-111">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="fab28-112">Masz teraz **M**odelu w Twojej **M**VC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fab28-112">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="fab28-113">Przygotowanie projektu do tworzenia szkieletów</span><span class="sxs-lookup"><span data-stu-id="fab28-113">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="fab28-114">Kliknij prawym przyciskiem myszy plik projektu, a następnie wybierz **Narzędzia > Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="fab28-114">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![Widok powyżej kroku](adding-model/_static/1.png)

- <span data-ttu-id="fab28-116">Dodaj następujące wyróżnione pakietów NuGet do *MvcMovie.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="fab28-116">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="fab28-117">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="fab28-117">Save the file.</span></span>

- <span data-ttu-id="fab28-118">Utwórz *Models/MvcMovieContext.cs* pliku i dodaj następującą `MvcMovieContext` klasy:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="fab28-118">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="fab28-119">Otwórz *Startup.cs* plik i dodać dwie deklaracje Using:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="fab28-119">Open the *Startup.cs* file and add two usings:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="fab28-120">Kontekst bazy danych, aby dodać *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="fab28-120">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="fab28-121">Ta wartość informuje klas modelu, które znajdują się w modelu danych programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fab28-121">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="fab28-122">Definiujesz co *zestaw jednostek* filmu obiektów, które będą reprezentowane w bazie danych jako tabelę filmu.</span><span class="sxs-lookup"><span data-stu-id="fab28-122">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="fab28-123">Skompiluj projekt, aby sprawdzić, czy nie ma żadnych błędów.</span><span class="sxs-lookup"><span data-stu-id="fab28-123">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="fab28-124">Tworzenie szkieletu MovieController</span><span class="sxs-lookup"><span data-stu-id="fab28-124">Scaffold the MovieController</span></span>

<span data-ttu-id="fab28-125">Otwórz okno terminala w folderze projektu i uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="fab28-125">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="fab28-126">Jeśli zostanie wyświetlony błąd `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span><span class="sxs-lookup"><span data-stu-id="fab28-126">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="fab28-127">Znajdujesz się w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="fab28-127">You are in the project directory.</span></span> <span data-ttu-id="fab28-128">Katalog projektu ma *Program.cs*, *Startup.cs* i *.csproj* plików.</span><span class="sxs-lookup"><span data-stu-id="fab28-128">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="fab28-129">Używana wersja dotnet jest 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="fab28-129">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="fab28-130">Uruchom `dotnet` Aby pobrać wersję.</span><span class="sxs-lookup"><span data-stu-id="fab28-130">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="fab28-131">Dodano `<DotNetCliToolReference>` elementu [pliku MvcMovie.csproj](#prepare-the-project-for-scaffolding).</span><span class="sxs-lookup"><span data-stu-id="fab28-131">You have added the `<DotNetCliToolReference>` element to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="fab28-132">Aparat szkieletów tworzy następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="fab28-132">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="fab28-133">Kontroler filmów (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="fab28-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="fab28-134">Pliki widoku razor dla stron Create, Delete, szczegóły, edycji i indeksu (*widoków/filmów/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="fab28-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="fab28-135">Automatyczne tworzenie [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki nosi nazwę *szkieletów*.</span><span class="sxs-lookup"><span data-stu-id="fab28-135">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="fab28-136">Konieczne będzie wkrótce aplikacji funkcjonalnej sieci web, która umożliwia zarządzanie filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fab28-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="fab28-137">Dodaj pliki do programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fab28-137">Add the files to Visual Studio</span></span>

* <span data-ttu-id="fab28-138">Dodaj *MovieController.cs* plik do projektu programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="fab28-138">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="fab28-139">Kliknij prawym przyciskiem myszy *kontrolerów* i wybierz polecenie **Dodaj > Dodaj pliki**.</span><span class="sxs-lookup"><span data-stu-id="fab28-139">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="fab28-140">Wybierz *MovieController.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="fab28-140">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="fab28-141">Dodaj *filmy* folderów i widoków:</span><span class="sxs-lookup"><span data-stu-id="fab28-141">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="fab28-142">Kliknij prawym przyciskiem myszy *widoków* i wybierz polecenie **Dodaj > Dodaj istniejący Folder**.</span><span class="sxs-lookup"><span data-stu-id="fab28-142">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="fab28-143">Przejdź do *widoków* folderu, wybierz opcję *Views\Movies*, a następnie wybierz **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="fab28-143">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="fab28-144">W **wybierz pliki, aby dodać z filmów** zaznacz pozycję **obejmują wszystkie**, a następnie **OK**.</span><span class="sxs-lookup"><span data-stu-id="fab28-144">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="fab28-145">Masz teraz bazę danych i stron do wyświetlania, edytowania, aktualizacji i usuwania danych.</span><span class="sxs-lookup"><span data-stu-id="fab28-145">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="fab28-146">W następnym samouczku firma Microsoft będzie współpracować z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fab28-146">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fab28-147">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fab28-147">Additional resources</span></span>

* [<span data-ttu-id="fab28-148">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="fab28-148">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="fab28-149">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="fab28-149">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="fab28-150">[Dodawanie widoku poprzedniej](adding-view.md)
> [obok Praca z SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="fab28-150">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
