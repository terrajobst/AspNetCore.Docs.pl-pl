---
title: Dodaj model do aplikacji platformy ASP.NET Core MVC
author: rick-anderson
description: Dodawanie modelu do prostej aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 77750ba0df7775d6a0e4744811848bfe9782d995
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="857cd-103">Dodaj model do aplikacji platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="857cd-103">Add a model to an ASP.NET Core MVC app</span></span>

[!INCLUDE [adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="857cd-104">Dodaj klasę do *modele* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="857cd-104">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="857cd-105">Dodaj następujący kod do *Models/Movie.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="857cd-105">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="857cd-106">`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="857cd-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="857cd-107">Utworzyć aplikację w celu zweryfikowania nie zawiera błędów i na koniec dodano **M**odelu do Twojej **M**VC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="857cd-107">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="857cd-108">Przygotowanie projektu do tworzenia szkieletów</span><span class="sxs-lookup"><span data-stu-id="857cd-108">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="857cd-109">Dodaj następujące wyróżnione pakietów NuGet do *MvcMovie.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="857cd-109">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="857cd-110">Zapisz plik i wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="857cd-110">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="857cd-111">Utwórz *Models/MvcMovieContext.cs* pliku i dodaj następującą `MvcMovieContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="857cd-111">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="857cd-112">Otwórz *Startup.cs* plik i dodać dwie deklaracje Using:</span><span class="sxs-lookup"><span data-stu-id="857cd-112">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="857cd-113">Kontekst bazy danych, aby dodać *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="857cd-113">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="857cd-114">Ta wartość informuje klas modelu, które znajdują się w modelu danych programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="857cd-114">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="857cd-115">Definiujesz co *zestaw jednostek* filmu obiektów, które będą reprezentowane w bazie danych jako tabelę filmu.</span><span class="sxs-lookup"><span data-stu-id="857cd-115">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="857cd-116">Skompiluj projekt, aby sprawdzić, czy nie ma żadnych błędów.</span><span class="sxs-lookup"><span data-stu-id="857cd-116">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="857cd-117">Tworzenie szkieletu MovieController</span><span class="sxs-lookup"><span data-stu-id="857cd-117">Scaffold the MovieController</span></span>

<span data-ttu-id="857cd-118">Otwórz okno terminala w folderze projektu i uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="857cd-118">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="857cd-119">Aparat szkieletów tworzy następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="857cd-119">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="857cd-120">Kontroler filmów (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="857cd-120">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="857cd-121">Pliki widoku razor dla stron Create, Delete, szczegóły, edycji i indeksu (*widoków/filmów/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="857cd-121">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="857cd-122">Automatyczne tworzenie [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki nosi nazwę *szkieletów*.</span><span class="sxs-lookup"><span data-stu-id="857cd-122">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="857cd-123">Konieczne będzie wkrótce aplikacji funkcjonalnej sieci web, która umożliwia zarządzanie filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="857cd-123">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="857cd-124">Masz teraz bazę danych i stron do wyświetlania, edytowania, aktualizacji i usuwania danych.</span><span class="sxs-lookup"><span data-stu-id="857cd-124">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="857cd-125">W następnym samouczku firma Microsoft będzie współpracować z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="857cd-125">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="857cd-126">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="857cd-126">Additional resources</span></span>

* [<span data-ttu-id="857cd-127">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="857cd-127">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="857cd-128">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="857cd-128">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="857cd-129">[Poprzednie — Dodawanie widoku](adding-view.md)
> [następne — Praca z bazy danych SQLite](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="857cd-129">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
