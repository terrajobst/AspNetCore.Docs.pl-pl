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
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a>Utwórz interfejs API sieci Web platformy ASP.NET Core MVC i Visual Studio Code w systemie Linux, macOS i systemu Windows

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)

W tym samouczku będziesz kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania". Nie Konstruuj interfejsu użytkownika.

Istnieją 3 wersje tego samouczka:

* System macOS, Linux, Windows: interfejsu API sieci Web za pomocą programu Visual Studio Code (w tym samouczku)
* System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)
* System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a>Konfigurowanie środowiska deweloperskiego

Pobierz i zainstaluj:
- [Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.
- [Kod Visual Studio](https://code.visualstudio.com)
- Visual Studio Code [rozszerzenia C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

## <a name="create-the-project"></a>Utwórz projekt

W konsoli uruchom następujące polecenia:

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

Otwórz *TodoApi* folderu w Visual Studio (kod VS) i wybierz *Startup.cs* pliku.

- Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"TodoApi". Dodaj je?"
- Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "TodoApi". Czy chcesz je dodać? Nie pytaj ponownie, nie teraz tak](web-api-vsc/_static/vsc_restore.png)

Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program. W przeglądarce przejdź do http://localhost: 5000/api/wartości. Wyświetlane są następujące:

`["value1","value2"]`

Zobacz [pomocy programu Visual Studio Code](#visual-studio-code-help) porady na temat używania w kodzie VS.

## <a name="add-support-for-entity-framework-core"></a>Dodaj obsługę Entity Framework Core

Tworzenie nowego projektu w programie .NET Core 2.0 dodaje dostawcę "Microsoft.AspNetCore.All" w *TodoApi.csproj* pliku. Nie istnieje potrzeba do zainstalowania [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) bazy danych dostawcy osobno. Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a>Dodaj klasę modelu

Model jest obiekt, który reprezentuje dane w aplikacji. W takim przypadku tylko model jest zadanie do wykonania.

Dodaj folder o nazwie *modele*. Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.

Dodaj `TodoItem` klasy następującym kodem:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.

## <a name="create-the-database-context"></a>Tworzenie kontekstu bazy danych

*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych. Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.

Dodaj `TodoContext` klasy w *modele* folderu:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Dodawanie kontrolera

W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`. Dodaj następujący kod:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uruchom aplikację

W kodzie VS naciśnij klawisz F5, aby uruchomić aplikację. Przejdź do http://localhost: 5000/api/todo ( `Todo` kontrolera właśnie utworzyliśmy).

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Visual Studio Code pomocy

- [Wprowadzenie](https://code.visualstudio.com/docs)
- [Debugowanie](https://code.visualstudio.com/docs/editor/debugging)
- [Zintegrowane terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Skróty klawiaturowe](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Skróty klawiaturowe Mac](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Skróty klawiaturowe systemu Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Skróty klawiaturowe systemu Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


