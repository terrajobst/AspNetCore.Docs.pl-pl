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
ms.openlocfilehash: 21f115eec924d5e4b21ad78398c8cbd99e02a0a8
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Wprowadzenie do korzystania z platformy ASP.NET Core MVC i Visual Studio dla komputerów Mac

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku uczy podstaw konstruowania platformy ASP.NET Core MVC aplikację sieciową przy użyciu [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/). 

[!INCLUDE[consider RP](../../includes/razor.md)]

Istnieją 3 wersje tego samouczka:

* System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* System Windows: [kompilacji platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek wymaga [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.

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

W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji. Uruchamia program Visual Studio [Kestrel](xref:fundamentals/servers/index#kestrel)spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest liczbą losowo wybranego portu.

![Przeglądarki za pomocą nowego projektu](start-mvc/b1.png)

* Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.
* Możesz uruchomić aplikację w trybie bez debugowania z lub debugowania **Uruchom** menu.

Domyślny szablon umożliwia **macierzystego o** i **skontaktuj się z** łącza. Powyższy obraz przeglądarki nie wyświetla tych łączy. W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.

![Przeglądarki za pomocą nowego projektu](start-mvc/b2.png)

W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

>[!div class="step-by-step"]
[Dalej](adding-controller.md)  
