---
title: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i programu Visual Studio Code
author: rick-anderson
description: Tworzenie interfejsu API sieci web w systemie macOS, Linux lub Windows przy użyciu platformy ASP.NET Core MVC i programu Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 740110908358a382f20bc1e54e98056296278acf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089667"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="bc8a2-103">Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bc8a2-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="bc8a2-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="bc8a2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="bc8a2-105">W tym samouczku Tworzenie internetowego interfejsu API do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="bc8a2-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="bc8a2-106">Interfejs użytkownika nie jest konstruowany.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-106">A UI isn't constructed.</span></span>

<span data-ttu-id="bc8a2-107">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="bc8a2-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="bc8a2-108">macOS i Linux, Windows: interfejs API sieci Web za pomocą programu Visual Studio Code (w tym samouczku)</span><span class="sxs-lookup"><span data-stu-id="bc8a2-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="bc8a2-109">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="bc8a2-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="bc8a2-110">Windows: [internetowego interfejsu API z programem Visual Studio for Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="bc8a2-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="bc8a2-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="bc8a2-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

<span data-ttu-id="bc8a2-112">Zobacz [pomocy programu Visual Studio Code](#visual-studio-code-help) porady na temat korzystania z programu VS Code.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-112">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="bc8a2-113">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="bc8a2-113">Create the project</span></span>

<span data-ttu-id="bc8a2-114">Z poziomu konsoli uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="bc8a2-114">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="bc8a2-115">*TodoApi* zostanie otwarty folder, w programie Visual Studio Code (VS Code).</span><span class="sxs-lookup"><span data-stu-id="bc8a2-115">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="bc8a2-116">Wybierz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-116">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="bc8a2-117">Wybierz **tak** do **Ostrzegaj** komunikat "wymagane zasoby do tworzenia i debugowania brakuje"TodoApi".</span><span class="sxs-lookup"><span data-stu-id="bc8a2-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="bc8a2-118">Dodane?"</span><span class="sxs-lookup"><span data-stu-id="bc8a2-118">Add them?"</span></span>
* <span data-ttu-id="bc8a2-119">Wybierz **przywrócić** do **informacje** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="bc8a2-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Program VS Code z zasobami Ostrzegaj wymagane do tworzenia i debugowania brakuje "TodoApi".](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="bc8a2-123">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-123">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="bc8a2-124">W przeglądarce przejdź do http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-124">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="bc8a2-125">Są wyświetlane następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="bc8a2-125">The following output is displayed:</span></span>

```json
["value1","value2"]
```



## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="bc8a2-126">Dodaj obsługę platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="bc8a2-126">Add support for Entity Framework Core</span></span>

:::moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc8a2-127">Tworzenie nowego projektu w programie ASP.NET Core 2.1 lub nowszej dodaje [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="bc8a2-127">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) to the project file:</span></span>

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

<span data-ttu-id="bc8a2-128">Tworzenie nowego projektu w programie ASP.NET Core 2.0 dodaje [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="bc8a2-128">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) to the project file:</span></span>

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

<span data-ttu-id="bc8a2-129">Nie ma potrzeby instalowania [Entity Framework Core InMemory](/ef/core/providers/in-memory/) bazy danych dostawcy osobno.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-129">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="bc8a2-130">Ten dostawca bazy danych umożliwia platformy Entity Framework Core ma być używany z bazą danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-130">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="bc8a2-131">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="bc8a2-131">Add a model class</span></span>

<span data-ttu-id="bc8a2-132">Model jest obiekt reprezentujący dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-132">A model is an object representing the data in your app.</span></span> <span data-ttu-id="bc8a2-133">W takim przypadku tylko model jest element do wykonania.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-133">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="bc8a2-134">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-134">Add a folder named *Models*.</span></span> <span data-ttu-id="bc8a2-135">Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-135">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="bc8a2-136">Dodaj `TodoItem` klasy z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="bc8a2-136">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="bc8a2-137">Baza danych generuje `Id` podczas `TodoItem` zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-137">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="bc8a2-138">Utwórz kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="bc8a2-138">Create the database context</span></span>

<span data-ttu-id="bc8a2-139">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-139">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="bc8a2-140">Tworzenie tej klasy, wynikające z `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-140">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="bc8a2-141">Dodaj `TodoContext` klasy w *modeli* folderu:</span><span class="sxs-lookup"><span data-stu-id="bc8a2-141">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="bc8a2-142">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="bc8a2-142">Add a controller</span></span>

<span data-ttu-id="bc8a2-143">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-143">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="bc8a2-144">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="bc8a2-144">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="bc8a2-145">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="bc8a2-145">Launch the app</span></span>

<span data-ttu-id="bc8a2-146">W programie VS Code naciśnij klawisz F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="bc8a2-146">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="bc8a2-147">Przejdź do http://localhost:5000/api/todo ( `Todo` utworzyliśmy kontrolera).</span><span class="sxs-lookup"><span data-stu-id="bc8a2-147">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="bc8a2-148">Visual Studio Code pomocy</span><span class="sxs-lookup"><span data-stu-id="bc8a2-148">Visual Studio Code help</span></span>

* [<span data-ttu-id="bc8a2-149">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="bc8a2-149">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="bc8a2-150">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="bc8a2-150">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="bc8a2-151">Zintegrowany terminal</span><span class="sxs-lookup"><span data-stu-id="bc8a2-151">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="bc8a2-152">Skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="bc8a2-152">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="bc8a2-153">skróty klawiaturowe dla systemu macOS</span><span class="sxs-lookup"><span data-stu-id="bc8a2-153">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="bc8a2-154">Skróty klawiaturowe w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="bc8a2-154">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="bc8a2-155">Skróty klawiaturowe Windows</span><span class="sxs-lookup"><span data-stu-id="bc8a2-155">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
