---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Podręcznik interfejsu API centrów SignalR platformy ASP.NET — klient modelu .NET (C#) | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 2 w przypadku klientów programu .NET, takich jak Windows Store (WinRT), WPF, Silverlight i wad...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 8ee3be32af6794dd352cdadb668fc0fbba70d815
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369292"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="6fd09-103">Podręcznik interfejsu API centrów SignalR platformy ASP.NET — klient modelu .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="6fd09-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>
====================
<span data-ttu-id="6fd09-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6fd09-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="6fd09-105">Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 2 w przypadku klientów programu .NET, takich jak Windows Store (WinRT), WPF, Silverlight oraz aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="6fd09-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="6fd09-106">Interfejsu API centrów SignalR umożliwia zdalne wywołania procedur (RPC) z serwera do połączonych klientów i od klientów z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6fd09-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="6fd09-107">W kodzie serwera należy zdefiniować metody, które mogą być wywoływane przez klientów, a wywołanie metody, które są uruchamiane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="6fd09-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="6fd09-108">W kodzie klienta definiowania metod, które mogą być wywoływane z serwera, a wywołanie metody, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6fd09-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="6fd09-109">SignalR zajmuje się wszystkie nadmiar klient serwer dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="6fd09-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="6fd09-110">SignalR oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="6fd09-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="6fd09-111">Wprowadzenie do SignalR, centra i połączenia trwałego lub samouczek, w którym przedstawiono sposób tworzenia kompletnej aplikacji SignalR, patrz [SignalR — wprowadzenie](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="6fd09-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="6fd09-112">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="6fd09-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="6fd09-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6fd09-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="6fd09-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6fd09-114">.NET 4.5</span></span>
> - <span data-ttu-id="6fd09-115">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="6fd09-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="6fd09-116">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="6fd09-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="6fd09-117">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="6fd09-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="6fd09-118">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="6fd09-118">Questions and comments</span></span>
> 
> <span data-ttu-id="6fd09-119">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="6fd09-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6fd09-120">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="6fd09-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="6fd09-121">Omówienie</span><span class="sxs-lookup"><span data-stu-id="6fd09-121">Overview</span></span>

<span data-ttu-id="6fd09-122">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="6fd09-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="6fd09-123">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="6fd09-123">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="6fd09-124">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="6fd09-124">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="6fd09-125">Połączenia między domenami od klientów programu Silverlight</span><span class="sxs-lookup"><span data-stu-id="6fd09-125">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="6fd09-126">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="6fd09-126">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="6fd09-127">Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF</span><span class="sxs-lookup"><span data-stu-id="6fd09-127">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="6fd09-128">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="6fd09-128">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="6fd09-129">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="6fd09-129">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="6fd09-130">Jak określić nagłówki HTTP</span><span class="sxs-lookup"><span data-stu-id="6fd09-130">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="6fd09-131">Jak określić certyfikaty klienta</span><span class="sxs-lookup"><span data-stu-id="6fd09-131">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="6fd09-132">Jak utworzyć serwer proxy koncentratora</span><span class="sxs-lookup"><span data-stu-id="6fd09-132">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="6fd09-133">Sposób definiowania metody na kliencie, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="6fd09-133">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="6fd09-134">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="6fd09-134">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="6fd09-135">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="6fd09-135">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="6fd09-136">Metody z parametrami, określając obiektów dynamicznych parametrów</span><span class="sxs-lookup"><span data-stu-id="6fd09-136">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="6fd09-137">Jak usunąć program obsługi</span><span class="sxs-lookup"><span data-stu-id="6fd09-137">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="6fd09-138">Jak wywołać metody serwera z poziomu klienta</span><span class="sxs-lookup"><span data-stu-id="6fd09-138">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="6fd09-139">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="6fd09-139">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="6fd09-140">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="6fd09-140">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="6fd09-141">Włączanie rejestrowania po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="6fd09-141">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="6fd09-142">WPF, Silverlight oraz przykłady kodu aplikacji konsoli dla metod klienta, które można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="6fd09-142">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="6fd09-143">Przykładowych projektach klientów platformy .NET na ten temat można znaleźć w następujących zasobach:</span><span class="sxs-lookup"><span data-stu-id="6fd09-143">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="6fd09-144">[gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) w witrynie GitHub.com (przykłady aplikacji konsoli WinRT, Silverlight,).</span><span class="sxs-lookup"><span data-stu-id="6fd09-144">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="6fd09-145">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) w witrynie GitHub.com (np. WPF).</span><span class="sxs-lookup"><span data-stu-id="6fd09-145">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="6fd09-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) w witrynie GitHub.com (np. aplikację konsoli).</span><span class="sxs-lookup"><span data-stu-id="6fd09-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="6fd09-147">Dokumentacja na temat programu server lub klientów języka JavaScript, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="6fd09-147">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="6fd09-148">Podręcznik interfejsu API centrów SignalR — serwer</span><span class="sxs-lookup"><span data-stu-id="6fd09-148">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="6fd09-149">Podręcznik interfejsu API centrów SignalR — klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="6fd09-149">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="6fd09-150">Łącza do tematów, dokumentacja interfejsu API są .NET 4.5 wersję interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6fd09-150">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="6fd09-151">Jeśli używasz programu .NET 4, zobacz [tematy interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="6fd09-151">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="6fd09-152">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="6fd09-152">Client setup</span></span>

<span data-ttu-id="6fd09-153">Zainstaluj [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pakietu NuGet (nie [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) pakietu).</span><span class="sxs-lookup"><span data-stu-id="6fd09-153">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="6fd09-154">Ten pakiet obsługuje WinRT, Silverlight, WPF, aplikacji konsoli i klienci Windows Phone, zarówno dla platformy .NET 4 i .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="6fd09-154">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="6fd09-155">Jeśli wersji biblioteki SignalR, że masz na komputerze klienckim jest inna niż wersja, która ma na serwerze, SignalR jest zazwyczaj możliwość dostosowania różnicy.</span><span class="sxs-lookup"><span data-stu-id="6fd09-155">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="6fd09-156">Na przykład serwer z systemem SignalR w wersji 2 będzie obsługiwać klientów, którzy mają zainstalowane 1.1.x, a także klientów, którzy mają w wersji 2 zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="6fd09-156">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="6fd09-157">Jeśli różnica między wersją na serwerze i wersji na komputerze klienckim jest zbyt duża, lub jeśli klient jest nowsza niż na serwerze, SignalR zgłasza `InvalidOperationException` wyjątku, gdy klient próbuje nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="6fd09-157">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="6fd09-158">Komunikat o błędzie "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="6fd09-158">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="6fd09-159">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="6fd09-159">How to establish a connection</span></span>

<span data-ttu-id="6fd09-160">Zanim usługa może nawiązać połączenie, należy utworzyć `HubConnection` obiektu i tworzyć serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="6fd09-160">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="6fd09-161">Aby nawiązać połączenie, należy wywołać `Start` metody `HubConnection` obiektu.</span><span class="sxs-lookup"><span data-stu-id="6fd09-161">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="6fd09-162">W przypadku klientów JavaScript należy zarejestrować co najmniej jeden program obsługi zdarzeń przed wywołaniem `Start` metodę, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="6fd09-162">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="6fd09-163">To nie jest wymagane dla klientów programu .NET.</span><span class="sxs-lookup"><span data-stu-id="6fd09-163">This is not necessary for .NET clients.</span></span> <span data-ttu-id="6fd09-164">Dla klientów JavaScript wygenerowany serwer proxy automatycznie tworzy kod serwery proxy do wszystkich centrów znajdujące się na serwerze i rejestrowania procedury obsługi jest wskazuje, które koncentratorów klienta zamierza użyć.</span><span class="sxs-lookup"><span data-stu-id="6fd09-164">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="6fd09-165">Ale dla klienta programu .NET utworzyć serwery proxy Centrum ręcznie, więc SignalR przyjęto założenie, że będziesz używać dowolne utworzonego serwera proxy dla Centrum.</span><span class="sxs-lookup"><span data-stu-id="6fd09-165">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="6fd09-166">Przykładowy kod używa domyślnej "/ signalr" adres URL, aby nawiązać połączenie z usługą SignalR.</span><span class="sxs-lookup"><span data-stu-id="6fd09-166">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="6fd09-167">Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="6fd09-167">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="6fd09-168">`Start` Metoda jest wykonywana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="6fd09-168">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="6fd09-169">Aby upewnić się, że kolejne wiersze kodu nie wykonany dopóki, po nawiązaniu połączenia, należy użyć `await` w metodzie asynchronicznej, ASP.NET 4.5 lub `.Wait()` w metodzie synchronicznej.</span><span class="sxs-lookup"><span data-stu-id="6fd09-169">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="6fd09-170">Nie używaj `.Wait()` w kliencie WinRT.</span><span class="sxs-lookup"><span data-stu-id="6fd09-170">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="6fd09-171">Połączenia między domenami od klientów programu Silverlight</span><span class="sxs-lookup"><span data-stu-id="6fd09-171">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="6fd09-172">Aby uzyskać informacje o sposobie włączania połączeń między domenami od klientów programu Silverlight, zobacz [wprowadzania Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="6fd09-172">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="6fd09-173">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="6fd09-173">How to configure the connection</span></span>

<span data-ttu-id="6fd09-174">Przed nawiązaniem połączenia, można określić dowolną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="6fd09-174">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="6fd09-175">Limit połączeń współbieżnych.</span><span class="sxs-lookup"><span data-stu-id="6fd09-175">Concurrent connections limit.</span></span>
- <span data-ttu-id="6fd09-176">Parametry ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="6fd09-176">Query string parameters.</span></span>
- <span data-ttu-id="6fd09-177">Metoda transportu.</span><span class="sxs-lookup"><span data-stu-id="6fd09-177">The transport method.</span></span>
- <span data-ttu-id="6fd09-178">Nagłówki HTTP.</span><span class="sxs-lookup"><span data-stu-id="6fd09-178">HTTP headers.</span></span>
- <span data-ttu-id="6fd09-179">Certyfikaty klienta.</span><span class="sxs-lookup"><span data-stu-id="6fd09-179">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="6fd09-180">Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF</span><span class="sxs-lookup"><span data-stu-id="6fd09-180">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="6fd09-181">W klientach programu WPF trzeba zwiększyć maksymalną liczbę równoczesnych połączeń od jego wartość domyślną 2.</span><span class="sxs-lookup"><span data-stu-id="6fd09-181">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="6fd09-182">Zalecana wartość wynosi 10.</span><span class="sxs-lookup"><span data-stu-id="6fd09-182">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="6fd09-183">Aby uzyskać więcej informacji, zobacz [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fd09-183">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="6fd09-184">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="6fd09-184">How to specify query string parameters</span></span>

<span data-ttu-id="6fd09-185">Jeśli chcesz wysyłać dane do serwera, gdy klient nawiąże połączenie, można dodać parametry ciągu zapytania do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="6fd09-185">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="6fd09-186">Poniższy przykład pokazuje, jak ustawić parametr ciągu zapytania w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="6fd09-186">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="6fd09-187">Poniższy przykład pokazuje, jak odczytywać parametr ciągu zapytania w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="6fd09-187">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="6fd09-188">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="6fd09-188">How to specify the transport method</span></span>

<span data-ttu-id="6fd09-189">Częścią procesu nawiązywania połączenia klienta SignalR zwykle negocjuje z serwerem, aby określić najlepsze transportu, która jest obsługiwana przez serwer i klienta.</span><span class="sxs-lookup"><span data-stu-id="6fd09-189">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="6fd09-190">Jeśli znasz już transport, którego chcesz użyć, można pominąć ten proces negocjacji.</span><span class="sxs-lookup"><span data-stu-id="6fd09-190">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="6fd09-191">Aby określić metodę transportu, należy przekazać w obiekcie transportu do metody uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="6fd09-191">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="6fd09-192">Poniższy przykład pokazuje, jak określić metodę transportu w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="6fd09-192">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="6fd09-193">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) przestrzeń nazw zawiera następujące klasy, używanych do określania transportu.</span><span class="sxs-lookup"><span data-stu-id="6fd09-193">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="6fd09-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="6fd09-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="6fd09-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="6fd09-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="6fd09-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (dostępne tylko wtedy, gdy serwera i klienta użyj .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="6fd09-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="6fd09-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (automatycznie wybiera najlepsze transportu, która jest obsługiwana zarówno przez klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="6fd09-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="6fd09-198">Jest to domyślny transport.</span><span class="sxs-lookup"><span data-stu-id="6fd09-198">This is the default transport.</span></span> <span data-ttu-id="6fd09-199">Te informacje w celu przekazywania `Start` metoda ma taki sam skutek jak nie przechodzi coś.)</span><span class="sxs-lookup"><span data-stu-id="6fd09-199">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="6fd09-200">ForeverFrame transport jest niedostępna na tej liście, ponieważ jest używany tylko przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6fd09-200">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="6fd09-201">Aby uzyskać informacje o sposobie Sprawdź metodą transportu w kodzie serwera, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - sposób uzyskiwania informacji na temat klienta z właściwości kontekstu](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="6fd09-201">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="6fd09-202">Aby uzyskać więcej informacji dotyczących transportu i planów awaryjnych, zobacz [wprowadzenie do SignalR - transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="6fd09-202">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="6fd09-203">Jak określić nagłówki HTTP</span><span class="sxs-lookup"><span data-stu-id="6fd09-203">How to specify HTTP headers</span></span>

<span data-ttu-id="6fd09-204">Aby ustawić nagłówki HTTP, użyj `Headers` właściwości w obiekcie połączenia.</span><span class="sxs-lookup"><span data-stu-id="6fd09-204">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="6fd09-205">Poniższy przykład pokazuje, jak dodać nagłówek HTTP.</span><span class="sxs-lookup"><span data-stu-id="6fd09-205">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="6fd09-206">Jak określić certyfikaty klienta</span><span class="sxs-lookup"><span data-stu-id="6fd09-206">How to specify client certificates</span></span>

<span data-ttu-id="6fd09-207">Aby dodać certyfikaty klienta, należy użyć `AddClientCertificate` metody na obiekt połączenia.</span><span class="sxs-lookup"><span data-stu-id="6fd09-207">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="6fd09-208">Jak utworzyć serwer proxy koncentratora</span><span class="sxs-lookup"><span data-stu-id="6fd09-208">How to create the Hub proxy</span></span>

<span data-ttu-id="6fd09-209">Aby zdefiniować metody dla klienta, który może wywołać koncentrator, z serwera i wywoływanie metod koncentratora na serwerze, należy utworzyć serwer proxy dla koncentratora przez wywołanie metody `CreateHubProxy` w obiekcie połączenia.</span><span class="sxs-lookup"><span data-stu-id="6fd09-209">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="6fd09-210">Ciąg przekazanej do `CreateHubProxy` jest nazwą klasy koncentratora lub nazwa określona przez `HubName` atrybutu, jeśli użyto jednego na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6fd09-210">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="6fd09-211">Dopasowywaniu nazwy nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="6fd09-211">Name matching is case-insensitive.</span></span>

<span data-ttu-id="6fd09-212">**Klasy koncentratora na serwerze**</span><span class="sxs-lookup"><span data-stu-id="6fd09-212">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="6fd09-213">**Utwórz serwer proxy klienta dla klasy koncentratora**</span><span class="sxs-lookup"><span data-stu-id="6fd09-213">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="6fd09-214">Jeśli dekoracji klasy koncentratora z `HubName` atrybutu, należy użyć tej nazwy.</span><span class="sxs-lookup"><span data-stu-id="6fd09-214">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="6fd09-215">**Klasy koncentratora na serwerze**</span><span class="sxs-lookup"><span data-stu-id="6fd09-215">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="6fd09-216">**Utwórz serwer proxy klienta dla klasy koncentratora**</span><span class="sxs-lookup"><span data-stu-id="6fd09-216">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="6fd09-217">Jeśli wywołasz `HubConnection.CreateHubProxy` wiele razy z takimi samymi `hubName`, możesz uzyskać takie same buforowane `IHubProxy` obiektu.</span><span class="sxs-lookup"><span data-stu-id="6fd09-217">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="6fd09-218">Sposób definiowania metody na kliencie, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="6fd09-218">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="6fd09-219">Aby zdefiniować metodę, która może wywołać serwera, należy użyć serwera proxy `On` metodę, aby zarejestrować program obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="6fd09-219">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="6fd09-220">Dopasowywaniu nazwy metody nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="6fd09-220">Method name matching is case-insensitive.</span></span> <span data-ttu-id="6fd09-221">Na przykład `Clients.All.UpdateStockPrice` będą wykonywane na serwerze `updateStockPrice`, `updatestockprice`, lub `UpdateStockPrice` na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="6fd09-221">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="6fd09-222">Innego klienta platformy mają różne wymagania dotyczące sposobu pisania kodu metody, aby zaktualizować interfejs użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6fd09-222">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="6fd09-223">Przykłady zamieszczone dotyczą klientów WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="6fd09-223">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="6fd09-224">WPF, Silverlight oraz przykłady aplikacji konsoli znajdują się w [osobnej sekcji w dalszej części tego tematu](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="6fd09-224">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="6fd09-225">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="6fd09-225">Methods without parameters</span></span>

<span data-ttu-id="6fd09-226">Jeśli metoda jest obsługa nie ma parametrów, użyj przeciążenia nieogólnego `On` metody:</span><span class="sxs-lookup"><span data-stu-id="6fd09-226">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="6fd09-227">**Kod serwera podczas wywoływania metody klienta bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="6fd09-227">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="6fd09-228">**Kod klienta WinRT metody wywoływane z serwera bez parametrów ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="6fd09-228">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="6fd09-229">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="6fd09-229">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="6fd09-230">Jeśli metoda jest obsługa ma parametry, należy określić typy parametrów jako typów ogólnego `On` metody.</span><span class="sxs-lookup"><span data-stu-id="6fd09-230">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="6fd09-231">Istnieją przeciążenia ogólne `On` metody umożliwiają określenie parametrów do 8 (4 w systemie Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="6fd09-231">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="6fd09-232">W poniższym przykładzie jeden parametr jest wysyłane do `UpdateStockPrice` metody.</span><span class="sxs-lookup"><span data-stu-id="6fd09-232">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="6fd09-233">**Kod serwera podczas wywoływania metody klienta z parametrem**</span><span class="sxs-lookup"><span data-stu-id="6fd09-233">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="6fd09-234">**Klasy zapasów użytym dla parametru**</span><span class="sxs-lookup"><span data-stu-id="6fd09-234">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="6fd09-235">**Kod klienta WinRT dla metody wywoływane z serwera za pomocą parametru ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="6fd09-235">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="6fd09-236">Metody z parametrami, określając obiektów dynamicznych parametrów</span><span class="sxs-lookup"><span data-stu-id="6fd09-236">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="6fd09-237">Jako alternatywę do określania parametrów jako typów ogólnego `On` metody, parametry można określić jako obiektów dynamicznych:</span><span class="sxs-lookup"><span data-stu-id="6fd09-237">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="6fd09-238">**Kod serwera podczas wywoływania metody klienta z parametrem**</span><span class="sxs-lookup"><span data-stu-id="6fd09-238">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="6fd09-239">**Klasy zapasów użytym dla parametru**</span><span class="sxs-lookup"><span data-stu-id="6fd09-239">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="6fd09-240">**Kod klienta WinRT dla metody wywoływane z serwera z parametrem przy użyciu obiekt dynamiczny parametr ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="6fd09-240">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="6fd09-241">Jak usunąć program obsługi</span><span class="sxs-lookup"><span data-stu-id="6fd09-241">How to remove a handler</span></span>

<span data-ttu-id="6fd09-242">Aby usunąć program obsługi, należy wywołać jej `Dispose` metody.</span><span class="sxs-lookup"><span data-stu-id="6fd09-242">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="6fd09-243">**Kod klienta dla metodę o nazwie z serwera**</span><span class="sxs-lookup"><span data-stu-id="6fd09-243">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="6fd09-244">**Kod klienta, aby usunąć program obsługi**</span><span class="sxs-lookup"><span data-stu-id="6fd09-244">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="6fd09-245">Jak wywołać metody serwera z poziomu klienta</span><span class="sxs-lookup"><span data-stu-id="6fd09-245">How to call server methods from the client</span></span>

<span data-ttu-id="6fd09-246">Aby wywołać metodę na serwerze, należy użyć `Invoke` metody serwera proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="6fd09-246">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="6fd09-247">Jeśli metoda nie zwraca żadnej wartości, użyj przeciążenia nieogólnego `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="6fd09-247">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="6fd09-248">**Kod serwera dla metody, która nie zwraca żadnej wartości**</span><span class="sxs-lookup"><span data-stu-id="6fd09-248">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="6fd09-249">**Kod klienta, wywołanie metody, która nie zwraca żadnej wartości**</span><span class="sxs-lookup"><span data-stu-id="6fd09-249">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="6fd09-250">Jeśli metoda nie zwraca wartości, należy określić typ zwracany jako ogólny typ `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="6fd09-250">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="6fd09-251">**Kod serwera dla metody, która nie zwraca wartości i przyjmuje parametr typu złożonego**</span><span class="sxs-lookup"><span data-stu-id="6fd09-251">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="6fd09-252">**Klasy zapasów użytym dla parametru i zwracają wartość**</span><span class="sxs-lookup"><span data-stu-id="6fd09-252">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="6fd09-253">**Kod klienta, wywołanie metody, która nie zwraca wartości i przyjmuje parametr typu złożonego w metodzie asynchronicznej ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="6fd09-253">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="6fd09-254">**Kod klienta, wywołanie metody, która nie zwraca wartości i przyjmuje parametr typu złożonego, metoda synchroniczna**</span><span class="sxs-lookup"><span data-stu-id="6fd09-254">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="6fd09-255">`Invoke` Metoda wykonuje asynchronicznie i zwraca `Task` obiektu.</span><span class="sxs-lookup"><span data-stu-id="6fd09-255">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="6fd09-256">Jeśli nie określisz `await` lub `.Wait()`, następnego wiersza kodu zostaną wykonane, zanim metodę, która wywołuje zakończenia.</span><span class="sxs-lookup"><span data-stu-id="6fd09-256">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="6fd09-257">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="6fd09-257">How to handle connection lifetime events</span></span>

<span data-ttu-id="6fd09-258">Biblioteka SignalR udostępnia następujące połączenia zdarzenia okresu istnienia, które może obsłużyć:</span><span class="sxs-lookup"><span data-stu-id="6fd09-258">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="6fd09-259">`Received`: Zgłaszane w przypadku nieodebrania żadnych danych w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="6fd09-259">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="6fd09-260">Udostępnia odebrane dane.</span><span class="sxs-lookup"><span data-stu-id="6fd09-260">Provides the received data.</span></span>
- <span data-ttu-id="6fd09-261">`ConnectionSlow`: Wywoływane, gdy klient wykrywa wolno lub często porzucanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="6fd09-261">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="6fd09-262">`Reconnecting`: Wywoływane, gdy transportu źródłowego rozpoczyna ponowne nawiązywanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="6fd09-262">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="6fd09-263">`Reconnected`: Wywoływane, gdy nawiązał transportu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="6fd09-263">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="6fd09-264">`StateChanged`: Wywoływane, gdy zmieni się stan połączenia.</span><span class="sxs-lookup"><span data-stu-id="6fd09-264">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="6fd09-265">Zawiera stan stary i nowy stan.</span><span class="sxs-lookup"><span data-stu-id="6fd09-265">Provides the old state and the new state.</span></span> <span data-ttu-id="6fd09-266">Aby uzyskać informacje na temat połączenia wartości stanu zobacz [wyliczenie Element ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="6fd09-266">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="6fd09-267">`Closed`: Wywoływane, gdy połączenie zostało rozłączone.</span><span class="sxs-lookup"><span data-stu-id="6fd09-267">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="6fd09-268">Na przykład, jeśli chcesz wyświetlić komunikaty ostrzegawcze dotyczące błędów, które nie są one krytyczny, ale powodują sporadyczne problemy z połączeniem, takie jak powolność lub zbyt częstej porzucenie połączenia, obsługi `ConnectionSlow` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="6fd09-268">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="6fd09-269">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia w SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="6fd09-269">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="6fd09-270">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="6fd09-270">How to handle errors</span></span>

<span data-ttu-id="6fd09-271">Jeśli nie włączysz jawnie szczegółowe komunikaty o błędach na serwerze, obiekt wyjątku, który zwraca SignalR, po wystąpieniu błędu zawiera minimalne informacje o tym błędzie.</span><span class="sxs-lookup"><span data-stu-id="6fd09-271">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="6fd09-272">Na przykład, jeśli wywołanie `newContosoChatMessage` zawiera komunikat o błędzie w obiekcie błąd zakończy się niepowodzeniem, "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Wysyłanie szczegółowe komunikaty o błędach do klientów w środowisku produkcyjnym jest niezalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach dla rozwiązywania problemów, użyj poniższego kodu, na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6fd09-272">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="6fd09-273">Do obsługi błędów, które wywołuje SignalR, można dodać program obsługi `Error` zdarzenie na obiekt połączenia.</span><span class="sxs-lookup"><span data-stu-id="6fd09-273">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="6fd09-274">Do obsługi błędów z wywołań metod, zawijania kodu w bloku try-catch.</span><span class="sxs-lookup"><span data-stu-id="6fd09-274">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="6fd09-275">Włączanie rejestrowania po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="6fd09-275">How to enable client-side logging</span></span>

<span data-ttu-id="6fd09-276">Aby włączyć rejestrowanie klienta, należy ustawić `TraceLevel` i `TraceWriter` właściwości w obiekcie połączenia.</span><span class="sxs-lookup"><span data-stu-id="6fd09-276">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="6fd09-277">WPF, Silverlight oraz przykłady kodu aplikacji konsoli dla metod klienta, które można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="6fd09-277">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="6fd09-278">Przykłady kodu, w przedstawionej wcześniej do definiowania metody klientów, które serwer może wywoływać dotyczą klientów WinRT.</span><span class="sxs-lookup"><span data-stu-id="6fd09-278">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="6fd09-279">Poniższe przykłady pokazują równoważny kod dla WPF, Silverlight oraz klientów aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="6fd09-279">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="6fd09-280">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="6fd09-280">Methods without parameters</span></span>

<span data-ttu-id="6fd09-281">**WPF kodu klienta dla metodę o nazwie z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="6fd09-281">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="6fd09-282">**Kod klienta Silverlight dla metodę o nazwie z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="6fd09-282">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="6fd09-283">**Kod klienta aplikacji konsolowej, metoda jest wywoływana z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="6fd09-283">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="6fd09-284">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="6fd09-284">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="6fd09-285">**WPF kodu klienta dla metodę o nazwie z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="6fd09-285">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="6fd09-286">**Kod klienta Silverlight dla metodę o nazwie z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="6fd09-286">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="6fd09-287">**Kod klienta aplikacji konsolowej, metoda jest wywoływana z serwera za pomocą parametru**</span><span class="sxs-lookup"><span data-stu-id="6fd09-287">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="6fd09-288">Metody z parametrami, określając obiektów dynamicznych parametrów</span><span class="sxs-lookup"><span data-stu-id="6fd09-288">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="6fd09-289">**WPF kodu klienta dla metodę o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny parametr**</span><span class="sxs-lookup"><span data-stu-id="6fd09-289">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="6fd09-290">**Kod klienta Silverlight dla metodę o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny parametr**</span><span class="sxs-lookup"><span data-stu-id="6fd09-290">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="6fd09-291">**Kod klienta aplikacji konsoli dla metody wywoływane z serwera z parametrem przy użyciu obiekt dynamiczny parametr**</span><span class="sxs-lookup"><span data-stu-id="6fd09-291">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
