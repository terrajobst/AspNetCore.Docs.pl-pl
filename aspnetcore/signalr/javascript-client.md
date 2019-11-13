---
title: Klient ASP.NET Core SignalR JavaScript
author: bradygaster
description: Omówienie ASP.NET Core SignalR klienta JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/javascript-client
ms.openlocfilehash: 926160a41c82853d83890f0d52b14d7d5561a990
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963770"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a>Klient ASP.NET Core SignalR JavaScript

Autor [Rachel Appel](https://twitter.com/rachelappel)

Biblioteka klienta ASP.NET Core SignalR JavaScript umożliwia deweloperom Wywoływanie kodu centrum po stronie serwera.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-client-package"></a>Zainstaluj pakiet klienta SignalR

Biblioteka klienta SignalR JavaScript jest dostarczana jako pakiet [npm](https://www.npmjs.com/) . Jeśli używasz programu Visual Studio, uruchom `npm install` z **konsoli Menedżera pakietów** w folderze głównym. Aby uzyskać Visual Studio Code, uruchom polecenie w **zintegrowanym terminalu**.

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

npm instaluje zawartość pakietu w folderze *node_modules\\@microsoft\signalr\dist\browser* . Utwórz nowy folder o nazwie *sygnalizujący* w folderze *wwwroot\\lib* . Skopiuj plik *sygnalizującer. js* do folderu *wwwroot\lib\signalr* .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm instaluje zawartość pakietu w folderze *node_modules\\@aspnet\signalr\dist\browser* . Utwórz nowy folder o nazwie *sygnalizujący* w folderze *wwwroot\\lib* . Skopiuj plik *sygnalizującer. js* do folderu *wwwroot\lib\signalr* .

::: moniker-end

## <a name="use-the-opno-locsignalr-javascript-client"></a>Korzystanie z klienta SignalR JavaScript

Odwołuje się do SignalR klienta JavaScript w elemencie `<script>`.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Nawiązywanie połączenia z centrum

Poniższy kod tworzy i uruchamia połączenie. Nazwa centrum nie uwzględnia wielkości liter.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Połączenia między źródłami

Zwykle przeglądarki ładują połączenia z tej samej domeny co żądana strona. Zdarza się jednak, że jest wymagane połączenie z inną domeną.

Aby uniemożliwić złośliwym lokacjom odczytywanie poufnych danych z innej lokacji, [połączenia między źródłami](xref:security/cors) są domyślnie wyłączone. Aby zezwolić na żądanie między źródłami, włącz je w klasie `Startup`.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Wywoływanie metod centrów z klienta

Klienci języka JavaScript wywołują metody publiczne w centrach za pomocą metody [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) elementu [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). Metoda `invoke` akceptuje dwa argumenty:

* Nazwa metody centrum. W poniższym przykładzie nazwa metody w centrum jest `SendMessage`.
* Wszystkie argumenty zdefiniowane w metodzie centrum. W poniższym przykładzie nazwa argumentu jest `message`. Przykładowy kod używa składni funkcji ze strzałką, która jest obsługiwana w bieżących wersjach wszystkich głównych przeglądarek z wyjątkiem programu Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Jeśli używasz usługi Azure SignalR w *trybie Bezserwerowym*, nie można wywoływać metod centralnych z poziomu klienta. Aby uzyskać więcej informacji, zobacz [dokumentację usługiSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

Metoda `invoke` zwraca [obietnicę](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)języka JavaScript. `Promise` jest rozpoznawana z wartością zwracaną (jeśli istnieje), gdy metoda zwraca serwer. Jeśli metoda na serwerze zgłasza błąd, `Promise` zostanie odrzucona z komunikatem o błędzie. Użyj `then` i `catch` metod na `Promise`, aby obsługiwać te przypadki (lub składnię `await`).

Metoda `send` zwraca `Promise`języka JavaScript. `Promise` jest rozpoznawany, gdy wiadomość została wysłana do serwera. Jeśli wystąpi błąd podczas wysyłania komunikatu, `Promise` jest odrzucany wraz z komunikatem o błędzie. Użyj `then` i `catch` metod na `Promise`, aby obsługiwać te przypadki (lub składnię `await`).

> [!NOTE]
> Użycie `send` nie czeka na odebranie komunikatu przez serwer. W związku z tym nie można zwrócić danych ani błędów z serwera.

## <a name="call-client-methods-from-hub"></a>Wywoływanie metod klienta z centrum

Aby odbierać komunikaty z centrum, zdefiniuj metodę przy użyciu metody [on](/javascript/api/%40aspnet/signalr/hubconnection#on) w `HubConnection`.

* Nazwa metody klienta JavaScript. W poniższym przykładzie nazwa metody jest `ReceiveMessage`.
* Argumenty przekazywane przez centrum do metody. W poniższym przykładzie wartość argumentu jest `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Poprzedni kod w `connection.on` jest uruchamiany, gdy kod po stronie serwera wywoła go przy użyciu metody [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) .

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR określa, która metoda klienta ma być wywoływana przez dopasowanie nazwy metody i argumentów zdefiniowanych w `SendAsync` i `connection.on`.

> [!NOTE]
> Najlepszym rozwiązaniem jest wywołanie metody [Start](/javascript/api/%40aspnet/signalr/hubconnection#start) na `HubConnection` po `on`. Dzięki temu programy obsługi zostaną zarejestrowane przed odebraniem wszelkich komunikatów.

## <a name="error-handling-and-logging"></a>Obsługa błędów i rejestrowanie

Łańcuchuje metodę `catch` na końcu metody `start` w celu obsługi błędów po stronie klienta. Użyj `console.error`, aby wyprowadzić błędy do konsoli przeglądarki.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Skonfiguruj śledzenie dzienników po stronie klienta, przekazując Rejestrator i typ zdarzenia, które będą rejestrowane po nawiązaniu połączenia. Komunikaty są rejestrowane z określonym poziomem dziennika i wyższym. Dostępne poziomy dzienników są następujące:

* `signalR.LogLevel.Error` &ndash; komunikatów o błędach. Rejestruje tylko wiadomości `Error`.
* `signalR.LogLevel.Warning` &ndash; komunikatów ostrzegawczych dotyczących potencjalnych błędów. Rejestruje `Warning`i `Error` komunikatów.
* `signalR.LogLevel.Information` &ndash; komunikatów o stanie bez błędów. Rejestruje wiadomości `Information`, `Warning`i `Error`.
* `signalR.LogLevel.Trace` &ndash; komunikatów śledzenia. Rejestruje wszystko, w tym dane przesyłane między koncentratorem a klientem.

Użyj metody [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) w [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) , aby skonfigurować poziom rejestrowania. Komunikaty są rejestrowane w konsoli przeglądarki.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Ponowne łączenie klientów

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Automatycznie Połącz ponownie

Klient JavaScript dla SignalR można skonfigurować w taki sposób, aby automatycznie ponownie łączyć się za pomocą metody `withAutomaticReconnect` w [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). Domyślnie nie będzie automatycznie ponownie łączyć się.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Bez żadnych parametrów `withAutomaticReconnect()` konfiguruje klienta tak, aby czekał 0, 2, 10 i 30 sekund przed podjęciem próby ponownego nawiązania połączenia, zatrzymując po czterech nieudanych próbach.

Przed rozpoczęciem jakichkolwiek prób ponownego łączenia `HubConnection` przechodzi do stanu `HubConnectionState.Reconnecting` i wyzwalane przez `onreconnecting` wywołania zwrotne, a nie przejście do stanu `Disconnected` i wyzwalanie `onclose` wywołań zwrotnych, takich jak `HubConnection` bez skonfigurowanego automatycznego ponownego łączenia. Dzięki temu można ostrzec użytkowników, że połączenie zostało utracone i wyłączyć elementy interfejsu użytkownika.

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Jeśli klient pomyślnie ponownie nawiąże połączenie w ramach pierwszych czterech prób, `HubConnection` przejdzie z powrotem do stanu `Connected` i uruchomi wywołania zwrotne `onreconnected`. Zapewnia to możliwość powiadomienia użytkowników o tym, że połączenie zostało ponownie nawiązane.

Ponieważ połączenie jest całkowicie nowe dla serwera, do wywołania zwrotnego `onreconnected` zostanie dostarczone nowe `connectionId`.

> [!WARNING]
> Parametr `connectionId` wywołania zwrotnego `onreconnected` zostanie niezdefiniowany, jeśli `HubConnection` został skonfigurowany do [pomijania negocjacji](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` nie skonfiguruje `HubConnection` w celu ponowienia nieudanych uruchomień początkowych, dlatego należy ręcznie obsługiwać błędy uruchamiania:

```javascript
async function start() {
    try {
        await connection.start();
        console.assert(connection.state === signalR.HubConnectionState.Connected);
        console.log("connected");
    } catch (err) {
        console.assert(connection.state === signalR.HubConnectionState.Disconnected);
        console.log(err);
        setTimeout(() => start(), 5000);
    }
};
```

Jeśli klient nie będzie mógł ponownie nawiązać połączenia w ramach pierwszych czterech prób, `HubConnection` przejdzie do stanu `Disconnected` i zostanie wyzwolony [jego wywołania](/javascript/api/%40aspnet/signalr/hubconnection#onclose) zwrotne. Dzięki temu użytkownicy mogą informować, że połączenie zostało trwale utracone i zalecamy odświeżenie strony:

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Aby skonfigurować niestandardową liczbę prób ponownego połączenia przed odłączeniem lub zmianą czasu ponownego połączenia, `withAutomaticReconnect` akceptuje tablicę liczb reprezentujących opóźnienie (w milisekundach) przed rozpoczęciem każdej próby ponownego nawiązania połączenia.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

Powyższy przykład konfiguruje `HubConnection`, aby rozpocząć próbę ponownego połączenia natychmiast po utracie połączenia. Dotyczy to również konfiguracji domyślnej.

Jeśli pierwsza próba ponownego połączenia nie powiedzie się, druga próba ponownego połączenia zostanie również uruchomiona natychmiast, a nie w ciągu 2 sekund, tak jak w przypadku konfiguracji domyślnej.

Jeśli druga próba ponownego połączenia nie powiedzie się, trzecia próba ponownego nawiązania połączenia rozpocznie się w ciągu 10 sekund, co jest ponownie podobne do konfiguracji domyślnej.

Zachowanie niestandardowe jest następnie niezgodne z zachowaniem domyślnym przez zatrzymanie po trzeciej nieudanej próbie nawiązania połączenia zamiast próby wykonania kolejnej próby ponownego połączenia w ciągu innych 30 sekund, takich jak w przypadku konfiguracji domyślnej.

Jeśli potrzebujesz jeszcze większą kontrolę nad chronometrażem i liczbą prób automatycznego ponownego połączenia, `withAutomaticReconnect` akceptuje obiekt implementujący interfejs `IRetryPolicy`, który ma pojedynczą metodę o nazwie `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds` przyjmuje jeden argument z typem `RetryContext`. `RetryContext` ma trzy właściwości: `previousRetryCount`, `elapsedMilliseconds` i `retryReason`, które są `number`, `number` i `Error` odpowiednio. Przed pierwszą próbą ponownego połączenia oba `previousRetryCount` i `elapsedMilliseconds` będą miały wartość zero, a `retryReason` będzie błędem, który spowodował utratę połączenia. Po każdym nieudanej próbie ponowieniu próby `previousRetryCount` będzie zwiększane o jeden, `elapsedMilliseconds` zostanie zaktualizowany w celu odzwierciedlenia ilości czasu poświęconego na przełączenie do tej pory w milisekundach, a `retryReason` będzie błędem, który spowodował, że Ostatnia próba ponownego połączenia nie powiedzie się.

`nextRetryDelayInMilliseconds` musi zwrócić liczbę określającą liczbę milisekund oczekiwania przed kolejną próbą ponownego połączenia lub `null`, jeśli `HubConnection` powinna zatrzymać Ponowne nawiązywanie połączenia.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: retryContext => {
          if (retryContext.elapsedMilliseconds < 60000) {
            // If we've been reconnecting for less than 60 seconds so far,
            // wait between 0 and 10 seconds before the next reconnect attempt.
            return Math.random() * 10000;
          } else {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
          }
        })
    .build();
```

Alternatywnie można napisać kod, który ponownie podłącze klienta ręcznie, jak pokazano w [ręcznym ponownym nawiązaniu połączenia](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Ręczne ponowne łączenie

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> W systemach wcześniejszych niż 3,0 klient JavaScript dla SignalR nie będzie automatycznie ponownie łączyć się. Musisz napisać kod, który ponownie podłącze klienta ręcznie.

::: moniker-end

Poniższy kod ilustruje typowe podejście do ponownego łączenia ręcznego:

1. Funkcja (w tym przypadku funkcja `start`) została utworzona w celu uruchomienia połączenia.
1. Wywołaj funkcję `start` w obsłudze zdarzeń `onclose` połączenia.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Implementacja rzeczywista będzie używać wykładniczej kopii zapasowej lub ponowić określoną liczbę razy przed pokazaniem.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja interfejsów API języka JavaScript](/javascript/api/?view=signalr-js-latest)
* [Samouczek JavaScript](xref:tutorials/signalr)
* [Samouczek WebPack i język TypeScript](xref:tutorials/signalr-typescript-webpack)
* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
* [Żądania między źródłami (CORS)](xref:security/cors)
* [Dokumentacja bezserwerowa usługi SignalR platformy Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)
