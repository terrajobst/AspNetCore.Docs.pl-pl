---
uid: signalr/get-started-signalr-core
title: Rozpoczynanie pracy z SignalR platformy ASP.NET Core
author: rachelappel
ms.author: rachelap
description: "W tym samouczku utworzysz aplikację dla platformy ASP.NET Core za pomocą biblioteki SignalR."
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
ms.openlocfilehash: 5e16569aa492e3639aa97abd241610b361fb0c56
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Samouczek: Rozpoczynanie pracy z SignalR dla platformy ASP.NET Core

Przez [Rachel Appel](https://twitter.com/rachelappel)

Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym, za pomocą biblioteki SignalR dla platformy ASP.NET Core.

   ![Rozwiązanie](get-started-signalr-core/_static/signalr-get-started-finished.png)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

W tym samouczku przedstawiono następujące SignalR zadań związanych z projektowaniem:

> [!div class="checklist"]
> * Tworzenie aplikacji sieci web platformy ASP.NET Core.
> * Utwórz koncentratora SignalR do dystrybuowania zawartości do klientów.
> * Biblioteka SignalR JavaScript służy do wysyłania wiadomości i wyświetlać aktualizacje w Centrum.

## <a name="prerequisites"></a>Wymagania wstępne

Należy zainstalować następujące oprogramowanie:

* [1 — zestaw SDK w wersji Preview .NET core 2.1.0](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) lub nowszy
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.6 lub nowszej z obciążeniem, ASP.NET i sieć web development
* [npm](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Tworzenie projektu platformy ASP.NET Core obsługującego SignalR klienta i serwera

1. Użyj **pliku** > **nowy projekt** menu opcję i wybierz polecenie **aplikacji sieci Web platformy ASP.NET Core**. Nazwij projekt `SignalRChat`.

  ![Okno dialogowe nowego projektu w programie Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Wybierz **aplikacji sieci Web** do tworzenia projektu za pomocą stron Razor. Następnie wybierz **Ok**. Upewnij się, że **platformy ASP.NET Core 2.1** wybrano selektora framework, chociaż SignalR działa na starsze wersje programu .NET.

  ![Okno dialogowe nowego projektu w programie Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  Biblioteki, w których hosta SignalR kod po stronie serwera są zawarte w szablonie projektu. Zainstaluj JavaScript po stronie klienta oddzielnie z [npm](https://www.npmjs.com/).

  ```console
   npm install @aspnet/signalr
  ```

3. Kopiuj *signalr.js* z *node_modules\\ @aspnet\signalr\dist\browser*  do *wwwroot\lib* folder w projekcie.

## <a name="create-the-signalr-hub"></a>Tworzenie Centrum SignalR

Koncentrator jest klasa, która służy jako wysokiego poziomu potok, który umożliwia klienta i serwera, wywoływanie metod na siebie.

1. Dodaj do projektu klasę, wybierając **pliku** > **nowy** > **pliku** i wybierając **Visual C# klasy**. 

1. Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Zawiera klasę, właściwości i zdarzeń związanych z zarządzaniem połączeń i grupy, a także wysyłania i odbierania danych.

1. Utwórz `Send` metodę, która wysyła komunikat do wszystkich klientów połączonych rozmów. Zwróć uwagę, zwraca `Task`, ponieważ asynchroniczny SignalR. Kod asynchroniczny skaluje się lepiej.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a>Konfigurowanie projektu do użycia biblioteki SignalR

Serwer SignalR musi być skonfigurowany, tak aby wie, do przekazywania żądań do SignalR.

1. Aby skonfigurować projekt SignalR, zmodyfikuj `ConfigureServices` metoda aplikacji `Startup` przy Wstawianie wywołania do `services.AddSignalR`.

  `services.AddSignalR` dodaje SignalR jako część [oprogramowanie pośredniczące platformy ASP.NET Core](xref:fundamentals/middleware/index) potoku.

1. Do konfigurowania tras sieci hubs przy użyciu `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Utwórz kod klienta SignalR

1. Zamień zawartość *Pages\Index.cshtml* następującym kodem:

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  Poprzedni kod HTML Wyświetla nazwę i pola wiadomości i przycisk przesyłania. Zwróć uwagę, odwołań do skryptów u dołu: odwołanie do biblioteki SignalR i *chat.js*.

1. Dodaj plik JavaScript *wwwroot\js* folder o nazwie *chat.js* i Dodaj do niej następujący kod:

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Uruchamianie aplikacji

1. Wybierz **debugowania** > **uruchomienie bez debugowania** przeglądarkę i załadować witryny sieci Web lokalnie. Skopiuj adres URL na pasku adresu.

1. Otwórz inne wystąpienie przeglądarki (dowolnej przeglądarki) i wklej adres URL na pasku adresu.

1. Wybierz albo przeglądarkę, wprowadź nazwę i wiadomość, a następnie kliknij przycisk **wysyłania** przycisku. Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.

  ![Rozwiązanie](get-started-signalr-core/_static/signalr-get-started-finished.png)
