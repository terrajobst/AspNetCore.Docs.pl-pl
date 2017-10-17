---
title: "Wprowadzenie do korzystania z elementu Razor strony platformy ASP.NET Core dla komputerów Mac"
author: rick-anderson
description: "Wprowadzenie do korzystania z Razor strony platformy ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac"
keywords: "Platformy ASP.NET Core, stron Razor, tworzenia szkieletu, Entity Framework Core, EF, EF Core, bazy danych, mac, macOS, Visual Studio dla komputerów Mac"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 9431e8160011d1485925db5cc4f9f83bf7381e97
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/19/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a>Wprowadzenie do korzystania z Razor strony platformy ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony. Firma Microsoft zaleca, należy wykonać [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka. Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące czynności:

* [Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy
* [Visual Studio dla komputerów Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

W terminalu uruchom następujące polecenia:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektu stron Razor. Otwórz w przeglądarce http://localhost: 5000, aby wyświetlić aplikację.

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Otwórz projekt

Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.

W programie Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz *RazorPagesMovie.csproj* pliku.

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji. Uruchamia program Visual Studio [usług IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview)spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:5000`.

W następnym samouczku modelu dodania do projektu.

>[!div class="step-by-step"]
[Następnie: Dodawanie modelu](xref:tutorials/razor-pages-mac/model)
