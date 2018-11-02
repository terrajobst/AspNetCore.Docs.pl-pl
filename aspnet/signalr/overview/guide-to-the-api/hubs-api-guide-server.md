---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Podręcznik biblioteki SignalR platformy ASP.NET do interfejsu API centrów — serwer (C#) | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API centrów SignalR platformy ASP.NET dla elementu SignalR w wersji 2, przy użyciu przykładów kodu, demonstrując...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 6545491cfa36bb9fee555eb0348ec0a319bff470
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758248"
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="4bf90-103">Podręcznik biblioteki SignalR platformy ASP.NET do interfejsu API centrów — serwer (C#)</span><span class="sxs-lookup"><span data-stu-id="4bf90-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>
====================
<span data-ttu-id="4bf90-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4bf90-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="4bf90-105">Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API centrów SignalR platformy ASP.NET dla elementu SignalR w wersji 2, przy użyciu przykładów kodu, demonstrując typowe opcje.</span><span class="sxs-lookup"><span data-stu-id="4bf90-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="4bf90-106">Interfejsu API centrów SignalR umożliwia zdalne wywołania procedur (RPC) z serwera do połączonych klientów i od klientów z serwerem.</span><span class="sxs-lookup"><span data-stu-id="4bf90-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="4bf90-107">W kodzie serwera należy zdefiniować metody, które mogą być wywoływane przez klientów, a wywołanie metody, które są uruchamiane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="4bf90-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="4bf90-108">W kodzie klienta definiowania metod, które mogą być wywoływane z serwera, a wywołanie metody, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="4bf90-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="4bf90-109">SignalR zajmuje się wszystkie nadmiar klient serwer dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="4bf90-110">SignalR oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="4bf90-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="4bf90-111">Wprowadzenie do SignalR, centra i połączenia trwałego, zobacz [wprowadzenie do SignalR 2](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="4bf90-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="4bf90-112">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="4bf90-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="4bf90-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4bf90-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="4bf90-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4bf90-114">.NET 4.5</span></span>
> - <span data-ttu-id="4bf90-115">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="4bf90-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="4bf90-116">Wersje tematu</span><span class="sxs-lookup"><span data-stu-id="4bf90-116">Topic versions</span></span>
> 
> <span data-ttu-id="4bf90-117">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="4bf90-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="4bf90-118">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="4bf90-118">Questions and comments</span></span>
> 
> <span data-ttu-id="4bf90-119">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="4bf90-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4bf90-120">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="4bf90-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="4bf90-121">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4bf90-121">Overview</span></span>

<span data-ttu-id="4bf90-122">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="4bf90-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="4bf90-123">Jak zarejestrować oprogramowanie pośredniczące SignalR</span><span class="sxs-lookup"><span data-stu-id="4bf90-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="4bf90-124">Adres URL /signalr</span><span class="sxs-lookup"><span data-stu-id="4bf90-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="4bf90-125">Konfigurowanie opcji SignalR</span><span class="sxs-lookup"><span data-stu-id="4bf90-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="4bf90-126">Jak utworzyć i używać klas Centrum</span><span class="sxs-lookup"><span data-stu-id="4bf90-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="4bf90-127">Okres istnienia obiektu Centrum</span><span class="sxs-lookup"><span data-stu-id="4bf90-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="4bf90-128">Wielkości liter pisane Centrum nazw klientów języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="4bf90-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="4bf90-129">Wiele centrów</span><span class="sxs-lookup"><span data-stu-id="4bf90-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="4bf90-130">Koncentratory silnie Typizowane</span><span class="sxs-lookup"><span data-stu-id="4bf90-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="4bf90-131">Sposób definiowania metody w klasie Centrum, który można wywoływać klientów</span><span class="sxs-lookup"><span data-stu-id="4bf90-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="4bf90-132">Pisane-wielkość liter w wyrazie nazwy metod w klientów języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="4bf90-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="4bf90-133">Kiedy są wykonywane asynchronicznie</span><span class="sxs-lookup"><span data-stu-id="4bf90-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="4bf90-134">Definiowanie przeciążenia</span><span class="sxs-lookup"><span data-stu-id="4bf90-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="4bf90-135">Raport z postępów z wywołań metod koncentratora</span><span class="sxs-lookup"><span data-stu-id="4bf90-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="4bf90-136">Sposób wywoływania metody z klasy koncentratora klienta</span><span class="sxs-lookup"><span data-stu-id="4bf90-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="4bf90-137">Wybieranie klientów, którzy będą otrzymywać RPC</span><span class="sxs-lookup"><span data-stu-id="4bf90-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="4bf90-138">Nazwy metod weryfikacji kompilacji</span><span class="sxs-lookup"><span data-stu-id="4bf90-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="4bf90-139">Dopasowywanie nazw bez uwzględniania wielkości liter — metoda</span><span class="sxs-lookup"><span data-stu-id="4bf90-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="4bf90-140">Wykonanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="4bf90-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="4bf90-141">Sposób zarządzania członkostwa w grupie z klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="4bf90-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="4bf90-142">Asynchroniczne wykonywanie metody dodawania i usuwania</span><span class="sxs-lookup"><span data-stu-id="4bf90-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="4bf90-143">Trwałość członkostwa grupy</span><span class="sxs-lookup"><span data-stu-id="4bf90-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="4bf90-144">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="4bf90-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="4bf90-145">Jak obsługiwać zdarzenia okresu istnienia połączenia w klasie Centrum</span><span class="sxs-lookup"><span data-stu-id="4bf90-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="4bf90-146">Kiedy są wywoływane onconnected elementu ondisconnected elementu i onreconnected elementu</span><span class="sxs-lookup"><span data-stu-id="4bf90-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="4bf90-147">Stan elementu wywołującego, pusta</span><span class="sxs-lookup"><span data-stu-id="4bf90-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="4bf90-148">Jak uzyskać informacji o kliencie z właściwości kontekstu</span><span class="sxs-lookup"><span data-stu-id="4bf90-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="4bf90-149">Jak przekazać stanu między klientami a klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="4bf90-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="4bf90-150">Sposób obsługi błędów w klasie Centrum</span><span class="sxs-lookup"><span data-stu-id="4bf90-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="4bf90-151">Jak wywołać metody klienta grup i zarządzanie nimi z poza klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="4bf90-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="4bf90-152">Wywoływanie metody klienta</span><span class="sxs-lookup"><span data-stu-id="4bf90-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="4bf90-153">Członkostwo w grupie zarządzania</span><span class="sxs-lookup"><span data-stu-id="4bf90-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="4bf90-154">Jak włączyć śledzenie</span><span class="sxs-lookup"><span data-stu-id="4bf90-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="4bf90-155">Jak dostosować potoku koncentratory</span><span class="sxs-lookup"><span data-stu-id="4bf90-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="4bf90-156">Dokumentacja na temat klientów programu na ten temat można znaleźć w następujących zasobach:</span><span class="sxs-lookup"><span data-stu-id="4bf90-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="4bf90-157">Podręcznik interfejsu API centrów SignalR — klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="4bf90-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="4bf90-158">Podręcznik interfejsu API centrów SignalR — klient modelu .NET</span><span class="sxs-lookup"><span data-stu-id="4bf90-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="4bf90-159">Składniki serwera dla SignalR 2 są dostępne tylko w .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="4bf90-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="4bf90-160">Serwery z systemem .NET 4.0, należy użyć SignalR v1.x.</span><span class="sxs-lookup"><span data-stu-id="4bf90-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="4bf90-161">Jak zarejestrować oprogramowanie pośredniczące SignalR</span><span class="sxs-lookup"><span data-stu-id="4bf90-161">How to register SignalR middleware</span></span>

<span data-ttu-id="4bf90-162">Aby zdefiniować trasę, którego klienci będą używać, aby nawiązać połączenie z Centrum, należy wywołać `MapSignalR` metoda podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4bf90-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="4bf90-163">`MapSignalR` jest [— metoda rozszerzenia](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) dla `OwinExtensions` klasy.</span><span class="sxs-lookup"><span data-stu-id="4bf90-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="4bf90-164">Poniższy przykład pokazuje jak zdefiniować trasy centrów SignalR, używając klasy początkowej OWIN.</span><span class="sxs-lookup"><span data-stu-id="4bf90-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="4bf90-165">W przypadku dodawania funkcji SignalR do aplikacji ASP.NET MVC, upewnij się, że trasa SignalR jest dodawana przed innych tras.</span><span class="sxs-lookup"><span data-stu-id="4bf90-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="4bf90-166">Aby uzyskać więcej informacji, zobacz [samouczek: wprowadzenie do SignalR 2 i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="4bf90-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="4bf90-167">Adres URL /signalr</span><span class="sxs-lookup"><span data-stu-id="4bf90-167">The /signalr URL</span></span>

<span data-ttu-id="4bf90-168">Domyślnie jest adres URL trasy, której klienci będą używać, aby nawiązać połączenie z Centrum "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="4bf90-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="4bf90-169">(Nie należy mylić tego adresu URL za pomocą adresu URL "/ signalr/centra", który jest automatycznie wygenerowanego pliku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4bf90-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="4bf90-170">Aby uzyskać więcej informacji na temat wygenerowany serwer proxy, zobacz [Podręcznik interfejsu API centrów SignalR — klient JavaScript — wygenerowany serwer proxy i przeznaczenie dla Ciebie](hubs-api-guide-javascript-client.md#genproxy).)</span><span class="sxs-lookup"><span data-stu-id="4bf90-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="4bf90-171">Może to być nadzwyczajne okoliczności, które to podstawowy adres URL nie można używać dla elementu SignalR; na przykład masz folder z projektu o nazwie *signalr* i nie chcesz zmienić nazwę.</span><span class="sxs-lookup"><span data-stu-id="4bf90-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="4bf90-172">W takim przypadku można zmienić podstawowy adres URL, jak pokazano w poniższych przykładach (Zastąp "/ signalr" w przykładowym kodzie za pomocą żądany adres URL).</span><span class="sxs-lookup"><span data-stu-id="4bf90-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="4bf90-173">**Kod serwera, który określa adres URL**</span><span class="sxs-lookup"><span data-stu-id="4bf90-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="4bf90-174">**Kod klienta JavaScript, który określa adres URL (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="4bf90-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="4bf90-175">**Kod klienta JavaScript, który określa adres URL (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="4bf90-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="4bf90-176">**Kod klienta platformy .NET, który określa adres URL**</span><span class="sxs-lookup"><span data-stu-id="4bf90-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="4bf90-177">Konfigurowanie opcji SignalR</span><span class="sxs-lookup"><span data-stu-id="4bf90-177">Configuring SignalR Options</span></span>

<span data-ttu-id="4bf90-178">Przeciążenia `MapSignalR` metody umożliwiają określenie niestandardowego adresu URL, mechanizm rozpoznawania zależności niestandardowej i następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="4bf90-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="4bf90-179">Włącz wywołania między domenami przy użyciu mechanizmu CORS i JSONP z klientów przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="4bf90-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="4bf90-180">Zazwyczaj, gdy przeglądarka ładuje stronę z `http://contoso.com`, połączenia SignalR jest w tej samej domenie, w `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="4bf90-181">Jeśli strona z `http://contoso.com` nawiązuje połączenie `http://fabrikam.com/signalr`, oznacza to połączenie między domenami.</span><span class="sxs-lookup"><span data-stu-id="4bf90-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="4bf90-182">Ze względów bezpieczeństwa połączeń między domenami są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="4bf90-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="4bf90-183">Aby uzyskać więcej informacji, zobacz [ASP.NET SignalR Podręcznik interfejsu API centrów — klient JavaScript — jak nawiązać połączenie między domenami](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="4bf90-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="4bf90-184">Włącz szczegółowe komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="4bf90-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="4bf90-185">Jeśli wystąpią błędy, domyślne zachowanie SignalR jest wysyłania do klientów komunikat z powiadomieniem bez szczegółowe informacje o co się stało.</span><span class="sxs-lookup"><span data-stu-id="4bf90-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="4bf90-186">Szczegółowe informacje o błędzie, wysyłają do klientów nie jest zalecane w środowisku produkcyjnym, ponieważ złośliwi użytkownicy mogą być w stanie użycie informacji o ataku na aplikację.</span><span class="sxs-lookup"><span data-stu-id="4bf90-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="4bf90-187">Na potrzeby rozwiązywania problemów można użyć tej opcji tymczasowo włączyć bardziej szczegółowy raportowanie błędów.</span><span class="sxs-lookup"><span data-stu-id="4bf90-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="4bf90-188">Wyłącz automatycznie generowanych plików serwera proxy JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4bf90-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="4bf90-189">Domyślnie plik języka JavaScript przy użyciu serwera proxy dla Twoich zajęciach Centrum jest generowany w odpowiedzi na adres URL "/ signalr/centra".</span><span class="sxs-lookup"><span data-stu-id="4bf90-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="4bf90-190">Jeśli nie chcesz używać serwery proxy JavaScript lub jeśli chcesz ręcznie wygenerować ten plik i odnoszą się do pliku fizycznego w klientach, można użyć tej opcji można wyłączyć generowania serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="4bf90-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="4bf90-191">Aby uzyskać więcej informacji, zobacz [Podręcznik interfejsu API centrów SignalR — klient JavaScript — sposobu tworzenia pliku fizycznego dla elementu SignalR wygenerowany serwer proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="4bf90-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="4bf90-192">Poniższy przykład pokazuje, jak określić te opcje i adres URL połączenia SignalR w wywołaniu `MapSignalR` metody.</span><span class="sxs-lookup"><span data-stu-id="4bf90-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="4bf90-193">Aby określić niestandardowy adres URL, zastąp "/ signalr" w przykładzie, za pomocą adresu URL, którego chcesz używać.</span><span class="sxs-lookup"><span data-stu-id="4bf90-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="4bf90-194">Jak utworzyć i używać klas Centrum</span><span class="sxs-lookup"><span data-stu-id="4bf90-194">How to create and use Hub classes</span></span>

<span data-ttu-id="4bf90-195">Aby utworzyć Centrum, należy utworzyć klasę, która jest pochodną [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="4bf90-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="4bf90-196">Poniższy przykład pokazuje prosty klasę Centrum aplikacji rozmów.</span><span class="sxs-lookup"><span data-stu-id="4bf90-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="4bf90-197">W tym przykładzie podłączony klient może wywołać `NewContosoChatMessage` metody, a jeśli tak jest, dane otrzymane jest wysłano do wszyscy połączeni klienci.</span><span class="sxs-lookup"><span data-stu-id="4bf90-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="4bf90-198">Okres istnienia obiektu Centrum</span><span class="sxs-lookup"><span data-stu-id="4bf90-198">Hub object lifetime</span></span>

<span data-ttu-id="4bf90-199">Nie utworzyć wystąpienia klasy koncentratora lub wywołać jego metody w kodzie na serwerze; wszystko jest wykonywane przez potok koncentratorów SignalR.</span><span class="sxs-lookup"><span data-stu-id="4bf90-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="4bf90-200">SignalR tworzy nowe wystąpienie klasy koncentratora każdej potrzebny do obsługi operacji koncentratora, np. kiedy klient nawiązuje połączenie, odłącza lub sprawia, że wywołanie metody do serwera.</span><span class="sxs-lookup"><span data-stu-id="4bf90-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="4bf90-201">Ponieważ wystąpienia klasy koncentratora przejściowy, nie możesz użyć ich do zarządzania stanem z jednego wywołania metody do następnego.</span><span class="sxs-lookup"><span data-stu-id="4bf90-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="4bf90-202">Każdorazowo serwer odbiera wywołania metody od klienta, nowe wystąpienie klasy procesów Centrum wiadomości.</span><span class="sxs-lookup"><span data-stu-id="4bf90-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="4bf90-203">Do zarządzania stanem przez wiele połączeń i wywołania metody, należy użyć innej metody takie jak bazy danych lub zmienna statyczna klasy koncentratora lub inną klasę, która nie pochodzi od `Hub`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="4bf90-204">Jeśli będzie się powtarzać dane w pamięci, za pomocą innej metody takie jak zmienna statyczna klasy koncentratora dane zostaną utracone podczas odtwarzania domen aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4bf90-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="4bf90-205">Jeśli chcesz wysyłać komunikaty do klientów z własnego kodu, który działa poza klasy koncentratora, nie przez utworzenie wystąpienia klasy wystąpienie koncentratora, ale możesz to zrobić poprzez odwołanie do obiektu kontekstu SignalR dla swojej klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="4bf90-206">Aby uzyskać więcej informacji, zobacz [jak wywołać metody klienta grup i zarządzanie nimi z poza klasy koncentratora](#callfromoutsidehub) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="4bf90-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="4bf90-207">Wielkości liter pisane Centrum nazw klientów języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="4bf90-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="4bf90-208">Domyślnie klientów języka JavaScript dotyczą Hubs przy użyciu wersji formacie camelcase nazwy klasy.</span><span class="sxs-lookup"><span data-stu-id="4bf90-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="4bf90-209">SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można jest zgodna z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4bf90-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="4bf90-210">Poprzedni przykład może być traktowana jako `contosoChatHub` w kodzie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4bf90-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="4bf90-211">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="4bf90-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="4bf90-212">**Klient JavaScript za pomocą wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="4bf90-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="4bf90-213">Jeśli chcesz określić inną nazwę dla klientów użyć, należy dodać `HubName` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="4bf90-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="4bf90-214">Kiedy używasz `HubName` atrybutu, nastąpiła żadna zmiana nazwy na format camelcase na klientów języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4bf90-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="4bf90-215">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="4bf90-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="4bf90-216">**Klient JavaScript za pomocą wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="4bf90-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="4bf90-217">Wiele centrów</span><span class="sxs-lookup"><span data-stu-id="4bf90-217">Multiple Hubs</span></span>

<span data-ttu-id="4bf90-218">W aplikacji, można zdefiniować wiele klas koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="4bf90-219">Po wykonaniu tej czynności, połączenie jest udostępniane, ale są osobne grupy:</span><span class="sxs-lookup"><span data-stu-id="4bf90-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="4bf90-220">Wszyscy klienci będą używać tego samego adresu URL do nawiązania połączenia SignalR z usługą ("/ signalr" lub adres URL niestandardowej, jeśli została określona), i czy połączenie jest używany przez wszystkie centra zdefiniowane przez usługę.</span><span class="sxs-lookup"><span data-stu-id="4bf90-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="4bf90-221">Nie ma żadnej różnicy wydajności, dla wielu centrów w porównaniu do definiowania wszystkich funkcji w Centrum w jednej klasie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="4bf90-222">Wszystkie centra uzyskać te same informacje o żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="4bf90-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="4bf90-223">Ponieważ wszystkie centra udostępnianie tego samego połączenia, tylko informacje żądania HTTP, który serwer jest, co składa się z oryginalnego żądania HTTP, który nawiązuje połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="4bf90-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="4bf90-224">Jeśli używasz żądanie połączenia do przekazywania informacji z klienta do serwera, określając ciąg zapytania nie może dostarczyć różne ciągi kwerend do różnych centrów.</span><span class="sxs-lookup"><span data-stu-id="4bf90-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="4bf90-225">Wszystkie centra będą otrzymywać te same informacje.</span><span class="sxs-lookup"><span data-stu-id="4bf90-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="4bf90-226">Wygenerowany plik serwery proxy JavaScript będzie zawierać serwerów proxy do wszystkich centrów w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="4bf90-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="4bf90-227">Aby uzyskać informacje o serwery proxy JavaScript, zobacz [Podręcznik interfejsu API centrów SignalR — klient JavaScript — wygenerowany serwer proxy i przeznaczenie dla Ciebie](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="4bf90-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="4bf90-228">Grupy są definiowane w obrębie centrów.</span><span class="sxs-lookup"><span data-stu-id="4bf90-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="4bf90-229">W SignalR, które można zdefiniować nazwę grupy do emisji przeznaczonych do podzestawów połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="4bf90-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="4bf90-230">Grupy są przechowywane osobno dla każdego Centrum.</span><span class="sxs-lookup"><span data-stu-id="4bf90-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="4bf90-231">Na przykład utworzyć grupę o nazwie "Administratorzy" zawierałoby jeden zbiór klientów do Twojej `ContosoChatHub` klasy i tej samej nazwy grupy może odwołać się do innego zestawu klientów dla Twojej `StockTickerHub` klasy.</span><span class="sxs-lookup"><span data-stu-id="4bf90-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="4bf90-232">Koncentratory silnie Typizowane</span><span class="sxs-lookup"><span data-stu-id="4bf90-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="4bf90-233">Aby zdefiniować interfejs dla metod koncentratora, które mogą klienta odwołania i Włącz funkcję Intellisense z metod koncentratora, pochodzi z Centrum `Hub<T>` (zostanie wprowadzony w SignalR 2.1) zamiast `Hub`:</span><span class="sxs-lookup"><span data-stu-id="4bf90-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="4bf90-234">Sposób definiowania metody w klasie Centrum, który można wywoływać klientów</span><span class="sxs-lookup"><span data-stu-id="4bf90-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="4bf90-235">Aby udostępnić metody koncentratora, który ma być wywoływane z klienta, należy zadeklarować metodę publiczną, jak pokazano w poniższych przykładach.</span><span class="sxs-lookup"><span data-stu-id="4bf90-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="4bf90-236">Można określić zwracany typ i parametry, w tym typy złożone i tablice, tak jak w dowolnej metody języka C#.</span><span class="sxs-lookup"><span data-stu-id="4bf90-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="4bf90-237">Wszelkie dane, które wystąpią w parametrach lub zwróć się do obiektu wywołującego jest przekazywana między klientem a serwerem przy użyciu pliku JSON, a SignalR obsługuje wiązania złożone obiekty i tablice obiektów automatycznie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="4bf90-238">Pisane-wielkość liter w wyrazie nazwy metod w klientów języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="4bf90-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="4bf90-239">Domyślnie klientów JavaScript dotyczą metod koncentratora za pomocą wersji formacie camelcase nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="4bf90-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="4bf90-240">SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można jest zgodna z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4bf90-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="4bf90-241">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="4bf90-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="4bf90-242">**Klient JavaScript za pomocą wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="4bf90-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="4bf90-243">Jeśli chcesz określić inną nazwę dla klientów użyć, należy dodać `HubMethodName` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="4bf90-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="4bf90-244">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="4bf90-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="4bf90-245">**Klient JavaScript za pomocą wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="4bf90-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="4bf90-246">Kiedy są wykonywane asynchronicznie</span><span class="sxs-lookup"><span data-stu-id="4bf90-246">When to execute asynchronously</span></span>

<span data-ttu-id="4bf90-247">Jeśli metoda będzie mieć długotrwałych albo do pracy, będzie obejmować oczekiwania, takich jak wyszukiwania w bazie danych lub wywołanie usługi sieci web, upewnij metody koncentratora asynchronicznych, zwracając [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (zamiast `void` zwracają) lub [ Zadanie&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) obiektu (zamiast `T` zwracany typ).</span><span class="sxs-lookup"><span data-stu-id="4bf90-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="4bf90-248">Po powrocie `Task` oczekuje obiektu z metody SignalR `Task` do ukończenia, a następnie wysyła nieopakowane wynik do klienta, więc nie ma żadnej różnicy w jak kod jest wywołanie metody w kliencie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="4bf90-249">Tworzenie metody koncentratora asynchronicznego pozwala uniknąć blokuje połączenia, gdy używa ona transportu protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4bf90-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="4bf90-250">Gdy metoda Centrum jest wykonywana synchronicznie i transport jest WebSocket, kolejnych wywołań metod koncentratora, z tego samego klienta są blokowane, dopiero po zakończeniu metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="4bf90-251">W poniższym przykładzie pokazano taką samą metodę jak kodowane, aby były uruchamiane synchronicznie lub asynchronicznie, a następnie kod klienta JavaScript, który działa w przypadku wywoływania danej wersji.</span><span class="sxs-lookup"><span data-stu-id="4bf90-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="4bf90-252">**Synchroniczne**</span><span class="sxs-lookup"><span data-stu-id="4bf90-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="4bf90-253">**Asynchroniczne**</span><span class="sxs-lookup"><span data-stu-id="4bf90-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="4bf90-254">**Klient JavaScript za pomocą wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="4bf90-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="4bf90-255">Aby uzyskać więcej informacji o sposobie używania metod asynchronicznych w programie ASP.NET 4.5, zobacz [przy użyciu metod asynchronicznych w ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="4bf90-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="4bf90-256">Definiowanie przeciążenia</span><span class="sxs-lookup"><span data-stu-id="4bf90-256">Defining Overloads</span></span>

<span data-ttu-id="4bf90-257">Jeśli chcesz zdefiniować przeciążenia metody, liczba parametrów w każdym przeciążenia musi być różne.</span><span class="sxs-lookup"><span data-stu-id="4bf90-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="4bf90-258">W przypadku przeciążenia różnicuje się tylko, określając typy różnych parametrów, zostanie skompilowana klasa Centrum, ale usługi SignalR spowoduje zgłoszenie wyjątku w czasie wykonywania, gdy klienci spróbują do wywołania, jednego z przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="4bf90-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="4bf90-259">Raport z postępów z wywołań metod koncentratora</span><span class="sxs-lookup"><span data-stu-id="4bf90-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="4bf90-260">SignalR 2.1 dodaje obsługę [wzorzec Raportowanie postępu](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) wprowadzone w .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="4bf90-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="4bf90-261">Aby zaimplementować raportowania postępu, zdefiniuj `IProgress<T>` parametr dla Twojego metodę koncentratora klienta mogą uzyskiwać dostęp do:</span><span class="sxs-lookup"><span data-stu-id="4bf90-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="4bf90-262">Podczas pisania — metoda długotrwałych serwera, ważne jest, aby użyć wzorca programowania asynchronicznego, takich jak Async / Await zamiast blokowania wątku koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="4bf90-263">Sposób wywoływania metody z klasy koncentratora klienta</span><span class="sxs-lookup"><span data-stu-id="4bf90-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="4bf90-264">Aby wywołać metody klienta, z serwera, należy użyć `Clients` właściwość w metodę w klasie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="4bf90-265">W poniższym przykładzie pokazano kod serwera, który wywołuje `addNewMessageToPage` na wszystkich połączonych klientów i kod klienta, który definiuje metodę w kliencie języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4bf90-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="4bf90-266">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="4bf90-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="4bf90-267">Wywoływanie metody klienta jest operacją asynchroniczną i zwraca `Task`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-267">Invoking a client method is an asynchronous operation and returns a `Task`.</span></span> <span data-ttu-id="4bf90-268">Użyj `await`:</span><span class="sxs-lookup"><span data-stu-id="4bf90-268">Use `await`:</span></span>

* <span data-ttu-id="4bf90-269">Aby upewnić się, komunikat jest wysyłany bez błędów.</span><span class="sxs-lookup"><span data-stu-id="4bf90-269">To ensure the message is sent without error.</span></span> 
* <span data-ttu-id="4bf90-270">Aby włączyć wychwytywanie i obsługa błędów w bloku try-catch.</span><span class="sxs-lookup"><span data-stu-id="4bf90-270">To enable catching and handling errors in a try-catch block.</span></span>

<span data-ttu-id="4bf90-271">**Klient JavaScript za pomocą wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="4bf90-271">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="4bf90-272">Nie można uzyskać wartość zwracaną z metody klienta; Składnia takich jak `int x = Clients.All.add(1,1)` nie działa.</span><span class="sxs-lookup"><span data-stu-id="4bf90-272">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="4bf90-273">Można określić typy złożone i tablice parametrów.</span><span class="sxs-lookup"><span data-stu-id="4bf90-273">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="4bf90-274">Poniższy przykład przekazuje typu złożonego do klienta w parametrze metody.</span><span class="sxs-lookup"><span data-stu-id="4bf90-274">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="4bf90-275">**Kod serwera, który wywołuje metodę klienta przy użyciu obiektu złożonego**</span><span class="sxs-lookup"><span data-stu-id="4bf90-275">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="4bf90-276">**Kod serwera, który definiuje obiekt złożony**</span><span class="sxs-lookup"><span data-stu-id="4bf90-276">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="4bf90-277">**Klient JavaScript za pomocą wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="4bf90-277">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="4bf90-278">Wybieranie klientów, którzy będą otrzymywać RPC</span><span class="sxs-lookup"><span data-stu-id="4bf90-278">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="4bf90-279">Zwraca właściwości klientów [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) obiektu, który zawiera kilka opcji umożliwiających określenie, którzy klienci otrzymają RPC:</span><span class="sxs-lookup"><span data-stu-id="4bf90-279">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="4bf90-280">Wszyscy połączeni klienci.</span><span class="sxs-lookup"><span data-stu-id="4bf90-280">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="4bf90-281">Tylko klienta wywołującego.</span><span class="sxs-lookup"><span data-stu-id="4bf90-281">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="4bf90-282">Wszyscy klienci oprócz klienta wywołującego.</span><span class="sxs-lookup"><span data-stu-id="4bf90-282">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="4bf90-283">Klientowi szczególne identyfikowane przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="4bf90-283">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="4bf90-284">Ten przykład wywołuje `addContosoChatMessageToPage` na klienta wywołującego i ma ten sam efekt jak użycie `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-284">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="4bf90-285">Wszyscy połączeni klienci oprócz określonych klientów, identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="4bf90-285">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="4bf90-286">Wszyscy połączeni klienci w określonej grupie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-286">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="4bf90-287">Wszyscy połączeni klienci, w określonej grupie oprócz określonych klientów, identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="4bf90-287">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="4bf90-288">Wszyscy połączeni klienci, w określonej grupie oprócz klienta wywołującego.</span><span class="sxs-lookup"><span data-stu-id="4bf90-288">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="4bf90-289">Określony użytkownik identyfikowany przez identyfikator użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4bf90-289">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="4bf90-290">Domyślnie jest to `IPrincipal.Identity.Name`, ale można to zmienić, [rejestrowanie implementację IUserIdProvider z hostem globalnego](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="4bf90-290">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="4bf90-291">Wszystkich klientów i grup na liście identyfikatorów połączeń.</span><span class="sxs-lookup"><span data-stu-id="4bf90-291">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="4bf90-292">Lista grup.</span><span class="sxs-lookup"><span data-stu-id="4bf90-292">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="4bf90-293">Użytkownika według nazwy.</span><span class="sxs-lookup"><span data-stu-id="4bf90-293">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="4bf90-294">Lista nazw użytkowników (zostanie wprowadzony w SignalR 2.1).</span><span class="sxs-lookup"><span data-stu-id="4bf90-294">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="4bf90-295">Nazwy metod weryfikacji kompilacji</span><span class="sxs-lookup"><span data-stu-id="4bf90-295">No compile-time validation for method names</span></span>

<span data-ttu-id="4bf90-296">Nazwa metody, które określisz, jest interpretowany jako obiekt dynamiczny, co oznacza, że brak funkcji IntelliSense i weryfikacji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="4bf90-296">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="4bf90-297">Wyrażenie jest obliczane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="4bf90-297">The expression is evaluated at run time.</span></span> <span data-ttu-id="4bf90-298">Podczas wywołania metody które wykonuje, SignalR wysyła nazwę metody i wartości parametrów do klienta, a jeśli klient ma metodę, który pasuje do nazwy, że metoda jest wywoływana i wartości parametrów są przekazywane do niego.</span><span class="sxs-lookup"><span data-stu-id="4bf90-298">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="4bf90-299">Jeśli odpowiadającej metody znajduje się na komputerze klienckim, zgłaszany jest błąd braku.</span><span class="sxs-lookup"><span data-stu-id="4bf90-299">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="4bf90-300">Aby uzyskać informacje o formacie dane, które SignalR przesyła do klientów w tle po wywołaniu metody klienta, zobacz [wprowadzenie do SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="4bf90-300">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="4bf90-301">Dopasowywanie nazw bez uwzględniania wielkości liter — metoda</span><span class="sxs-lookup"><span data-stu-id="4bf90-301">Case-insensitive method name matching</span></span>

<span data-ttu-id="4bf90-302">Dopasowywaniu nazwy metody nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="4bf90-302">Method name matching is case-insensitive.</span></span> <span data-ttu-id="4bf90-303">Na przykład `Clients.All.addContosoChatMessageToPage` będą wykonywane na serwerze `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, lub `addContosoChatMessageToPage` na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="4bf90-303">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="4bf90-304">Wykonanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="4bf90-304">Asynchronous execution</span></span>

<span data-ttu-id="4bf90-305">Asynchronicznie wykonuje metodę, która wywołania.</span><span class="sxs-lookup"><span data-stu-id="4bf90-305">The method that you call executes asynchronously.</span></span> <span data-ttu-id="4bf90-306">Wszelki kod, który jest dostarczany po wywołania metody, aby klient wykona się natychmiast bez oczekiwania na SignalR zakończyć przesyłania danych do klientów, chyba że określisz, że kolejne wiersze kodu powinna czekać na ukończenie metody.</span><span class="sxs-lookup"><span data-stu-id="4bf90-306">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="4bf90-307">Poniższy przykład kodu pokazuje sposób wykonywania dwóch metod klienta po kolei.</span><span class="sxs-lookup"><span data-stu-id="4bf90-307">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="4bf90-308">**Za pomocą Await (.NET 4.5)**</span><span class="sxs-lookup"><span data-stu-id="4bf90-308">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="4bf90-309">Jeśli używasz `await` poczekać, aż metodę klienta zakończy się przed wykonaniem następnego wiersza kodu, który nie oznacza to, czy klienci będą otrzymywać komunikat faktycznie, przed wykonaniem następnego wiersza kodu.</span><span class="sxs-lookup"><span data-stu-id="4bf90-309">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="4bf90-310">"Wykonanie" wywołania metody klienta oznacza jedynie, że SignalR zrobił wszystko, co jest niezbędne do wysyłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="4bf90-310">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="4bf90-311">Weryfikacji, czy klienci się komunikat, należy mieć program ten mechanizm samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-311">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="4bf90-312">Na przykład można kod `MessageReceived` metody koncentratora, w `addContosoChatMessageToPage` metody na kliencie, można wywołać `MessageReceived` po wykonaniu niezależnie od pracy należy wykonać na kliencie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-312">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="4bf90-313">W `MessageReceived` Centrum można wykonać pracę zależy od klienta faktycznie odbierania i przetwarzania wywołania metody oryginalnym.</span><span class="sxs-lookup"><span data-stu-id="4bf90-313">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="4bf90-314">Jak używać zmiennej ciągu jako nazwy metody</span><span class="sxs-lookup"><span data-stu-id="4bf90-314">How to use a string variable as the method name</span></span>

<span data-ttu-id="4bf90-315">Jeśli chcesz wywołać metodę klienta przy użyciu zmiennej ciągu jako nazwy metody rzutowania `Clients.All` (lub `Clients.Others`, `Clients.Caller`, itp.) do `IClientProxy` , a następnie wywołać [Invoke (methodName, argumenty...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="4bf90-315">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="4bf90-316">Sposób zarządzania członkostwa w grupie z klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="4bf90-316">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="4bf90-317">Grupami w SignalR udostępnia metody emisji komunikatów określony podzbiór połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="4bf90-317">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="4bf90-318">Grupa może zawierać dowolną liczbę klientów, a klient może należeć do dowolnej liczby grup.</span><span class="sxs-lookup"><span data-stu-id="4bf90-318">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="4bf90-319">Aby zarządzanie członkostwem w grupie, należy użyć [Dodaj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) i [Usuń](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metod dostarczonych przez `Groups` właściwość klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-319">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="4bf90-320">W poniższym przykładzie przedstawiono `Groups.Add` i `Groups.Remove` metody używane w metodach koncentratora, które są wywoływane przez kod klienta, a następnie kod klienta JavaScript, która je wywołuje.</span><span class="sxs-lookup"><span data-stu-id="4bf90-320">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="4bf90-321">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="4bf90-321">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="4bf90-322">**Klient JavaScript za pomocą wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="4bf90-322">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="4bf90-323">Nie trzeba jawnie tworzyć grupy.</span><span class="sxs-lookup"><span data-stu-id="4bf90-323">You don't have to explicitly create groups.</span></span> <span data-ttu-id="4bf90-324">Obowiązuje grupy jest automatycznie tworzony po raz pierwszy należy określić jego nazwę w wywołaniu `Groups.Add`, i zostanie usunięta po usunięciu ostatniego połączenia z członkostwa w nim.</span><span class="sxs-lookup"><span data-stu-id="4bf90-324">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="4bf90-325">Nie ma żadnych interfejsów API w celu uzyskania listy członkostwa grupy lub Podaj listę grup.</span><span class="sxs-lookup"><span data-stu-id="4bf90-325">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="4bf90-326">SignalR wysyła wiadomości do klientów i grup na podstawie [model pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), a serwer nie przechowuje listę grup lub członkostwa w grupach.</span><span class="sxs-lookup"><span data-stu-id="4bf90-326">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="4bf90-327">Pozwala to zmaksymalizować skalowalność, ponieważ po każdym dodaniu węzła w farmie sieci web, stan, który przechowuje SignalR są propagowane do nowego węzła.</span><span class="sxs-lookup"><span data-stu-id="4bf90-327">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="4bf90-328">Asynchroniczne wykonywanie metody dodawania i usuwania</span><span class="sxs-lookup"><span data-stu-id="4bf90-328">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="4bf90-329">`Groups.Add` i `Groups.Remove` metody są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-329">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="4bf90-330">Jeśli chcesz dodać do grupy klienta i natychmiast wysłać wiadomość do klienta przy użyciu grupy, należy upewnić się, że `Groups.Add` metoda zakończy się pierwsza.</span><span class="sxs-lookup"><span data-stu-id="4bf90-330">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="4bf90-331">Poniższy przykład kodu pokazuje, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="4bf90-331">The following code example shows how to do that.</span></span>

<span data-ttu-id="4bf90-332">**Dodawanie klienta do grupy i następnie komunikatów klienta**</span><span class="sxs-lookup"><span data-stu-id="4bf90-332">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="4bf90-333">Trwałość członkostwa grupy</span><span class="sxs-lookup"><span data-stu-id="4bf90-333">Group membership persistence</span></span>

<span data-ttu-id="4bf90-334">SignalR śledzi połączenia, nie dla użytkowników, więc jeśli użytkownik ma znajdować się w tej samej grupie za każdym razem, gdy użytkownik nawiązuje połączenie, musisz wywołać `Groups.Add` za każdym razem, gdy użytkownik nawiązuje nowe połączenie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-334">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="4bf90-335">Po do tymczasowej utraty łączności czasami biblioteki SignalR można przywrócić połączenia automatycznie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-335">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="4bf90-336">W takim przypadku SignalR jest przywrócenie tego samego połączenia, nie ustanawiania nowego połączenia, a więc członkostwa w grupie klienta zostanie automatycznie przywrócony.</span><span class="sxs-lookup"><span data-stu-id="4bf90-336">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="4bf90-337">Jest to możliwe nawet, jeśli tymczasowe przerwanie jest wynikiem ponownego rozruchu serwera lub awarii, ponieważ stan połączenia dla każdego klienta, w tym członkostwa w grupach jest zwrotnego do klienta.</span><span class="sxs-lookup"><span data-stu-id="4bf90-337">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="4bf90-338">Jeśli serwer ulegnie awarii, został zastąpiony nowym serwerze, zanim upłynie limit czasu połączenia klienta można automatycznie ponownie połączenie z nowym serwerem i ponownego rejestrowania się w grupach, które jest członkiem.</span><span class="sxs-lookup"><span data-stu-id="4bf90-338">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="4bf90-339">Jeśli połączenie nie będzie można przywrócić automatycznie po utracie łączności, lub gdy upłynie limit czasu połączenia lub gdy klient odłączy się (na przykład, gdy przeglądarka wraca do nowej strony), członkostwa w grupie zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="4bf90-339">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="4bf90-340">Następnym razem, gdy użytkownik nawiąże połączenie będzie nowe połączenie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-340">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="4bf90-341">Aby zachować członkostwa w grupach, gdy ten sam użytkownik nawiązuje nowe połączenie, aplikacja musi śledzić skojarzenia między użytkownikami i grupami w celu przywrócenia członkostwa w grupach za każdym razem użytkownik ustanawia nowego połączenia.</span><span class="sxs-lookup"><span data-stu-id="4bf90-341">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="4bf90-342">Aby uzyskać więcej informacji na temat połączeń i ponowne łączenie, zobacz [sposób obsługi zdarzenia okresu istnienia połączenia w klasie Centrum](#connectionlifetime) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="4bf90-342">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="4bf90-343">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="4bf90-343">Single-user groups</span></span>

<span data-ttu-id="4bf90-344">Aplikacje, które zwykle używanie biblioteki SignalR mieć do śledzenia skojarzenia między użytkownikami i połączeń, aby dowiedzieć się, użytkownika, który wysłał wiadomość, a które użytkownicy powinni otrzymywać wiadomości.</span><span class="sxs-lookup"><span data-stu-id="4bf90-344">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="4bf90-345">Grupy są używane w jednym z dwóch często używane wzorce dla tych czynności.</span><span class="sxs-lookup"><span data-stu-id="4bf90-345">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="4bf90-346">Grupy pojedynczego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4bf90-346">Single-user groups.</span></span>

    <span data-ttu-id="4bf90-347">Określ nazwę użytkownika jako nazwę grupy i dodać do grupy bieżący identyfikator połączenia, za każdym razem, gdy użytkownik nawiązuje połączenie, lub ponownie nawiąże połączenie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-347">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="4bf90-348">Aby wysyłać komunikaty do użytkownika Wyślij do grupy.</span><span class="sxs-lookup"><span data-stu-id="4bf90-348">To send messages to the user you send to the group.</span></span> <span data-ttu-id="4bf90-349">Wadą tej metody jest to, że grupa nie zapewniają sposób, aby sprawdzić, czy użytkownik jest w trybie online lub offline.</span><span class="sxs-lookup"><span data-stu-id="4bf90-349">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="4bf90-350">Śledź skojarzenia między nazwami użytkowników i identyfikatorów połączeń.</span><span class="sxs-lookup"><span data-stu-id="4bf90-350">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="4bf90-351">Przechowywanie skojarzenie między każdej nazwy użytkownika i jeden lub więcej identyfikatorów połączeń słownika lub bazy danych i zaktualizować przechowywanych danych za każdym razem, użytkownik połączeniu lub rozłączeniu.</span><span class="sxs-lookup"><span data-stu-id="4bf90-351">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="4bf90-352">Aby wysyłać komunikaty do użytkownika, należy określić identyfikatorów połączeń.</span><span class="sxs-lookup"><span data-stu-id="4bf90-352">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="4bf90-353">Wadą tej metody jest to, że zajmuje więcej pamięci.</span><span class="sxs-lookup"><span data-stu-id="4bf90-353">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="4bf90-354">Jak obsługiwać zdarzenia okresu istnienia połączenia w klasie Centrum</span><span class="sxs-lookup"><span data-stu-id="4bf90-354">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="4bf90-355">Typowe przyczyny Obsługa zdarzeń okresu istnienia połączenia są do śledzenia, czy użytkownik jest połączony, czy nie, a także do śledzenia skojarzenie między nazwy użytkowników i identyfikatorów połączeń.</span><span class="sxs-lookup"><span data-stu-id="4bf90-355">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="4bf90-356">Aby uruchomić własny kod, gdy klienci łączyć i rozłączać, Zastąp `OnConnected`, `OnDisconnected`, i `OnReconnected` metod wirtualnych Centrum klasy, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-356">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="4bf90-357">Kiedy są wywoływane onconnected elementu ondisconnected elementu i onreconnected elementu</span><span class="sxs-lookup"><span data-stu-id="4bf90-357">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="4bf90-358">Każdorazowo, przeglądarka wraca do nowej strony, nowe połączenie ma zostać nawiązane, co oznacza, że zostanie wykonana SignalR `OnDisconnected` metody następuje `OnConnected` metody.</span><span class="sxs-lookup"><span data-stu-id="4bf90-358">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="4bf90-359">Po nawiązaniu nowego połączenia SignalR zawsze tworzy nowy identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="4bf90-359">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="4bf90-360">`OnReconnected` Metoda jest wywoływana, gdy nastąpiła tymczasowe przerwanie w łączności, SignalR automatyczne odzyskiwanie, takie jak kiedy kabla jest tymczasowo odłączony i ponowne łączenie zakończone przed upływem limitu czasu połączenia. `OnDisconnected` Metoda jest wywoływana, gdy klient został odłączony SignalR nie automatycznie ponownie połączyć, np. gdy przeglądarka wraca do nowej strony.</span><span class="sxs-lookup"><span data-stu-id="4bf90-360">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="4bf90-361">Dlatego jest możliwe sekwencji zdarzeń dla danego klienta `OnConnected`, `OnReconnected`, `OnDisconnected`; lub `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-361">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="4bf90-362">Nie będziesz widzieć sekwencji `OnConnected`, `OnDisconnected`, `OnReconnected` dla danego połączenia.</span><span class="sxs-lookup"><span data-stu-id="4bf90-362">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="4bf90-363">`OnDisconnected` Metoda nie jest wywoływana w niektórych scenariuszach, na przykład jeśli serwer ulegnie awarii lub domena aplikacji zostanie odtworzona.</span><span class="sxs-lookup"><span data-stu-id="4bf90-363">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="4bf90-364">Gdy inny serwer, który jest dostarczany w wierszu lub domena aplikacji kończy jego odtwarzanie, niektórzy klienci można ponownie połączyć i szybko `OnReconnected` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="4bf90-364">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="4bf90-365">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia w SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="4bf90-365">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="4bf90-366">Stan elementu wywołującego, pusta</span><span class="sxs-lookup"><span data-stu-id="4bf90-366">Caller state not populated</span></span>

<span data-ttu-id="4bf90-367">Metody obsługi zdarzeń okresu istnienia połączenia są nazywane z serwera, co oznacza, że stan, aby umieścić w `state` obiektu na kliencie nie zostanie wypełniony w `Caller` właściwości na serwerze.</span><span class="sxs-lookup"><span data-stu-id="4bf90-367">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="4bf90-368">Aby uzyskać informacje o `state` obiektu i `Caller` właściwości, zobacz [sposób przekazywania stanu między klientami a klasy koncentratora](#passstate) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="4bf90-368">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="4bf90-369">Jak uzyskać informacji o kliencie z właściwości kontekstu</span><span class="sxs-lookup"><span data-stu-id="4bf90-369">How to get information about the client from the Context property</span></span>

<span data-ttu-id="4bf90-370">Aby uzyskać informacje dotyczące klienta, użyj `Context` właściwość klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-370">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="4bf90-371">`Context` Właściwość zwraca [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) obiektu, który zapewnia dostęp do następujących informacji:</span><span class="sxs-lookup"><span data-stu-id="4bf90-371">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="4bf90-372">Identyfikator połączenia klienta wywołującego.</span><span class="sxs-lookup"><span data-stu-id="4bf90-372">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="4bf90-373">Identyfikator połączenia, który jest identyfikatorem GUID, który jest przypisywany przez SignalR (we własnym kodzie nie można określić wartości).</span><span class="sxs-lookup"><span data-stu-id="4bf90-373">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="4bf90-374">Istnieje jeden identyfikator połączenia dla poszczególnych połączeń i tego samego połączenia, którego identyfikator jest używany przez wszystkie centra, jeśli masz wiele centrów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4bf90-374">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="4bf90-375">Dane nagłówka HTTP.</span><span class="sxs-lookup"><span data-stu-id="4bf90-375">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="4bf90-376">Możesz też pobrać nagłówki HTTP od `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-376">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="4bf90-377">Przyczyna wiele odwołań do tak samo jest fakt, że `Context.Headers` utworzono najpierw `Context.Request` właściwość została dodana później i `Context.Headers` został zachowany na potrzeby zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="4bf90-377">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="4bf90-378">Wykonywanie zapytań dotyczących danych ciągu.</span><span class="sxs-lookup"><span data-stu-id="4bf90-378">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="4bf90-379">Możesz też pobrać dane ciągu zapytania z `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-379">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="4bf90-380">Ciąg zapytania, które otrzymujesz w tej właściwości jest tą, która została użyta z żądania HTTP, które nawiązaniu połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="4bf90-380">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="4bf90-381">Aby dodać parametry ciągu zapytania w kliencie, konfigurowanie połączenia z to wygodny sposób przekazywania danych dotyczących klienta z klienta do serwera.</span><span class="sxs-lookup"><span data-stu-id="4bf90-381">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="4bf90-382">Poniższy przykład przedstawia sposób dodać ciąg zapytania w kliencie JavaScript, korzystając z wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="4bf90-382">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="4bf90-383">Aby uzyskać więcej informacji na temat ustawiania parametrów ciągu zapytania, zapoznaj się z przewodnikami interfejsu API, aby uzyskać [JavaScript](hubs-api-guide-javascript-client.md) i [.NET](hubs-api-guide-net-client.md) klientów.</span><span class="sxs-lookup"><span data-stu-id="4bf90-383">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="4bf90-384">Można znaleźć metody transportu używane dla połączeń w dane ciągu zapytania wraz z innymi wartościami, używane wewnętrznie przez element SignalR:</span><span class="sxs-lookup"><span data-stu-id="4bf90-384">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="4bf90-385">Wartość `transportMethod` będzie "webSockets", "serverSentEvents", "foreverFrame" lub "longPolling".</span><span class="sxs-lookup"><span data-stu-id="4bf90-385">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="4bf90-386">Należy pamiętać, że jeśli zaznaczysz tę wartość `OnConnected` metody obsługi zdarzeń, w niektórych scenariuszach może być początkowo uzyskać wartość transportu, która nie jest to metoda końcowego wynegocjowanym transport dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="4bf90-386">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="4bf90-387">W tym przypadku metoda spowoduje zgłoszenie wyjątku i zostanie wywołana ponownie później po nawiązaniu metoda końcowego transportu.</span><span class="sxs-lookup"><span data-stu-id="4bf90-387">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="4bf90-388">Pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-388">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="4bf90-389">Możesz też pobrać pliki cookie z `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-389">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="4bf90-390">Informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="4bf90-390">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="4bf90-391">Obiekt kontekstu HTTP dla żądania:</span><span class="sxs-lookup"><span data-stu-id="4bf90-391">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="4bf90-392">Użyj tej metody, zamiast `HttpContext.Current` można pobrać `HttpContext` obiektu dla połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="4bf90-392">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="4bf90-393">Jak przekazać stanu między klientami a klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="4bf90-393">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="4bf90-394">Udostępnia serwer proxy klienta `state` obiekt, w którym można przechowywać dane, które mają być przekazywane do serwera z każdego wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="4bf90-394">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="4bf90-395">Na serwerze mają dostęp do danych, to w `Clients.Caller` właściwości w metodach koncentratora, które są wywoływane przez klientów.</span><span class="sxs-lookup"><span data-stu-id="4bf90-395">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="4bf90-396">`Clients.Caller` Właściwość jest pusta dla metody obsługi zdarzeń okresu istnienia połączenia `OnConnected`, `OnDisconnected`, i `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-396">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="4bf90-397">Tworzenie lub aktualizowanie danych w `state` obiektu i `Clients.Caller` właściwości działa w obu kierunkach.</span><span class="sxs-lookup"><span data-stu-id="4bf90-397">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="4bf90-398">Można zaktualizować wartości na serwerze i są one przekazywane do klienta.</span><span class="sxs-lookup"><span data-stu-id="4bf90-398">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="4bf90-399">Poniższy przykład pokazuje kod klienta JavaScript, który zapisuje stan w celu przesłania go do serwera z każdego wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="4bf90-399">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="4bf90-400">Poniższy przykład przedstawia równoważny kod w klienta platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="4bf90-400">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="4bf90-401">W klasie Centrum uzyskujesz dostęp do tych danych w `Clients.Caller` właściwości.</span><span class="sxs-lookup"><span data-stu-id="4bf90-401">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="4bf90-402">Poniższy przykład pokazuje kod, który pobiera stan określonego w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-402">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="4bf90-403">Ten mechanizm utrwalanie stanu nie jest przeznaczony dla dużych ilości danych, ponieważ wszystko, co umieścić w `state` lub `Clients.Caller` właściwość jest zwrotnego z każdego wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="4bf90-403">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="4bf90-404">Jest to przydatne dla mniejszych elementów, takich jak nazwy użytkowników lub liczniki.</span><span class="sxs-lookup"><span data-stu-id="4bf90-404">It's useful for smaller items such as user names or counters.</span></span>


<span data-ttu-id="4bf90-405">W VB.NET lub koncentrator silnie typizowane, obiekt wywołujący stanu nie są dostępne za pośrednictwem `Clients.Caller`; zamiast tego użyj `Clients.CallerState` (zostanie wprowadzony w SignalR 2.1):</span><span class="sxs-lookup"><span data-stu-id="4bf90-405">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="4bf90-406">**Za pomocą CallerState w języku C#**</span><span class="sxs-lookup"><span data-stu-id="4bf90-406">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="4bf90-407">**Za pomocą CallerState w języku Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="4bf90-407">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="4bf90-408">Sposób obsługi błędów w klasie Centrum</span><span class="sxs-lookup"><span data-stu-id="4bf90-408">How to handle errors in the Hub class</span></span>

<span data-ttu-id="4bf90-409">Do obsługi błędów występujących w Twoich metodach klasy koncentratora, najpierw upewnij się, "zauważysz" wszystkie wyjątki od operacji asynchronicznej (takich jak wywoływanie metod klienta) przy użyciu `await`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-409">To handle errors that occur in your Hub class methods, first ensure you "observe" any exceptions from async operations (such as invoking client methods) by using `await`.</span></span> <span data-ttu-id="4bf90-410">Następnie należy użyć co najmniej jeden z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="4bf90-410">Then use one or more of the following methods:</span></span>

- <span data-ttu-id="4bf90-411">Zabalit kodzie metoda bloków try-catch, a następnie zaloguj się obiekt wyjątku.</span><span class="sxs-lookup"><span data-stu-id="4bf90-411">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="4bf90-412">Dla celów debugowania wyjątku można wysłać do klienta, ale zabezpieczeń przyczyny wysyłanie szczegółowych informacji do klientów w środowisku produkcyjnym nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="4bf90-412">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="4bf90-413">Utwórz moduł potoku koncentratorów, który obsługuje [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) metody.</span><span class="sxs-lookup"><span data-stu-id="4bf90-413">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="4bf90-414">Poniższy przykład pokazuje moduł potoku, który rejestruje błędy, kod w pliku Startup.cs, która wprowadza moduł potokiem koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="4bf90-414">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="4bf90-415">Użyj `HubException` klasy (zostanie wprowadzony w SignalR 2).</span><span class="sxs-lookup"><span data-stu-id="4bf90-415">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="4bf90-416">Ten błąd może być wyrzucanych z jakiegokolwiek wywołania koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-416">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="4bf90-417">`HubError` Konstruktor przyjmuje komunikat w formacie ciągu i obiekt do przechowywania dodatkowe dane dotyczące błędu.</span><span class="sxs-lookup"><span data-stu-id="4bf90-417">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="4bf90-418">SignalR spowoduje automatyczne serializować wyjątku i wysyłać je do klienta, w którym będzie służyć do odrzucania lub Niepowodzenie wywołania metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-418">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="4bf90-419">Poniższe przykłady kodu przedstawiają sposób throw `HubException` podczas wywołania koncentratora i sposób obsługi wyjątków na komputerach klienckich JavaScript i .NET.</span><span class="sxs-lookup"><span data-stu-id="4bf90-419">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="4bf90-420">**Kod serwera ukazujące klasy HubException**</span><span class="sxs-lookup"><span data-stu-id="4bf90-420">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="4bf90-421">**Kod klienta JavaScript ukazujące odpowiedź do zgłaszania HubException w z koncentratorem**</span><span class="sxs-lookup"><span data-stu-id="4bf90-421">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="4bf90-422">**Prezentacja odpowiedź do zgłaszania HubException w z koncentratorem kod klienta .NET**</span><span class="sxs-lookup"><span data-stu-id="4bf90-422">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="4bf90-423">Aby uzyskać więcej informacji na temat modułów potoku koncentratora, zobacz [sposobu dostosowywania potoku koncentratory](#hubpipeline) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="4bf90-423">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="4bf90-424">Jak włączyć śledzenie</span><span class="sxs-lookup"><span data-stu-id="4bf90-424">How to enable tracing</span></span>

<span data-ttu-id="4bf90-425">Aby włączyć śledzenie po stronie serwera, należy dodać system.diagnostics element do pliku Web.config, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="4bf90-425">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="4bf90-426">Po uruchomieniu aplikacji w programie Visual Studio możesz wyświetlić dzienniki w **dane wyjściowe** okna.</span><span class="sxs-lookup"><span data-stu-id="4bf90-426">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="4bf90-427">Jak wywołać metody klienta grup i zarządzanie nimi z poza klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="4bf90-427">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="4bf90-428">Do wywołania metody klienta, z innej klasy niż klasa koncentratora, Pobierz odwołanie do obiektu context SignalR dla koncentratora i używać go wywoływać metody na kliencie lub zarządzać grupami.</span><span class="sxs-lookup"><span data-stu-id="4bf90-428">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="4bf90-429">Poniższy przykład `StockTicker` klasy pobiera obiekt kontekstu, zapisuje go w wystąpieniu klasy, przechowuje wystąpienia klasy statycznej właściwości i używa kontekstu z pojedyncze wystąpienie klasy w celu wywołania `updateStockPrice` metody na komputerach klienckich, które są podłączone do koncentratora o nazwie `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-429">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="4bf90-430">Jeśli musisz używać kontekstu wielu i godziny w obiekcie długotrwałe raz Uzyskaj odwołanie i zapisz go, a nie wprowadzenie go ponownie za każdym razem.</span><span class="sxs-lookup"><span data-stu-id="4bf90-430">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="4bf90-431">Pobieranie kontekstu po zapewnia SignalR i wysyła wiadomości do klientów w tej samej kolejności, w której metody koncentratora wprowadzić klienta wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="4bf90-431">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="4bf90-432">Aby uzyskać samouczek, który pokazuje, jak używać kontekstu SignalR dla koncentratora, zobacz [emisje serwera z użyciem ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="4bf90-432">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="4bf90-433">Wywoływanie metody klienta</span><span class="sxs-lookup"><span data-stu-id="4bf90-433">Calling client methods</span></span>

<span data-ttu-id="4bf90-434">Można określić, którzy klienci otrzymają RPC, ale ma mniej opcji niż pod numerem klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-434">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="4bf90-435">Przyczyną jest to, że kontekst nie jest skojarzony z określonym wywołania klienta, więc żadnych metod wymagających znajomości bieżący identyfikator połączenia, takie jak `Clients.Others`, lub `Clients.Caller`, lub `Clients.OthersInGroup`, nie są dostępne.</span><span class="sxs-lookup"><span data-stu-id="4bf90-435">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="4bf90-436">Dostępne są następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="4bf90-436">The following options are available:</span></span>

- <span data-ttu-id="4bf90-437">Wszyscy połączeni klienci.</span><span class="sxs-lookup"><span data-stu-id="4bf90-437">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="4bf90-438">Klientowi szczególne identyfikowane przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="4bf90-438">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="4bf90-439">Wszyscy połączeni klienci oprócz określonych klientów, identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="4bf90-439">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="4bf90-440">Wszyscy połączeni klienci w określonej grupie.</span><span class="sxs-lookup"><span data-stu-id="4bf90-440">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="4bf90-441">Wszystkich połączonych klientów w określonej grupie oprócz określonych klientów, identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="4bf90-441">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="4bf90-442">W przypadku wywołania do klasy-Hub metody w klasie koncentratora, możesz przekazać identyfikator bieżącego połączenia i używać go z `Clients.Client`, `Clients.AllExcept`, lub `Clients.Group` do symulacji `Clients.Caller`, `Clients.Others`, lub `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-442">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="4bf90-443">W poniższym przykładzie `MoveShapeHub` klasy przekazuje identyfikator połączenia, aby `Broadcaster` klasy tak, aby `Broadcaster` klasy można symulować `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="4bf90-443">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="4bf90-444">Członkostwo w grupie zarządzania</span><span class="sxs-lookup"><span data-stu-id="4bf90-444">Managing group membership</span></span>

<span data-ttu-id="4bf90-445">Do zarządzania grupami mają te same opcje tak jak w klasie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-445">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="4bf90-446">Dodawanie klienta do grupy</span><span class="sxs-lookup"><span data-stu-id="4bf90-446">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="4bf90-447">Usunięcie klienta z grupy</span><span class="sxs-lookup"><span data-stu-id="4bf90-447">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="4bf90-448">Jak dostosować potoku koncentratory</span><span class="sxs-lookup"><span data-stu-id="4bf90-448">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="4bf90-449">Biblioteki SignalR można wstawić własny kod do potoku koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4bf90-449">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="4bf90-450">Poniższy przykład przedstawia niestandardowego modułu potoku koncentratora, który rejestruje każdego przychodzące wywołania metody, odebrane od klienta i wychodzące wywołanie metody wywoływane na komputerze klienckim:</span><span class="sxs-lookup"><span data-stu-id="4bf90-450">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="4bf90-451">Poniższy kod w *Startup.cs* pliku rejestruje moduł do działania w potoku koncentratora:</span><span class="sxs-lookup"><span data-stu-id="4bf90-451">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="4bf90-452">Istnieje wiele różnych metod, które można przesłonić.</span><span class="sxs-lookup"><span data-stu-id="4bf90-452">There are many different methods that you can override.</span></span> <span data-ttu-id="4bf90-453">Aby uzyskać pełną listę, zobacz [metody HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="4bf90-453">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
