---
title: Obsługa protokółu Websocket w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/10/2019
uid: fundamentals/websockets
ms.openlocfilehash: bba9cf051deaf57efdd82ca2fb1318fce79bd6cc
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223222"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="5fcae-103">Obsługa protokółu Websocket w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5fcae-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="5fcae-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Andrew Stanton pielęgniarki](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="5fcae-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="5fcae-105">W tym artykule wyjaśniono, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5fcae-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="5fcae-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) to protokół, który umożliwia kanały komunikacja dwukierunkowa trwałego połączenia protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="5fcae-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="5fcae-107">Jest on używany w aplikacjach korzystających z komunikacji szybki, w czasie rzeczywistym, takich jak rozmowy, pulpit nawigacyjny i gier, aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5fcae-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="5fcae-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5fcae-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="5fcae-109">Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="5fcae-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5fcae-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5fcae-110">Prerequisites</span></span>

* <span data-ttu-id="5fcae-111">Platforma ASP.NET Core 1.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="5fcae-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="5fcae-112">Dowolny system operacyjny, który obsługuje platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5fcae-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="5fcae-113">Windows 7 / Windows Server 2008 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="5fcae-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="5fcae-114">Linux</span><span class="sxs-lookup"><span data-stu-id="5fcae-114">Linux</span></span>
  * <span data-ttu-id="5fcae-115">macOS</span><span class="sxs-lookup"><span data-stu-id="5fcae-115">macOS</span></span>
  
* <span data-ttu-id="5fcae-116">Jeśli aplikacja działa w systemie Windows za pomocą programu IIS:</span><span class="sxs-lookup"><span data-stu-id="5fcae-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="5fcae-117">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="5fcae-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="5fcae-118">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="5fcae-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="5fcae-119">Musi być włączona funkcja WebSockets (zobacz [obsługi usług IIS/IIS Express](#iisiis-express-support) sekcji.).</span><span class="sxs-lookup"><span data-stu-id="5fcae-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="5fcae-120">Jeśli aplikacja jest uruchamiana [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="5fcae-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="5fcae-121">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="5fcae-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="5fcae-122">W przypadku obsługiwanych przeglądarek, zobacz https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="5fcae-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="5fcae-123">Kiedy należy używać funkcji WebSockets</span><span class="sxs-lookup"><span data-stu-id="5fcae-123">When to use WebSockets</span></span>

<span data-ttu-id="5fcae-124">Do pracy bezpośrednio za pomocą połączenia gniazda, należy użyć funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="5fcae-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="5fcae-125">Aby uzyskać optymalną wydajność w czasie rzeczywistym gry, na przykład użyć funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="5fcae-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="5fcae-126">[Biblioteki SignalR platformy ASP.NET Core](xref:signalr/introduction) to biblioteka, która ułatwia dodawanie funkcji internetowych w czasie rzeczywistym do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5fcae-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="5fcae-127">Używa funkcji WebSockets, jeśli to możliwe.</span><span class="sxs-lookup"><span data-stu-id="5fcae-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="5fcae-128">Jak używać funkcji WebSockets</span><span class="sxs-lookup"><span data-stu-id="5fcae-128">How to use WebSockets</span></span>

* <span data-ttu-id="5fcae-129">Zainstaluj [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="5fcae-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="5fcae-130">Konfigurowanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="5fcae-130">Configure the middleware.</span></span>
* <span data-ttu-id="5fcae-131">Akceptują żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5fcae-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="5fcae-132">Wysyłanie i odbieranie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="5fcae-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="5fcae-133">Konfigurowanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="5fcae-133">Configure the middleware</span></span>

<span data-ttu-id="5fcae-134">Dodaj oprogramowanie pośredniczące Websocket w `Configure` metody `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="5fcae-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5fcae-135">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="5fcae-135">The following settings can be configured:</span></span>

* <span data-ttu-id="5fcae-136">`KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="5fcae-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="5fcae-137">Wartość domyślna to dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="5fcae-137">The default is two minutes.</span></span>
* <span data-ttu-id="5fcae-138">`ReceiveBufferSize` — Rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="5fcae-138">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="5fcae-139">Użytkownicy zaawansowani trzeba to zmienić dotyczące dostosowywania wydajności na podstawie rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="5fcae-139">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="5fcae-140">Wartość domyślna to 4 KB.</span><span class="sxs-lookup"><span data-stu-id="5fcae-140">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5fcae-141">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="5fcae-141">The following settings can be configured:</span></span>

* <span data-ttu-id="5fcae-142">`KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="5fcae-142">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="5fcae-143">Wartość domyślna to dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="5fcae-143">The default is two minutes.</span></span>
* <span data-ttu-id="5fcae-144">`ReceiveBufferSize` — Rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="5fcae-144">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="5fcae-145">Użytkownicy zaawansowani trzeba to zmienić dotyczące dostosowywania wydajności na podstawie rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="5fcae-145">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="5fcae-146">Wartość domyślna to 4 KB.</span><span class="sxs-lookup"><span data-stu-id="5fcae-146">The default is 4 KB.</span></span>
* <span data-ttu-id="5fcae-147">`AllowedOrigins` -Lista dozwolonych wartości nagłówka pochodzenia dla żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5fcae-147">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="5fcae-148">Domyślnie wszystkie źródła są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="5fcae-148">By default, all origins are allowed.</span></span> <span data-ttu-id="5fcae-149">Aby uzyskać szczegółowe informacje, zobacz "WebSocket pochodzenia ograniczenia" poniżej.</span><span class="sxs-lookup"><span data-stu-id="5fcae-149">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="5fcae-150">Akceptować żądania protokołu WebSocket</span><span class="sxs-lookup"><span data-stu-id="5fcae-150">Accept WebSocket requests</span></span>

<span data-ttu-id="5fcae-151">Gdzieś później w cyklu życia żądania (w dalszej części `Configure` metody lub Akcja kontrolera MVC, na przykład) sprawdź, czy jest on żądanie protokołu WebSocket i zaakceptuj żądanie protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5fcae-151">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="5fcae-152">Poniższy przykład znajduje się w dalszej części w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="5fcae-152">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="5fcae-153">Żądanie protokołu WebSocket może występować na dowolny adres URL, ale ten przykładowy kod akceptuje tylko żądania dotyczące `/ws`.</span><span class="sxs-lookup"><span data-stu-id="5fcae-153">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

<span data-ttu-id="5fcae-154">Korzystając z protokołu WebSocket, możesz **musi** zachować potoku oprogramowania pośredniczącego, uruchomione przez czas trwania połączenia.</span><span class="sxs-lookup"><span data-stu-id="5fcae-154">When using a WebSocket, you **must** keep the middleware pipeline running for the duration of the connection.</span></span> <span data-ttu-id="5fcae-155">Jeśli użytkownik podejmie próbę wysłania lub odebrania komunikatu protokołu WebSocket, po zakończeniu potoku oprogramowania pośredniczącego, może wystąpić wyjątek, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="5fcae-155">If you attempt to send or receive a WebSocket message after the middleware pipeline ends, you may get an exception like the following:</span></span>

```
System.Net.WebSockets.WebSocketException (0x80004005): The remote party closed the WebSocket connection without completing the close handshake. ---> System.ObjectDisposedException: Cannot write to the response body, the response has completed.
Object name: 'HttpResponseStream'.
```

<span data-ttu-id="5fcae-156">Jeśli używasz usługę w tle do zapisywania danych WebSocket, upewnij się, że nadal uruchomione potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="5fcae-156">If you're using a background service to write data to a WebSocket, make sure you keep the middleware pipeline running.</span></span> <span data-ttu-id="5fcae-157">To zrobić za pomocą <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span><span class="sxs-lookup"><span data-stu-id="5fcae-157">Do this by using a <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span></span> <span data-ttu-id="5fcae-158">Przekaż `TaskCompletionSource` do tła usługi i jego wywołania <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> po zakończeniu za pomocą protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5fcae-158">Pass the `TaskCompletionSource` to your background service and have it call <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> when you finish with the WebSocket.</span></span> <span data-ttu-id="5fcae-159">Następnie `await` <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> właściwości podczas żądania.</span><span class="sxs-lookup"><span data-stu-id="5fcae-159">Then `await` the <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> property during the request.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="5fcae-160">Wysyłanie i odbieranie komunikatów</span><span class="sxs-lookup"><span data-stu-id="5fcae-160">Send and receive messages</span></span>

<span data-ttu-id="5fcae-161">`AcceptWebSocketAsync` Metoda uaktualnia połączenie TCP z połączeniem WebSocket i zapewnia [WebSocket](/dotnet/core/api/system.net.websockets.websocket) obiektu.</span><span class="sxs-lookup"><span data-stu-id="5fcae-161">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="5fcae-162">Użyj `WebSocket` obiekt do wysyłania i odbierania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="5fcae-162">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="5fcae-163">Przekazuje kodu pokazanego wcześniej, która akceptuje żądanie protokołu WebSocket `WebSocket` obiekt `Echo` metody.</span><span class="sxs-lookup"><span data-stu-id="5fcae-163">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="5fcae-164">Kod odbiera komunikat i natychmiast wysyła z powrotem tę samą wiadomość.</span><span class="sxs-lookup"><span data-stu-id="5fcae-164">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="5fcae-165">Wiadomości wysyłanych i odbieranych w pętli, dopóki klient zamyka połączenie:</span><span class="sxs-lookup"><span data-stu-id="5fcae-165">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="5fcae-166">Akceptując połączeniem WebSocket, przed rozpoczęciem pętli, kończy się potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="5fcae-166">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="5fcae-167">Po zamknięciu gniazda, rozwija potoku.</span><span class="sxs-lookup"><span data-stu-id="5fcae-167">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="5fcae-168">Oznacza to, że żądanie zatrzymuje, przenoszenie do przodu w potoku po zaakceptowaniu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5fcae-168">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="5fcae-169">Po zakończeniu pętli i gniazda jest zamknięty, żądanie będzie kontynuowane wykonywanie kopii zapasowych potoku.</span><span class="sxs-lookup"><span data-stu-id="5fcae-169">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="handle-client-disconnects"></a><span data-ttu-id="5fcae-170">Rozłącza klienta uchwytu</span><span class="sxs-lookup"><span data-stu-id="5fcae-170">Handle client disconnects</span></span>

<span data-ttu-id="5fcae-171">Serwer nie jest automatycznie powiadomiony, gdy klient odłączy się z powodu utraty łączności.</span><span class="sxs-lookup"><span data-stu-id="5fcae-171">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="5fcae-172">Serwer odebrał komunikatu o rozłączeniu, tylko wtedy, gdy klient wysyła, którego nie można wykonać w przypadku utraty połączenia z Internetem.</span><span class="sxs-lookup"><span data-stu-id="5fcae-172">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="5fcae-173">Jeśli chcesz wykonania określonego działania, jeśli tak się stanie, należy ustawić limit czasu po nic nie zostanie odebrana od klienta w przedziale czasu.</span><span class="sxs-lookup"><span data-stu-id="5fcae-173">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="5fcae-174">Jeśli klient nie jest zawsze wysyłanie komunikatów i użytkownik nie chce limitu czasu, po prostu, ponieważ połączenie przechodzi bezczynności, należy mieć klient używał czasomierza, aby wysłać komunikat ping co X sekund.</span><span class="sxs-lookup"><span data-stu-id="5fcae-174">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="5fcae-175">Na serwerze, jeśli wiadomość nie dotarła w ciągu 2\*X (w sekundach) po poprzedniej, Zakończ połączenie i raport, który klient odłączony.</span><span class="sxs-lookup"><span data-stu-id="5fcae-175">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="5fcae-176">Poczekaj, aż dwa razy przewidywanym czasie pozostawić dodatkowy czas opóźnienia sieci, które mogą blokować komunikat ping.</span><span class="sxs-lookup"><span data-stu-id="5fcae-176">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="5fcae-177">Ograniczenie pochodzenia WebSocket</span><span class="sxs-lookup"><span data-stu-id="5fcae-177">WebSocket origin restriction</span></span>

<span data-ttu-id="5fcae-178">Zabezpieczenia udostępniane przez mechanizm CORS nie dotyczą funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="5fcae-178">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="5fcae-179">Czy przeglądarek **nie**:</span><span class="sxs-lookup"><span data-stu-id="5fcae-179">Browsers do **not**:</span></span>

* <span data-ttu-id="5fcae-180">Wykonywanie żądań krótkiej mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="5fcae-180">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="5fcae-181">Przestrzeganie ograniczenia określone w `Access-Control` nagłówków w przypadku wysyłania żądań protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5fcae-181">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="5fcae-182">Jednak wysłać przeglądarek `Origin` nagłówka podczas wystawiania żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5fcae-182">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="5fcae-183">Aplikacje powinny być konfigurowane do sprawdzania poprawności tych nagłówków, aby upewnić się, że dozwolone są tylko WebSockets pochodzące z oczekiwanym źródła.</span><span class="sxs-lookup"><span data-stu-id="5fcae-183">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="5fcae-184">Jeśli przechowujesz serwera na "https://server.com"i hosting w systemie klienta"https://client.com", Dodaj "https://client.com" Aby `AllowedOrigins` listy dla funkcji WebSockets sprawdzić.</span><span class="sxs-lookup"><span data-stu-id="5fcae-184">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="5fcae-185">`Origin` Nagłówek jest kontrolowany przez klienta i, podobnie jak `Referer` nagłówka, mogą zostać sfałszowane.</span><span class="sxs-lookup"><span data-stu-id="5fcae-185">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="5fcae-186">Czy **nie** Użyj tych nagłówków jako mechanizm uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="5fcae-186">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="5fcae-187">Obsługa usług IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="5fcae-187">IIS/IIS Express support</span></span>

<span data-ttu-id="5fcae-188">Windows Server 2012 lub nowszym i Windows 8 lub nowszym z usług IIS/IIS Express 8 lub nowszy jest obsługa protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5fcae-188">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="5fcae-189">Gniazda Websocket są zawsze włączone, korzystając z usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="5fcae-189">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="5fcae-190">Włączanie funkcji WebSockets w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="5fcae-190">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="5fcae-191">Aby włączyć obsługę protokołu WebSocket w systemie Windows Server 2012 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="5fcae-191">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="5fcae-192">Te kroki nie są wymagane w przypadku korzystania z usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="5fcae-192">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="5fcae-193">Użyj **Dodaj role i funkcje** kreatora z **Zarządzaj** menu lub linku w **Menedżera serwera**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-193">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="5fcae-194">Wybierz **Instalacja oparta na rolach lub oparta na funkcjach**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-194">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="5fcae-195">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-195">Select **Next**.</span></span>
1. <span data-ttu-id="5fcae-196">Wybierz odpowiedni serwer (serwer lokalny jest wybrane domyślnie).</span><span class="sxs-lookup"><span data-stu-id="5fcae-196">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="5fcae-197">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-197">Select **Next**.</span></span>
1. <span data-ttu-id="5fcae-198">Rozwiń **serwer sieci Web (IIS)** w **role** drzewa, a następnie rozwiń **serwera sieci Web**, a następnie rozwiń węzeł **opracowywanie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-198">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="5fcae-199">Wybierz **protokołu WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-199">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="5fcae-200">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-200">Select **Next**.</span></span>
1. <span data-ttu-id="5fcae-201">Jeśli nie są wymagane dodatkowe funkcje, wybierz opcję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-201">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="5fcae-202">Wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-202">Select **Install**.</span></span>
1. <span data-ttu-id="5fcae-203">Po zakończeniu instalacji wybierz **Zamknij** aby zakończyć pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="5fcae-203">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="5fcae-204">Aby włączyć obsługę protokołu WebSocket w systemie Windows 8 lub nowszy:</span><span class="sxs-lookup"><span data-stu-id="5fcae-204">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="5fcae-205">Te kroki nie są wymagane w przypadku korzystania z usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="5fcae-205">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="5fcae-206">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="5fcae-206">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="5fcae-207">Otwórz następujące węzły: **Internetowe usługi informacyjne** > **usługi World Wide Web** > **funkcje tworzenia aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-207">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="5fcae-208">Wybierz **protokołu WebSocket** funkcji.</span><span class="sxs-lookup"><span data-stu-id="5fcae-208">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="5fcae-209">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-209">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="5fcae-210">Wyłącz WebSocket, gdy na języku Node.js przy użyciu biblioteki socket.io</span><span class="sxs-lookup"><span data-stu-id="5fcae-210">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="5fcae-211">Jeśli dzięki obsłudze protokołu WebSocket w [biblioteki socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), Wyłącz domyślne IIS WebSocket modułu przy użyciu `webSocket` element *web.config* lub *applicationHost.config*. Jeśli ten krok nie jest wykonywana, Moduł IIS WebSocket próbuje obsługi komunikacji protokołu WebSocket, a nie środowiska Node.js i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5fcae-211">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="5fcae-212">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="5fcae-212">Next steps</span></span>

<span data-ttu-id="5fcae-213">[Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) którego zostanie dołączony, ten artykuł stanowi app echa.</span><span class="sxs-lookup"><span data-stu-id="5fcae-213">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="5fcae-214">Posiada strony sieci web, która sprawia, że połączeń protokołu WebSocket i wszelkich komunikatów odebranych wysyła ponownie serwer do klienta.</span><span class="sxs-lookup"><span data-stu-id="5fcae-214">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="5fcae-215">Uruchamianie aplikacji z poziomu wiersza polecenia (go nie skonfigurowała do uruchamiania w programie Visual Studio z programem IIS Express) i przejdź do http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="5fcae-215">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="5fcae-216">Strony sieci web pokazuje stan połączenia w lewym górnym rogu:</span><span class="sxs-lookup"><span data-stu-id="5fcae-216">The web page shows the connection status in the upper left:</span></span>

![Początkowy stan strony sieci web](websockets/_static/start.png)

<span data-ttu-id="5fcae-218">Wybierz **Connect** wysyłać żądanie protokołu WebSocket adres URL wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="5fcae-218">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="5fcae-219">Wprowadź wiadomość testową, a następnie wybierz pozycję **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-219">Enter a test message and select **Send**.</span></span> <span data-ttu-id="5fcae-220">Po zakończeniu wybierz pozycję **Zamknij gniazda**.</span><span class="sxs-lookup"><span data-stu-id="5fcae-220">When done, select **Close Socket**.</span></span> <span data-ttu-id="5fcae-221">**Dziennika komunikacji** sekcji raporty każdy otwarty, wysyłanie i zamknij akcji, jak to się dzieje.</span><span class="sxs-lookup"><span data-stu-id="5fcae-221">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Początkowy stan strony sieci web](websockets/_static/end.png)
