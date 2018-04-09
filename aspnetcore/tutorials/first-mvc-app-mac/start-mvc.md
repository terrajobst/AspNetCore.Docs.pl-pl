---
title: Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio dla komputerów Mac
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z platformy ASP.NET Core MVC i Visual Studio
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: ffa620f07251c52c785672d8fbeefacac31ed4c1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio dla komputerów Mac

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku uczy podstaw konstruowania platformy ASP.NET Core MVC aplikację sieciową przy użyciu [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/). 

[!INCLUDE [consider RP](../../includes/razor.md)]

Istnieją 3 wersje tego samouczka:

* System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* System Windows: [kompilacji platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci web

W programie Visual Studio, wybierz **Plik > nowe rozwiązanie**.

![System macOS nowego rozwiązania](../first-web-api-mac/_static/sln.png)

Wybierz **aplikacji .NET Core > platformy ASP.NET Core > sieci Web aplikacji > Dalej**.

![okno dialogowe macOS nowego projektu](start-mvc/1.png)

Nazwij projekt **MvcMovie**, a następnie wybierz **Utwórz**.

![okno dialogowe macOS nowego projektu](start-mvc/2.png)

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji. Uruchamia program Visual Studio [Kestrel](xref:fundamentals/servers/index#kestrel)spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest liczbą losowo wybranego portu.

![Przeglądarki za pomocą nowego projektu](start-mvc/b1.png)

* Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.
* Możesz uruchomić aplikację w trybie bez debugowania z lub debugowania **Uruchom** menu.

Domyślny szablon umożliwia **macierzystego o** i **skontaktuj się z** łącza. Powyższy obraz przeglądarki nie wyświetla tych łączy. W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.

![Przeglądarki za pomocą nowego projektu](start-mvc/b2.png)

W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

> [!div class="step-by-step"]
> [Next](adding-controller.md)  
