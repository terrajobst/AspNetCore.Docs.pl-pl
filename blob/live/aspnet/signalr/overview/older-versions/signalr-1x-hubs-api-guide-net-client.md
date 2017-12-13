---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: "Podręcznik interfejsu API koncentratorów SignalR platformy ASP.NET — klienta .NET (SignalR 1.x) | Dokumentacja firmy Microsoft"
author: pfletcher
description: "Ten dokument zawiera wprowadzenie do korzystania z interfejsu API koncentratorów dla biblioteki SignalR w wersji 2 w klientów platformy .NET, takich jak Sklep Windows (WinRT), WPF, Silverlight i wad..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9cf99ba7887e7db847097a63c0a964ef5d461a9d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="884de-103">Podręcznik interfejsu API koncentratorów SignalR platformy ASP.NET — klienta .NET (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="884de-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>
====================
<span data-ttu-id="884de-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="884de-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="884de-105">Ten dokument zawiera wprowadzenie do korzystania z interfejsu API koncentratorów dla biblioteki SignalR w wersji 2 w klientów platformy .NET, takie jak magazynu systemu Windows (WinRT), WPF i Silverlight oraz aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="884de-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="884de-106">Interfejs API koncentratorów SignalR umożliwia upewnij zdalnych wywołań procedur (RPC) z serwera do połączonych klientów i klientów na serwerze.</span><span class="sxs-lookup"><span data-stu-id="884de-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="884de-107">W kodzie serwera zdefiniuj metody, które mogą być wywoływane przez klientów i wywołania metod, które są uruchamiane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="884de-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="884de-108">W kodu klienta należy zdefiniować metody, które mogą być wywoływane z serwera i wywołania metody, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="884de-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="884de-109">SignalR odpowiada on za wszystkie żmudne procesy klient serwer dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="884de-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="884de-110">SignalR udostępnia również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="884de-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="884de-111">Wprowadzenie do SignalR, koncentratorach i połączeń trwałych lub Samouczek przedstawiający sposób tworzenia pełnej aplikacji SignalR, patrz [SignalR — wprowadzenie](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="884de-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="884de-112">Omówienie</span><span class="sxs-lookup"><span data-stu-id="884de-112">Overview</span></span>

<span data-ttu-id="884de-113">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="884de-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="884de-114">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="884de-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="884de-115">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="884de-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="884de-116">Między domenami połączenia od klientów programu Silverlight</span><span class="sxs-lookup"><span data-stu-id="884de-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="884de-117">Jak skonfigurować połączenia</span><span class="sxs-lookup"><span data-stu-id="884de-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="884de-118">Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF</span><span class="sxs-lookup"><span data-stu-id="884de-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="884de-119">Jak określać parametrów ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="884de-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="884de-120">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="884de-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="884de-121">Jak określić nagłówków HTTP</span><span class="sxs-lookup"><span data-stu-id="884de-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="884de-122">Jak określić certyfikaty klienta</span><span class="sxs-lookup"><span data-stu-id="884de-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="884de-123">Tworzenie serwera proxy koncentratora</span><span class="sxs-lookup"><span data-stu-id="884de-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="884de-124">Sposób definiowania metod na komputerze klienckim, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="884de-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="884de-125">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="884de-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="884de-126">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="884de-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="884de-127">Metody z parametrami, określając obiekty dynamiczne parametrów</span><span class="sxs-lookup"><span data-stu-id="884de-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="884de-128">Jak usunąć program obsługi</span><span class="sxs-lookup"><span data-stu-id="884de-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="884de-129">Sposób wywołania metody serwera z klienta</span><span class="sxs-lookup"><span data-stu-id="884de-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="884de-130">Sposób obsługi zdarzeń okres istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="884de-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="884de-131">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="884de-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="884de-132">Jak włączyć rejestrowanie klienta</span><span class="sxs-lookup"><span data-stu-id="884de-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="884de-133">WPF, Silverlight i przykładów kodu aplikacji konsoli dla metod klienta, które może wywoływać serwera</span><span class="sxs-lookup"><span data-stu-id="884de-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="884de-134">Przykładowych projektach klienta .NET na ten temat można znaleźć w następujących zasobach:</span><span class="sxs-lookup"><span data-stu-id="884de-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="884de-135">[Gustaw armenta / SignalR próbek](https://github.com/gustavo-armenta/SignalR-Samples) w witrynie GitHub.com (przykłady aplikacji konsoli WinRT, Silverlight,).</span><span class="sxs-lookup"><span data-stu-id="884de-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="884de-136">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) w witrynie GitHub.com (przykład WPF).</span><span class="sxs-lookup"><span data-stu-id="884de-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="884de-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) w witrynie GitHub.com (przykład aplikacji konsoli).</span><span class="sxs-lookup"><span data-stu-id="884de-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="884de-138">Dokumentacja na temat programu server lub klientów języka JavaScript, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="884de-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="884de-139">Podręcznik interfejsu API koncentratorów SignalR — serwer</span><span class="sxs-lookup"><span data-stu-id="884de-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="884de-140">Podręcznik interfejsu API koncentratorów SignalR — JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="884de-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="884de-141">Linki do tematów dokumentacji interfejsu API są wersja platformy .NET 4.5 interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="884de-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="884de-142">Jeśli używasz programu .NET 4, zobacz [wersji .NET 4 tematy interfejsu API](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="884de-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="884de-143">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="884de-143">Client setup</span></span>

<span data-ttu-id="884de-144">Zainstaluj [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pakietu NuGet (nie [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) pakietu).</span><span class="sxs-lookup"><span data-stu-id="884de-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="884de-145">Ten pakiet obsługuje WinRT, Silverlight, WPF, aplikacji konsoli i klienci Windows Phone dla platformy .NET 4.5 i .NET 4.</span><span class="sxs-lookup"><span data-stu-id="884de-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="884de-146">Wersja SignalR zainstalowanego na komputerze klienckim jest inna niż wersja zainstalowanego na serwerze, SignalR jest często możliwość dostosowania różnicy.</span><span class="sxs-lookup"><span data-stu-id="884de-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="884de-147">Na przykład po zwolnieniu SignalR w wersji 2.0 i należy zainstalować na serwerze, serwer obsługuje klientów, którzy mają zainstalowane, a także klientów, którzy mają zainstalowane 2.0 1.1.x.</span><span class="sxs-lookup"><span data-stu-id="884de-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="884de-148">Jeśli różnica między wersją na serwerze i wersja na komputerze klienckim jest zbyt duża, zgłasza SignalR `InvalidOperationException` wyjątku, gdy klient próbuje nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="884de-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="884de-149">Komunikat o błędzie "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="884de-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="884de-150">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="884de-150">How to establish a connection</span></span>

<span data-ttu-id="884de-151">Przed może nawiązać połączenie, należy utworzyć `HubConnection` obiektu i utworzyć serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="884de-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="884de-152">Aby nawiązać połączenie, należy wywołać `Start` metoda `HubConnection` obiektu.</span><span class="sxs-lookup"><span data-stu-id="884de-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="884de-153">W przypadku klientów JavaScript należy zarejestrować co najmniej jeden program obsługi zdarzeń przed wywołaniem `Start` metody do nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="884de-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="884de-154">To nie jest wymagane w przypadku klientów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="884de-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="884de-155">Dla klientów języka JavaScript, kod wygenerowany serwer proxy po automatycznie tworzy serwery proxy dla wszystkich koncentratorów, które istnieją na serwerze, a rejestrowanie obsługi jest sposób wskazuje które koncentratorów klienta zamierza użyć.</span><span class="sxs-lookup"><span data-stu-id="884de-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="884de-156">Ale dla klienta programu .NET utworzyć proxy koncentratora ręcznie, więc SignalR przyjęto założenie, że będziesz używać dowolnego Centrum utworzonego na serwerze proxy dla.</span><span class="sxs-lookup"><span data-stu-id="884de-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="884de-157">Przykładowy kod używa domyślnej "/ signalr" adres URL do łączenia się z usługą SignalR.</span><span class="sxs-lookup"><span data-stu-id="884de-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="884de-158">Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [ASP.NET SignalR koncentratory interfejsu API przewodnik - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="884de-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="884de-159">`Start` Metoda wykonuje asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="884de-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="884de-160">Aby upewnić się, że kolejnych wierszy kodu nie są wykonywane dopiero po nawiązaniu połączenia, należy użyć `await` w ASP.NET 4.5 metody asynchronicznej lub `.Wait()` w metodzie synchronicznej.</span><span class="sxs-lookup"><span data-stu-id="884de-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="884de-161">Nie używaj `.Wait()` w kliencie WinRT.</span><span class="sxs-lookup"><span data-stu-id="884de-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="884de-162">`HubConnection` Klasa jest bezpieczne wątkowo.</span><span class="sxs-lookup"><span data-stu-id="884de-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="884de-163">Między domenami połączenia od klientów programu Silverlight</span><span class="sxs-lookup"><span data-stu-id="884de-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="884de-164">Aby uzyskać informacje o sposobie włączania połączeń między domenami z klientów programu Silverlight, zobacz [wprowadzania Service Available Across Domain Boundaries](https://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="884de-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="884de-165">Jak skonfigurować połączenia</span><span class="sxs-lookup"><span data-stu-id="884de-165">How to configure the connection</span></span>

<span data-ttu-id="884de-166">Przed nawiązaniem połączenia można określić dowolną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="884de-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="884de-167">Limit równoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="884de-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="884de-168">Parametry ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="884de-168">Query string parameters.</span></span>
- <span data-ttu-id="884de-169">Metoda transportu.</span><span class="sxs-lookup"><span data-stu-id="884de-169">The transport method.</span></span>
- <span data-ttu-id="884de-170">Nagłówki HTTP.</span><span class="sxs-lookup"><span data-stu-id="884de-170">HTTP headers.</span></span>
- <span data-ttu-id="884de-171">Certyfikaty klienta.</span><span class="sxs-lookup"><span data-stu-id="884de-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="884de-172">Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF</span><span class="sxs-lookup"><span data-stu-id="884de-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="884de-173">W klientach WPF może być konieczne zwiększyć maksymalną liczbę równoczesnych połączeń od wartość domyślną 2.</span><span class="sxs-lookup"><span data-stu-id="884de-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="884de-174">Zalecana wartość to 10.</span><span class="sxs-lookup"><span data-stu-id="884de-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="884de-175">Aby uzyskać więcej informacji, zobacz [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="884de-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="884de-176">Jak określać parametrów ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="884de-176">How to specify query string parameters</span></span>

<span data-ttu-id="884de-177">Jeśli chcesz wysyłać dane do serwera, gdy klient nawiąże połączenie, możesz dodać parametrów ciągu zapytania do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="884de-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="884de-178">Poniższy przykład pokazuje, jak ustawić parametr ciągu zapytania w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="884de-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="884de-179">Poniższy przykład pokazuje, jak można odczytać parametr ciągu zapytania w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="884de-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="884de-180">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="884de-180">How to specify the transport method</span></span>

<span data-ttu-id="884de-181">W ramach procesu łączenia klienta SignalR zwykle negocjuje z serwerem w celu ustalenia najlepsze transport, który jest obsługiwany przez zarówno serwera i klienta.</span><span class="sxs-lookup"><span data-stu-id="884de-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="884de-182">Jeśli znasz już transport, który ma być używany, można pominąć ten proces negocjacji.</span><span class="sxs-lookup"><span data-stu-id="884de-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="884de-183">Aby określić metodę transportu, należy przekazać w obiekcie transportu do metody rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="884de-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="884de-184">Poniższy przykład pokazuje, jak określić metodę transportu w kodu klienta.</span><span class="sxs-lookup"><span data-stu-id="884de-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="884de-185">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/en-us/library/jj918090(v=vs.111).aspx) przestrzeń nazw zawiera następujące klasy używanych do określania transportu.</span><span class="sxs-lookup"><span data-stu-id="884de-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/en-us/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="884de-186">[LongPollingTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="884de-186">[LongPollingTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="884de-187">[ServerSentEventsTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="884de-187">[ServerSentEventsTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="884de-188">[WebSocketTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (dostępne tylko wtedy, gdy zarówno serwera, jak i klienta korzystają z platformy .NET 4.5.)</span><span class="sxs-lookup"><span data-stu-id="884de-188">[WebSocketTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="884de-189">[AutoTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (automatycznie wybiera najlepsze transport, który jest obsługiwana zarówno przez klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="884de-189">[AutoTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="884de-190">Jest to domyślny transport.</span><span class="sxs-lookup"><span data-stu-id="884de-190">This is the default transport.</span></span> <span data-ttu-id="884de-191">Przekazywanie, to aby `Start` metoda ma ten sam efekt co w niczego nie przekazywany.)</span><span class="sxs-lookup"><span data-stu-id="884de-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="884de-192">ForeverFrame transport nie jest uwzględniony na liście, ponieważ jest używany tylko przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="884de-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="884de-193">Aby dowiedzieć się, jak sprawdzić Metoda transportu w kodzie serwera, zobacz [ASP.NET SignalR koncentratory interfejsu API przewodnik - Server - metody uzyskiwania informacji o kliencie z właściwości kontekstu](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="884de-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="884de-194">Aby uzyskać więcej informacji na temat transportów i przejścia, zobacz [wprowadzenie do biblioteki SignalR — transportu i przejścia](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="884de-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="884de-195">Jak określić nagłówków HTTP</span><span class="sxs-lookup"><span data-stu-id="884de-195">How to specify HTTP headers</span></span>

<span data-ttu-id="884de-196">Aby ustawić nagłówków HTTP, użyj `Headers` właściwości obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="884de-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="884de-197">Poniższy przykład przedstawia sposób dodawania nagłówka HTTP.</span><span class="sxs-lookup"><span data-stu-id="884de-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="884de-198">Jak określić certyfikaty klienta</span><span class="sxs-lookup"><span data-stu-id="884de-198">How to specify client certificates</span></span>

<span data-ttu-id="884de-199">Aby dodać certyfikaty klienta, użyj `AddClientCertificate` metody dla obiekt połączenia.</span><span class="sxs-lookup"><span data-stu-id="884de-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="884de-200">Tworzenie serwera proxy koncentratora</span><span class="sxs-lookup"><span data-stu-id="884de-200">How to create the Hub proxy</span></span>

<span data-ttu-id="884de-201">Aby zdefiniować metody na komputerze klienckim koncentratora można wywołać z serwera, a do wywołania metody koncentratora na serwerze, należy utworzyć serwer proxy dla koncentratora przez wywołanie metody `CreateHubProxy` na obiekt połączenia.</span><span class="sxs-lookup"><span data-stu-id="884de-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="884de-202">Ciąg przekazywane w celu `CreateHubProxy` to nazwa klasy koncentratora lub nazwa określona przez `HubName` atrybut, jeśli użyto jednego serwera.</span><span class="sxs-lookup"><span data-stu-id="884de-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="884de-203">Dopasowywaniu nazwy nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="884de-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="884de-204">**Klasy koncentratora na serwerze**</span><span class="sxs-lookup"><span data-stu-id="884de-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="884de-205">**Utwórz serwer proxy klienta dla koncentratora klasy**</span><span class="sxs-lookup"><span data-stu-id="884de-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="884de-206">Jeśli dekoracji klasy koncentratora z `HubName` atrybutu, użyj tej nazwy.</span><span class="sxs-lookup"><span data-stu-id="884de-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="884de-207">**Klasy koncentratora na serwerze**</span><span class="sxs-lookup"><span data-stu-id="884de-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="884de-208">**Utwórz serwer proxy klienta dla koncentratora klasy**</span><span class="sxs-lookup"><span data-stu-id="884de-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="884de-209">Obiekt serwera proxy jest bezpieczne wątkowo.</span><span class="sxs-lookup"><span data-stu-id="884de-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="884de-210">W rzeczywistości, jeśli wywołujesz `HubConnection.CreateHubProxy` wiele razy z tym samym `hubName`, możesz uzyskać je w pamięci podręcznej `IHubProxy` obiektu.</span><span class="sxs-lookup"><span data-stu-id="884de-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="884de-211">Sposób definiowania metod na komputerze klienckim, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="884de-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="884de-212">Aby określić metodę można wywołać serwera, Użyj serwera proxy `On` metodę, aby zarejestrować program obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="884de-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="884de-213">Dopasowania nazwy metody jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="884de-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="884de-214">Na przykład `Clients.All.UpdateStockPrice` będą wykonywane na serwerze `updateStockPrice`, `updatestockprice`, lub `UpdateStockPrice` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="884de-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="884de-215">Platformy innego klienta mają różne wymagania dotyczące sposobu pisania kodu metody można zaktualizować interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="884de-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="884de-216">Przykłady pokazano dotyczą klientów WinRT (.NET Sklepu Windows).</span><span class="sxs-lookup"><span data-stu-id="884de-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="884de-217">WPF, Silverlight i przykłady aplikacji konsoli znajdują się w [osobnej sekcji w dalszej części tego tematu](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="884de-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="884de-218">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="884de-218">Methods without parameters</span></span>

<span data-ttu-id="884de-219">Jeśli metoda w przypadku obsługi nie ma parametrów, użyj przeciążenia nierodzajową `On` metody:</span><span class="sxs-lookup"><span data-stu-id="884de-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="884de-220">**Wywołanie metody klienta bez parametrów kodu serwera**</span><span class="sxs-lookup"><span data-stu-id="884de-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="884de-221">**Kod klienta WinRT metody wywoływane z serwera bez parametrów ([zawiera przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="884de-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="884de-222">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="884de-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="884de-223">Jeśli metoda w przypadku obsługi ma parametry, określ typy parametrów jako typy ogólne z `On` metody.</span><span class="sxs-lookup"><span data-stu-id="884de-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="884de-224">Brak przeciążeń ogólnego `On` sposób można określić maksymalnie 8 parametrów (4 w systemie Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="884de-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="884de-225">W poniższym przykładzie jeden parametr jest wysyłane do `UpdateStockPrice` metody.</span><span class="sxs-lookup"><span data-stu-id="884de-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="884de-226">**Wywołanie metody klienta z parametrem kodu serwera**</span><span class="sxs-lookup"><span data-stu-id="884de-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="884de-227">**Klasy zapasów użyte w parametrze**</span><span class="sxs-lookup"><span data-stu-id="884de-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="884de-228">**Kod klienta WinRT metody wywoływane z serwera z parametrem ([zawiera przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="884de-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="884de-229">Metody z parametrami, określając obiekty dynamiczne parametrów</span><span class="sxs-lookup"><span data-stu-id="884de-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="884de-230">Jako alternatywę do określania parametrów jako typów ogólnych z `On` metody, można określić parametrów jako obiekty dynamiczne:</span><span class="sxs-lookup"><span data-stu-id="884de-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="884de-231">**Wywołanie metody klienta z parametrem kodu serwera**</span><span class="sxs-lookup"><span data-stu-id="884de-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="884de-232">**Klasy zapasów użyte w parametrze**</span><span class="sxs-lookup"><span data-stu-id="884de-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="884de-233">**Kod klienta WinRT metody o nazwie z serwera z parametrem dla parametru za pomocą obiektu dynamicznego ([zawiera przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="884de-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="884de-234">Jak usunąć program obsługi</span><span class="sxs-lookup"><span data-stu-id="884de-234">How to remove a handler</span></span>

<span data-ttu-id="884de-235">Aby usunąć program obsługi, należy wywołać jej `Dispose` metody.</span><span class="sxs-lookup"><span data-stu-id="884de-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="884de-236">**Kodu klienta dla metody o nazwie z serwera**</span><span class="sxs-lookup"><span data-stu-id="884de-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="884de-237">**Kod klienta, aby usunąć program obsługi**</span><span class="sxs-lookup"><span data-stu-id="884de-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="884de-238">Sposób wywołania metody serwera z klienta</span><span class="sxs-lookup"><span data-stu-id="884de-238">How to call server methods from the client</span></span>

<span data-ttu-id="884de-239">Aby wywołać metodę na serwerze, należy użyć `Invoke` metody dla serwera proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="884de-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="884de-240">Jeśli metoda serwera nie ma zwracanych wartości, użyj przeciążenia nierodzajową `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="884de-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="884de-241">**Kod serwera dla metody, która nie ma wartości zwracane**</span><span class="sxs-lookup"><span data-stu-id="884de-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="884de-242">**Kod klienta podczas wywoływania metody, która nie ma wartości zwracane**</span><span class="sxs-lookup"><span data-stu-id="884de-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="884de-243">Jeśli metoda serwera ma wartość zwracaną, określ typ zwracany jako typ ogólny `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="884de-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="884de-244">**Kod serwera dla metody, która ma wartość zwracaną i przyjmuje parametr typu złożonego**</span><span class="sxs-lookup"><span data-stu-id="884de-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="884de-245">**Klasa zasobu użyte w parametrze i zwracać wartość**</span><span class="sxs-lookup"><span data-stu-id="884de-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="884de-246">**Kod klienta podczas wywoływania metody, która ma wartość zwracaną i przyjmuje parametr typu złożonego, to metoda asynchroniczna ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="884de-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="884de-247">**Wywoływanie metody, która ma wartość zwracaną i przyjmuje parametr typu złożonego, metoda synchroniczna kodu klienta**</span><span class="sxs-lookup"><span data-stu-id="884de-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="884de-248">`Invoke` Metoda wykonuje asynchronicznie i zwraca `Task` obiektu.</span><span class="sxs-lookup"><span data-stu-id="884de-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="884de-249">Jeśli nie określisz `await` lub `.Wait()`, następnym wierszu kodu zostanie wykonany przed metody, które można wywołać zakończono wykonywanie.</span><span class="sxs-lookup"><span data-stu-id="884de-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="884de-250">Sposób obsługi zdarzeń okres istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="884de-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="884de-251">Biblioteka SignalR udostępnia następujące połączenie okres istnienia zdarzeń, które może obsłużyć:</span><span class="sxs-lookup"><span data-stu-id="884de-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="884de-252">`Received`: Zgłaszane w przypadku nieodebrania żadnych danych w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="884de-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="884de-253">Udostępnia odebranych danych.</span><span class="sxs-lookup"><span data-stu-id="884de-253">Provides the received data.</span></span>
- <span data-ttu-id="884de-254">`ConnectionSlow`: Wywoływane, gdy klient wykryje wolne lub często porzucanie połączenie.</span><span class="sxs-lookup"><span data-stu-id="884de-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="884de-255">`Reconnecting`: Wywoływane, gdy transportu źródłowego rozpoczyna ponowne nawiązywanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="884de-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="884de-256">`Reconnected`: Wywoływane, gdy podłączył transportu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="884de-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="884de-257">`StateChanged`: Wywoływane po zmianie stanu połączenia.</span><span class="sxs-lookup"><span data-stu-id="884de-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="884de-258">Zawiera stan stary i nowy stan.</span><span class="sxs-lookup"><span data-stu-id="884de-258">Provides the old state and the new state.</span></span> <span data-ttu-id="884de-259">Aby uzyskać informacje o połączeniu zobacz wartości stanu [wyliczenie Element ConnectionState](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="884de-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="884de-260">`Closed`: Wywoływane, gdy połączenie zostało rozłączone.</span><span class="sxs-lookup"><span data-stu-id="884de-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="884de-261">Na przykład, jeśli chcesz wyświetlić komunikaty ostrzegawcze błędów, które nie są krytyczne, ale powodują sporadyczne problemy z połączeniem, takie jak powolność lub zbyt częstej porzucenie połączenia, obsługi `ConnectionSlow` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="884de-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="884de-262">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługi zdarzeń okres istnienia połączenia w SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="884de-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="884de-263">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="884de-263">How to handle errors</span></span>

<span data-ttu-id="884de-264">Jeśli nie włączysz jawnie szczegółowe komunikaty o błędach na serwerze, obiekt wyjątku, który zwraca SignalR po wystąpieniu błędu zawiera minimalne informacje o błędzie.</span><span class="sxs-lookup"><span data-stu-id="884de-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="884de-265">Na przykład, jeśli wywołanie `newContosoChatMessage` zawiera komunikat o błędzie w obiekcie błąd kończy się niepowodzeniem, "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Wysyłanie szczegółowe komunikaty o błędach do klientów w środowisku produkcyjnym nie jest zalecana ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach dla rozwiązywania problemów, należy użyć poniższego kodu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="884de-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="884de-266">Do obsługi błędów, które wywołuje SignalR, można dodać obsługi dla `Error` zdarzenia dla obiekt połączenia.</span><span class="sxs-lookup"><span data-stu-id="884de-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="884de-267">Do obsługi błędów z wywołań metody opisywanego, ujmij kod w bloku try-catch.</span><span class="sxs-lookup"><span data-stu-id="884de-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="884de-268">Jak włączyć rejestrowanie klienta</span><span class="sxs-lookup"><span data-stu-id="884de-268">How to enable client-side logging</span></span>

<span data-ttu-id="884de-269">Aby włączyć rejestrowanie klienta, należy ustawić `TraceLevel` i `TraceWriter` właściwości obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="884de-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="884de-270">WPF, Silverlight i przykładów kodu aplikacji konsoli dla metod klienta, które może wywoływać serwera</span><span class="sxs-lookup"><span data-stu-id="884de-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="884de-271">Przykłady kodu pokazano wcześniej do definiowania metod klienta, które można wywołać serwera dotyczą klientów WinRT.</span><span class="sxs-lookup"><span data-stu-id="884de-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="884de-272">Poniższe przykłady Pokaż równoważne kod WPF, Silverlight i klientów aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="884de-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="884de-273">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="884de-273">Methods without parameters</span></span>

<span data-ttu-id="884de-274">**WPF kodu klienta dla metody o nazwie z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="884de-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="884de-275">**Kod klienta Silverlight dla metody wywoływane z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="884de-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="884de-276">**Kod klienta aplikacji konsoli metody wywoływane z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="884de-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="884de-277">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="884de-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="884de-278">**WPF kodu klienta dla metody o nazwie z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="884de-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="884de-279">**Kod klienta Silverlight dla metody o nazwie z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="884de-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="884de-280">**Kod klienta aplikacji konsoli metody wywoływane z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="884de-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="884de-281">Metody z parametrami, określając obiekty dynamiczne parametrów</span><span class="sxs-lookup"><span data-stu-id="884de-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="884de-282">**WPF kodu klienta dla metody o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny dla parametru**</span><span class="sxs-lookup"><span data-stu-id="884de-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="884de-283">**Kod klienta Silverlight dla metody o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny dla parametru**</span><span class="sxs-lookup"><span data-stu-id="884de-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="884de-284">**Kod klienta aplikacji konsoli metody o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny dla parametru**</span><span class="sxs-lookup"><span data-stu-id="884de-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
