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
ms.openlocfilehash: 40f9259101e5d006378562a27e97948641e29450
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="4224f-104">Utwórz interfejs API sieci Web platformy ASP.NET Core MVC i Visual Studio Code w systemie Linux, macOS i systemu Windows</span><span class="sxs-lookup"><span data-stu-id="4224f-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="4224f-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="4224f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="4224f-106">W tym samouczku będziesz kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="4224f-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="4224f-107">Nie Konstruuj interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4224f-107">You won’t build a UI.</span></span>

<span data-ttu-id="4224f-108">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="4224f-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="4224f-109">System macOS, Linux, Windows: interfejsu API sieci Web za pomocą programu Visual Studio Code (w tym samouczku)</span><span class="sxs-lookup"><span data-stu-id="4224f-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="4224f-110">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="4224f-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="4224f-111">System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="4224f-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="4224f-112">Konfigurowanie środowiska deweloperskiego</span><span class="sxs-lookup"><span data-stu-id="4224f-112">Set up your development environment</span></span>

<span data-ttu-id="4224f-113">Pobierz i zainstaluj:</span><span class="sxs-lookup"><span data-stu-id="4224f-113">Download and install:</span></span>
- <span data-ttu-id="4224f-114">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="4224f-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="4224f-115">Kod Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4224f-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="4224f-116">Visual Studio Code [rozszerzenia C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="4224f-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="4224f-117">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="4224f-117">Create the project</span></span>

<span data-ttu-id="4224f-118">W konsoli uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="4224f-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="4224f-119">Otwórz *TodoApi* folderu w Visual Studio (kod VS) i wybierz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="4224f-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="4224f-120">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"TodoApi".</span><span class="sxs-lookup"><span data-stu-id="4224f-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="4224f-121">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="4224f-121">Add them?"</span></span>
- <span data-ttu-id="4224f-122">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="4224f-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "TodoApi".](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="4224f-126">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="4224f-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="4224f-127">W przeglądarce przejdź do http://localhost: 5000/api/wartości.</span><span class="sxs-lookup"><span data-stu-id="4224f-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="4224f-128">Wyświetlane są następujące:</span><span class="sxs-lookup"><span data-stu-id="4224f-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="4224f-129">Zobacz [pomocy programu Visual Studio Code](#visual-studio-code-help) porady na temat używania w kodzie VS.</span><span class="sxs-lookup"><span data-stu-id="4224f-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="4224f-130">Dodaj obsługę Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4224f-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="4224f-131">Tworzenie nowego projektu w programie .NET Core 2.0 dodaje dostawcę "Microsoft.AspNetCore.All" w *TodoApi.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="4224f-131">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="4224f-132">Nie istnieje potrzeba do zainstalowania [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) bazy danych dostawcy osobno.</span><span class="sxs-lookup"><span data-stu-id="4224f-132">There is no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="4224f-133">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="4224f-133">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="4224f-134">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="4224f-134">Add a model class</span></span>

<span data-ttu-id="4224f-135">Model jest obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4224f-135">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="4224f-136">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="4224f-136">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="4224f-137">Dodaj folder o nazwie *modele*.</span><span class="sxs-lookup"><span data-stu-id="4224f-137">Add a folder named *Models*.</span></span> <span data-ttu-id="4224f-138">Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="4224f-138">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="4224f-139">Dodaj `TodoItem` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4224f-139">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="4224f-140">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="4224f-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="4224f-141">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="4224f-141">Create the database context</span></span>

<span data-ttu-id="4224f-142">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="4224f-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="4224f-143">Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="4224f-143">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="4224f-144">Dodaj `TodoContext` klasy w *modele* folderu:</span><span class="sxs-lookup"><span data-stu-id="4224f-144">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="4224f-145">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="4224f-145">Add a controller</span></span>

<span data-ttu-id="4224f-146">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="4224f-146">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="4224f-147">Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="4224f-147">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="4224f-148">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="4224f-148">Launch the app</span></span>

<span data-ttu-id="4224f-149">W kodzie VS naciśnij klawisz F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="4224f-149">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="4224f-150">Przejdź do http://localhost: 5000/api/todo ( `Todo` kontrolera właśnie utworzyliśmy).</span><span class="sxs-lookup"><span data-stu-id="4224f-150">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="4224f-151">Visual Studio Code pomocy</span><span class="sxs-lookup"><span data-stu-id="4224f-151">Visual Studio Code help</span></span>

- [<span data-ttu-id="4224f-152">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="4224f-152">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="4224f-153">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="4224f-153">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="4224f-154">Zintegrowane terminali</span><span class="sxs-lookup"><span data-stu-id="4224f-154">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="4224f-155">Skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="4224f-155">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="4224f-156">Skróty klawiaturowe Mac</span><span class="sxs-lookup"><span data-stu-id="4224f-156">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="4224f-157">Skróty klawiaturowe systemu Linux</span><span class="sxs-lookup"><span data-stu-id="4224f-157">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="4224f-158">Skróty klawiaturowe systemu Windows</span><span class="sxs-lookup"><span data-stu-id="4224f-158">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


