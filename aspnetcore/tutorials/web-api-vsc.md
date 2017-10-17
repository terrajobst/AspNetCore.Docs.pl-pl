---
title: Tworzenie interfejsu API sieci Web platformy ASP.NET Core i kodzie VS
description: Tworzenie interfejsu API sieci web na macOS, Linux lub Windows z platformy ASP.NET Core MVC i Visual Studio Code
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: "Platformy ASP.NET Core, WebAPI, interfejs API sieci Web, REST, Mac, Linux, HTTP, usługi, usługi HTTP kodzie VS"
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: caf40ee1c2d45d2fbf33b07d707fa4f1be98d31c
ms.sourcegitcommit: 8b5733f1cd5d2c2b6d432bf82fcd4be2d2d6b2a3
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/28/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="db6c1-104">Utwórz interfejs API sieci Web platformy ASP.NET Core MVC i Visual Studio Code w systemie Linux, macOS i systemu Windows</span><span class="sxs-lookup"><span data-stu-id="db6c1-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="db6c1-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="db6c1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="db6c1-106">W tym samouczku będziesz kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="db6c1-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="db6c1-107">Nie Konstruuj interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="db6c1-107">You won’t build a UI.</span></span>

<span data-ttu-id="db6c1-108">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="db6c1-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="db6c1-109">System macOS, Linux, Windows: interfejsu API sieci Web za pomocą programu Visual Studio Code (w tym samouczku)</span><span class="sxs-lookup"><span data-stu-id="db6c1-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="db6c1-110">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="db6c1-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="db6c1-111">System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="db6c1-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="db6c1-112">Konfigurowanie środowiska deweloperskiego</span><span class="sxs-lookup"><span data-stu-id="db6c1-112">Set up your development environment</span></span>

<span data-ttu-id="db6c1-113">Pobierz i zainstaluj:</span><span class="sxs-lookup"><span data-stu-id="db6c1-113">Download and install:</span></span>
- <span data-ttu-id="db6c1-114">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="db6c1-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="db6c1-115">Kod Visual Studio</span><span class="sxs-lookup"><span data-stu-id="db6c1-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="db6c1-116">Visual Studio Code [rozszerzenia C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="db6c1-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="db6c1-117">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="db6c1-117">Create the project</span></span>

<span data-ttu-id="db6c1-118">W konsoli uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="db6c1-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="db6c1-119">Otwórz *TodoApi* folderu w Visual Studio (kod VS) i wybierz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="db6c1-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="db6c1-120">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"TodoApi".</span><span class="sxs-lookup"><span data-stu-id="db6c1-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="db6c1-121">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="db6c1-121">Add them?"</span></span>
- <span data-ttu-id="db6c1-122">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="db6c1-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "TodoApi".](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="db6c1-126">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="db6c1-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="db6c1-127">W przeglądarce przejdź do http://localhost: 5000/api/wartości.</span><span class="sxs-lookup"><span data-stu-id="db6c1-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="db6c1-128">Wyświetlane są następujące:</span><span class="sxs-lookup"><span data-stu-id="db6c1-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="db6c1-129">Zobacz [pomocy programu Visual Studio Code](#visual-studio-code-help) porady na temat używania w kodzie VS.</span><span class="sxs-lookup"><span data-stu-id="db6c1-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="db6c1-130">Dodaj obsługę Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="db6c1-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="db6c1-131">Edytuj *TodoApi.csproj* plik, aby zainstalować [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="db6c1-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="db6c1-132">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="db6c1-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

<span data-ttu-id="db6c1-133">Uruchom `dotnet restore` do pobrania i zainstalowania dostawcy danych InMemory Core EF.</span><span class="sxs-lookup"><span data-stu-id="db6c1-133">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="db6c1-134">Można uruchomić `dotnet restore` z terminala i wprowadź `⌘⇧P` (macOS) lub `Ctrl+Shift+P` (Linux) w kodzie VS, a następnie wpisz **.NET**.</span><span class="sxs-lookup"><span data-stu-id="db6c1-134">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="db6c1-135">Wybierz **.NET: przywracania pakietów**.</span><span class="sxs-lookup"><span data-stu-id="db6c1-135">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="db6c1-136">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="db6c1-136">Add a model class</span></span>

<span data-ttu-id="db6c1-137">Model jest obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db6c1-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="db6c1-138">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="db6c1-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="db6c1-139">Dodaj folder o nazwie *modele*.</span><span class="sxs-lookup"><span data-stu-id="db6c1-139">Add a folder named *Models*.</span></span> <span data-ttu-id="db6c1-140">Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="db6c1-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="db6c1-141">Dodaj `TodoItem` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="db6c1-141">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="db6c1-142">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="db6c1-142">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="db6c1-143">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="db6c1-143">Create the database context</span></span>

<span data-ttu-id="db6c1-144">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="db6c1-144">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="db6c1-145">Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="db6c1-145">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="db6c1-146">Dodaj `TodoContext` klasy w *modele* folderu:</span><span class="sxs-lookup"><span data-stu-id="db6c1-146">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="db6c1-147">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="db6c1-147">Add a controller</span></span>

<span data-ttu-id="db6c1-148">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="db6c1-148">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="db6c1-149">Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="db6c1-149">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="db6c1-150">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="db6c1-150">Launch the app</span></span>

<span data-ttu-id="db6c1-151">W kodzie VS naciśnij klawisz F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="db6c1-151">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="db6c1-152">Przejdź do http://localhost: 5000/api/todo ( `Todo` kontrolera właśnie utworzyliśmy).</span><span class="sxs-lookup"><span data-stu-id="db6c1-152">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="db6c1-153">Visual Studio Code pomocy</span><span class="sxs-lookup"><span data-stu-id="db6c1-153">Visual Studio Code help</span></span>

- [<span data-ttu-id="db6c1-154">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="db6c1-154">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="db6c1-155">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="db6c1-155">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="db6c1-156">Zintegrowane terminali</span><span class="sxs-lookup"><span data-stu-id="db6c1-156">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="db6c1-157">Skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="db6c1-157">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="db6c1-158">Skróty klawiaturowe Mac</span><span class="sxs-lookup"><span data-stu-id="db6c1-158">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="db6c1-159">Skróty klawiaturowe systemu Linux</span><span class="sxs-lookup"><span data-stu-id="db6c1-159">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="db6c1-160">Skróty klawiaturowe systemu Windows</span><span class="sxs-lookup"><span data-stu-id="db6c1-160">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


