---
title: Wprowadzenie do platformy ASP.NET Core MVC w systemie macOS, Linux lub Windows
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC i programu Visual Studio Code w systemach macOS, Linux i Windows
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/31/2018
ms.locfileid: "36275278"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a>Wprowadzenie do platformy ASP.NET Core MVC w systemie macOS, Linux lub Windows

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Ta seria samouczków obejmuje podstawy tworzenia, ASP.NET Core MVC sieci web aplikacji za pomocą [programu Visual Studio Code](https://code.visualstudio.com) (VS Code). W samouczku założono familarity z programem VS Code. Zobacz [wprowadzenie do programu VS Code](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji. 

[!INCLUDE [consider RP](../../includes/razor.md)]

Istnieją 3 wersje tego samouczka:

* System macOS: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [tworzenie aplikacji MVC platformy ASP.NET Core z programem Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* System macOS, Linux i Windows: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a>Tworzenie aplikacji sieci web za pomocą polecenia dotnet

W terminalu uruchom następujące polecenia:

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Otwórz *MvcMovie* folderu w programie Visual Studio Code (VS Code) i wybierz pozycję *Startup.cs* pliku.

- Wybierz **tak** do **Ostrzegaj** komunikat "wymagane zasoby do tworzenia i debugowania brakuje"MvcMovie". Dodane?"
- Wybierz **przywrócić** do **informacje** komunikatu "Istnieją nierozwiązane zależności".

![Program VS Code z zasobami Ostrzegaj wymagane do tworzenia i debugowania brakuje "MvcMovie". Dodaj je? Nie pytaj ponownie, nie teraz tak, a także informacje o — istnieją nierozwiązane zależności — Przywracanie — Zamknij](../web-api-vsc/_static/vsc_restore.png)

Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.

![uruchamianie aplikacji](../first-mvc-app/start-mvc/_static/1.png)

Uruchamia program VS Code [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web i uruchamia aplikację. Należy zauważyć, że na pasku adresu `localhost:5000` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.

Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza. Na powyższej ilustracji przeglądarki nie wyświetla tych łączy. W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.

![Ikona Nawigacja w prawym górnym rogu](../first-mvc-app/start-mvc/_static/2.png)

W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

## <a name="visual-studio-code-help"></a>Visual Studio Code pomocy

- [Wprowadzenie](https://code.visualstudio.com/docs)
- [Debugowanie](https://code.visualstudio.com/docs/editor/debugging)
- [Zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Skróty klawiaturowe](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [skróty klawiaturowe dla systemu macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Skróty klawiaturowe w systemie Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Skróty klawiaturowe Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [Następne — Dodawanie kontrolera](adding-controller.md)
