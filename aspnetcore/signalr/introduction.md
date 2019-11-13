---
title: Wprowadzenie do ASP.NET Core SignalR
author: bradygaster
description: Dowiedz się, w jaki sposób biblioteka SignalR ASP.NET Core upraszcza Dodawanie funkcji do aplikacji w czasie rzeczywistym.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/introduction
ms.openlocfilehash: 7108d9f223db78937dd1203a1cb4b890006b20ec
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963939"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a><span data-ttu-id="fd6bd-103">Wprowadzenie do ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="fd6bd-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-opno-locsignalr"></a><span data-ttu-id="fd6bd-104">Co to jest SignalR?</span><span class="sxs-lookup"><span data-stu-id="fd6bd-104">What is SignalR?</span></span>

<span data-ttu-id="fd6bd-105">ASP.NET Core SignalR jest biblioteką Open Source, która upraszcza Dodawanie funkcji sieci Web w czasie rzeczywistym do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="fd6bd-106">Funkcja sieci Web w czasie rzeczywistym umożliwia serwerowi natychmiastowe wypychanie zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="fd6bd-107">Dobre kandydaci dla SignalR:</span><span class="sxs-lookup"><span data-stu-id="fd6bd-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="fd6bd-108">Aplikacje, które wymagają aktualizacji o wysokiej częstotliwości z serwera.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="fd6bd-109">Przykładami są gry, sieci społecznościowe, głosowania, licytacji, mapy i aplikacje GPS.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="fd6bd-110">Pulpity nawigacyjne i monitorowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="fd6bd-111">Przykłady obejmują pulpity nawigacyjne firmy, natychmiastowe aktualizacje sprzedaży lub alerty dotyczące podróży.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="fd6bd-112">Aplikacje do współpracy.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-112">Collaborative apps.</span></span> <span data-ttu-id="fd6bd-113">Aplikacje tablicowe i oprogramowanie do spotkań zespołowych to przykłady aplikacji do współpracy.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="fd6bd-114">Aplikacje, które wymagają powiadomień.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-114">Apps that require notifications.</span></span> <span data-ttu-id="fd6bd-115">Sieci społecznościowe, wiadomości e-mail, rozmowy, gry, alerty podróży i wiele innych aplikacji używają powiadomień.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

SignalR<span data-ttu-id="fd6bd-116"> udostępnia interfejs API do tworzenia [zdalnych wywołań procedur serwer-klient (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="fd6bd-116"> provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="fd6bd-117">Wywołania RPC wywołują funkcje JavaScript na klientach z kodu .NET Core po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="fd6bd-118">Poniżej przedstawiono niektóre funkcje SignalR dla ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fd6bd-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="fd6bd-119">Obsługuje automatyczne zarządzanie połączeniami.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="fd6bd-120">Wysyła komunikaty do wszystkich połączonych klientów jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="fd6bd-121">Na przykład pokój rozmów.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-121">For example, a chat room.</span></span>
* <span data-ttu-id="fd6bd-122">Wysyła komunikaty do określonych klientów lub grup klientów.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="fd6bd-123">Skaluje się do obsługi zwiększania ruchu.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="fd6bd-124">Źródło jest hostowane w [repozytoriumSignalR w serwisie GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span><span class="sxs-lookup"><span data-stu-id="fd6bd-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="fd6bd-125">Transporty</span><span class="sxs-lookup"><span data-stu-id="fd6bd-125">Transports</span></span>

SignalR<span data-ttu-id="fd6bd-126"> obsługuje kilka technik obsługi komunikacji w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="fd6bd-126"> supports several techniques for handling real-time communications:</span></span>

* [<span data-ttu-id="fd6bd-127">Obiekty WebSocket</span><span class="sxs-lookup"><span data-stu-id="fd6bd-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="fd6bd-128">Zdarzenia wysłane przez serwer</span><span class="sxs-lookup"><span data-stu-id="fd6bd-128">Server-Sent Events</span></span>
* <span data-ttu-id="fd6bd-129">Długotrwałe sondowanie</span><span class="sxs-lookup"><span data-stu-id="fd6bd-129">Long Polling</span></span>

SignalR<span data-ttu-id="fd6bd-130"> automatycznie wybiera najlepszą metodę transportu, która znajduje się w możliwościach serwera i klienta.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-130"> automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="fd6bd-131">Centra</span><span class="sxs-lookup"><span data-stu-id="fd6bd-131">Hubs</span></span>

SignalR<span data-ttu-id="fd6bd-132"> używa *centrów* do komunikacji między klientami a serwerami.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-132"> uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="fd6bd-133">Koncentrator jest potokiem wysokiego poziomu, który umożliwia klientowi i serwerowi wywoływanie metod ze sobą.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> SignalR<span data-ttu-id="fd6bd-134"> obsługuje automatyczne wysyłanie między granicami maszyn, umożliwiając klientom wywoływanie metod na serwerze i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-134"> handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="fd6bd-135">Parametry z jednoznacznie określonymi typami można przekazać do metod, które umożliwiają powiązanie modelu.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> SignalR<span data-ttu-id="fd6bd-136"> oferuje dwa wbudowane protokoły centrum: protokół tekstowy oparty na formacie JSON i protokół binarny oparty na [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="fd6bd-136"> provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="fd6bd-137">MessagePack zazwyczaj tworzy mniejsze komunikaty w porównaniu z formatem JSON.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="fd6bd-138">Starsze przeglądarki muszą obsługiwać [XHR Level 2](https://caniuse.com/#feat=xhr2) , aby zapewnić obsługę protokołu MessagePack.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="fd6bd-139">Centra wywołują kod po stronie klienta, wysyłając komunikaty zawierające nazwę i parametry metody po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="fd6bd-140">Obiekty wysyłane jako parametry metody są deserializowane przy użyciu skonfigurowanego protokołu.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="fd6bd-141">Klient próbuje dopasować nazwę do metody w kodzie po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="fd6bd-142">Gdy klient znajdzie dopasowanie, wywołuje metodę i przekazuje do niej dane parametrów, które są deserializowane.</span><span class="sxs-lookup"><span data-stu-id="fd6bd-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fd6bd-143">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fd6bd-143">Additional resources</span></span>

* <span data-ttu-id="fd6bd-144">[Wprowadzenie do SignalR dla ASP.NET Core](xref:tutorials/signalr)</span><span class="sxs-lookup"><span data-stu-id="fd6bd-144">[Get started with SignalR for ASP.NET Core](xref:tutorials/signalr)</span></span>
* [<span data-ttu-id="fd6bd-145">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="fd6bd-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="fd6bd-146">Centra</span><span class="sxs-lookup"><span data-stu-id="fd6bd-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="fd6bd-147">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd6bd-147">JavaScript client</span></span>](xref:signalr/javascript-client)
