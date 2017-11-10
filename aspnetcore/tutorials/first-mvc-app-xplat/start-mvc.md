---
title: Wprowadzenie do platformy ASP.NET Core MVC Mac, Linux lub Windows
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio Code, Mac, Linux i Windows
keywords: Platformy ASP.NET Core MVC, VS, kod programu Visual Studio, Mac, Linux, w systemie Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 87ce5dca011a7bc37d34799db159d933c158cba1
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a>Wprowadzenie do platformy ASP.NET MVC rdzeni na Mac, Linux lub Windows

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku udzieli Ci podstawy platformy ASP.NET Core MVC sieci web aplikacji za pomocą [Visual Studio Code](https://code.visualstudio.com) (VS kodu). Samouczek jest przeznaczony dla familarity z kodem VS. Zobacz [wprowadzenie kodu VS](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji. [!INCLUDE[consider RP](../../includes/razor.md)]

Istnieją 3 wersje tego samouczka:

* System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* System Windows: [utworzyć platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC z kodem Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="install-vs-code-and-net-core"></a>Zainstaluj kodzie VS i .NET Core

Ten samouczek wymaga [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym. Zobacz [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) dla wersji platformy ASP.NET Core 1.1.

Zainstaluj następujące czynności:

* [Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.
* [Kod Visual Studio](https://code.visualstudio.com)
* Kod VS [rozszerzenia C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

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

  - [Skróty klawiaturowe Mac](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Skróty klawiaturowe systemu Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Skróty klawiaturowe systemu Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[Następne — Dodawanie kontrolera](adding-controller.md)
