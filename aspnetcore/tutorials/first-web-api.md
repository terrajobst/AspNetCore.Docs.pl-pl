---
title: Tworzenie interfejsu API sieci Web przy użyciu programu Visual Studio i ASP.NET Core dla systemu Windows
author: rick-anderson
description: Tworzenie składnika web API platformy ASP.NET Core MVC i Visual Studio dla systemu Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 962c24a7e654328df7e8893e589e45b19e87b931
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="edcba-103">Tworzenie interfejsu API sieci Web przy użyciu programu Visual Studio i ASP.NET Core dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="edcba-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="edcba-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="edcba-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

::: moniker range="= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]
::: moniker-end

<span data-ttu-id="edcba-106">W tym samouczku tworzy interfejs API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="edcba-106">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="edcba-107">Interfejs użytkownika (UI) nie jest tworzone.</span><span class="sxs-lookup"><span data-stu-id="edcba-107">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="edcba-108">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="edcba-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="edcba-109">Systemu Windows: Interfejs API z programu Visual Studio dla systemu Windows (w tym samouczku) sieci Web</span><span class="sxs-lookup"><span data-stu-id="edcba-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="edcba-110">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="edcba-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="edcba-111">System macOS, Linux, Windows: [interfejsu API sieci Web z kodem Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="edcba-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="edcba-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="edcba-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="edcba-113">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="edcba-113">Create the project</span></span>

<span data-ttu-id="edcba-114">Wykonaj następujące czynności w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="edcba-114">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="edcba-115">Z **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="edcba-115">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="edcba-116">Wybierz **aplikacji sieci Web platformy ASP.NET Core** szablonu.</span><span class="sxs-lookup"><span data-stu-id="edcba-116">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="edcba-117">Nazwij projekt *TodoApi* i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="edcba-117">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="edcba-118">W **nowe podstawowe aplikacji sieci Web ASP.NET - TodoApi** okno dialogowe, wybierz wersję platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="edcba-118">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="edcba-119">Wybierz **interfejsu API** szablon i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="edcba-119">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="edcba-120">Czy **nie** wybierz **Włącz obsługę Docker**.</span><span class="sxs-lookup"><span data-stu-id="edcba-120">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="edcba-121">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="edcba-121">Launch the app</span></span>

<span data-ttu-id="edcba-122">W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="edcba-122">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="edcba-123">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="edcba-123">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="edcba-124">Chrome, Microsoft Edge i przeglądarki Firefox można wyświetlić następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="edcba-124">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="edcba-125">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="edcba-125">Add a model class</span></span>

<span data-ttu-id="edcba-126">Model jest obiekt reprezentujący danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="edcba-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="edcba-127">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="edcba-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="edcba-128">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="edcba-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="edcba-129">Wybierz **dodać** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="edcba-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="edcba-130">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="edcba-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="edcba-131">Klasy modelu może przejść w dowolnym miejscu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="edcba-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="edcba-132">*Modele* folder jest używany przez Konwencję dla klas modelu.</span><span class="sxs-lookup"><span data-stu-id="edcba-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="edcba-133">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="edcba-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="edcba-134">Nazwa klasy *TodoItem* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="edcba-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="edcba-135">Aktualizacja `TodoItem` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="edcba-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="edcba-136">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="edcba-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="edcba-137">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="edcba-137">Create the database context</span></span>

<span data-ttu-id="edcba-138">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="edcba-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="edcba-139">Ta klasa jest tworzona przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="edcba-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="edcba-140">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="edcba-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="edcba-141">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="edcba-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="edcba-142">Zastąp klasę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="edcba-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="edcba-143">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="edcba-143">Add a controller</span></span>

<span data-ttu-id="edcba-144">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="edcba-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="edcba-145">Wybierz **dodać** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="edcba-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="edcba-146">W **Dodaj nowy element** okno dialogowe, wybierz opcję **Klasa kontrolera interfejsu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="edcba-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="edcba-147">Nazwa klasy *TodoController*i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="edcba-147">Name the class *TodoController*, and click **Add**.</span></span>

![Okno dialogowe nowego elementu z kontrolerem dodatek do wyszukiwania sieci web i pole Kontroler interfejsu API wybrane](first-web-api/_static/new_controller.png)

<span data-ttu-id="edcba-149">Zastąp klasę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="edcba-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="edcba-150">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="edcba-150">Launch the app</span></span>

<span data-ttu-id="edcba-151">W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="edcba-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="edcba-152">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="edcba-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="edcba-153">Przejdź do `Todo` kontroler na `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="edcba-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
