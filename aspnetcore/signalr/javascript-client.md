---
title: ASP.NET Core SignalR JavaScript klienta
author: bradygaster
description: Omówienie platformy ASP.NET Core SignalR JavaScript klienta.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: 1565aa38a69113781d7c272a1710298cccc1f045
ms.sourcegitcommit: 3eedd6180fbbdcb81a8e1ebdbeb035bf4f2feb92
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284517"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="324f7-103">ASP.NET Core SignalR JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="324f7-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="324f7-104">Przez [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="324f7-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="324f7-105">Biblioteki klienta platformy ASP.NET Core SignalR JavaScript umożliwia deweloperom wywołanie kodu koncentratora po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="324f7-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="324f7-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="324f7-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="324f7-107">Zainstaluj pakiet klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="324f7-107">Install the SignalR client package</span></span>

<span data-ttu-id="324f7-108">Biblioteka klienta SignalR JavaScript jest dostarczana jako [npm](https://www.npmjs.com/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="324f7-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="324f7-109">Jeśli używasz programu Visual Studio, uruchom `npm install` z **Konsola Menedżera pakietów** podczas gdy w folderze głównym.</span><span class="sxs-lookup"><span data-stu-id="324f7-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="324f7-110">Dla programu Visual Studio Code, uruchom polecenie **zintegrowany Terminal**.</span><span class="sxs-lookup"><span data-stu-id="324f7-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="324f7-111">npm instaluje zawartość pakietu w *node_modules\\@aspnet\signalr\dist\browser* folderu.</span><span class="sxs-lookup"><span data-stu-id="324f7-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="324f7-112">Utwórz nowy folder o nazwie *signalr* w obszarze *wwwroot\\lib* folderu.</span><span class="sxs-lookup"><span data-stu-id="324f7-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="324f7-113">Kopiuj *signalr.js* plik *wwwroot\lib\signalr* folderu.</span><span class="sxs-lookup"><span data-stu-id="324f7-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="324f7-114">Używanie klienta SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="324f7-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="324f7-115">Odwoływać się do klienta SignalR JavaScript w `<script>` elementu.</span><span class="sxs-lookup"><span data-stu-id="324f7-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="324f7-116">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="324f7-116">Connect to a hub</span></span>

<span data-ttu-id="324f7-117">Poniższy kod tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="324f7-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="324f7-118">Nazwa Centrum jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="324f7-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="324f7-119">Połączenia między źródłami</span><span class="sxs-lookup"><span data-stu-id="324f7-119">Cross-origin connections</span></span>

<span data-ttu-id="324f7-120">Zazwyczaj przeglądarek ładowanie połączeń z tej samej domenie co żądanej strony.</span><span class="sxs-lookup"><span data-stu-id="324f7-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="324f7-121">Istnieją jednak sytuacje, gdy wymagane jest połączenie do innej domeny.</span><span class="sxs-lookup"><span data-stu-id="324f7-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="324f7-122">Aby zapobiec złośliwych witryn odczytywanie poufnych danych z innej lokacji [połączeń cross-origin](xref:security/cors) są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="324f7-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="324f7-123">Aby zezwolić na żądania między źródłami, włącz go w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="324f7-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="324f7-124">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="324f7-124">Call hub methods from client</span></span>

<span data-ttu-id="324f7-125">Klientów JavaScript wywołania metod publicznych w centrach za pośrednictwem [wywołania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) metody [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="324f7-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="324f7-126">`invoke` Metoda przyjmuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="324f7-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="324f7-127">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="324f7-127">The name of the hub method.</span></span> <span data-ttu-id="324f7-128">W poniższym przykładzie jest nazwa metody koncentratora `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="324f7-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="324f7-129">Argumenty zdefiniowane w metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="324f7-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="324f7-130">W poniższym przykładzie jest nazwa argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="324f7-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="324f7-131">Przykładowy kod używa Składnia funkcji strzałki, które jest obsługiwane w aktualnych wersjach wszystkich popularnych przeglądarkach, z wyjątkiem programu Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="324f7-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="324f7-132">Jeśli używasz usługi Azure SignalR Service w *trybu bez użycia serwera*, nie można wywołać metody koncentratora klienta.</span><span class="sxs-lookup"><span data-stu-id="324f7-132">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="324f7-133">Aby uzyskać więcej informacji, zobacz [dokumentacji usługi SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="324f7-133">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="324f7-134">`invoke` Metoda zwraca JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span><span class="sxs-lookup"><span data-stu-id="324f7-134">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="324f7-135">`Promise` Rozwiązania z wartością zwracaną (jeśli istnieją) po powrocie z metody, które na serwerze.</span><span class="sxs-lookup"><span data-stu-id="324f7-135">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="324f7-136">Jeśli metoda na serwerze zgłasza błąd, `Promise` zostanie odrzucone z powodu błędu.</span><span class="sxs-lookup"><span data-stu-id="324f7-136">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="324f7-137">Użyj `then` i `catch` metod `Promise` sam Obsługa tych przypadków (lub `await` składni).</span><span class="sxs-lookup"><span data-stu-id="324f7-137">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="324f7-138">`send` Metoda zwraca JavaScript `Promise`.</span><span class="sxs-lookup"><span data-stu-id="324f7-138">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="324f7-139">`Promise` Zostanie rozwiązany, kiedy wiadomość została wysłana do serwera.</span><span class="sxs-lookup"><span data-stu-id="324f7-139">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="324f7-140">Jeśli występuje błąd podczas wysyłania komunikatu `Promise` zostanie odrzucone z powodu błędu.</span><span class="sxs-lookup"><span data-stu-id="324f7-140">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="324f7-141">Użyj `then` i `catch` metod `Promise` sam Obsługa tych przypadków (lub `await` składni).</span><span class="sxs-lookup"><span data-stu-id="324f7-141">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="324f7-142">Za pomocą `send` nie czeka, aż serwer odebrał komunikat.</span><span class="sxs-lookup"><span data-stu-id="324f7-142">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="324f7-143">W związku z tym nie jest możliwe zwracane błędy lub dane z serwera.</span><span class="sxs-lookup"><span data-stu-id="324f7-143">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="324f7-144">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="324f7-144">Call client methods from hub</span></span>

<span data-ttu-id="324f7-145">Aby odbierać komunikaty z Centrum, definiowanie metody przy użyciu [na](/javascript/api/%40aspnet/signalr/hubconnection#on) metody `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="324f7-145">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="324f7-146">Nazwa metody klienta JavaScript.</span><span class="sxs-lookup"><span data-stu-id="324f7-146">The name of the JavaScript client method.</span></span> <span data-ttu-id="324f7-147">W poniższym przykładzie nazwa metody jest `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="324f7-147">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="324f7-148">Argumenty Centrum przekazuje do metody.</span><span class="sxs-lookup"><span data-stu-id="324f7-148">Arguments the hub passes to the method.</span></span> <span data-ttu-id="324f7-149">W poniższym przykładzie wartość argumentu jest `message`.</span><span class="sxs-lookup"><span data-stu-id="324f7-149">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="324f7-150">Powyższy kod w `connection.on` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metody.</span><span class="sxs-lookup"><span data-stu-id="324f7-150">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="324f7-151">Określa SignalR, jakiej metody klienta do wywołania, dopasowując nazwy metody i argumenty zdefiniowane w `SendAsync` i `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="324f7-151">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="324f7-152">Najlepszym rozwiązaniem, należy wywołać [start](/javascript/api/%40aspnet/signalr/hubconnection#start) metody `HubConnection` po `on`.</span><span class="sxs-lookup"><span data-stu-id="324f7-152">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="324f7-153">Daje to pewność, że inne programy obsługi są rejestrowane, zanim jakiekolwiek komunikaty są odbierane.</span><span class="sxs-lookup"><span data-stu-id="324f7-153">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="324f7-154">Rejestrowanie i obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="324f7-154">Error handling and logging</span></span>

<span data-ttu-id="324f7-155">Łańcuch `catch` metody do końca `start` metodę, aby obsługiwać błędy po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="324f7-155">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="324f7-156">Użyj `console.error` błędy dane wyjściowe do konsoli w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="324f7-156">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="324f7-157">Konfiguracja po stronie klienta dziennika śledzenia, przekazując rejestratora i typu zdarzenia logowania, gdy połączenie zostało nawiązane.</span><span class="sxs-lookup"><span data-stu-id="324f7-157">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="324f7-158">Komunikaty są rejestrowane, z poziomu określonego dziennika lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="324f7-158">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="324f7-159">Poziomy dziennika dostępne są następujące:</span><span class="sxs-lookup"><span data-stu-id="324f7-159">Available log levels are as follows:</span></span>

* <span data-ttu-id="324f7-160">`signalR.LogLevel.Error` &ndash; Komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="324f7-160">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="324f7-161">Dzienniki `Error` tylko komunikaty.</span><span class="sxs-lookup"><span data-stu-id="324f7-161">Logs `Error` messages only.</span></span>
* <span data-ttu-id="324f7-162">`signalR.LogLevel.Warning` &ndash; Komunikaty ostrzegawcze o potencjalnych błędów.</span><span class="sxs-lookup"><span data-stu-id="324f7-162">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="324f7-163">Dzienniki `Warning`, i `Error` wiadomości.</span><span class="sxs-lookup"><span data-stu-id="324f7-163">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="324f7-164">`signalR.LogLevel.Information` &ndash; Komunikaty o stanie bez błędów.</span><span class="sxs-lookup"><span data-stu-id="324f7-164">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="324f7-165">Dzienniki `Information`, `Warning`, i `Error` wiadomości.</span><span class="sxs-lookup"><span data-stu-id="324f7-165">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="324f7-166">`signalR.LogLevel.Trace` &ndash; Komunikaty śledzenia.</span><span class="sxs-lookup"><span data-stu-id="324f7-166">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="324f7-167">Rejestruje wszystkim, łącznie z danych przesyłanych między Centrum a klientem.</span><span class="sxs-lookup"><span data-stu-id="324f7-167">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="324f7-168">Użyj [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metody [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) skonfigurować poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="324f7-168">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="324f7-169">Komunikaty są rejestrowane w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="324f7-169">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="324f7-170">Ponowne łączenie klientów</span><span class="sxs-lookup"><span data-stu-id="324f7-170">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="324f7-171">Automatyczne ponowne łączenie</span><span class="sxs-lookup"><span data-stu-id="324f7-171">Automatically reconnect</span></span>

<span data-ttu-id="324f7-172">Klient JavaScript dla biblioteki SignalR można skonfigurować w celu automatycznie ponownie połączyć się przy użyciu `withAutomaticReconnect` metody [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="324f7-172">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="324f7-173">Nie będzie automatycznie ponownie się domyślnie.</span><span class="sxs-lookup"><span data-stu-id="324f7-173">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="324f7-174">Bez żadnych parametrów `withAutomaticReconnect()` konfiguruje klienta w celu odczekaj 0, 2, 10 i 30 sekund, odpowiednio, przed próbą każdą próbę ponownego nawiązania połączenia, trwa zatrzymywanie po czterech nieudanych próbach.</span><span class="sxs-lookup"><span data-stu-id="324f7-174">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="324f7-175">Przed rozpoczęciem wszelkich prób ponownego połączenia `HubConnection` spowoduje przejście do `HubConnectionState.Reconnecting` stanu i szybko jego `onreconnecting` wywołania zwrotne zamiast przechodzenia do `Disconnected` stanu i wyzwalając jego `onclose` wywołań zwrotnych, takich jak `HubConnection`bez automatycznie ponownie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="324f7-175">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="324f7-176">Zapewnia to możliwość ostrzegać użytkowników, że połączenie zostało utracone i wyłączający elementy interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="324f7-176">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="324f7-177">Jeśli klient pomyślnie połączy się ponownie w ramach jego pierwsze cztery prób `HubConnection` przejdą do `Connected` stanu i szybko jego `onreconnected` wywołań zwrotnych.</span><span class="sxs-lookup"><span data-stu-id="324f7-177">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="324f7-178">Zapewnia możliwości, aby poinformować użytkowników, dla których ponownie ustanowić połączenia.</span><span class="sxs-lookup"><span data-stu-id="324f7-178">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="324f7-179">Ponieważ połączenia całkowicie nowych odwołuje się do serwera, nowy `connectionId` zostanie udzielona `onreconnected` wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="324f7-179">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="324f7-180">`onreconnected` Przez wywołanie zwrotne `connectionId` parametr będzie niezdefiniowane, jeżeli `HubConnection` został skonfigurowany do [pominąć negocjacji](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="324f7-180">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="324f7-181">`withAutomaticReconnect()` nie Konfiguruj `HubConnection` próbę uruchomienia początkowego błędów, więc błędy start muszą być obsługiwani ręcznie:</span><span class="sxs-lookup"><span data-stu-id="324f7-181">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="324f7-182">Jeśli klient nie pomyślnie ponownie połączyć w ramach jego pierwsze cztery prób `HubConnection` spowoduje przejście do `Disconnected` stanu i szybko jego [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) wywołań zwrotnych.</span><span class="sxs-lookup"><span data-stu-id="324f7-182">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="324f7-183">Zapewnia to możliwość Poinformuj użytkowników o połączenie zostało trwale utracone i zaleca, aby odświeżyć stronę:</span><span class="sxs-lookup"><span data-stu-id="324f7-183">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="324f7-184">Aby można było skonfigurować niestandardowe liczbę prób ponownego połączenia przed rozłączeniem lub zmienić czas ponownego nawiązania połączenia `withAutomaticReconnect` akceptuje tablicy liczb reprezentujący opóźnienie (w milisekundach) oczekiwania przed uruchomieniem każdą próbę ponownego połączenia.</span><span class="sxs-lookup"><span data-stu-id="324f7-184">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="324f7-185">Poprzedni przykład konfiguruje `HubConnection` można uruchomić próby Ponowne podłączenia natychmiast, po połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="324f7-185">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="324f7-186">Dotyczy to również w konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="324f7-186">This is also true for the default configuration.</span></span>

<span data-ttu-id="324f7-187">Jeśli pierwsza próba ponownego połączenia nie powiedzie się, druga próba ponownego nawiązania połączenia również rozpocznie się natychmiast zamiast czekać na 2 sekundy, tak jak miałoby to miejsce w konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="324f7-187">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="324f7-188">Jeśli druga próba ponownego połączenia nie powiedzie się, trzeci próba ponownego nawiązania połączenia zostanie uruchomiona w ciągu 10 sekund, które jest ponownie, takich jak konfiguracja domyślna.</span><span class="sxs-lookup"><span data-stu-id="324f7-188">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="324f7-189">Niestandardowe zachowanie następnie diverges ponownie z zachowania domyślnego, zatrzymując po trzecie reconnect próby błędu zamiast próby jeden bardziej ponownie próbę innego 30 sekund, tak jak miałoby to miejsce w konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="324f7-189">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="324f7-190">Jeśli chcesz, aby jeszcze bardziej kontrolować czas i liczba automatyczne ponowne łączenie prób `withAutomaticReconnect` akceptuje implementacji obiektu `IReconnectPolicy` interfejs, który zawiera jedną metodę o nazwie `nextRetryDelayInMilliseconds`.</span><span class="sxs-lookup"><span data-stu-id="324f7-190">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IReconnectPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="324f7-191">`nextRetryDelayInMilliseconds` przyjmuje dwa argumenty `previousRetryCount` i `elapsedMilliseconds`, które są zarówno liczby.</span><span class="sxs-lookup"><span data-stu-id="324f7-191">`nextRetryDelayInMilliseconds` takes two arguments, `previousRetryCount` and `elapsedMilliseconds`, which are both numbers.</span></span> <span data-ttu-id="324f7-192">Przed pierwsza próba ponownego nawiązania połączenia zarówno `previousRetryCount` i `elapsedMilliseconds` będzie mieć wartość zero.</span><span class="sxs-lookup"><span data-stu-id="324f7-192">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero.</span></span> <span data-ttu-id="324f7-193">Po każdej próbie ponawiania nie powiodło się `previousRetryCount` jest zwiększany o jedną i `elapsedMilliseconds` zostanie zaktualizowany, aby odzwierciedlić czas poświęcony na ponowne łączenie do tej pory w milisekundach.</span><span class="sxs-lookup"><span data-stu-id="324f7-193">After each failed retry attempt, `previousRetryCount` will be incremented by one and `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds.</span></span>

<span data-ttu-id="324f7-194">`nextRetryDelayInMilliseconds` musi zwracać wartość liczbowa reprezentująca liczbę milisekund oczekiwania przed kolejnym próby ponownego nawiązania połączenia lub `null` Jeśli `HubConnection` ma zostać zatrzymana, ponowne łączenie.</span><span class="sxs-lookup"><span data-stu-id="324f7-194">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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
        }
    })
    .build();
```

<span data-ttu-id="324f7-195">Alternatywnie, można napisać kod, który zostanie nawiązana ponownie ręcznie, jak pokazano w kliencie [ręcznie połączyć](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="324f7-195">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="324f7-196">Ręcznie połączyć</span><span class="sxs-lookup"><span data-stu-id="324f7-196">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="324f7-197">Przed 3.0 nie automatycznie ponownie klienta JavaScript dla elementu SignalR.</span><span class="sxs-lookup"><span data-stu-id="324f7-197">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="324f7-198">Należy napisać kod, który zostanie nawiązana ponownie ręcznie klienta.</span><span class="sxs-lookup"><span data-stu-id="324f7-198">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="324f7-199">Poniższy kod przedstawia metody typowego ręczne ponowne nawiązanie połączenia:</span><span class="sxs-lookup"><span data-stu-id="324f7-199">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="324f7-200">Funkcji (w tym przypadku `start` funkcji) jest tworzony, aby rozpocząć połączenie.</span><span class="sxs-lookup"><span data-stu-id="324f7-200">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="324f7-201">Wywołaj `start` funkcji przez połączenie `onclose` programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="324f7-201">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="324f7-202">Implementacja rzeczywistych będzie używać wycofywanie wykładnicze lub spróbuj ponownie określoną liczbę razy, przed rezygnacją.</span><span class="sxs-lookup"><span data-stu-id="324f7-202">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="324f7-203">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="324f7-203">Additional resources</span></span>

* [<span data-ttu-id="324f7-204">Dokumentacja interfejsów API języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="324f7-204">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="324f7-205">Samouczek języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="324f7-205">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="324f7-206">Samouczek dotyczący WebPack i TypeScript</span><span class="sxs-lookup"><span data-stu-id="324f7-206">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="324f7-207">Centra</span><span class="sxs-lookup"><span data-stu-id="324f7-207">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="324f7-208">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="324f7-208">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="324f7-209">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="324f7-209">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="324f7-210">Żądań Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="324f7-210">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="324f7-211">Dokumentacja usługi Azure SignalR Service bez użycia serwera</span><span class="sxs-lookup"><span data-stu-id="324f7-211">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
