---
title: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core w systemie macOS przy użyciu programu Visual Studio dla komputerów Mac
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę ze stronami Razor w programie ASP.NET Core przy użyciu programu Visual Studio dla komputerów Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 8a4cc6c52684558ce9176f98205e439096e88cbf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089654"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core w systemie macOS przy użyciu programu Visual Studio dla komputerów Mac

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku pokazano podstawy tworzenia aplikacji sieci web programu ASP.NET Core Razor strony. Zalecane jest przejrzenie [wprowadzenie do stron Razor](xref:razor-pages/index) przed rozpoczęciem tego samouczka. Strony razor jest zalecany sposób tworzenia interfejsu użytkownika dla aplikacji sieci web w programie ASP.NET Core.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

W terminalu uruchom następujące polecenia:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektów stron Razor. Otwórz w przeglądarce http://localhost:5000 Aby wyświetlić aplikację.

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Otwórz projekt

Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.

Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *RazorPagesMovie.csproj* pliku.

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji. Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5000`.

W następnym samouczku dodamy modelu do projektu.

> [!div class="step-by-step"]
> [Następnie: Dodawanie modelu](xref:tutorials/razor-pages-mac/model)
