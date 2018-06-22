---
title: Tworzenie składnika Web API platformy ASP.NET Core i kodu programu Visual Studio
author: rick-anderson
description: Tworzenie interfejsu API sieci web na macOS, Linux lub Windows z platformy ASP.NET Core MVC i Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4c41c949a9b5ca8db8928a0a53aff928fd7c8a4e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291676"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="35a7c-103">Tworzenie składnika Web API platformy ASP.NET Core i kodu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35a7c-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="35a7c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="35a7c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="35a7c-105">W tym samouczku kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="35a7c-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="35a7c-106">Interfejs użytkownika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="35a7c-106">A UI isn't constructed.</span></span>

<span data-ttu-id="35a7c-107">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="35a7c-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="35a7c-108">System macOS, Linux, Windows: interfejsu API sieci Web za pomocą programu Visual Studio Code (w tym samouczku)</span><span class="sxs-lookup"><span data-stu-id="35a7c-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="35a7c-109">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="35a7c-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="35a7c-110">System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="35a7c-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="35a7c-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="35a7c-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="35a7c-112">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="35a7c-112">Create the project</span></span>

<span data-ttu-id="35a7c-113">W konsoli uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="35a7c-113">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="35a7c-114">*TodoApi* zostanie otwarty folder w Visual Studio (kod VS).</span><span class="sxs-lookup"><span data-stu-id="35a7c-114">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="35a7c-115">Wybierz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="35a7c-115">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="35a7c-116">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"TodoApi".</span><span class="sxs-lookup"><span data-stu-id="35a7c-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="35a7c-117">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="35a7c-117">Add them?"</span></span>
* <span data-ttu-id="35a7c-118">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="35a7c-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "TodoApi".](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="35a7c-122">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="35a7c-122">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="35a7c-123">W przeglądarce przejdź do http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="35a7c-123">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="35a7c-124">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="35a7c-124">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="35a7c-125">Zobacz [pomocy programu Visual Studio Code](#visual-studio-code-help) porady na temat używania w kodzie VS.</span><span class="sxs-lookup"><span data-stu-id="35a7c-125">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="35a7c-126">Dodaj obsługę Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="35a7c-126">Add support for Entity Framework Core</span></span>

:::moniker range="<= aspnetcore-2.0"
<span data-ttu-id="35a7c-127">Tworzenie nowego projektu w programie ASP.NET 2.0 Core dodaje [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) odwołanie do pakietu *TodoApi.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="35a7c-127">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="35a7c-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="35a7c-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
<span data-ttu-id="35a7c-129">Tworzenie nowego projektu platformy ASP.NET Core 2.1 lub nowszym dodaje [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) odwołanie do pakietu *TodoApi.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="35a7c-129">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="35a7c-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="35a7c-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end

<span data-ttu-id="35a7c-131">Nie istnieje potrzeba do zainstalowania [Entity Framework Core InMemory](/ef/core/providers/in-memory/) bazy danych dostawcy osobno.</span><span class="sxs-lookup"><span data-stu-id="35a7c-131">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="35a7c-132">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="35a7c-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="35a7c-133">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="35a7c-133">Add a model class</span></span>

<span data-ttu-id="35a7c-134">Model jest obiekt reprezentujący dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35a7c-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="35a7c-135">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="35a7c-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="35a7c-136">Dodaj folder o nazwie *modele*.</span><span class="sxs-lookup"><span data-stu-id="35a7c-136">Add a folder named *Models*.</span></span> <span data-ttu-id="35a7c-137">Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="35a7c-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="35a7c-138">Dodaj `TodoItem` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="35a7c-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="35a7c-139">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="35a7c-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="35a7c-140">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="35a7c-140">Create the database context</span></span>

<span data-ttu-id="35a7c-141">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="35a7c-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="35a7c-142">Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="35a7c-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="35a7c-143">Dodaj `TodoContext` klasy w *modele* folderu:</span><span class="sxs-lookup"><span data-stu-id="35a7c-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="35a7c-144">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="35a7c-144">Add a controller</span></span>

<span data-ttu-id="35a7c-145">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="35a7c-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="35a7c-146">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="35a7c-146">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="35a7c-147">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="35a7c-147">Launch the app</span></span>

<span data-ttu-id="35a7c-148">W kodzie VS naciśnij klawisz F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="35a7c-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="35a7c-149">Przejdź do http://localhost:5000/api/todo ( `Todo` kontrolera utworzyliśmy).</span><span class="sxs-lookup"><span data-stu-id="35a7c-149">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="35a7c-150">Visual Studio Code pomocy</span><span class="sxs-lookup"><span data-stu-id="35a7c-150">Visual Studio Code help</span></span>

* [<span data-ttu-id="35a7c-151">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="35a7c-151">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="35a7c-152">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="35a7c-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="35a7c-153">Zintegrowane terminali</span><span class="sxs-lookup"><span data-stu-id="35a7c-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="35a7c-154">Skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="35a7c-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="35a7c-155">System macOS skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="35a7c-155">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="35a7c-156">Skróty klawiaturowe systemu Linux</span><span class="sxs-lookup"><span data-stu-id="35a7c-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="35a7c-157">Skróty klawiaturowe systemu Windows</span><span class="sxs-lookup"><span data-stu-id="35a7c-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
