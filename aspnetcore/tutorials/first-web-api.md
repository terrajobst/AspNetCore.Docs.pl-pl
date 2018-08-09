---
title: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i Visual Studio
author: rick-anderson
description: Tworzenie internetowego interfejsu API platformy ASP.NET Core MVC i programu Visual Studio w Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 2694388324cdbd246aad6c88d8439171704dfe89
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722519"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a><span data-ttu-id="27b7a-103">Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b7a-103">Create a Web API with ASP.NET Core and Visual Studio</span></span>

<span data-ttu-id="27b7a-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="27b7a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="27b7a-105">Ten samouczek opiera się internetowego interfejsu API do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="27b7a-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="27b7a-106">Nie jest tworzony interfejs użytkownika (UI).</span><span class="sxs-lookup"><span data-stu-id="27b7a-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="27b7a-107">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="27b7a-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="27b7a-108">Windows: Web API za pomocą programu Visual Studio w Windows (w tym samouczku)</span><span class="sxs-lookup"><span data-stu-id="27b7a-108">Windows: Web API with Visual Studio on Windows (This tutorial)</span></span>
* <span data-ttu-id="27b7a-109">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="27b7a-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="27b7a-110">macOS i Linux, Windows: [interfejsu API sieci Web za pomocą programu Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="27b7a-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="27b7a-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="27b7a-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="27b7a-112">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="27b7a-112">Create the project</span></span>

<span data-ttu-id="27b7a-113">Wykonaj następujące kroki w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="27b7a-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="27b7a-114">Z **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="27b7a-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="27b7a-115">Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu.</span><span class="sxs-lookup"><span data-stu-id="27b7a-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="27b7a-116">Nadaj projektowi nazwę *TodoApi* i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="27b7a-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="27b7a-117">W **nowej podstawowej aplikacji sieci Web ASP.NET - TodoApi** okno dialogowe, wybierz wersję platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="27b7a-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="27b7a-118">Wybierz **API** szablon i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="27b7a-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="27b7a-119">Czy **nie** wybierz **włączyć obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="27b7a-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="27b7a-120">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="27b7a-120">Launch the app</span></span>

<span data-ttu-id="27b7a-121">W programie Visual Studio naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="27b7a-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="27b7a-122">Program Visual Studio otworzy w przeglądarce i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="27b7a-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="27b7a-123">Chrome, Microsoft Edge i przeglądarki Firefox wyświetlane następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="27b7a-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="27b7a-124">Jeśli używasz programu Internet Explorer, zostanie wyświetlony monit, aby zapisać *values.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="27b7a-124">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="27b7a-125">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="27b7a-125">Add a model class</span></span>

<span data-ttu-id="27b7a-126">Model jest obiekt reprezentujący dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27b7a-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="27b7a-127">W takim przypadku tylko model jest element do wykonania.</span><span class="sxs-lookup"><span data-stu-id="27b7a-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="27b7a-128">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="27b7a-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="27b7a-129">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="27b7a-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="27b7a-130">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="27b7a-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="27b7a-131">Klasy modeli można przejść w dowolnym miejscu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="27b7a-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="27b7a-132">*Modeli* folder jest używany przez Konwencję dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="27b7a-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="27b7a-133">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="27b7a-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="27b7a-134">Nazwa klasy *TodoItem* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="27b7a-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="27b7a-135">Aktualizacja `TodoItem` klasy z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="27b7a-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="27b7a-136">Baza danych generuje `Id` podczas `TodoItem` zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="27b7a-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="27b7a-137">Utwórz kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="27b7a-137">Create the database context</span></span>

<span data-ttu-id="27b7a-138">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="27b7a-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="27b7a-139">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="27b7a-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="27b7a-140">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="27b7a-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="27b7a-141">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="27b7a-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="27b7a-142">Zastąp klasę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="27b7a-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="27b7a-143">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="27b7a-143">Add a controller</span></span>

<span data-ttu-id="27b7a-144">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="27b7a-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="27b7a-145">Wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="27b7a-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="27b7a-146">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="27b7a-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="27b7a-147">Nazwa klasy *TodoController*i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="27b7a-147">Name the class *TodoController*, and click **Add**.</span></span>

![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu API wybrane](first-web-api/_static/new_controller.png)

<span data-ttu-id="27b7a-149">Zastąp klasę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="27b7a-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="27b7a-150">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="27b7a-150">Launch the app</span></span>

<span data-ttu-id="27b7a-151">W programie Visual Studio naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="27b7a-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="27b7a-152">Program Visual Studio otworzy w przeglądarce i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="27b7a-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="27b7a-153">Przejdź do `Todo` kontroler na `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="27b7a-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
