---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Podręcznik interfejsu API centrów SignalR platformy ASP.NET — klient modelu .NET (C#) | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 2 w przypadku klientów programu .NET, takich jak Windows Store (WinRT), WPF, Silverlight i wad...
ms.author: riande
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9981b8b91be3395b1a3aa7e0cabb1b7f455d47be
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396184"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="b4189-103">Podręcznik interfejsu API centrów SignalR platformy ASP.NET — klient modelu .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="b4189-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>
====================

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="b4189-104">Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 2 w przypadku klientów programu .NET, takich jak Windows Store (WinRT), WPF, Silverlight oraz aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="b4189-104">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="b4189-105">Interfejsu API centrów SignalR umożliwia zdalne wywołania procedur (RPC) z serwera do połączonych klientów i od klientów z serwerem.</span><span class="sxs-lookup"><span data-stu-id="b4189-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="b4189-106">W kodzie serwera należy zdefiniować metody, które mogą być wywoływane przez klientów, a wywołanie metody, które są uruchamiane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="b4189-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="b4189-107">W kodzie klienta definiowania metod, które mogą być wywoływane z serwera, a wywołanie metody, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="b4189-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="b4189-108">SignalR zajmuje się wszystkie nadmiar klient serwer dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="b4189-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="b4189-109">SignalR oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="b4189-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="b4189-110">Wprowadzenie do SignalR, centra i połączenia trwałego lub samouczek, w którym przedstawiono sposób tworzenia kompletnej aplikacji SignalR, patrz [SignalR — wprowadzenie](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="b4189-110">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="b4189-111">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="b4189-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="b4189-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b4189-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="b4189-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b4189-113">.NET 4.5</span></span>
> - <span data-ttu-id="b4189-114">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="b4189-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="b4189-115">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="b4189-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="b4189-116">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="b4189-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="b4189-117">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="b4189-117">Questions and comments</span></span>
>
> <span data-ttu-id="b4189-118">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="b4189-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b4189-119">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b4189-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="b4189-120">Omówienie</span><span class="sxs-lookup"><span data-stu-id="b4189-120">Overview</span></span>

<span data-ttu-id="b4189-121">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="b4189-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="b4189-122">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="b4189-122">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="b4189-123">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="b4189-123">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="b4189-124">Połączenia między domenami od klientów programu Silverlight</span><span class="sxs-lookup"><span data-stu-id="b4189-124">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="b4189-125">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="b4189-125">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="b4189-126">Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF</span><span class="sxs-lookup"><span data-stu-id="b4189-126">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="b4189-127">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="b4189-127">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="b4189-128">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="b4189-128">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="b4189-129">Jak określić nagłówki HTTP</span><span class="sxs-lookup"><span data-stu-id="b4189-129">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="b4189-130">Jak określić certyfikaty klienta</span><span class="sxs-lookup"><span data-stu-id="b4189-130">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="b4189-131">Jak utworzyć serwer proxy koncentratora</span><span class="sxs-lookup"><span data-stu-id="b4189-131">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="b4189-132">Sposób definiowania metody na kliencie, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="b4189-132">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="b4189-133">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="b4189-133">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="b4189-134">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="b4189-134">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="b4189-135">Metody z parametrami, określając obiektów dynamicznych parametrów</span><span class="sxs-lookup"><span data-stu-id="b4189-135">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="b4189-136">Jak usunąć program obsługi</span><span class="sxs-lookup"><span data-stu-id="b4189-136">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="b4189-137">Jak wywołać metody serwera z poziomu klienta</span><span class="sxs-lookup"><span data-stu-id="b4189-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="b4189-138">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="b4189-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="b4189-139">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="b4189-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="b4189-140">Włączanie rejestrowania po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="b4189-140">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="b4189-141">WPF, Silverlight oraz przykłady kodu aplikacji konsoli dla metod klienta, które można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="b4189-141">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="b4189-142">Przykładowych projektach klientów platformy .NET na ten temat można znaleźć w następujących zasobach:</span><span class="sxs-lookup"><span data-stu-id="b4189-142">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="b4189-143">[gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) w witrynie GitHub.com (przykłady aplikacji konsoli WinRT, Silverlight,).</span><span class="sxs-lookup"><span data-stu-id="b4189-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="b4189-144">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) w witrynie GitHub.com (np. WPF).</span><span class="sxs-lookup"><span data-stu-id="b4189-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="b4189-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) w witrynie GitHub.com (np. aplikację konsoli).</span><span class="sxs-lookup"><span data-stu-id="b4189-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="b4189-146">Dokumentacja na temat programu server lub klientów języka JavaScript, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="b4189-146">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="b4189-147">Podręcznik interfejsu API centrów SignalR — serwer</span><span class="sxs-lookup"><span data-stu-id="b4189-147">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="b4189-148">Podręcznik interfejsu API centrów SignalR — klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="b4189-148">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="b4189-149">Łącza do tematów, dokumentacja interfejsu API są .NET 4.5 wersję interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b4189-149">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="b4189-150">Jeśli używasz programu .NET 4, zobacz [tematy interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="b4189-150">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="b4189-151">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="b4189-151">Client setup</span></span>

<span data-ttu-id="b4189-152">Zainstaluj [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pakietu NuGet (nie [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) pakietu).</span><span class="sxs-lookup"><span data-stu-id="b4189-152">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="b4189-153">Ten pakiet obsługuje WinRT, Silverlight, WPF, aplikacji konsoli i klienci Windows Phone, zarówno dla platformy .NET 4 i .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="b4189-153">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="b4189-154">Jeśli wersji biblioteki SignalR, że masz na komputerze klienckim jest inna niż wersja, która ma na serwerze, SignalR jest zazwyczaj możliwość dostosowania różnicy.</span><span class="sxs-lookup"><span data-stu-id="b4189-154">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="b4189-155">Na przykład serwer z systemem SignalR w wersji 2 będzie obsługiwać klientów, którzy mają zainstalowane 1.1.x, a także klientów, którzy mają w wersji 2 zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="b4189-155">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="b4189-156">Jeśli różnica między wersją na serwerze i wersji na komputerze klienckim jest zbyt duża, lub jeśli klient jest nowsza niż na serwerze, SignalR zgłasza `InvalidOperationException` wyjątku, gdy klient próbuje nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="b4189-156">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="b4189-157">Komunikat o błędzie "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="b4189-157">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="b4189-158">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="b4189-158">How to establish a connection</span></span>

<span data-ttu-id="b4189-159">Zanim usługa może nawiązać połączenie, należy utworzyć `HubConnection` obiektu i tworzyć serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="b4189-159">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="b4189-160">Aby nawiązać połączenie, należy wywołać `Start` metody `HubConnection` obiektu.</span><span class="sxs-lookup"><span data-stu-id="b4189-160">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="b4189-161">W przypadku klientów JavaScript należy zarejestrować co najmniej jeden program obsługi zdarzeń przed wywołaniem `Start` metodę, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="b4189-161">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="b4189-162">To nie jest wymagane dla klientów programu .NET.</span><span class="sxs-lookup"><span data-stu-id="b4189-162">This is not necessary for .NET clients.</span></span> <span data-ttu-id="b4189-163">Dla klientów JavaScript wygenerowany serwer proxy automatycznie tworzy kod serwery proxy do wszystkich centrów znajdujące się na serwerze i rejestrowania procedury obsługi jest wskazuje, które koncentratorów klienta zamierza użyć.</span><span class="sxs-lookup"><span data-stu-id="b4189-163">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="b4189-164">Ale dla klienta programu .NET utworzyć serwery proxy Centrum ręcznie, więc SignalR przyjęto założenie, że będziesz używać dowolne utworzonego serwera proxy dla Centrum.</span><span class="sxs-lookup"><span data-stu-id="b4189-164">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="b4189-165">Przykładowy kod używa domyślnej "/ signalr" adres URL, aby nawiązać połączenie z usługą SignalR.</span><span class="sxs-lookup"><span data-stu-id="b4189-165">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="b4189-166">Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="b4189-166">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="b4189-167">`Start` Metoda jest wykonywana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="b4189-167">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="b4189-168">Aby upewnić się, że kolejne wiersze kodu nie wykonany dopóki, po nawiązaniu połączenia, należy użyć `await` w metodzie asynchronicznej, ASP.NET 4.5 lub `.Wait()` w metodzie synchronicznej.</span><span class="sxs-lookup"><span data-stu-id="b4189-168">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="b4189-169">Nie używaj `.Wait()` w kliencie WinRT.</span><span class="sxs-lookup"><span data-stu-id="b4189-169">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="b4189-170">Połączenia między domenami od klientów programu Silverlight</span><span class="sxs-lookup"><span data-stu-id="b4189-170">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="b4189-171">Aby uzyskać informacje o sposobie włączania połączeń między domenami od klientów programu Silverlight, zobacz [wprowadzania Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="b4189-171">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="b4189-172">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="b4189-172">How to configure the connection</span></span>

<span data-ttu-id="b4189-173">Przed nawiązaniem połączenia, można określić dowolną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="b4189-173">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="b4189-174">Limit połączeń współbieżnych.</span><span class="sxs-lookup"><span data-stu-id="b4189-174">Concurrent connections limit.</span></span>
- <span data-ttu-id="b4189-175">Parametry ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="b4189-175">Query string parameters.</span></span>
- <span data-ttu-id="b4189-176">Metoda transportu.</span><span class="sxs-lookup"><span data-stu-id="b4189-176">The transport method.</span></span>
- <span data-ttu-id="b4189-177">Nagłówki HTTP.</span><span class="sxs-lookup"><span data-stu-id="b4189-177">HTTP headers.</span></span>
- <span data-ttu-id="b4189-178">Certyfikaty klienta.</span><span class="sxs-lookup"><span data-stu-id="b4189-178">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="b4189-179">Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF</span><span class="sxs-lookup"><span data-stu-id="b4189-179">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="b4189-180">W klientach programu WPF trzeba zwiększyć maksymalną liczbę równoczesnych połączeń od jego wartość domyślną 2.</span><span class="sxs-lookup"><span data-stu-id="b4189-180">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="b4189-181">Zalecana wartość wynosi 10.</span><span class="sxs-lookup"><span data-stu-id="b4189-181">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="b4189-182">Aby uzyskać więcej informacji, zobacz [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4189-182">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="b4189-183">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="b4189-183">How to specify query string parameters</span></span>

<span data-ttu-id="b4189-184">Jeśli chcesz wysyłać dane do serwera, gdy klient nawiąże połączenie, można dodać parametry ciągu zapytania do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="b4189-184">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="b4189-185">Poniższy przykład pokazuje, jak ustawić parametr ciągu zapytania w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="b4189-185">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="b4189-186">Poniższy przykład pokazuje, jak odczytywać parametr ciągu zapytania w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="b4189-186">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="b4189-187">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="b4189-187">How to specify the transport method</span></span>

<span data-ttu-id="b4189-188">Częścią procesu nawiązywania połączenia klienta SignalR zwykle negocjuje z serwerem, aby określić najlepsze transportu, która jest obsługiwana przez serwer i klienta.</span><span class="sxs-lookup"><span data-stu-id="b4189-188">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="b4189-189">Jeśli znasz już transport, którego chcesz użyć, można pominąć ten proces negocjacji.</span><span class="sxs-lookup"><span data-stu-id="b4189-189">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="b4189-190">Aby określić metodę transportu, należy przekazać w obiekcie transportu do metody uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="b4189-190">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="b4189-191">Poniższy przykład pokazuje, jak określić metodę transportu w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="b4189-191">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="b4189-192">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) przestrzeń nazw zawiera następujące klasy, używanych do określania transportu.</span><span class="sxs-lookup"><span data-stu-id="b4189-192">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="b4189-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="b4189-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="b4189-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="b4189-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="b4189-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (dostępne tylko wtedy, gdy serwera i klienta użyj .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="b4189-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="b4189-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (automatycznie wybiera najlepsze transportu, która jest obsługiwana zarówno przez klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="b4189-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="b4189-197">Jest to domyślny transport.</span><span class="sxs-lookup"><span data-stu-id="b4189-197">This is the default transport.</span></span> <span data-ttu-id="b4189-198">Te informacje w celu przekazywania `Start` metoda ma taki sam skutek jak nie przechodzi coś.)</span><span class="sxs-lookup"><span data-stu-id="b4189-198">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="b4189-199">ForeverFrame transport jest niedostępna na tej liście, ponieważ jest używany tylko przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b4189-199">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="b4189-200">Aby uzyskać informacje o sposobie Sprawdź metodą transportu w kodzie serwera, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - sposób uzyskiwania informacji na temat klienta z właściwości kontekstu](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="b4189-200">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="b4189-201">Aby uzyskać więcej informacji dotyczących transportu i planów awaryjnych, zobacz [wprowadzenie do SignalR - transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="b4189-201">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="b4189-202">Jak określić nagłówki HTTP</span><span class="sxs-lookup"><span data-stu-id="b4189-202">How to specify HTTP headers</span></span>

<span data-ttu-id="b4189-203">Aby ustawić nagłówki HTTP, użyj `Headers` właściwości w obiekcie połączenia.</span><span class="sxs-lookup"><span data-stu-id="b4189-203">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="b4189-204">Poniższy przykład pokazuje, jak dodać nagłówek HTTP.</span><span class="sxs-lookup"><span data-stu-id="b4189-204">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="b4189-205">Jak określić certyfikaty klienta</span><span class="sxs-lookup"><span data-stu-id="b4189-205">How to specify client certificates</span></span>

<span data-ttu-id="b4189-206">Aby dodać certyfikaty klienta, należy użyć `AddClientCertificate` metody na obiekt połączenia.</span><span class="sxs-lookup"><span data-stu-id="b4189-206">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="b4189-207">Jak utworzyć serwer proxy koncentratora</span><span class="sxs-lookup"><span data-stu-id="b4189-207">How to create the Hub proxy</span></span>

<span data-ttu-id="b4189-208">Aby zdefiniować metody dla klienta, który może wywołać koncentrator, z serwera i wywoływanie metod koncentratora na serwerze, należy utworzyć serwer proxy dla koncentratora przez wywołanie metody `CreateHubProxy` w obiekcie połączenia.</span><span class="sxs-lookup"><span data-stu-id="b4189-208">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="b4189-209">Ciąg przekazanej do `CreateHubProxy` jest nazwą klasy koncentratora lub nazwa określona przez `HubName` atrybutu, jeśli użyto jednego na serwerze.</span><span class="sxs-lookup"><span data-stu-id="b4189-209">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="b4189-210">Dopasowywaniu nazwy nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="b4189-210">Name matching is case-insensitive.</span></span>

<span data-ttu-id="b4189-211">**Klasy koncentratora na serwerze**</span><span class="sxs-lookup"><span data-stu-id="b4189-211">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="b4189-212">**Utwórz serwer proxy klienta dla klasy koncentratora**</span><span class="sxs-lookup"><span data-stu-id="b4189-212">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="b4189-213">Jeśli dekoracji klasy koncentratora z `HubName` atrybutu, należy użyć tej nazwy.</span><span class="sxs-lookup"><span data-stu-id="b4189-213">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="b4189-214">**Klasy koncentratora na serwerze**</span><span class="sxs-lookup"><span data-stu-id="b4189-214">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="b4189-215">**Utwórz serwer proxy klienta dla klasy koncentratora**</span><span class="sxs-lookup"><span data-stu-id="b4189-215">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="b4189-216">Jeśli wywołasz `HubConnection.CreateHubProxy` wiele razy z takimi samymi `hubName`, możesz uzyskać takie same buforowane `IHubProxy` obiektu.</span><span class="sxs-lookup"><span data-stu-id="b4189-216">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="b4189-217">Sposób definiowania metody na kliencie, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="b4189-217">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="b4189-218">Aby zdefiniować metodę, która może wywołać serwera, należy użyć serwera proxy `On` metodę, aby zarejestrować program obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="b4189-218">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="b4189-219">Dopasowywaniu nazwy metody nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="b4189-219">Method name matching is case-insensitive.</span></span> <span data-ttu-id="b4189-220">Na przykład `Clients.All.UpdateStockPrice` będą wykonywane na serwerze `updateStockPrice`, `updatestockprice`, lub `UpdateStockPrice` na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="b4189-220">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="b4189-221">Innego klienta platformy mają różne wymagania dotyczące sposobu pisania kodu metody, aby zaktualizować interfejs użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b4189-221">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="b4189-222">Przykłady zamieszczone dotyczą klientów WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="b4189-222">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="b4189-223">WPF, Silverlight oraz przykłady aplikacji konsoli znajdują się w [osobnej sekcji w dalszej części tego tematu](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="b4189-223">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="b4189-224">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="b4189-224">Methods without parameters</span></span>

<span data-ttu-id="b4189-225">Jeśli metoda jest obsługa nie ma parametrów, użyj przeciążenia nieogólnego `On` metody:</span><span class="sxs-lookup"><span data-stu-id="b4189-225">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="b4189-226">**Kod serwera podczas wywoływania metody klienta bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="b4189-226">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="b4189-227">**Kod klienta WinRT metody wywoływane z serwera bez parametrów ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="b4189-227">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="b4189-228">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="b4189-228">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="b4189-229">Jeśli metoda jest obsługa ma parametry, należy określić typy parametrów jako typów ogólnego `On` metody.</span><span class="sxs-lookup"><span data-stu-id="b4189-229">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="b4189-230">Istnieją przeciążenia ogólne `On` metody umożliwiają określenie parametrów do 8 (4 w systemie Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="b4189-230">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="b4189-231">W poniższym przykładzie jeden parametr jest wysyłane do `UpdateStockPrice` metody.</span><span class="sxs-lookup"><span data-stu-id="b4189-231">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="b4189-232">**Kod serwera podczas wywoływania metody klienta z parametrem**</span><span class="sxs-lookup"><span data-stu-id="b4189-232">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="b4189-233">**Klasy zapasów użytym dla parametru**</span><span class="sxs-lookup"><span data-stu-id="b4189-233">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="b4189-234">**Kod klienta WinRT dla metody wywoływane z serwera za pomocą parametru ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="b4189-234">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="b4189-235">Metody z parametrami, określając obiektów dynamicznych parametrów</span><span class="sxs-lookup"><span data-stu-id="b4189-235">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="b4189-236">Jako alternatywę do określania parametrów jako typów ogólnego `On` metody, parametry można określić jako obiektów dynamicznych:</span><span class="sxs-lookup"><span data-stu-id="b4189-236">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="b4189-237">**Kod serwera podczas wywoływania metody klienta z parametrem**</span><span class="sxs-lookup"><span data-stu-id="b4189-237">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="b4189-238">**Klasy zapasów użytym dla parametru**</span><span class="sxs-lookup"><span data-stu-id="b4189-238">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="b4189-239">**Kod klienta WinRT dla metody wywoływane z serwera z parametrem przy użyciu obiekt dynamiczny parametr ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="b4189-239">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="b4189-240">Jak usunąć program obsługi</span><span class="sxs-lookup"><span data-stu-id="b4189-240">How to remove a handler</span></span>

<span data-ttu-id="b4189-241">Aby usunąć program obsługi, należy wywołać jej `Dispose` metody.</span><span class="sxs-lookup"><span data-stu-id="b4189-241">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="b4189-242">**Kod klienta dla metodę o nazwie z serwera**</span><span class="sxs-lookup"><span data-stu-id="b4189-242">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="b4189-243">**Kod klienta, aby usunąć program obsługi**</span><span class="sxs-lookup"><span data-stu-id="b4189-243">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="b4189-244">Jak wywołać metody serwera z poziomu klienta</span><span class="sxs-lookup"><span data-stu-id="b4189-244">How to call server methods from the client</span></span>

<span data-ttu-id="b4189-245">Aby wywołać metodę na serwerze, należy użyć `Invoke` metody serwera proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="b4189-245">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="b4189-246">Jeśli metoda nie zwraca żadnej wartości, użyj przeciążenia nieogólnego `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="b4189-246">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="b4189-247">**Kod serwera dla metody, która nie zwraca żadnej wartości**</span><span class="sxs-lookup"><span data-stu-id="b4189-247">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="b4189-248">**Kod klienta, wywołanie metody, która nie zwraca żadnej wartości**</span><span class="sxs-lookup"><span data-stu-id="b4189-248">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="b4189-249">Jeśli metoda nie zwraca wartości, należy określić typ zwracany jako ogólny typ `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="b4189-249">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="b4189-250">**Kod serwera dla metody, która nie zwraca wartości i przyjmuje parametr typu złożonego**</span><span class="sxs-lookup"><span data-stu-id="b4189-250">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="b4189-251">**Klasy zapasów użytym dla parametru i zwracają wartość**</span><span class="sxs-lookup"><span data-stu-id="b4189-251">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="b4189-252">**Kod klienta, wywołanie metody, która nie zwraca wartości i przyjmuje parametr typu złożonego w metodzie asynchronicznej ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="b4189-252">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="b4189-253">**Kod klienta, wywołanie metody, która nie zwraca wartości i przyjmuje parametr typu złożonego, metoda synchroniczna**</span><span class="sxs-lookup"><span data-stu-id="b4189-253">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="b4189-254">`Invoke` Metoda wykonuje asynchronicznie i zwraca `Task` obiektu.</span><span class="sxs-lookup"><span data-stu-id="b4189-254">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="b4189-255">Jeśli nie określisz `await` lub `.Wait()`, następnego wiersza kodu zostaną wykonane, zanim metodę, która wywołuje zakończenia.</span><span class="sxs-lookup"><span data-stu-id="b4189-255">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="b4189-256">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="b4189-256">How to handle connection lifetime events</span></span>

<span data-ttu-id="b4189-257">Biblioteka SignalR udostępnia następujące połączenia zdarzenia okresu istnienia, które może obsłużyć:</span><span class="sxs-lookup"><span data-stu-id="b4189-257">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="b4189-258">`Received`: Zgłaszane w przypadku nieodebrania żadnych danych w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="b4189-258">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="b4189-259">Udostępnia odebrane dane.</span><span class="sxs-lookup"><span data-stu-id="b4189-259">Provides the received data.</span></span>
- <span data-ttu-id="b4189-260">`ConnectionSlow`: Wywoływane, gdy klient wykrywa wolno lub często porzucanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="b4189-260">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="b4189-261">`Reconnecting`: Wywoływane, gdy transportu źródłowego rozpoczyna ponowne nawiązywanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="b4189-261">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="b4189-262">`Reconnected`: Wywoływane, gdy nawiązał transportu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="b4189-262">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="b4189-263">`StateChanged`: Wywoływane, gdy zmieni się stan połączenia.</span><span class="sxs-lookup"><span data-stu-id="b4189-263">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="b4189-264">Zawiera stan stary i nowy stan.</span><span class="sxs-lookup"><span data-stu-id="b4189-264">Provides the old state and the new state.</span></span> <span data-ttu-id="b4189-265">Aby uzyskać informacje na temat połączenia wartości stanu zobacz [wyliczenie Element ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="b4189-265">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="b4189-266">`Closed`: Wywoływane, gdy połączenie zostało rozłączone.</span><span class="sxs-lookup"><span data-stu-id="b4189-266">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="b4189-267">Na przykład, jeśli chcesz wyświetlić komunikaty ostrzegawcze dotyczące błędów, które nie są one krytyczny, ale powodują sporadyczne problemy z połączeniem, takie jak powolność lub zbyt częstej porzucenie połączenia, obsługi `ConnectionSlow` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="b4189-267">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="b4189-268">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia w SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="b4189-268">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="b4189-269">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="b4189-269">How to handle errors</span></span>

<span data-ttu-id="b4189-270">Jeśli nie włączysz jawnie szczegółowe komunikaty o błędach na serwerze, obiekt wyjątku, który zwraca SignalR, po wystąpieniu błędu zawiera minimalne informacje o tym błędzie.</span><span class="sxs-lookup"><span data-stu-id="b4189-270">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="b4189-271">Na przykład, jeśli wywołanie `newContosoChatMessage` zawiera komunikat o błędzie w obiekcie błąd zakończy się niepowodzeniem, "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Wysyłanie szczegółowe komunikaty o błędach do klientów w środowisku produkcyjnym jest niezalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach dla rozwiązywania problemów, użyj poniższego kodu, na serwerze.</span><span class="sxs-lookup"><span data-stu-id="b4189-271">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="b4189-272">Do obsługi błędów, które wywołuje SignalR, można dodać program obsługi `Error` zdarzenie na obiekt połączenia.</span><span class="sxs-lookup"><span data-stu-id="b4189-272">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="b4189-273">Do obsługi błędów z wywołań metod, zawijania kodu w bloku try-catch.</span><span class="sxs-lookup"><span data-stu-id="b4189-273">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="b4189-274">Włączanie rejestrowania po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="b4189-274">How to enable client-side logging</span></span>

<span data-ttu-id="b4189-275">Aby włączyć rejestrowanie klienta, należy ustawić `TraceLevel` i `TraceWriter` właściwości w obiekcie połączenia.</span><span class="sxs-lookup"><span data-stu-id="b4189-275">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="b4189-276">WPF, Silverlight oraz przykłady kodu aplikacji konsoli dla metod klienta, które można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="b4189-276">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="b4189-277">Przykłady kodu, w przedstawionej wcześniej do definiowania metody klientów, które serwer może wywoływać dotyczą klientów WinRT.</span><span class="sxs-lookup"><span data-stu-id="b4189-277">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="b4189-278">Poniższe przykłady pokazują równoważny kod dla WPF, Silverlight oraz klientów aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="b4189-278">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="b4189-279">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="b4189-279">Methods without parameters</span></span>

<span data-ttu-id="b4189-280">**WPF kodu klienta dla metodę o nazwie z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="b4189-280">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="b4189-281">**Kod klienta Silverlight dla metodę o nazwie z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="b4189-281">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="b4189-282">**Kod klienta aplikacji konsolowej, metoda jest wywoływana z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="b4189-282">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="b4189-283">Metody z parametrami, określając typy parametrów</span><span class="sxs-lookup"><span data-stu-id="b4189-283">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="b4189-284">**WPF kodu klienta dla metodę o nazwie z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="b4189-284">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="b4189-285">**Kod klienta Silverlight dla metodę o nazwie z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="b4189-285">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="b4189-286">**Kod klienta aplikacji konsolowej, metoda jest wywoływana z serwera za pomocą parametru**</span><span class="sxs-lookup"><span data-stu-id="b4189-286">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="b4189-287">Metody z parametrami, określając obiektów dynamicznych parametrów</span><span class="sxs-lookup"><span data-stu-id="b4189-287">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="b4189-288">**WPF kodu klienta dla metodę o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny parametr**</span><span class="sxs-lookup"><span data-stu-id="b4189-288">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="b4189-289">**Kod klienta Silverlight dla metodę o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny parametr**</span><span class="sxs-lookup"><span data-stu-id="b4189-289">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="b4189-290">**Kod klienta aplikacji konsoli dla metody wywoływane z serwera z parametrem przy użyciu obiekt dynamiczny parametr**</span><span class="sxs-lookup"><span data-stu-id="b4189-290">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
