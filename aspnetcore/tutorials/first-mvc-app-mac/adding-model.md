---
title: Dodawanie modelu do aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac
author: rick-anderson
description: Dodawanie modelu do prostej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 09/22/2017
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 6db079558ccf4515a37a90f7a9e2608333acd7cf
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961389"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="88f15-103">Kliknij prawym przyciskiem myszy *modele* folder, a następnie wybierz **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="88f15-103">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="88f15-104">W **nowy plik** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="88f15-104">In the **New File** dialog:</span></span>

  * <span data-ttu-id="88f15-105">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="88f15-105">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="88f15-106">Wybierz **pustą klasę** w słabe center.</span><span class="sxs-lookup"><span data-stu-id="88f15-106">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="88f15-107">Nazwa klasy **film** i wybierz **nowy**.</span><span class="sxs-lookup"><span data-stu-id="88f15-107">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="88f15-108">Dodaj następujące właściwości `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="88f15-108">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="88f15-109">`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="88f15-109">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="88f15-110">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="88f15-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="88f15-111">Masz teraz **M**odelu w Twojej **M**VC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f15-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="88f15-112">Przygotowanie projektu do tworzenia szkieletów</span><span class="sxs-lookup"><span data-stu-id="88f15-112">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="88f15-113">Kliknij prawym przyciskiem myszy plik projektu, a następnie wybierz **Narzędzia > Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="88f15-113">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![Widok powyżej kroku](adding-model/_static/1.png)

- <span data-ttu-id="88f15-115">Dodaj następujące wyróżnione pakietów NuGet do *MvcMovie.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="88f15-115">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="88f15-116">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="88f15-116">Save the file.</span></span>

- <span data-ttu-id="88f15-117">Utwórz *Models/MvcMovieContext.cs* pliku i dodaj następującą `MvcMovieContext` klasy:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="88f15-117">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="88f15-118">Otwórz *Startup.cs* plik i dodać dwie deklaracje Using:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="88f15-118">Open the *Startup.cs* file and add two usings:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="88f15-119">Kontekst bazy danych, aby dodać *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="88f15-119">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="88f15-120">Ta wartość informuje klas modelu, które znajdują się w modelu danych programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="88f15-120">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="88f15-121">Definiujesz co *zestaw jednostek* filmu obiektów, które będą reprezentowane w bazie danych jako tabelę filmu.</span><span class="sxs-lookup"><span data-stu-id="88f15-121">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="88f15-122">Skompiluj projekt, aby sprawdzić, czy nie ma żadnych błędów.</span><span class="sxs-lookup"><span data-stu-id="88f15-122">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="88f15-123">Tworzenie szkieletu MovieController</span><span class="sxs-lookup"><span data-stu-id="88f15-123">Scaffold the MovieController</span></span>

<span data-ttu-id="88f15-124">Otwórz okno terminala w folderze projektu i uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="88f15-124">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="88f15-125">Jeśli zostanie wyświetlony błąd `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span><span class="sxs-lookup"><span data-stu-id="88f15-125">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="88f15-126">Znajdujesz się w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="88f15-126">You are in the project directory.</span></span> <span data-ttu-id="88f15-127">Katalog projektu ma *Program.cs*, *Startup.cs* i *.csproj* plików.</span><span class="sxs-lookup"><span data-stu-id="88f15-127">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="88f15-128">Używana wersja dotnet jest 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="88f15-128">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="88f15-129">Uruchom `dotnet` Aby pobrać wersję.</span><span class="sxs-lookup"><span data-stu-id="88f15-129">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="88f15-130">Dodano `<DotNetCliToolReference>` elementu [pliku MvcMovie.csproj](#prepare-the-project-for-scaffolding).</span><span class="sxs-lookup"><span data-stu-id="88f15-130">You have added the `<DotNetCliToolReference>` element to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="88f15-131">Aparat szkieletów tworzy następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="88f15-131">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="88f15-132">Kontroler filmów (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="88f15-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="88f15-133">Pliki widoku razor dla stron Create, Delete, szczegóły, edycji i indeksu (*widoków/filmów/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="88f15-133">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="88f15-134">Automatyczne tworzenie [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki nosi nazwę *szkieletów*.</span><span class="sxs-lookup"><span data-stu-id="88f15-134">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="88f15-135">Konieczne będzie wkrótce aplikacji funkcjonalnej sieci web, która umożliwia zarządzanie filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="88f15-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="88f15-136">Dodaj pliki do programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="88f15-136">Add the files to Visual Studio</span></span>

* <span data-ttu-id="88f15-137">Dodaj *MovieController.cs* plik do projektu programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="88f15-137">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="88f15-138">Kliknij prawym przyciskiem myszy *kontrolerów* i wybierz polecenie **Dodaj > Dodaj pliki**.</span><span class="sxs-lookup"><span data-stu-id="88f15-138">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="88f15-139">Wybierz *MovieController.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="88f15-139">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="88f15-140">Dodaj *filmy* folderów i widoków:</span><span class="sxs-lookup"><span data-stu-id="88f15-140">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="88f15-141">Kliknij prawym przyciskiem myszy *widoków* i wybierz polecenie **Dodaj > Dodaj istniejący Folder**.</span><span class="sxs-lookup"><span data-stu-id="88f15-141">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="88f15-142">Przejdź do *widoków* folderu, wybierz opcję *Views\Movies*, a następnie wybierz **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="88f15-142">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="88f15-143">W **wybierz pliki, aby dodać z filmów** zaznacz pozycję **obejmują wszystkie**, a następnie **OK**.</span><span class="sxs-lookup"><span data-stu-id="88f15-143">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="88f15-144">Masz teraz bazę danych i stron do wyświetlania, edytowania, aktualizacji i usuwania danych.</span><span class="sxs-lookup"><span data-stu-id="88f15-144">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="88f15-145">W następnym samouczku firma Microsoft będzie współpracować z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="88f15-145">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="88f15-146">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="88f15-146">Additional resources</span></span>

* [<span data-ttu-id="88f15-147">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="88f15-147">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="88f15-148">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="88f15-148">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="88f15-149">[Dodawanie widoku poprzedniej](adding-view.md)
> [obok Praca z SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="88f15-149">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
