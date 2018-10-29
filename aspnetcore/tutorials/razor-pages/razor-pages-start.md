---
title: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać stron Razor w programie ASP.NET Core. Dowiedz się, jak utworzyć model, generowanie kodu dla stron Razor, platformy Entity Framework Core i SQL Server na użytek dostęp do danych, dodać funkcje wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracje do aktualizacji modelu.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 4ee9a014114e2536f7584b2a1ff9d699fb971aa8
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206981"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-2.0"

Zaleca się wykonać wersji platformy ASP.NET Core 2.1 w tym samouczku. Ma ona **znacznie** łatwiejszej do obserwowania i obejmuje więcej funkcji. Wybierz **platformy ASP.NET Core 2.1** w selektorze wersja.

![Selektor wersji w spisie treści](razor-pages-start/_static/v21.png)

::: moniker-end

W tym samouczku pokazano podstawy tworzenia aplikacji sieci web programu ASP.NET Core Razor strony. Strony razor jest zalecany sposób tworzenia interfejsu użytkownika dla aplikacji sieci web w programie ASP.NET Core.

Istnieją trzy wersje tego samouczka:

* Windows: W tym samouczku.
* System MacOS: [Rozpoczynanie pracy ze stronami Razor z programem Visual Studio dla komputerów Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* System macOS, Linux i Windows: [Rozpoczynanie pracy ze stronami ASP.NET Core Razor w programie Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core. Nadaj projektowi nazwę **RazorPagesMovie**. Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne, jeśli kopiujesz/wklejasz kod.
 ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2.1.png)
* Wybierz **platformy ASP.NET Core 2.1** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.

 ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

Szablon programu Visual Studio tworzy projekt startowy:

![Eksplorator rozwiązań](razor-pages-start/_static/se2.1.png)

Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** do uruchomienia bez dołączania debugera. Wybierz **Akceptuj** do wyrażenia zgody na śledzenie. Ta aplikacja nie może śledzić informacje osobiste. Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).

![Strona główna lub indeks](razor-pages-start/_static/homeGDPR.png)

Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:

![Strona główna lub indeks](razor-pages-start/_static/home2.1.png)

* Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji. Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego. Localhost obsługują tylko żądania sieci web z komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Na wcześniejszej ilustracji numer portu to 5001. Po uruchomieniu aplikacji, zobaczysz inny numer portu.
* Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core. Nadaj projektowi nazwę **RazorPagesMovie**. Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne, jeśli kopiujesz/wklejasz kod.
  ![Nowa aplikacja internetowa ASP.NET Core](../../razor-pages/index/_static/np.png)
* Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

Szablon programu Visual Studio tworzy projekt startowy:

![Eksplorator rozwiązań](razor-pages-start/_static/se.png)

Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** do uruchomienia bez dołączania debugera

![Strona główna lub indeks](razor-pages-start/_static/home.png)

* Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację. Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego. Localhost obsługują tylko żądania sieci web z komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Na wcześniejszej ilustracji numer portu to 5000. Po uruchomieniu aplikacji, zobaczysz inny numer portu.
* Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [Następnie: Dodawanie modelu](xref:tutorials/razor-pages/model)
