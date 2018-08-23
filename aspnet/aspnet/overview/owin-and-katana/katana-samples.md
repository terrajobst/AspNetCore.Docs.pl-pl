---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Przykłady Katana | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: b8ce2b40a19e0429f1ccedb03b8f829582652d24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756295"
---
<a name="katana-samples"></a><span data-ttu-id="104ad-102">Przykłady Katana</span><span class="sxs-lookup"><span data-stu-id="104ad-102">Katana Samples</span></span>
====================
<span data-ttu-id="104ad-103">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="104ad-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="104ad-104">Przykłady Katana</span><span class="sxs-lookup"><span data-stu-id="104ad-104">Katana Samples</span></span>

<span data-ttu-id="104ad-105">**ASP.NET kieruje przykładowe** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span><span class="sxs-lookup"><span data-stu-id="104ad-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="104ad-106">W niektórych aplikacjach można dołączyć składniki OWIN w tabeli trasy Asp.Net równolegle składniki bez OWIN.</span><span class="sxs-lookup"><span data-stu-id="104ad-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="104ad-107">Ten przykład pokazuje, jak użyć metod rozszerzających RouteCollection MapOwinPath i udostępniane przez Microsoft.Owin.Host.SystemWeb MapOwinRoute.</span><span class="sxs-lookup"><span data-stu-id="104ad-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="104ad-108">**Rozgałęzianie potoków przykładowe** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span><span class="sxs-lookup"><span data-stu-id="104ad-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="104ad-109">Potoki przetwarzania żądania OWIN czy muszą być liniowego, może zostać rozgałęzione do przetwarzania żądań w różny sposób.</span><span class="sxs-lookup"><span data-stu-id="104ad-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="104ad-110">W tym przykładzie pokazano, jak do budowy potoku rozgałęziania, na podstawie ścieżki żądania lub inne dane na żądanie, takich jak nagłówki.</span><span class="sxs-lookup"><span data-stu-id="104ad-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="104ad-111">Te składniki są dostępne w pakiecie nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="104ad-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="104ad-112">**Przykład niestandardowych Server** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span><span class="sxs-lookup"><span data-stu-id="104ad-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="104ad-113">Pokazuje, jak użyć niestandardowego serwera OWIN, gdy hostingu samodzielnego OWIN.</span><span class="sxs-lookup"><span data-stu-id="104ad-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="104ad-114">**Przykładowy osadzony** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span><span class="sxs-lookup"><span data-stu-id="104ad-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="104ad-115">Niektóre serwery OWIN mogą być uruchamiane wewnątrz własnego procesu (&quot;może być samodzielnie hostowane&quot;).</span><span class="sxs-lookup"><span data-stu-id="104ad-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="104ad-116">W tym przykładzie pokazano, jak można uruchomić za pomocą narzędzi dostarczanych przez pakiet nuget Microsoft.Owin.Hosting aplikacji OWIN.</span><span class="sxs-lookup"><span data-stu-id="104ad-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="104ad-117">**Przykładowe HelloWorld** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span><span class="sxs-lookup"><span data-stu-id="104ad-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="104ad-118">OWIN to serwer HTTP abstrakcji interfejsu API, który umożliwia przenoszenia aplikacji na różnych serwerach.</span><span class="sxs-lookup"><span data-stu-id="104ad-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="104ad-119">W tym przykładzie pokazano, jak napisać aplikację Hello World w niektórych **proste otoki** wokół nieprzetworzone abstrakcji OWIN i uruchom go na serwerze sieci web takich jak ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="104ad-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="104ad-120">**Przykładowa aplikacja Hello World pierwotne OWIN** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span><span class="sxs-lookup"><span data-stu-id="104ad-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="104ad-121">W tym przykładzie przedstawiono sposób pisania aplikacji Hello World za pomocą **pierwotne** abstrakcji OWIN i uruchom go na serwerze sieci web, takimi jak Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="104ad-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="104ad-122">**Przykładowe SignalR** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span><span class="sxs-lookup"><span data-stu-id="104ad-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="104ad-123">Pokazuje, jak na potrzeby samodzielnego hostowania SignalR przy użyciu OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="104ad-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="104ad-124">Aby uzyskać więcej informacji na temat biblioteki SignalR własnym hostingu, zobacz [samouczek: Host samodzielny SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="104ad-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="104ad-125">**Statyczne pliki przykładowe** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span><span class="sxs-lookup"><span data-stu-id="104ad-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="104ad-126">Pokazuje, jak do obsługi żądań HTTP dla plików statycznych przy użyciu OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="104ad-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="104ad-127">**Interfejs API sieci Web** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span><span class="sxs-lookup"><span data-stu-id="104ad-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="104ad-128">W tym przykładzie pokazano, jak hostowanie OWIN w usługach IIS, a następnie Dodaj interfejs API sieci Web do potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="104ad-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="104ad-129">**Sieci Web przykładowej gniazda** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span><span class="sxs-lookup"><span data-stu-id="104ad-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="104ad-130">Pokazuje, jak do obsługi gniazda sieci Web w OWIN przy użyciu [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) klasy.</span><span class="sxs-lookup"><span data-stu-id="104ad-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
