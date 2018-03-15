---
title: "Wybór między ASP.NET i ASP.NET Core"
author: rick-anderson
description: "Dowiedz się, jak wybrać ASP.NET i ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 03/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 3986e904d6670c451edc5c9338dc07e18d3c207d
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="40241-103">Wybór między ASP.NET i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40241-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="40241-104">Niezależnie od tego, w przypadku tworzenia aplikacji sieci web platformy ASP.NET zawiera rozwiązanie dla Ciebie: z przedsiębiorstwa aplikacji sieci web przeznaczonych dla systemu Windows Server, do małych mikrousług przeznaczonych dla kontenery systemu Linux i wszystko między nimi.</span><span class="sxs-lookup"><span data-stu-id="40241-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="40241-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40241-105">ASP.NET Core</span></span>

<span data-ttu-id="40241-106">Platformy ASP.NET Core to open source, międzyplatformowa struktura do tworzenia aplikacji sieci web nowoczesny, oparte na chmurze systemu Windows, system macOS lub Linux.</span><span class="sxs-lookup"><span data-stu-id="40241-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="40241-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="40241-107">ASP.NET</span></span>

<span data-ttu-id="40241-108">ASP.NET to dojrzały platforma, która zawiera wszystkich usług niezbędnych do tworzenia klasy korporacyjnej, aplikacje oparte na serwerze sieci web w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="40241-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web apps on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="40241-109">Które z nich jest dla mnie odpowiednia?</span><span class="sxs-lookup"><span data-stu-id="40241-109">Which one is right for me?</span></span>

| <span data-ttu-id="40241-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40241-110">ASP.NET Core</span></span> | <span data-ttu-id="40241-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="40241-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="40241-112">Tworzenie dla systemu Windows, system macOS lub Linux</span><span class="sxs-lookup"><span data-stu-id="40241-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="40241-113">Kompilacji dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="40241-113">Build for Windows</span></span>|
|<span data-ttu-id="40241-114">[Stron razor](xref:mvc/razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web począwszy od platformy ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="40241-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="40241-115">Zobacz też [MVC](xref:mvc/overview), [interfejs API sieci Web](xref:tutorials/first-web-api), i [SignalR](xref:signalr/introduction-signalr-core).</span><span class="sxs-lookup"><span data-stu-id="40241-115">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction-signalr-core).</span></span>|<span data-ttu-id="40241-116">Użyj [sieci Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [interfejs API sieci Web](/aspnet/web-api/), lub [stron sieci Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="40241-116">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="40241-117">Wiele wersji na maszynie</span><span class="sxs-lookup"><span data-stu-id="40241-117">Multiple versions per machine</span></span>|<span data-ttu-id="40241-118">Jednej wersji na maszynie</span><span class="sxs-lookup"><span data-stu-id="40241-118">One version per machine</span></span>|
|<span data-ttu-id="40241-119">Tworzenie za pomocą programu Visual Studio, [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), lub [Visual Studio Code](https://code.visualstudio.com/) przy użyciu języka C# lub języka F #</span><span class="sxs-lookup"><span data-stu-id="40241-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="40241-120">Tworzenie z programem Visual Studio za pomocą C#, VB i F #</span><span class="sxs-lookup"><span data-stu-id="40241-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="40241-121">Większą wydajność niż ASP.NET</span><span class="sxs-lookup"><span data-stu-id="40241-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="40241-122">Dobrą wydajność</span><span class="sxs-lookup"><span data-stu-id="40241-122">Good performance</span></span>|
|[<span data-ttu-id="40241-123">Wybierz środowisko uruchomieniowe .NET Framework lub .NET Core</span><span class="sxs-lookup"><span data-stu-id="40241-123">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="40241-124">Użyj środowiska uruchomieniowego .NET Framework</span><span class="sxs-lookup"><span data-stu-id="40241-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="40241-125">Scenariusze platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40241-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="40241-126">[Stron razor](xref:mvc/razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web począwszy od platformy ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="40241-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="40241-127">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="40241-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="40241-128">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="40241-128">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="40241-129">W czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="40241-129">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="40241-130">Scenariusze programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="40241-130">ASP.NET scenarios</span></span>

* [<span data-ttu-id="40241-131">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="40241-131">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="40241-132">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="40241-132">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="40241-133">W czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="40241-133">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="40241-134">Resources</span><span class="sxs-lookup"><span data-stu-id="40241-134">Resources</span></span>

* [<span data-ttu-id="40241-135">Wprowadzenie do programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="40241-135">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="40241-136">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40241-136">Introduction to ASP.NET Core</span></span>](xref:index)
