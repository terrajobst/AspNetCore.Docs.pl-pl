---
title: Tworzenie składnika Web API platformy ASP.NET Core i kodu programu Visual Studio
author: rick-anderson
description: Tworzenie interfejsu API sieci web na macOS, Linux lub Windows z platformy ASP.NET Core MVC i Visual Studio Code
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: f991aeadbaa3f7696d6fd6b8791d26248e7560a6
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="db1c6-103">Tworzenie składnika Web API platformy ASP.NET Core i kodu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="db1c6-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="db1c6-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="db1c6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

::: moniker range="= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]
::: moniker-end

<span data-ttu-id="db1c6-106">W tym samouczku kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="db1c6-106">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="db1c6-107">Interfejs użytkownika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="db1c6-107">A UI isn't constructed.</span></span>

<span data-ttu-id="db1c6-108">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="db1c6-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="db1c6-109">System macOS, Linux, Windows: interfejsu API sieci Web za pomocą programu Visual Studio Code (w tym samouczku)</span><span class="sxs-lookup"><span data-stu-id="db1c6-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="db1c6-110">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="db1c6-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="db1c6-111">System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="db1c6-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="db1c6-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="db1c6-112">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="db1c6-113">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="db1c6-113">Create the project</span></span>

<span data-ttu-id="db1c6-114">W konsoli uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="db1c6-114">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="db1c6-115">*TodoApi* zostanie otwarty folder w Visual Studio (kod VS).</span><span class="sxs-lookup"><span data-stu-id="db1c6-115">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="db1c6-116">Wybierz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="db1c6-116">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="db1c6-117">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"TodoApi".</span><span class="sxs-lookup"><span data-stu-id="db1c6-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="db1c6-118">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="db1c6-118">Add them?"</span></span>
* <span data-ttu-id="db1c6-119">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="db1c6-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "TodoApi".](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="db1c6-123">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="db1c6-123">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="db1c6-124">W przeglądarce przejdź do http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="db1c6-124">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="db1c6-125">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="db1c6-125">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="db1c6-126">Zobacz [pomocy programu Visual Studio Code](#visual-studio-code-help) porady na temat używania w kodzie VS.</span><span class="sxs-lookup"><span data-stu-id="db1c6-126">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="db1c6-127">Dodaj obsługę Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="db1c6-127">Add support for Entity Framework Core</span></span>

:::moniker range="<= aspnetcore-2.0"
<span data-ttu-id="db1c6-128">Tworzenie nowego projektu w programie ASP.NET 2.0 Core dodaje [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) odwołanie do pakietu *TodoApi.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="db1c6-128">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="db1c6-129">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="db1c6-129">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
<span data-ttu-id="db1c6-130">Tworzenie nowego projektu platformy ASP.NET Core 2.1 lub nowszym dodaje [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) odwołanie do pakietu *TodoApi.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="db1c6-130">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="db1c6-131">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="db1c6-131">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end

<span data-ttu-id="db1c6-132">Nie istnieje potrzeba do zainstalowania [Entity Framework Core InMemory](/ef/core/providers/in-memory/) bazy danych dostawcy osobno.</span><span class="sxs-lookup"><span data-stu-id="db1c6-132">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="db1c6-133">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="db1c6-133">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="db1c6-134">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="db1c6-134">Add a model class</span></span>

<span data-ttu-id="db1c6-135">Model jest obiekt reprezentujący dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db1c6-135">A model is an object representing the data in your app.</span></span> <span data-ttu-id="db1c6-136">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="db1c6-136">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="db1c6-137">Dodaj folder o nazwie *modele*.</span><span class="sxs-lookup"><span data-stu-id="db1c6-137">Add a folder named *Models*.</span></span> <span data-ttu-id="db1c6-138">Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="db1c6-138">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="db1c6-139">Dodaj `TodoItem` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="db1c6-139">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="db1c6-140">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="db1c6-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="db1c6-141">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="db1c6-141">Create the database context</span></span>

<span data-ttu-id="db1c6-142">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="db1c6-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="db1c6-143">Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="db1c6-143">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="db1c6-144">Dodaj `TodoContext` klasy w *modele* folderu:</span><span class="sxs-lookup"><span data-stu-id="db1c6-144">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="db1c6-145">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="db1c6-145">Add a controller</span></span>

<span data-ttu-id="db1c6-146">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="db1c6-146">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="db1c6-147">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="db1c6-147">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="db1c6-148">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="db1c6-148">Launch the app</span></span>

<span data-ttu-id="db1c6-149">W kodzie VS naciśnij klawisz F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="db1c6-149">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="db1c6-150">Przejdź do http://localhost:5000/api/todo ( `Todo` kontrolera utworzyliśmy).</span><span class="sxs-lookup"><span data-stu-id="db1c6-150">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="db1c6-151">Visual Studio Code pomocy</span><span class="sxs-lookup"><span data-stu-id="db1c6-151">Visual Studio Code help</span></span>

* [<span data-ttu-id="db1c6-152">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="db1c6-152">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="db1c6-153">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="db1c6-153">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="db1c6-154">Zintegrowane terminali</span><span class="sxs-lookup"><span data-stu-id="db1c6-154">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="db1c6-155">Skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="db1c6-155">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="db1c6-156">System macOS skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="db1c6-156">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="db1c6-157">Skróty klawiaturowe systemu Linux</span><span class="sxs-lookup"><span data-stu-id="db1c6-157">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="db1c6-158">Skróty klawiaturowe systemu Windows</span><span class="sxs-lookup"><span data-stu-id="db1c6-158">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
