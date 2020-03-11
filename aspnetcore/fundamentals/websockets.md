---
title: Obsługa obiektów WebSockets w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z usługą WebSockets w ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: fundamentals/websockets
ms.openlocfilehash: a8040003374906fd93e12c9fde44c4a5ccc2cf37
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655786"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="18ee0-103">Obsługa obiektów WebSockets w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="18ee0-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="18ee0-104">Autorzy [Dykstra](https://github.com/tdykstra) i [Andrew Stanton-pielęgniarki](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="18ee0-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="18ee0-105">W tym artykule wyjaśniono, jak rozpocząć pracę z usługą WebSockets w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="18ee0-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="18ee0-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) to protokół, który umożliwia komunikację dwukierunkową trwałych kanałów komunikacji za pośrednictwem połączeń TCP.</span><span class="sxs-lookup"><span data-stu-id="18ee0-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="18ee0-107">Jest on używany w aplikacjach, które korzystają z szybkiej komunikacji w czasie rzeczywistym, takiej jak czat, pulpit nawigacyjny i aplikacje do gier.</span><span class="sxs-lookup"><span data-stu-id="18ee0-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="18ee0-108">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="18ee0-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="18ee0-109">[Jak uruchomić](#sample-app).</span><span class="sxs-lookup"><span data-stu-id="18ee0-109">[How to run](#sample-app).</span></span>

## <a name="signalr"></a><span data-ttu-id="18ee0-110">SignalR</span><span class="sxs-lookup"><span data-stu-id="18ee0-110">SignalR</span></span>

<span data-ttu-id="18ee0-111">[ASP.NET Core sygnalizujący](xref:signalr/introduction) to biblioteka, która upraszcza Dodawanie funkcji sieci Web w czasie rzeczywistym do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="18ee0-111">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="18ee0-112">W miarę możliwości używa obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="18ee0-112">It uses WebSockets whenever possible.</span></span>

<span data-ttu-id="18ee0-113">W przypadku większości aplikacji zaleca się sygnalizowanie za pośrednictwem nieprzetworzonych gniazd WebSockets.</span><span class="sxs-lookup"><span data-stu-id="18ee0-113">For most applications, we recommend SignalR over raw WebSockets.</span></span> <span data-ttu-id="18ee0-114">Sygnalizujący zapewnia rezerwę transportową dla środowisk, w których usługi WebSockets są niedostępne.</span><span class="sxs-lookup"><span data-stu-id="18ee0-114">SignalR provides transport fallback for environments where WebSockets is not available.</span></span> <span data-ttu-id="18ee0-115">Udostępnia także prosty, zdalny model aplikacji.</span><span class="sxs-lookup"><span data-stu-id="18ee0-115">It also provides a simple remote procedure call app model.</span></span> <span data-ttu-id="18ee0-116">W większości scenariuszy sygnalizujący nie ma znaczącej niekorzystnej wydajności w porównaniu z korzystaniem z nieprzetworzonych gniazd WebSockets.</span><span class="sxs-lookup"><span data-stu-id="18ee0-116">And in most scenarios, SignalR has no significant performance disadvantage compared to using raw WebSockets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18ee0-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="18ee0-117">Prerequisites</span></span>

* <span data-ttu-id="18ee0-118">ASP.NET Core 1,1 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="18ee0-118">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="18ee0-119">Dowolny system operacyjny, który obsługuje ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="18ee0-119">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="18ee0-120">Windows 7/Windows Server 2008 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="18ee0-120">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="18ee0-121">Linux</span><span class="sxs-lookup"><span data-stu-id="18ee0-121">Linux</span></span>
  * <span data-ttu-id="18ee0-122">macOS</span><span class="sxs-lookup"><span data-stu-id="18ee0-122">macOS</span></span>
  
* <span data-ttu-id="18ee0-123">Jeśli aplikacja działa w systemie Windows z usługami IIS:</span><span class="sxs-lookup"><span data-stu-id="18ee0-123">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="18ee0-124">Windows 8/Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="18ee0-124">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="18ee0-125">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="18ee0-125">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="18ee0-126">Należy włączyć obiekty WebSockets (zobacz sekcję [Obsługa usług IIS/IIS Express](#iisiis-express-support) .).</span><span class="sxs-lookup"><span data-stu-id="18ee0-126">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="18ee0-127">Jeśli aplikacja jest uruchamiana na serwerze [http. sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="18ee0-127">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="18ee0-128">Windows 8/Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="18ee0-128">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="18ee0-129">W przypadku obsługiwanych przeglądarek Zobacz https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="18ee0-129">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

::: moniker range="< aspnetcore-2.1"

## <a name="nuget-package"></a><span data-ttu-id="18ee0-130">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="18ee0-130">NuGet package</span></span>

<span data-ttu-id="18ee0-131">Zainstaluj pakiet [Microsoft. AspNetCore. WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) .</span><span class="sxs-lookup"><span data-stu-id="18ee0-131">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>

::: moniker-end

## <a name="configure-the-middleware"></a><span data-ttu-id="18ee0-132">Konfigurowanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="18ee0-132">Configure the middleware</span></span>


<span data-ttu-id="18ee0-133">Dodaj oprogramowanie pośredniczące WebSockets w metodzie `Configure` klasy `Startup`:</span><span class="sxs-lookup"><span data-stu-id="18ee0-133">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="18ee0-134">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="18ee0-134">The following settings can be configured:</span></span>

* <span data-ttu-id="18ee0-135">`KeepAliveInterval` — jak często wysyłać ramki "ping" do klienta, aby upewnić się, że serwer proxy utrzymuje otwarte połączenie.</span><span class="sxs-lookup"><span data-stu-id="18ee0-135">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="18ee0-136">Wartość domyślna to dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="18ee0-136">The default is two minutes.</span></span>
* <span data-ttu-id="18ee0-137">`ReceiveBufferSize` — rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="18ee0-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="18ee0-138">Użytkownicy zaawansowani mogą wymagać zmiany wydajności na podstawie rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="18ee0-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="18ee0-139">Wartość domyślna to 4 KB.</span><span class="sxs-lookup"><span data-stu-id="18ee0-139">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="18ee0-140">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="18ee0-140">The following settings can be configured:</span></span>

* <span data-ttu-id="18ee0-141">`KeepAliveInterval` — jak często wysyłać ramki "ping" do klienta, aby upewnić się, że serwer proxy utrzymuje otwarte połączenie.</span><span class="sxs-lookup"><span data-stu-id="18ee0-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="18ee0-142">Wartość domyślna to dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="18ee0-142">The default is two minutes.</span></span>
* <span data-ttu-id="18ee0-143"><xref:Microsoft.AspNetCore.Builder.WebSocketOptions.ReceiveBufferSize> — rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="18ee0-143"><xref:Microsoft.AspNetCore.Builder.WebSocketOptions.ReceiveBufferSize> - The size of the buffer used to receive data.</span></span> <span data-ttu-id="18ee0-144">Użytkownicy zaawansowani mogą wymagać zmiany wydajności na podstawie rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="18ee0-144">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="18ee0-145">Wartość domyślna to 4 KB.</span><span class="sxs-lookup"><span data-stu-id="18ee0-145">The default is 4 KB.</span></span>
* <span data-ttu-id="18ee0-146">`AllowedOrigins` — lista dozwolonych wartości nagłówka pierwotnego dla żądań protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="18ee0-146">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="18ee0-147">Domyślnie wszystkie źródła są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="18ee0-147">By default, all origins are allowed.</span></span> <span data-ttu-id="18ee0-148">Aby uzyskać szczegółowe informacje, zobacz sekcję "ograniczenie pochodzenia protokołu WebSocket" poniżej.</span><span class="sxs-lookup"><span data-stu-id="18ee0-148">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

## <a name="accept-websocket-requests"></a><span data-ttu-id="18ee0-149">Akceptuj żądania protokołu WebSocket</span><span class="sxs-lookup"><span data-stu-id="18ee0-149">Accept WebSocket requests</span></span>

<span data-ttu-id="18ee0-150">Na przykład w dalszej części cyklu życia żądania (w dalszej części metody `Configure` lub w metodzie działania) Sprawdź, czy jest to żądanie protokołu WebSocket i zaakceptuj żądanie protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="18ee0-150">Somewhere later in the request life cycle (later in the `Configure` method or in an action method, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="18ee0-151">Poniższy przykład znajduje się w dalszej części metody `Configure`:</span><span class="sxs-lookup"><span data-stu-id="18ee0-151">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="18ee0-152">Żądanie WebSocket może znajdować się na dowolnym adresie URL, ale ten przykładowy kod akceptuje tylko żądania dla `/ws`.</span><span class="sxs-lookup"><span data-stu-id="18ee0-152">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

<span data-ttu-id="18ee0-153">W przypadku korzystania z protokołu WebSocket **należy** zachować potok pośredniczący uruchomiony przez czas trwania połączenia.</span><span class="sxs-lookup"><span data-stu-id="18ee0-153">When using a WebSocket, you **must** keep the middleware pipeline running for the duration of the connection.</span></span> <span data-ttu-id="18ee0-154">Jeśli spróbujesz wysłać lub odebrać komunikat protokołu WebSocket po zakończeniu potoku programu pośredniczącego, może wystąpić wyjątek podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="18ee0-154">If you attempt to send or receive a WebSocket message after the middleware pipeline ends, you may get an exception like the following:</span></span>

```
System.Net.WebSockets.WebSocketException (0x80004005): The remote party closed the WebSocket connection without completing the close handshake. ---> System.ObjectDisposedException: Cannot write to the response body, the response has completed.
Object name: 'HttpResponseStream'.
```

<span data-ttu-id="18ee0-155">Jeśli używasz usługi w tle do zapisywania danych do protokołu WebSocket, upewnij się, że działa potok pośredniczący.</span><span class="sxs-lookup"><span data-stu-id="18ee0-155">If you're using a background service to write data to a WebSocket, make sure you keep the middleware pipeline running.</span></span> <span data-ttu-id="18ee0-156">W tym celu należy użyć <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span><span class="sxs-lookup"><span data-stu-id="18ee0-156">Do this by using a <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span></span> <span data-ttu-id="18ee0-157">Przekaż `TaskCompletionSource` do usługi w tle i Wywołaj <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> po zakończeniu pracy z obiektem WebSocket.</span><span class="sxs-lookup"><span data-stu-id="18ee0-157">Pass the `TaskCompletionSource` to your background service and have it call <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> when you finish with the WebSocket.</span></span> <span data-ttu-id="18ee0-158">Następnie `await` Właściwość <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> w trakcie żądania, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="18ee0-158">Then `await` the <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> property during the request, as shown in the following example:</span></span>

```csharp
app.Use(async (context, next) => {
    var socket = await context.WebSockets.AcceptWebSocketAsync();
    var socketFinishedTcs = new TaskCompletionSource<object>();

    BackgroundSocketProcessor.AddSocket(socket, socketFinishedTcs); 

    await socketFinishedTcs.Task;
});
```
<span data-ttu-id="18ee0-159">Zamknięty wyjątek protokołu WebSocket może również wystąpić, Jeśli powrócisz zbyt szybko z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="18ee0-159">The WebSocket closed exception can also happen if you return too soon from an action method.</span></span> <span data-ttu-id="18ee0-160">Jeśli zaakceptujesz gniazdo w metodzie Action, poczekaj na wykonanie kodu, który używa gniazda przed powrotem z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="18ee0-160">If you accept a socket in an action method, wait for the code that uses the socket to complete before returning from the action method.</span></span>

<span data-ttu-id="18ee0-161">Nigdy nie używaj `Task.Wait()`, `Task.Result`lub podobnych wywołań blokowania, aby poczekać na zakończenie pracy gniazda, co może spowodować poważne problemy z wątkami.</span><span class="sxs-lookup"><span data-stu-id="18ee0-161">Never use `Task.Wait()`, `Task.Result`, or similar blocking calls to wait for the socket to complete, as that can cause serious threading issues.</span></span> <span data-ttu-id="18ee0-162">Zawsze używaj `await`.</span><span class="sxs-lookup"><span data-stu-id="18ee0-162">Always use `await`.</span></span>

## <a name="send-and-receive-messages"></a><span data-ttu-id="18ee0-163">Wysyłanie i odbieranie komunikatów</span><span class="sxs-lookup"><span data-stu-id="18ee0-163">Send and receive messages</span></span>

<span data-ttu-id="18ee0-164">Metoda `AcceptWebSocketAsync` uaktualnia połączenie TCP do połączenia WebSocket i udostępnia obiekt [WebSocket](/dotnet/core/api/system.net.websockets.websocket) .</span><span class="sxs-lookup"><span data-stu-id="18ee0-164">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="18ee0-165">Użyj obiektu `WebSocket`, aby wysyłać i odbierać wiadomości.</span><span class="sxs-lookup"><span data-stu-id="18ee0-165">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="18ee0-166">Kod pokazywany wcześniej, który akceptuje żądanie protokołu WebSocket, przekazuje obiekt `WebSocket` do metody `Echo`.</span><span class="sxs-lookup"><span data-stu-id="18ee0-166">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="18ee0-167">Kod odbiera komunikat i natychmiast wysyła komunikat z powrotem.</span><span class="sxs-lookup"><span data-stu-id="18ee0-167">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="18ee0-168">Komunikaty są wysyłane i odbierane w pętli, dopóki klient nie zamknie połączenia:</span><span class="sxs-lookup"><span data-stu-id="18ee0-168">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

<span data-ttu-id="18ee0-169">Po zaakceptowaniu połączenia z użyciem protokołu WebSocket przed rozpoczęciem pętli potok oprogramowania pośredniczącego zostaje zakończony.</span><span class="sxs-lookup"><span data-stu-id="18ee0-169">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="18ee0-170">Po zamknięciu gniazda potok nie jest zawijany.</span><span class="sxs-lookup"><span data-stu-id="18ee0-170">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="18ee0-171">Oznacza to, że żądanie przestaje poruszać się w potoku po zaakceptowaniu protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="18ee0-171">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="18ee0-172">Gdy pętla zostanie zakończona, a gniazdo jest zamknięte, żądanie wykonuje kopię zapasową potoku.</span><span class="sxs-lookup"><span data-stu-id="18ee0-172">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="handle-client-disconnects"></a><span data-ttu-id="18ee0-173">Obsługa odłączeń klientów</span><span class="sxs-lookup"><span data-stu-id="18ee0-173">Handle client disconnects</span></span>

<span data-ttu-id="18ee0-174">Serwer nie zostanie automatycznie poinformowany, gdy klient odłączy się ze względu na utratę łączności.</span><span class="sxs-lookup"><span data-stu-id="18ee0-174">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="18ee0-175">Serwer odbiera komunikat rozłączenia tylko wtedy, gdy klient wysyła go, którego nie można zrobić w przypadku utraty połączenia z Internetem.</span><span class="sxs-lookup"><span data-stu-id="18ee0-175">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="18ee0-176">Jeśli chcesz wykonać pewne działania, należy ustawić limit czasu po odebraniu niczego z klienta w określonym przedziale czasu.</span><span class="sxs-lookup"><span data-stu-id="18ee0-176">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="18ee0-177">Jeśli klient nie zawsze wysyła komunikaty i nie chcesz przekroczyć limitu czasu, ponieważ połączenie przechodzi w stan bezczynności, klient wysyła komunikat ping co X s przy użyciu czasomierza.</span><span class="sxs-lookup"><span data-stu-id="18ee0-177">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="18ee0-178">Na serwerze, jeśli wiadomość nie dotarła w ciągu 2\*X sekund po poprzedniej, Zakończ połączenie i Zgłoś, że klient został odłączony.</span><span class="sxs-lookup"><span data-stu-id="18ee0-178">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="18ee0-179">Poczekaj dwa razy oczekiwany przedział czasu na pozostawienie dodatkowego czasu na opóźnienia sieci, które mogą przytrzymać komunikat ping.</span><span class="sxs-lookup"><span data-stu-id="18ee0-179">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="18ee0-180">Ograniczenie pochodzenia obiektu WebSocket</span><span class="sxs-lookup"><span data-stu-id="18ee0-180">WebSocket origin restriction</span></span>

<span data-ttu-id="18ee0-181">Ochrona zapewniana przez mechanizm CORS nie ma zastosowania do obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="18ee0-181">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="18ee0-182">Przeglądarki **nie**:</span><span class="sxs-lookup"><span data-stu-id="18ee0-182">Browsers do **not**:</span></span>

* <span data-ttu-id="18ee0-183">Wykonaj żądania funkcji CORS przed inspekcją.</span><span class="sxs-lookup"><span data-stu-id="18ee0-183">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="18ee0-184">Przestrzeganie ograniczeń określonych w `Access-Control` nagłówkach podczas wykonywania żądań WebSocket.</span><span class="sxs-lookup"><span data-stu-id="18ee0-184">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="18ee0-185">Jednak przeglądarki wysyłają nagłówek `Origin` podczas wystawiania żądań protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="18ee0-185">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="18ee0-186">Aplikacje powinny być skonfigurowane do sprawdzania poprawności tych nagłówków, aby upewnić się, że dozwolone są tylko usługi WebSockets pochodzące z oczekiwanych źródeł.</span><span class="sxs-lookup"><span data-stu-id="18ee0-186">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="18ee0-187">Jeśli serwer jest hostem "https://server.com" i hostuje klienta na "https://client.com", Dodaj "https://client.com" do listy `AllowedOrigins` dla obiektów WebSockets do zweryfikowania.</span><span class="sxs-lookup"><span data-stu-id="18ee0-187">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="18ee0-188">Nagłówek `Origin` jest kontrolowany przez klienta, podobnie jak nagłówek `Referer`, może zostać sfałszowany.</span><span class="sxs-lookup"><span data-stu-id="18ee0-188">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="18ee0-189">Tych nagłówków **nie** należy używać jako mechanizmu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="18ee0-189">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="18ee0-190">Obsługa usług IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="18ee0-190">IIS/IIS Express support</span></span>

<span data-ttu-id="18ee0-191">System Windows Server 2012 lub nowszy oraz system Windows 8 lub nowszy z usługami IIS/IIS Express 8 lub nowszym obsługują protokół WebSocket.</span><span class="sxs-lookup"><span data-stu-id="18ee0-191">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="18ee0-192">W przypadku korzystania z IIS Express usługi WebSockets są zawsze włączone.</span><span class="sxs-lookup"><span data-stu-id="18ee0-192">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="18ee0-193">Włączanie obiektów WebSockets w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="18ee0-193">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="18ee0-194">Aby włączyć obsługę protokołu WebSocket w systemie Windows Server 2012 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="18ee0-194">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="18ee0-195">Te kroki nie są wymagane w przypadku korzystania z IIS Express</span><span class="sxs-lookup"><span data-stu-id="18ee0-195">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="18ee0-196">Użyj kreatora **dodawania ról i funkcji** z menu **Zarządzaj** lub łącza w **Menedżer serwera**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-196">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="18ee0-197">Wybierz opcję **Instalacja oparta na rolach lub oparta na funkcjach**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-197">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="18ee0-198">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-198">Select **Next**.</span></span>
1. <span data-ttu-id="18ee0-199">Wybierz odpowiedni serwer (serwer lokalny jest domyślnie wybrany).</span><span class="sxs-lookup"><span data-stu-id="18ee0-199">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="18ee0-200">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-200">Select **Next**.</span></span>
1. <span data-ttu-id="18ee0-201">Rozwiń węzeł **serwer sieci Web (IIS)** w drzewie **role** , rozwiń węzeł **serwer sieci Web**, a następnie rozwiń węzeł **Programowanie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-201">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="18ee0-202">Wybierz pozycję **Protokół WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-202">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="18ee0-203">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-203">Select **Next**.</span></span>
1. <span data-ttu-id="18ee0-204">Jeśli nie są potrzebne dodatkowe funkcje, wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-204">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="18ee0-205">Wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-205">Select **Install**.</span></span>
1. <span data-ttu-id="18ee0-206">Po zakończeniu instalacji wybierz pozycję **Zamknij** , aby zakończyć pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="18ee0-206">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="18ee0-207">Aby włączyć obsługę protokołu WebSocket w systemie Windows 8 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="18ee0-207">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="18ee0-208">Te kroki nie są wymagane w przypadku korzystania z IIS Express</span><span class="sxs-lookup"><span data-stu-id="18ee0-208">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="18ee0-209">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **włączać lub wyłączać funkcje systemu Windows** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="18ee0-209">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="18ee0-210">Otwórz następujące węzły: **Internet Information Services** > **World Wide Web Services** > **funkcje projektowania aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-210">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="18ee0-211">Wybierz funkcję **protokołu WebSocket** .</span><span class="sxs-lookup"><span data-stu-id="18ee0-211">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="18ee0-212">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-212">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="18ee0-213">Wyłączenie protokołu WebSocket w przypadku używania socket.io w programie Node. js</span><span class="sxs-lookup"><span data-stu-id="18ee0-213">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="18ee0-214">W przypadku korzystania z obsługi protokołu WebSocket w programie [Socket.IO](https://socket.io/) w [węźle Node. js](https://nodejs.org/)należy wyłączyć domyślny moduł WebSocket usług IIS przy użyciu elementu `webSocket` w *pliku Web. config* lub *ApplicationHost. config*. Jeśli ten krok nie zostanie wykonany, moduł WebSocket usług IIS podejmie próbę obsługi komunikacji z użyciem protokołu WebSocket zamiast środowiska Node. js i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="18ee0-214">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="sample-app"></a><span data-ttu-id="18ee0-215">Przykładowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="18ee0-215">Sample app</span></span>

<span data-ttu-id="18ee0-216">[Przykładowa aplikacja](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) , która jest dołączona do tego artykułu, jest aplikacją echo.</span><span class="sxs-lookup"><span data-stu-id="18ee0-216">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="18ee0-217">Ma stronę sieci Web, która udostępnia połączenia protokołu WebSocket, a serwer wysyła wszystkie komunikaty odebrane z powrotem do klienta.</span><span class="sxs-lookup"><span data-stu-id="18ee0-217">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="18ee0-218">Uruchom aplikację z wiersza polecenia (nie jest on skonfigurowany do uruchamiania z programu Visual Studio z IIS Express) i przejdź do http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="18ee0-218">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="18ee0-219">Na stronie sieci Web zostanie wyświetlony stan połączenia w lewym górnym rogu:</span><span class="sxs-lookup"><span data-stu-id="18ee0-219">The web page shows the connection status in the upper left:</span></span>

![Początkowy stan strony sieci Web](websockets/_static/start.png)

<span data-ttu-id="18ee0-221">Wybierz pozycję **Połącz** , aby wysłać żądanie protokołu WebSocket do podanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="18ee0-221">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="18ee0-222">Wprowadź wiadomość testową i wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-222">Enter a test message and select **Send**.</span></span> <span data-ttu-id="18ee0-223">Po zakończeniu wybierz pozycję **Zamknij gniazdo**.</span><span class="sxs-lookup"><span data-stu-id="18ee0-223">When done, select **Close Socket**.</span></span> <span data-ttu-id="18ee0-224">Sekcja **dziennika komunikacji** zgłasza poszczególne akcje otwierania, wysyłania i zamykania w miarę ich działania.</span><span class="sxs-lookup"><span data-stu-id="18ee0-224">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Początkowy stan strony sieci Web](websockets/_static/end.png)

