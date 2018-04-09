---
title: Rozpoczynanie pracy z Razor strony platformy ASP.NET Core na macOS z programem Visual Studio dla komputerów Mac
author: rick-anderson
description: Dowiedzieć się, jak rozpocząć pracę z Razor strony platformy ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac.
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 6d9233c952c632022c0b37ef1ea6a5320e4eeb6d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>Rozpoczynanie pracy z Razor strony platformy ASP.NET Core na macOS z programem Visual Studio dla komputerów Mac

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony. Firma Microsoft zaleca przejrzenie [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka. Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

W terminalu uruchom następujące polecenia:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektu stron Razor. Otwórz w przeglądarce http://localhost:5000 Aby wyświetlić aplikację.

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Otwórz projekt

Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.

W programie Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz *RazorPagesMovie.csproj* pliku.

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji. Uruchamia program Visual Studio [Kestrel](xref:fundamentals/servers/kestrel)spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:5000`.

W następnym samouczku modelu dodania do projektu.

> [!div class="step-by-step"]
> [Następnie: Dodawanie modelu](xref:tutorials/razor-pages-mac/model)
