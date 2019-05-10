---
title: ASP.NET Core SignalR JavaScript klienta
author: bradygaster
description: Omówienie platformy ASP.NET Core SignalR JavaScript klienta.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: 8ac6528b203831f426feabf4cdd3e0ed293b75c5
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902596"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript klienta

Przez [Rachel Appel](http://twitter.com/rachelappel)

Biblioteki klienta platformy ASP.NET Core SignalR JavaScript umożliwia deweloperom wywołanie kodu koncentratora po stronie serwera.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Zainstaluj pakiet klienta SignalR

Biblioteka klienta SignalR JavaScript jest dostarczana jako [npm](https://www.npmjs.com/) pakietu. Jeśli używasz programu Visual Studio, uruchom `npm install` z **Konsola Menedżera pakietów** podczas gdy w folderze głównym. Dla programu Visual Studio Code, uruchom polecenie **zintegrowany Terminal**.

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm instaluje zawartość pakietu w *node_modules\\@aspnet\signalr\dist\browser* folderu. Utwórz nowy folder o nazwie *signalr* w obszarze *wwwroot\\lib* folderu. Kopiuj *signalr.js* plik *wwwroot\lib\signalr* folderu.

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
* Argumenty zdefiniowane w metody koncentratora. W poniższym przykładzie jest nazwa argumentu `message`. Przykładowy kod używa Składnia funkcji strzałki, które jest obsługiwane w aktualnych wersjach wszystkich popularnych przeglądarkach, z wyjątkiem programu Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Jeśli używasz usługi Azure SignalR Service w *trybu bez użycia serwera*, nie można wywołać metody koncentratora klienta. Aby uzyskać więcej informacji, zobacz [dokumentacji usługi SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

`invoke` Metoda zwraca JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise). `Promise` Rozwiązania z wartością zwracaną (jeśli istnieją) po powrocie z metody, które na serwerze. Jeśli metoda na serwerze zgłasza błąd, `Promise` zostanie odrzucone z powodu błędu. Użyj `then` i `catch` metod `Promise` sam Obsługa tych przypadków (lub `await` składni).

`send` Metoda zwraca JavaScript `Promise`. `Promise` Zostanie rozwiązany, kiedy wiadomość została wysłana do serwera. Jeśli występuje błąd podczas wysyłania komunikatu `Promise` zostanie odrzucone z powodu błędu. Użyj `then` i `catch` metod `Promise` sam Obsługa tych przypadków (lub `await` składni).

> [!NOTE]
> Za pomocą `send` nie czeka, aż serwer odebrał komunikat. W związku z tym nie jest możliwe zwracane błędy lub dane z serwera.

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

### <a name="automatically-reconnect"></a>Automatyczne ponowne łączenie

Klient JavaScript dla biblioteki SignalR można skonfigurować w celu automatycznie ponownie połączyć się przy użyciu `withAutomaticReconnect` metody [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). Nie będzie automatycznie ponownie się domyślnie.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Bez żadnych parametrów `withAutomaticReconnect()` konfiguruje klienta w celu odczekaj 0, 2, 10 i 30 sekund, odpowiednio, przed próbą każdą próbę ponownego nawiązania połączenia, trwa zatrzymywanie po czterech nieudanych próbach.

Przed rozpoczęciem wszelkich prób ponownego połączenia `HubConnection` spowoduje przejście do `HubConnectionState.Reconnecting` stanu i szybko jego `onreconnecting` wywołania zwrotne zamiast przechodzenia do `Disconnected` stanu i wyzwalając jego `onclose` wywołań zwrotnych, takich jak `HubConnection`bez automatycznie ponownie skonfigurowane. Zapewnia to możliwość ostrzegać użytkowników, że połączenie zostało utracone i wyłączający elementy interfejsu użytkownika.

```javascript
connection.onreconnecting((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
  document.getElementById("messagesList").appendChild(li);
});
```

Jeśli klient pomyślnie połączy się ponownie w ramach jego pierwsze cztery prób `HubConnection` przejdą do `Connected` stanu i szybko jego `onreconnected` wywołań zwrotnych. Zapewnia możliwości, aby poinformować użytkowników, dla których ponownie ustanowić połączenia.

Ponieważ połączenia całkowicie nowych odwołuje się do serwera, nowy `connectionId` zostanie udzielona `onreconnected` wywołania zwrotnego.

> [!WARNING]
> `onreconnected` Przez wywołanie zwrotne `connectionId` parametr będzie niezdefiniowane, jeżeli `HubConnection` został skonfigurowany do [pominąć negocjacji](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
  console.assert(connection.state === signalR.HubConnectionState.Connected);

  document.getElementById("messageInput").disabled = false;

  const li = document.createElement("li");
  li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
  document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` nie Konfiguruj `HubConnection` próbę uruchomienia początkowego błędów, więc błędy start muszą być obsługiwani ręcznie:

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

Jeśli klient nie pomyślnie ponownie połączyć w ramach jego pierwsze cztery prób `HubConnection` spowoduje przejście do `Disconnected` stanu i szybko jego [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) wywołań zwrotnych. Zapewnia to możliwość Poinformuj użytkowników o połączenie zostało trwale utracone i zaleca, aby odświeżyć stronę:

```javascript
connection.onclose((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Disconnected);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
  document.getElementById("messagesList").appendChild(li);
})
```

Aby można było skonfigurować niestandardowe liczbę prób ponownego połączenia przed rozłączeniem lub zmienić czas ponownego nawiązania połączenia `withAutomaticReconnect` akceptuje tablicy liczb reprezentujący opóźnienie (w milisekundach) oczekiwania przed uruchomieniem każdą próbę ponownego połączenia.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

Poprzedni przykład konfiguruje `HubConnection` można uruchomić próby Ponowne podłączenia natychmiast, po połączenie zostanie przerwane. Dotyczy to również w konfiguracji domyślnej.

Jeśli pierwsza próba ponownego połączenia nie powiedzie się, druga próba ponownego nawiązania połączenia również rozpocznie się natychmiast zamiast czekać na 2 sekundy, tak jak miałoby to miejsce w konfiguracji domyślnej.

Jeśli druga próba ponownego połączenia nie powiedzie się, trzeci próba ponownego nawiązania połączenia zostanie uruchomiona w ciągu 10 sekund, które jest ponownie, takich jak konfiguracja domyślna.

Niestandardowe zachowanie następnie diverges ponownie z zachowania domyślnego, zatrzymując po trzecie reconnect próby błędu zamiast próby jeden bardziej ponownie próbę innego 30 sekund, tak jak miałoby to miejsce w konfiguracji domyślnej.

Jeśli chcesz, aby jeszcze bardziej kontrolować czas i liczba automatyczne ponowne łączenie prób `withAutomaticReconnect` akceptuje implementacji obiektu `IReconnectPolicy` interfejs, który zawiera jedną metodę o nazwie `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds` przyjmuje dwa argumenty `previousRetryCount` i `elapsedMilliseconds`, które są zarówno liczby. Przed pierwsza próba ponownego nawiązania połączenia zarówno `previousRetryCount` i `elapsedMilliseconds` będzie mieć wartość zero. Po każdej próbie ponawiania nie powiodło się `previousRetryCount` jest zwiększany o jedną i `elapsedMilliseconds` zostanie zaktualizowany, aby odzwierciedlić czas poświęcony na ponowne łączenie do tej pory w milisekundach.

`nextRetryDelayInMilliseconds` musi zwracać wartość liczbowa reprezentująca liczbę milisekund oczekiwania przed kolejnym próby ponownego nawiązania połączenia lub `null` Jeśli `HubConnection` ma zostać zatrzymana, ponowne łączenie.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: (previousRetryCount, elapsedMilliseconds) => {
          if (elapsedMilliseconds < 60000) {
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

Alternatywnie, można napisać kod, który zostanie nawiązana ponownie ręcznie, jak pokazano w kliencie [ręcznie połączyć](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Ręcznie połączyć

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Przed 3.0 nie automatycznie ponownie klienta JavaScript dla elementu SignalR. Należy napisać kod, który zostanie nawiązana ponownie ręcznie klienta.

::: moniker-end

Poniższy kod przedstawia metody typowego ręczne ponowne nawiązanie połączenia:

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
* [Dokumentacja usługi Azure SignalR Service bez użycia serwera](/azure/azure-signalr/signalr-concept-serverless-development-config)
