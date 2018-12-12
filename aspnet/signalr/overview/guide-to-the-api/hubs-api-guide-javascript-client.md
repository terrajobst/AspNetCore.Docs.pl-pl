---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Podręcznik interfejsu API centrów SignalR platformy ASP.NET — klient JavaScript | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 2 w klientów języka JavaScript, takie jak przeglądarki i applicat Windows Store (WinJS)...
ms.author: riande
ms.date: 09/28/2015
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8e493eda256351904da49e1222773f188e6a2058
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288069"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="a8918-103">Podręcznik interfejsu API centrów SignalR platformy ASP.NET — klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="a8918-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="a8918-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a8918-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="a8918-105">Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 2 w klientów języka JavaScript, takie jak przeglądarki i aplikacje Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="a8918-105">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="a8918-106">Interfejsu API centrów SignalR umożliwia zdalne wywołania procedur (RPC) z serwera do połączonych klientów i od klientów z serwerem.</span><span class="sxs-lookup"><span data-stu-id="a8918-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="a8918-107">W kodzie serwera należy zdefiniować metody, które mogą być wywoływane przez klientów, a wywołanie metody, które są uruchamiane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="a8918-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="a8918-108">W kodzie klienta definiowania metod, które mogą być wywoływane z serwera, a wywołanie metody, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a8918-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="a8918-109">SignalR zajmuje się wszystkie nadmiar klient serwer dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="a8918-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="a8918-110">SignalR oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="a8918-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="a8918-111">Wprowadzenie do SignalR, centra i połączenia trwałego, zobacz [wprowadzenie do SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="a8918-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="a8918-112">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="a8918-112">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="a8918-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a8918-113">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="a8918-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a8918-114">.NET 4.5</span></span>
> - <span data-ttu-id="a8918-115">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="a8918-115">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="a8918-116">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="a8918-116">Previous versions of this topic</span></span>
>
> <span data-ttu-id="a8918-117">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="a8918-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="a8918-118">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="a8918-118">Questions and comments</span></span>
>
> <span data-ttu-id="a8918-119">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="a8918-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a8918-120">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="a8918-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="a8918-121">Omówienie</span><span class="sxs-lookup"><span data-stu-id="a8918-121">Overview</span></span>

<span data-ttu-id="a8918-122">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="a8918-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="a8918-123">Wygenerowany serwer proxy i przeznaczenie dla Ciebie</span><span class="sxs-lookup"><span data-stu-id="a8918-123">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="a8918-124">Kiedy należy używać wygenerowany serwer proxy</span><span class="sxs-lookup"><span data-stu-id="a8918-124">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="a8918-125">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="a8918-125">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="a8918-126">Jak odwoływać się dynamicznie generowanym serwera proxy</span><span class="sxs-lookup"><span data-stu-id="a8918-126">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="a8918-127">Jak utworzyć plik fizyczny dla elementu SignalR wygenerowany serwer proxy</span><span class="sxs-lookup"><span data-stu-id="a8918-127">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="a8918-128">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="a8918-128">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="a8918-129">$. connection.hub jest taki sam obiekt, tworzy tego $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="a8918-129">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="a8918-130">Wykonanie asynchroniczne metody start</span><span class="sxs-lookup"><span data-stu-id="a8918-130">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="a8918-131">Jak nawiązać połączenie między domenami</span><span class="sxs-lookup"><span data-stu-id="a8918-131">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="a8918-132">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="a8918-132">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="a8918-133">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="a8918-133">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="a8918-134">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="a8918-134">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="a8918-135">Jak uzyskać serwera proxy dla klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="a8918-135">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="a8918-136">Sposób definiowania metody na kliencie, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="a8918-136">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="a8918-137">Jak wywołać metody serwera z poziomu klienta</span><span class="sxs-lookup"><span data-stu-id="a8918-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="a8918-138">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="a8918-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="a8918-139">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="a8918-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="a8918-140">Włączanie rejestrowania po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="a8918-140">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="a8918-141">Dokumentacja na temat programu server lub klientów programu .NET, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="a8918-141">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="a8918-142">Podręcznik interfejsu API centrów SignalR — serwer</span><span class="sxs-lookup"><span data-stu-id="a8918-142">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="a8918-143">Podręcznik interfejsu API centrów SignalR — klient modelu .NET</span><span class="sxs-lookup"><span data-stu-id="a8918-143">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="a8918-144">Składnik serwera SignalR 2 jest dostępna tylko w .NET 4.5 (chociaż klienta platformy .NET dla SignalR 2 program .NET 4.0).</span><span class="sxs-lookup"><span data-stu-id="a8918-144">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="a8918-145">Wygenerowany serwer proxy i przeznaczenie dla Ciebie</span><span class="sxs-lookup"><span data-stu-id="a8918-145">The generated proxy and what it does for you</span></span>

<span data-ttu-id="a8918-146">Można programować klienta JavaScript do komunikowania się z usługą SignalR, z lub bez serwera proxy, który generuje SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8918-146">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="a8918-147">Serwer proxy jest dla Ciebie to uprościć jego składnię kodu używana do łączenia metod zapisywania, które wymaga serwera, i wywoływać metody na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a8918-147">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="a8918-148">Podczas pisania kodu w celu wywołania metody serwera wygenerowany serwer proxy umożliwia użycie składni, która wygląda tak, jakby były wykonywania funkcji lokalnej: możesz napisać `serverMethod(arg1, arg2)` zamiast `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="a8918-148">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="a8918-149">Składnia wygenerowany serwer proxy umożliwia również natychmiastowe i zrozumiałym błąd po stronie klienta, jeśli wpisujesz nazwę metody serwera.</span><span class="sxs-lookup"><span data-stu-id="a8918-149">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="a8918-150">A jeśli ręcznie utworzyć plik, który definiuje serwery proxy, możesz również uzyskać obsługę technologii IntelliSense do pisania kodu, który wywołuje metody serwera.</span><span class="sxs-lookup"><span data-stu-id="a8918-150">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="a8918-151">Na przykład załóżmy, że masz następujące klasy koncentratora na serwerze:</span><span class="sxs-lookup"><span data-stu-id="a8918-151">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="a8918-152">W poniższych przykładach kodu pokazano, co JavaScript kod wygląda do wywoływania `NewContosoChatMessage` metody na serwerze i odbieranie wywołań `addContosoChatMessageToPage` metody z serwera.</span><span class="sxs-lookup"><span data-stu-id="a8918-152">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="a8918-153">**Za pomocą wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="a8918-153">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="a8918-154">**Bez wygenerowany serwer proxy**</span><span class="sxs-lookup"><span data-stu-id="a8918-154">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="a8918-155">Kiedy należy używać wygenerowany serwer proxy</span><span class="sxs-lookup"><span data-stu-id="a8918-155">When to use the generated proxy</span></span>

<span data-ttu-id="a8918-156">Jeśli chcesz zarejestrować wiele procedur obsługi zdarzeń dla metod klienta, który wywołuje serwer, nie możesz użyć wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="a8918-156">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="a8918-157">W przeciwnym razie można używać wygenerowany serwer proxy lub nie na podstawie kodowania preferencji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a8918-157">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="a8918-158">Jeśli nie chcesz go użyć, nie musisz odwoływać się do adresu URL "koncentratory/signalr" w `script` elementu w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="a8918-158">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="a8918-159">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="a8918-159">Client setup</span></span>

<span data-ttu-id="a8918-160">Klient JavaScript wymaga odwołania do technologii jQuery i plik JavaScript core SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8918-160">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="a8918-161">Wersja jQuery musi być 1.6.4 lub nowsze wersje główne, takich jak 1.7.2, 1.8.2 lub 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="a8918-161">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="a8918-162">Jeśli zdecydujesz się używać wygenerowany serwer proxy, należy również odwołanie do serwera proxy SignalR wygenerowany plik JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a8918-162">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="a8918-163">Poniższy przykład pokazuje, jak odwołania może wyglądać na stronie HTML, który używa wygenerowany serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="a8918-163">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="a8918-164">Te odwołania muszą być zawarte w następującej kolejności: jQuery imię i nazwisko core SignalR po tym i serwery proxy SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8918-164">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="a8918-165">Jak odwoływać się dynamicznie generowanym serwera proxy</span><span class="sxs-lookup"><span data-stu-id="a8918-165">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="a8918-166">W powyższym przykładzie odwołanie do serwera proxy SignalR, generowany jest dynamicznie generowanego kodu JavaScript, nie do pliku fizycznego.</span><span class="sxs-lookup"><span data-stu-id="a8918-166">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="a8918-167">SignalR tworzy kod JavaScript dla serwera proxy na bieżąco i służy ona do klienta w odpowiedzi na adres URL "/ signalr/centra".</span><span class="sxs-lookup"><span data-stu-id="a8918-167">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="a8918-168">Jeśli określono inny podstawowy adres URL dla połączenia SignalR na serwerze w Twojej `MapSignalR` metody, adres URL pliku dynamicznie generowanym serwera proxy jest adres URL niestandardowego za pomocą "/ koncentratory" dołączone do niego.</span><span class="sxs-lookup"><span data-stu-id="a8918-168">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="a8918-169">Dla klientów systemu Windows 8 (Windows Store) JavaScript należy użyć pliku fizycznego serwera proxy zamiast generowanych dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="a8918-169">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="a8918-170">Aby uzyskać więcej informacji, zobacz [sposobu tworzenia pliku fizycznego dla elementu SignalR wygenerowany serwer proxy](#manualproxy) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="a8918-170">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="a8918-171">W technologii ASP.NET MVC 4 lub 5 widoku Razor należy użyć tylda do odwoływania się do katalogu głównego aplikacji, w której można się odwołać plik serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="a8918-171">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="a8918-172">Aby uzyskać więcej informacji na temat korzystania z SignalR w MVC 5, zobacz [wprowadzenie do SignalR i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="a8918-172">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="a8918-173">W widoku Razor programu ASP.NET MVC 3, należy użyć `Url.Content` usługi serwera proxy dla odwołania do pliku:</span><span class="sxs-lookup"><span data-stu-id="a8918-173">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="a8918-174">W aplikacji formularzy sieci Web ASP.NET, za pomocą `ResolveClientUrl` swoje serwery proxy odwołanie do pliku lub zarejestrować go za pośrednictwem Menedżera skryptów przy użyciu ścieżki względnej katalogu głównego aplikacji (rozpoczynający się tyldą):</span><span class="sxs-lookup"><span data-stu-id="a8918-174">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="a8918-175">Zgodnie z ogólną zasadą należy użyć tej samej metody dla określania adresu URL "/ signalr/centra", której użyjesz dla plików CSS i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a8918-175">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="a8918-176">Jeśli określisz adresu URL bez użycia tyldy w niektórych scenariuszach aplikacja będzie działać poprawnie podczas testowania w programie Visual Studio za pomocą usług IIS Express, ale zakończy się niepowodzeniem z powodu błędu 404, podczas wdrażania usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="a8918-176">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="a8918-177">Aby uzyskać więcej informacji, zobacz **rozpoznawania odwołań do zasobów na poziomie głównym** w [serwerów sieci Web w programie Visual Studio dla projektów sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="a8918-177">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="a8918-178">Po uruchomieniu projektu sieci web w programie Visual Studio 2013 w trybie debugowania, jeśli używasz programu Internet Explorer jako przeglądarki widać plik serwera proxy w **Eksploratora rozwiązań** w obszarze **dokumenty skryptów**, jak pokazano na na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="a8918-178">When you run a web project in Visual Studio 2013 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Plik wygenerowany serwer proxy JavaScript w Eksploratorze rozwiązań](hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="a8918-180">Aby wyświetlić zawartość pliku, kliknij dwukrotnie **koncentratory**.</span><span class="sxs-lookup"><span data-stu-id="a8918-180">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="a8918-181">Jeśli nie używasz programu Visual Studio 2012 lub 2013 i programu Internet Explorer lub jeśli nie jesteś w trybie debugowania, możesz także uzyskać zawartość pliku, przechodząc do adresu URL "/ signalR/centra".</span><span class="sxs-lookup"><span data-stu-id="a8918-181">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="a8918-182">Na przykład, jeśli witryna jest hostowana na `http://localhost:56699`, przejdź do `http://localhost:56699/SignalR/hubs` w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="a8918-182">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="a8918-183">Jak utworzyć plik fizyczny dla elementu SignalR wygenerowany serwer proxy</span><span class="sxs-lookup"><span data-stu-id="a8918-183">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="a8918-184">Jako alternatywy dla dynamicznie generowanych serwera proxy można utworzyć pliku fizycznego, który ma kod serwera proxy i odwoływać się do tego pliku.</span><span class="sxs-lookup"><span data-stu-id="a8918-184">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="a8918-185">Można w tym kontrolę nad buforowania i tworzenia pakietów zachowanie lub korzystania z funkcji IntelliSense podczas kodowania wywołania metody serwera.</span><span class="sxs-lookup"><span data-stu-id="a8918-185">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="a8918-186">Aby utworzyć plik serwera proxy, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="a8918-186">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="a8918-187">Zainstaluj [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="a8918-187">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="a8918-188">Otwórz wiersz polecenia i przejdź do *narzędzia* folder zawierający plik SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="a8918-188">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="a8918-189">Folder narzędzia znajduje się w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="a8918-189">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="a8918-190">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a8918-190">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="a8918-191">Ścieżka do Twojej *.dll* jest zazwyczaj *bin* folderu w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="a8918-191">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="a8918-192">To polecenie tworzy plik o nazwie *server.js* w tym samym folderze co *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="a8918-192">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="a8918-193">Umieść *server.js* plików w folderze właściwe w projekcie, zmienić jego nazwę odpowiednio do aplikacji i Dodaj odwołanie do niego zamiast odwołania "koncentratory/signalr".</span><span class="sxs-lookup"><span data-stu-id="a8918-193">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="a8918-194">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="a8918-194">How to establish a connection</span></span>

<span data-ttu-id="a8918-195">Przed może nawiązać połączenie, należy utworzyć obiekt połączenia, utworzyć serwer proxy i zarejestruj procedury obsługi zdarzeń dla metod, które mogą być wywoływane z serwera.</span><span class="sxs-lookup"><span data-stu-id="a8918-195">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="a8918-196">Po skonfigurowaniu serwera proxy i programów obsługi nawiązania połączenia przez wywołanie metody `start` metody.</span><span class="sxs-lookup"><span data-stu-id="a8918-196">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="a8918-197">Jeśli używasz wygenerowany serwer proxy, nie trzeba utworzyć obiekt połączenia we własnym kodzie, ponieważ kod wygenerowany serwer proxy zrobi to za Ciebie.</span><span class="sxs-lookup"><span data-stu-id="a8918-197">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="a8918-198">**Nawiąż połączenie (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-198">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="a8918-199">**Nawiąż połączenie (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-199">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="a8918-200">Przykładowy kod używa domyślnej "/ signalr" adres URL, aby nawiązać połączenie z usługą SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8918-200">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="a8918-201">Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="a8918-201">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="a8918-202">Domyślnie lokalizacją Centrum jest bieżący serwer; Jeśli łączysz się z innym serwerem, podaj adres URL przed wywołaniem `start` metodzie, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a8918-202">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="a8918-203">Zwykle rejestrowanie procedur obsługi zdarzeń przed wywołaniem `start` metodę, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="a8918-203">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="a8918-204">Jeśli chcesz zarejestrować niektóre procedury obsługi zdarzeń po ustanowieniu połączenia, możesz to zrobić, ale musisz się zarejestrować, co najmniej jeden z Twojej handler(s) zdarzeń przed wywołaniem `start` metody.</span><span class="sxs-lookup"><span data-stu-id="a8918-204">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="a8918-205">Jeden przyczyną jest to, że może istnieć wiele centrów w aplikacji, ale nie chcesz wyzwolić `OnConnected` zdarzenia dla każdego Centrum, jeśli tylko będą korzystać z jednym z nich.</span><span class="sxs-lookup"><span data-stu-id="a8918-205">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="a8918-206">Po nawiązaniu połączenia jest obecność metodę klienta dla serwera proxy koncentratora, co informuje SignalR do wyzwolenia `OnConnected` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="a8918-206">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="a8918-207">Jeżeli nie zarejestrujesz dowolnej procedury obsługi zdarzeń przed wywołaniem `start` metody, można do wywołania metody koncentratora, ale Centrum `OnConnected` nie można wywołać metody i żadnych metod klienta zostanie wywołana z serwera.</span><span class="sxs-lookup"><span data-stu-id="a8918-207">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="a8918-208">$. connection.hub jest taki sam obiekt, tworzy tego $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="a8918-208">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="a8918-209">Jak widać w przykładach, gdy używasz wygenerowany serwer proxy `$.connection.hub` odwołuje się do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="a8918-209">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="a8918-210">Jest to ten sam obiekt, który można uzyskać przez wywołanie metody `$.hubConnection()` gdy nie używasz wygenerowany serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="a8918-210">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="a8918-211">Kod wygenerowany serwer proxy tworzy połączenie, wykonując następującą instrukcję:</span><span class="sxs-lookup"><span data-stu-id="a8918-211">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Tworzenie połączenia w pliku wygenerowanego serwera proxy](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="a8918-213">Wygenerowany serwer proxy jest używany, można wykonać z `$.connection.hub` , można wykonać za pomocą obiektu połączenia, gdy nie używasz wygenerowany serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="a8918-213">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="a8918-214">Wykonanie asynchroniczne metody start</span><span class="sxs-lookup"><span data-stu-id="a8918-214">Asynchronous execution of the start method</span></span>

<span data-ttu-id="a8918-215">`start` Metoda jest wykonywana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="a8918-215">The `start` method executes asynchronously.</span></span> <span data-ttu-id="a8918-216">Zwraca [obiektu opóźnionych jQuery](http://api.jquery.com/category/deferred-object/), co oznacza, że można dodać funkcji wywołania zwrotnego wywoływania metod, takich jak `pipe`, `done`, i `fail`.</span><span class="sxs-lookup"><span data-stu-id="a8918-216">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="a8918-217">Jeśli masz kod, który ma zostać wykonany po nawiązaniu połączenia, takie jak wywołanie metody serwera, umieść ten kod w funkcji wywołania zwrotnego lub wywołać go z funkcji wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="a8918-217">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="a8918-218">`.done` Metody wywołania zwrotnego jest wykonywane po ustanowieniu połączenia, a po dowolny kod, który ma swojej `OnConnected` zakończy się wykonywanie metody obsługi zdarzeń na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a8918-218">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="a8918-219">Jeśli umieścisz instrukcję "Teraz connected", z poprzedniego przykładu jako następnego wiersza kodu po `start` wywołania metody (nie w `.done` wywołania zwrotnego), `console.log` wiersza zostaną wykonane, zanim połączenie zostanie nawiązane, jak pokazano w następującym przykład:</span><span class="sxs-lookup"><span data-stu-id="a8918-219">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Problem ze sposobem pisania kodu, który jest uruchamiany po nawiązaniu połączenia](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="a8918-221">Jak nawiązać połączenie między domenami</span><span class="sxs-lookup"><span data-stu-id="a8918-221">How to establish a cross-domain connection</span></span>

<span data-ttu-id="a8918-222">Zazwyczaj, gdy przeglądarka ładuje stronę z `http://contoso.com`, połączenia SignalR jest w tej samej domenie, w `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="a8918-222">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="a8918-223">Jeśli strona z `http://contoso.com` nawiązuje połączenie `http://fabrikam.com/signalr`, oznacza to połączenie między domenami.</span><span class="sxs-lookup"><span data-stu-id="a8918-223">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="a8918-224">Ze względów bezpieczeństwa połączeń między domenami są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="a8918-224">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="a8918-225">W SignalR 1.x żądania obejmujące różne domeny była kontrolowana przez pojedynczej flagi EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="a8918-225">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="a8918-226">Ta flaga kontrolowane żądania CORS i JSONP.</span><span class="sxs-lookup"><span data-stu-id="a8918-226">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="a8918-227">Funkcje i elastyczność, obsługa wszystkich mechanizmu CORS zostało usunięte z składnika serwera SignalR (klientów JavaScript nadal CORS normalnie z niego korzystać w przypadku wykrycia, czy przeglądarka obsługuje on), i nowego oprogramowania pośredniczącego OWIN został udostępniony do obsługi tych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="a8918-227">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="a8918-228">Jeśli JSONP jest wymagany na kliencie (do obsługi żądań między domenami w starszych przeglądarkach), trzeba będzie ją można jawnie włączyć, ustawiając `EnableJSONP` na `HubConfiguration` obiekt `true`, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="a8918-228">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="a8918-229">JSONP jest domyślnie wyłączony, ponieważ jest to mniej bezpieczna niż mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="a8918-229">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="a8918-230">**Dodawanie Microsoft.Owin.Cors do projektu:** Aby zainstalować tę bibliotekę, uruchom następujące polecenie w konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="a8918-230">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="a8918-231">To polecenie spowoduje dodanie 2.1.0 wersji pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="a8918-231">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="a8918-232">Wywoływanie UseCors</span><span class="sxs-lookup"><span data-stu-id="a8918-232">Calling UseCors</span></span>

 <span data-ttu-id="a8918-233">Poniższy fragment kodu demonstruje sposób implementacji połączeń między domenami w SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="a8918-233">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="a8918-234">**Implementowanie żądania między domenami w SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="a8918-234">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="a8918-235">Poniższy kod ilustruje sposób włączyć mechanizm CORS i JSONP w projekcie biblioteki SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="a8918-235">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="a8918-236">Ten przykładowy kod korzysta `Map` i `RunSignalR` zamiast `MapSignalR`, dzięki czemu oprogramowanie pośredniczące CORS działa tylko w przypadku żądania SignalR, które wymagają obsługi mechanizmu CORS (a nie dla całego ruchu w ścieżce określonej w `MapSignalR`.) Mapy może również dla innego oprogramowania pośredniczącego, które musi zostać uruchomiony dla określonego prefiksu adresu URL, a nie dla całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a8918-236">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="a8918-237">Nie należy ustawiać `jQuery.support.cors` na wartość true w kodzie.</span><span class="sxs-lookup"><span data-stu-id="a8918-237">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![Nie jest ustawiana na wartość true jQuery.support.cors](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="a8918-239">SignalR obsługuje korzystanie z mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="a8918-239">SignalR handles the use of CORS.</span></span> <span data-ttu-id="a8918-240">Ustawienie `jQuery.support.cors` do wartości true powoduje wyłączenie JSONP, ponieważ powoduje ona SignalR założył, przeglądarka obsługuje mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="a8918-240">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="a8918-241">Podczas łączenia z adresem URL localhost, programu Internet Explorer 10 nie należy wziąć pod uwagę jej połączenie między domenami, dzięki czemu aplikacja będzie działać lokalnie za pomocą programu Internet Explorer 10, nawet jeśli nie zostały włączone połączenia między domenami na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a8918-241">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="a8918-242">Aby dowiedzieć się, jak za pomocą połączeń między domenami, za pomocą programu Internet Explorer 9, zobacz [wątek w witrynie StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="a8918-242">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="a8918-243">Aby dowiedzieć się, jak za pomocą przeglądarki Chrome przy użyciu połączenia między domenami, zobacz [wątek w witrynie StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="a8918-243">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="a8918-244">Przykładowy kod używa domyślnej "/ signalr" adres URL, aby nawiązać połączenie z usługą SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8918-244">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="a8918-245">Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="a8918-245">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="a8918-246">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="a8918-246">How to configure the connection</span></span>

<span data-ttu-id="a8918-247">Przed nawiązaniem połączenia, można określić parametry ciągu zapytania lub Określ metodę transportu.</span><span class="sxs-lookup"><span data-stu-id="a8918-247">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="a8918-248">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="a8918-248">How to specify query string parameters</span></span>

<span data-ttu-id="a8918-249">Jeśli chcesz wysyłać dane do serwera, gdy klient nawiąże połączenie, można dodać parametry ciągu zapytania do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="a8918-249">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="a8918-250">Poniższe przykłady pokazują, jak można ustawić jako parametr ciągu zapytania w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="a8918-250">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="a8918-251">**Ustaw wartość ciągu zapytania przed wywołaniem metody start (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-251">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="a8918-252">**Ustaw wartość ciągu zapytania przed wywołaniem metody start (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-252">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="a8918-253">Poniższy przykład pokazuje, jak odczytywać parametr ciągu zapytania w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="a8918-253">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="a8918-254">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="a8918-254">How to specify the transport method</span></span>

<span data-ttu-id="a8918-255">Częścią procesu nawiązywania połączenia klienta SignalR zwykle negocjuje z serwerem, aby określić najlepsze transportu, która jest obsługiwana przez serwer i klienta.</span><span class="sxs-lookup"><span data-stu-id="a8918-255">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="a8918-256">Jeśli znasz już transport, którego chcesz użyć, można pominąć ten proces negocjacji, określając metodą transportu podczas wywoływania `start` metody.</span><span class="sxs-lookup"><span data-stu-id="a8918-256">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="a8918-257">**Kod klienta, który określa metodę transportu (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-257">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="a8918-258">**Kod klienta, który określa metodę transportu (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-258">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="a8918-259">Alternatywnie można określić wielu metod transportu, w kolejności, w której chcesz SignalR, aby wypróbować je:</span><span class="sxs-lookup"><span data-stu-id="a8918-259">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="a8918-260">**Kod klienta, który określa rezerwowy schematu transport w niestandardowych (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-260">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="a8918-261">**Kod klienta, który określa rezerwowy schemat transportu niestandardowe (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-261">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="a8918-262">Do określania metodą transportu, można użyć następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="a8918-262">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="a8918-263">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="a8918-263">"webSockets"</span></span>
- <span data-ttu-id="a8918-264">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="a8918-264">"foreverFrame"</span></span>
- <span data-ttu-id="a8918-265">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="a8918-265">"serverSentEvents"</span></span>
- <span data-ttu-id="a8918-266">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="a8918-266">"longPolling"</span></span>

<span data-ttu-id="a8918-267">Poniższe przykłady pokazują, jak można dowiedzieć się, jakiej metody transportu jest używany przez połączenie.</span><span class="sxs-lookup"><span data-stu-id="a8918-267">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="a8918-268">**Kod klienta, który zawiera metodę transportu, używane przez połączenia (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-268">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="a8918-269">**Kod klienta, który zawiera metodę transportu, używany przez połączenie (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-269">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="a8918-270">Aby uzyskać informacje o sposobie Sprawdź metodą transportu w kodzie serwera, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - sposób uzyskiwania informacji na temat klienta z właściwości kontekstu](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="a8918-270">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="a8918-271">Aby uzyskać więcej informacji dotyczących transportu i planów awaryjnych, zobacz [wprowadzenie do SignalR - transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="a8918-271">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="a8918-272">Jak uzyskać serwera proxy dla klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="a8918-272">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="a8918-273">Każdy obiekt połączenia, którą tworzysz hermetyzuje informacje dotyczące połączenia SignalR service, który zawiera co najmniej jedną klasę koncentratora.</span><span class="sxs-lookup"><span data-stu-id="a8918-273">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="a8918-274">Aby komunikować się z klasy koncentratora, należy użyć obiektu serwera proxy utworzonych samodzielnie (Jeśli nie używasz wygenerowany serwer proxy) lub która zostanie wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="a8918-274">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="a8918-275">Na komputerze klienckim nazwę serwera proxy jest wersja formacie camelcase nazwy klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="a8918-275">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="a8918-276">SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można jest zgodna z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a8918-276">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="a8918-277">**Klasy koncentratora na serwerze**</span><span class="sxs-lookup"><span data-stu-id="a8918-277">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="a8918-278">**Pobierz odwołanie do serwera proxy wygenerowanego klienta dla koncentratora**</span><span class="sxs-lookup"><span data-stu-id="a8918-278">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="a8918-279">**Utwórz serwer proxy klienta dla klasy koncentratora (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-279">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="a8918-280">Jeśli dekoracji klasy koncentratora z `HubName` atrybutu, należy użyć dokładnej nazwy bez wprowadzania zmian w przypadku.</span><span class="sxs-lookup"><span data-stu-id="a8918-280">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="a8918-281">**Klasy koncentratora na serwerze z atrybutem HubName**</span><span class="sxs-lookup"><span data-stu-id="a8918-281">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="a8918-282">**Pobierz odwołanie do serwera proxy wygenerowanego klienta dla koncentratora**</span><span class="sxs-lookup"><span data-stu-id="a8918-282">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="a8918-283">**Utwórz serwer proxy klienta dla klasy koncentratora (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-283">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="a8918-284">Sposób definiowania metody na kliencie, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="a8918-284">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="a8918-285">Aby zdefiniować metodę, która serwera można wywołać z koncentratorem, Dodaj program obsługi zdarzeń do serwera proxy koncentratora za pomocą `client` właściwość wygenerowany serwer proxy lub wywołanie `on` metody, jeśli nie używasz wygenerowany serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="a8918-285">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="a8918-286">Parametry mogą być złożone obiekty.</span><span class="sxs-lookup"><span data-stu-id="a8918-286">The parameters can be complex objects.</span></span>

<span data-ttu-id="a8918-287">Dodawanie obsługi zdarzeń, przed wywołaniem `start` metodę, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="a8918-287">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="a8918-288">(Jeśli chcesz dodać procedury obsługi zdarzeń po wywołaniu `start` metody, zobacz uwagi w [jak nawiązać połączenie](#establishconnection) we wcześniejszej części tego dokumentu, a następnie użyj składni przedstawionej w celu zdefiniowania metody bez użycia wygenerowany serwer proxy.)</span><span class="sxs-lookup"><span data-stu-id="a8918-288">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="a8918-289">Dopasowywaniu nazwy metody nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="a8918-289">Method name matching is case-insensitive.</span></span> <span data-ttu-id="a8918-290">Na przykład `Clients.All.addContosoChatMessageToPage` będą wykonywane na serwerze `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, lub `addcontosochatmessagetopage` na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="a8918-290">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="a8918-291">**Zdefiniuj metodę na komputerze klienckim (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-291">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="a8918-292">**Alternatywny sposób definiowania metody na komputerze klienckim (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-292">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="a8918-293">**Zdefiniuj metodę na kliencie (bez wygenerowany serwer proxy lub podczas dodawania po wywołaniu metody start)**</span><span class="sxs-lookup"><span data-stu-id="a8918-293">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="a8918-294">**Kod serwera, który wywołuje metodę klienta**</span><span class="sxs-lookup"><span data-stu-id="a8918-294">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="a8918-295">Poniższe przykłady obiektu złożonego jako parametr metody.</span><span class="sxs-lookup"><span data-stu-id="a8918-295">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="a8918-296">**Zdefiniuj metodę na komputerze klienckim, który przyjmuje obiekt złożony (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-296">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="a8918-297">**Zdefiniuj metodę na komputerze klienckim, który przyjmuje obiekt złożony (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-297">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="a8918-298">**Kod serwera, który definiuje obiekt złożony**</span><span class="sxs-lookup"><span data-stu-id="a8918-298">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="a8918-299">**Kod serwera, który wywołuje metodę klienta przy użyciu obiektu złożonego**</span><span class="sxs-lookup"><span data-stu-id="a8918-299">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="a8918-300">Jak wywołać metody serwera z poziomu klienta</span><span class="sxs-lookup"><span data-stu-id="a8918-300">How to call server methods from the client</span></span>

<span data-ttu-id="a8918-301">Aby wywołać metody serwera od klienta, należy użyć `server` właściwość wygenerowany serwer proxy lub `invoke` metody serwera proxy koncentratora, jeśli nie używasz wygenerowany serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="a8918-301">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="a8918-302">Wartości zwracane lub parametry mogą być złożone obiekty.</span><span class="sxs-lookup"><span data-stu-id="a8918-302">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="a8918-303">Przekaż camelcase wersję nazwę metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="a8918-303">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="a8918-304">SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można jest zgodna z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a8918-304">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="a8918-305">W poniższych przykładach pokazano, jak wywoływać metody serwera, który nie ma wartości zwracanej i sposób wywoływania metody serwera, który ma wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="a8918-305">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="a8918-306">**Metoda serwera nie atrybutem HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="a8918-306">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="a8918-307">**Kod serwera, który definiuje obiektu złożonego przekazanego do parametru**</span><span class="sxs-lookup"><span data-stu-id="a8918-307">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="a8918-308">**Kod klienta, który wywołuje metodę serwera (za pomocą wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-308">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="a8918-309">**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-309">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="a8918-310">Jeśli dekorowane metody koncentratora, za pomocą `HubMethodName` atrybutu, należy użyć tej nazwy bez wprowadzania zmian w przypadku.</span><span class="sxs-lookup"><span data-stu-id="a8918-310">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="a8918-311">**Metoda serwera** atrybutem HubMethodName</span><span class="sxs-lookup"><span data-stu-id="a8918-311">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="a8918-312">**Kod klienta, który wywołuje metodę serwera (za pomocą wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-312">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="a8918-313">**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-313">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="a8918-314">Sposób wywoływania metody serwera, która nie zwraca żadnej wartości można znaleźć w poprzednich przykładach.</span><span class="sxs-lookup"><span data-stu-id="a8918-314">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="a8918-315">Następujące przykłady przedstawiają sposób wywoływania metody serwera, która nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="a8918-315">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="a8918-316">**Kod serwera dla metody, która nie zwraca wartości**</span><span class="sxs-lookup"><span data-stu-id="a8918-316">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="a8918-317">**Klasy zapasów, używany do** zwracają wartość</span><span class="sxs-lookup"><span data-stu-id="a8918-317">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="a8918-318">**Kod klienta, który wywołuje metodę serwera (za pomocą wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-318">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="a8918-319">**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-319">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="a8918-320">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="a8918-320">How to handle connection lifetime events</span></span>

<span data-ttu-id="a8918-321">Biblioteka SignalR udostępnia następujące połączenia zdarzenia okresu istnienia, które może obsłużyć:</span><span class="sxs-lookup"><span data-stu-id="a8918-321">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="a8918-322">`starting`: Wywoływane przed wszystkie dane są wysyłane za pośrednictwem połączenia.</span><span class="sxs-lookup"><span data-stu-id="a8918-322">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="a8918-323">`received`: Zgłaszane w przypadku nieodebrania żadnych danych w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="a8918-323">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="a8918-324">Udostępnia odebrane dane.</span><span class="sxs-lookup"><span data-stu-id="a8918-324">Provides the received data.</span></span>
- <span data-ttu-id="a8918-325">`connectionSlow`: Wywoływane, gdy klient wykrywa wolno lub często porzucanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="a8918-325">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="a8918-326">`reconnecting`: Wywoływane, gdy transportu źródłowego rozpoczyna ponowne nawiązywanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="a8918-326">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="a8918-327">`reconnected`: Wywoływane, gdy nawiązał transportu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="a8918-327">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="a8918-328">`stateChanged`: Wywoływane, gdy zmieni się stan połączenia.</span><span class="sxs-lookup"><span data-stu-id="a8918-328">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="a8918-329">Zawiera stan stary i nowy stan (łączenie, połączony, ponowne nawiązywanie połączenia i Rozłączono).</span><span class="sxs-lookup"><span data-stu-id="a8918-329">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="a8918-330">`disconnected`: Wywoływane, gdy połączenie zostało rozłączone.</span><span class="sxs-lookup"><span data-stu-id="a8918-330">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="a8918-331">Na przykład, jeśli chcesz wyświetlić komunikaty ostrzegawcze, gdy występują problemy z połączeniem, które mogłyby spowodować zauważalnego opóźnienia, obsługi `connectionSlow` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="a8918-331">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="a8918-332">**Obsługuj zdarzenie connectionSlow (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-332">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="a8918-333">**Obsługuj zdarzenie connectionSlow (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-333">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="a8918-334">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia w SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="a8918-334">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="a8918-335">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="a8918-335">How to handle errors</span></span>

<span data-ttu-id="a8918-336">Udostępnia klienta SignalR JavaScript `error` zdarzenia, które można dodać program obsługi.</span><span class="sxs-lookup"><span data-stu-id="a8918-336">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="a8918-337">Metoda niepowodzenie umożliwia również dodawanie obsługi błędów, które wynikają z wywołania metody serwera.</span><span class="sxs-lookup"><span data-stu-id="a8918-337">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="a8918-338">Jeśli nie włączysz jawnie szczegółowe komunikaty o błędach na serwerze, obiekt wyjątku, który zwraca SignalR, po wystąpieniu błędu zawiera minimalne informacje o tym błędzie.</span><span class="sxs-lookup"><span data-stu-id="a8918-338">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="a8918-339">Na przykład, jeśli wywołanie `newContosoChatMessage` zawiera komunikat o błędzie w obiekcie błąd zakończy się niepowodzeniem, "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Wysyłanie szczegółowe komunikaty o błędach do klientów w środowisku produkcyjnym jest niezalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach dla rozwiązywania problemów, użyj poniższego kodu, na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a8918-339">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="a8918-340">Poniższy przykład pokazuje, jak dodać program obsługi zdarzenia błędu.</span><span class="sxs-lookup"><span data-stu-id="a8918-340">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="a8918-341">**Dodaj procedurę obsługi błędów (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-341">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="a8918-342">**Dodaj procedurę obsługi błędów (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-342">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="a8918-343">Poniższy przykład pokazuje, jak obsłużyć błąd z wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="a8918-343">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="a8918-344">**Obsłużyć błąd w wywołaniu metody (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-344">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="a8918-345">**Obsłużyć błąd w wywołaniu metody (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-345">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="a8918-346">W przypadku niepowodzenia wywołania metody `error` zdarzenie jest również zgłaszane, więc swój kod w `error` obsługę metody i w `.fail` metody wywołania zwrotnego jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="a8918-346">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="a8918-347">Włączanie rejestrowania po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="a8918-347">How to enable client-side logging</span></span>

<span data-ttu-id="a8918-348">Aby włączyć połączenia logowania po stronie klienta, należy ustawić `logging` właściwości w obiekcie połączenia przed wywołaniem `start` metodę, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="a8918-348">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="a8918-349">**Włączanie rejestrowania (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-349">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="a8918-350">**Włączanie rejestrowania (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="a8918-350">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="a8918-351">Aby wyświetlić dzienniki, Otwórz narzędzia deweloperskie przeglądarki i przejdź do karty konsoli. Aby uzyskać samouczek, który zawiera instrukcje krok po kroku i ekran zrzuty, które pokazują, jak to zrobić, zobacz [emisje serwera z użyciem ASP.NET Signalr - Włącz rejestrowanie](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span><span class="sxs-lookup"><span data-stu-id="a8918-351">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span></span>
