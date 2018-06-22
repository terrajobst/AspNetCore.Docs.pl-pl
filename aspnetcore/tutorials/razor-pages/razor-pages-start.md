---
title: Rozpoczynanie pracy z Razor strony platformy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony. Stron razor jest zalecane w przypadku obciążeń sieci web w ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e317b49f2ad33c392de33bc32a87f67bb8cb72a0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278047"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Rozpoczynanie pracy z Razor strony platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-2.0"

Zaleca się wykonanie wersji platformy ASP.NET Core 2.1 tego samouczka. Ma ona **znacznie** łatwiej wykonaj i obejmuje więcej funkcji. Wybierz **platformy ASP.NET Core 2.1** w selektorze wersji.

![Selektor wersji w spisie treści](razor-pages-start/_static/v21.png)

::: moniker-end

Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony. Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.

Istnieją trzy wersje tego samouczka:

* Systemu Windows: W tym samouczku
* System MacOS: [wprowadzenie stron Razor przy użyciu programu Visual Studio dla komputerów Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* System macOS, Linux i Windows: [wprowadzenie do platformy ASP.NET Core Razor stron w programie Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a>Wymagania wstępne

[! OBEJMUJĄ [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

* W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.
* Utwórz nową aplikację sieci Web platformy ASP.NET Core. Nazwij projekt **RazorPagesMovie**. Ważne jest, aby nazwa projektu *RazorPagesMovie* , przestrzenie nazw będzie dopasowania, gdy użytkownik kopiowanie/wklejanie kodu.
 ![nową aplikację sieci Web Core ASP.NET](razor-pages-start/_static/np_2.1.png)
* Wybierz **platformy ASP.NET Core 2.1** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.

 ![nową aplikację sieci Web Core ASP.NET](razor-pages-start/_static/np_2_2.1.png)

Szablon programu Visual Studio tworzy projekt starter:

![Eksplorator rozwiązań](razor-pages-start/_static/se2.1.png)

Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** działać bez dołączanie debugera. Wybierz **Akceptuj** o zgodę do śledzenia. Ta aplikacja nie śledzi informacje osobiste. Kod szablonu wygenerowanego zawiera zasoby, które ułatwiają korzystanie z [rozporządzenia ogólne ochrony danych (GDPR)](xref:security/gdpr).

![Strona główna lub indeks](razor-pages-start/_static/homeGDPR.png)

Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:

![Strona główna lub indeks](razor-pages-start/_static/home2.1.png)

* Uruchamia program Visual Studio [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację. Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego. Localhost obsługują tylko żądania sieci web z komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Na poprzedniej ilustracji numer portu to 5000. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.
* Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a>Wymagania wstępne

[! OBEJMUJĄ [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

* W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.
* Utwórz nową aplikację sieci Web platformy ASP.NET Core. Nazwij projekt **RazorPagesMovie**. Ważne jest, aby nazwa projektu *RazorPagesMovie* , przestrzenie nazw będzie dopasowania, gdy użytkownik kopiowanie/wklejanie kodu.
  ![nową aplikację sieci Web Core ASP.NET](../../razor-pages/index/_static/np.png)
* Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

Szablon programu Visual Studio tworzy projekt starter:

![Eksplorator rozwiązań](razor-pages-start/_static/se.png)

Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** działać bez dołączanie debugera

![Strona główna lub indeks](razor-pages-start/_static/home.png)

* Uruchamia program Visual Studio [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację. Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego. Localhost obsługują tylko żądania sieci web z komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Na poprzedniej ilustracji numer portu to 5000. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.
* Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [Następnie: Dodawanie modelu](xref:tutorials/razor-pages/model)
