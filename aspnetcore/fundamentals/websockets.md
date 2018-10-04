---
title: Obsługa protokółu Websocket w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: fc3f70fb888797216b2ccc911a9f69eaae6ac01c
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577733"
---
# <a name="websockets-support-in-aspnet-core"></a>Obsługa protokółu Websocket w programie ASP.NET Core

Przez [Tom Dykstra](https://github.com/tdykstra) i [Andrew Stanton pielęgniarki](https://github.com/anurse)

W tym artykule wyjaśniono, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) to protokół, który umożliwia kanały komunikacja dwukierunkowa trwałego połączenia protokołu TCP. Jest on używany w aplikacjach korzystających z komunikacji szybki, w czasie rzeczywistym, takich jak rozmowy, pulpit nawigacyjny i gier, aplikacji.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)). Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać więcej informacji.

## <a name="prerequisites"></a>Wymagania wstępne

* Platforma ASP.NET Core 1.1 lub nowszej
* Dowolny system operacyjny, który obsługuje platformy ASP.NET Core:
  
  * Windows 7 / Windows Server 2008 lub nowszy
  * Linux
  * macOS
  
* Jeśli aplikacja działa w systemie Windows za pomocą programu IIS:

  * Windows 8 / Windows Server 2012 lub nowszy
  * Usługi IIS 8 / 8 usług IIS Express
  * Gniazda Websocket wymaga włączenia w usługach IIS (zobacz [obsługi usług IIS/IIS Express](#iisiis-express-support) sekcji.)
  
* Jeśli aplikacja jest uruchamiana [HTTP.sys](xref:fundamentals/servers/httpsys):

  * Windows 8 / Windows Server 2012 lub nowszy

* W przypadku obsługiwanych przeglądarek, zobacz https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Kiedy należy używać funkcji WebSockets

Do pracy bezpośrednio za pomocą połączenia gniazda, należy użyć funkcji WebSockets. Aby uzyskać optymalną wydajność w czasie rzeczywistym gry, na przykład użyć funkcji WebSockets.

[Biblioteki SignalR platformy ASP.NET Core](xref:signalr/introduction) to biblioteka, która ułatwia dodawanie funkcji internetowych w czasie rzeczywistym do aplikacji. Używa funkcji WebSockets, jeśli to możliwe.

## <a name="how-to-use-websockets"></a>Jak używać funkcji WebSockets

* Zainstaluj [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pakietu.
* Konfigurowanie oprogramowania pośredniczącego.
* Akceptują żądania protokołu WebSocket.
* Wysyłanie i odbieranie wiadomości.

### <a name="configure-the-middleware"></a>Konfigurowanie oprogramowania pośredniczącego

Dodaj oprogramowanie pośredniczące Websocket w `Configure` metody `Startup` klasy:

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Można skonfigurować następujące ustawienia:

* `KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymanie otwartego połączenia.
* `ReceiveBufferSize` — Rozmiar buforu używany do odbierania danych. Użytkownicy zaawansowani trzeba to zmienić dotyczące dostosowywania wydajności na podstawie rozmiaru danych.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Akceptować żądania protokołu WebSocket

Gdzieś później w cyklu życia żądania (w dalszej części `Configure` metody lub Akcja kontrolera MVC, na przykład) sprawdź, czy jest on żądanie protokołu WebSocket i zaakceptuj żądanie protokołu WebSocket.

Poniższy przykład znajduje się w dalszej części w `Configure` metody:

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Żądanie protokołu WebSocket może występować na dowolny adres URL, ale ten przykładowy kod akceptuje tylko żądania dotyczące `/ws`.

### <a name="send-and-receive-messages"></a>Wysyłanie i odbieranie komunikatów

`AcceptWebSocketAsync` Metoda uaktualnia połączenie TCP z połączeniem WebSocket i zapewnia [WebSocket](/dotnet/core/api/system.net.websockets.websocket) obiektu. Użyj `WebSocket` obiekt do wysyłania i odbierania komunikatów.

Przekazuje kodu pokazanego wcześniej, która akceptuje żądanie protokołu WebSocket `WebSocket` obiekt `Echo` metody. Kod odbiera komunikat i natychmiast wysyła z powrotem tę samą wiadomość. Wiadomości wysyłanych i odbieranych w pętli, dopóki klient zamyka połączenie:

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Akceptując połączeniem WebSocket, przed rozpoczęciem pętli, kończy się potoku oprogramowania pośredniczącego. Po zamknięciu gniazda, rozwija potoku. Oznacza to, że żądanie zatrzymuje, przenoszenie do przodu w potoku po zaakceptowaniu WebSocket. Po zakończeniu pętli i gniazda jest zamknięty, żądanie będzie kontynuowane wykonywanie kopii zapasowych potoku.

## <a name="iisiis-express-support"></a>Obsługa usług IIS/IIS Express

Windows Server 2012 lub nowszym i Windows 8 lub nowszym z usług IIS/IIS Express 8 lub nowszy jest obsługa protokołu WebSocket.

Aby włączyć obsługę protokołu WebSocket w systemie Windows Server 2012 lub nowszym:

1. Użyj **Dodaj role i funkcje** kreatora z **Zarządzaj** menu lub linku w **Menedżera serwera**.
1. Wybierz **Instalacja oparta na rolach lub oparta na funkcjach**. Wybierz **dalej**.
1. Wybierz odpowiedni serwer (serwer lokalny jest wybrane domyślnie). Wybierz **dalej**.
1. Rozwiń **serwer sieci Web (IIS)** w **role** drzewa, a następnie rozwiń **serwera sieci Web**, a następnie rozwiń węzeł **opracowywanie aplikacji**.
1. Wybierz **protokołu WebSocket**. Wybierz **dalej**.
1. Jeśli nie są wymagane dodatkowe funkcje, wybierz opcję **dalej**.
1. Wybierz **zainstalować**.
1. Po zakończeniu instalacji wybierz **Zamknij** aby zakończyć pracę kreatora.

Aby włączyć obsługę protokołu WebSocket w systemie Windows 8 lub nowszy:

1. Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).
1. Otwórz następujące węzły: **Internetowe usługi informacyjne** > **usługi World Wide Web** > **funkcje tworzenia aplikacji**.
1. Wybierz **protokołu WebSocket** funkcji. Wybierz **OK**.

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a>Wyłącz WebSocket, gdy na języku Node.js przy użyciu biblioteki socket.io

Jeśli dzięki obsłudze protokołu WebSocket w [biblioteki socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), Wyłącz domyślne IIS WebSocket modułu przy użyciu `webSocket` element *web.config* lub *applicationHost.config*. Jeśli ten krok nie jest wykonywana, Moduł IIS WebSocket próbuje obsługi komunikacji protokołu WebSocket, a nie środowiska Node.js i aplikacji.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Następne kroki

[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) którego zostanie dołączony, ten artykuł stanowi app echa. Posiada strony sieci web, która sprawia, że połączeń protokołu WebSocket i wszelkich komunikatów odebranych wysyła ponownie serwer do klienta. Uruchamianie aplikacji z poziomu wiersza polecenia (go nie skonfigurowała do uruchamiania w programie Visual Studio z programem IIS Express) i przejdź do http://localhost:5000. Strony sieci web pokazuje stan połączenia w lewym górnym rogu:

![Początkowy stan strony sieci web](websockets/_static/start.png)

Wybierz **Connect** wysyłać żądanie protokołu WebSocket adres URL wyświetlany. Wprowadź wiadomość testową, a następnie wybierz pozycję **wysyłania**. Po zakończeniu wybierz pozycję **Zamknij gniazda**. **Dziennika komunikacji** sekcji raporty każdy otwarty, wysyłanie i zamknij akcji, jak to się dzieje.

![Początkowy stan strony sieci web](websockets/_static/end.png)
