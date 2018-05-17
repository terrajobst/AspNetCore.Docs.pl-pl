---
title: Wybór między ASP.NET i ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wybrać ASP.NET i ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 0c6924d40b7327d2032a0278c56a0b4fa41d15a1
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="8a54e-103">Wybór między ASP.NET i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a54e-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="8a54e-104">Niezależnie od tego, jakiego typu aplikację tworzysz ASP.NET ma dla Ciebie rozwiązanie: od aplikacji sieci web przeznaczonych dla systemu Windows Server, do małych mikrousług przeznaczonych dla kontenerów systemu Linux i wiele innych.</span><span class="sxs-lookup"><span data-stu-id="8a54e-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="8a54e-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a54e-105">ASP.NET Core</span></span>

<span data-ttu-id="8a54e-106">Platformy ASP.NET Core to open source, międzyplatformowa struktura do tworzenia aplikacji sieci web nowoczesny, oparte na chmurze systemu Windows, system macOS lub Linux.</span><span class="sxs-lookup"><span data-stu-id="8a54e-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="8a54e-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8a54e-107">ASP.NET</span></span>

<span data-ttu-id="8a54e-108">ASP.NET to dojrzały platforma, która zawiera wszystkich usług niezbędnych do tworzenia korporacyjnej, aplikacje oparte na serwerze sieci web w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="8a54e-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="8a54e-109">Wybór Framework</span><span class="sxs-lookup"><span data-stu-id="8a54e-109">Framework selection</span></span>

<span data-ttu-id="8a54e-110">Przejrzeć tabelę poniżej, aby określić, które framework jest najbardziej odpowiednią do potrzeb.</span><span class="sxs-lookup"><span data-stu-id="8a54e-110">Review the table below to determine which framework is most appropriate for your needs.</span></span>

| <span data-ttu-id="8a54e-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a54e-111">ASP.NET Core</span></span> | <span data-ttu-id="8a54e-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8a54e-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="8a54e-113">Tworzenie dla systemu Windows, system macOS lub Linux</span><span class="sxs-lookup"><span data-stu-id="8a54e-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="8a54e-114">Tworzenie dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="8a54e-114">Build for Windows</span></span>|
|<span data-ttu-id="8a54e-115">[Stron razor](xref:mvc/razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web począwszy od platformy ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="8a54e-115">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="8a54e-116">Zobacz też [MVC](xref:mvc/overview), [interfejs API sieci Web](xref:tutorials/first-web-api), i [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="8a54e-116">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="8a54e-117">Użyj [sieci Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [interfejs API sieci Web](/aspnet/web-api/), [elementów Webhook](/aspnet/webhooks/), lub [stron sieci Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="8a54e-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="8a54e-118">Wiele wersji na maszynie</span><span class="sxs-lookup"><span data-stu-id="8a54e-118">Multiple versions per machine</span></span>|<span data-ttu-id="8a54e-119">Jedna wersja na maszynie</span><span class="sxs-lookup"><span data-stu-id="8a54e-119">One version per machine</span></span>|
|<span data-ttu-id="8a54e-120">Tworzenie za pomocą programu Visual Studio, [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), lub [Visual Studio Code](https://code.visualstudio.com/) przy użyciu języka C# lub języka F #</span><span class="sxs-lookup"><span data-stu-id="8a54e-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="8a54e-121">Tworzenie z programem Visual Studio za pomocą C#, VB i F #</span><span class="sxs-lookup"><span data-stu-id="8a54e-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="8a54e-122">Większa wydajność niż ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8a54e-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="8a54e-123">Dobra wydajność</span><span class="sxs-lookup"><span data-stu-id="8a54e-123">Good performance</span></span>|
|[<span data-ttu-id="8a54e-124">Wybierz środowisko uruchomieniowe .NET Framework lub .NET Core</span><span class="sxs-lookup"><span data-stu-id="8a54e-124">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="8a54e-125">Używasz środowiska uruchomieniowego .NET Framework</span><span class="sxs-lookup"><span data-stu-id="8a54e-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="8a54e-126">Scenariusze platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a54e-126">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="8a54e-127">[Stron razor](xref:mvc/razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web począwszy od platformy ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="8a54e-127">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="8a54e-128">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="8a54e-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="8a54e-129">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="8a54e-129">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="8a54e-130">W czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="8a54e-130">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="8a54e-131">Scenariusze programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8a54e-131">ASP.NET scenarios</span></span>

* [<span data-ttu-id="8a54e-132">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="8a54e-132">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="8a54e-133">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="8a54e-133">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="8a54e-134">W czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="8a54e-134">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="8a54e-135">Zasoby</span><span class="sxs-lookup"><span data-stu-id="8a54e-135">Resources</span></span>

* [<span data-ttu-id="8a54e-136">Wprowadzenie do programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8a54e-136">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="8a54e-137">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a54e-137">Introduction to ASP.NET Core</span></span>](xref:index)
