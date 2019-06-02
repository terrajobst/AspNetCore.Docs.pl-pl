---
title: Obsługa protokółu Websocket w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/10/2019
uid: fundamentals/websockets
ms.openlocfilehash: 4c49a5349c0718e5c59f30e6d51caf7a43fa0454
ms.sourcegitcommit: c5339594101d30b189f61761275b7d310e80d18a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2019
ms.locfileid: "66458449"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="3908f-103">Obsługa protokółu Websocket w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3908f-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="3908f-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Andrew Stanton pielęgniarki](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="3908f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="3908f-105">W tym artykule wyjaśniono, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3908f-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="3908f-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) to protokół, który umożliwia kanały komunikacja dwukierunkowa trwałego połączenia protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="3908f-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="3908f-107">Jest on używany w aplikacjach korzystających z komunikacji szybki, w czasie rzeczywistym, takich jak rozmowy, pulpit nawigacyjny i gier, aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3908f-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="3908f-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="3908f-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="3908f-109">[Jak uruchomić](#sample-app).</span><span class="sxs-lookup"><span data-stu-id="3908f-109">[How to run](#sample-app).</span></span>

## <a name="signalr"></a><span data-ttu-id="3908f-110">SignalR</span><span class="sxs-lookup"><span data-stu-id="3908f-110">SignalR</span></span>

<span data-ttu-id="3908f-111">[Biblioteki SignalR platformy ASP.NET Core](xref:signalr/introduction) to biblioteka, która ułatwia dodawanie funkcji internetowych w czasie rzeczywistym do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3908f-111">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="3908f-112">Używa funkcji WebSockets, jeśli to możliwe.</span><span class="sxs-lookup"><span data-stu-id="3908f-112">It uses WebSockets whenever possible.</span></span>

<span data-ttu-id="3908f-113">W przypadku większości aplikacji zalecamy SignalR przez protokół WebSockets raw.</span><span class="sxs-lookup"><span data-stu-id="3908f-113">For most applications, we recommend SignalR over raw WebSockets.</span></span> <span data-ttu-id="3908f-114">Biblioteka SignalR udostępnia transport rezerwowej w środowiskach, gdzie funkcja WebSockets jest niedostępna.</span><span class="sxs-lookup"><span data-stu-id="3908f-114">SignalR provides transport fallback for environments where WebSockets is not available.</span></span> <span data-ttu-id="3908f-115">Umożliwia także modelu aplikacji wywołanie procedury zdalnej proste.</span><span class="sxs-lookup"><span data-stu-id="3908f-115">It also provides a simple remote procedure call app model.</span></span> <span data-ttu-id="3908f-116">I w większości przypadków SignalR nie wadą istotnie poprawiającą wydajność w porównaniu do używa funkcji WebSockets raw.</span><span class="sxs-lookup"><span data-stu-id="3908f-116">And in most scenarios, SignalR has no significant performance disadvantage compared to using raw WebSockets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3908f-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3908f-117">Prerequisites</span></span>

* <span data-ttu-id="3908f-118">Platforma ASP.NET Core 1.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="3908f-118">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="3908f-119">Dowolny system operacyjny, który obsługuje platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3908f-119">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="3908f-120">Windows 7 / Windows Server 2008 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="3908f-120">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="3908f-121">Linux</span><span class="sxs-lookup"><span data-stu-id="3908f-121">Linux</span></span>
  * <span data-ttu-id="3908f-122">macOS</span><span class="sxs-lookup"><span data-stu-id="3908f-122">macOS</span></span>
  
* <span data-ttu-id="3908f-123">Jeśli aplikacja działa w systemie Windows za pomocą programu IIS:</span><span class="sxs-lookup"><span data-stu-id="3908f-123">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="3908f-124">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="3908f-124">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="3908f-125">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="3908f-125">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="3908f-126">Musi być włączona funkcja WebSockets (zobacz [obsługi usług IIS/IIS Express](#iisiis-express-support) sekcji.).</span><span class="sxs-lookup"><span data-stu-id="3908f-126">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="3908f-127">Jeśli aplikacja jest uruchamiana [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="3908f-127">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="3908f-128">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="3908f-128">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="3908f-129">W przypadku obsługiwanych przeglądarek, zobacz https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="3908f-129">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

::: moniker range="< aspnetcore-2.1"

## <a name="nuget-package"></a><span data-ttu-id="3908f-130">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="3908f-130">NuGet package</span></span>

<span data-ttu-id="3908f-131">Zainstaluj [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="3908f-131">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>

::: moniker-end

## <a name="configure-the-middleware"></a><span data-ttu-id="3908f-132">Konfigurowanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="3908f-132">Configure the middleware</span></span>


<span data-ttu-id="3908f-133">Dodaj oprogramowanie pośredniczące Websocket w `Configure` metody `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="3908f-133">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="3908f-134">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="3908f-134">The following settings can be configured:</span></span>

* <span data-ttu-id="3908f-135">`KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="3908f-135">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="3908f-136">Wartość domyślna to dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="3908f-136">The default is two minutes.</span></span>
* <span data-ttu-id="3908f-137">`ReceiveBufferSize` — Rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="3908f-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="3908f-138">Użytkownicy zaawansowani trzeba to zmienić dotyczące dostosowywania wydajności na podstawie rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="3908f-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="3908f-139">Wartość domyślna to 4 KB.</span><span class="sxs-lookup"><span data-stu-id="3908f-139">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3908f-140">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="3908f-140">The following settings can be configured:</span></span>

* <span data-ttu-id="3908f-141">`KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="3908f-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="3908f-142">Wartość domyślna to dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="3908f-142">The default is two minutes.</span></span>
* <span data-ttu-id="3908f-143">`ReceiveBufferSize` — Rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="3908f-143">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="3908f-144">Użytkownicy zaawansowani trzeba to zmienić dotyczące dostosowywania wydajności na podstawie rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="3908f-144">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="3908f-145">Wartość domyślna to 4 KB.</span><span class="sxs-lookup"><span data-stu-id="3908f-145">The default is 4 KB.</span></span>
* <span data-ttu-id="3908f-146">`AllowedOrigins` -Lista dozwolonych wartości nagłówka pochodzenia dla żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3908f-146">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="3908f-147">Domyślnie wszystkie źródła są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="3908f-147">By default, all origins are allowed.</span></span> <span data-ttu-id="3908f-148">Aby uzyskać szczegółowe informacje, zobacz "WebSocket pochodzenia ograniczenia" poniżej.</span><span class="sxs-lookup"><span data-stu-id="3908f-148">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

## <a name="accept-websocket-requests"></a><span data-ttu-id="3908f-149">Akceptować żądania protokołu WebSocket</span><span class="sxs-lookup"><span data-stu-id="3908f-149">Accept WebSocket requests</span></span>

<span data-ttu-id="3908f-150">Gdzieś później w cyklu życia żądania (w dalszej części `Configure` metodę lub metody akcji, na przykład) sprawdź, czy jest on żądanie protokołu WebSocket i zaakceptuj żądanie protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3908f-150">Somewhere later in the request life cycle (later in the `Configure` method or in an action method, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="3908f-151">Poniższy przykład znajduje się w dalszej części w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="3908f-151">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="3908f-152">Żądanie protokołu WebSocket może występować na dowolny adres URL, ale ten przykładowy kod akceptuje tylko żądania dotyczące `/ws`.</span><span class="sxs-lookup"><span data-stu-id="3908f-152">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

<span data-ttu-id="3908f-153">Korzystając z protokołu WebSocket, możesz **musi** zachować potoku oprogramowania pośredniczącego, uruchomione przez czas trwania połączenia.</span><span class="sxs-lookup"><span data-stu-id="3908f-153">When using a WebSocket, you **must** keep the middleware pipeline running for the duration of the connection.</span></span> <span data-ttu-id="3908f-154">Jeśli użytkownik podejmie próbę wysłania lub odebrania komunikatu protokołu WebSocket, po zakończeniu potoku oprogramowania pośredniczącego, może wystąpić wyjątek, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="3908f-154">If you attempt to send or receive a WebSocket message after the middleware pipeline ends, you may get an exception like the following:</span></span>

```
System.Net.WebSockets.WebSocketException (0x80004005): The remote party closed the WebSocket connection without completing the close handshake. ---> System.ObjectDisposedException: Cannot write to the response body, the response has completed.
Object name: 'HttpResponseStream'.
```

<span data-ttu-id="3908f-155">Jeśli używasz usługę w tle do zapisywania danych WebSocket, upewnij się, że nadal uruchomione potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="3908f-155">If you're using a background service to write data to a WebSocket, make sure you keep the middleware pipeline running.</span></span> <span data-ttu-id="3908f-156">To zrobić za pomocą <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span><span class="sxs-lookup"><span data-stu-id="3908f-156">Do this by using a <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span></span> <span data-ttu-id="3908f-157">Przekaż `TaskCompletionSource` do tła usługi i jego wywołania <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> po zakończeniu za pomocą protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3908f-157">Pass the `TaskCompletionSource` to your background service and have it call <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> when you finish with the WebSocket.</span></span> <span data-ttu-id="3908f-158">Następnie `await` <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> właściwości podczas wykonywania żądania, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="3908f-158">Then `await` the <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> property during the request, as shown in the following example:</span></span>

```csharp
app.Use(async (context, next) => {
    var socket = await context.WebSockets.AcceptWebSocketAsync();
    var socketFinishedTcs = new TaskCompletionSource<object>();

    BackgroundSocketProcessor.AddSocket(socket, socketFinishedTcs); 

    await socketFinishedTcs.Task;
});
```
<span data-ttu-id="3908f-159">Wyjątek protokołu WebSocket zamknięte również może nastąpić, jeśli wrócisz zbyt szybko z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="3908f-159">The WebSocket closed exception can also happen if you return too soon from an action method.</span></span> <span data-ttu-id="3908f-160">Jeśli zdecydujesz się zaakceptować gniazda w metodzie akcji, poczekaj, aż kod, który używa gniazda, które należy wykonać przed powrotem z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="3908f-160">If you accept a socket in an action method, wait for the code that uses the socket to complete before returning from the action method.</span></span>

<span data-ttu-id="3908f-161">Nigdy nie używaj `Task.Wait()`, `Task.Result`, lub podobne wywołania blokowania oczekiwać dla gniazda ukończyć, i co może powodować poważne problemy wielowątkowości.</span><span class="sxs-lookup"><span data-stu-id="3908f-161">Never use `Task.Wait()`, `Task.Result`, or similar blocking calls to wait for the socket to complete, as that can cause serious threading issues.</span></span> <span data-ttu-id="3908f-162">Zawsze używaj `await`.</span><span class="sxs-lookup"><span data-stu-id="3908f-162">Always use `await`.</span></span>

## <a name="send-and-receive-messages"></a><span data-ttu-id="3908f-163">Wysyłanie i odbieranie komunikatów</span><span class="sxs-lookup"><span data-stu-id="3908f-163">Send and receive messages</span></span>

<span data-ttu-id="3908f-164">`AcceptWebSocketAsync` Metoda uaktualnia połączenie TCP z połączeniem WebSocket i zapewnia [WebSocket](/dotnet/core/api/system.net.websockets.websocket) obiektu.</span><span class="sxs-lookup"><span data-stu-id="3908f-164">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="3908f-165">Użyj `WebSocket` obiekt do wysyłania i odbierania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="3908f-165">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="3908f-166">Przekazuje kodu pokazanego wcześniej, która akceptuje żądanie protokołu WebSocket `WebSocket` obiekt `Echo` metody.</span><span class="sxs-lookup"><span data-stu-id="3908f-166">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="3908f-167">Kod odbiera komunikat i natychmiast wysyła z powrotem tę samą wiadomość.</span><span class="sxs-lookup"><span data-stu-id="3908f-167">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="3908f-168">Wiadomości wysyłanych i odbieranych w pętli, dopóki klient zamyka połączenie:</span><span class="sxs-lookup"><span data-stu-id="3908f-168">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

<span data-ttu-id="3908f-169">Akceptując połączeniem WebSocket, przed rozpoczęciem pętli, kończy się potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="3908f-169">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="3908f-170">Po zamknięciu gniazda, rozwija potoku.</span><span class="sxs-lookup"><span data-stu-id="3908f-170">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="3908f-171">Oznacza to, że żądanie zatrzymuje, przenoszenie do przodu w potoku po zaakceptowaniu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3908f-171">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="3908f-172">Po zakończeniu pętli i gniazda jest zamknięty, żądanie będzie kontynuowane wykonywanie kopii zapasowych potoku.</span><span class="sxs-lookup"><span data-stu-id="3908f-172">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="handle-client-disconnects"></a><span data-ttu-id="3908f-173">Rozłącza klienta uchwytu</span><span class="sxs-lookup"><span data-stu-id="3908f-173">Handle client disconnects</span></span>

<span data-ttu-id="3908f-174">Serwer nie jest automatycznie powiadomiony, gdy klient odłączy się z powodu utraty łączności.</span><span class="sxs-lookup"><span data-stu-id="3908f-174">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="3908f-175">Serwer odebrał komunikatu o rozłączeniu, tylko wtedy, gdy klient wysyła, którego nie można wykonać w przypadku utraty połączenia z Internetem.</span><span class="sxs-lookup"><span data-stu-id="3908f-175">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="3908f-176">Jeśli chcesz wykonania określonego działania, jeśli tak się stanie, należy ustawić limit czasu po nic nie zostanie odebrana od klienta w przedziale czasu.</span><span class="sxs-lookup"><span data-stu-id="3908f-176">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="3908f-177">Jeśli klient nie jest zawsze wysyłanie komunikatów i użytkownik nie chce limitu czasu, po prostu, ponieważ połączenie przechodzi bezczynności, należy mieć klient używał czasomierza, aby wysłać komunikat ping co X sekund.</span><span class="sxs-lookup"><span data-stu-id="3908f-177">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="3908f-178">Na serwerze, jeśli wiadomość nie dotarła w ciągu 2\*X (w sekundach) po poprzedniej, Zakończ połączenie i raport, który klient odłączony.</span><span class="sxs-lookup"><span data-stu-id="3908f-178">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="3908f-179">Poczekaj, aż dwa razy przewidywanym czasie pozostawić dodatkowy czas opóźnienia sieci, które mogą blokować komunikat ping.</span><span class="sxs-lookup"><span data-stu-id="3908f-179">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="3908f-180">Ograniczenie pochodzenia WebSocket</span><span class="sxs-lookup"><span data-stu-id="3908f-180">WebSocket origin restriction</span></span>

<span data-ttu-id="3908f-181">Zabezpieczenia udostępniane przez mechanizm CORS nie dotyczą funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3908f-181">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="3908f-182">Czy przeglądarek **nie**:</span><span class="sxs-lookup"><span data-stu-id="3908f-182">Browsers do **not**:</span></span>

* <span data-ttu-id="3908f-183">Wykonywanie żądań krótkiej mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="3908f-183">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="3908f-184">Przestrzeganie ograniczenia określone w `Access-Control` nagłówków w przypadku wysyłania żądań protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3908f-184">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="3908f-185">Jednak wysłać przeglądarek `Origin` nagłówka podczas wystawiania żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3908f-185">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="3908f-186">Aplikacje powinny być konfigurowane do sprawdzania poprawności tych nagłówków, aby upewnić się, że dozwolone są tylko WebSockets pochodzące z oczekiwanym źródła.</span><span class="sxs-lookup"><span data-stu-id="3908f-186">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="3908f-187">Jeśli przechowujesz serwera na "https://server.com"i hosting w systemie klienta"https://client.com", Dodaj "https://client.com" Aby `AllowedOrigins` listy dla funkcji WebSockets sprawdzić.</span><span class="sxs-lookup"><span data-stu-id="3908f-187">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="3908f-188">`Origin` Nagłówek jest kontrolowany przez klienta i, podobnie jak `Referer` nagłówka, mogą zostać sfałszowane.</span><span class="sxs-lookup"><span data-stu-id="3908f-188">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="3908f-189">Czy **nie** Użyj tych nagłówków jako mechanizm uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="3908f-189">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="3908f-190">Obsługa usług IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="3908f-190">IIS/IIS Express support</span></span>

<span data-ttu-id="3908f-191">Windows Server 2012 lub nowszym i Windows 8 lub nowszym z usług IIS/IIS Express 8 lub nowszy jest obsługa protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3908f-191">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="3908f-192">Gniazda Websocket są zawsze włączone, korzystając z usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="3908f-192">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="3908f-193">Włączanie funkcji WebSockets w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="3908f-193">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="3908f-194">Aby włączyć obsługę protokołu WebSocket w systemie Windows Server 2012 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="3908f-194">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="3908f-195">Te kroki nie są wymagane w przypadku korzystania z usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="3908f-195">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="3908f-196">Użyj **Dodaj role i funkcje** kreatora z **Zarządzaj** menu lub linku w **Menedżera serwera**.</span><span class="sxs-lookup"><span data-stu-id="3908f-196">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="3908f-197">Wybierz **Instalacja oparta na rolach lub oparta na funkcjach**.</span><span class="sxs-lookup"><span data-stu-id="3908f-197">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="3908f-198">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="3908f-198">Select **Next**.</span></span>
1. <span data-ttu-id="3908f-199">Wybierz odpowiedni serwer (serwer lokalny jest wybrane domyślnie).</span><span class="sxs-lookup"><span data-stu-id="3908f-199">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="3908f-200">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="3908f-200">Select **Next**.</span></span>
1. <span data-ttu-id="3908f-201">Rozwiń **serwer sieci Web (IIS)** w **role** drzewa, a następnie rozwiń **serwera sieci Web**, a następnie rozwiń węzeł **opracowywanie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="3908f-201">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="3908f-202">Wybierz **protokołu WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="3908f-202">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="3908f-203">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="3908f-203">Select **Next**.</span></span>
1. <span data-ttu-id="3908f-204">Jeśli nie są wymagane dodatkowe funkcje, wybierz opcję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="3908f-204">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="3908f-205">Wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="3908f-205">Select **Install**.</span></span>
1. <span data-ttu-id="3908f-206">Po zakończeniu instalacji wybierz **Zamknij** aby zakończyć pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="3908f-206">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="3908f-207">Aby włączyć obsługę protokołu WebSocket w systemie Windows 8 lub nowszy:</span><span class="sxs-lookup"><span data-stu-id="3908f-207">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="3908f-208">Te kroki nie są wymagane w przypadku korzystania z usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="3908f-208">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="3908f-209">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="3908f-209">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="3908f-210">Otwórz następujące węzły: **Internetowe usługi informacyjne** > **usługi World Wide Web** > **funkcje tworzenia aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="3908f-210">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="3908f-211">Wybierz **protokołu WebSocket** funkcji.</span><span class="sxs-lookup"><span data-stu-id="3908f-211">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="3908f-212">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3908f-212">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="3908f-213">Wyłącz WebSocket, gdy na języku Node.js przy użyciu biblioteki socket.io</span><span class="sxs-lookup"><span data-stu-id="3908f-213">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="3908f-214">Jeśli dzięki obsłudze protokołu WebSocket w [biblioteki socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), Wyłącz domyślne IIS WebSocket modułu przy użyciu `webSocket` element *web.config* lub *applicationHost.config*. Jeśli ten krok nie jest wykonywana, Moduł IIS WebSocket próbuje obsługi komunikacji protokołu WebSocket, a nie środowiska Node.js i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3908f-214">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="sample-app"></a><span data-ttu-id="3908f-215">Przykładowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="3908f-215">Sample app</span></span>

<span data-ttu-id="3908f-216">[Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) którego zostanie dołączony, ten artykuł stanowi app echa.</span><span class="sxs-lookup"><span data-stu-id="3908f-216">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="3908f-217">Posiada strony sieci web, która sprawia, że połączeń protokołu WebSocket i wszelkich komunikatów odebranych wysyła ponownie serwer do klienta.</span><span class="sxs-lookup"><span data-stu-id="3908f-217">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="3908f-218">Uruchamianie aplikacji z poziomu wiersza polecenia (go nie skonfigurowała do uruchamiania w programie Visual Studio z programem IIS Express) i przejdź do http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="3908f-218">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="3908f-219">Strony sieci web pokazuje stan połączenia w lewym górnym rogu:</span><span class="sxs-lookup"><span data-stu-id="3908f-219">The web page shows the connection status in the upper left:</span></span>

![Początkowy stan strony sieci web](websockets/_static/start.png)

<span data-ttu-id="3908f-221">Wybierz **Connect** wysyłać żądanie protokołu WebSocket adres URL wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="3908f-221">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="3908f-222">Wprowadź wiadomość testową, a następnie wybierz pozycję **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="3908f-222">Enter a test message and select **Send**.</span></span> <span data-ttu-id="3908f-223">Po zakończeniu wybierz pozycję **Zamknij gniazda**.</span><span class="sxs-lookup"><span data-stu-id="3908f-223">When done, select **Close Socket**.</span></span> <span data-ttu-id="3908f-224">**Dziennika komunikacji** sekcji raporty każdy otwarty, wysyłanie i zamknij akcji, jak to się dzieje.</span><span class="sxs-lookup"><span data-stu-id="3908f-224">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Początkowy stan strony sieci web](websockets/_static/end.png)

