---
title: Tworzenie interfejsu API sieci Web przy użyciu programu Visual Studio i ASP.NET Core dla systemu Windows
author: rick-anderson
description: Tworzenie składnika web API platformy ASP.NET Core MVC i Visual Studio dla systemu Windows
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 92b1b28205584d2f08a5dc8124e5c50aa938c80f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="c3a02-103">Tworzenie interfejsu API sieci Web przy użyciu programu Visual Studio i ASP.NET Core dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="c3a02-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="c3a02-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="c3a02-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="c3a02-105">W tym samouczku tworzy interfejs API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="c3a02-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="c3a02-106">Interfejs użytkownika (UI) nie jest tworzone.</span><span class="sxs-lookup"><span data-stu-id="c3a02-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="c3a02-107">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="c3a02-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="c3a02-108">Systemu Windows: Interfejs API z programu Visual Studio dla systemu Windows (w tym samouczku) sieci Web</span><span class="sxs-lookup"><span data-stu-id="c3a02-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="c3a02-109">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="c3a02-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="c3a02-110">System macOS, Linux, Windows: [interfejsu API sieci Web z kodem Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="c3a02-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE [intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="c3a02-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="c3a02-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="c3a02-112">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="c3a02-112">Create the project</span></span>

<span data-ttu-id="c3a02-113">W programie Visual Studio, wybierz **pliku** menu > **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="c3a02-113">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="c3a02-114">Wybierz **.NET Core** > **aplikacji sieci Web platformy ASP.NET Core** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="c3a02-114">Select **.NET Core** > **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="c3a02-115">Nazwij projekt `TodoApi` i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="c3a02-115">Name the project `TodoApi` and select **OK**.</span></span>

![Okno dialogowe nowego projektu](first-web-api/_static/new-project.png)

<span data-ttu-id="c3a02-117">W **nowe podstawowe aplikacji sieci Web ASP.NET - TodoApi** okno dialogowe, wybierz opcję **interfejsu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="c3a02-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **API** template.</span></span> <span data-ttu-id="c3a02-118">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="c3a02-118">Select **OK**.</span></span> <span data-ttu-id="c3a02-119">Czy **nie** wybierz **Włącz obsługę Docker**.</span><span class="sxs-lookup"><span data-stu-id="c3a02-119">Do **not** select **Enable Docker Support**.</span></span>

![Okno dialogowe nowego aplikacji sieci Web platformy ASP.NET z szablonem projektu interfejsu API sieci Web z platformy ASP.NET Core szablonów](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="c3a02-121">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="c3a02-121">Launch the app</span></span>

<span data-ttu-id="c3a02-122">W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="c3a02-122">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="c3a02-123">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port/api/values`, gdzie *portu* jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="c3a02-123">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="c3a02-124">Chrome, Microsoft Edge i przeglądarki Firefox można wyświetlić następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="c3a02-124">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="c3a02-125">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="c3a02-125">Add a model class</span></span>

<span data-ttu-id="c3a02-126">Model jest obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c3a02-126">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="c3a02-127">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="c3a02-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="c3a02-128">Dodaj folder o nazwie "Modele".</span><span class="sxs-lookup"><span data-stu-id="c3a02-128">Add a folder named "Models".</span></span> <span data-ttu-id="c3a02-129">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="c3a02-129">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="c3a02-130">Wybierz **dodać** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="c3a02-130">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="c3a02-131">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="c3a02-131">Name the folder *Models*.</span></span>

<span data-ttu-id="c3a02-132">Uwaga: Klasy modelu dowolnym Przejdź w projekcie.</span><span class="sxs-lookup"><span data-stu-id="c3a02-132">Note: The model classes go anywhere in the project.</span></span> <span data-ttu-id="c3a02-133">*Modele* folder jest używany przez Konwencję dla klas modelu.</span><span class="sxs-lookup"><span data-stu-id="c3a02-133">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="c3a02-134">Dodaj `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="c3a02-134">Add a `TodoItem` class.</span></span> <span data-ttu-id="c3a02-135">Kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="c3a02-135">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="c3a02-136">Nazwa klasy `TodoItem` i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="c3a02-136">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="c3a02-137">Aktualizacja `TodoItem` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="c3a02-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="c3a02-138">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="c3a02-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="c3a02-139">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="c3a02-139">Create the database context</span></span>

<span data-ttu-id="c3a02-140">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="c3a02-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="c3a02-141">Ta klasa jest tworzona przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="c3a02-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="c3a02-142">Dodaj `TodoContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="c3a02-142">Add a `TodoContext` class.</span></span> <span data-ttu-id="c3a02-143">Kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="c3a02-143">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="c3a02-144">Nazwa klasy `TodoContext` i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="c3a02-144">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="c3a02-145">Zastąp klasę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="c3a02-145">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="c3a02-146">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="c3a02-146">Add a controller</span></span>

<span data-ttu-id="c3a02-147">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="c3a02-147">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="c3a02-148">Wybierz **dodać** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="c3a02-148">Select **Add** > **New Item**.</span></span> <span data-ttu-id="c3a02-149">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasy kontrolera interfejsu API sieci Web** szablonu.</span><span class="sxs-lookup"><span data-stu-id="c3a02-149">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="c3a02-150">Nazwa klasy `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="c3a02-150">Name the class `TodoController`.</span></span>

![Okno dialogowe nowego elementu z kontrolerem dodatek do wyszukiwania sieci web i pole Kontroler interfejsu API wybrane](first-web-api/_static/new_controller.png)

<span data-ttu-id="c3a02-152">Zastąp klasę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="c3a02-152">Replace the class with the following code:</span></span>

[!INCLUDE [code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="c3a02-153">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="c3a02-153">Launch the app</span></span>

<span data-ttu-id="c3a02-154">W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="c3a02-154">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="c3a02-155">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port/api/values`, gdzie *portu* jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="c3a02-155">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="c3a02-156">Przejdź do `Todo` kontroler na `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="c3a02-156">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE [last part of web API](../includes/webApi/end.md)]

[!INCLUDE[Javascript Jquery](../includes/add-javascript-jquery/index.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

