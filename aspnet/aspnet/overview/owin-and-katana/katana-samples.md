---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Przykłady Katana | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: e81da1e650d8dfd24a3e0fda6aa42b7f360ce12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393322"
---
<a name="katana-samples"></a><span data-ttu-id="79663-102">Przykłady Katana</span><span class="sxs-lookup"><span data-stu-id="79663-102">Katana Samples</span></span>
====================
<span data-ttu-id="79663-103">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="79663-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="79663-104">Przykłady Katana</span><span class="sxs-lookup"><span data-stu-id="79663-104">Katana Samples</span></span>

<span data-ttu-id="79663-105">**ASP.NET kieruje przykładowe** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="79663-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="79663-106">W niektórych aplikacjach można dołączyć składniki OWIN w tabeli trasy Asp.Net równolegle składniki bez OWIN.</span><span class="sxs-lookup"><span data-stu-id="79663-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="79663-107">Ten przykład pokazuje, jak użyć metod rozszerzających RouteCollection MapOwinPath i udostępniane przez Microsoft.Owin.Host.SystemWeb MapOwinRoute.</span><span class="sxs-lookup"><span data-stu-id="79663-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="79663-108">**Rozgałęzianie potoków przykładowe** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="79663-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="79663-109">Potoki przetwarzania żądania OWIN czy muszą być liniowego, może zostać rozgałęzione do przetwarzania żądań w różny sposób.</span><span class="sxs-lookup"><span data-stu-id="79663-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="79663-110">W tym przykładzie pokazano, jak do budowy potoku rozgałęziania, na podstawie ścieżki żądania lub inne dane na żądanie, takich jak nagłówki.</span><span class="sxs-lookup"><span data-stu-id="79663-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="79663-111">Te składniki są dostępne w pakiecie nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="79663-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="79663-112">**Przykład niestandardowych Server** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="79663-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="79663-113">Pokazuje, jak użyć niestandardowego serwera OWIN, gdy hostingu samodzielnego OWIN.</span><span class="sxs-lookup"><span data-stu-id="79663-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="79663-114">**Przykładowy osadzony** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="79663-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="79663-115">Niektóre serwery OWIN mogą być uruchamiane wewnątrz własnego procesu (&quot;może być samodzielnie hostowane&quot;).</span><span class="sxs-lookup"><span data-stu-id="79663-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="79663-116">W tym przykładzie pokazano, jak można uruchomić za pomocą narzędzi dostarczanych przez pakiet nuget Microsoft.Owin.Hosting aplikacji OWIN.</span><span class="sxs-lookup"><span data-stu-id="79663-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="79663-117">**Przykładowe HelloWorld** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="79663-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="79663-118">OWIN to serwer HTTP abstrakcji interfejsu API, który umożliwia przenoszenia aplikacji na różnych serwerach.</span><span class="sxs-lookup"><span data-stu-id="79663-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="79663-119">W tym przykładzie pokazano, jak napisać aplikację Hello World w niektórych **proste otoki** wokół nieprzetworzone abstrakcji OWIN i uruchom go na serwerze sieci web takich jak ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="79663-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="79663-120">**Przykładowa aplikacja Hello World pierwotne OWIN** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="79663-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="79663-121">W tym przykładzie przedstawiono sposób pisania aplikacji Hello World za pomocą **pierwotne** abstrakcji OWIN i uruchom go na serwerze sieci web, takimi jak Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="79663-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="79663-122">**Przykładowe SignalR** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="79663-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="79663-123">Pokazuje, jak na potrzeby samodzielnego hostowania SignalR przy użyciu OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="79663-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="79663-124">Aby uzyskać więcej informacji na temat biblioteki SignalR własnym hostingu, zobacz [samouczek: Host samodzielny SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="79663-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="79663-125">**Statyczne pliki przykładowe** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="79663-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="79663-126">Pokazuje, jak do obsługi żądań HTTP dla plików statycznych przy użyciu OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="79663-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="79663-127">**Interfejs API sieci Web** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="79663-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="79663-128">W tym przykładzie pokazano, jak hostowanie OWIN w usługach IIS, a następnie Dodaj interfejs API sieci Web do potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="79663-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="79663-129">**Sieci Web przykładowej gniazda** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="79663-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="79663-130">Pokazuje, jak do obsługi gniazda sieci Web w OWIN przy użyciu [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) klasy.</span><span class="sxs-lookup"><span data-stu-id="79663-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
