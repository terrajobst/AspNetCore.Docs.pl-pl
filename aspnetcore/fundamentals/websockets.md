---
title: Obsługę protokołu WebSockets w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z Websocket w ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: ede8064b5e77024b843357d4715869b3495b9147
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2018
---
# <a name="websockets-support-in-aspnet-core"></a>Obsługę protokołu WebSockets w ASP.NET Core

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Andrew Stanton pielęgniarki](https://github.com/anurse)

W tym artykule wyjaśniono, jak rozpocząć pracę z Websocket w ASP.NET Core. [Protokół WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) to protokół, który umożliwia kanały dwukierunkowej komunikacji trwałego za pośrednictwem połączeń TCP. Jest on używany w aplikacjach korzystających z komunikacji szybkiego, w czasie rzeczywistym, takich jak rozmowy, pulpit nawigacyjny i gier aplikacji.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)). Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać więcej informacji.

## <a name="prerequisites"></a>Wymagania wstępne

* Platformy ASP.NET Core 1.1 lub nowszej
* Dowolnego systemu operacyjnego, który obsługuje platformy ASP.NET Core:
  
  * Windows 7 / Windows Server 2008 lub nowszy
  * Linux
  * macOS
  
* Jeśli aplikacja jest uruchamiana w systemie Windows z programem IIS:

  * Windows 8 / Windows Server 2012 lub nowszy
  * Usługi IIS 8 / 8 usług IIS Express
  * Protokół WebSockets musi być włączona w programie IIS (zobacz [Obsługa programu IIS/IIS Express](#iisiis-express-support) sekcji.)
  
* Jeśli aplikacja jest uruchamiana [HTTP.sys](xref:fundamentals/servers/httpsys):

  * Windows 8 / Windows Server 2012 lub nowszy

* W przypadku obsługiwanych przeglądarek, zobacz https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Kiedy należy używać protokołu WebSockets

Pracować bezpośrednio za pomocą połączenia gniazda za pomocą protokołu WebSockets. Aby uzyskać optymalną wydajność w czasie rzeczywistym gry, na przykład użyć protokołu WebSockets.

[Biblioteka SignalR platformy ASP.NET](/aspnet/signalr/overview/getting-started/introduction-to-signalr) zapewnia bardziej rozbudowane model aplikacji dla funkcji w czasie rzeczywistym, ale działa tylko na platformie ASP.NET 4.x nie platformy ASP.NET Core. Wersja platformy ASP.NET Core SignalR zaplanowano wydanie z platformy ASP.NET Core 2.1. Zobacz [platformy ASP.NET Core 2.1 wysokiego poziomu planowania](https://github.com/aspnet/Announcements/issues/288).

Do czasu zwolnienia jest SignalR Core, można Websocket. Jednak funkcje, które zapewnia SignalR musi podać i obsługiwane przez dewelopera. Na przykład:

* Obsługa większej liczby wersji przeglądarki przy użyciu automatycznego powrotu do metod alternatywnych transportu.
* Automatyczne ponowne łączenie, kiedy obniży połączenia.
* Obsługa klientów wywoływania metod, na serwerze lub na odwrót.
* Obsługa skalowania na wielu serwerach.

## <a name="how-to-use-it"></a>Jak z niego korzystać

* Zainstaluj [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pakietu.
* Konfigurowanie oprogramowania pośredniczącego.
* Zaakceptować żądania protokołu WebSocket.
* Wysyłanie i odbieranie wiadomości.

### <a name="configure-the-middleware"></a>Konfigurowanie oprogramowania pośredniczącego

Dodaj oprogramowanie pośredniczące Websocket w `Configure` metody `Startup` klasy:

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Można skonfigurować następujące ustawienia:

* `KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymać otwarte połączenie.
* `ReceiveBufferSize` -Rozmiar buforu używany do odbierania danych. Użytkownicy wersji Advanced trzeba zmienić to dostrajania wydajności, zależnie od rozmiaru danych.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Akceptować żądania protokołu WebSocket

Nowsze gdzieś w cyklu życia żądania (dalszej części `Configure` metody lub Akcja kontrolera MVC, na przykład) sprawdź, czy jest to żądanie protokołu WebSocket i zaakceptować żądania protokołu WebSocket.

Poniższy przykład pochodzi z później w `Configure` metody:

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Żądanie protokołu WebSocket może występować na dowolny adres URL, ale ten przykładowy kod akceptuje tylko żądania dotyczące `/ws`.

### <a name="send-and-receive-messages"></a>Wysyłanie i odbieranie wiadomości

`AcceptWebSocketAsync` Metoda uaktualnia połączenie TCP do połączenia obiektu WebSocket i udostępnia [WebSocket](/dotnet/core/api/system.net.websockets.websocket) obiektu. Użyj `WebSocket` obiekt do wysyłania i odbierania wiadomości.

Przekazuje kodu pokazano wcześniej, który akceptuje żądanie protokołu WebSocket `WebSocket` do obiektu `Echo` metody. Kod odbiera komunikat i natychmiast odsyła tę samą wiadomość. Wiadomości wysyłanych i odbieranych w pętli, dopóki klient zamyka połączenie:

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Akceptując połączenia obiektu WebSocket przed rozpoczęciem pętli kończy się potoku oprogramowania pośredniczącego. Po zamknięciu gniazda, cofa potoku. Oznacza to żądanie zatrzymania przeniesienie do przodu w potoku po zaakceptowaniu żądania WebSocket. Po zakończeniu pętli i gniazda jest zamknięty, żądanie będzie kontynuowana, Utwórz kopię zapasową potoku.

## <a name="iisiis-express-support"></a>Obsługa programu IIS/IIS Express

Windows Server 2012 lub nowszym i Windows 8 lub nowszym z programu IIS/IIS Express 8 lub nowszy zawiera obsługę protokołu WebSocket.

Aby włączyć obsługę protokołu WebSocket w systemie Windows Server 2012 lub nowszym:

1. Użyj **Dodaj role i funkcje** kreatora z **Zarządzaj** menu lub link w **Menedżera serwera**.
1. Wybierz **Instalacja roli lub funkcji**. Wybierz **dalej**.
1. Wybierz odpowiedni serwer (serwer lokalny jest domyślnie zaznaczona). Wybierz **dalej**.
1. Rozwiń węzeł **serwer sieci Web (IIS)** w **ról** drzewa, a następnie rozwiń **serwera sieci Web**, a następnie rozwiń węzeł **projektowanie aplikacji**.
1. Wybierz **protokół WebSocket**. Wybierz **dalej**.
1. Jeśli nie są potrzebne dodatkowe funkcje, wybierz **dalej**.
1. Wybierz **zainstalować**.
1. Po zakończeniu instalacji wybierz **Zamknij** aby zakończyć pracę kreatora.

Aby włączyć obsługę protokołu WebSocket w systemie Windows 8 lub nowszy:

1. Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu).
1. Otwórz następujące węzły: **Internetowe usługi informacyjne** > **usługi sieci World Wide Web** > **funkcje tworzenia aplikacji**.
1. Wybierz **protokół WebSocket** funkcji. Wybierz **OK**.

**Wyłączanie protokołu WebSocket za pomocą użyciu biblioteki socket.io node.js**

Jeśli przy użyciu obsługi protokołu WebSocket w [użyciu biblioteki socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), wyłącz użyciu moduł WebSocket usług IIS domyślne `webSocket` element w *web.config* lub *applicationHost.config*. Jeśli ten krok nie jest wykonywana, moduł WebSocket usług IIS próbuje obsługi komunikacji protokołu WebSocket zamiast Node.js i aplikacji.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Następne kroki

[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) dołączony ten artykuł jest aplikacją echo. Istnieją strony sieci web, która umożliwia nawiązanie połączenia obiektu WebSocket i ponownie serwer wysyła komunikaty, które otrzymuje do klienta. Uruchamianie aplikacji z wiersza polecenia (go nie skonfigurował do uruchomienia z programu Visual Studio z programem IIS Express) i przejdź do http://localhost:5000. Strony sieci web w lewym górnym rogu jest widoczny stan połączenia:

![Początkowy stan strony sieci web](websockets/_static/start.png)

Wybierz **Connect** wysłać żądania protokołu WebSocket do adres URL wyświetlany. Wprowadź wiadomość testową, a następnie wybierz **wysyłania**. Po zakończeniu wybierz **Zamknij gniazda**. **Dziennika komunikacji** sekcji raporty każdego otwarty, wysyłania i zamknij akcji w postaci, w jakiej się stanie.

![Początkowy stan strony sieci web](websockets/_static/end.png)
