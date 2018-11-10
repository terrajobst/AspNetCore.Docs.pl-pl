---
title: Obsługa protokółu Websocket w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/06/2018
uid: fundamentals/websockets
ms.openlocfilehash: 3a649f88699d61636d9aa7fbfe4468ca67b3b018
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225411"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="b6594-103">Obsługa protokółu Websocket w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6594-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="b6594-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Andrew Stanton pielęgniarki](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="b6594-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="b6594-105">W tym artykule wyjaśniono, jak rozpocząć pracę z gniazda Websocket w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6594-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="b6594-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) to protokół, który umożliwia kanały komunikacja dwukierunkowa trwałego połączenia protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="b6594-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="b6594-107">Jest on używany w aplikacjach korzystających z komunikacji szybki, w czasie rzeczywistym, takich jak rozmowy, pulpit nawigacyjny i gier, aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6594-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="b6594-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b6594-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="b6594-109">Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="b6594-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6594-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b6594-110">Prerequisites</span></span>

* <span data-ttu-id="b6594-111">Platforma ASP.NET Core 1.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="b6594-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="b6594-112">Dowolny system operacyjny, który obsługuje platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b6594-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="b6594-113">Windows 7 / Windows Server 2008 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="b6594-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="b6594-114">Linux</span><span class="sxs-lookup"><span data-stu-id="b6594-114">Linux</span></span>
  * <span data-ttu-id="b6594-115">macOS</span><span class="sxs-lookup"><span data-stu-id="b6594-115">macOS</span></span>
  
* <span data-ttu-id="b6594-116">Jeśli aplikacja działa w systemie Windows za pomocą programu IIS:</span><span class="sxs-lookup"><span data-stu-id="b6594-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="b6594-117">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="b6594-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="b6594-118">Usługi IIS 8 / 8 usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="b6594-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="b6594-119">Musi być włączona funkcja WebSockets (zobacz [obsługi usług IIS/IIS Express](#iisiis-express-support) sekcji.).</span><span class="sxs-lookup"><span data-stu-id="b6594-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="b6594-120">Jeśli aplikacja jest uruchamiana [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="b6594-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="b6594-121">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="b6594-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="b6594-122">W przypadku obsługiwanych przeglądarek, zobacz https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="b6594-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="b6594-123">Kiedy należy używać funkcji WebSockets</span><span class="sxs-lookup"><span data-stu-id="b6594-123">When to use WebSockets</span></span>

<span data-ttu-id="b6594-124">Do pracy bezpośrednio za pomocą połączenia gniazda, należy użyć funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b6594-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="b6594-125">Aby uzyskać optymalną wydajność w czasie rzeczywistym gry, na przykład użyć funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b6594-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="b6594-126">[Biblioteki SignalR platformy ASP.NET Core](xref:signalr/introduction) to biblioteka, która ułatwia dodawanie funkcji internetowych w czasie rzeczywistym do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6594-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="b6594-127">Używa funkcji WebSockets, jeśli to możliwe.</span><span class="sxs-lookup"><span data-stu-id="b6594-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="b6594-128">Jak używać funkcji WebSockets</span><span class="sxs-lookup"><span data-stu-id="b6594-128">How to use WebSockets</span></span>

* <span data-ttu-id="b6594-129">Zainstaluj [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="b6594-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="b6594-130">Konfigurowanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="b6594-130">Configure the middleware.</span></span>
* <span data-ttu-id="b6594-131">Akceptują żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b6594-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="b6594-132">Wysyłanie i odbieranie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="b6594-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="b6594-133">Konfigurowanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="b6594-133">Configure the middleware</span></span>

<span data-ttu-id="b6594-134">Dodaj oprogramowanie pośredniczące Websocket w `Configure` metody `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="b6594-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b6594-135">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="b6594-135">The following settings can be configured:</span></span>

* <span data-ttu-id="b6594-136">`KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="b6594-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="b6594-137">Wartość domyślna to dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="b6594-137">The default is two minutes.</span></span>
* <span data-ttu-id="b6594-138">`ReceiveBufferSize` — Rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="b6594-138">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="b6594-139">Użytkownicy zaawansowani trzeba to zmienić dotyczące dostosowywania wydajności na podstawie rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="b6594-139">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="b6594-140">Wartość domyślna to 4 KB.</span><span class="sxs-lookup"><span data-stu-id="b6594-140">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b6594-141">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="b6594-141">The following settings can be configured:</span></span>

* <span data-ttu-id="b6594-142">`KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="b6594-142">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="b6594-143">Wartość domyślna to dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="b6594-143">The default is two minutes.</span></span>
* <span data-ttu-id="b6594-144">`ReceiveBufferSize` — Rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="b6594-144">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="b6594-145">Użytkownicy zaawansowani trzeba to zmienić dotyczące dostosowywania wydajności na podstawie rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="b6594-145">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="b6594-146">Wartość domyślna to 4 KB.</span><span class="sxs-lookup"><span data-stu-id="b6594-146">The default is 4 KB.</span></span>
* <span data-ttu-id="b6594-147">`AllowedOrigins` -Lista dozwolonych wartości nagłówka pochodzenia dla żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b6594-147">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="b6594-148">Domyślnie wszystkie źródła są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="b6594-148">By default, all origins are allowed.</span></span> <span data-ttu-id="b6594-149">Aby uzyskać szczegółowe informacje, zobacz "WebSocket pochodzenia ograniczenia" poniżej.</span><span class="sxs-lookup"><span data-stu-id="b6594-149">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="b6594-150">Akceptować żądania protokołu WebSocket</span><span class="sxs-lookup"><span data-stu-id="b6594-150">Accept WebSocket requests</span></span>

<span data-ttu-id="b6594-151">Gdzieś później w cyklu życia żądania (w dalszej części `Configure` metody lub Akcja kontrolera MVC, na przykład) sprawdź, czy jest on żądanie protokołu WebSocket i zaakceptuj żądanie protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b6594-151">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="b6594-152">Poniższy przykład znajduje się w dalszej części w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="b6594-152">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="b6594-153">Żądanie protokołu WebSocket może występować na dowolny adres URL, ale ten przykładowy kod akceptuje tylko żądania dotyczące `/ws`.</span><span class="sxs-lookup"><span data-stu-id="b6594-153">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="b6594-154">Wysyłanie i odbieranie komunikatów</span><span class="sxs-lookup"><span data-stu-id="b6594-154">Send and receive messages</span></span>

<span data-ttu-id="b6594-155">`AcceptWebSocketAsync` Metoda uaktualnia połączenie TCP z połączeniem WebSocket i zapewnia [WebSocket](/dotnet/core/api/system.net.websockets.websocket) obiektu.</span><span class="sxs-lookup"><span data-stu-id="b6594-155">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="b6594-156">Użyj `WebSocket` obiekt do wysyłania i odbierania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="b6594-156">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="b6594-157">Przekazuje kodu pokazanego wcześniej, która akceptuje żądanie protokołu WebSocket `WebSocket` obiekt `Echo` metody.</span><span class="sxs-lookup"><span data-stu-id="b6594-157">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="b6594-158">Kod odbiera komunikat i natychmiast wysyła z powrotem tę samą wiadomość.</span><span class="sxs-lookup"><span data-stu-id="b6594-158">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="b6594-159">Wiadomości wysyłanych i odbieranych w pętli, dopóki klient zamyka połączenie:</span><span class="sxs-lookup"><span data-stu-id="b6594-159">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="b6594-160">Akceptując połączeniem WebSocket, przed rozpoczęciem pętli, kończy się potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="b6594-160">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="b6594-161">Po zamknięciu gniazda, rozwija potoku.</span><span class="sxs-lookup"><span data-stu-id="b6594-161">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="b6594-162">Oznacza to, że żądanie zatrzymuje, przenoszenie do przodu w potoku po zaakceptowaniu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b6594-162">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="b6594-163">Po zakończeniu pętli i gniazda jest zamknięty, żądanie będzie kontynuowane wykonywanie kopii zapasowych potoku.</span><span class="sxs-lookup"><span data-stu-id="b6594-163">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="websocket-origin-restriction"></a><span data-ttu-id="b6594-164">Ograniczenie pochodzenia WebSocket</span><span class="sxs-lookup"><span data-stu-id="b6594-164">WebSocket origin restriction</span></span>

<span data-ttu-id="b6594-165">Zabezpieczenia udostępniane przez mechanizm CORS nie dotyczą funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b6594-165">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="b6594-166">Czy przeglądarek **nie**:</span><span class="sxs-lookup"><span data-stu-id="b6594-166">Browsers do **not**:</span></span>

* <span data-ttu-id="b6594-167">Wykonywanie żądań krótkiej mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="b6594-167">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="b6594-168">Przestrzeganie ograniczenia określone w `Access-Control` nagłówków w przypadku wysyłania żądań protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b6594-168">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="b6594-169">Jednak wysłać przeglądarek `Origin` nagłówka podczas wystawiania żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b6594-169">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="b6594-170">Aplikacje powinny być konfigurowane do sprawdzania poprawności tych nagłówków, aby upewnić się, że dozwolone są tylko WebSockets pochodzące z oczekiwanym źródła.</span><span class="sxs-lookup"><span data-stu-id="b6594-170">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="b6594-171">Jeśli przechowujesz serwera na "https://server.com"i hosting w systemie klienta"https://client.com", Dodaj "https://client.com" Aby `AllowedOrigins` listy dla funkcji WebSockets sprawdzić.</span><span class="sxs-lookup"><span data-stu-id="b6594-171">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

```csharp
app.UseWebSockets(new WebSocketOptions()
{
    AllowedOrigins.Add("https://client.com");
    AllowedOrigins.Add("https://www.client.com");
});
```

> [!NOTE]
> <span data-ttu-id="b6594-172">`Origin` Nagłówek jest kontrolowany przez klienta i, podobnie jak `Referer` nagłówka, mogą zostać sfałszowane.</span><span class="sxs-lookup"><span data-stu-id="b6594-172">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="b6594-173">Czy **nie** Użyj tych nagłówków jako mechanizm uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="b6594-173">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="b6594-174">Obsługa usług IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="b6594-174">IIS/IIS Express support</span></span>

<span data-ttu-id="b6594-175">Windows Server 2012 lub nowszym i Windows 8 lub nowszym z usług IIS/IIS Express 8 lub nowszy jest obsługa protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b6594-175">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="b6594-176">Gniazda Websocket są zawsze włączone, korzystając z usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b6594-176">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="b6594-177">Włączanie funkcji WebSockets w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="b6594-177">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="b6594-178">Aby włączyć obsługę protokołu WebSocket w systemie Windows Server 2012 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="b6594-178">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="b6594-179">Te kroki nie są wymagane w przypadku korzystania z usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="b6594-179">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="b6594-180">Użyj **Dodaj role i funkcje** kreatora z **Zarządzaj** menu lub linku w **Menedżera serwera**.</span><span class="sxs-lookup"><span data-stu-id="b6594-180">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="b6594-181">Wybierz **Instalacja oparta na rolach lub oparta na funkcjach**.</span><span class="sxs-lookup"><span data-stu-id="b6594-181">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="b6594-182">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="b6594-182">Select **Next**.</span></span>
1. <span data-ttu-id="b6594-183">Wybierz odpowiedni serwer (serwer lokalny jest wybrane domyślnie).</span><span class="sxs-lookup"><span data-stu-id="b6594-183">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="b6594-184">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="b6594-184">Select **Next**.</span></span>
1. <span data-ttu-id="b6594-185">Rozwiń **serwer sieci Web (IIS)** w **role** drzewa, a następnie rozwiń **serwera sieci Web**, a następnie rozwiń węzeł **opracowywanie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="b6594-185">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="b6594-186">Wybierz **protokołu WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="b6594-186">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="b6594-187">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="b6594-187">Select **Next**.</span></span>
1. <span data-ttu-id="b6594-188">Jeśli nie są wymagane dodatkowe funkcje, wybierz opcję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="b6594-188">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="b6594-189">Wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="b6594-189">Select **Install**.</span></span>
1. <span data-ttu-id="b6594-190">Po zakończeniu instalacji wybierz **Zamknij** aby zakończyć pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="b6594-190">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="b6594-191">Aby włączyć obsługę protokołu WebSocket w systemie Windows 8 lub nowszy:</span><span class="sxs-lookup"><span data-stu-id="b6594-191">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="b6594-192">Te kroki nie są wymagane w przypadku korzystania z usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="b6594-192">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="b6594-193">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="b6594-193">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="b6594-194">Otwórz następujące węzły: **Internetowe usługi informacyjne** > **usługi World Wide Web** > **funkcje tworzenia aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="b6594-194">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="b6594-195">Wybierz **protokołu WebSocket** funkcji.</span><span class="sxs-lookup"><span data-stu-id="b6594-195">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="b6594-196">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6594-196">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="b6594-197">Wyłącz WebSocket, gdy na języku Node.js przy użyciu biblioteki socket.io</span><span class="sxs-lookup"><span data-stu-id="b6594-197">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="b6594-198">Jeśli dzięki obsłudze protokołu WebSocket w [biblioteki socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), Wyłącz domyślne IIS WebSocket modułu przy użyciu `webSocket` element *web.config* lub *applicationHost.config*. Jeśli ten krok nie jest wykonywana, Moduł IIS WebSocket próbuje obsługi komunikacji protokołu WebSocket, a nie środowiska Node.js i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6594-198">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="b6594-199">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b6594-199">Next steps</span></span>

<span data-ttu-id="b6594-200">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) którego zostanie dołączony, ten artykuł stanowi app echa.</span><span class="sxs-lookup"><span data-stu-id="b6594-200">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="b6594-201">Posiada strony sieci web, która sprawia, że połączeń protokołu WebSocket i wszelkich komunikatów odebranych wysyła ponownie serwer do klienta.</span><span class="sxs-lookup"><span data-stu-id="b6594-201">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="b6594-202">Uruchamianie aplikacji z poziomu wiersza polecenia (go nie skonfigurowała do uruchamiania w programie Visual Studio z programem IIS Express) i przejdź do http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="b6594-202">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="b6594-203">Strony sieci web pokazuje stan połączenia w lewym górnym rogu:</span><span class="sxs-lookup"><span data-stu-id="b6594-203">The web page shows the connection status in the upper left:</span></span>

![Początkowy stan strony sieci web](websockets/_static/start.png)

<span data-ttu-id="b6594-205">Wybierz **Connect** wysyłać żądanie protokołu WebSocket adres URL wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="b6594-205">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="b6594-206">Wprowadź wiadomość testową, a następnie wybierz pozycję **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="b6594-206">Enter a test message and select **Send**.</span></span> <span data-ttu-id="b6594-207">Po zakończeniu wybierz pozycję **Zamknij gniazda**.</span><span class="sxs-lookup"><span data-stu-id="b6594-207">When done, select **Close Socket**.</span></span> <span data-ttu-id="b6594-208">**Dziennika komunikacji** sekcji raporty każdy otwarty, wysyłanie i zamknij akcji, jak to się dzieje.</span><span class="sxs-lookup"><span data-stu-id="b6594-208">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Początkowy stan strony sieci web](websockets/_static/end.png)
