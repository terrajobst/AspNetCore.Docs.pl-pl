---
title: Rozpoczynanie pracy z Razor strony platformy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony. Stron razor jest zalecane w przypadku obciążeń sieci web w ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 124181d3193bb9e2025b2c1d9f9b8f9995365ad6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Rozpoczynanie pracy z Razor strony platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony. Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.

Istnieją trzy wersje tego samouczka:

* Systemu Windows: W tym samouczku
* System MacOS: [wprowadzenie stron Razor przy użyciu programu Visual Studio dla komputerów Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* System macOS, Linux i Windows: [wprowadzenie do platformy ASP.NET Core Razor stron w programie Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

* W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.
* Utwórz nową aplikację sieci Web platformy ASP.NET Core. Nazwij projekt **RazorPagesMovie**. Ważne jest, aby nazwa projektu *RazorPagesMovie* , przestrzenie nazw będzie dopasowania, gdy użytkownik kopiowanie/wklejanie kodu.
  ![nową aplikację sieci Web Core ASP.NET](../../mvc/razor-pages/index/_static/np.png)
* Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.

  [!INCLUDE [install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

Szablon programu Visual Studio tworzy projekt starter:

![Eksplorator rozwiązań](razor-pages-start/_static/se.png)

Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** działać bez dołączanie debugera

![Strona główna lub indeks](razor-pages-start/_static/home.png)

* Uruchamia program Visual Studio [usług IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację. Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego. Localhost obsługują tylko żądania sieci web z komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Na poprzedniej ilustracji numer portu to 5000. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.
* Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

> [!div class="step-by-step"]
> [Następnie: Dodawanie modelu](xref:tutorials/razor-pages/model)
