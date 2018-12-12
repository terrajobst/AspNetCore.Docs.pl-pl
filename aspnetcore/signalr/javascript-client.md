---
title: ASP.NET Core SignalR JavaScript klienta
author: tdykstra
description: Omówienie platformy ASP.NET Core SignalR JavaScript klienta.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: cd64a65889227d84615768bc3d8fddcd362fbba4
ms.sourcegitcommit: eef99d14d96dc8c3c1bb0e2c4cb14da152f8a952
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53022482"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="0f7ce-103">ASP.NET Core SignalR JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="0f7ce-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="0f7ce-104">Przez [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="0f7ce-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="0f7ce-105">Biblioteki klienta platformy ASP.NET Core SignalR JavaScript umożliwia deweloperom wywołanie kodu koncentratora po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="0f7ce-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0f7ce-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="0f7ce-107">Zainstaluj pakiet klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="0f7ce-107">Install the SignalR client package</span></span>

<span data-ttu-id="0f7ce-108">Biblioteka klienta SignalR JavaScript jest dostarczana jako [npm](https://www.npmjs.com/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="0f7ce-109">Jeśli używasz programu Visual Studio, uruchom `npm install` z **Konsola Menedżera pakietów** podczas gdy w folderze głównym.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="0f7ce-110">Dla programu Visual Studio Code, uruchom polecenie **zintegrowany Terminal**.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="0f7ce-111">npm instaluje zawartość pakietu w *node_modules\\@aspnet\signalr\dist\browser* folderu.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="0f7ce-112">Utwórz nowy folder o nazwie *signalr* w obszarze *wwwroot\\lib* folderu.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="0f7ce-113">Kopiuj *signalr.js* plik *wwwroot\lib\signalr* folderu.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="0f7ce-114">Używanie klienta SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="0f7ce-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="0f7ce-115">Odwoływać się do klienta SignalR JavaScript w `<script>` elementu.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="0f7ce-116">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="0f7ce-116">Connect to a hub</span></span>

<span data-ttu-id="0f7ce-117">Poniższy kod tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="0f7ce-118">Nazwa Centrum jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

### <a name="cross-origin-connections"></a><span data-ttu-id="0f7ce-119">Połączenia między źródłami</span><span class="sxs-lookup"><span data-stu-id="0f7ce-119">Cross-origin connections</span></span>

<span data-ttu-id="0f7ce-120">Zazwyczaj przeglądarek ładowanie połączeń z tej samej domenie co żądanej strony.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="0f7ce-121">Istnieją jednak sytuacje, gdy wymagane jest połączenie do innej domeny.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="0f7ce-122">Aby zapobiec złośliwych witryn odczytywanie poufnych danych z innej lokacji [połączeń cross-origin](xref:security/cors) są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="0f7ce-123">Aby zezwolić na żądania między źródłami, włącz go w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="0f7ce-124">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="0f7ce-124">Call hub methods from client</span></span>

<span data-ttu-id="0f7ce-125">Klientów JavaScript wywołania metod publicznych w centrach za pośrednictwem [wywołania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) metody [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="0f7ce-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="0f7ce-126">`invoke` Metoda przyjmuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="0f7ce-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="0f7ce-127">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-127">The name of the hub method.</span></span> <span data-ttu-id="0f7ce-128">W poniższym przykładzie jest nazwa metody koncentratora `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="0f7ce-129">Argumenty zdefiniowane w metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="0f7ce-130">W poniższym przykładzie jest nazwa argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-130">In the following example, the argument name is `message`.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="0f7ce-131">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="0f7ce-131">Call client methods from hub</span></span>

<span data-ttu-id="0f7ce-132">Aby odbierać komunikaty z Centrum, definiowanie metody przy użyciu [na](/javascript/api/%40aspnet/signalr/hubconnection#on) metody `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-132">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="0f7ce-133">Nazwa metody klienta JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="0f7ce-134">W poniższym przykładzie nazwa metody jest `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="0f7ce-135">Argumenty Centrum przekazuje do metody.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="0f7ce-136">W poniższym przykładzie wartość argumentu jest `message`.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="0f7ce-137">Powyższy kod w `connection.on` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metody.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-137">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="0f7ce-138">Określa SignalR, jakiej metody klienta do wywołania, dopasowując nazwy metody i argumenty zdefiniowane w `SendAsync` i `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="0f7ce-139">Najlepszym rozwiązaniem, należy wywołać [start](/javascript/api/%40aspnet/signalr/hubconnection#start) metody `HubConnection` po `on`.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-139">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="0f7ce-140">Daje to pewność, że inne programy obsługi są rejestrowane, zanim jakiekolwiek komunikaty są odbierane.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-140">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="0f7ce-141">Rejestrowanie i obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="0f7ce-141">Error handling and logging</span></span>

<span data-ttu-id="0f7ce-142">Łańcuch `catch` metody do końca `start` metodę, aby obsługiwać błędy po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-142">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="0f7ce-143">Użyj `console.error` błędy dane wyjściowe do konsoli w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-143">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=43-45)]

<span data-ttu-id="0f7ce-144">Konfiguracja po stronie klienta dziennika śledzenia, przekazując rejestratora i typu zdarzenia logowania, gdy połączenie zostało nawiązane.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-144">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="0f7ce-145">Komunikaty są rejestrowane, z poziomu określonego dziennika lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-145">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="0f7ce-146">Poziomy dziennika dostępne są następujące:</span><span class="sxs-lookup"><span data-stu-id="0f7ce-146">Available log levels are as follows:</span></span>

* <span data-ttu-id="0f7ce-147">`signalR.LogLevel.Error` &ndash; Komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-147">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="0f7ce-148">Dzienniki `Error` tylko komunikaty.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-148">Logs `Error` messages only.</span></span>
* <span data-ttu-id="0f7ce-149">`signalR.LogLevel.Warning` &ndash; Komunikaty ostrzegawcze o potencjalnych błędów.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-149">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="0f7ce-150">Dzienniki `Warning`, i `Error` wiadomości.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-150">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="0f7ce-151">`signalR.LogLevel.Information` &ndash; Komunikaty o stanie bez błędów.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-151">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="0f7ce-152">Dzienniki `Information`, `Warning`, i `Error` wiadomości.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-152">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="0f7ce-153">`signalR.LogLevel.Trace` &ndash; Komunikaty śledzenia.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-153">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="0f7ce-154">Rejestruje wszystkim, łącznie z danych przesyłanych między Centrum a klientem.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-154">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="0f7ce-155">Użyj [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metody [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) skonfigurować poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-155">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="0f7ce-156">Komunikaty są rejestrowane w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-156">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="0f7ce-157">Ponowne łączenie klientów</span><span class="sxs-lookup"><span data-stu-id="0f7ce-157">Reconnect clients</span></span>

<span data-ttu-id="0f7ce-158">Automatyczne nie ponowne połączenia klienta JavaScript dla elementu SignalR.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-158">The JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="0f7ce-159">Należy napisać kod, który zostanie nawiązana ponownie ręcznie klienta.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-159">You must write code that will reconnect your client manually.</span></span> <span data-ttu-id="0f7ce-160">Poniższy kod przedstawia metody typowego ponowne nawiązanie połączenia:</span><span class="sxs-lookup"><span data-stu-id="0f7ce-160">The following code demonstrates a typical reconnection approach:</span></span>

1. <span data-ttu-id="0f7ce-161">Funkcji (w tym przypadku `start` funkcji) jest tworzony, aby rozpocząć połączenie.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-161">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="0f7ce-162">Wywołaj `start` funkcji przez połączenie `onclose` programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-162">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="0f7ce-163">Implementacja rzeczywistych będzie używać wycofywanie wykładnicze lub spróbuj ponownie określoną liczbę razy, przed rezygnacją.</span><span class="sxs-lookup"><span data-stu-id="0f7ce-163">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0f7ce-164">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0f7ce-164">Additional resources</span></span>

* [<span data-ttu-id="0f7ce-165">Dokumentacja interfejsów API języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="0f7ce-165">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="0f7ce-166">Samouczek języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="0f7ce-166">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="0f7ce-167">Samouczek dotyczący WebPack i TypeScript</span><span class="sxs-lookup"><span data-stu-id="0f7ce-167">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="0f7ce-168">Centra</span><span class="sxs-lookup"><span data-stu-id="0f7ce-168">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="0f7ce-169">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="0f7ce-169">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="0f7ce-170">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="0f7ce-170">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="0f7ce-171">Żądań Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="0f7ce-171">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
