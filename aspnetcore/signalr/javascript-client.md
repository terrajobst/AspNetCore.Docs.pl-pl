---
title: ASP.NET Core SignalR JavaScript klienta
author: bradygaster
description: Omówienie platformy ASP.NET Core SignalR JavaScript klienta.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/28/2019
uid: signalr/javascript-client
ms.openlocfilehash: f314e1fe0ef0ea73a28b332404a09f2956524132
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412380"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript klienta

Przez [Rachel Appel](https://twitter.com/rachelappel)

Biblioteki klienta platformy ASP.NET Core SignalR JavaScript umożliwia deweloperom wywołanie kodu koncentratora po stronie serwera.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Zainstaluj pakiet klienta SignalR

Biblioteka klienta SignalR JavaScript jest dostarczana jako [npm](https://www.npmjs.com/) pakietu. Jeśli używasz programu Visual Studio, uruchom `npm install` z **Konsola Menedżera pakietów** podczas gdy w folderze głównym. Dla programu Visual Studio Code, uruchom polecenie **zintegrowany Terminal**.

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

npm instaluje zawartość pakietu w *node_modules\\@microsoft\signalr\dist\browser* folderu. Utwórz nowy folder o nazwie *signalr* w obszarze *wwwroot\\lib* folderu. Kopiuj *signalr.js* plik *wwwroot\lib\signalr* folderu.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm instaluje zawartość pakietu w *node_modules\\@aspnet\signalr\dist\browser* folderu. Utwórz nowy folder o nazwie *signalr* w obszarze *wwwroot\\lib* folderu. Kopiuj *signalr.js* plik *wwwroot\lib\signalr* folderu.

::: moniker-end

## <a name="use-the-signalr-javascript-client"></a>Używanie klienta SignalR JavaScript

Odwoływać się do klienta SignalR JavaScript w `<script>` elementu.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Połączenia z koncentratorem

Poniższy kod tworzy i uruchamia połączenie. Nazwa Centrum jest uwzględniana wielkość liter.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Połączenia między źródłami

Zazwyczaj przeglądarek ładowanie połączeń z tej samej domenie co żądanej strony. Istnieją jednak sytuacje, gdy wymagane jest połączenie do innej domeny.

Aby zapobiec złośliwych witryn odczytywanie poufnych danych z innej lokacji [połączeń cross-origin](xref:security/cors) są domyślnie wyłączone. Aby zezwolić na żądania między źródłami, włącz go w `Startup` klasy.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Wywoływanie metod koncentratora z klienta

Klientów JavaScript wywołania metod publicznych w centrach za pośrednictwem [wywołania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) metody [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). `invoke` Metoda przyjmuje dwa argumenty:

* Nazwa metody koncentratora. W poniższym przykładzie jest nazwa metody koncentratora `SendMessage`.
* Argumenty zdefiniowane w metody koncentratora. W poniższym przykładzie jest nazwa argumentu `message`. Przykładowy kod używa składni funkcji ze strzałką, która jest obsługiwana w bieżących wersjach wszystkich głównych przeglądarek z wyjątkiem programu Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Jeśli używasz usługi Azure sygnalizującej w *trybie*bezserwerowym, nie możesz wywoływać metod centralnych z poziomu klienta. Aby uzyskać więcej informacji, zobacz [dokumentację usługi sygnalizującej](/azure/azure-signalr/signalr-concept-serverless-development-config).

Metoda zwraca obietnicę języka JavaScript. [](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) `invoke` `Promise` Zostanie rozwiązany z wartością zwracaną (jeśli istnieje), gdy metoda zwraca serwer. Jeśli metoda na serwerze zgłasza błąd, `Promise` zostaje odrzucona z komunikatem o błędzie. Użyj metod `then` i `catch` dla `Promise` samej siebie, aby obsłużyć te przypadki `await` (lub składnię).

Metoda zwraca kod JavaScript `Promise`. `send` Jest `Promise` rozpoznawany, gdy wiadomość została wysłana na serwer. Jeśli wystąpi błąd podczas wysyłania komunikatu, `Promise` zostanie on odrzucony z komunikatem o błędzie. Użyj metod `then` i `catch` dla `Promise` samej siebie, aby obsłużyć te przypadki `await` (lub składnię).

> [!NOTE]
> Użycie `send` nie czeka na odebranie komunikatu przez serwer. W związku z tym nie można zwrócić danych ani błędów z serwera.

## <a name="call-client-methods-from-hub"></a>Wywoływanie metody klienta z Centrum

Aby odbierać komunikaty z Centrum, definiowanie metody przy użyciu [na](/javascript/api/%40aspnet/signalr/hubconnection#on) metody `HubConnection`.

* Nazwa metody klienta JavaScript. W poniższym przykładzie nazwa metody jest `ReceiveMessage`.
* Argumenty Centrum przekazuje do metody. W poniższym przykładzie wartość argumentu jest `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Powyższy kod w `connection.on` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metody.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

Określa SignalR, jakiej metody klienta do wywołania, dopasowując nazwy metody i argumenty zdefiniowane w `SendAsync` i `connection.on`.

> [!NOTE]
> Najlepszym rozwiązaniem, należy wywołać [start](/javascript/api/%40aspnet/signalr/hubconnection#start) metody `HubConnection` po `on`. Daje to pewność, że inne programy obsługi są rejestrowane, zanim jakiekolwiek komunikaty są odbierane.

## <a name="error-handling-and-logging"></a>Rejestrowanie i obsługa błędów

Łańcuch `catch` metody do końca `start` metodę, aby obsługiwać błędy po stronie klienta. Użyj `console.error` błędy dane wyjściowe do konsoli w przeglądarce.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Konfiguracja po stronie klienta dziennika śledzenia, przekazując rejestratora i typu zdarzenia logowania, gdy połączenie zostało nawiązane. Komunikaty są rejestrowane, z poziomu określonego dziennika lub nowszy. Poziomy dziennika dostępne są następujące:

* `signalR.LogLevel.Error` &ndash; Komunikaty o błędach. Dzienniki `Error` tylko komunikaty.
* `signalR.LogLevel.Warning` &ndash; Komunikaty ostrzegawcze o potencjalnych błędów. Dzienniki `Warning`, i `Error` wiadomości.
* `signalR.LogLevel.Information` &ndash; Komunikaty o stanie bez błędów. Dzienniki `Information`, `Warning`, i `Error` wiadomości.
* `signalR.LogLevel.Trace` &ndash; Komunikaty śledzenia. Rejestruje wszystkim, łącznie z danych przesyłanych między Centrum a klientem.

Użyj [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metody [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) skonfigurować poziom dziennika. Komunikaty są rejestrowane w konsoli przeglądarki.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Ponowne łączenie klientów

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Automatycznie Połącz ponownie

Klient JavaScript dla usługi Signal można skonfigurować do automatycznego ponownego nawiązywania połączenia przy użyciu `withAutomaticReconnect` metody w [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). Domyślnie nie będzie automatycznie ponownie łączyć się.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Bez żadnych parametrów program `withAutomaticReconnect()` skonfiguruje klienta tak, aby czekał 0, 2, 10 i 30 sekund przed podjęciem próby ponownego połączenia, zatrzymywanie po czterech nieudanych próbach.

Przed rozpoczęciem dowolnych prób ponownego `HubConnection` połączenia nastąpi `HubConnectionState.Reconnecting` przejście do stanu i `onreconnecting` uruchomienie jego `Disconnected` wywołania zwrotnego zamiast przejścia do stanu i wyzwalanie jego `onclose` wywołań zwrotnych, takich jak `HubConnection`bez skonfigurowanego automatycznego ponownego łączenia. Dzięki temu można ostrzec użytkowników, że połączenie zostało utracone i wyłączyć elementy interfejsu użytkownika.

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Jeśli klient pomyślnie ponownie nawiązuje połączenie w ramach pierwszych czterech prób, `HubConnection` nastąpi powrót `Connected` do stanu i wyzwolenie `onreconnected` jego wywołania zwrotnego. Zapewnia to możliwość powiadomienia użytkowników o tym, że połączenie zostało ponownie nawiązane.

Ponieważ połączenie jest całkowicie nowe dla serwera, zostanie dostarczone `connectionId` `onreconnected` nowe wywołanie zwrotne.

> [!WARNING]
> Parametr wywołania zwrotnego zostanie niezdefiniowany, [](xref:signalr/configuration#configure-client-options)Jeśli zostałskonfigurowanydopomijania`HubConnection`negocjacji. `onreconnected` `connectionId`

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()`nie zostanie skonfigurowana `HubConnection` do ponawiania początkowych nieudanych uruchomień, dlatego należy ręcznie obsługiwać błędy uruchamiania:

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

Jeśli klient nie `HubConnection` będzie mógł ponownie nawiązać połączenia w ramach pierwszych czterech prób, przejdzie `Disconnected` do stanu i uruchomi wywołania zwrotne jego [zamknięcia](/javascript/api/%40aspnet/signalr/hubconnection#onclose) . Dzięki temu użytkownicy mogą informować, że połączenie zostało trwale utracone i zalecamy odświeżenie strony:

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Aby skonfigurować niestandardową liczbę prób ponownego połączenia przed odłączeniem lub zmianą czasu ponownego połączenia, program akceptuje `withAutomaticReconnect` tablicę liczb reprezentujących opóźnienie (w milisekundach) przed rozpoczęciem każdej próby ponownego nawiązania połączenia.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

W powyższym przykładzie `HubConnection` konfiguruje się, aby rozpocząć próba ponownego połączenia natychmiast po utracie połączenia. Dotyczy to również konfiguracji domyślnej.

Jeśli pierwsza próba ponownego połączenia nie powiedzie się, druga próba ponownego połączenia zostanie również uruchomiona natychmiast, a nie w ciągu 2 sekund, tak jak w przypadku konfiguracji domyślnej.

Jeśli druga próba ponownego połączenia nie powiedzie się, trzecia próba ponownego nawiązania połączenia rozpocznie się w ciągu 10 sekund, co jest ponownie podobne do konfiguracji domyślnej.

Zachowanie niestandardowe jest następnie niezgodne z zachowaniem domyślnym przez zatrzymanie po trzeciej nieudanej próbie nawiązania połączenia zamiast próby wykonania kolejnej próby ponownego połączenia w ciągu innych 30 sekund, takich jak w przypadku konfiguracji domyślnej.

Jeśli chcesz jeszcze większą kontrolę nad chronometrażem i liczbą prób automatycznego ponownego połączenia, `withAutomaticReconnect` zaakceptuje obiekt `IRetryPolicy` implementujący interfejs, który ma pojedynczą metodę o nazwie `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds`przyjmuje jeden argument z typem `RetryContext`. `previousRetryCount` Matrzy`elapsedMilliseconds` właściwości: ,a`retryReason`które są odpowiednio,a`number`ia. `number` `Error` `RetryContext` Przed pierwszym ponownym połączeniem, oba `previousRetryCount` i `elapsedMilliseconds` będą `retryReason` miały wartość zero, a będzie to błąd, który spowodował utratę połączenia. Po każdym nieudanej próbie `previousRetryCount` ponowieniu próby zostanie `elapsedMilliseconds` zaktualizowany w celu odzwierciedlenia ilości czasu poświęconego na przełączenie do tej pory `retryReason` w milisekundach, a będzie to błąd, który spowodował ostatnią próbę ponownego połączenia udało.

`nextRetryDelayInMilliseconds`musi zwracać liczbę określającą liczbę milisekund oczekiwania przed kolejną próbą ponownego połączenia lub `null` `HubConnection` Jeśli należy zatrzymać Ponowne nawiązywanie połączenia.

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

Alternatywnie można napisać kod, który ponownie podłącze klienta ręcznie, jak pokazano w ręcznym [ponownym nawiązaniu połączenia](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Ręczne ponowne łączenie

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> W systemach wcześniejszych niż 3,0 klient JavaScript dla sygnalizującego nie będzie automatycznie ponownie łączyć się. Należy napisać kod, który zostanie nawiązana ponownie ręcznie klienta.

::: moniker-end

Poniższy kod ilustruje typowe podejście do ponownego łączenia ręcznego:

1. Funkcji (w tym przypadku `start` funkcji) jest tworzony, aby rozpocząć połączenie.
1. Wywołaj `start` funkcji przez połączenie `onclose` programu obsługi zdarzeń.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Implementacja rzeczywistych będzie używać wycofywanie wykładnicze lub spróbuj ponownie określoną liczbę razy, przed rezygnacją.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja interfejsów API języka JavaScript](/javascript/api/?view=signalr-js-latest)
* [Samouczek języka JavaScript](xref:tutorials/signalr)
* [Samouczek dotyczący WebPack i TypeScript](xref:tutorials/signalr-typescript-webpack)
* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
* [Żądań Cross-Origin (CORS)](xref:security/cors)
* [Dokumentacja bezserwerowa usługi sygnalizującej platformę Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)
