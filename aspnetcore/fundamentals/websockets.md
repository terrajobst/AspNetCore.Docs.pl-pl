---
title: Obsługę protokołu WebSockets w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z Websocket w ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/websockets
ms.openlocfilehash: ee529f1aaadb6b6062bed56003c51f161eae7e72
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273800"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="99f0e-103">Obsługę protokołu WebSockets w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="99f0e-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="99f0e-104">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Andrew Stanton pielęgniarki](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="99f0e-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="99f0e-105">W tym artykule wyjaśniono, jak rozpocząć pracę z Websocket w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99f0e-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="99f0e-106">[Protokół WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) to protokół, który umożliwia kanały dwukierunkowej komunikacji trwałego za pośrednictwem połączeń TCP.</span><span class="sxs-lookup"><span data-stu-id="99f0e-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="99f0e-107">Jest on używany w aplikacjach korzystających z komunikacji szybkiego, w czasie rzeczywistym, takich jak rozmowy, pulpit nawigacyjny i gier aplikacji.</span><span class="sxs-lookup"><span data-stu-id="99f0e-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="99f0e-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="99f0e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="99f0e-109">Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="99f0e-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99f0e-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="99f0e-110">Prerequisites</span></span>

* <span data-ttu-id="99f0e-111">Platformy ASP.NET Core 1.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="99f0e-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="99f0e-112">Dowolnego systemu operacyjnego, który obsługuje platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="99f0e-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="99f0e-113">Windows 7 / Windows Server 2008 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="99f0e-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="99f0e-114">Linux</span><span class="sxs-lookup"><span data-stu-id="99f0e-114">Linux</span></span>
  * <span data-ttu-id="99f0e-115">macOS</span><span class="sxs-lookup"><span data-stu-id="99f0e-115">macOS</span></span>
  
* <span data-ttu-id="99f0e-116">Jeśli aplikacja jest uruchamiana w systemie Windows z programem IIS:</span><span class="sxs-lookup"><span data-stu-id="99f0e-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="99f0e-117">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="99f0e-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="99f0e-118">Usługi IIS 8 / 8 usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="99f0e-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="99f0e-119">Protokół WebSockets musi być włączona w programie IIS (zobacz [Obsługa programu IIS/IIS Express](#iisiis-express-support) sekcji.)</span><span class="sxs-lookup"><span data-stu-id="99f0e-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="99f0e-120">Jeśli aplikacja jest uruchamiana [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="99f0e-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="99f0e-121">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="99f0e-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="99f0e-122">W przypadku obsługiwanych przeglądarek, zobacz https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="99f0e-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="99f0e-123">Kiedy należy używać protokołu WebSockets</span><span class="sxs-lookup"><span data-stu-id="99f0e-123">When to use WebSockets</span></span>

<span data-ttu-id="99f0e-124">Pracować bezpośrednio za pomocą połączenia gniazda za pomocą protokołu WebSockets.</span><span class="sxs-lookup"><span data-stu-id="99f0e-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="99f0e-125">Aby uzyskać optymalną wydajność w czasie rzeczywistym gry, na przykład użyć protokołu WebSockets.</span><span class="sxs-lookup"><span data-stu-id="99f0e-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="99f0e-126">[Biblioteka SignalR platformy ASP.NET](/aspnet/signalr/overview/getting-started/introduction-to-signalr) zapewnia bardziej rozbudowane model aplikacji dla funkcji w czasie rzeczywistym, ale działa tylko na platformie ASP.NET 4.x nie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99f0e-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer app model for real-time functionality, but it only runs on ASP.NET 4.x, not ASP.NET Core.</span></span> <span data-ttu-id="99f0e-127">Wersja platformy ASP.NET Core SignalR zaplanowano wydanie z platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="99f0e-127">An ASP.NET Core version of SignalR is scheduled for release with ASP.NET Core 2.1.</span></span> <span data-ttu-id="99f0e-128">Zobacz [platformy ASP.NET Core 2.1 wysokiego poziomu planowania](https://github.com/aspnet/Announcements/issues/288).</span><span class="sxs-lookup"><span data-stu-id="99f0e-128">See [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288).</span></span>

<span data-ttu-id="99f0e-129">Do czasu zwolnienia jest SignalR Core, można Websocket.</span><span class="sxs-lookup"><span data-stu-id="99f0e-129">Until SignalR Core is released, WebSockets can be used.</span></span> <span data-ttu-id="99f0e-130">Jednak funkcje, które zapewnia SignalR musi podać i obsługiwane przez dewelopera.</span><span class="sxs-lookup"><span data-stu-id="99f0e-130">However, features that SignalR provides must be provided and supported by the developer.</span></span> <span data-ttu-id="99f0e-131">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="99f0e-131">For example:</span></span>

* <span data-ttu-id="99f0e-132">Obsługa większej liczby wersji przeglądarki przy użyciu automatycznego powrotu do metod alternatywnych transportu.</span><span class="sxs-lookup"><span data-stu-id="99f0e-132">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="99f0e-133">Automatyczne ponowne łączenie, kiedy obniży połączenia.</span><span class="sxs-lookup"><span data-stu-id="99f0e-133">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="99f0e-134">Obsługa klientów wywoływania metod, na serwerze lub na odwrót.</span><span class="sxs-lookup"><span data-stu-id="99f0e-134">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="99f0e-135">Obsługa skalowania na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="99f0e-135">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="99f0e-136">Jak z niego korzystać</span><span class="sxs-lookup"><span data-stu-id="99f0e-136">How to use it</span></span>

* <span data-ttu-id="99f0e-137">Zainstaluj [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="99f0e-137">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="99f0e-138">Konfigurowanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="99f0e-138">Configure the middleware.</span></span>
* <span data-ttu-id="99f0e-139">Zaakceptować żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="99f0e-139">Accept WebSocket requests.</span></span>
* <span data-ttu-id="99f0e-140">Wysyłanie i odbieranie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="99f0e-140">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="99f0e-141">Konfigurowanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="99f0e-141">Configure the middleware</span></span>

<span data-ttu-id="99f0e-142">Dodaj oprogramowanie pośredniczące Websocket w `Configure` metody `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="99f0e-142">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="99f0e-143">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="99f0e-143">The following settings can be configured:</span></span>

* <span data-ttu-id="99f0e-144">`KeepAliveInterval` -Jak często wysyłać ramki "ping" do klienta, aby upewnić się, serwery proxy utrzymać otwarte połączenie.</span><span class="sxs-lookup"><span data-stu-id="99f0e-144">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="99f0e-145">`ReceiveBufferSize` -Rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="99f0e-145">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="99f0e-146">Użytkownicy wersji Advanced trzeba zmienić to dostrajania wydajności, zależnie od rozmiaru danych.</span><span class="sxs-lookup"><span data-stu-id="99f0e-146">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="99f0e-147">Akceptować żądania protokołu WebSocket</span><span class="sxs-lookup"><span data-stu-id="99f0e-147">Accept WebSocket requests</span></span>

<span data-ttu-id="99f0e-148">Nowsze gdzieś w cyklu życia żądania (dalszej części `Configure` metody lub Akcja kontrolera MVC, na przykład) sprawdź, czy jest to żądanie protokołu WebSocket i zaakceptować żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="99f0e-148">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="99f0e-149">Poniższy przykład pochodzi z później w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="99f0e-149">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="99f0e-150">Żądanie protokołu WebSocket może występować na dowolny adres URL, ale ten przykładowy kod akceptuje tylko żądania dotyczące `/ws`.</span><span class="sxs-lookup"><span data-stu-id="99f0e-150">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="99f0e-151">Wysyłanie i odbieranie wiadomości</span><span class="sxs-lookup"><span data-stu-id="99f0e-151">Send and receive messages</span></span>

<span data-ttu-id="99f0e-152">`AcceptWebSocketAsync` Metoda uaktualnia połączenie TCP do połączenia obiektu WebSocket i udostępnia [WebSocket](/dotnet/core/api/system.net.websockets.websocket) obiektu.</span><span class="sxs-lookup"><span data-stu-id="99f0e-152">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="99f0e-153">Użyj `WebSocket` obiekt do wysyłania i odbierania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="99f0e-153">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="99f0e-154">Przekazuje kodu pokazano wcześniej, który akceptuje żądanie protokołu WebSocket `WebSocket` do obiektu `Echo` metody.</span><span class="sxs-lookup"><span data-stu-id="99f0e-154">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="99f0e-155">Kod odbiera komunikat i natychmiast odsyła tę samą wiadomość.</span><span class="sxs-lookup"><span data-stu-id="99f0e-155">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="99f0e-156">Wiadomości wysyłanych i odbieranych w pętli, dopóki klient zamyka połączenie:</span><span class="sxs-lookup"><span data-stu-id="99f0e-156">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="99f0e-157">Akceptując połączenia obiektu WebSocket przed rozpoczęciem pętli kończy się potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="99f0e-157">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="99f0e-158">Po zamknięciu gniazda, cofa potoku.</span><span class="sxs-lookup"><span data-stu-id="99f0e-158">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="99f0e-159">Oznacza to żądanie zatrzymania przeniesienie do przodu w potoku po zaakceptowaniu żądania WebSocket.</span><span class="sxs-lookup"><span data-stu-id="99f0e-159">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="99f0e-160">Po zakończeniu pętli i gniazda jest zamknięty, żądanie będzie kontynuowana, Utwórz kopię zapasową potoku.</span><span class="sxs-lookup"><span data-stu-id="99f0e-160">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="99f0e-161">Obsługa programu IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="99f0e-161">IIS/IIS Express support</span></span>

<span data-ttu-id="99f0e-162">Windows Server 2012 lub nowszym i Windows 8 lub nowszym z programu IIS/IIS Express 8 lub nowszy zawiera obsługę protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="99f0e-162">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="99f0e-163">Aby włączyć obsługę protokołu WebSocket w systemie Windows Server 2012 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="99f0e-163">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="99f0e-164">Użyj **Dodaj role i funkcje** kreatora z **Zarządzaj** menu lub link w **Menedżera serwera**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-164">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="99f0e-165">Wybierz **Instalacja roli lub funkcji**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-165">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="99f0e-166">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-166">Select **Next**.</span></span>
1. <span data-ttu-id="99f0e-167">Wybierz odpowiedni serwer (serwer lokalny jest domyślnie zaznaczona).</span><span class="sxs-lookup"><span data-stu-id="99f0e-167">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="99f0e-168">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-168">Select **Next**.</span></span>
1. <span data-ttu-id="99f0e-169">Rozwiń węzeł **serwer sieci Web (IIS)** w **ról** drzewa, a następnie rozwiń **serwera sieci Web**, a następnie rozwiń węzeł **projektowanie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-169">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="99f0e-170">Wybierz **protokół WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-170">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="99f0e-171">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-171">Select **Next**.</span></span>
1. <span data-ttu-id="99f0e-172">Jeśli nie są potrzebne dodatkowe funkcje, wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-172">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="99f0e-173">Wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-173">Select **Install**.</span></span>
1. <span data-ttu-id="99f0e-174">Po zakończeniu instalacji wybierz **Zamknij** aby zakończyć pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="99f0e-174">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="99f0e-175">Aby włączyć obsługę protokołu WebSocket w systemie Windows 8 lub nowszy:</span><span class="sxs-lookup"><span data-stu-id="99f0e-175">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="99f0e-176">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="99f0e-176">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="99f0e-177">Otwórz następujące węzły: **Internetowe usługi informacyjne** > **usługi sieci World Wide Web** > **funkcje tworzenia aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-177">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="99f0e-178">Wybierz **protokół WebSocket** funkcji.</span><span class="sxs-lookup"><span data-stu-id="99f0e-178">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="99f0e-179">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-179">Select **OK**.</span></span>

<span data-ttu-id="99f0e-180">**Wyłączanie protokołu WebSocket za pomocą użyciu biblioteki socket.io node.js**</span><span class="sxs-lookup"><span data-stu-id="99f0e-180">**Disable WebSocket when using socket.io on node.js**</span></span>

<span data-ttu-id="99f0e-181">Jeśli przy użyciu obsługi protokołu WebSocket w [użyciu biblioteki socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), wyłącz użyciu moduł WebSocket usług IIS domyślne `webSocket` element w *web.config* lub *applicationHost.config*. Jeśli ten krok nie jest wykonywana, moduł WebSocket usług IIS próbuje obsługi komunikacji protokołu WebSocket zamiast Node.js i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="99f0e-181">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="99f0e-182">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="99f0e-182">Next steps</span></span>

<span data-ttu-id="99f0e-183">[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) dołączony ten artykuł jest aplikacją echo.</span><span class="sxs-lookup"><span data-stu-id="99f0e-183">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="99f0e-184">Istnieją strony sieci web, która umożliwia nawiązanie połączenia obiektu WebSocket i ponownie serwer wysyła komunikaty, które otrzymuje do klienta.</span><span class="sxs-lookup"><span data-stu-id="99f0e-184">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="99f0e-185">Uruchamianie aplikacji z wiersza polecenia (go nie skonfigurował do uruchomienia z programu Visual Studio z programem IIS Express) i przejdź do http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="99f0e-185">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="99f0e-186">Strony sieci web w lewym górnym rogu jest widoczny stan połączenia:</span><span class="sxs-lookup"><span data-stu-id="99f0e-186">The web page shows the connection status in the upper left:</span></span>

![Początkowy stan strony sieci web](websockets/_static/start.png)

<span data-ttu-id="99f0e-188">Wybierz **Connect** wysłać żądania protokołu WebSocket do adres URL wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="99f0e-188">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="99f0e-189">Wprowadź wiadomość testową, a następnie wybierz **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-189">Enter a test message and select **Send**.</span></span> <span data-ttu-id="99f0e-190">Po zakończeniu wybierz **Zamknij gniazda**.</span><span class="sxs-lookup"><span data-stu-id="99f0e-190">When done, select **Close Socket**.</span></span> <span data-ttu-id="99f0e-191">**Dziennika komunikacji** sekcji raporty każdego otwarty, wysyłania i zamknij akcji w postaci, w jakiej się stanie.</span><span class="sxs-lookup"><span data-stu-id="99f0e-191">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Początkowy stan strony sieci web](websockets/_static/end.png)
