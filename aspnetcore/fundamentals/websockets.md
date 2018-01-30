---
title: "Obsługę protokołu WebSockets w ASP.NET Core"
author: tdykstra
description: "Dowiedz się, jak rozpocząć pracę z Websocket w ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Wprowadzenie do protokołu WebSockets w platformy ASP.NET Core

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Andrew Stanton pielęgniarki](https://github.com/anurse)

W tym artykule wyjaśniono, jak rozpocząć pracę z Websocket w ASP.NET Core. [Protokół WebSocket](https://wikipedia.org/wiki/WebSocket) jest to protokół umożliwiający kanały dwukierunkowej komunikacji trwałego za pośrednictwem połączeń TCP. Służy do aplikacji, takich jak rozmowa, giełdowych, gier, dowolnym miejscu w czasie rzeczywistym funkcji w aplikacji sieci web.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)). Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać więcej informacji.


## <a name="prerequisites"></a>Wymagania wstępne

* Platformy ASP.NET Core 1.1 (nie działa w wersji 1.0)
* Dowolnego systemu operacyjnego, platformy ASP.NET Core uruchamianego na:
  
  * Windows 7 / Windows Server 2008 lub nowszy
  * Linux
  * macOS

* **Wyjątek**: Jeśli aplikacja będzie działać w systemie Windows z programem IIS, lub z WebListener, należy użyć:

  * Windows 8 / Windows Server 2012 lub nowszy
  * Usługi IIS 8 / 8 usług IIS Express
  * Protokół WebSocket musi być włączona w programie IIS

* W przypadku obsługiwanych przeglądarek zobacz http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Kiedy stosować

Za pomocą Websocket muszą pracować bezpośrednio z połączenia gniazda. Na przykład może być konieczne najlepsze możliwe wydajności w czasie rzeczywistym gry.

[Biblioteka SignalR platformy ASP.NET](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) zapewnia na pełniejsze modelu aplikacji dla funkcji w czasie rzeczywistym, ale działa tylko na platformy ASP.NET, nie platformy ASP.NET Core. Wersja SignalR Core jest w fazie projektowania; Postępuj zgodnie z jego postępu, zobacz [repozytorium GitHub dla SignalR Core](https://github.com/aspnet/SignalR).

Jeśli nie chcesz czekać na SignalR Core służy protokół WebSockets bezpośrednio teraz. Jednak może być konieczne tworzenie funkcji, które zapewni SignalR, na przykład:

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

Dodaj oprogramowanie pośredniczące Websocket w `Configure` metody `Startup` klasy.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Można skonfigurować następujące ustawienia:

* `KeepAliveInterval`-Porady często wysyłać ramki "ping" do klienta, aby upewnić się, że serwery proxy utrzymać otwarte połączenie.
* `ReceiveBufferSize`-Rozmiar buforu używany do odbierania danych. Tylko użytkownicy zaawansowani musi zmieniać, dostrajania wydajności na podstawie rozmiaru ich danych.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Akceptować żądania protokołu WebSocket

Nowsze gdzieś w cyklu życia żądania (dalszej części `Configure` metody lub Akcja kontrolera MVC, na przykład) sprawdź, czy jest to żądanie protokołu WebSocket i zaakceptować żądania protokołu WebSocket.

W tym przykładzie pochodzi z później w `Configure` metody.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Żądanie protokołu WebSocket może występować na dowolny adres URL, ale ten przykładowy kod akceptuje tylko żądania dotyczące `/ws`.

### <a name="send-and-receive-messages"></a>Wysyłanie i odbieranie wiadomości

`AcceptWebSocketAsync` Metoda uaktualnia się przez połączenie obiektu WebSocket połączenie TCP i umożliwia [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) obiektu. Obiekt WebSocket używany do wysyłania i odbierania wiadomości.

Przekazuje kodu pokazano wcześniej, który akceptuje żądanie protokołu WebSocket `WebSocket` do obiektu `Echo` metody; w tym `Echo` metody. Kod odbiera komunikat i natychmiast odsyła tę samą wiadomość. Pozostaje on objęty w pętli, dopóki klient zamyka połączenie operacją. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Po zaakceptowaniu żądania WebSocket przed rozpoczęciem pętlę kończy się potoku oprogramowania pośredniczącego.  Po zamknięciu gniazda, cofa potoku. Oznacza to zatrzymuje żądania przeniesienie do przodu w potoku po zaakceptowaniu protokołu WebSocket, podobnie jak czy naciśnięcie Akcja kontrolera MVC, np.  Jednak po ukończeniu tej pętli i zamknąć gniazda, żądanie będzie kontynuowana, Utwórz kopię zapasową potoku.

## <a name="next-steps"></a>Następne kroki

[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) dołączony ten artykuł jest aplikacją echo proste. Ma strony sieci web, która umożliwia nawiązanie połączenia obiektu WebSocket, a serwer po prostu ponownie wysyła do klienta komunikaty, które otrzymuje. Go uruchomić z wiersza polecenia (go nie skonfigurował do uruchomienia z programu Visual Studio z programem IIS Express) i przejdź do http://localhost: 5000. Strony sieci web w lewym górnym rogu jest widoczny stan połączenia:

![Początkowy stan strony sieci web](websockets/_static/start.png)

Wybierz **Connect** wysłać żądania protokołu WebSocket do adres URL wyświetlany.  Wprowadź wiadomość testową, a następnie wybierz **wysyłania**. Po zakończeniu wybierz **Zamknij gniazda**. **Dziennika komunikacji** sekcji raporty każdego otwarty, wysyłania i zamknij akcji w postaci, w jakiej się stanie.

![Początkowy stan strony sieci web](websockets/_static/end.png)
