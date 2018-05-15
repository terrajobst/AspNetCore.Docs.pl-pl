---
title: Tworzenie interfejsu API sieci Web przy użyciu programu Visual Studio i ASP.NET Core dla systemu Windows
author: rick-anderson
description: Tworzenie składnika web API platformy ASP.NET Core MVC i Visual Studio dla systemu Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: cb46f8b4013488dbe2bb5ca3d08a8c6e452141dd
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="707cf-103">Tworzenie interfejsu API sieci Web przy użyciu programu Visual Studio i ASP.NET Core dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="707cf-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="707cf-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="707cf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="707cf-105">W tym samouczku tworzy interfejs API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="707cf-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="707cf-106">Interfejs użytkownika (UI) nie jest tworzone.</span><span class="sxs-lookup"><span data-stu-id="707cf-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="707cf-107">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="707cf-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="707cf-108">Systemu Windows: Interfejs API z programu Visual Studio dla systemu Windows (w tym samouczku) sieci Web</span><span class="sxs-lookup"><span data-stu-id="707cf-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="707cf-109">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="707cf-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="707cf-110">System macOS, Linux, Windows: [interfejsu API sieci Web z kodem Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="707cf-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="707cf-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="707cf-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="707cf-112">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="707cf-112">Create the project</span></span>

<span data-ttu-id="707cf-113">Wykonaj następujące czynności w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="707cf-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="707cf-114">Z **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="707cf-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="707cf-115">Wybierz **aplikacji sieci Web platformy ASP.NET Core** szablonu.</span><span class="sxs-lookup"><span data-stu-id="707cf-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="707cf-116">Nazwij projekt *TodoApi* i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="707cf-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="707cf-117">W **nowe podstawowe aplikacji sieci Web ASP.NET - TodoApi** okno dialogowe, wybierz wersję platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="707cf-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="707cf-118">Wybierz **interfejsu API** szablon i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="707cf-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="707cf-119">Czy **nie** wybierz **Włącz obsługę Docker**.</span><span class="sxs-lookup"><span data-stu-id="707cf-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="707cf-120">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="707cf-120">Launch the app</span></span>

<span data-ttu-id="707cf-121">W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="707cf-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="707cf-122">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="707cf-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="707cf-123">Chrome, Microsoft Edge i przeglądarki Firefox można wyświetlić następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="707cf-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="707cf-124">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="707cf-124">Add a model class</span></span>

<span data-ttu-id="707cf-125">Model jest obiekt reprezentujący danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="707cf-125">A model is an object representing the data in the app.</span></span> <span data-ttu-id="707cf-126">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="707cf-126">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="707cf-127">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="707cf-127">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="707cf-128">Wybierz **dodać** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="707cf-128">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="707cf-129">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="707cf-129">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="707cf-130">Klasy modelu może przejść w dowolnym miejscu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="707cf-130">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="707cf-131">*Modele* folder jest używany przez Konwencję dla klas modelu.</span><span class="sxs-lookup"><span data-stu-id="707cf-131">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="707cf-132">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="707cf-132">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="707cf-133">Nazwa klasy *TodoItem* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="707cf-133">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="707cf-134">Aktualizacja `TodoItem` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="707cf-134">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="707cf-135">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="707cf-135">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="707cf-136">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="707cf-136">Create the database context</span></span>

<span data-ttu-id="707cf-137">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="707cf-137">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="707cf-138">Ta klasa jest tworzona przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="707cf-138">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="707cf-139">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="707cf-139">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="707cf-140">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="707cf-140">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="707cf-141">Zastąp klasę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="707cf-141">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="707cf-142">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="707cf-142">Add a controller</span></span>

<span data-ttu-id="707cf-143">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="707cf-143">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="707cf-144">Wybierz **dodać** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="707cf-144">Select **Add** > **New Item**.</span></span> <span data-ttu-id="707cf-145">W **Dodaj nowy element** okno dialogowe, wybierz opcję **Klasa kontrolera interfejsu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="707cf-145">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="707cf-146">Nazwa klasy *TodoController*i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="707cf-146">Name the class *TodoController*, and click **Add**.</span></span>

![Okno dialogowe nowego elementu z kontrolerem dodatek do wyszukiwania sieci web i pole Kontroler interfejsu API wybrane](first-web-api/_static/new_controller.png)

<span data-ttu-id="707cf-148">Zastąp klasę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="707cf-148">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="707cf-149">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="707cf-149">Launch the app</span></span>

<span data-ttu-id="707cf-150">W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="707cf-150">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="707cf-151">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="707cf-151">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="707cf-152">Przejdź do `Todo` kontroler na `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="707cf-152">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
