---
title: "Wybór między ASP.NET i ASP.NET Core"
author: rick-anderson
description: "Dowiedz się, jak wybrać ASP.NET i ASP.NET Core."
keywords: "Platformy ASP.NET Core podstawy, omówienie"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: article
ms.assetid: f0d17abf-3c69-413e-87fc-30780805e33f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 875064bd3437acc4e2a53220e19e86431d8c159b
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2017
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="b3721-104">Wybór między ASP.NET i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3721-104">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="b3721-105">Niezależnie od tego, w przypadku tworzenia aplikacji sieci web platformy ASP.NET zawiera rozwiązanie dla Ciebie: z aplikacji sieci web przeznaczonych dla systemu Windows Server, aby małych mikrousług przeznaczonych dla kontenery systemu Linux i wszystko między nimi.</span><span class="sxs-lookup"><span data-stu-id="b3721-105">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="b3721-106">Platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3721-106">ASP.NET Core</span></span>

<span data-ttu-id="b3721-107">Platformy ASP.NET Core to open source, międzyplatformowa struktura do tworzenia aplikacji sieci web nowoczesny, oparte na chmurze dla systemu Windows, system macOS lub Linux.</span><span class="sxs-lookup"><span data-stu-id="b3721-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="b3721-108">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b3721-108">ASP.NET</span></span>

<span data-ttu-id="b3721-109">ASP.NET to dojrzały platforma, która zawiera wszystkich usług niezbędnych do tworzenia klasy korporacyjnej, na serwerze aplikacji sieci web w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="b3721-109">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="b3721-110">Które z nich jest dla mnie odpowiednia?</span><span class="sxs-lookup"><span data-stu-id="b3721-110">Which one is right for me?</span></span>

| <span data-ttu-id="b3721-111">Platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3721-111">ASP.NET Core</span></span> | <span data-ttu-id="b3721-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b3721-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="b3721-113">Tworzenie dla systemu Windows, system macOS lub Linux</span><span class="sxs-lookup"><span data-stu-id="b3721-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="b3721-114">Kompilacji dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="b3721-114">Build for Windows</span></span>|
|<span data-ttu-id="b3721-115">[Stron razor](xref:mvc/razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web platformy ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="b3721-115">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="b3721-116">Zobacz też [MVC](xref:mvc/overview) i [interfejs API sieci Web](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="b3721-116">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="b3721-117">Użyj [sieci Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [interfejs API sieci Web](https://docs.microsoft.com/aspnet/web-api/), lub [stron sieci Web](https://docs.microsoft.com/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="b3721-117">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="b3721-118">Wiele wersji na maszynie</span><span class="sxs-lookup"><span data-stu-id="b3721-118">Multiple versions per machine</span></span>|<span data-ttu-id="b3721-119">Jednej wersji na maszynie</span><span class="sxs-lookup"><span data-stu-id="b3721-119">One version per machine</span></span>|
|<span data-ttu-id="b3721-120">Tworzenie za pomocą programu Visual Studio, [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), lub [Visual Studio Code](https://code.visualstudio.com/) przy użyciu języka C# lub języka F #</span><span class="sxs-lookup"><span data-stu-id="b3721-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="b3721-121">Tworzenie z programem Visual Studio za pomocą C#, VB i F #</span><span class="sxs-lookup"><span data-stu-id="b3721-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="b3721-122">Większą wydajność niż ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b3721-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="b3721-123">Dobrą wydajność</span><span class="sxs-lookup"><span data-stu-id="b3721-123">Good performance</span></span>|
|[<span data-ttu-id="b3721-124">Wybierz środowisko uruchomieniowe .NET Framework lub .NET Core</span><span class="sxs-lookup"><span data-stu-id="b3721-124">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="b3721-125">Użyj środowiska uruchomieniowego .NET Framework</span><span class="sxs-lookup"><span data-stu-id="b3721-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="b3721-126">Scenariusze platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3721-126">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="b3721-127">[Stron razor](xref:mvc/razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web platformy ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="b3721-127">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="b3721-128">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="b3721-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="b3721-129">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="b3721-129">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="b3721-130">Scenariusze programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b3721-130">ASP.NET scenarios</span></span>

* [<span data-ttu-id="b3721-131">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="b3721-131">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="b3721-132">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="b3721-132">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="b3721-133">W czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="b3721-133">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="b3721-134">Resources</span><span class="sxs-lookup"><span data-stu-id="b3721-134">Resources</span></span>

* [<span data-ttu-id="b3721-135">Wprowadzenie do programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b3721-135">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="b3721-136">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3721-136">Introduction to ASP.NET Core</span></span>](xref:index)
