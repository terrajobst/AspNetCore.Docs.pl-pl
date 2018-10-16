---
title: Obsługa protokółu Websocket w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: b1e2180ed8dc93e2474ecca371d386830b7f3a9f
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348458"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="260c9-103">Obsługa protokółu Websocket w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="260c9-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="260c9-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Andrew Stanton pielęgniarki](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="260c9-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="260c9-105">W tym artykule wyjaśniono, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="260c9-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="260c9-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) to protokół, który umożliwia kanały komunikacja dwukierunkowa trwałego połączenia protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="260c9-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="260c9-107">Jest on używany w aplikacjach korzystających z komunikacji szybki, w czasie rzeczywistym, takich jak rozmowy, pulpit nawigacyjny i gier, aplikacji.</span><span class="sxs-lookup"><span data-stu-id="260c9-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="260c9-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="260c9-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="260c9-109">Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="260c9-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="260c9-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="260c9-110">Prerequisites</span></span>

* <span data-ttu-id="260c9-111">Platforma ASP.NET Core 1.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="260c9-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="260c9-112">Dowolny system operacyjny, który obsługuje platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="260c9-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="260c9-113">Windows 7 / Windows Server 2008 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="260c9-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="260c9-114">Linux</span><span class="sxs-lookup"><span data-stu-id="260c9-114">Linux</span></span>
  * <span data-ttu-id="260c9-115">macOS</span><span class="sxs-lookup"><span data-stu-id="260c9-115">macOS</span></span>
  
* <span data-ttu-id="260c9-116">Jeśli aplikacja działa w systemie Windows za pomocą programu IIS:</span><span class="sxs-lookup"><span data-stu-id="260c9-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="260c9-117">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="260c9-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="260c9-118">Usługi IIS 8 / 8 usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="260c9-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="260c9-119">Musi być włączona funkcja WebSockets (zobacz [obsługi usług IIS/IIS Express](#iisiis-express-support) sekcji.).</span><span class="sxs-lookup"><span data-stu-id="260c9-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="260c9-120">Jeśli aplikacja jest uruchamiana [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="260c9-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="260c9-121">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="260c9-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="260c9-122">W przypadku obsługiwanych przeglądarek, zobacz https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="260c9-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="260c9-123">Kiedy należy używać funkcji WebSockets</span><span class="sxs-lookup"><span data-stu-id="260c9-123">When to use WebSockets</span></span>

<span data-ttu-id="260c9-124">Do pracy bezpośrednio za pomocą połączenia gniazda, należy użyć funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="260c9-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="260c9-125">Aby uzyskać optymalną wydajność w czasie rzeczywistym gry, na przykład użyć funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="260c9-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="260c9-126">[Biblioteki SignalR platformy ASP.NET Core](xref:signalr/introduction) to biblioteka, która ułatwia dodawanie funkcji internetowych w czasie rzeczywistym do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="260c9-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="260c9-127">Używa funkcji WebSockets, jeśli to możliwe.</span><span class="sxs-lookup"><span data-stu-id="260c9-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="260c9-128">Jak używać funkcji WebSockets</span><span class="sxs-lookup"><span data-stu-id="260c9-128">How to use WebSockets</span></span>

* <span data-ttu-id="260c9-129">Zainstaluj [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="260c9-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="260c9-130">Konfigurowanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="260c9-130">Configure the middleware.</span></span>
* <span data-ttu-id="260c9-131">Akceptują żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="260c9-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="260c9-132">Wysyłanie i odbieranie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="260c9-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="260c9-133">Konfigurowanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="260c9-133">Configure the middleware</span></span>

<span data-ttu-id="260c9-134">Dodaj oprogramowanie pośredniczące Websocket w `Configure` metody `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="260c9-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

<span data-ttu-id="260c9-135">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="260c9-135">The following settings can be configured:</span></span>

* <span data-ttu-id="260c9-136">`KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="260c9-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="260c9-137">`ReceiveBufferSize` — Rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="260c9-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="260c9-138">Użytkownicy zaawansowani trzeba to zmienić dotyczące dostosowywania wydajności na podstawie rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="260c9-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="260c9-139">Akceptować żądania protokołu WebSocket</span><span class="sxs-lookup"><span data-stu-id="260c9-139">Accept WebSocket requests</span></span>

<span data-ttu-id="260c9-140">Gdzieś później w cyklu życia żądania (w dalszej części `Configure` metody lub Akcja kontrolera MVC, na przykład) sprawdź, czy jest on żądanie protokołu WebSocket i zaakceptuj żądanie protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="260c9-140">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="260c9-141">Poniższy przykład znajduje się w dalszej części w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="260c9-141">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="260c9-142">Żądanie protokołu WebSocket może występować na dowolny adres URL, ale ten przykładowy kod akceptuje tylko żądania dotyczące `/ws`.</span><span class="sxs-lookup"><span data-stu-id="260c9-142">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="260c9-143">Wysyłanie i odbieranie komunikatów</span><span class="sxs-lookup"><span data-stu-id="260c9-143">Send and receive messages</span></span>

<span data-ttu-id="260c9-144">`AcceptWebSocketAsync` Metoda uaktualnia połączenie TCP z połączeniem WebSocket i zapewnia [WebSocket](/dotnet/core/api/system.net.websockets.websocket) obiektu.</span><span class="sxs-lookup"><span data-stu-id="260c9-144">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="260c9-145">Użyj `WebSocket` obiekt do wysyłania i odbierania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="260c9-145">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="260c9-146">Przekazuje kodu pokazanego wcześniej, która akceptuje żądanie protokołu WebSocket `WebSocket` obiekt `Echo` metody.</span><span class="sxs-lookup"><span data-stu-id="260c9-146">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="260c9-147">Kod odbiera komunikat i natychmiast wysyła z powrotem tę samą wiadomość.</span><span class="sxs-lookup"><span data-stu-id="260c9-147">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="260c9-148">Wiadomości wysyłanych i odbieranych w pętli, dopóki klient zamyka połączenie:</span><span class="sxs-lookup"><span data-stu-id="260c9-148">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="260c9-149">Akceptując połączeniem WebSocket, przed rozpoczęciem pętli, kończy się potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="260c9-149">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="260c9-150">Po zamknięciu gniazda, rozwija potoku.</span><span class="sxs-lookup"><span data-stu-id="260c9-150">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="260c9-151">Oznacza to, że żądanie zatrzymuje, przenoszenie do przodu w potoku po zaakceptowaniu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="260c9-151">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="260c9-152">Po zakończeniu pętli i gniazda jest zamknięty, żądanie będzie kontynuowane wykonywanie kopii zapasowych potoku.</span><span class="sxs-lookup"><span data-stu-id="260c9-152">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="260c9-153">Obsługa usług IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="260c9-153">IIS/IIS Express support</span></span>

<span data-ttu-id="260c9-154">Windows Server 2012 lub nowszym i Windows 8 lub nowszym z usług IIS/IIS Express 8 lub nowszy jest obsługa protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="260c9-154">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="260c9-155">Gniazda Websocket są zawsze włączone, korzystając z usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="260c9-155">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="260c9-156">Włączanie funkcji WebSockets w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="260c9-156">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="260c9-157">Aby włączyć obsługę protokołu WebSocket w systemie Windows Server 2012 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="260c9-157">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="260c9-158">Te kroki nie są wymagane w przypadku korzystania z usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="260c9-158">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="260c9-159">Użyj **Dodaj role i funkcje** kreatora z **Zarządzaj** menu lub linku w **Menedżera serwera**.</span><span class="sxs-lookup"><span data-stu-id="260c9-159">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="260c9-160">Wybierz **Instalacja oparta na rolach lub oparta na funkcjach**.</span><span class="sxs-lookup"><span data-stu-id="260c9-160">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="260c9-161">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="260c9-161">Select **Next**.</span></span>
1. <span data-ttu-id="260c9-162">Wybierz odpowiedni serwer (serwer lokalny jest wybrane domyślnie).</span><span class="sxs-lookup"><span data-stu-id="260c9-162">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="260c9-163">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="260c9-163">Select **Next**.</span></span>
1. <span data-ttu-id="260c9-164">Rozwiń **serwer sieci Web (IIS)** w **role** drzewa, a następnie rozwiń **serwera sieci Web**, a następnie rozwiń węzeł **opracowywanie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="260c9-164">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="260c9-165">Wybierz **protokołu WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="260c9-165">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="260c9-166">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="260c9-166">Select **Next**.</span></span>
1. <span data-ttu-id="260c9-167">Jeśli nie są wymagane dodatkowe funkcje, wybierz opcję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="260c9-167">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="260c9-168">Wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="260c9-168">Select **Install**.</span></span>
1. <span data-ttu-id="260c9-169">Po zakończeniu instalacji wybierz **Zamknij** aby zakończyć pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="260c9-169">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="260c9-170">Aby włączyć obsługę protokołu WebSocket w systemie Windows 8 lub nowszy:</span><span class="sxs-lookup"><span data-stu-id="260c9-170">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="260c9-171">Te kroki nie są wymagane w przypadku korzystania z usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="260c9-171">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="260c9-172">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="260c9-172">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="260c9-173">Otwórz następujące węzły: **Internetowe usługi informacyjne** > **usługi World Wide Web** > **funkcje tworzenia aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="260c9-173">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="260c9-174">Wybierz **protokołu WebSocket** funkcji.</span><span class="sxs-lookup"><span data-stu-id="260c9-174">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="260c9-175">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="260c9-175">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="260c9-176">Wyłącz WebSocket, gdy na języku Node.js przy użyciu biblioteki socket.io</span><span class="sxs-lookup"><span data-stu-id="260c9-176">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="260c9-177">Jeśli dzięki obsłudze protokołu WebSocket w [biblioteki socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), Wyłącz domyślne IIS WebSocket modułu przy użyciu `webSocket` element *web.config* lub *applicationHost.config*. Jeśli ten krok nie jest wykonywana, Moduł IIS WebSocket próbuje obsługi komunikacji protokołu WebSocket, a nie środowiska Node.js i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="260c9-177">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="260c9-178">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="260c9-178">Next steps</span></span>

<span data-ttu-id="260c9-179">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) którego zostanie dołączony, ten artykuł stanowi app echa.</span><span class="sxs-lookup"><span data-stu-id="260c9-179">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="260c9-180">Posiada strony sieci web, która sprawia, że połączeń protokołu WebSocket i wszelkich komunikatów odebranych wysyła ponownie serwer do klienta.</span><span class="sxs-lookup"><span data-stu-id="260c9-180">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="260c9-181">Uruchamianie aplikacji z poziomu wiersza polecenia (go nie skonfigurowała do uruchamiania w programie Visual Studio z programem IIS Express) i przejdź do http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="260c9-181">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="260c9-182">Strony sieci web pokazuje stan połączenia w lewym górnym rogu:</span><span class="sxs-lookup"><span data-stu-id="260c9-182">The web page shows the connection status in the upper left:</span></span>

![Początkowy stan strony sieci web](websockets/_static/start.png)

<span data-ttu-id="260c9-184">Wybierz **Connect** wysyłać żądanie protokołu WebSocket adres URL wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="260c9-184">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="260c9-185">Wprowadź wiadomość testową, a następnie wybierz pozycję **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="260c9-185">Enter a test message and select **Send**.</span></span> <span data-ttu-id="260c9-186">Po zakończeniu wybierz pozycję **Zamknij gniazda**.</span><span class="sxs-lookup"><span data-stu-id="260c9-186">When done, select **Close Socket**.</span></span> <span data-ttu-id="260c9-187">**Dziennika komunikacji** sekcji raporty każdy otwarty, wysyłanie i zamknij akcji, jak to się dzieje.</span><span class="sxs-lookup"><span data-stu-id="260c9-187">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Początkowy stan strony sieci web](websockets/_static/end.png)
