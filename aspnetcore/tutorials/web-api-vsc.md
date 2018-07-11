---
title: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i programu Visual Studio Code
author: rick-anderson
description: Tworzenie interfejsu API sieci web w systemie macOS, Linux lub Windows przy użyciu platformy ASP.NET Core MVC i programu Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4c41c949a9b5ca8db8928a0a53aff928fd7c8a4e
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216240"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i programu Visual Studio Code

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)

W tym samouczku Tworzenie internetowego interfejsu API do zarządzania listę elementów "do wykonania". Interfejs użytkownika nie jest konstruowany.

Istnieją trzy wersje tego samouczka:

* macOS i Linux, Windows: interfejs API sieci Web za pomocą programu Visual Studio Code (w tym samouczku)
* System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)
* Windows: [internetowego interfejsu API z programem Visual Studio for Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a>Utwórz projekt

Z poziomu konsoli uruchom następujące polecenia:

```console
dotnet new webapi -o TodoApi
code TodoApi
```

*TodoApi* zostanie otwarty folder, w programie Visual Studio Code (VS Code). Wybierz *Startup.cs* pliku.

* Wybierz **tak** do **Ostrzegaj** komunikat "wymagane zasoby do tworzenia i debugowania brakuje"TodoApi". Dodane?"
* Wybierz **przywrócić** do **informacje** komunikatu "Istnieją nierozwiązane zależności".

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Program VS Code z zasobami Ostrzegaj wymagane do tworzenia i debugowania brakuje "TodoApi". Dodaj je? Nie pytaj ponownie, nie teraz tak](web-api-vsc/_static/vsc_restore.png)

Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program. W przeglądarce przejdź do http://localhost:5000/api/values. Są wyświetlane następujące dane wyjściowe:

```json
["value1","value2"]
```

Zobacz [pomocy programu Visual Studio Code](#visual-studio-code-help) porady na temat korzystania z programu VS Code.

## <a name="add-support-for-entity-framework-core"></a>Dodaj obsługę platformy Entity Framework Core

:::moniker range="<= aspnetcore-2.0"
Tworzenie nowego projektu w programie ASP.NET Core 2.0 dodaje [pakiet](https://www.nuget.org/packages/Microsoft.AspNetCore.All) odwołanie do pakietu *TodoApi.csproj* pliku:

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
Tworzenie nowego projektu w programie ASP.NET Core 2.1 lub nowszej dodaje [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) odwołanie do pakietu *TodoApi.csproj* pliku:

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end

Nie ma potrzeby instalowania [Entity Framework Core InMemory](/ef/core/providers/in-memory/) bazy danych dostawcy osobno. Ten dostawca bazy danych umożliwia platformy Entity Framework Core ma być używany z bazą danych w pamięci.

## <a name="add-a-model-class"></a>Dodawanie klasy modelu

Model jest obiekt reprezentujący dane w aplikacji. W takim przypadku tylko model jest element do wykonania.

Dodaj folder o nazwie *modeli*. Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modeli* folder jest używany przez Konwencję.

Dodaj `TodoItem` klasy z następującym kodem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Baza danych generuje `Id` podczas `TodoItem` zostanie utworzony.

## <a name="create-the-database-context"></a>Utwórz kontekst bazy danych

*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework. Tworzenie tej klasy, wynikające z `Microsoft.EntityFrameworkCore.DbContext` klasy.

Dodaj `TodoContext` klasy w *modeli* folderu:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Dodawanie kontrolera

W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`. Zastąp jego zawartość następującym kodem:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uruchom aplikację

W programie VS Code naciśnij klawisz F5, aby uruchomić aplikację. Przejdź do http://localhost:5000/api/todo ( `Todo` utworzyliśmy kontrolera).

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Visual Studio Code pomocy

* [Wprowadzenie](https://code.visualstudio.com/docs)
* [Debugowanie](https://code.visualstudio.com/docs/editor/debugging)
* [Zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Skróty klawiaturowe](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [skróty klawiaturowe dla systemu macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Skróty klawiaturowe w systemie Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Skróty klawiaturowe Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
