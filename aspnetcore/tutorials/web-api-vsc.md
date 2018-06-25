---
title: Tworzenie składnika Web API platformy ASP.NET Core i kodu programu Visual Studio
author: rick-anderson
description: Tworzenie interfejsu API sieci web na macOS, Linux lub Windows z platformy ASP.NET Core MVC i Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4c41c949a9b5ca8db8928a0a53aff928fd7c8a4e
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36291676"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>Tworzenie składnika Web API platformy ASP.NET Core i kodu programu Visual Studio

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)

W tym samouczku kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania". Interfejs użytkownika nie jest tworzony.

Istnieją trzy wersje tego samouczka:

* System macOS, Linux, Windows: interfejsu API sieci Web za pomocą programu Visual Studio Code (w tym samouczku)
* System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)
* System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a>Utwórz projekt

W konsoli uruchom następujące polecenia:

```console
dotnet new webapi -o TodoApi
code TodoApi
```

*TodoApi* zostanie otwarty folder w Visual Studio (kod VS). Wybierz *Startup.cs* pliku.

* Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"TodoApi". Dodaj je?"
* Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "TodoApi". Czy chcesz je dodać? Nie pytaj ponownie, nie teraz tak](web-api-vsc/_static/vsc_restore.png)

Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program. W przeglądarce przejdź do http://localhost:5000/api/values. Wyświetlane są następujące dane wyjściowe:

```json
["value1","value2"]
```

Zobacz [pomocy programu Visual Studio Code](#visual-studio-code-help) porady na temat używania w kodzie VS.

## <a name="add-support-for-entity-framework-core"></a>Dodaj obsługę Entity Framework Core

:::moniker range="<= aspnetcore-2.0"
Tworzenie nowego projektu w programie ASP.NET 2.0 Core dodaje [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) odwołanie do pakietu *TodoApi.csproj* pliku:

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
Tworzenie nowego projektu platformy ASP.NET Core 2.1 lub nowszym dodaje [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) odwołanie do pakietu *TodoApi.csproj* pliku:

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end

Nie istnieje potrzeba do zainstalowania [Entity Framework Core InMemory](/ef/core/providers/in-memory/) bazy danych dostawcy osobno. Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.

## <a name="add-a-model-class"></a>Dodaj klasę modelu

Model jest obiekt reprezentujący dane w aplikacji. W takim przypadku tylko model jest zadanie do wykonania.

Dodaj folder o nazwie *modele*. Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.

Dodaj `TodoItem` klasy następującym kodem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.

## <a name="create-the-database-context"></a>Tworzenie kontekstu bazy danych

*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych. Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.

Dodaj `TodoContext` klasy w *modele* folderu:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Dodawanie kontrolera

W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`. Zastąp jego zawartość następującym kodem:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uruchom aplikację

W kodzie VS naciśnij klawisz F5, aby uruchomić aplikację. Przejdź do http://localhost:5000/api/todo ( `Todo` kontrolera utworzyliśmy).

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Visual Studio Code pomocy

* [Wprowadzenie](https://code.visualstudio.com/docs)
* [Debugowanie](https://code.visualstudio.com/docs/editor/debugging)
* [Zintegrowane terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Skróty klawiaturowe](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [System macOS skróty klawiaturowe](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Skróty klawiaturowe systemu Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Skróty klawiaturowe systemu Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
