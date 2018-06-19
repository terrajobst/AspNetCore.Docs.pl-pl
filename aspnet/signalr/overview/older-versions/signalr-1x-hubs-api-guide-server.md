---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: Podręcznik interfejsu API koncentratorów SignalR platformy ASP.NET — serwera (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API koncentratorów SignalR platformy ASP.NET SignalR w wersji 1.1, z demonstratin przykłady kodu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 96155b1c648e5f6092b3ba67a560197f86a593b9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044185"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="9b579-103">Podręcznik interfejsu API koncentratorów SignalR platformy ASP.NET — serwera (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="9b579-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="9b579-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9b579-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="9b579-105">Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API koncentratorów SignalR platformy ASP.NET dla biblioteki SignalR w wersji 1.1, z przykładów kodu prezentacja typowe opcje.</span><span class="sxs-lookup"><span data-stu-id="9b579-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="9b579-106">Interfejs API koncentratorów SignalR umożliwia upewnij zdalnych wywołań procedur (RPC) z serwera do połączonych klientów i klientów na serwerze.</span><span class="sxs-lookup"><span data-stu-id="9b579-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="9b579-107">W kodzie serwera zdefiniuj metody, które mogą być wywoływane przez klientów i wywołania metod, które są uruchamiane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="9b579-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="9b579-108">W kodu klienta należy zdefiniować metody, które mogą być wywoływane z serwera i wywołania metody, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="9b579-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="9b579-109">SignalR odpowiada on za wszystkie żmudne procesy klient serwer dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="9b579-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="9b579-110">SignalR udostępnia również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="9b579-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="9b579-111">Wprowadzenie do SignalR, koncentratorach i połączeń trwałych lub Samouczek przedstawiający sposób tworzenia pełnej aplikacji SignalR, patrz [SignalR — wprowadzenie](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b579-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="9b579-112">Omówienie</span><span class="sxs-lookup"><span data-stu-id="9b579-112">Overview</span></span>

<span data-ttu-id="9b579-113">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="9b579-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="9b579-114">Jak zarejestrować trasy SignalR i skonfigurować opcje SignalR</span><span class="sxs-lookup"><span data-stu-id="9b579-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="9b579-115">Adres URL /signalr</span><span class="sxs-lookup"><span data-stu-id="9b579-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="9b579-116">Konfigurowanie opcji SignalR</span><span class="sxs-lookup"><span data-stu-id="9b579-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="9b579-117">Jak utworzyć i używać klas elementu Hub</span><span class="sxs-lookup"><span data-stu-id="9b579-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="9b579-118">Okres istnienia obiektu Centrum</span><span class="sxs-lookup"><span data-stu-id="9b579-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="9b579-119">Wielkość liter formatu Centrum nazw klientów języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="9b579-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="9b579-120">Wiele centrów</span><span class="sxs-lookup"><span data-stu-id="9b579-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="9b579-121">Sposób definiowania metod w klasie koncentratora, której klienci mogą wywoływać</span><span class="sxs-lookup"><span data-stu-id="9b579-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="9b579-122">Wielkość liter formatu nazw klientów języka JavaScript — metoda</span><span class="sxs-lookup"><span data-stu-id="9b579-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="9b579-123">Kiedy asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="9b579-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="9b579-124">Definiowanie przeciążenia</span><span class="sxs-lookup"><span data-stu-id="9b579-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="9b579-125">Wywoływanie metody z klasy koncentratora klienta</span><span class="sxs-lookup"><span data-stu-id="9b579-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="9b579-126">Wybieranie których klienci otrzymają RPC</span><span class="sxs-lookup"><span data-stu-id="9b579-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="9b579-127">Weryfikacja nie kompilacji dla nazw — metoda</span><span class="sxs-lookup"><span data-stu-id="9b579-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="9b579-128">Dopasowywanie nazw bez uwzględniania wielkości liter — metoda</span><span class="sxs-lookup"><span data-stu-id="9b579-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="9b579-129">Wykonanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="9b579-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="9b579-130">Jak zarządzać członkostwa w grupie z klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="9b579-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="9b579-131">Asynchroniczne wykonywanie metod Add i Remove</span><span class="sxs-lookup"><span data-stu-id="9b579-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="9b579-132">Trwałość członkostwa grupy</span><span class="sxs-lookup"><span data-stu-id="9b579-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="9b579-133">Grupy jednego użytkownika</span><span class="sxs-lookup"><span data-stu-id="9b579-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="9b579-134">Sposób obsługi zdarzeń okres istnienia połączenia w klasie Centrum</span><span class="sxs-lookup"><span data-stu-id="9b579-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="9b579-135">Kiedy są wywoływane OnConnected ondisconnected elementu i onreconnected elementu</span><span class="sxs-lookup"><span data-stu-id="9b579-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="9b579-136">Stan elementu wywołującego pusta</span><span class="sxs-lookup"><span data-stu-id="9b579-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="9b579-137">Jak uzyskać informacji o kliencie z właściwości kontekstu</span><span class="sxs-lookup"><span data-stu-id="9b579-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="9b579-138">Sposób przekazywania stan między klientami a klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="9b579-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="9b579-139">Sposób obsługi błędów w klasie Centrum</span><span class="sxs-lookup"><span data-stu-id="9b579-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="9b579-140">Jak wywoływać metod klienta i zarządzanie grupami z poza klasą Centrum</span><span class="sxs-lookup"><span data-stu-id="9b579-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="9b579-141">Wywoływanie metody klienta</span><span class="sxs-lookup"><span data-stu-id="9b579-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="9b579-142">Członkostwo w grupie zarządzania</span><span class="sxs-lookup"><span data-stu-id="9b579-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="9b579-143">Jak włączyć śledzenie</span><span class="sxs-lookup"><span data-stu-id="9b579-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="9b579-144">Dostosowywanie potoku koncentratory</span><span class="sxs-lookup"><span data-stu-id="9b579-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="9b579-145">Dokumentacja na temat programu klientów zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="9b579-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="9b579-146">Podręcznik interfejsu API koncentratorów SignalR — JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="9b579-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="9b579-147">Podręcznik interfejsu API koncentratorów SignalR - klienta .NET</span><span class="sxs-lookup"><span data-stu-id="9b579-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="9b579-148">Linki do tematów dokumentacji interfejsu API są wersja platformy .NET 4.5 interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="9b579-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="9b579-149">Jeśli używasz programu .NET 4, zobacz [wersji .NET 4 tematy interfejsu API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="9b579-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="9b579-150">Jak zarejestrować trasy SignalR i skonfigurować opcje SignalR</span><span class="sxs-lookup"><span data-stu-id="9b579-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="9b579-151">Aby zdefiniować trasy, którego klienci będą używać do nawiązania połączenia z koncentratorem, należy wywołać [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) metody podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9b579-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="9b579-152">`MapHubs`jest [— metoda rozszerzenia](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) dla `System.Web.Routing.RouteCollection` klasy.</span><span class="sxs-lookup"><span data-stu-id="9b579-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="9b579-153">Poniższy przykład przedstawia sposób definiowania trasy koncentratory SignalR w *Global.asax* pliku.</span><span class="sxs-lookup"><span data-stu-id="9b579-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="9b579-154">Jeśli dodajesz SignalR funkcje do aplikacji platformy ASP.NET MVC, upewnij się, że trasy SignalR został dodany przed innych tras.</span><span class="sxs-lookup"><span data-stu-id="9b579-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="9b579-155">Aby uzyskać więcej informacji, zobacz [samouczek: wprowadzenie do korzystania z SignalR i MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b579-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="9b579-156">Adres URL /signalr</span><span class="sxs-lookup"><span data-stu-id="9b579-156">The /signalr URL</span></span>

<span data-ttu-id="9b579-157">Domyślnie jest adres URL trasy, którego klienci będą używać do nawiązania połączenia z koncentratorem "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="9b579-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="9b579-158">(Nie należy mylić ten adres URL z adresu URL "/ signalr/hubs", który jest automatycznie wygenerowanego pliku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b579-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="9b579-159">Aby uzyskać więcej informacji na temat wygenerowany serwer proxy, zobacz [Podręcznik interfejsu API koncentratorów SignalR - JavaScript Client - wygenerowany serwer proxy i jakie operacje dla Ciebie](index.md).)</span><span class="sxs-lookup"><span data-stu-id="9b579-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="9b579-160">Może to być nadzwyczajne okoliczności powodujących, że ten podstawowy adres URL nie można używać dla biblioteki SignalR; na przykład istnieje folder projektu o nazwie *signalr* i nie chcesz zmienić nazwę.</span><span class="sxs-lookup"><span data-stu-id="9b579-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="9b579-161">W takim przypadku można zmienić podstawowy adres URL, jak pokazano w poniższych przykładach (Zastąp "/ signalr" w przykładowym kodzie z żądany adres URL).</span><span class="sxs-lookup"><span data-stu-id="9b579-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="9b579-162">**Kod serwera, który określa adres URL**</span><span class="sxs-lookup"><span data-stu-id="9b579-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="9b579-163">**Kod JavaScript klienta, który określa adres URL (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="9b579-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="9b579-164">**Kod JavaScript klienta, który określa adres URL (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="9b579-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="9b579-165">**Kod klienta .NET, który określa adres URL**</span><span class="sxs-lookup"><span data-stu-id="9b579-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="9b579-166">Konfigurowanie opcji SignalR</span><span class="sxs-lookup"><span data-stu-id="9b579-166">Configuring SignalR Options</span></span>

<span data-ttu-id="9b579-167">Overloads z `MapHubs` metody pozwalają określić niestandardowy adres URL, moduł rozpoznawania zależności niestandardowych i następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="9b579-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="9b579-168">Włącz połączenia między domenami z klientów przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="9b579-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="9b579-169">Zwykle Jeśli przeglądarka ładuje strony z `http://contoso.com`, połączenia SignalR jest w tej samej domenie, w `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="9b579-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="9b579-170">Jeśli strona z `http://contoso.com` nawiązuje połączenie `http://fabrikam.com/signalr`, czyli połączenia między domenami.</span><span class="sxs-lookup"><span data-stu-id="9b579-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="9b579-171">Ze względów bezpieczeństwa połączeń między domenami są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="9b579-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="9b579-172">Aby uzyskać więcej informacji, zobacz [ASP.NET SignalR koncentratory interfejsu API przewodnik - JavaScript Client - jak nawiązać połączenie między domenami](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b579-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="9b579-173">Włącz szczegółowe komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="9b579-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="9b579-174">Jeśli wystąpią błędy, domyślne zachowanie SignalR ma wysłać do klientów komunikatu powiadomienia bez szczegółowe informacje o co się stało.</span><span class="sxs-lookup"><span data-stu-id="9b579-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="9b579-175">Wysyłanie szczegółowe informacje o błędzie do klientów nie jest zalecane w środowisku produkcyjnym, ponieważ złośliwi użytkownicy mogą mieć możliwość skorzystaj z informacji w ataków na aplikację.</span><span class="sxs-lookup"><span data-stu-id="9b579-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="9b579-176">Do rozwiązywania problemów, służy tę opcję, aby włączyć raportowanie błędów więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="9b579-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="9b579-177">Wyłączenie automatycznie generowanych plików proxy JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b579-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="9b579-178">Domyślnie plik JavaScript z serwerów proxy dla Twojej klas elementu Hub jest generowany w odpowiedzi na adres URL "/ signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="9b579-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="9b579-179">Jeśli nie chcesz używać serwery proxy JavaScript, lub jeśli chcesz wygenerować ten plik ręcznie i odwoływać się do fizycznego pliku w klientach, służy tę opcję, aby wyłączyć generowanie serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="9b579-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="9b579-180">Aby uzyskać więcej informacji, zobacz [Podręcznik interfejsu API koncentratorów SignalR - JavaScript klient - serwer proxy generowany sposobu tworzenia pliku fizycznego dla elementu SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b579-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="9b579-181">Poniższy przykład przedstawia sposób określić te opcje i adres URL połączenia SignalR w wywołaniu `MapHubs` metody.</span><span class="sxs-lookup"><span data-stu-id="9b579-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="9b579-182">Aby określić niestandardowy adres URL, zastąp "/ signalr" w przykładzie z adresem URL, którego chcesz używać.</span><span class="sxs-lookup"><span data-stu-id="9b579-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="9b579-183">Jak utworzyć i używać klas elementu Hub</span><span class="sxs-lookup"><span data-stu-id="9b579-183">How to create and use Hub classes</span></span>

<span data-ttu-id="9b579-184">Koncentrator, utworzyć klasę, która jest pochodną [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="9b579-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="9b579-185">W poniższym przykładzie przedstawiono prostą klasę Centrum dla aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="9b579-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="9b579-186">W tym przykładzie można wywołać połączony klient `NewContosoChatMessage` metody, i tak, aby wszyscy połączeni klienci emitowania odebrane dane.</span><span class="sxs-lookup"><span data-stu-id="9b579-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="9b579-187">Okres istnienia obiektu Centrum</span><span class="sxs-lookup"><span data-stu-id="9b579-187">Hub object lifetime</span></span>

<span data-ttu-id="9b579-188">Nie utworzyć wystąpienia klasy koncentratora lub wywołać metod w kodzie na serwerze; wszystko jest realizowane automatycznie przez potoku koncentratory SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b579-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="9b579-189">SignalR tworzy nowe wystąpienie klasy koncentratora każdej musi do obsługi operacji koncentratora, takich jak gdy klient nawiąże połączenie, rozłącza lub sprawia, że wywołanie metody do serwera.</span><span class="sxs-lookup"><span data-stu-id="9b579-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="9b579-190">Ponieważ wystąpień klasy koncentratora są przejściowe, nie możesz użyć ich do zarządzania stanem z jedną metodę wywołania do następnego.</span><span class="sxs-lookup"><span data-stu-id="9b579-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="9b579-191">Zawsze serwer odbiera wywołanie metody od klienta, nowe wystąpienie klasy procesów Centrum wiadomości.</span><span class="sxs-lookup"><span data-stu-id="9b579-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="9b579-192">Aby zachować stan za pośrednictwem wiele połączeń i wywołania metody, przy użyciu innej metody, takie jak bazy danych lub statyczna zmienna na klasy koncentratora lub inną klasę, która nie pochodzi od `Hub`.</span><span class="sxs-lookup"><span data-stu-id="9b579-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="9b579-193">Jeśli będą się powtarzać dane w pamięci, przy użyciu metody takie jak zmienna statyczna klasy koncentratora, dane zostaną utracone podczas odtwarzania domen aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9b579-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="9b579-194">Jeśli chcesz wysyłać do klientów z własnego kodu uruchomioną poza klasą Centrum nie można go przez utworzenie wystąpienia klasy wystąpienie koncentratora, ale możesz zrobić to poprzez odwołanie do obiektu context z SignalR dla klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="9b579-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="9b579-195">Aby uzyskać więcej informacji, zobacz [jak wywoływać metod klienta i zarządzanie grupami z poza klasą Centrum](#callfromoutsidehub) dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="9b579-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="9b579-196">Wielkość liter formatu Centrum nazw klientów języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="9b579-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="9b579-197">Domyślnie klientów języka JavaScript odwoływać się do koncentratorów, za pomocą wersji formatu — z uwzględnieniem wielkości liter nazwy klasy.</span><span class="sxs-lookup"><span data-stu-id="9b579-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="9b579-198">SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można są zgodne z konwencjami JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b579-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="9b579-199">Poprzedni przykład, czy określone jako `contosoChatHub` w kodzie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b579-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="9b579-200">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="9b579-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="9b579-201">**JavaScript klienta przy użyciu wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="9b579-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="9b579-202">Jeśli chcesz określić inną nazwę dla klientów do używania, Dodaj `HubName` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9b579-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="9b579-203">Jeśli używasz `HubName` atrybutu, nie ulega zmianie nazwę na format camelcase na klientów języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b579-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="9b579-204">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="9b579-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="9b579-205">**JavaScript klienta przy użyciu wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="9b579-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="9b579-206">Wiele centrów</span><span class="sxs-lookup"><span data-stu-id="9b579-206">Multiple Hubs</span></span>

<span data-ttu-id="9b579-207">Można zdefiniować wiele klas elementu Hub w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9b579-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="9b579-208">Po wykonaniu tej czynności, połączenie jest udostępnione, ale grup są oddzielone:</span><span class="sxs-lookup"><span data-stu-id="9b579-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="9b579-209">Wszyscy klienci będą używać tego samego adresu URL nawiązać połączenia z usługą SignalR ("/ signalr" lub adres URL niestandardowej, jeśli została określona), i czy połączenie jest używany przez wszystkie koncentratory zdefiniowane przez usługę.</span><span class="sxs-lookup"><span data-stu-id="9b579-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="9b579-210">Nie ma żadnej różnicy wydajności dla wielu koncentratorów w porównaniu do definiowania wszystkie funkcje Centrum w jednej klasy.</span><span class="sxs-lookup"><span data-stu-id="9b579-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="9b579-211">Wszystkie koncentratory uzyskać informacje o tej samej żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b579-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="9b579-212">Ponieważ wszystkie koncentratory mają tego samego połączenia, tylko informacje żądania HTTP, które pobiera serwera jest, co polega na oryginalnego żądania HTTP, który nawiązuje połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b579-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="9b579-213">Jeśli używasz żądanie połączenia do przekazywania informacji z klienta do serwera, określając ciąg zapytania nie może dostarczyć różne ciągi zapytania do różnych centrów.</span><span class="sxs-lookup"><span data-stu-id="9b579-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="9b579-214">Wszystkie koncentratory otrzyma tych samych informacji.</span><span class="sxs-lookup"><span data-stu-id="9b579-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="9b579-215">Wygenerowany plik serwery proxy JavaScript będzie zawierać serwerów proxy dla wszystkich koncentratorów w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="9b579-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="9b579-216">Aby uzyskać informacje o serwery proxy JavaScript, zobacz [Podręcznik interfejsu API koncentratorów SignalR - JavaScript Client - wygenerowany serwer proxy i jakie operacje dla Ciebie](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b579-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="9b579-217">Grupy są definiowane w koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="9b579-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="9b579-218">W SignalR, którą można zdefiniować nazwę grupy w celu emisji podzbiorowi klientów połączonych.</span><span class="sxs-lookup"><span data-stu-id="9b579-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="9b579-219">Grupy są obsługiwane oddzielnie dla każdej koncentratora.</span><span class="sxs-lookup"><span data-stu-id="9b579-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="9b579-220">Na przykład grupa o nazwie "Administratorzy" to jeden zestaw klientów z `ContosoChatHub` klasy i tej samej nazwy grupy może odwoływać się do innej grupy klientów dla Twojego `StockTickerHub` klasy.</span><span class="sxs-lookup"><span data-stu-id="9b579-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="9b579-221">Sposób definiowania metod w klasie koncentratora, której klienci mogą wywoływać</span><span class="sxs-lookup"><span data-stu-id="9b579-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="9b579-222">Aby aktywować metody koncentratora, który ma zostać wywołane z klienta, należy zadeklarować publiczną metodę, jak pokazano w poniższych przykładach.</span><span class="sxs-lookup"><span data-stu-id="9b579-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="9b579-223">Można określić zwracany typ i parametry, łącznie z typami złożonymi i tablic, tak jak w dowolnej metody C#.</span><span class="sxs-lookup"><span data-stu-id="9b579-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="9b579-224">Wszystkie dane, które zostanie wyświetlony w parametrach lub zwróć się do obiektu wywołującego przesyłane między klientem a serwerem przy użyciu formatu JSON, a SignalR obsługuje automatycznie powiązania złożone obiekty i tablice obiektów.</span><span class="sxs-lookup"><span data-stu-id="9b579-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="9b579-225">Wielkość liter formatu nazw klientów języka JavaScript — metoda</span><span class="sxs-lookup"><span data-stu-id="9b579-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="9b579-226">Domyślnie klienci JavaScript odwołanie do metod koncentratora za pomocą wersji formatu — z uwzględnieniem wielkości liter w nazwie metody.</span><span class="sxs-lookup"><span data-stu-id="9b579-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="9b579-227">SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można są zgodne z konwencjami JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b579-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="9b579-228">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="9b579-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="9b579-229">**JavaScript klienta przy użyciu wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="9b579-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="9b579-230">Jeśli chcesz określić inną nazwę dla klientów do używania, Dodaj `HubMethodName` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9b579-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="9b579-231">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="9b579-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="9b579-232">**JavaScript klienta przy użyciu wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="9b579-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="9b579-233">Kiedy asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="9b579-233">When to execute asynchronously</span></span>

<span data-ttu-id="9b579-234">Jeśli metoda będzie można długotrwałe lub ma pracę który będzie obejmują oczekujące, takich jak wyszukiwania w bazie danych lub wywołania usługi sieci web należy metody koncentratora asynchroniczne zwracając [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (zamiast `void` zwracać) lub [ Zadanie&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) obiektu (zamiast `T` zwracany typ).</span><span class="sxs-lookup"><span data-stu-id="9b579-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="9b579-235">Po powrocie `Task` obiektu z metody SignalR czeka na `Task` aby zakończyć, a następnie wysyła bez otoki wynik do klienta, więc nie ma żadnej różnicy w sposób code wywołania metody w kliencie.</span><span class="sxs-lookup"><span data-stu-id="9b579-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="9b579-236">Tworzenie metody koncentratora asynchroniczne pozwala uniknąć blokuje połączenia, gdy używa transportu protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9b579-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="9b579-237">Gdy metody koncentratora wykonuje synchronicznie i transport jest protokołu WebSocket, kolejne wywołania metody koncentratora od tego samego klienta są zablokowane, dopiero po zakończeniu metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="9b579-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="9b579-238">W poniższym przykładzie przedstawiono tej samej metody kodowanych, aby były uruchamiane synchronicznie lub asynchronicznie, a następnie kod JavaScript klienta, która działa w przypadku wywoływania danej wersji.</span><span class="sxs-lookup"><span data-stu-id="9b579-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="9b579-239">**Synchroniczne**</span><span class="sxs-lookup"><span data-stu-id="9b579-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="9b579-240">**Asynchroniczne — program ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="9b579-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="9b579-241">**JavaScript klienta przy użyciu wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="9b579-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="9b579-242">Aby uzyskać więcej informacji o sposobie używania metod asynchronicznych w programie ASP.NET 4.5, zobacz [przy użyciu metod asynchronicznych w technologii ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="9b579-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="9b579-243">Definiowanie przeciążenia</span><span class="sxs-lookup"><span data-stu-id="9b579-243">Defining Overloads</span></span>

<span data-ttu-id="9b579-244">Jeśli chcesz zdefiniować przeciążenia metody, liczba parametrów w każdym przeciążenia musi być różne.</span><span class="sxs-lookup"><span data-stu-id="9b579-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="9b579-245">W przypadku przeciążenia jest rozróżnianie właśnie, określając różne typy parametrów, klasy koncentratora zostanie skompilowany, ale usługi SignalR spowoduje zgłoszenie wyjątku w czasie wykonywania, gdy klienci spróbują do wywołania, jednego z przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="9b579-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="9b579-246">Wywoływanie metody z klasy koncentratora klienta</span><span class="sxs-lookup"><span data-stu-id="9b579-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="9b579-247">Aby wywołać metody klienta, z serwera, należy użyć `Clients` właściwość w metodę w klasie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="9b579-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="9b579-248">W poniższym przykładzie pokazano kod serwera, który wywołuje `addNewMessageToPage` na wszystkich połączonych klientów i kod klienta, który definiuje metodę w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b579-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="9b579-249">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="9b579-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="9b579-250">**JavaScript klienta przy użyciu wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="9b579-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="9b579-251">Nie można pobrać wartość zwracaną z metody klienta; Składnia, takich jak `int x = Clients.All.add(1,1)` nie działa.</span><span class="sxs-lookup"><span data-stu-id="9b579-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="9b579-252">Można określić typy złożone i tablic parametrów.</span><span class="sxs-lookup"><span data-stu-id="9b579-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="9b579-253">Poniższy przykład przekazuje typu złożonego do klienta w parametru metody.</span><span class="sxs-lookup"><span data-stu-id="9b579-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="9b579-254">**Kod serwera, która wywołuje metodę klienta przy użyciu obiekt złożony**</span><span class="sxs-lookup"><span data-stu-id="9b579-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="9b579-255">**Kod serwera, który definiuje obiekt złożony**</span><span class="sxs-lookup"><span data-stu-id="9b579-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="9b579-256">**JavaScript klienta przy użyciu wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="9b579-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="9b579-257">Wybieranie których klienci otrzymają RPC</span><span class="sxs-lookup"><span data-stu-id="9b579-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="9b579-258">Zwraca właściwości klientów [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) obiekt, który zapewnia kilka opcji określenie, którzy klienci otrzymają RPC:</span><span class="sxs-lookup"><span data-stu-id="9b579-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="9b579-259">Wszyscy połączeni klienci.</span><span class="sxs-lookup"><span data-stu-id="9b579-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="9b579-260">Tylko klienta wywołującego.</span><span class="sxs-lookup"><span data-stu-id="9b579-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="9b579-261">Wszyscy klienci oprócz klienta wywołującego.</span><span class="sxs-lookup"><span data-stu-id="9b579-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="9b579-262">Określonego klienta identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="9b579-263">W tym przykładzie wywołuje `addContosoChatMessageToPage` na klienta wywołującego i działa tak samo jak przy użyciu `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="9b579-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="9b579-264">Wszyscy połączeni klienci oprócz określonych klientów, identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="9b579-265">Wszyscy połączeni klienci w określonej grupie.</span><span class="sxs-lookup"><span data-stu-id="9b579-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="9b579-266">Wszyscy połączeni klienci w określonej grupie oprócz określonych klientów, identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="9b579-267">Wszyscy połączeni klienci w określonej grupie oprócz klienta wywołującego.</span><span class="sxs-lookup"><span data-stu-id="9b579-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="9b579-268">Weryfikacja nie kompilacji dla nazw — metoda</span><span class="sxs-lookup"><span data-stu-id="9b579-268">No compile-time validation for method names</span></span>

<span data-ttu-id="9b579-269">Określona nazwa metody jest interpretowany jako obiekt dynamiczny, co oznacza, że nie istnieje IntelliSense lub weryfikacji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="9b579-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="9b579-270">Wyrażenie jest obliczane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="9b579-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="9b579-271">Podczas wywołania metody, SignalR wysyła nazwę metody i wartości parametrów do klienta, a jeśli klient ma metody odpowiadającej nazwie, że metoda jest wywoływana i wartości parametrów są przekazywane do niego.</span><span class="sxs-lookup"><span data-stu-id="9b579-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="9b579-272">Jeśli odpowiadającej metody znajduje się na komputerze klienckim, nie błąd.</span><span class="sxs-lookup"><span data-stu-id="9b579-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="9b579-273">Aby uzyskać informacje o formacie dane, które SignalR przesyła do klienta w tle podczas wywoływania metody klienta, zobacz [wprowadzenie do SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b579-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="9b579-274">Dopasowywanie nazw bez uwzględniania wielkości liter — metoda</span><span class="sxs-lookup"><span data-stu-id="9b579-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="9b579-275">Dopasowania nazwy metody jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="9b579-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="9b579-276">Na przykład `Clients.All.addContosoChatMessageToPage` będą wykonywane na serwerze `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, lub `addContosoChatMessageToPage` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="9b579-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="9b579-277">Wykonanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="9b579-277">Asynchronous execution</span></span>

<span data-ttu-id="9b579-278">Asynchronicznie wykonuje metodę, która jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="9b579-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="9b579-279">Wszelki kod, który jest dostarczany po wywołaniu metody klientowi wykona natychmiast bez oczekiwania na SignalR na zakończenie przesyłania danych do klientów, chyba że zostanie określony, że kolejnych wierszy kodu powinna czekać na ukończenie — metoda.</span><span class="sxs-lookup"><span data-stu-id="9b579-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="9b579-280">W poniższych przykładach kodu przedstawiają sposób wykonania dwóch metod klienta po kolei, jeden przy użyciu kod, który działa w programie .NET 4.5, a drugi przy użyciu kod, który działa w .NET 4.</span><span class="sxs-lookup"><span data-stu-id="9b579-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="9b579-281">**Przykład .NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="9b579-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="9b579-282">**Przykład .NET 4**</span><span class="sxs-lookup"><span data-stu-id="9b579-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="9b579-283">Jeśli używasz `await` lub `ContinueWith` do poczekaj na zakończenie metodę klienta, przed wykonaniem następnego wiersza kodu, nie oznacza że klienci faktycznie zostanie wyświetlony komunikat, przed wykonaniem następnego wiersza kodu.</span><span class="sxs-lookup"><span data-stu-id="9b579-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="9b579-284">Tylko klienta wywołania metody "zakończenia" oznacza, że SignalR przeprowadził wszystko, co jest niezbędne do wysyłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="9b579-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="9b579-285">Potrzebujesz weryfikacji, że klienci komunikat, masz program mechanizmu samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="9b579-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="9b579-286">Na przykład można kodu `MessageReceived` metody koncentratora, w `addContosoChatMessageToPage` metody na kliencie może wywołać `MessageReceived` po wykonaniu niezależnie od przypadku pracy należy wykonać na kliencie.</span><span class="sxs-lookup"><span data-stu-id="9b579-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="9b579-287">W `MessageReceived` koncentratora pozwala wykonywać pracę zależy od klienta faktycznie odbioru i przetwarzania oryginalnego wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="9b579-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="9b579-288">Jak używać zmiennej ciągu jako nazwy — metoda</span><span class="sxs-lookup"><span data-stu-id="9b579-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="9b579-289">Aby wywołać metodę klienta za pomocą zmiennej ciągu jako nazwy metody rzutowania `Clients.All` (lub `Clients.Others`, `Clients.Caller`, itd.) do `IClientProxy` , a następnie wywołać [Invoke (methodName, argumenty...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="9b579-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="9b579-290">Jak zarządzać członkostwa w grupie z klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="9b579-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="9b579-291">Grupy w SignalR udostępnia metody emisji wiadomości do określonego podzbiór połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="9b579-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="9b579-292">Grupa może zawierać dowolną liczbę klientów, a klient może być członkiem dowolnej liczby grup.</span><span class="sxs-lookup"><span data-stu-id="9b579-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="9b579-293">Aby zarządzać członkostwa w grupie, należy użyć [Dodaj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) i [Usuń](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metody udostępniane przez `Groups` właściwość klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="9b579-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="9b579-294">W poniższym przykładzie przedstawiono `Groups.Add` i `Groups.Remove` metody używane w metodach koncentratora, które są wywoływane przez kod klienta, a następnie kod JavaScript klienta, który je wywołuje.</span><span class="sxs-lookup"><span data-stu-id="9b579-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="9b579-295">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="9b579-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="9b579-296">**JavaScript klienta przy użyciu wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="9b579-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="9b579-297">Nie trzeba jawnie tworzyć grupy.</span><span class="sxs-lookup"><span data-stu-id="9b579-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="9b579-298">Obowiązująca grupy jest tworzony automatycznie przy pierwszym określić jego nazwę w wywołaniu `Groups.Add`, i zostaje usunięte po usunięciu ostatniego połączenia z członkostwa w nim.</span><span class="sxs-lookup"><span data-stu-id="9b579-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="9b579-299">Brak żadnego interfejsu API do pobierania listy członkostwa grupy lub grup.</span><span class="sxs-lookup"><span data-stu-id="9b579-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="9b579-300">SignalR wysyła wiadomości do klientów i grup na podstawie [modelu pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), i nie przechowuje list grup lub członkostwa w grupach.</span><span class="sxs-lookup"><span data-stu-id="9b579-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="9b579-301">Dzięki temu można zmaksymalizować skalowalność, ponieważ po dodaniu węzła do farmy sieci web SignalR zachowuje stan ma być propagowane do nowego węzła.</span><span class="sxs-lookup"><span data-stu-id="9b579-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="9b579-302">Asynchroniczne wykonywanie metod Add i Remove</span><span class="sxs-lookup"><span data-stu-id="9b579-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="9b579-303">`Groups.Add` i `Groups.Remove` metod wykonania asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="9b579-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="9b579-304">Jeśli chcesz dodać do grupy klienta i natychmiast Wyślij wiadomość do klienta przy użyciu grupy, należy upewnić się, że `Groups.Add` metoda zakończy się najpierw.</span><span class="sxs-lookup"><span data-stu-id="9b579-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="9b579-305">W poniższych przykładach kodu pokazano, jak to zrobić, za pomocą kodu, który działa w .NET 4.5 i za pomocą kodu, który działa w .NET 4</span><span class="sxs-lookup"><span data-stu-id="9b579-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="9b579-306">**Przykład .NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="9b579-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="9b579-307">**Przykład .NET 4**</span><span class="sxs-lookup"><span data-stu-id="9b579-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="9b579-308">Trwałość członkostwa grupy</span><span class="sxs-lookup"><span data-stu-id="9b579-308">Group membership persistence</span></span>

<span data-ttu-id="9b579-309">SignalR śledzi połączenia, nie użytkowników, więc jeśli chcesz, aby użytkownik musiał być w tej samej grupie za każdym razem, gdy użytkownik nawiązuje połączenie, należy wywołać `Groups.Add` za każdym razem, gdy użytkownik ustanawia nowego połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="9b579-310">Po do tymczasowej utraty łączności czasami SignalR można przywrócić połączenia automatycznie.</span><span class="sxs-lookup"><span data-stu-id="9b579-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="9b579-311">W takim przypadku SignalR przywraca tego samego połączenia, bez możliwości tworzenia nowego połączenia, a więc członkostwa grupy klienta zostanie automatycznie przywrócony.</span><span class="sxs-lookup"><span data-stu-id="9b579-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="9b579-312">Jest to możliwe nawet po tymczasowego podziału jest wynikiem ponownym uruchomieniu serwera lub niepowodzenie, ponieważ stan połączenia dla każdego klienta, w tym członkostwa w grupach, jest zwrotnego do klienta.</span><span class="sxs-lookup"><span data-stu-id="9b579-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="9b579-313">Jeśli serwer ulegnie awarii, zostało zastąpione przez nowy serwer, zanim upłynie limit czasu połączenia klienta można automatycznie ponownie na nowym serwerze i ponownie zarejestrować się w grupach, które jest członkiem.</span><span class="sxs-lookup"><span data-stu-id="9b579-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="9b579-314">Gdy połączenia nie mogą zostać przywrócone automatycznie po utracie łączności, lub po upływie limitu czasu połączenia lub gdy klient odłączy się (na przykład, gdy przeglądarka przechodzi do nowej strony), członkostwa w grupach zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="9b579-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="9b579-315">Następnym razem, gdy użytkownik łączy będzie nowego połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="9b579-316">Aby zachować członkostwa w grupach, gdy ten sam użytkownik ustanawia nowego połączenia, aplikacja musi śledzić skojarzenia między użytkownikami i grupami, a następnie przywróć członkostwa w grupach za każdym razem użytkownik ustanawia nowego połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="9b579-317">Aby uzyskać więcej informacji na temat połączeń i ponowne łączenie, zobacz [sposobu obsługi zdarzenia okresu istnienia połączenia w klasie Centrum](#connectionlifetime) dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="9b579-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="9b579-318">Grupy jednego użytkownika</span><span class="sxs-lookup"><span data-stu-id="9b579-318">Single-user groups</span></span>

<span data-ttu-id="9b579-319">Aplikacje używające SignalR zwykle mają do śledzenia skojarzenia między użytkownikami i połączeń, aby dowiedzieć się, użytkownika, który wysłał komunikat i które użytkownicy powinni otrzymywać wiadomości.</span><span class="sxs-lookup"><span data-stu-id="9b579-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="9b579-320">Grupy są używane w jednym z dwóch typowe wzorce dla operacją.</span><span class="sxs-lookup"><span data-stu-id="9b579-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="9b579-321">Grupy pojedynczego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="9b579-321">Single-user groups.</span></span>

    <span data-ttu-id="9b579-322">Można określić nazwę użytkownika, jako nazwę grupy i Dodaj do grupy bieżący identyfikator połączenia, za każdym razem, gdy użytkownik nawiąże połączenie lub ponownie nawiąże połączenie.</span><span class="sxs-lookup"><span data-stu-id="9b579-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="9b579-323">Do wysyłania wiadomości do użytkowników, możesz wysłać do grupy.</span><span class="sxs-lookup"><span data-stu-id="9b579-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="9b579-324">Wadą tej metody jest, że grupa nie udostępniają sposób, aby sprawdzić, czy użytkownik jest w trybie online lub offline.</span><span class="sxs-lookup"><span data-stu-id="9b579-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="9b579-325">Śledzenie skojarzenia między nazwami użytkowników i identyfikatorów połączeń.</span><span class="sxs-lookup"><span data-stu-id="9b579-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="9b579-326">Można przechowywać skojarzenia między każdej nazwy użytkownika i co najmniej jednego połączenia identyfikatorów słownika lub bazy danych i zaktualizować przechowywanych danych za każdym razem, użytkownik połączeniu lub rozłączeniu.</span><span class="sxs-lookup"><span data-stu-id="9b579-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="9b579-327">Aby wysłać wiadomości do użytkownika, należy określić identyfikatorów połączeń.</span><span class="sxs-lookup"><span data-stu-id="9b579-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="9b579-328">Wadą tej metody jest, że ma więcej pamięci.</span><span class="sxs-lookup"><span data-stu-id="9b579-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="9b579-329">Sposób obsługi zdarzeń okres istnienia połączenia w klasie Centrum</span><span class="sxs-lookup"><span data-stu-id="9b579-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="9b579-330">Typowe przyczyny na potrzeby obsługi zdarzeń okres istnienia połączenia są do śledzenia czy użytkownik jest połączony i nie i do śledzenia skojarzenie między nazwy użytkowników i identyfikatorów połączeń.</span><span class="sxs-lookup"><span data-stu-id="9b579-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="9b579-331">Do uruchamiania własnego kodu, gdy klienci Połącz lub Rozłącz, Zastąp `OnConnected`, `OnDisconnected`, i `OnReconnected` metody wirtualne koncentratora klasy, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="9b579-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="9b579-332">Kiedy są wywoływane OnConnected ondisconnected elementu i onreconnected elementu</span><span class="sxs-lookup"><span data-stu-id="9b579-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="9b579-333">Zawsze przeglądarką przechodzi do nowej strony, nowe połączenie ma należy ustalić, co oznacza, że zostanie wykonany SignalR `OnDisconnected` metody następuje `OnConnected` metody.</span><span class="sxs-lookup"><span data-stu-id="9b579-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="9b579-334">Po nawiązaniu nowego połączenia SignalR zawsze tworzy nowy identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="9b579-335">`OnReconnected` Metoda jest wywoływana, gdy nastąpiła tymczasowe przerwy w łączności SignalR automatycznie odzyskiwanie, takich jak kabla jest tymczasowo odłączony po ponownym ustanowieniu połączenia, zanim upłynie limit czasu połączenia. `OnDisconnected` Metoda jest wywoływana, gdy klient został odłączony, a SignalR nie można automatycznie ponownie zestawione, takie jak kiedy przeglądarką przechodzi do nowej strony.</span><span class="sxs-lookup"><span data-stu-id="9b579-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="9b579-336">W związku z tym jest możliwe sekwencji zdarzeń dla danego klienta `OnConnected`, `OnReconnected`, `OnDisconnected`; lub `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="9b579-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="9b579-337">Sekwencja nie będą widzieć `OnConnected`, `OnDisconnected`, `OnReconnected` dla danego połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="9b579-338">`OnDisconnected` — Metoda nie jest wywoływana w niektórych scenariuszach, na przykład jeśli serwer ulegnie uszkodzeniu lub pobiera odtwarzania domen aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9b579-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="9b579-339">Gdy inny serwer, który jest dostarczany w wierszu lub domena aplikacji zakończeniu jej odtworzenia, niektórzy klienci może istnieć możliwość Połącz się ponownie i wyzwalać `OnReconnected` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="9b579-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="9b579-340">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługi zdarzeń okres istnienia połączenia w SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b579-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="9b579-341">Stan elementu wywołującego pusta</span><span class="sxs-lookup"><span data-stu-id="9b579-341">Caller state not populated</span></span>

<span data-ttu-id="9b579-342">Metody obsługi zdarzeń okres istnienia połączenia są nazywane z serwera, co oznacza, że należy umieścić w stan `state` obiektu na kliencie nie zostaną wypełnione w `Caller` właściwości na serwerze.</span><span class="sxs-lookup"><span data-stu-id="9b579-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="9b579-343">Informacje o `state` obiektu i `Caller` właściwości, zobacz [sposób przekazywania stan między klientami a klasy koncentratora](#passstate) dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="9b579-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="9b579-344">Jak uzyskać informacji o kliencie z właściwości kontekstu</span><span class="sxs-lookup"><span data-stu-id="9b579-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="9b579-345">Aby uzyskać informacje o kliencie, należy użyć `Context` właściwość klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="9b579-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="9b579-346">`Context` Zwraca [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) obiektu, który zapewnia dostęp do następujących informacji:</span><span class="sxs-lookup"><span data-stu-id="9b579-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="9b579-347">Identyfikator połączenia klienta wywołującego.</span><span class="sxs-lookup"><span data-stu-id="9b579-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="9b579-348">Identyfikator połączenia jest identyfikatorem GUID, który jest przypisywany przez SignalR (w swoim własnym kodem nie można określić wartości).</span><span class="sxs-lookup"><span data-stu-id="9b579-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="9b579-349">Istnieje jeden identyfikator połączenia dla każdego połączenia i tego samego połączenia, którego identyfikator jest używany przez wszystkie koncentratory, jeśli masz wiele koncentratorów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9b579-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="9b579-350">Dane nagłówka HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b579-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="9b579-351">Można także uzyskać nagłówki HTTP od `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="9b579-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="9b579-352">Przyczyna wiele odwołań do samo jest to, że `Context.Headers` został utworzony, `Context.Request` właściwość została dodana później, a `Context.Headers` został zachowany na potrzeby zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="9b579-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="9b579-353">Zapytanie danych ciągu.</span><span class="sxs-lookup"><span data-stu-id="9b579-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="9b579-354">Można również uzyskać dane ciągu zapytania z `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="9b579-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="9b579-355">Ciąg zapytania, który można pobrać tej właściwości jest ten, który został użyty w żądaniu HTTP, który nawiązaniu połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b579-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="9b579-356">Możesz dodać parametrów ciągu zapytania w kliencie, konfigurując połączenia, które stanowi wygodny sposób przekazywania danych o kliencie od klienta do serwera.</span><span class="sxs-lookup"><span data-stu-id="9b579-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="9b579-357">Poniższy przykład przedstawia sposób dodanie ciągu zapytania w kliencie JavaScript, gdy używasz wygenerowany serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="9b579-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="9b579-358">Aby uzyskać więcej informacji na temat ustawiania parametrów ciągu zapytania, zobacz przewodniki interfejsu API dla [JavaScript](index.md) i [.NET](index.md) klientów.</span><span class="sxs-lookup"><span data-stu-id="9b579-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="9b579-359">Metoda transportu używaną dla połączenia danych ciąg zapytania, oraz inne wartości, używana wewnętrznie przez SignalR można znaleźć:</span><span class="sxs-lookup"><span data-stu-id="9b579-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="9b579-360">Wartość `transportMethod` będzie "Websocket", "serverSentEvents", "foreverFrame" lub "longPolling".</span><span class="sxs-lookup"><span data-stu-id="9b579-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="9b579-361">Należy pamiętać, że jeśli wybierzesz tę wartość `OnConnected` metoda obsługi zdarzeń, w niektórych scenariuszach może wstępnie pobrać wartość transportu, która nie jest metodą końcowego wynegocjowanym transport dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="9b579-362">W takim przypadku metoda zgłosi wyjątek i zostanie wywołany ponownie później po nawiązaniu metodę końcowego transportu.</span><span class="sxs-lookup"><span data-stu-id="9b579-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="9b579-363">Pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="9b579-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="9b579-364">Można także uzyskać plików cookie z `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="9b579-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="9b579-365">Informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="9b579-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="9b579-366">Obiekt kontekstu HTTP dla żądania:</span><span class="sxs-lookup"><span data-stu-id="9b579-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="9b579-367">Użyj tej metody, zamiast pobierania `HttpContext.Current` uzyskanie `HttpContext` obiektu połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b579-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="9b579-368">Sposób przekazywania stan między klientami a klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="9b579-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="9b579-369">Serwer proxy klienta udostępnia `state` obiektu, w którym można przechowywać dane, które mają zostać przesłane do serwera przy każdym wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="9b579-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="9b579-370">Na serwerze można uzyskać dostępu do tych danych w `Clients.Caller` właściwości w metodach koncentratora, które są wywoływane przez klientów.</span><span class="sxs-lookup"><span data-stu-id="9b579-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="9b579-371">`Clients.Caller` Właściwość jest pusta dla metody obsługi zdarzeń okres istnienia połączenia `OnConnected`, `OnDisconnected`, i `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="9b579-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="9b579-372">Tworzenie lub aktualizowanie danych w `state` obiektu i `Clients.Caller` właściwości działa w obu kierunkach.</span><span class="sxs-lookup"><span data-stu-id="9b579-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="9b579-373">Można aktualizować wartości na serwerze i są one przekazywane z powrotem do klienta.</span><span class="sxs-lookup"><span data-stu-id="9b579-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="9b579-374">Poniższy przykład przedstawia kod klienta JavaScript, który zapisuje stan w celu przesłania go do serwera z każdego wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="9b579-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="9b579-375">Poniższy przykład przedstawia kod odpowiednik w klienta programu .NET.</span><span class="sxs-lookup"><span data-stu-id="9b579-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="9b579-376">W klasie Centrum mogą uzyskiwać dostęp do tych danych w `Clients.Caller` właściwości.</span><span class="sxs-lookup"><span data-stu-id="9b579-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="9b579-377">Poniższy przykład przedstawia kod, który pobiera stan określonego w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="9b579-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="9b579-378">Ten mechanizm utrwalanie stanu nie jest przeznaczony dla dużych ilości danych, ponieważ wszystko, co należy umieścić w `state` lub `Clients.Caller` właściwość jest zwrotnego z każdego wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="9b579-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="9b579-379">Jest to przydatne w przypadku mniejszych elementy, takie jak nazwy użytkowników lub liczników.</span><span class="sxs-lookup"><span data-stu-id="9b579-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="9b579-380">Sposób obsługi błędów w klasie Centrum</span><span class="sxs-lookup"><span data-stu-id="9b579-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="9b579-381">Do obsługi błędów, które występują w Centrum metody klasy, użyj jednej lub obu z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="9b579-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="9b579-382">Zawijanie kodu metody w bloków try-catch i dziennika obiekt wyjątku.</span><span class="sxs-lookup"><span data-stu-id="9b579-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="9b579-383">Wyjątek na potrzeby debugowania można wysłać do klienta, ale zabezpieczeń powodów wysyłanie szczegółowych informacji do klientów w środowisku produkcyjnym nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="9b579-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="9b579-384">Utwórz moduł potoku koncentratory, który obsługuje [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) metody.</span><span class="sxs-lookup"><span data-stu-id="9b579-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="9b579-385">W poniższym przykładzie przedstawiono modułu potoku, który rejestruje błędy, następuje kod w pliku Global.asax, który injects modułu z potokiem koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="9b579-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="9b579-386">Aby uzyskać więcej informacji na temat modułów potoku koncentratora, zobacz [sposób dostosowywania procesu koncentratory](#hubpipeline) dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="9b579-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="9b579-387">Jak włączyć śledzenie</span><span class="sxs-lookup"><span data-stu-id="9b579-387">How to enable tracing</span></span>

<span data-ttu-id="9b579-388">Aby włączyć śledzenie po stronie serwera, Dodaj system.diagnostics element do pliku Web.config, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="9b579-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="9b579-389">Po uruchomieniu aplikacji w programie Visual Studio można wyświetlić dzienniki w **dane wyjściowe** okna.</span><span class="sxs-lookup"><span data-stu-id="9b579-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="9b579-390">Jak wywoływać metod klienta i zarządzanie grupami z poza klasą Centrum</span><span class="sxs-lookup"><span data-stu-id="9b579-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="9b579-391">Aby wywołać klienta metody z innej klasy niż klasa koncentratora, pobrać odwołanie do obiektu context z SignalR dla koncentratora i używać, aby wywoływać metod na kliencie lub Zarządzanie grupami.</span><span class="sxs-lookup"><span data-stu-id="9b579-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="9b579-392">Poniższy przykład `StockTicker` klasy pobiera obiekt kontekstu, zapisze go w wystąpieniu klasy przechowuje wystąpienia klasy statycznej właściwości i używa kontekstu z pojedyncze wystąpienie klasy do wywołania `updateStockPrice` metody na komputerach klienckich, które są podłączone do koncentratora o nazwie `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="9b579-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="9b579-393">Jeśli musisz używać wielu godzin kontekstu w obiekcie długotrwałe, Pobierz odwołanie jednokrotnie i zapisz go zamiast pobierania go ponownie za każdym razem.</span><span class="sxs-lookup"><span data-stu-id="9b579-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="9b579-394">Uzyskanie kontekstu raz zapewnia SignalR i wysyła wiadomości do klientów w tej samej kolejności, w której metody koncentratora upewnij klienta wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="9b579-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="9b579-395">Samouczek, który pokazuje, jak używać kontekstu SignalR dla koncentratora, zobacz [serwera emisji z ASP.NET SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b579-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="9b579-396">Wywoływanie metody klienta</span><span class="sxs-lookup"><span data-stu-id="9b579-396">Calling client methods</span></span>

<span data-ttu-id="9b579-397">Można określić, którzy klienci otrzymają RPC, ale ma mniej opcji niż pod numerem klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="9b579-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="9b579-398">Przyczyną tego błędu jest to, czy kontekst nie jest skojarzony z określonego wywołania klienta, więc wszystkie metody wymagające wiedzy bieżący identyfikator połączenia, takich jak `Clients.Others`, lub `Clients.Caller`, lub `Clients.OthersInGroup`, nie są dostępne.</span><span class="sxs-lookup"><span data-stu-id="9b579-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="9b579-399">Dostępne są następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="9b579-399">The following options are available:</span></span>

- <span data-ttu-id="9b579-400">Wszyscy połączeni klienci.</span><span class="sxs-lookup"><span data-stu-id="9b579-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="9b579-401">Określonego klienta identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="9b579-402">Wszyscy połączeni klienci oprócz określonych klientów, identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="9b579-403">Wszyscy połączeni klienci w określonej grupie.</span><span class="sxs-lookup"><span data-stu-id="9b579-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="9b579-404">Wszystkich połączonych klientów należących do określonej grupy, z wyjątkiem określonych klientach, identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="9b579-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="9b579-405">Jeśli dzwonisz w klasie z systemem innym niż Centrum metody w klasie koncentratora, można przekazywać w bieżący identyfikator połączenia i użyj jej z `Clients.Client`, `Clients.AllExcept`, lub `Clients.Group` do symulowania `Clients.Caller`, `Clients.Others`, lub `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="9b579-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="9b579-406">W poniższym przykładzie `MoveShapeHub` klasy przekazuje identyfikator połączenia, aby `Broadcaster` klasy, aby `Broadcaster` klasy można symulować `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="9b579-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="9b579-407">Członkostwo w grupie zarządzania</span><span class="sxs-lookup"><span data-stu-id="9b579-407">Managing group membership</span></span>

<span data-ttu-id="9b579-408">Do zarządzania grupami mają te same opcje tak jak w klasie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="9b579-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="9b579-409">Dodawanie klienta do grupy</span><span class="sxs-lookup"><span data-stu-id="9b579-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="9b579-410">Usunięcie klienta z grupy</span><span class="sxs-lookup"><span data-stu-id="9b579-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="9b579-411">Dostosowywanie potoku koncentratory</span><span class="sxs-lookup"><span data-stu-id="9b579-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="9b579-412">SignalR pozwala na iniekcję własny kod do potoku koncentratora.</span><span class="sxs-lookup"><span data-stu-id="9b579-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="9b579-413">W poniższym przykładzie przedstawiono niestandardowego modułu potoku koncentratora zaloguje się każdego przychodzącego wywołania metody otrzymała od klienta i wychodzących wywołanie metody wywoływane na komputerze klienckim:</span><span class="sxs-lookup"><span data-stu-id="9b579-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="9b579-414">Poniższy kod w *Global.asax* pliku rejestruje modułu do działania w potoku koncentratora:</span><span class="sxs-lookup"><span data-stu-id="9b579-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="9b579-415">Istnieje wiele różnych metod, które można zastąpić.</span><span class="sxs-lookup"><span data-stu-id="9b579-415">There are many different methods that you can override.</span></span> <span data-ttu-id="9b579-416">Aby uzyskać pełną listę, zobacz [metody HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="9b579-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
