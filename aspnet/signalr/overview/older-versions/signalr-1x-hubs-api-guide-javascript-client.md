---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: Podręcznik interfejsu API centrów SignalR 1.x — klient JavaScript | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 1.1 w klientów języka JavaScript, takie jak przeglądarki i Windows Store (WinJS) applic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 3467d7e76180017e811e52c35325f45b3a124ca2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363432"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="8690e-103">Podręcznik interfejsu API centrów SignalR 1.x — klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="8690e-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="8690e-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8690e-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="8690e-105">Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 1.1 w klientów języka JavaScript, takie jak przeglądarki i aplikacje Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="8690e-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="8690e-106">Interfejsu API centrów SignalR umożliwia zdalne wywołania procedur (RPC) z serwera do połączonych klientów i od klientów z serwerem.</span><span class="sxs-lookup"><span data-stu-id="8690e-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="8690e-107">W kodzie serwera należy zdefiniować metody, które mogą być wywoływane przez klientów, a wywołanie metody, które są uruchamiane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="8690e-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="8690e-108">W kodzie klienta definiowania metod, które mogą być wywoływane z serwera, a wywołanie metody, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8690e-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="8690e-109">SignalR zajmuje się wszystkie nadmiar klient serwer dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="8690e-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="8690e-110">SignalR oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="8690e-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="8690e-111">Wprowadzenie do SignalR, centra i połączenia trwałego lub samouczek, w którym przedstawiono sposób tworzenia kompletnej aplikacji SignalR, patrz [SignalR — wprowadzenie](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="8690e-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="8690e-112">Omówienie</span><span class="sxs-lookup"><span data-stu-id="8690e-112">Overview</span></span>

<span data-ttu-id="8690e-113">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="8690e-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="8690e-114">Wygenerowany serwer proxy i przeznaczenie dla Ciebie</span><span class="sxs-lookup"><span data-stu-id="8690e-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="8690e-115">Kiedy należy używać wygenerowany serwer proxy</span><span class="sxs-lookup"><span data-stu-id="8690e-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="8690e-116">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="8690e-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="8690e-117">Jak odwoływać się dynamicznie generowanym serwera proxy</span><span class="sxs-lookup"><span data-stu-id="8690e-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="8690e-118">Jak utworzyć plik fizyczny dla elementu SignalR wygenerowany serwer proxy</span><span class="sxs-lookup"><span data-stu-id="8690e-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="8690e-119">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="8690e-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="8690e-120">$. connection.hub jest taki sam obiekt, tworzy tego $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="8690e-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="8690e-121">Wykonanie asynchroniczne metody start</span><span class="sxs-lookup"><span data-stu-id="8690e-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="8690e-122">Jak nawiązać połączenie między domenami</span><span class="sxs-lookup"><span data-stu-id="8690e-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="8690e-123">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="8690e-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="8690e-124">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="8690e-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="8690e-125">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="8690e-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="8690e-126">Jak uzyskać serwera proxy dla klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="8690e-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="8690e-127">Sposób definiowania metody na kliencie, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="8690e-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="8690e-128">Jak wywołać metody serwera z poziomu klienta</span><span class="sxs-lookup"><span data-stu-id="8690e-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="8690e-129">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="8690e-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="8690e-130">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="8690e-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="8690e-131">Włączanie rejestrowania po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="8690e-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="8690e-132">Dokumentacja na temat programu server lub klientów programu .NET, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="8690e-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="8690e-133">Podręcznik interfejsu API centrów SignalR — serwer</span><span class="sxs-lookup"><span data-stu-id="8690e-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="8690e-134">Podręcznik interfejsu API centrów SignalR — klient modelu .NET</span><span class="sxs-lookup"><span data-stu-id="8690e-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="8690e-135">Łącza do tematów, dokumentacja interfejsu API są .NET 4.5 wersję interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="8690e-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="8690e-136">Jeśli używasz programu .NET 4, zobacz [tematy interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="8690e-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="8690e-137">Wygenerowany serwer proxy i przeznaczenie dla Ciebie</span><span class="sxs-lookup"><span data-stu-id="8690e-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="8690e-138">Można programować klienta JavaScript do komunikowania się z usługą SignalR, z lub bez serwera proxy, który generuje SignalR.</span><span class="sxs-lookup"><span data-stu-id="8690e-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="8690e-139">Serwer proxy jest dla Ciebie to uprościć jego składnię kodu używana do łączenia metod zapisywania, które wymaga serwera, i wywoływać metody na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8690e-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="8690e-140">Podczas pisania kodu w celu wywołania metody serwera wygenerowany serwer proxy umożliwia użycie składni, która wygląda tak, jakby były wykonywania funkcji lokalnej: możesz napisać `serverMethod(arg1, arg2)` zamiast `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="8690e-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="8690e-141">Składnia wygenerowany serwer proxy umożliwia również natychmiastowe i zrozumiałym błąd po stronie klienta, jeśli wpisujesz nazwę metody serwera.</span><span class="sxs-lookup"><span data-stu-id="8690e-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="8690e-142">A jeśli ręcznie utworzyć plik, który definiuje serwery proxy, możesz również uzyskać obsługę technologii IntelliSense do pisania kodu, który wywołuje metody serwera.</span><span class="sxs-lookup"><span data-stu-id="8690e-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="8690e-143">Na przykład załóżmy, że masz następujące klasy koncentratora na serwerze:</span><span class="sxs-lookup"><span data-stu-id="8690e-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="8690e-144">W poniższych przykładach kodu pokazano, co JavaScript kod wygląda do wywoływania `NewContosoChatMessage` metody na serwerze i odbieranie wywołań `addContosoChatMessageToPage` metody z serwera.</span><span class="sxs-lookup"><span data-stu-id="8690e-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="8690e-145">**Za pomocą wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="8690e-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="8690e-146">**Bez wygenerowany serwer proxy**</span><span class="sxs-lookup"><span data-stu-id="8690e-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="8690e-147">Kiedy należy używać wygenerowany serwer proxy</span><span class="sxs-lookup"><span data-stu-id="8690e-147">When to use the generated proxy</span></span>

<span data-ttu-id="8690e-148">Jeśli chcesz zarejestrować wiele procedur obsługi zdarzeń dla metod klienta, który wywołuje serwer, nie możesz użyć wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="8690e-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="8690e-149">W przeciwnym razie można używać wygenerowany serwer proxy lub nie na podstawie kodowania preferencji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8690e-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="8690e-150">Jeśli nie chcesz go użyć, nie musisz odwoływać się do adresu URL "koncentratory/signalr" w `script` elementu w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="8690e-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="8690e-151">Instalacja klienta</span><span class="sxs-lookup"><span data-stu-id="8690e-151">Client setup</span></span>

<span data-ttu-id="8690e-152">Klient JavaScript wymaga odwołania do technologii jQuery i plik JavaScript core SignalR.</span><span class="sxs-lookup"><span data-stu-id="8690e-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="8690e-153">Wersja jQuery musi być 1.6.4 lub nowsze wersje główne, takich jak 1.7.2, 1.8.2 lub 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="8690e-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="8690e-154">Jeśli zdecydujesz się używać wygenerowany serwer proxy, należy również odwołanie do serwera proxy SignalR wygenerowany plik JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8690e-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="8690e-155">Poniższy przykład pokazuje, jak odwołania może wyglądać na stronie HTML, który używa wygenerowany serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="8690e-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="8690e-156">Te odwołania muszą być zawarte w następującej kolejności: jQuery imię i nazwisko core SignalR po tym i serwery proxy SignalR.</span><span class="sxs-lookup"><span data-stu-id="8690e-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="8690e-157">Jak odwoływać się dynamicznie generowanym serwera proxy</span><span class="sxs-lookup"><span data-stu-id="8690e-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="8690e-158">W powyższym przykładzie odwołanie do serwera proxy SignalR, generowany jest dynamicznie generowanego kodu JavaScript, nie do pliku fizycznego.</span><span class="sxs-lookup"><span data-stu-id="8690e-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="8690e-159">SignalR tworzy kod JavaScript dla serwera proxy na bieżąco i służy ona do klienta w odpowiedzi na adres URL "/ signalr/centra".</span><span class="sxs-lookup"><span data-stu-id="8690e-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="8690e-160">Jeśli określono inny podstawowy adres URL dla połączenia SignalR na serwerze w Twojej `MapHubs` metody, adres URL pliku dynamicznie generowanym serwera proxy jest adres URL niestandardowego za pomocą "/ koncentratory" dołączone do niego.</span><span class="sxs-lookup"><span data-stu-id="8690e-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="8690e-161">Dla klientów systemu Windows 8 (Windows Store) JavaScript należy użyć pliku fizycznego serwera proxy zamiast generowanych dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="8690e-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="8690e-162">Aby uzyskać więcej informacji, zobacz [sposobu tworzenia pliku fizycznego dla elementu SignalR wygenerowany serwer proxy](#manualproxy) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="8690e-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="8690e-163">W widoku Razor programu ASP.NET MVC 4 należy użyć tylda do odwoływania się do katalogu głównego aplikacji, w której można się odwołać plik serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="8690e-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="8690e-164">Aby uzyskać więcej informacji na temat korzystania z SignalR w MVC 4, zobacz [wprowadzenie do SignalR i MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="8690e-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="8690e-165">W widoku Razor programu ASP.NET MVC 3, należy użyć `Url.Content` usługi serwera proxy dla odwołania do pliku:</span><span class="sxs-lookup"><span data-stu-id="8690e-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="8690e-166">W aplikacji formularzy sieci Web ASP.NET, za pomocą `ResolveClientUrl` swoje serwery proxy odwołanie do pliku lub zarejestrować go za pośrednictwem Menedżera skryptów przy użyciu ścieżki względnej katalogu głównego aplikacji (rozpoczynający się tyldą):</span><span class="sxs-lookup"><span data-stu-id="8690e-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="8690e-167">Zgodnie z ogólną zasadą należy użyć tej samej metody dla określania adresu URL "/ signalr/centra", której użyjesz dla plików CSS i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8690e-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="8690e-168">Jeśli określisz adresu URL bez użycia tyldy w niektórych scenariuszach aplikacja będzie działać poprawnie podczas testowania w programie Visual Studio za pomocą usług IIS Express, ale zakończy się niepowodzeniem z powodu błędu 404, podczas wdrażania usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="8690e-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="8690e-169">Aby uzyskać więcej informacji, zobacz **rozpoznawania odwołań do zasobów na poziomie głównym** w [serwerów sieci Web w programie Visual Studio dla projektów sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="8690e-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="8690e-170">Po uruchomieniu projektu sieci web w programie Visual Studio 2012 w trybie debugowania, jeśli używasz programu Internet Explorer jako przeglądarki widać plik serwera proxy w **Eksploratora rozwiązań** w obszarze **dokumenty skryptów**, jak pokazano na na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="8690e-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Plik wygenerowany serwer proxy JavaScript w Eksploratorze rozwiązań](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="8690e-172">Aby wyświetlić zawartość pliku, kliknij dwukrotnie **koncentratory**.</span><span class="sxs-lookup"><span data-stu-id="8690e-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="8690e-173">Jeśli nie używasz programu Visual Studio 2012 i program Internet Explorer lub jeśli nie jesteś w trybie debugowania, możesz także uzyskać zawartość pliku, przechodząc do adresu URL "/ signalR/centra".</span><span class="sxs-lookup"><span data-stu-id="8690e-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="8690e-174">Na przykład, jeśli witryna jest hostowana na `http://localhost:56699`, przejdź do `http://localhost:56699/SignalR/hubs` w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="8690e-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="8690e-175">Jak utworzyć plik fizyczny dla elementu SignalR wygenerowany serwer proxy</span><span class="sxs-lookup"><span data-stu-id="8690e-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="8690e-176">Jako alternatywy dla dynamicznie generowanych serwera proxy można utworzyć pliku fizycznego, który ma kod serwera proxy i odwoływać się do tego pliku.</span><span class="sxs-lookup"><span data-stu-id="8690e-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="8690e-177">Można w tym kontrolę nad buforowania i tworzenia pakietów zachowanie lub korzystania z funkcji IntelliSense podczas kodowania wywołania metody serwera.</span><span class="sxs-lookup"><span data-stu-id="8690e-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="8690e-178">Aby utworzyć plik serwera proxy, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="8690e-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="8690e-179">Zainstaluj [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="8690e-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="8690e-180">Otwórz wiersz polecenia i przejdź do *narzędzia* folder zawierający plik SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="8690e-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="8690e-181">Folder narzędzia znajduje się w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="8690e-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="8690e-182">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8690e-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="8690e-183">Ścieżka do Twojej *.dll* jest zazwyczaj *bin* folderu w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="8690e-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="8690e-184">To polecenie tworzy plik o nazwie *server.js* w tym samym folderze co *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="8690e-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="8690e-185">Umieść *server.js* plików w folderze właściwe w projekcie, zmienić jego nazwę odpowiednio do aplikacji i Dodaj odwołanie do niego zamiast odwołania "koncentratory/signalr".</span><span class="sxs-lookup"><span data-stu-id="8690e-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="8690e-186">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="8690e-186">How to establish a connection</span></span>

<span data-ttu-id="8690e-187">Przed może nawiązać połączenie, należy utworzyć obiekt połączenia, utworzyć serwer proxy i zarejestruj procedury obsługi zdarzeń dla metod, które mogą być wywoływane z serwera.</span><span class="sxs-lookup"><span data-stu-id="8690e-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="8690e-188">Po skonfigurowaniu serwera proxy i programów obsługi nawiązania połączenia przez wywołanie metody `start` metody.</span><span class="sxs-lookup"><span data-stu-id="8690e-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="8690e-189">Jeśli używasz wygenerowany serwer proxy, nie trzeba utworzyć obiekt połączenia we własnym kodzie, ponieważ kod wygenerowany serwer proxy zrobi to za Ciebie.</span><span class="sxs-lookup"><span data-stu-id="8690e-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="8690e-190">**Nawiąż połączenie (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="8690e-191">**Nawiąż połączenie (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="8690e-192">Przykładowy kod używa domyślnej "/ signalr" adres URL, aby nawiązać połączenie z usługą SignalR.</span><span class="sxs-lookup"><span data-stu-id="8690e-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="8690e-193">Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="8690e-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="8690e-194">Zwykle rejestrowanie procedur obsługi zdarzeń przed wywołaniem `start` metodę, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="8690e-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="8690e-195">Jeśli chcesz zarejestrować niektóre procedury obsługi zdarzeń po ustanowieniu połączenia, możesz to zrobić, ale musisz się zarejestrować, co najmniej jeden z Twojej handler(s) zdarzeń przed wywołaniem `start` metody.</span><span class="sxs-lookup"><span data-stu-id="8690e-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="8690e-196">Jeden przyczyną jest to, że może istnieć wiele centrów w aplikacji, ale nie chcesz wyzwolić `OnConnected` zdarzenia dla każdego Centrum, jeśli tylko będą korzystać z jednym z nich.</span><span class="sxs-lookup"><span data-stu-id="8690e-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="8690e-197">Po nawiązaniu połączenia jest obecność metodę klienta dla serwera proxy koncentratora, co informuje SignalR do wyzwolenia `OnConnected` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="8690e-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="8690e-198">Jeżeli nie zarejestrujesz dowolnej procedury obsługi zdarzeń przed wywołaniem `start` metody, można do wywołania metody koncentratora, ale Centrum `OnConnected` nie można wywołać metody i żadnych metod klienta zostanie wywołana z serwera.</span><span class="sxs-lookup"><span data-stu-id="8690e-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="8690e-199">$. connection.hub jest taki sam obiekt, tworzy tego $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="8690e-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="8690e-200">Jak widać w przykładach, gdy używasz wygenerowany serwer proxy `$.connection.hub` odwołuje się do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="8690e-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="8690e-201">Jest to ten sam obiekt, który można uzyskać przez wywołanie metody `$.hubConnection()` gdy nie używasz wygenerowany serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="8690e-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="8690e-202">Kod wygenerowany serwer proxy tworzy połączenie, wykonując następującą instrukcję:</span><span class="sxs-lookup"><span data-stu-id="8690e-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Tworzenie połączenia w pliku wygenerowanego serwera proxy](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="8690e-204">Wygenerowany serwer proxy jest używany, można wykonać z `$.connection.hub` , można wykonać za pomocą obiektu połączenia, gdy nie używasz wygenerowany serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="8690e-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="8690e-205">Wykonanie asynchroniczne metody start</span><span class="sxs-lookup"><span data-stu-id="8690e-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="8690e-206">`start` Metoda jest wykonywana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="8690e-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="8690e-207">Zwraca [obiektu opóźnionych jQuery](http://api.jquery.com/category/deferred-object/), co oznacza, że można dodać funkcji wywołania zwrotnego wywoływania metod, takich jak `pipe`, `done`, i `fail`.</span><span class="sxs-lookup"><span data-stu-id="8690e-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="8690e-208">Jeśli masz kod, który ma zostać wykonany po nawiązaniu połączenia, takie jak wywołanie metody serwera, umieść ten kod w funkcji wywołania zwrotnego lub wywołać go z funkcji wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="8690e-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="8690e-209">`.done` Metody wywołania zwrotnego jest wykonywane po ustanowieniu połączenia, a po dowolny kod, który ma swojej `OnConnected` zakończy się wykonywanie metody obsługi zdarzeń na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8690e-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="8690e-210">Jeśli umieścisz instrukcję "Teraz connected", z poprzedniego przykładu jako następnego wiersza kodu po `start` wywołania metody (nie w `.done` wywołania zwrotnego), `console.log` wiersza zostaną wykonane, zanim połączenie zostanie nawiązane, jak pokazano w następującym przykład:</span><span class="sxs-lookup"><span data-stu-id="8690e-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Problem ze sposobem pisania kodu, który jest uruchamiany po nawiązaniu połączenia](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="8690e-212">Jak nawiązać połączenie między domenami</span><span class="sxs-lookup"><span data-stu-id="8690e-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="8690e-213">Zazwyczaj, gdy przeglądarka ładuje stronę z `http://contoso.com`, połączenia SignalR jest w tej samej domenie, w `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="8690e-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="8690e-214">Jeśli strona z `http://contoso.com` nawiązuje połączenie `http://fabrikam.com/signalr`, oznacza to połączenie między domenami.</span><span class="sxs-lookup"><span data-stu-id="8690e-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="8690e-215">Ze względów bezpieczeństwa połączeń między domenami są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="8690e-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="8690e-216">Aby nawiązać połączenie między domenami, upewnij się, że połączenia między domenami są włączone na serwerze, a adres URL połączenia można określić podczas tworzenia obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="8690e-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="8690e-217">SignalR użyje odpowiedniej technologii dla połączeń między domenami, takich jak [JSONP](http://en.wikipedia.org/wiki/JSONP) lub [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="8690e-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="8690e-218">Na serwerze, należy włączyć połączenia między domenami, wybierając tę opcję, gdy wywołujesz `MapHubs` metody.</span><span class="sxs-lookup"><span data-stu-id="8690e-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="8690e-219">Na komputerze klienckim należy określić adres URL, podczas tworzenia obiektu połączenia (bez wygenerowany serwer proxy), lub przed wywołaniem metody start (z wygenerowany serwer proxy).</span><span class="sxs-lookup"><span data-stu-id="8690e-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="8690e-220">**Kod klienta, który określa połączenie między domenami (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="8690e-221">**Kod klienta, który określa połączenie między domenami (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="8690e-222">Kiedy używasz `$.hubConnection` konstruktora, nie trzeba uwzględnić `signalr` w adresie URL, ponieważ jest automatycznie dodawany (chyba że określisz `useDefaultUrl` jako `false`).</span><span class="sxs-lookup"><span data-stu-id="8690e-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="8690e-223">Możesz utworzyć wiele połączeń do różnych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="8690e-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="8690e-224">Nie należy ustawiać `jQuery.support.cors` na wartość true w kodzie.</span><span class="sxs-lookup"><span data-stu-id="8690e-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![Nie jest ustawiana na wartość true jQuery.support.cors](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="8690e-226">SignalR obsługuje stosowanie JSONP lub mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="8690e-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="8690e-227">Ustawienie `jQuery.support.cors` do wartości true powoduje wyłączenie JSONP, ponieważ powoduje ona SignalR założył, przeglądarka obsługuje mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="8690e-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="8690e-228">Podczas łączenia z adresem URL localhost, programu Internet Explorer 10 nie należy wziąć pod uwagę jej połączenie między domenami, dzięki czemu aplikacja będzie działać lokalnie za pomocą programu Internet Explorer 10, nawet jeśli nie zostały włączone połączenia między domenami na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8690e-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="8690e-229">Aby dowiedzieć się, jak za pomocą połączeń między domenami, za pomocą programu Internet Explorer 9, zobacz [wątek w witrynie StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="8690e-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="8690e-230">Aby dowiedzieć się, jak za pomocą przeglądarki Chrome przy użyciu połączenia między domenami, zobacz [wątek w witrynie StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="8690e-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="8690e-231">Przykładowy kod używa domyślnej "/ signalr" adres URL, aby nawiązać połączenie z usługą SignalR.</span><span class="sxs-lookup"><span data-stu-id="8690e-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="8690e-232">Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="8690e-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="8690e-233">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="8690e-233">How to configure the connection</span></span>

<span data-ttu-id="8690e-234">Przed nawiązaniem połączenia, można określić parametry ciągu zapytania lub Określ metodę transportu.</span><span class="sxs-lookup"><span data-stu-id="8690e-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="8690e-235">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="8690e-235">How to specify query string parameters</span></span>

<span data-ttu-id="8690e-236">Jeśli chcesz wysyłać dane do serwera, gdy klient nawiąże połączenie, można dodać parametry ciągu zapytania do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="8690e-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="8690e-237">Poniższe przykłady pokazują, jak można ustawić jako parametr ciągu zapytania w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="8690e-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="8690e-238">**Ustaw wartość ciągu zapytania przed wywołaniem metody start (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="8690e-239">**Ustaw wartość ciągu zapytania przed wywołaniem metody start (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="8690e-240">Poniższy przykład pokazuje, jak odczytywać parametr ciągu zapytania w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="8690e-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="8690e-241">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="8690e-241">How to specify the transport method</span></span>

<span data-ttu-id="8690e-242">Częścią procesu nawiązywania połączenia klienta SignalR zwykle negocjuje z serwerem, aby określić najlepsze transportu, która jest obsługiwana przez serwer i klienta.</span><span class="sxs-lookup"><span data-stu-id="8690e-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="8690e-243">Jeśli znasz już transport, którego chcesz użyć, można pominąć ten proces negocjacji, określając metodą transportu podczas wywoływania `start` metody.</span><span class="sxs-lookup"><span data-stu-id="8690e-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="8690e-244">**Kod klienta, który określa metodę transportu (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="8690e-245">**Kod klienta, który określa metodę transportu (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="8690e-246">Alternatywnie można określić wielu metod transportu, w kolejności, w której chcesz SignalR, aby wypróbować je:</span><span class="sxs-lookup"><span data-stu-id="8690e-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="8690e-247">**Kod klienta, który określa rezerwowy schematu transport w niestandardowych (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="8690e-248">**Kod klienta, który określa rezerwowy schemat transportu niestandardowe (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="8690e-249">Do określania metodą transportu, można użyć następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="8690e-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="8690e-250">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="8690e-250">"webSockets"</span></span>
- <span data-ttu-id="8690e-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="8690e-251">"foreverFrame"</span></span>
- <span data-ttu-id="8690e-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="8690e-252">"serverSentEvents"</span></span>
- <span data-ttu-id="8690e-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="8690e-253">"longPolling"</span></span>

<span data-ttu-id="8690e-254">Poniższe przykłady pokazują, jak można dowiedzieć się, jakiej metody transportu jest używany przez połączenie.</span><span class="sxs-lookup"><span data-stu-id="8690e-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="8690e-255">**Kod klienta, który zawiera metodę transportu, używane przez połączenia (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="8690e-256">**Kod klienta, który zawiera metodę transportu, używany przez połączenie (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="8690e-257">Aby uzyskać informacje o sposobie Sprawdź metodą transportu w kodzie serwera, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - sposób uzyskiwania informacji na temat klienta z właściwości kontekstu](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="8690e-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="8690e-258">Aby uzyskać więcej informacji dotyczących transportu i planów awaryjnych, zobacz [wprowadzenie do SignalR - transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="8690e-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="8690e-259">Jak uzyskać serwera proxy dla klasy koncentratora</span><span class="sxs-lookup"><span data-stu-id="8690e-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="8690e-260">Każdy obiekt połączenia, którą tworzysz hermetyzuje informacje dotyczące połączenia SignalR service, który zawiera co najmniej jedną klasę koncentratora.</span><span class="sxs-lookup"><span data-stu-id="8690e-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="8690e-261">Aby komunikować się z klasy koncentratora, należy użyć obiektu serwera proxy utworzonych samodzielnie (Jeśli nie używasz wygenerowany serwer proxy) lub która zostanie wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="8690e-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="8690e-262">Na komputerze klienckim nazwę serwera proxy jest wersja formacie camelcase nazwy klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="8690e-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="8690e-263">SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można jest zgodna z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8690e-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="8690e-264">**Klasy koncentratora na serwerze**</span><span class="sxs-lookup"><span data-stu-id="8690e-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="8690e-265">**Pobierz odwołanie do serwera proxy wygenerowanego klienta dla koncentratora**</span><span class="sxs-lookup"><span data-stu-id="8690e-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="8690e-266">**Utwórz serwer proxy klienta dla klasy koncentratora (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="8690e-267">Jeśli dekoracji klasy koncentratora z `HubName` atrybutu, należy użyć dokładnej nazwy bez wprowadzania zmian w przypadku.</span><span class="sxs-lookup"><span data-stu-id="8690e-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="8690e-268">**Klasy koncentratora na serwerze z atrybutem HubName**</span><span class="sxs-lookup"><span data-stu-id="8690e-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="8690e-269">**Pobierz odwołanie do serwera proxy wygenerowanego klienta dla koncentratora**</span><span class="sxs-lookup"><span data-stu-id="8690e-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="8690e-270">**Utwórz serwer proxy klienta dla klasy koncentratora (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="8690e-271">Sposób definiowania metody na kliencie, który można wywołać serwera</span><span class="sxs-lookup"><span data-stu-id="8690e-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="8690e-272">Aby zdefiniować metodę, która serwera można wywołać z koncentratorem, Dodaj program obsługi zdarzeń do serwera proxy koncentratora za pomocą `client` właściwość wygenerowany serwer proxy lub wywołanie `on` metody, jeśli nie używasz wygenerowany serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="8690e-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="8690e-273">Parametry mogą być złożone obiekty.</span><span class="sxs-lookup"><span data-stu-id="8690e-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="8690e-274">Dodawanie obsługi zdarzeń, przed wywołaniem `start` metodę, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="8690e-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="8690e-275">(Jeśli chcesz dodać procedury obsługi zdarzeń po wywołaniu `start` metody, zobacz uwagi w [jak nawiązać połączenie](#establishconnection) we wcześniejszej części tego dokumentu, a następnie użyj składni przedstawionej w celu zdefiniowania metody bez użycia wygenerowany serwer proxy.)</span><span class="sxs-lookup"><span data-stu-id="8690e-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="8690e-276">Dopasowywaniu nazwy metody nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="8690e-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="8690e-277">Na przykład `Clients.All.addContosoChatMessageToPage` będą wykonywane na serwerze `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, lub `addcontosochatmessagetopage` na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="8690e-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="8690e-278">**Zdefiniuj metodę na komputerze klienckim (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="8690e-279">**Alternatywny sposób definiowania metody na komputerze klienckim (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="8690e-280">**Zdefiniuj metodę na kliencie (bez wygenerowany serwer proxy lub podczas dodawania po wywołaniu metody start)**</span><span class="sxs-lookup"><span data-stu-id="8690e-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="8690e-281">**Kod serwera, który wywołuje metodę klienta**</span><span class="sxs-lookup"><span data-stu-id="8690e-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="8690e-282">Poniższe przykłady obiektu złożonego jako parametr metody.</span><span class="sxs-lookup"><span data-stu-id="8690e-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="8690e-283">**Zdefiniuj metodę na komputerze klienckim, który przyjmuje obiekt złożony (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="8690e-284">**Zdefiniuj metodę na komputerze klienckim, który przyjmuje obiekt złożony (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="8690e-285">**Kod serwera, który definiuje obiekt złożony**</span><span class="sxs-lookup"><span data-stu-id="8690e-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="8690e-286">**Kod serwera, który wywołuje metodę klienta przy użyciu obiektu złożonego**</span><span class="sxs-lookup"><span data-stu-id="8690e-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="8690e-287">Jak wywołać metody serwera z poziomu klienta</span><span class="sxs-lookup"><span data-stu-id="8690e-287">How to call server methods from the client</span></span>

<span data-ttu-id="8690e-288">Aby wywołać metody serwera od klienta, należy użyć `server` właściwość wygenerowany serwer proxy lub `invoke` metody serwera proxy koncentratora, jeśli nie używasz wygenerowany serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="8690e-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="8690e-289">Wartości zwracane lub parametry mogą być złożone obiekty.</span><span class="sxs-lookup"><span data-stu-id="8690e-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="8690e-290">Przekaż camelcase wersję nazwę metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="8690e-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="8690e-291">SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można jest zgodna z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8690e-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="8690e-292">W poniższych przykładach pokazano, jak wywoływać metody serwera, który nie ma wartości zwracanej i sposób wywoływania metody serwera, który ma wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="8690e-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="8690e-293">**Metoda serwera nie atrybutem HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="8690e-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="8690e-294">**Kod serwera, który definiuje obiektu złożonego przekazanego do parametru**</span><span class="sxs-lookup"><span data-stu-id="8690e-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="8690e-295">**Kod klienta, który wywołuje metodę serwera (za pomocą wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="8690e-296">**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="8690e-297">Jeśli dekorowane metody koncentratora, za pomocą `HubMethodName` atrybutu, należy użyć tej nazwy bez wprowadzania zmian w przypadku.</span><span class="sxs-lookup"><span data-stu-id="8690e-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="8690e-298">**Metoda serwera** atrybutem HubMethodName</span><span class="sxs-lookup"><span data-stu-id="8690e-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="8690e-299">**Kod klienta, który wywołuje metodę serwera (za pomocą wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="8690e-300">**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="8690e-301">Sposób wywoływania metody serwera, która nie zwraca żadnej wartości można znaleźć w poprzednich przykładach.</span><span class="sxs-lookup"><span data-stu-id="8690e-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="8690e-302">Następujące przykłady przedstawiają sposób wywoływania metody serwera, która nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="8690e-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="8690e-303">**Kod serwera dla metody, która nie zwraca wartości**</span><span class="sxs-lookup"><span data-stu-id="8690e-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="8690e-304">**Klasy zapasów, używany do** zwracają wartość</span><span class="sxs-lookup"><span data-stu-id="8690e-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="8690e-305">**Kod klienta, który wywołuje metodę serwera (za pomocą wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="8690e-306">**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="8690e-307">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="8690e-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="8690e-308">Biblioteka SignalR udostępnia następujące połączenia zdarzenia okresu istnienia, które może obsłużyć:</span><span class="sxs-lookup"><span data-stu-id="8690e-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="8690e-309">`starting`: Wywoływane, zanim wszystkie dane są wysyłane za pośrednictwem połączenia.</span><span class="sxs-lookup"><span data-stu-id="8690e-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="8690e-310">`received`: Zgłaszane w przypadku nieodebrania żadnych danych w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="8690e-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="8690e-311">Udostępnia odebrane dane.</span><span class="sxs-lookup"><span data-stu-id="8690e-311">Provides the received data.</span></span>
- <span data-ttu-id="8690e-312">`connectionSlow`: Wywoływane, gdy klient wykrywa wolno lub często porzucanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="8690e-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="8690e-313">`reconnecting`: Wywoływane, gdy transportu źródłowego rozpoczyna ponowne nawiązywanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="8690e-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="8690e-314">`reconnected`: Wywoływane, gdy nawiązał transportu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="8690e-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="8690e-315">`stateChanged`: Wywoływane, gdy zmieni się stan połączenia.</span><span class="sxs-lookup"><span data-stu-id="8690e-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="8690e-316">Zawiera stan stary i nowy stan (łączenie, połączony, ponowne nawiązywanie połączenia i Rozłączono).</span><span class="sxs-lookup"><span data-stu-id="8690e-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="8690e-317">`disconnected`: Wywoływane, gdy połączenie zostało rozłączone.</span><span class="sxs-lookup"><span data-stu-id="8690e-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="8690e-318">Na przykład, jeśli chcesz wyświetlić komunikaty ostrzegawcze, gdy występują problemy z połączeniem, które mogłyby spowodować zauważalnego opóźnienia, obsługi `connectionSlow` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="8690e-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="8690e-319">**Obsługuj zdarzenie connectionSlow (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="8690e-320">**Obsługuj zdarzenie connectionSlow (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="8690e-321">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia w SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="8690e-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="8690e-322">Sposób obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="8690e-322">How to handle errors</span></span>

<span data-ttu-id="8690e-323">Udostępnia klienta SignalR JavaScript `error` zdarzenia, które można dodać program obsługi.</span><span class="sxs-lookup"><span data-stu-id="8690e-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="8690e-324">Metoda niepowodzenie umożliwia również dodawanie obsługi błędów, które wynikają z wywołania metody serwera.</span><span class="sxs-lookup"><span data-stu-id="8690e-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="8690e-325">Jeśli nie włączysz jawnie szczegółowe komunikaty o błędach na serwerze, obiekt wyjątku, który zwraca SignalR, po wystąpieniu błędu zawiera minimalne informacje o tym błędzie.</span><span class="sxs-lookup"><span data-stu-id="8690e-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="8690e-326">Na przykład, jeśli wywołanie `newContosoChatMessage` zawiera komunikat o błędzie w obiekcie błąd zakończy się niepowodzeniem, "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Wysyłanie szczegółowe komunikaty o błędach do klientów w środowisku produkcyjnym jest niezalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach dla rozwiązywania problemów, użyj poniższego kodu, na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8690e-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="8690e-327">Poniższy przykład pokazuje, jak dodać program obsługi zdarzenia błędu.</span><span class="sxs-lookup"><span data-stu-id="8690e-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="8690e-328">**Dodaj procedurę obsługi błędów (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="8690e-329">**Dodaj procedurę obsługi błędów (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="8690e-330">Poniższy przykład pokazuje, jak obsłużyć błąd z wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="8690e-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="8690e-331">**Obsłużyć błąd w wywołaniu metody (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="8690e-332">**Obsłużyć błąd w wywołaniu metody (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="8690e-333">W przypadku niepowodzenia wywołania metody `error` zdarzenie jest również zgłaszane, więc swój kod w `error` obsługę metody i w `.fail` metody wywołania zwrotnego jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="8690e-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="8690e-334">Włączanie rejestrowania po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="8690e-334">How to enable client-side logging</span></span>

<span data-ttu-id="8690e-335">Aby włączyć połączenia logowania po stronie klienta, należy ustawić `logging` właściwości w obiekcie połączenia przed wywołaniem `start` metodę, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="8690e-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="8690e-336">**Włączanie rejestrowania (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="8690e-337">**Włączanie rejestrowania (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="8690e-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="8690e-338">Aby wyświetlić dzienniki, Otwórz narzędzia deweloperskie przeglądarki i przejdź do karty konsoli. Aby uzyskać samouczek, który zawiera instrukcje krok po kroku i ekran zrzuty, które pokazują, jak to zrobić, zobacz [emisje serwera z użyciem ASP.NET Signalr - Włącz rejestrowanie](index.md).</span><span class="sxs-lookup"><span data-stu-id="8690e-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
