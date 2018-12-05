---
title: Wprowadzenie do ASP.NET Core MVC i programu Visual Studio dla komputerów Mac
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC i programu Visual Studio
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 1e3cc91003ac946ac2e5efcd79e20d9215ded902
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862229"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Wprowadzenie do ASP.NET Core MVC i programu Visual Studio dla komputerów Mac

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku pokazano podstawy tworzenia, ASP.NET Core MVC sieci web aplikacji za pomocą [programu Visual Studio dla komputerów Mac](https://www.visualstudio.com/vs/visual-studio-mac/).

[!INCLUDE [consider RP](../../includes/razor.md)]

Istnieją 3 wersje tego samouczka:

* System macOS: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [kompilacji platformy ASP.NET Core MVC aplikacji za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* Linux, macOS i Windows: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci web

W programie Visual Studio, wybierz **pliku** > **nowe rozwiązanie**.

![Nowe rozwiązanie w systemie macOS](../first-web-api-mac/_static/sln.png)

Wybierz **aplikacji programu .NET Core** > **platformy ASP.NET Core** > **aplikacji internetowej ASP.NET Core (MVC)** > **dalej**.

![okno dialogowe z systemem macOS nowego projektu](start-mvc/1.png)

Nadaj projektowi nazwę **MvcMovie**, a następnie wybierz pozycję **Utwórz**.

![okno dialogowe z systemem macOS nowego projektu](start-mvc/2.png)

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom** > **Rozpocznij bez debugowania** do uruchomienia aplikacji. Uruchomieniu programu Visual Studio [Kestrel](xref:fundamentals/servers/index#kestrel) serwera spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest numer portu wybranego losowo.

![Przeglądarki za pomocą nowego projektu](start-mvc/b1.png)

* Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Po uruchomieniu aplikacji, zobaczysz inny numer portu.
* Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **Uruchom** menu.

Domyślny szablon umożliwia **Home**, **o**, i **skontaktuj się z pomocą** łącza. Powyższej ilustracji przeglądarki nie wyświetla tych łączy. W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.

![Przeglądarki za pomocą nowego projektu](start-mvc/b2.png)

W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

> [!div class="step-by-step"]
> [Next](adding-controller.md)  
