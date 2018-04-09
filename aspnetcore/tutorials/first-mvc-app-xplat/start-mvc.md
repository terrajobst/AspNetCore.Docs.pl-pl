---
title: Wprowadzenie do platformy ASP.NET Core MVC macOS, Linux lub Windows
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z platformy ASP.NET Core MVC i Visual Studio Code na macOS, Linux i Windows
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 50fbd54c6b0cc1146271afda7e45a0dab590dd7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a>Wprowadzenie do platformy ASP.NET Core MVC macOS, Linux lub Windows

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku udzieli Ci podstawy platformy ASP.NET Core MVC sieci web aplikacji za pomocą [Visual Studio Code](https://code.visualstudio.com) (VS kodu). Samouczek jest przeznaczony dla familarity z kodem VS. Zobacz [wprowadzenie kodu VS](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji. 

[!INCLUDE [consider RP](../../includes/razor.md)]

Istnieją 3 wersje tego samouczka:

* System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* System Windows: [utworzyć platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC z kodem Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a>Tworzenie aplikacji sieci web w usłudze dotnet

W terminalu uruchom następujące polecenia:

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Otwórz *MvcMovie* folderu w Visual Studio (kod VS) i wybierz *Startup.cs* pliku.

- Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"MvcMovie". Dodaj je?"
- Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "MvcMovie". Czy chcesz je dodać? Nie pytaj ponownie, nie teraz, tak jak również informacje — istnieją nierozwiązane zależności - Restore - Zamknij](../web-api-vsc/_static/vsc_restore.png)

Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.

![uruchomionej aplikacji](../first-mvc-app/start-mvc/_static/1.png)

Uruchamia kod VS [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web i uruchamia aplikację. Należy zauważyć, że na pasku adresu `localhost:5000` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.

Domyślny szablon umożliwia pracy **macierzystego o** i **skontaktuj się z** łącza. Powyższy obraz przeglądarki nie wyświetla tych łączy. W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.

![Ikona nawigacji w prawym górnym rogu](../first-mvc-app/start-mvc/_static/2.png)

W następnej części tego samouczka możemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

## <a name="visual-studio-code-help"></a>Visual Studio Code pomocy

- [Wprowadzenie](https://code.visualstudio.com/docs)
- [Debugowanie](https://code.visualstudio.com/docs/editor/debugging)
- [Zintegrowane terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Skróty klawiaturowe](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [System macOS skróty klawiaturowe](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Skróty klawiaturowe systemu Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Skróty klawiaturowe systemu Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [Następne — Dodawanie kontrolera](adding-controller.md)
