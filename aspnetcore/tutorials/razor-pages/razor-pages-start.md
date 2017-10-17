---
title: Wprowadzenie do platformy ASP.NET Core stron Razor
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core stron Razor
keywords: Platformy ASP.NET Core stron Razor, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 9d647657f21dd7e952808e5fe020f7a9e8767cd8
ms.sourcegitcommit: 3ba32b2b6425ed94604cb0f681db0d5bb5f8ad58
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/28/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a>Wprowadzenie do platformy ASP.NET Core stron Razor

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony. Firma Microsoft zaleca, należy wykonać [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka. Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

* W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.
* Utwórz nową aplikację sieci Web platformy ASP.NET Core. Nazwij projekt **RazorPagesMovie**. Ważne jest, aby nazwa projektu *RazorPagesMovie* , przestrzenie nazw będzie dopasowania, gdy użytkownik kopiowanie/wklejanie kodu.
  ![nową aplikację sieci Web Core ASP.NET](../../mvc/razor-pages/index/_static/np.png)
* Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.
  ![Aplikacja sieci Web (Razor strony)](../../mvc/razor-pages/index/_static/np2.png)

Szablon programu Visual Studio tworzy projekt starter:

![Eksplorator rozwiązań](razor-pages-start/_static/se.png)

Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** działać bez dołączanie debugera

![Strona główna lub indeks](razor-pages-start/_static/home.png)

* Uruchamia program Visual Studio [usług IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację. Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego. Localhost obsługują tylko żądania sieci web z komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Na poprzedniej ilustracji numer portu to 5000. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.
* Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[Następnie: Dodawanie modelu](xref:tutorials/razor-pages/model)
