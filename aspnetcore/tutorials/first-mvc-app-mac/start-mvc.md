---
title: "Wprowadzenie do korzystania z platformy ASP.NET Core MVC i Visual Studio dla komputerów Mac"
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio
keywords: "Visual Studio platformy ASP.NET Core MVC, dla komputerów Mac programu Entity Framework"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 028d6d3246908a9cd44a6834449d2fdbc9cae0b8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Wprowadzenie do korzystania z platformy ASP.NET Core MVC i Visual Studio dla komputerów Mac

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku uczy podstaw konstruowania platformy ASP.NET Core MVC aplikację sieciową przy użyciu [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/). [!INCLUDE[consider RP](../../includes/razor.md)]

Istnieją 3 wersje tego samouczka:

* System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* System Windows: [kompilacji platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek wymaga [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym. Zobacz [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) dla wersji platformy ASP.NET Core 1.1.

Zainstaluj następujące czynności:

- [Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy
- [Visual Studio dla komputerów Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci web

W programie Visual Studio, wybierz **Plik > nowe rozwiązanie**.

![System macOS nowego rozwiązania](../first-web-api-mac/_static/sln.png)

Wybierz **aplikacji .NET Core > platformy ASP.NET Core > sieci Web aplikacji > Dalej**.

![okno dialogowe macOS nowego projektu](start-mvc/1.png)

Nazwij projekt **MvcMovie**, a następnie wybierz **Utwórz**.

![okno dialogowe macOS nowego projektu](start-mvc/2.png)

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji. Uruchamia program Visual Studio [usług IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview)spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest liczbą losowo wybranego portu.

![Przeglądarki za pomocą nowego projektu](start-mvc/b1.png)

* Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.
* Możesz uruchomić aplikację w trybie bez debugowania z lub debugowania **Uruchom** menu.

Domyślny szablon umożliwia **macierzystego o** i **skontaktuj się z** łącza. Powyższy obraz przeglądarki nie wyświetla tych łączy. W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.

![Przeglądarki za pomocą nowego projektu](start-mvc/b2.png)

W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

>[!div class="step-by-step"]
[Dalej](adding-controller.md)  
