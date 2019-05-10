---
title: Obsługa protokółu Websocket w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/websockets
ms.openlocfilehash: 1b62dc91453437518e4b8f6f8dd0915977130766
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901252"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="7219c-103">Obsługa protokółu Websocket w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7219c-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="7219c-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Andrew Stanton pielęgniarki](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="7219c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="7219c-105">W tym artykule wyjaśniono, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7219c-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="7219c-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) to protokół, który umożliwia kanały komunikacja dwukierunkowa trwałego połączenia protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="7219c-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="7219c-107">Jest on używany w aplikacjach korzystających z komunikacji szybki, w czasie rzeczywistym, takich jak rozmowy, pulpit nawigacyjny i gier, aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7219c-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="7219c-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7219c-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="7219c-109">Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="7219c-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7219c-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7219c-110">Prerequisites</span></span>

* <span data-ttu-id="7219c-111">Platforma ASP.NET Core 1.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="7219c-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="7219c-112">Dowolny system operacyjny, który obsługuje platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7219c-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="7219c-113">Windows 7 / Windows Server 2008 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="7219c-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="7219c-114">Linux</span><span class="sxs-lookup"><span data-stu-id="7219c-114">Linux</span></span>
  * <span data-ttu-id="7219c-115">macOS</span><span class="sxs-lookup"><span data-stu-id="7219c-115">macOS</span></span>
  
* <span data-ttu-id="7219c-116">Jeśli aplikacja działa w systemie Windows za pomocą programu IIS:</span><span class="sxs-lookup"><span data-stu-id="7219c-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="7219c-117">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="7219c-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="7219c-118">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="7219c-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="7219c-119">Musi być włączona funkcja WebSockets (zobacz [obsługi usług IIS/IIS Express](#iisiis-express-support) sekcji.).</span><span class="sxs-lookup"><span data-stu-id="7219c-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="7219c-120">Jeśli aplikacja jest uruchamiana [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="7219c-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="7219c-121">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="7219c-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="7219c-122">W przypadku obsługiwanych przeglądarek, zobacz https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="7219c-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="7219c-123">Kiedy należy używać funkcji WebSockets</span><span class="sxs-lookup"><span data-stu-id="7219c-123">When to use WebSockets</span></span>

<span data-ttu-id="7219c-124">Do pracy bezpośrednio za pomocą połączenia gniazda, należy użyć funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="7219c-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="7219c-125">Aby uzyskać optymalną wydajność w czasie rzeczywistym gry, na przykład użyć funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="7219c-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="7219c-126">[Biblioteki SignalR platformy ASP.NET Core](xref:signalr/introduction) to biblioteka, która ułatwia dodawanie funkcji internetowych w czasie rzeczywistym do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7219c-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="7219c-127">Używa funkcji WebSockets, jeśli to możliwe.</span><span class="sxs-lookup"><span data-stu-id="7219c-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="7219c-128">Jak używać funkcji WebSockets</span><span class="sxs-lookup"><span data-stu-id="7219c-128">How to use WebSockets</span></span>

* <span data-ttu-id="7219c-129">Zainstaluj [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="7219c-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="7219c-130">Konfigurowanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="7219c-130">Configure the middleware.</span></span>
* <span data-ttu-id="7219c-131">Akceptują żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7219c-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="7219c-132">Wysyłanie i odbieranie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="7219c-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="7219c-133">Konfigurowanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="7219c-133">Configure the middleware</span></span>

<span data-ttu-id="7219c-134">Dodaj oprogramowanie pośredniczące Websocket w `Configure` metody `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="7219c-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7219c-135">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="7219c-135">The following settings can be configured:</span></span>

* <span data-ttu-id="7219c-136">`KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="7219c-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="7219c-137">Wartość domyślna to dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="7219c-137">The default is two minutes.</span></span>
* <span data-ttu-id="7219c-138">`ReceiveBufferSize` — Rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="7219c-138">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="7219c-139">Użytkownicy zaawansowani trzeba to zmienić dotyczące dostosowywania wydajności na podstawie rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="7219c-139">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="7219c-140">Wartość domyślna to 4 KB.</span><span class="sxs-lookup"><span data-stu-id="7219c-140">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7219c-141">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="7219c-141">The following settings can be configured:</span></span>

* <span data-ttu-id="7219c-142">`KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="7219c-142">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="7219c-143">Wartość domyślna to dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="7219c-143">The default is two minutes.</span></span>
* <span data-ttu-id="7219c-144">`ReceiveBufferSize` — Rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="7219c-144">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="7219c-145">Użytkownicy zaawansowani trzeba to zmienić dotyczące dostosowywania wydajności na podstawie rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="7219c-145">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="7219c-146">Wartość domyślna to 4 KB.</span><span class="sxs-lookup"><span data-stu-id="7219c-146">The default is 4 KB.</span></span>
* <span data-ttu-id="7219c-147">`AllowedOrigins` -Lista dozwolonych wartości nagłówka pochodzenia dla żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7219c-147">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="7219c-148">Domyślnie wszystkie źródła są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="7219c-148">By default, all origins are allowed.</span></span> <span data-ttu-id="7219c-149">Aby uzyskać szczegółowe informacje, zobacz "WebSocket pochodzenia ograniczenia" poniżej.</span><span class="sxs-lookup"><span data-stu-id="7219c-149">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="7219c-150">Akceptować żądania protokołu WebSocket</span><span class="sxs-lookup"><span data-stu-id="7219c-150">Accept WebSocket requests</span></span>

<span data-ttu-id="7219c-151">Gdzieś później w cyklu życia żądania (w dalszej części `Configure` metody lub Akcja kontrolera MVC, na przykład) sprawdź, czy jest on żądanie protokołu WebSocket i zaakceptuj żądanie protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7219c-151">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="7219c-152">Poniższy przykład znajduje się w dalszej części w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="7219c-152">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="7219c-153">Żądanie protokołu WebSocket może występować na dowolny adres URL, ale ten przykładowy kod akceptuje tylko żądania dotyczące `/ws`.</span><span class="sxs-lookup"><span data-stu-id="7219c-153">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="7219c-154">Wysyłanie i odbieranie komunikatów</span><span class="sxs-lookup"><span data-stu-id="7219c-154">Send and receive messages</span></span>

<span data-ttu-id="7219c-155">`AcceptWebSocketAsync` Metoda uaktualnia połączenie TCP z połączeniem WebSocket i zapewnia [WebSocket](/dotnet/core/api/system.net.websockets.websocket) obiektu.</span><span class="sxs-lookup"><span data-stu-id="7219c-155">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="7219c-156">Użyj `WebSocket` obiekt do wysyłania i odbierania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="7219c-156">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="7219c-157">Przekazuje kodu pokazanego wcześniej, która akceptuje żądanie protokołu WebSocket `WebSocket` obiekt `Echo` metody.</span><span class="sxs-lookup"><span data-stu-id="7219c-157">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="7219c-158">Kod odbiera komunikat i natychmiast wysyła z powrotem tę samą wiadomość.</span><span class="sxs-lookup"><span data-stu-id="7219c-158">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="7219c-159">Wiadomości wysyłanych i odbieranych w pętli, dopóki klient zamyka połączenie:</span><span class="sxs-lookup"><span data-stu-id="7219c-159">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="7219c-160">Akceptując połączeniem WebSocket, przed rozpoczęciem pętli, kończy się potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="7219c-160">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="7219c-161">Po zamknięciu gniazda, rozwija potoku.</span><span class="sxs-lookup"><span data-stu-id="7219c-161">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="7219c-162">Oznacza to, że żądanie zatrzymuje, przenoszenie do przodu w potoku po zaakceptowaniu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7219c-162">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="7219c-163">Po zakończeniu pętli i gniazda jest zamknięty, żądanie będzie kontynuowane wykonywanie kopii zapasowych potoku.</span><span class="sxs-lookup"><span data-stu-id="7219c-163">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="handle-client-disconnects"></a><span data-ttu-id="7219c-164">Rozłącza klienta uchwytu</span><span class="sxs-lookup"><span data-stu-id="7219c-164">Handle client disconnects</span></span>

<span data-ttu-id="7219c-165">Serwer nie jest automatycznie powiadomiony, gdy klient odłączy się z powodu utraty łączności.</span><span class="sxs-lookup"><span data-stu-id="7219c-165">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="7219c-166">Serwer odebrał komunikatu o rozłączeniu, tylko wtedy, gdy klient wysyła, którego nie można wykonać w przypadku utraty połączenia z Internetem.</span><span class="sxs-lookup"><span data-stu-id="7219c-166">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="7219c-167">Jeśli chcesz wykonania określonego działania, jeśli tak się stanie, należy ustawić limit czasu po nic nie zostanie odebrana od klienta w przedziale czasu.</span><span class="sxs-lookup"><span data-stu-id="7219c-167">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="7219c-168">Jeśli klient nie jest zawsze wysyłanie komunikatów i użytkownik nie chce limitu czasu, po prostu, ponieważ połączenie przechodzi bezczynności, należy mieć klient używał czasomierza, aby wysłać komunikat ping co X sekund.</span><span class="sxs-lookup"><span data-stu-id="7219c-168">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="7219c-169">Na serwerze, jeśli wiadomość nie dotarła w ciągu 2\*X (w sekundach) po poprzedniej, Zakończ połączenie i raport, który klient odłączony.</span><span class="sxs-lookup"><span data-stu-id="7219c-169">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="7219c-170">Poczekaj, aż dwa razy przewidywanym czasie pozostawić dodatkowy czas opóźnienia sieci, które mogą blokować komunikat ping.</span><span class="sxs-lookup"><span data-stu-id="7219c-170">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="7219c-171">Ograniczenie pochodzenia WebSocket</span><span class="sxs-lookup"><span data-stu-id="7219c-171">WebSocket origin restriction</span></span>

<span data-ttu-id="7219c-172">Zabezpieczenia udostępniane przez mechanizm CORS nie dotyczą funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="7219c-172">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="7219c-173">Czy przeglądarek **nie**:</span><span class="sxs-lookup"><span data-stu-id="7219c-173">Browsers do **not**:</span></span>

* <span data-ttu-id="7219c-174">Wykonywanie żądań krótkiej mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="7219c-174">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="7219c-175">Przestrzeganie ograniczenia określone w `Access-Control` nagłówków w przypadku wysyłania żądań protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7219c-175">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="7219c-176">Jednak wysłać przeglądarek `Origin` nagłówka podczas wystawiania żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7219c-176">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="7219c-177">Aplikacje powinny być konfigurowane do sprawdzania poprawności tych nagłówków, aby upewnić się, że dozwolone są tylko WebSockets pochodzące z oczekiwanym źródła.</span><span class="sxs-lookup"><span data-stu-id="7219c-177">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="7219c-178">Jeśli przechowujesz serwera na "https://server.com"i hosting w systemie klienta"https://client.com", Dodaj "https://client.com" Aby `AllowedOrigins` listy dla funkcji WebSockets sprawdzić.</span><span class="sxs-lookup"><span data-stu-id="7219c-178">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="7219c-179">`Origin` Nagłówek jest kontrolowany przez klienta i, podobnie jak `Referer` nagłówka, mogą zostać sfałszowane.</span><span class="sxs-lookup"><span data-stu-id="7219c-179">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="7219c-180">Czy **nie** Użyj tych nagłówków jako mechanizm uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7219c-180">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="7219c-181">Obsługa usług IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="7219c-181">IIS/IIS Express support</span></span>

<span data-ttu-id="7219c-182">Windows Server 2012 lub nowszym i Windows 8 lub nowszym z usług IIS/IIS Express 8 lub nowszy jest obsługa protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7219c-182">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="7219c-183">Gniazda Websocket są zawsze włączone, korzystając z usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7219c-183">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="7219c-184">Włączanie funkcji WebSockets w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="7219c-184">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="7219c-185">Aby włączyć obsługę protokołu WebSocket w systemie Windows Server 2012 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="7219c-185">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="7219c-186">Te kroki nie są wymagane w przypadku korzystania z usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="7219c-186">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="7219c-187">Użyj **Dodaj role i funkcje** kreatora z **Zarządzaj** menu lub linku w **Menedżera serwera**.</span><span class="sxs-lookup"><span data-stu-id="7219c-187">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="7219c-188">Wybierz **Instalacja oparta na rolach lub oparta na funkcjach**.</span><span class="sxs-lookup"><span data-stu-id="7219c-188">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="7219c-189">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="7219c-189">Select **Next**.</span></span>
1. <span data-ttu-id="7219c-190">Wybierz odpowiedni serwer (serwer lokalny jest wybrane domyślnie).</span><span class="sxs-lookup"><span data-stu-id="7219c-190">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="7219c-191">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="7219c-191">Select **Next**.</span></span>
1. <span data-ttu-id="7219c-192">Rozwiń **serwer sieci Web (IIS)** w **role** drzewa, a następnie rozwiń **serwera sieci Web**, a następnie rozwiń węzeł **opracowywanie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="7219c-192">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="7219c-193">Wybierz **protokołu WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="7219c-193">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="7219c-194">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="7219c-194">Select **Next**.</span></span>
1. <span data-ttu-id="7219c-195">Jeśli nie są wymagane dodatkowe funkcje, wybierz opcję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7219c-195">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="7219c-196">Wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="7219c-196">Select **Install**.</span></span>
1. <span data-ttu-id="7219c-197">Po zakończeniu instalacji wybierz **Zamknij** aby zakończyć pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="7219c-197">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="7219c-198">Aby włączyć obsługę protokołu WebSocket w systemie Windows 8 lub nowszy:</span><span class="sxs-lookup"><span data-stu-id="7219c-198">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="7219c-199">Te kroki nie są wymagane w przypadku korzystania z usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="7219c-199">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="7219c-200">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="7219c-200">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="7219c-201">Otwórz następujące węzły: **Internetowe usługi informacyjne** > **usługi World Wide Web** > **funkcje tworzenia aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="7219c-201">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="7219c-202">Wybierz **protokołu WebSocket** funkcji.</span><span class="sxs-lookup"><span data-stu-id="7219c-202">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="7219c-203">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7219c-203">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="7219c-204">Wyłącz WebSocket, gdy na języku Node.js przy użyciu biblioteki socket.io</span><span class="sxs-lookup"><span data-stu-id="7219c-204">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="7219c-205">Jeśli dzięki obsłudze protokołu WebSocket w [biblioteki socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), Wyłącz domyślne IIS WebSocket modułu przy użyciu `webSocket` element *web.config* lub *applicationHost.config*. Jeśli ten krok nie jest wykonywana, Moduł IIS WebSocket próbuje obsługi komunikacji protokołu WebSocket, a nie środowiska Node.js i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7219c-205">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="7219c-206">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7219c-206">Next steps</span></span>

<span data-ttu-id="7219c-207">[Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) którego zostanie dołączony, ten artykuł stanowi app echa.</span><span class="sxs-lookup"><span data-stu-id="7219c-207">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="7219c-208">Posiada strony sieci web, która sprawia, że połączeń protokołu WebSocket i wszelkich komunikatów odebranych wysyła ponownie serwer do klienta.</span><span class="sxs-lookup"><span data-stu-id="7219c-208">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="7219c-209">Uruchamianie aplikacji z poziomu wiersza polecenia (go nie skonfigurowała do uruchamiania w programie Visual Studio z programem IIS Express) i przejdź do http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="7219c-209">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="7219c-210">Strony sieci web pokazuje stan połączenia w lewym górnym rogu:</span><span class="sxs-lookup"><span data-stu-id="7219c-210">The web page shows the connection status in the upper left:</span></span>

![Początkowy stan strony sieci web](websockets/_static/start.png)

<span data-ttu-id="7219c-212">Wybierz **Connect** wysyłać żądanie protokołu WebSocket adres URL wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="7219c-212">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="7219c-213">Wprowadź wiadomość testową, a następnie wybierz pozycję **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="7219c-213">Enter a test message and select **Send**.</span></span> <span data-ttu-id="7219c-214">Po zakończeniu wybierz pozycję **Zamknij gniazda**.</span><span class="sxs-lookup"><span data-stu-id="7219c-214">When done, select **Close Socket**.</span></span> <span data-ttu-id="7219c-215">**Dziennika komunikacji** sekcji raporty każdy otwarty, wysyłanie i zamknij akcji, jak to się dzieje.</span><span class="sxs-lookup"><span data-stu-id="7219c-215">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Początkowy stan strony sieci web](websockets/_static/end.png)
