---
title: "Obsługę protokołu WebSockets w ASP.NET Core"
author: tdykstra
description: "Dowiedz się, jak rozpocząć pracę z Websocket w ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="4b04e-103">Wprowadzenie do protokołu WebSockets w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4b04e-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="4b04e-104">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Andrew Stanton pielęgniarki](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="4b04e-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="4b04e-105">W tym artykule wyjaśniono, jak rozpocząć pracę z Websocket w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4b04e-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="4b04e-106">[Protokół WebSocket](https://wikipedia.org/wiki/WebSocket) jest to protokół umożliwiający kanały dwukierunkowej komunikacji trwałego za pośrednictwem połączeń TCP.</span><span class="sxs-lookup"><span data-stu-id="4b04e-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="4b04e-107">Służy do aplikacji, takich jak rozmowa, giełdowych, gier, dowolnym miejscu w czasie rzeczywistym funkcji w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="4b04e-107">It's used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="4b04e-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4b04e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="4b04e-109">Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4b04e-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="4b04e-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="4b04e-110">Prerequisites</span></span>

* <span data-ttu-id="4b04e-111">Platformy ASP.NET Core 1.1 (nie działa w wersji 1.0)</span><span class="sxs-lookup"><span data-stu-id="4b04e-111">ASP.NET Core 1.1 (doesn't run on 1.0)</span></span>
* <span data-ttu-id="4b04e-112">Dowolnego systemu operacyjnego, platformy ASP.NET Core uruchamianego na:</span><span class="sxs-lookup"><span data-stu-id="4b04e-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="4b04e-113">Windows 7 / Windows Server 2008 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="4b04e-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="4b04e-114">Linux</span><span class="sxs-lookup"><span data-stu-id="4b04e-114">Linux</span></span>
  * <span data-ttu-id="4b04e-115">macOS</span><span class="sxs-lookup"><span data-stu-id="4b04e-115">macOS</span></span>

* <span data-ttu-id="4b04e-116">**Wyjątek**: Jeśli aplikacja będzie działać w systemie Windows z programem IIS, lub z WebListener, należy użyć:</span><span class="sxs-lookup"><span data-stu-id="4b04e-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="4b04e-117">Windows 8 / Windows Server 2012 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="4b04e-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="4b04e-118">Usługi IIS 8 / 8 usług IIS Express</span><span class="sxs-lookup"><span data-stu-id="4b04e-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="4b04e-119">Protokół WebSocket musi być włączona w programie IIS</span><span class="sxs-lookup"><span data-stu-id="4b04e-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="4b04e-120">W przypadku obsługiwanych przeglądarek zobacz http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="4b04e-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="4b04e-121">Kiedy stosować</span><span class="sxs-lookup"><span data-stu-id="4b04e-121">When to use it</span></span>

<span data-ttu-id="4b04e-122">Za pomocą Websocket muszą pracować bezpośrednio z połączenia gniazda.</span><span class="sxs-lookup"><span data-stu-id="4b04e-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="4b04e-123">Na przykład może być konieczne najlepsze możliwe wydajności w czasie rzeczywistym gry.</span><span class="sxs-lookup"><span data-stu-id="4b04e-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="4b04e-124">[Biblioteka SignalR platformy ASP.NET](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) zapewnia na pełniejsze modelu aplikacji dla funkcji w czasie rzeczywistym, ale działa tylko na platformy ASP.NET, nie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4b04e-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="4b04e-125">Wersja SignalR Core jest w fazie projektowania; Postępuj zgodnie z jego postępu, zobacz [repozytorium GitHub dla SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="4b04e-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="4b04e-126">Jeśli nie chcesz czekać na SignalR Core służy protokół WebSockets bezpośrednio teraz.</span><span class="sxs-lookup"><span data-stu-id="4b04e-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="4b04e-127">Jednak może być konieczne tworzenie funkcji, które zapewni SignalR, na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b04e-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="4b04e-128">Obsługa większej liczby wersji przeglądarki przy użyciu automatycznego powrotu do metod alternatywnych transportu.</span><span class="sxs-lookup"><span data-stu-id="4b04e-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="4b04e-129">Automatyczne ponowne łączenie, kiedy obniży połączenia.</span><span class="sxs-lookup"><span data-stu-id="4b04e-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="4b04e-130">Obsługa klientów wywoływania metod, na serwerze lub na odwrót.</span><span class="sxs-lookup"><span data-stu-id="4b04e-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="4b04e-131">Obsługa skalowania na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="4b04e-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="4b04e-132">Jak z niego korzystać</span><span class="sxs-lookup"><span data-stu-id="4b04e-132">How to use it</span></span>

* <span data-ttu-id="4b04e-133">Zainstaluj [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="4b04e-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="4b04e-134">Konfigurowanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="4b04e-134">Configure the middleware.</span></span>
* <span data-ttu-id="4b04e-135">Zaakceptować żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4b04e-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="4b04e-136">Wysyłanie i odbieranie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="4b04e-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="4b04e-137">Konfigurowanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="4b04e-137">Configure the middleware</span></span>

<span data-ttu-id="4b04e-138">Dodaj oprogramowanie pośredniczące Websocket w `Configure` metody `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="4b04e-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="4b04e-139">Można skonfigurować następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="4b04e-139">The following settings can be configured:</span></span>

* <span data-ttu-id="4b04e-140">`KeepAliveInterval`-Porady często wysyłać ramki "ping" do klienta, aby upewnić się, że serwery proxy utrzymać otwarte połączenie.</span><span class="sxs-lookup"><span data-stu-id="4b04e-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="4b04e-141">`ReceiveBufferSize`-Rozmiar buforu używany do odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="4b04e-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="4b04e-142">Tylko użytkownicy zaawansowani musi zmieniać, dostrajania wydajności na podstawie rozmiaru ich danych.</span><span class="sxs-lookup"><span data-stu-id="4b04e-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="4b04e-143">Akceptować żądania protokołu WebSocket</span><span class="sxs-lookup"><span data-stu-id="4b04e-143">Accept WebSocket requests</span></span>

<span data-ttu-id="4b04e-144">Nowsze gdzieś w cyklu życia żądania (dalszej części `Configure` metody lub Akcja kontrolera MVC, na przykład) sprawdź, czy jest to żądanie protokołu WebSocket i zaakceptować żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4b04e-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="4b04e-145">W tym przykładzie pochodzi z później w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="4b04e-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="4b04e-146">Żądanie protokołu WebSocket może występować na dowolny adres URL, ale ten przykładowy kod akceptuje tylko żądania dotyczące `/ws`.</span><span class="sxs-lookup"><span data-stu-id="4b04e-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="4b04e-147">Wysyłanie i odbieranie wiadomości</span><span class="sxs-lookup"><span data-stu-id="4b04e-147">Send and receive messages</span></span>

<span data-ttu-id="4b04e-148">`AcceptWebSocketAsync` Metoda uaktualnia się przez połączenie obiektu WebSocket połączenie TCP i umożliwia [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) obiektu.</span><span class="sxs-lookup"><span data-stu-id="4b04e-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="4b04e-149">Obiekt WebSocket używany do wysyłania i odbierania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="4b04e-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="4b04e-150">Przekazuje kodu pokazano wcześniej, który akceptuje żądanie protokołu WebSocket `WebSocket` do obiektu `Echo` metody; w tym `Echo` metody.</span><span class="sxs-lookup"><span data-stu-id="4b04e-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="4b04e-151">Kod odbiera komunikat i natychmiast odsyła tę samą wiadomość.</span><span class="sxs-lookup"><span data-stu-id="4b04e-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="4b04e-152">Pozostaje on objęty w pętli, dopóki klient zamyka połączenie operacją.</span><span class="sxs-lookup"><span data-stu-id="4b04e-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="4b04e-153">Po zaakceptowaniu żądania WebSocket przed rozpoczęciem pętlę kończy się potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="4b04e-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="4b04e-154">Po zamknięciu gniazda, cofa potoku.</span><span class="sxs-lookup"><span data-stu-id="4b04e-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="4b04e-155">Oznacza to zatrzymuje żądania przeniesienie do przodu w potoku po zaakceptowaniu protokołu WebSocket, podobnie jak czy naciśnięcie Akcja kontrolera MVC, np.</span><span class="sxs-lookup"><span data-stu-id="4b04e-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="4b04e-156">Jednak po ukończeniu tej pętli i zamknąć gniazda, żądanie będzie kontynuowana, Utwórz kopię zapasową potoku.</span><span class="sxs-lookup"><span data-stu-id="4b04e-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b04e-157">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4b04e-157">Next steps</span></span>

<span data-ttu-id="4b04e-158">[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) dołączony ten artykuł jest aplikacją echo proste.</span><span class="sxs-lookup"><span data-stu-id="4b04e-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="4b04e-159">Ma strony sieci web, która umożliwia nawiązanie połączenia obiektu WebSocket, a serwer po prostu ponownie wysyła do klienta komunikaty, które otrzymuje.</span><span class="sxs-lookup"><span data-stu-id="4b04e-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="4b04e-160">Go uruchomić z wiersza polecenia (go nie skonfigurował do uruchomienia z programu Visual Studio z programem IIS Express) i przejdź do http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="4b04e-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="4b04e-161">Strony sieci web w lewym górnym rogu jest widoczny stan połączenia:</span><span class="sxs-lookup"><span data-stu-id="4b04e-161">The web page shows connection status at the upper left:</span></span>

![Początkowy stan strony sieci web](websockets/_static/start.png)

<span data-ttu-id="4b04e-163">Wybierz **Connect** wysłać żądania protokołu WebSocket do adres URL wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="4b04e-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="4b04e-164">Wprowadź wiadomość testową, a następnie wybierz **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="4b04e-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="4b04e-165">Po zakończeniu wybierz **Zamknij gniazda**.</span><span class="sxs-lookup"><span data-stu-id="4b04e-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="4b04e-166">**Dziennika komunikacji** sekcji raporty każdego otwarty, wysyłania i zamknij akcji w postaci, w jakiej się stanie.</span><span class="sxs-lookup"><span data-stu-id="4b04e-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Początkowy stan strony sieci web](websockets/_static/end.png)
