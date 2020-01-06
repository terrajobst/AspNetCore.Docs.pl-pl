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
ms.openlocfilehash: eaf737642cdbd7ab2b1b5c16538b47a70cddd332
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354696"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a><span data-ttu-id="16608-103">Klient ASP.NET Core SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="16608-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="16608-104">Przez [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="16608-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="16608-105">Biblioteka klienta ASP.NET Core SignalR JavaScript umożliwia deweloperom Wywoływanie kodu centrum po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="16608-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="16608-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="16608-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-opno-locsignalr-client-package"></a><span data-ttu-id="16608-107">Zainstaluj pakiet klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="16608-107">Install the SignalR client package</span></span>

<span data-ttu-id="16608-108">Biblioteka klienta SignalR JavaScript jest dostarczana jako pakiet [npm](https://www.npmjs.com/) .</span><span class="sxs-lookup"><span data-stu-id="16608-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="16608-109">Jeśli używasz programu Visual Studio, uruchom `npm install` z **Konsola Menedżera pakietów** podczas gdy w folderze głównym.</span><span class="sxs-lookup"><span data-stu-id="16608-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="16608-110">Dla programu Visual Studio Code, uruchom polecenie **zintegrowany Terminal**.</span><span class="sxs-lookup"><span data-stu-id="16608-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

<span data-ttu-id="16608-111">npm instaluje zawartość pakietu w *node_modules\\@microsoft\signalr\dist\browser* folderu.</span><span class="sxs-lookup"><span data-stu-id="16608-111">npm installs the package contents in the *node_modules\\@microsoft\signalr\dist\browser* folder.</span></span> <span data-ttu-id="16608-112">Utwórz nowy folder o nazwie *signalr* w obszarze *wwwroot\\lib* folderu.</span><span class="sxs-lookup"><span data-stu-id="16608-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="16608-113">Kopiuj *signalr.js* plik *wwwroot\lib\signalr* folderu.</span><span class="sxs-lookup"><span data-stu-id="16608-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="16608-114">npm instaluje zawartość pakietu w *node_modules\\@aspnet\signalr\dist\browser* folderu.</span><span class="sxs-lookup"><span data-stu-id="16608-114">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="16608-115">Utwórz nowy folder o nazwie *signalr* w obszarze *wwwroot\\lib* folderu.</span><span class="sxs-lookup"><span data-stu-id="16608-115">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="16608-116">Kopiuj *signalr.js* plik *wwwroot\lib\signalr* folderu.</span><span class="sxs-lookup"><span data-stu-id="16608-116">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

## <a name="use-the-opno-locsignalr-javascript-client"></a><span data-ttu-id="16608-117">Korzystanie z klienta SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="16608-117">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="16608-118">Odwołuje się do SignalR klienta JavaScript w elemencie `<script>`.</span><span class="sxs-lookup"><span data-stu-id="16608-118">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="16608-119">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="16608-119">Connect to a hub</span></span>

<span data-ttu-id="16608-120">Poniższy kod tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="16608-120">The following code creates and starts a connection.</span></span> <span data-ttu-id="16608-121">Nazwa Centrum jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="16608-121">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="16608-122">Połączenia między źródłami</span><span class="sxs-lookup"><span data-stu-id="16608-122">Cross-origin connections</span></span>

<span data-ttu-id="16608-123">Zazwyczaj przeglądarek ładowanie połączeń z tej samej domenie co żądanej strony.</span><span class="sxs-lookup"><span data-stu-id="16608-123">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="16608-124">Istnieją jednak sytuacje, gdy wymagane jest połączenie do innej domeny.</span><span class="sxs-lookup"><span data-stu-id="16608-124">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="16608-125">Aby zapobiec złośliwych witryn odczytywanie poufnych danych z innej lokacji [połączeń cross-origin](xref:security/cors) są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="16608-125">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="16608-126">Aby zezwolić na żądania między źródłami, włącz go w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="16608-126">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="16608-127">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="16608-127">Call hub methods from client</span></span>

<span data-ttu-id="16608-128">Klientów JavaScript wywołania metod publicznych w centrach za pośrednictwem [wywołania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) metody [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="16608-128">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="16608-129">`invoke` Metoda przyjmuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="16608-129">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="16608-130">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="16608-130">The name of the hub method.</span></span> <span data-ttu-id="16608-131">W poniższym przykładzie jest nazwa metody koncentratora `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="16608-131">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="16608-132">Argumenty zdefiniowane w metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="16608-132">Any arguments defined in the hub method.</span></span> <span data-ttu-id="16608-133">W poniższym przykładzie jest nazwa argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="16608-133">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="16608-134">Przykładowy kod używa składni funkcji ze strzałką, która jest obsługiwana w bieżących wersjach wszystkich głównych przeglądarek z wyjątkiem programu Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="16608-134">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="16608-135">Jeśli używasz usługi Azure SignalR w *trybie Bezserwerowym*, nie można wywoływać metod centralnych z poziomu klienta.</span><span class="sxs-lookup"><span data-stu-id="16608-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="16608-136">Aby uzyskać więcej informacji, zobacz [dokumentację usługiSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="16608-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="16608-137">Metoda `invoke` zwraca [obietnicę](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="16608-137">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="16608-138">`Promise` jest rozpoznawana z wartością zwracaną (jeśli istnieje), gdy metoda zwraca serwer.</span><span class="sxs-lookup"><span data-stu-id="16608-138">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="16608-139">Jeśli metoda na serwerze zgłasza błąd, `Promise` zostanie odrzucona z komunikatem o błędzie.</span><span class="sxs-lookup"><span data-stu-id="16608-139">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="16608-140">Użyj `then` i `catch` metod na `Promise`, aby obsługiwać te przypadki (lub składnię `await`).</span><span class="sxs-lookup"><span data-stu-id="16608-140">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="16608-141">Metoda `send` zwraca `Promise`języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="16608-141">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="16608-142">`Promise` jest rozpoznawany, gdy wiadomość została wysłana do serwera.</span><span class="sxs-lookup"><span data-stu-id="16608-142">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="16608-143">Jeśli wystąpi błąd podczas wysyłania komunikatu, `Promise` jest odrzucany wraz z komunikatem o błędzie.</span><span class="sxs-lookup"><span data-stu-id="16608-143">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="16608-144">Użyj `then` i `catch` metod na `Promise`, aby obsługiwać te przypadki (lub składnię `await`).</span><span class="sxs-lookup"><span data-stu-id="16608-144">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="16608-145">Użycie `send` nie czeka na odebranie komunikatu przez serwer.</span><span class="sxs-lookup"><span data-stu-id="16608-145">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="16608-146">W związku z tym nie można zwrócić danych ani błędów z serwera.</span><span class="sxs-lookup"><span data-stu-id="16608-146">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="16608-147">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="16608-147">Call client methods from hub</span></span>

<span data-ttu-id="16608-148">Aby odbierać komunikaty z Centrum, definiowanie metody przy użyciu [na](/javascript/api/%40aspnet/signalr/hubconnection#on) metody `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="16608-148">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="16608-149">Nazwa metody klienta JavaScript.</span><span class="sxs-lookup"><span data-stu-id="16608-149">The name of the JavaScript client method.</span></span> <span data-ttu-id="16608-150">W poniższym przykładzie nazwa metody jest `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="16608-150">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="16608-151">Argumenty Centrum przekazuje do metody.</span><span class="sxs-lookup"><span data-stu-id="16608-151">Arguments the hub passes to the method.</span></span> <span data-ttu-id="16608-152">W poniższym przykładzie wartość argumentu jest `message`.</span><span class="sxs-lookup"><span data-stu-id="16608-152">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="16608-153">Powyższy kod w `connection.on` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metody.</span><span class="sxs-lookup"><span data-stu-id="16608-153">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR<span data-ttu-id="16608-154"> określa, która metoda klienta ma być wywoływana przez dopasowanie nazwy metody i argumentów zdefiniowanych w `SendAsync` i `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="16608-154"> determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="16608-155">Najlepszym rozwiązaniem, należy wywołać [start](/javascript/api/%40aspnet/signalr/hubconnection#start) metody `HubConnection` po `on`.</span><span class="sxs-lookup"><span data-stu-id="16608-155">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="16608-156">Daje to pewność, że inne programy obsługi są rejestrowane, zanim jakiekolwiek komunikaty są odbierane.</span><span class="sxs-lookup"><span data-stu-id="16608-156">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="16608-157">Rejestrowanie i obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="16608-157">Error handling and logging</span></span>

<span data-ttu-id="16608-158">Łańcuch `catch` metody do końca `start` metodę, aby obsługiwać błędy po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="16608-158">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="16608-159">Użyj `console.error` błędy dane wyjściowe do konsoli w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="16608-159">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="16608-160">Konfiguracja po stronie klienta dziennika śledzenia, przekazując rejestratora i typu zdarzenia logowania, gdy połączenie zostało nawiązane.</span><span class="sxs-lookup"><span data-stu-id="16608-160">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="16608-161">Komunikaty są rejestrowane, z poziomu określonego dziennika lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="16608-161">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="16608-162">Poziomy dziennika dostępne są następujące:</span><span class="sxs-lookup"><span data-stu-id="16608-162">Available log levels are as follows:</span></span>

* <span data-ttu-id="16608-163">`signalR.LogLevel.Error` &ndash; Komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="16608-163">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="16608-164">Dzienniki `Error` tylko komunikaty.</span><span class="sxs-lookup"><span data-stu-id="16608-164">Logs `Error` messages only.</span></span>
* <span data-ttu-id="16608-165">`signalR.LogLevel.Warning` &ndash; Komunikaty ostrzegawcze o potencjalnych błędów.</span><span class="sxs-lookup"><span data-stu-id="16608-165">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="16608-166">Dzienniki `Warning`, i `Error` wiadomości.</span><span class="sxs-lookup"><span data-stu-id="16608-166">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="16608-167">`signalR.LogLevel.Information` &ndash; Komunikaty o stanie bez błędów.</span><span class="sxs-lookup"><span data-stu-id="16608-167">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="16608-168">Dzienniki `Information`, `Warning`, i `Error` wiadomości.</span><span class="sxs-lookup"><span data-stu-id="16608-168">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="16608-169">`signalR.LogLevel.Trace` &ndash; Komunikaty śledzenia.</span><span class="sxs-lookup"><span data-stu-id="16608-169">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="16608-170">Rejestruje wszystkim, łącznie z danych przesyłanych między Centrum a klientem.</span><span class="sxs-lookup"><span data-stu-id="16608-170">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="16608-171">Użyj [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metody [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) skonfigurować poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="16608-171">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="16608-172">Komunikaty są rejestrowane w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="16608-172">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="16608-173">Ponowne łączenie klientów</span><span class="sxs-lookup"><span data-stu-id="16608-173">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="16608-174">Automatycznie Połącz ponownie</span><span class="sxs-lookup"><span data-stu-id="16608-174">Automatically reconnect</span></span>

<span data-ttu-id="16608-175">Klient JavaScript dla SignalR można skonfigurować w taki sposób, aby automatycznie ponownie łączyć się za pomocą metody `withAutomaticReconnect` w [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="16608-175">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="16608-176">Domyślnie nie będzie automatycznie ponownie łączyć się.</span><span class="sxs-lookup"><span data-stu-id="16608-176">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="16608-177">Bez żadnych parametrów `withAutomaticReconnect()` konfiguruje klienta tak, aby czekał 0, 2, 10 i 30 sekund przed podjęciem próby ponownego nawiązania połączenia, zatrzymując po czterech nieudanych próbach.</span><span class="sxs-lookup"><span data-stu-id="16608-177">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="16608-178">Przed rozpoczęciem jakichkolwiek prób ponownego łączenia `HubConnection` przechodzi do stanu `HubConnectionState.Reconnecting` i wyzwalane przez `onreconnecting` wywołania zwrotne, a nie przejście do stanu `Disconnected` i wyzwalanie `onclose` wywołań zwrotnych, takich jak `HubConnection` bez skonfigurowanego automatycznego ponownego łączenia.</span><span class="sxs-lookup"><span data-stu-id="16608-178">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="16608-179">Dzięki temu można ostrzec użytkowników, że połączenie zostało utracone i wyłączyć elementy interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="16608-179">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="16608-180">Jeśli klient pomyślnie ponownie nawiąże połączenie w ramach pierwszych czterech prób, `HubConnection` przejdzie z powrotem do stanu `Connected` i uruchomi wywołania zwrotne `onreconnected`.</span><span class="sxs-lookup"><span data-stu-id="16608-180">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="16608-181">Zapewnia to możliwość powiadomienia użytkowników o tym, że połączenie zostało ponownie nawiązane.</span><span class="sxs-lookup"><span data-stu-id="16608-181">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="16608-182">Ponieważ połączenie jest całkowicie nowe dla serwera, do wywołania zwrotnego `onreconnected` zostanie dostarczone nowe `connectionId`.</span><span class="sxs-lookup"><span data-stu-id="16608-182">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="16608-183">Parametr `connectionId` wywołania zwrotnego `onreconnected` zostanie niezdefiniowany, jeśli `HubConnection` został skonfigurowany do [pomijania negocjacji](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="16608-183">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="16608-184">`withAutomaticReconnect()` nie skonfiguruje `HubConnection` w celu ponowienia nieudanych uruchomień początkowych, dlatego należy ręcznie obsługiwać błędy uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="16608-184">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="16608-185">Jeśli klient nie będzie mógł ponownie nawiązać połączenia w ramach pierwszych czterech prób, `HubConnection` przejdzie do stanu `Disconnected` i zostanie wyzwolony [jego wywołania](/javascript/api/%40aspnet/signalr/hubconnection#onclose) zwrotne.</span><span class="sxs-lookup"><span data-stu-id="16608-185">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="16608-186">Dzięki temu użytkownicy mogą informować, że połączenie zostało trwale utracone i zalecamy odświeżenie strony:</span><span class="sxs-lookup"><span data-stu-id="16608-186">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="16608-187">Aby skonfigurować niestandardową liczbę prób ponownego połączenia przed odłączeniem lub zmianą czasu ponownego połączenia, `withAutomaticReconnect` akceptuje tablicę liczb reprezentujących opóźnienie (w milisekundach) przed rozpoczęciem każdej próby ponownego nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="16608-187">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="16608-188">Powyższy przykład konfiguruje `HubConnection`, aby rozpocząć próbę ponownego połączenia natychmiast po utracie połączenia.</span><span class="sxs-lookup"><span data-stu-id="16608-188">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="16608-189">Dotyczy to również konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="16608-189">This is also true for the default configuration.</span></span>

<span data-ttu-id="16608-190">Jeśli pierwsza próba ponownego połączenia nie powiedzie się, druga próba ponownego połączenia zostanie również uruchomiona natychmiast, a nie w ciągu 2 sekund, tak jak w przypadku konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="16608-190">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="16608-191">Jeśli druga próba ponownego połączenia nie powiedzie się, trzecia próba ponownego nawiązania połączenia rozpocznie się w ciągu 10 sekund, co jest ponownie podobne do konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="16608-191">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="16608-192">Zachowanie niestandardowe jest następnie niezgodne z zachowaniem domyślnym przez zatrzymanie po trzeciej nieudanej próbie nawiązania połączenia zamiast próby wykonania kolejnej próby ponownego połączenia w ciągu innych 30 sekund, takich jak w przypadku konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="16608-192">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="16608-193">Jeśli potrzebujesz jeszcze większą kontrolę nad chronometrażem i liczbą prób automatycznego ponownego połączenia, `withAutomaticReconnect` akceptuje obiekt implementujący interfejs `IRetryPolicy`, który ma pojedynczą metodę o nazwie `nextRetryDelayInMilliseconds`.</span><span class="sxs-lookup"><span data-stu-id="16608-193">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="16608-194">`nextRetryDelayInMilliseconds` przyjmuje jeden argument z typem `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="16608-194">`nextRetryDelayInMilliseconds` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="16608-195">`RetryContext` ma trzy właściwości: `previousRetryCount`, `elapsedMilliseconds` i `retryReason`, które są `number`, `number` i `Error` odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="16608-195">The `RetryContext` has three properties: `previousRetryCount`, `elapsedMilliseconds` and `retryReason` which are a `number`, a `number` and an `Error` respectively.</span></span> <span data-ttu-id="16608-196">Przed pierwszą próbą ponownego połączenia oba `previousRetryCount` i `elapsedMilliseconds` będą miały wartość zero, a `retryReason` będzie błędem, który spowodował utratę połączenia.</span><span class="sxs-lookup"><span data-stu-id="16608-196">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero, and the `retryReason` will be the Error that caused the connection to be lost.</span></span> <span data-ttu-id="16608-197">Po każdym nieudanej próbie ponowieniu próby `previousRetryCount` będzie zwiększane o jeden, `elapsedMilliseconds` zostanie zaktualizowany w celu odzwierciedlenia ilości czasu poświęconego na przełączenie do tej pory w milisekundach, a `retryReason` będzie błędem, który spowodował, że Ostatnia próba ponownego połączenia nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="16608-197">After each failed retry attempt, `previousRetryCount` will be incremented by one, `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds, and the `retryReason` will be the Error that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="16608-198">`nextRetryDelayInMilliseconds` musi zwrócić liczbę określającą liczbę milisekund oczekiwania przed kolejną próbą ponownego połączenia lub `null`, jeśli `HubConnection` powinna zatrzymać Ponowne nawiązywanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="16608-198">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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
        }
    })
    .build();
```

<span data-ttu-id="16608-199">Alternatywnie można napisać kod, który ponownie podłącze klienta ręcznie, jak pokazano w [ręcznym ponownym nawiązaniu połączenia](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="16608-199">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="16608-200">Ręczne ponowne łączenie</span><span class="sxs-lookup"><span data-stu-id="16608-200">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="16608-201">W systemach wcześniejszych niż 3,0 klient JavaScript dla SignalR nie będzie automatycznie ponownie łączyć się.</span><span class="sxs-lookup"><span data-stu-id="16608-201">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="16608-202">Należy napisać kod, który zostanie nawiązana ponownie ręcznie klienta.</span><span class="sxs-lookup"><span data-stu-id="16608-202">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="16608-203">Poniższy kod ilustruje typowe podejście do ponownego łączenia ręcznego:</span><span class="sxs-lookup"><span data-stu-id="16608-203">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="16608-204">Funkcji (w tym przypadku `start` funkcji) jest tworzony, aby rozpocząć połączenie.</span><span class="sxs-lookup"><span data-stu-id="16608-204">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="16608-205">Wywołaj `start` funkcji przez połączenie `onclose` programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="16608-205">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="16608-206">Implementacja rzeczywistych będzie używać wycofywanie wykładnicze lub spróbuj ponownie określoną liczbę razy, przed rezygnacją.</span><span class="sxs-lookup"><span data-stu-id="16608-206">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16608-207">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="16608-207">Additional resources</span></span>

* [<span data-ttu-id="16608-208">Dokumentacja interfejsów API języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="16608-208">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="16608-209">Samouczek języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="16608-209">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="16608-210">Samouczek dotyczący WebPack i TypeScript</span><span class="sxs-lookup"><span data-stu-id="16608-210">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="16608-211">Centra</span><span class="sxs-lookup"><span data-stu-id="16608-211">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="16608-212">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="16608-212">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="16608-213">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="16608-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="16608-214">Żądań Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="16608-214">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* <span data-ttu-id="16608-215">[Dokumentacja bezserwerowa usługi SignalR platformy Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="16608-215">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
