---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: Podręcznik interfejsu API centrów SignalR platformy ASP.NET — klient modelu .NET (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 2 w przypadku klientów programu .NET, takich jak Windows Store (WinRT), WPF, Silverlight i wad...
ms.author: riande
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 5889429645ea1c682ea43c4b17afb3745318e32d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757344"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="0fc1b-103">Podręcznik interfejsu API centrów SignalR platformy ASP.NET — klient modelu .NET (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="0fc1b-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>
====================
<span data-ttu-id="0fc1b-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0fc1b-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="0fc1b-105">Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 2 w przypadku klientów programu .NET, takich jak Windows Store (WinRT), WPF, Silverlight oraz aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="0fc1b-106">Interfejsu API centrów SignalR umożliwia zdalne wywołania procedur (RPC) z serwera do połączonych klientów i od klientów z serwerem.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="0fc1b-107">W kodzie serwera należy zdefiniować metody, które mogą być wywoływane przez klientów, a wywołanie metody, które są uruchamiane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="0fc1b-108">W kodzie klienta definiowania metod, które mogą być wywoływane z serwera, a wywołanie metody, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="0fc1b-109">SignalR zajmuje się wszystkie nadmiar klient serwer dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="0fc1b-110">SignalR oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="0fc1b-111">Wprowadzenie do SignalR, centra i połączenia trwałego lub samouczek, w którym przedstawiono sposób tworzenia kompletnej aplikacji SignalR, patrz [SignalR — wprowadzenie](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="0fc1b-112">Omówienie</span><span class="sxs-lookup"><span data-stu-id="0fc1b-112">Overview</span></span>

<span data-ttu-id="0fc1b-113">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="0fc1b-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="0fc1b-114">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="0fc1b-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="0fc1b-115">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="0fc1b-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="0fc1b-116">Połączenia między domenami od klientów programu Silverlight</span><span class="sxs-lookup"><span data-stu-id="0fc1b-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="0fc1b-117">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="0fc1b-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="0fc1b-118">Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF</span><span class="sxs-lookup"><span data-stu-id="0fc1b-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="0fc1b-119">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="0fc1b-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="0fc1b-120">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="0fc1b-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="0fc1b-121">Jak określić nagłówki HTTP</span><span class="sxs-lookup"><span data-stu-id="0fc1b-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="0fc1b-122">Jak określić certyfikaty klienta</span><span class="sxs-lookup"><span data-stu-id="0fc1b-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="0fc1b-123">Jak utworzyć serwer proxy koncentratora</span><span class="sxs-lookup"><span data-stu-id="0fc1b-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="0fc1b-124">Sposób definiowania metody na kliencie, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="0fc1b-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="0fc1b-125">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="0fc1b-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="0fc1b-126">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="0fc1b-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="0fc1b-127">Metody z parametrami, określając obiektów dynamicznych parametrów</span><span class="sxs-lookup"><span data-stu-id="0fc1b-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="0fc1b-128">Jak usunąć program obsługi</span><span class="sxs-lookup"><span data-stu-id="0fc1b-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="0fc1b-129">Jak wywołać metody serwera z poziomu klienta</span><span class="sxs-lookup"><span data-stu-id="0fc1b-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="0fc1b-130">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="0fc1b-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="0fc1b-131">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="0fc1b-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="0fc1b-132">Włączanie rejestrowania po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="0fc1b-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="0fc1b-133">WPF, Silverlight oraz przykłady kodu aplikacji konsoli dla metod klienta, które można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="0fc1b-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="0fc1b-134">Przykładowych projektach klientów platformy .NET na ten temat można znaleźć w następujących zasobach:</span><span class="sxs-lookup"><span data-stu-id="0fc1b-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="0fc1b-135">[gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) w witrynie GitHub.com (przykłady aplikacji konsoli WinRT, Silverlight,).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="0fc1b-136">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) w witrynie GitHub.com (np. WPF).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="0fc1b-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) w witrynie GitHub.com (np. aplikację konsoli).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="0fc1b-138">Dokumentacja na temat programu server lub klientów języka JavaScript, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="0fc1b-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="0fc1b-139">Podręcznik interfejsu API centrów SignalR — serwer</span><span class="sxs-lookup"><span data-stu-id="0fc1b-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="0fc1b-140">Podręcznik interfejsu API centrów SignalR — klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="0fc1b-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="0fc1b-141">Łącza do tematów, dokumentacja interfejsu API są .NET 4.5 wersję interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="0fc1b-142">Jeśli używasz programu .NET 4, zobacz [tematy interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="0fc1b-143">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="0fc1b-143">Client setup</span></span>

<span data-ttu-id="0fc1b-144">Zainstaluj [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pakietu NuGet (nie [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) pakietu).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="0fc1b-145">Ten pakiet obsługuje WinRT, Silverlight, WPF, aplikacji konsoli i klienci Windows Phone, zarówno dla platformy .NET 4 i .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="0fc1b-146">Jeśli wersji biblioteki SignalR, że masz na komputerze klienckim jest inna niż wersja, która ma na serwerze, SignalR jest zazwyczaj możliwość dostosowania różnicy.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="0fc1b-147">Na przykład po wydaniu SignalR w wersji 2.0 i można zainstalować na serwerze, serwer obsługuje klientów, którzy mają 1.1.x zainstalowane, a także klientów, którzy mają zainstalowany w wersji 2.0.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="0fc1b-148">Jeśli różnica między wersją na serwerze i wersji na komputerze klienckim jest zbyt duża, SignalR zgłasza `InvalidOperationException` wyjątku, gdy klient próbuje nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="0fc1b-149">Komunikat o błędzie "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="0fc1b-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="0fc1b-150">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="0fc1b-150">How to establish a connection</span></span>

<span data-ttu-id="0fc1b-151">Zanim usługa może nawiązać połączenie, należy utworzyć `HubConnection` obiektu i tworzyć serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="0fc1b-152">Aby nawiązać połączenie, należy wywołać `Start` metody `HubConnection` obiektu.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="0fc1b-153">W przypadku klientów JavaScript należy zarejestrować co najmniej jeden program obsługi zdarzeń przed wywołaniem `Start` metodę, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="0fc1b-154">To nie jest wymagane dla klientów programu .NET.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="0fc1b-155">Dla klientów JavaScript wygenerowany serwer proxy automatycznie tworzy kod serwery proxy do wszystkich centrów znajdujące się na serwerze i rejestrowania procedury obsługi jest wskazuje, które koncentratorów klienta zamierza użyć.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="0fc1b-156">Ale dla klienta programu .NET utworzyć serwery proxy Centrum ręcznie, więc SignalR przyjęto założenie, że będziesz używać dowolne utworzonego serwera proxy dla Centrum.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="0fc1b-157">Przykładowy kod używa domyślnej "/ signalr" adres URL, aby nawiązać połączenie z usługą SignalR.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="0fc1b-158">Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="0fc1b-159">`Start` Metoda jest wykonywana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="0fc1b-160">Aby upewnić się, że kolejne wiersze kodu nie wykonany dopóki, po nawiązaniu połączenia, należy użyć `await` w metodzie asynchronicznej, ASP.NET 4.5 lub `.Wait()` w metodzie synchronicznej.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="0fc1b-161">Nie używaj `.Wait()` w kliencie WinRT.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="0fc1b-162">`HubConnection` Klasa jest metodą o bezpiecznych wątkach.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="0fc1b-163">Połączenia między domenami od klientów programu Silverlight</span><span class="sxs-lookup"><span data-stu-id="0fc1b-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="0fc1b-164">Aby uzyskać informacje o sposobie włączania połączeń między domenami od klientów programu Silverlight, zobacz [wprowadzania Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="0fc1b-165">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="0fc1b-165">How to configure the connection</span></span>

<span data-ttu-id="0fc1b-166">Przed nawiązaniem połączenia, można określić dowolną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="0fc1b-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="0fc1b-167">Limit połączeń współbieżnych.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="0fc1b-168">Parametry ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-168">Query string parameters.</span></span>
- <span data-ttu-id="0fc1b-169">Metoda transportu.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-169">The transport method.</span></span>
- <span data-ttu-id="0fc1b-170">Nagłówki HTTP.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-170">HTTP headers.</span></span>
- <span data-ttu-id="0fc1b-171">Certyfikaty klienta.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="0fc1b-172">Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF</span><span class="sxs-lookup"><span data-stu-id="0fc1b-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="0fc1b-173">W klientach programu WPF trzeba zwiększyć maksymalną liczbę równoczesnych połączeń od jego wartość domyślną 2.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="0fc1b-174">Zalecana wartość wynosi 10.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="0fc1b-175">Aby uzyskać więcej informacji, zobacz [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="0fc1b-176">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="0fc1b-176">How to specify query string parameters</span></span>

<span data-ttu-id="0fc1b-177">Jeśli chcesz wysyłać dane do serwera, gdy klient nawiąże połączenie, można dodać parametry ciągu zapytania do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="0fc1b-178">Poniższy przykład pokazuje, jak ustawić parametr ciągu zapytania w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="0fc1b-179">Poniższy przykład pokazuje, jak odczytywać parametr ciągu zapytania w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="0fc1b-180">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="0fc1b-180">How to specify the transport method</span></span>

<span data-ttu-id="0fc1b-181">Częścią procesu nawiązywania połączenia klienta SignalR zwykle negocjuje z serwerem, aby określić najlepsze transportu, która jest obsługiwana przez serwer i klienta.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="0fc1b-182">Jeśli znasz już transport, którego chcesz użyć, można pominąć ten proces negocjacji.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="0fc1b-183">Aby określić metodę transportu, należy przekazać w obiekcie transportu do metody uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="0fc1b-184">Poniższy przykład pokazuje, jak określić metodę transportu w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="0fc1b-185">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) przestrzeń nazw zawiera następujące klasy, używanych do określania transportu.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="0fc1b-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="0fc1b-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="0fc1b-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="0fc1b-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="0fc1b-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (dostępne tylko wtedy, gdy serwera i klienta użyj .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="0fc1b-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (automatycznie wybiera najlepsze transportu, która jest obsługiwana zarówno przez klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="0fc1b-190">Jest to domyślny transport.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-190">This is the default transport.</span></span> <span data-ttu-id="0fc1b-191">Te informacje w celu przekazywania `Start` metoda ma taki sam skutek jak nie przechodzi coś.)</span><span class="sxs-lookup"><span data-stu-id="0fc1b-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="0fc1b-192">ForeverFrame transport jest niedostępna na tej liście, ponieważ jest używany tylko przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="0fc1b-193">Aby uzyskać informacje o sposobie Sprawdź metodą transportu w kodzie serwera, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - sposób uzyskiwania informacji na temat klienta z właściwości kontekstu](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="0fc1b-194">Aby uzyskać więcej informacji dotyczących transportu i planów awaryjnych, zobacz [wprowadzenie do SignalR - transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="0fc1b-195">Jak określić nagłówki HTTP</span><span class="sxs-lookup"><span data-stu-id="0fc1b-195">How to specify HTTP headers</span></span>

<span data-ttu-id="0fc1b-196">Aby ustawić nagłówki HTTP, użyj `Headers` właściwości w obiekcie połączenia.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="0fc1b-197">Poniższy przykład pokazuje, jak dodać nagłówek HTTP.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="0fc1b-198">Jak określić certyfikaty klienta</span><span class="sxs-lookup"><span data-stu-id="0fc1b-198">How to specify client certificates</span></span>

<span data-ttu-id="0fc1b-199">Aby dodać certyfikaty klienta, należy użyć `AddClientCertificate` metody na obiekt połączenia.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="0fc1b-200">Jak utworzyć serwer proxy koncentratora</span><span class="sxs-lookup"><span data-stu-id="0fc1b-200">How to create the Hub proxy</span></span>

<span data-ttu-id="0fc1b-201">Aby zdefiniować metody dla klienta, który może wywołać koncentrator, z serwera i wywoływanie metod koncentratora na serwerze, należy utworzyć serwer proxy dla koncentratora przez wywołanie metody `CreateHubProxy` w obiekcie połączenia.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="0fc1b-202">Ciąg przekazanej do `CreateHubProxy` jest nazwą klasy koncentratora lub nazwa określona przez `HubName` atrybutu, jeśli użyto jednego na serwerze.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="0fc1b-203">Dopasowywaniu nazwy nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="0fc1b-204">**Klasy koncentratora na serwerze**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="0fc1b-205">**Utwórz serwer proxy klienta dla klasy koncentratora**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="0fc1b-206">Jeśli dekoracji klasy koncentratora z `HubName` atrybutu, należy użyć tej nazwy.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="0fc1b-207">**Klasy koncentratora na serwerze**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="0fc1b-208">**Utwórz serwer proxy klienta dla klasy koncentratora**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="0fc1b-209">Obiekt serwera proxy jest metodą o bezpiecznych wątkach.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="0fc1b-210">W rzeczywistości, jeśli wywołasz `HubConnection.CreateHubProxy` wiele razy z takimi samymi `hubName`, możesz uzyskać takie same buforowane `IHubProxy` obiektu.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="0fc1b-211">Sposób definiowania metody na kliencie, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="0fc1b-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="0fc1b-212">Aby zdefiniować metodę, która może wywołać serwera, należy użyć serwera proxy `On` metodę, aby zarejestrować program obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="0fc1b-213">Dopasowywaniu nazwy metody nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="0fc1b-214">Na przykład `Clients.All.UpdateStockPrice` będą wykonywane na serwerze `updateStockPrice`, `updatestockprice`, lub `UpdateStockPrice` na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="0fc1b-215">Innego klienta platformy mają różne wymagania dotyczące sposobu pisania kodu metody, aby zaktualizować interfejs użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="0fc1b-216">Przykłady zamieszczone dotyczą klientów WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="0fc1b-217">WPF, Silverlight oraz przykłady aplikacji konsoli znajdują się w [osobnej sekcji w dalszej części tego tematu](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="0fc1b-218">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="0fc1b-218">Methods without parameters</span></span>

<span data-ttu-id="0fc1b-219">Jeśli metoda jest obsługa nie ma parametrów, użyj przeciążenia nieogólnego `On` metody:</span><span class="sxs-lookup"><span data-stu-id="0fc1b-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="0fc1b-220">**Kod serwera podczas wywoływania metody klienta bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="0fc1b-221">**Kod klienta WinRT metody wywoływane z serwera bez parametrów ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="0fc1b-222">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="0fc1b-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="0fc1b-223">Jeśli metoda jest obsługa ma parametry, należy określić typy parametrów jako typów ogólnego `On` metody.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="0fc1b-224">Istnieją przeciążenia ogólne `On` metody umożliwiają określenie parametrów do 8 (4 w systemie Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="0fc1b-225">W poniższym przykładzie jeden parametr jest wysyłane do `UpdateStockPrice` metody.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="0fc1b-226">**Kod serwera podczas wywoływania metody klienta z parametrem**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="0fc1b-227">**Klasy zapasów użytym dla parametru**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="0fc1b-228">**Kod klienta WinRT dla metody wywoływane z serwera za pomocą parametru ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="0fc1b-229">Metody z parametrami, określając obiektów dynamicznych parametrów</span><span class="sxs-lookup"><span data-stu-id="0fc1b-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="0fc1b-230">Jako alternatywę do określania parametrów jako typów ogólnego `On` metody, parametry można określić jako obiektów dynamicznych:</span><span class="sxs-lookup"><span data-stu-id="0fc1b-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="0fc1b-231">**Kod serwera podczas wywoływania metody klienta z parametrem**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="0fc1b-232">**Klasy zapasów użytym dla parametru**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="0fc1b-233">**Kod klienta WinRT dla metody wywoływane z serwera z parametrem przy użyciu obiekt dynamiczny parametr ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="0fc1b-234">Jak usunąć program obsługi</span><span class="sxs-lookup"><span data-stu-id="0fc1b-234">How to remove a handler</span></span>

<span data-ttu-id="0fc1b-235">Aby usunąć program obsługi, należy wywołać jej `Dispose` metody.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="0fc1b-236">**Kod klienta dla metodę o nazwie z serwera**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="0fc1b-237">**Kod klienta, aby usunąć program obsługi**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="0fc1b-238">Jak wywołać metody serwera z poziomu klienta</span><span class="sxs-lookup"><span data-stu-id="0fc1b-238">How to call server methods from the client</span></span>

<span data-ttu-id="0fc1b-239">Aby wywołać metodę na serwerze, należy użyć `Invoke` metody serwera proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="0fc1b-240">Jeśli metoda nie zwraca żadnej wartości, użyj przeciążenia nieogólnego `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="0fc1b-241">**Kod serwera dla metody, która nie zwraca żadnej wartości**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="0fc1b-242">**Kod klienta, wywołanie metody, która nie zwraca żadnej wartości**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="0fc1b-243">Jeśli metoda nie zwraca wartości, należy określić typ zwracany jako ogólny typ `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="0fc1b-244">**Kod serwera dla metody, która nie zwraca wartości i przyjmuje parametr typu złożonego**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="0fc1b-245">**Klasy zapasów użytym dla parametru i zwracają wartość**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="0fc1b-246">**Kod klienta, wywołanie metody, która nie zwraca wartości i przyjmuje parametr typu złożonego w metodzie asynchronicznej ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="0fc1b-247">**Kod klienta, wywołanie metody, która nie zwraca wartości i przyjmuje parametr typu złożonego, metoda synchroniczna**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="0fc1b-248">`Invoke` Metoda wykonuje asynchronicznie i zwraca `Task` obiektu.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="0fc1b-249">Jeśli nie określisz `await` lub `.Wait()`, następnego wiersza kodu zostaną wykonane, zanim metodę, która wywołuje zakończenia.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="0fc1b-250">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="0fc1b-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="0fc1b-251">Biblioteka SignalR udostępnia następujące połączenia zdarzenia okresu istnienia, które może obsłużyć:</span><span class="sxs-lookup"><span data-stu-id="0fc1b-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="0fc1b-252">`Received`: Zgłaszane w przypadku nieodebrania żadnych danych w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="0fc1b-253">Udostępnia odebrane dane.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-253">Provides the received data.</span></span>
- <span data-ttu-id="0fc1b-254">`ConnectionSlow`: Wywoływane, gdy klient wykrywa wolno lub często porzucanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="0fc1b-255">`Reconnecting`: Wywoływane, gdy transportu źródłowego rozpoczyna ponowne nawiązywanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="0fc1b-256">`Reconnected`: Wywoływane, gdy nawiązał transportu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="0fc1b-257">`StateChanged`: Wywoływane, gdy zmieni się stan połączenia.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="0fc1b-258">Zawiera stan stary i nowy stan.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-258">Provides the old state and the new state.</span></span> <span data-ttu-id="0fc1b-259">Aby uzyskać informacje na temat połączenia wartości stanu zobacz [wyliczenie Element ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="0fc1b-260">`Closed`: Wywoływane, gdy połączenie zostało rozłączone.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="0fc1b-261">Na przykład, jeśli chcesz wyświetlić komunikaty ostrzegawcze dotyczące błędów, które nie są one krytyczny, ale powodują sporadyczne problemy z połączeniem, takie jak powolność lub zbyt częstej porzucenie połączenia, obsługi `ConnectionSlow` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="0fc1b-262">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia w SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="0fc1b-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="0fc1b-263">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="0fc1b-263">How to handle errors</span></span>

<span data-ttu-id="0fc1b-264">Jeśli nie włączysz jawnie szczegółowe komunikaty o błędach na serwerze, obiekt wyjątku, który zwraca SignalR, po wystąpieniu błędu zawiera minimalne informacje o tym błędzie.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="0fc1b-265">Na przykład, jeśli wywołanie `newContosoChatMessage` zawiera komunikat o błędzie w obiekcie błąd zakończy się niepowodzeniem, "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Wysyłanie szczegółowe komunikaty o błędach do klientów w środowisku produkcyjnym jest niezalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach dla rozwiązywania problemów, użyj poniższego kodu, na serwerze.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="0fc1b-266">Do obsługi błędów, które wywołuje SignalR, można dodać program obsługi `Error` zdarzenie na obiekt połączenia.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="0fc1b-267">Do obsługi błędów z wywołań metod, zawijania kodu w bloku try-catch.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="0fc1b-268">Włączanie rejestrowania po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="0fc1b-268">How to enable client-side logging</span></span>

<span data-ttu-id="0fc1b-269">Aby włączyć rejestrowanie klienta, należy ustawić `TraceLevel` i `TraceWriter` właściwości w obiekcie połączenia.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="0fc1b-270">WPF, Silverlight oraz przykłady kodu aplikacji konsoli dla metod klienta, które można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="0fc1b-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="0fc1b-271">Przykłady kodu, w przedstawionej wcześniej do definiowania metody klientów, które serwer może wywoływać dotyczą klientów WinRT.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="0fc1b-272">Poniższe przykłady pokazują równoważny kod dla WPF, Silverlight oraz klientów aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="0fc1b-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="0fc1b-273">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="0fc1b-273">Methods without parameters</span></span>

<span data-ttu-id="0fc1b-274">**WPF kodu klienta dla metodę o nazwie z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="0fc1b-275">**Kod klienta Silverlight dla metodę o nazwie z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="0fc1b-276">**Kod klienta aplikacji konsolowej, metoda jest wywoływana z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="0fc1b-277">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="0fc1b-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="0fc1b-278">**WPF kodu klienta dla metodę o nazwie z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="0fc1b-279">**Kod klienta Silverlight dla metodę o nazwie z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="0fc1b-280">**Kod klienta aplikacji konsolowej, metoda jest wywoływana z serwera za pomocą parametru**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="0fc1b-281">Metody z parametrami, określając obiektów dynamicznych parametrów</span><span class="sxs-lookup"><span data-stu-id="0fc1b-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="0fc1b-282">**WPF kodu klienta dla metodę o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny parametr**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="0fc1b-283">**Kod klienta Silverlight dla metodę o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny parametr**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="0fc1b-284">**Kod klienta aplikacji konsoli dla metody wywoływane z serwera z parametrem przy użyciu obiekt dynamiczny parametr**</span><span class="sxs-lookup"><span data-stu-id="0fc1b-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
