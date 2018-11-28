---
title: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i Visual Studio
author: rick-anderson
description: Tworzenie internetowego interfejsu API platformy ASP.NET Core MVC i programu Visual Studio w Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450700"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a><span data-ttu-id="75337-103">Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="75337-103">Create a Web API with ASP.NET Core and Visual Studio</span></span>

<span data-ttu-id="75337-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="75337-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="75337-105">Ten samouczek opiera się internetowego interfejsu API do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="75337-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="75337-106">Nie jest tworzony interfejs użytkownika (UI).</span><span class="sxs-lookup"><span data-stu-id="75337-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="75337-107">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="75337-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="75337-108">Windows: Web API za pomocą programu Visual Studio w Windows (tego samouczka, zobacz [wersja wideo](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span><span class="sxs-lookup"><span data-stu-id="75337-108">Windows: Web API with Visual Studio on Windows (This tutorial, see the [video version](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span></span>
* <span data-ttu-id="75337-109">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="75337-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="75337-110">macOS i Linux, Windows: [interfejsu API sieci Web za pomocą programu Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="75337-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> <span data-ttu-id="75337-111">W tej chwili testujemy użyteczność nowo zaproponowanej struktury spisu treści dla dokumentacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75337-111">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="75337-112">Jeśli możesz poświęcić chwilę na wykonanie ćwiczenia polegającego na znalezieniu 7 różnych tematów w aktualnym lub zaproponowanym spisie treści, [kliknij tutaj i weź udział w badaniu](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="75337-112">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="75337-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="75337-113">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="75337-114">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="75337-114">Create the project</span></span>

<span data-ttu-id="75337-115">Wykonaj następujące kroki w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="75337-115">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="75337-116">Z **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="75337-116">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="75337-117">Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu.</span><span class="sxs-lookup"><span data-stu-id="75337-117">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="75337-118">Nadaj projektowi nazwę *TodoApi* i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="75337-118">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="75337-119">W **nowej podstawowej aplikacji sieci Web ASP.NET - TodoApi** okno dialogowe, wybierz wersję platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75337-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="75337-120">Wybierz **API** szablon i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="75337-120">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="75337-121">Czy **nie** wybierz **włączyć obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="75337-121">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="75337-122">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="75337-122">Launch the app</span></span>

<span data-ttu-id="75337-123">W programie Visual Studio naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="75337-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="75337-124">Program Visual Studio otworzy w przeglądarce i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="75337-124">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="75337-125">Chrome, Microsoft Edge i przeglądarki Firefox wyświetlane następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="75337-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="75337-126">Jeśli używasz programu Internet Explorer, zostanie wyświetlony monit, aby zapisać *values.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="75337-126">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="75337-127">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="75337-127">Add a model class</span></span>

<span data-ttu-id="75337-128">Model jest obiekt reprezentujący dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="75337-128">A model is an object representing the data in the app.</span></span> <span data-ttu-id="75337-129">W takim przypadku tylko model jest element do wykonania.</span><span class="sxs-lookup"><span data-stu-id="75337-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="75337-130">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="75337-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="75337-131">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="75337-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="75337-132">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="75337-132">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="75337-133">Klasy modeli można przejść w dowolnym miejscu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="75337-133">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="75337-134">*Modeli* folder jest używany przez Konwencję dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="75337-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="75337-135">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="75337-135">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="75337-136">Nazwa klasy *TodoItem* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="75337-136">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="75337-137">Aktualizacja `TodoItem` klasy z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="75337-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="75337-138">Baza danych generuje `Id` podczas `TodoItem` zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="75337-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="75337-139">Utwórz kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="75337-139">Create the database context</span></span>

<span data-ttu-id="75337-140">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="75337-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="75337-141">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="75337-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="75337-142">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="75337-142">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="75337-143">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="75337-143">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="75337-144">Zastąp klasę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="75337-144">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="75337-145">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="75337-145">Add a controller</span></span>

<span data-ttu-id="75337-146">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="75337-146">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="75337-147">Wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="75337-147">Select **Add** > **New Item**.</span></span> <span data-ttu-id="75337-148">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="75337-148">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="75337-149">Nazwa klasy *TodoController*i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="75337-149">Name the class *TodoController*, and click **Add**.</span></span>

![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu API wybrane](first-web-api/_static/new_controller.png)

<span data-ttu-id="75337-151">Zastąp klasę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="75337-151">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="75337-152">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="75337-152">Launch the app</span></span>

<span data-ttu-id="75337-153">W programie Visual Studio naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="75337-153">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="75337-154">Program Visual Studio otworzy w przeglądarce i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="75337-154">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="75337-155">Przejdź do `Todo` kontroler na `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="75337-155">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
