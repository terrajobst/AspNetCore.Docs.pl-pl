---
uid: aspnet/overview/owin-and-katana/katana-samples
title: "Przykłady Katana | Dokumentacja firmy Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="katana-samples"></a><span data-ttu-id="08de3-102">Przykłady Katana</span><span class="sxs-lookup"><span data-stu-id="08de3-102">Katana Samples</span></span>
====================
<span data-ttu-id="08de3-103">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="08de3-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="08de3-104">Przykłady Katana</span><span class="sxs-lookup"><span data-stu-id="08de3-104">Katana Samples</span></span>

<span data-ttu-id="08de3-105">**ASP.NET kieruje próbki** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="08de3-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="08de3-106">W niektórych aplikacjach ma Podłączanie składników OWIN w tabeli tras Asp.Net równolegle składniki z systemem innym niż OWIN.</span><span class="sxs-lookup"><span data-stu-id="08de3-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="08de3-107">Ten przykład przedstawia sposób użycia metody rozszerzenia klasy RouteCollection MapOwinPath i MapOwinRoute dostarczonych przez Microsoft.Owin.Host.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="08de3-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="08de3-108">**Rozgałęzianie potoków próbki** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="08de3-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="08de3-109">Potoki przetwarzania żądania OWIN zbędna, nie jest liniowa, można można rozgałęzić do przetwarzania żądań na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="08de3-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="08de3-110">W tym przykładzie pokazano, jak utworzyć potok rozgałęziania na podstawie ścieżki żądania lub inne dane żądania, takich jak nagłówki.</span><span class="sxs-lookup"><span data-stu-id="08de3-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="08de3-111">Składniki te są dostępne w pakiecie nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="08de3-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="08de3-112">**Przykład niestandardowych Server** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="08de3-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="08de3-113">Przedstawia sposób użycia niestandardowego serwera OWIN, gdy hostingu samodzielnego OWIN.</span><span class="sxs-lookup"><span data-stu-id="08de3-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="08de3-114">**Osadzonego próbce** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="08de3-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="08de3-115">Niektóre serwery OWIN mogą być uruchamiane wewnątrz własnego procesu (&quot;hosta samodzielnego&quot;).</span><span class="sxs-lookup"><span data-stu-id="08de3-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="08de3-116">W tym przykładzie pokazano, jak rozpocząć korzystanie z narzędzi dostarczanych przez pakiet nuget Microsoft.Owin.Hosting aplikacji OWIN.</span><span class="sxs-lookup"><span data-stu-id="08de3-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="08de3-117">**Przykładowe HelloWorld** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="08de3-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="08de3-118">OWIN to serwer HTTP abstrakcji interfejsu API, zapewniające przenośność aplikacji na różnych serwerach.</span><span class="sxs-lookup"><span data-stu-id="08de3-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="08de3-119">W tym przykładzie przedstawiono sposób tworzenia aplikacji Hello World niektórych **proste otoki** wokół pierwotnych abstrakcji OWIN i uruchom go na serwerze sieci web program ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="08de3-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="08de3-120">**Przykładowa aplikacja Hello World Raw OWIN** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="08de3-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="08de3-121">W tym przykładzie pokazano, jak aplikacja Hello World przy użyciu **raw** abstrakcji OWIN i uruchom go na serwerze sieci web, takich jak Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="08de3-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="08de3-122">**Przykładowe SignalR** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="08de3-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="08de3-123">Pokazuje, jak hosta samodzielnego SignalR za pomocą OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="08de3-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="08de3-124">Aby uzyskać więcej informacji na temat własnym hostingu SignalR, zobacz [samouczek: SignalR hosta samodzielnego](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="08de3-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="08de3-125">**Przykładowe pliki statyczne** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="08de3-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="08de3-126">Przedstawia sposób obsługi żądań HTTP dotyczących plików statycznych przy użyciu OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="08de3-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="08de3-127">**Interfejs API sieci Web** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="08de3-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="08de3-128">W tym przykładzie pokazano, jak udostępniać OWIN w usługach IIS i Dodaj interfejs API sieci Web do potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="08de3-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="08de3-129">**Sieci Web przykładowej gniazda** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="08de3-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="08de3-130">Przedstawia sposób obsługi gniazda sieci Web w OWIN za pomocą [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) klasy.</span><span class="sxs-lookup"><span data-stu-id="08de3-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
