---
title: Tworzenie składnika Web API platformy ASP.NET Core i kodu programu Visual Studio
author: rick-anderson
description: Tworzenie interfejsu API sieci web na macOS, Linux lub Windows z platformy ASP.NET Core MVC i Visual Studio Code
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 705524a62018b9f0fbd8c40fa1b70d4c62ee236e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="85d50-103">Tworzenie składnika Web API platformy ASP.NET Core i kodu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="85d50-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="85d50-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="85d50-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="85d50-105">W tym samouczku kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="85d50-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="85d50-106">Interfejs użytkownika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="85d50-106">A UI isn't constructed.</span></span>

<span data-ttu-id="85d50-107">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="85d50-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="85d50-108">System macOS, Linux, Windows: interfejsu API sieci Web za pomocą programu Visual Studio Code (w tym samouczku)</span><span class="sxs-lookup"><span data-stu-id="85d50-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="85d50-109">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="85d50-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="85d50-110">System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="85d50-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE [template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="85d50-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="85d50-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="85d50-112">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="85d50-112">Create the project</span></span>

<span data-ttu-id="85d50-113">W konsoli uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="85d50-113">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="85d50-114">Otwórz *TodoApi* folderu w Visual Studio (kod VS) i wybierz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="85d50-114">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="85d50-115">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"TodoApi".</span><span class="sxs-lookup"><span data-stu-id="85d50-115">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="85d50-116">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="85d50-116">Add them?"</span></span>
- <span data-ttu-id="85d50-117">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="85d50-117">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "TodoApi".](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="85d50-121">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="85d50-121">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="85d50-122">W przeglądarce przejdź do http://localhost:5000/api/values .</span><span class="sxs-lookup"><span data-stu-id="85d50-122">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="85d50-123">Wyświetlane są następujące:</span><span class="sxs-lookup"><span data-stu-id="85d50-123">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="85d50-124">Zobacz [pomocy programu Visual Studio Code](#visual-studio-code-help) porady na temat używania w kodzie VS.</span><span class="sxs-lookup"><span data-stu-id="85d50-124">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="85d50-125">Dodaj obsługę Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="85d50-125">Add support for Entity Framework Core</span></span>

<span data-ttu-id="85d50-126">Tworzenie nowego projektu w programie .NET Core 2.0 dodaje dostawcę "Microsoft.AspNetCore.All" w *TodoApi.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="85d50-126">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="85d50-127">Nie istnieje potrzeba do zainstalowania [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) bazy danych dostawcy osobno.</span><span class="sxs-lookup"><span data-stu-id="85d50-127">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="85d50-128">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="85d50-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="85d50-129">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="85d50-129">Add a model class</span></span>

<span data-ttu-id="85d50-130">Model jest obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="85d50-130">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="85d50-131">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="85d50-131">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="85d50-132">Dodaj folder o nazwie *modele*.</span><span class="sxs-lookup"><span data-stu-id="85d50-132">Add a folder named *Models*.</span></span> <span data-ttu-id="85d50-133">Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="85d50-133">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="85d50-134">Dodaj `TodoItem` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="85d50-134">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="85d50-135">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="85d50-135">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="85d50-136">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="85d50-136">Create the database context</span></span>

<span data-ttu-id="85d50-137">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="85d50-137">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="85d50-138">Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="85d50-138">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="85d50-139">Dodaj `TodoContext` klasy w *modele* folderu:</span><span class="sxs-lookup"><span data-stu-id="85d50-139">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="85d50-140">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="85d50-140">Add a controller</span></span>

<span data-ttu-id="85d50-141">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="85d50-141">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="85d50-142">Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="85d50-142">Add the following code:</span></span>

[!INCLUDE [code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="85d50-143">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="85d50-143">Launch the app</span></span>

<span data-ttu-id="85d50-144">W kodzie VS naciśnij klawisz F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="85d50-144">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="85d50-145">Przejdź do http://localhost:5000/api/todo ( `Todo` kontrolera właśnie utworzyliśmy).</span><span class="sxs-lookup"><span data-stu-id="85d50-145">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE [last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="85d50-146">Visual Studio Code pomocy</span><span class="sxs-lookup"><span data-stu-id="85d50-146">Visual Studio Code help</span></span>

- [<span data-ttu-id="85d50-147">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="85d50-147">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="85d50-148">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="85d50-148">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="85d50-149">Zintegrowane terminali</span><span class="sxs-lookup"><span data-stu-id="85d50-149">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="85d50-150">Skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="85d50-150">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="85d50-151">System macOS skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="85d50-151">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="85d50-152">Skróty klawiaturowe systemu Linux</span><span class="sxs-lookup"><span data-stu-id="85d50-152">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="85d50-153">Skróty klawiaturowe systemu Windows</span><span class="sxs-lookup"><span data-stu-id="85d50-153">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE [next steps](../includes/webApi/next.md)]

