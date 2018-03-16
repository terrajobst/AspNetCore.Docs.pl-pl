---
title: Tworzenie interfejsu API sieci Web platformy ASP.NET Core i kodzie VS
author: rick-anderson
description: Tworzenie interfejsu API sieci web na macOS, Linux lub Windows z platformy ASP.NET Core MVC i Visual Studio Code
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 12b1c3cea5101b4da673a1ad82cc1ad461f2a38d
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="b78d2-103">Utwórz interfejs API sieci Web platformy ASP.NET Core MVC i Visual Studio Code w systemie Linux, macOS i systemu Windows</span><span class="sxs-lookup"><span data-stu-id="b78d2-103">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="b78d2-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="b78d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="b78d2-105">W tym samouczku kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="b78d2-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="b78d2-106">Interfejs użytkownika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="b78d2-106">A UI isn't constructed.</span></span>

<span data-ttu-id="b78d2-107">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="b78d2-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="b78d2-108">System macOS, Linux, Windows: interfejsu API sieci Web za pomocą programu Visual Studio Code (w tym samouczku)</span><span class="sxs-lookup"><span data-stu-id="b78d2-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="b78d2-109">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="b78d2-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="b78d2-110">System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="b78d2-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="b78d2-111">Konfigurowanie środowiska deweloperskiego</span><span class="sxs-lookup"><span data-stu-id="b78d2-111">Set up your development environment</span></span>

<span data-ttu-id="b78d2-112">Pobierz i zainstaluj:</span><span class="sxs-lookup"><span data-stu-id="b78d2-112">Download and install:</span></span>
- <span data-ttu-id="b78d2-113">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="b78d2-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="b78d2-114">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b78d2-114">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="b78d2-115">Visual Studio Code [rozszerzenia C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="b78d2-115">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="b78d2-116">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="b78d2-116">Create the project</span></span>

<span data-ttu-id="b78d2-117">W konsoli uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b78d2-117">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="b78d2-118">Otwórz *TodoApi* folderu w Visual Studio (kod VS) i wybierz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="b78d2-118">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="b78d2-119">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"TodoApi".</span><span class="sxs-lookup"><span data-stu-id="b78d2-119">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="b78d2-120">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="b78d2-120">Add them?"</span></span>
- <span data-ttu-id="b78d2-121">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="b78d2-121">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "TodoApi".](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="b78d2-125">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="b78d2-125">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="b78d2-126">W przeglądarce przejdź do http://localhost: 5000/api/wartości.</span><span class="sxs-lookup"><span data-stu-id="b78d2-126">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="b78d2-127">Wyświetlane są następujące:</span><span class="sxs-lookup"><span data-stu-id="b78d2-127">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="b78d2-128">Zobacz [pomocy programu Visual Studio Code](#visual-studio-code-help) porady na temat używania w kodzie VS.</span><span class="sxs-lookup"><span data-stu-id="b78d2-128">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="b78d2-129">Dodaj obsługę Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b78d2-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="b78d2-130">Tworzenie nowego projektu w programie .NET Core 2.0 dodaje dostawcę "Microsoft.AspNetCore.All" w *TodoApi.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="b78d2-130">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="b78d2-131">Nie istnieje potrzeba do zainstalowania [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) bazy danych dostawcy osobno.</span><span class="sxs-lookup"><span data-stu-id="b78d2-131">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="b78d2-132">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="b78d2-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="b78d2-133">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="b78d2-133">Add a model class</span></span>

<span data-ttu-id="b78d2-134">Model jest obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b78d2-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="b78d2-135">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="b78d2-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="b78d2-136">Dodaj folder o nazwie *modele*.</span><span class="sxs-lookup"><span data-stu-id="b78d2-136">Add a folder named *Models*.</span></span> <span data-ttu-id="b78d2-137">Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="b78d2-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="b78d2-138">Dodaj `TodoItem` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b78d2-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="b78d2-139">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="b78d2-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="b78d2-140">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="b78d2-140">Create the database context</span></span>

<span data-ttu-id="b78d2-141">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="b78d2-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="b78d2-142">Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="b78d2-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="b78d2-143">Dodaj `TodoContext` klasy w *modele* folderu:</span><span class="sxs-lookup"><span data-stu-id="b78d2-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="b78d2-144">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="b78d2-144">Add a controller</span></span>

<span data-ttu-id="b78d2-145">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="b78d2-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="b78d2-146">Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="b78d2-146">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="b78d2-147">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="b78d2-147">Launch the app</span></span>

<span data-ttu-id="b78d2-148">W kodzie VS naciśnij klawisz F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="b78d2-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="b78d2-149">Przejdź do http://localhost: 5000/api/todo ( `Todo` kontrolera właśnie utworzyliśmy).</span><span class="sxs-lookup"><span data-stu-id="b78d2-149">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="b78d2-150">Visual Studio Code pomocy</span><span class="sxs-lookup"><span data-stu-id="b78d2-150">Visual Studio Code help</span></span>

- [<span data-ttu-id="b78d2-151">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="b78d2-151">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="b78d2-152">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="b78d2-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="b78d2-153">Zintegrowane terminali</span><span class="sxs-lookup"><span data-stu-id="b78d2-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="b78d2-154">Skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="b78d2-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="b78d2-155">System macOS skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="b78d2-155">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="b78d2-156">Skróty klawiaturowe systemu Linux</span><span class="sxs-lookup"><span data-stu-id="b78d2-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="b78d2-157">Skróty klawiaturowe systemu Windows</span><span class="sxs-lookup"><span data-stu-id="b78d2-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


