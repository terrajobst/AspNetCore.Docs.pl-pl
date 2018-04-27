---
title: Wprowadzenie do platformy ASP.NET Core SignalR
author: rachelappel
description: Dowiedz się, jak biblioteka ASP.NET Core SignalR ułatwia dodawanie do aplikacji funkcji sieci web w czasie rzeczywistym.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: fa9b10201b5dc0e67bcd6d1321a3737e2025fda4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="4fef2-103">Wprowadzenie do platformy ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="4fef2-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="4fef2-104">Przez [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="4fef2-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>


[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-signalr"></a><span data-ttu-id="4fef2-105">Co to jest SignalR?</span><span class="sxs-lookup"><span data-stu-id="4fef2-105">What is SignalR?</span></span>

<span data-ttu-id="4fef2-106">SignalR platformy ASP.NET Core to biblioteki, która ułatwia dodawanie funkcji sieci web w czasie rzeczywistym do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4fef2-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="4fef2-107">Funkcje sieci web w czasie rzeczywistym umożliwia natychmiastowe kod po stronie serwera do wypychania zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="4fef2-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="4fef2-108">Nadaje SignalR:</span><span class="sxs-lookup"><span data-stu-id="4fef2-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="4fef2-109">Aplikacje, które wymagają wysokiej częstotliwości aktualizacji z serwera.</span><span class="sxs-lookup"><span data-stu-id="4fef2-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="4fef2-110">Przykłady są gier, sieci społecznościowych głosowania, aukcji, map i GPS aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4fef2-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="4fef2-111">Pulpity nawigacyjne i monitorowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4fef2-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="4fef2-112">Przykładem mogą być firmy pulpity nawigacyjne, błyskawiczne aktualizacji sprzedaży, lub przesyłane alerty.</span><span class="sxs-lookup"><span data-stu-id="4fef2-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="4fef2-113">Aplikacje współpracy.</span><span class="sxs-lookup"><span data-stu-id="4fef2-113">Collaborative apps.</span></span> <span data-ttu-id="4fef2-114">Tablica aplikacji i zespołu spotkania oprogramowania to przykłady współpracy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4fef2-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="4fef2-115">Aplikacje, które wymagają powiadomienia.</span><span class="sxs-lookup"><span data-stu-id="4fef2-115">Apps that require notifications.</span></span> <span data-ttu-id="4fef2-116">Sieci społecznościowych, poczty e-mail, rozmowy, gier, alerty podróży i wiele innych aplikacji użyj powiadomień.</span><span class="sxs-lookup"><span data-stu-id="4fef2-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="4fef2-117">Biblioteka SignalR udostępnia interfejs API do tworzenia klienta serwera [zdalnych wywołań procedur (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="4fef2-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="4fef2-118">RPC wywołują funkcje JavaScript na komputerach klienckich z kodu .NET Core po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="4fef2-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="4fef2-119">Biblioteka SignalR dla platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4fef2-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="4fef2-120">Obsługuje zarządzanie połączeniami automatycznie.</span><span class="sxs-lookup"><span data-stu-id="4fef2-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="4fef2-121">Umożliwia emituje wiadomości jednocześnie do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="4fef2-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="4fef2-122">Na przykład pokoju rozmów.</span><span class="sxs-lookup"><span data-stu-id="4fef2-122">For example, a chat room.</span></span>
* <span data-ttu-id="4fef2-123">Umożliwia wysyłanie komunikatów do określonych klientów lub grupy klientów.</span><span class="sxs-lookup"><span data-stu-id="4fef2-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="4fef2-124">Jest open-powierzając jej ich konserwację na [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="4fef2-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="4fef2-125">Skalowalne.</span><span class="sxs-lookup"><span data-stu-id="4fef2-125">Scalable.</span></span>

<span data-ttu-id="4fef2-126">Połączenie między klientem a serwerem jest trwała, w przeciwieństwie do połączenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="4fef2-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="4fef2-127">Transporty</span><span class="sxs-lookup"><span data-stu-id="4fef2-127">Transports</span></span>

<span data-ttu-id="4fef2-128">Abstract SignalR przez kilka technik tworzenia aplikacji sieci web czasu rzeczywistego.</span><span class="sxs-lookup"><span data-stu-id="4fef2-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="4fef2-129">[Protokół WebSockets](https://tools.ietf.org/html/rfc7118) jest optymalna transportu, ale innych technik, takich jak zdarzenia Server-Sent i długi sondowania można użyć w przypadku te nie są dostępne.</span><span class="sxs-lookup"><span data-stu-id="4fef2-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="4fef2-130">SignalR automatycznie wykryje i zainicjować odpowiednie transportu oparte na funkcji serwera i klienta są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4fef2-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs-and-endpoints"></a><span data-ttu-id="4fef2-131">Koncentratory i punkty końcowe</span><span class="sxs-lookup"><span data-stu-id="4fef2-131">Hubs and Endpoints</span></span>

<span data-ttu-id="4fef2-132">Biblioteka SignalR używa punktów końcowych i koncentratory do komunikacji między klientami a serwerami.</span><span class="sxs-lookup"><span data-stu-id="4fef2-132">SignalR uses Hubs and Endpoints to communicate between clients and servers.</span></span> <span data-ttu-id="4fef2-133">Interfejs API koncentratory obejmuje większości scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="4fef2-133">The Hubs API covers the most scenarios.</span></span>

<span data-ttu-id="4fef2-134">Koncentrator jest oparty na interfejs API punktu końcowego, który umożliwia klienta i serwera, wywoływanie metod od siebie wzajemnie potoku wysokiego poziomu.</span><span class="sxs-lookup"><span data-stu-id="4fef2-134">A hub is a high-level pipeline built upon the Endpoint API that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="4fef2-135">SignalR obsługuje wysyłki poza granicami maszyny automatycznie, co pozwala klientom wywoływać metod na serwerze jako łatwo jako metody lokalne i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="4fef2-135">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="4fef2-136">Koncentratory zezwala na przekazywanie jednoznacznie parametrów do metod, co pozwala wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="4fef2-136">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="4fef2-137">Biblioteka SignalR udostępnia dwa protokoły wbudowanych koncentratora: protokół tekst na podstawie JSON i protokół binarny na podstawie [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="4fef2-137">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="4fef2-138">MessagePack tworzy zazwyczaj wiadomości mniejszych niż przy użyciu formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="4fef2-138">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="4fef2-139">Starsze przeglądarki musi obsługiwać [XHR poziom 2](https://caniuse.com/#feat=xhr2) do obsługi protokołu MessagePack.</span><span class="sxs-lookup"><span data-stu-id="4fef2-139">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="4fef2-140">Koncentratory wywoływać kod po stronie klienta przez wysyłanie wiadomości przy użyciu aktywny transport.</span><span class="sxs-lookup"><span data-stu-id="4fef2-140">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="4fef2-141">Komunikaty zawierają nazwę i parametry metody po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4fef2-141">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="4fef2-142">Obiekty wysyłane jako parametry metody są deserializacji za pomocą protokołu skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="4fef2-142">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="4fef2-143">Klient próbuje jest zgodna z nazwą metody w kodzie po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4fef2-143">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="4fef2-144">W przypadku dopasowania w metodzie klienta jest wykonywane przy użyciu danych parametru zdeserializowany.</span><span class="sxs-lookup"><span data-stu-id="4fef2-144">When a match happens, the client method runs using the deserialized parameter data.</span></span>

<span data-ttu-id="4fef2-145">Punkty końcowe Podaj raw API przypominającej gniazda, włączanie ich do odczytu i zapisu z klienta.</span><span class="sxs-lookup"><span data-stu-id="4fef2-145">Endpoints provide a raw socket-like API, enabling them to read and write from the client.</span></span> <span data-ttu-id="4fef2-146">To deweloperom grupowania, emisji i inne funkcje.</span><span class="sxs-lookup"><span data-stu-id="4fef2-146">It's up to the developer to handle grouping, broadcasting, and other functions.</span></span> <span data-ttu-id="4fef2-147">Interfejs API koncentratory jest oparty na warstwie punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="4fef2-147">The Hubs API is built on top of the Endpoints layer.</span></span>

<span data-ttu-id="4fef2-148">Na poniższym diagramie przedstawiono relację między koncentratorów, punktów końcowych i klientów.</span><span class="sxs-lookup"><span data-stu-id="4fef2-148">The following diagram shows the relationship between hubs, endpoints, and clients.</span></span>

![Mapa SignalR](introduction/_static/signalr-core-architecture.png)

## <a name="related-resources"></a><span data-ttu-id="4fef2-150">Zasoby pokrewne</span><span class="sxs-lookup"><span data-stu-id="4fef2-150">Related resources</span></span>

[<span data-ttu-id="4fef2-151">Rozpoczynanie pracy z SignalR dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4fef2-151">Get started with SignalR for ASP.NET Core</span></span>](xref:signalr/get-started)
