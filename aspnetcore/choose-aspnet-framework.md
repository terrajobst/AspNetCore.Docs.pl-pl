---
title: "Wybór między ASP.NET i ASP.NET Core"
author: rick-anderson
description: "Dowiedz się, jak wybrać ASP.NET i ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: c909c9a852549577c4a9fbc461aaf3f710b301ef
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="800fc-103">Wybór między ASP.NET i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="800fc-103">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="800fc-104">Niezależnie od tego, w przypadku tworzenia aplikacji sieci web platformy ASP.NET zawiera rozwiązanie dla Ciebie: z aplikacji sieci web przeznaczonych dla systemu Windows Server, aby małych mikrousług przeznaczonych dla kontenery systemu Linux i wszystko między nimi.</span><span class="sxs-lookup"><span data-stu-id="800fc-104">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="800fc-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="800fc-105">ASP.NET Core</span></span>

<span data-ttu-id="800fc-106">Platformy ASP.NET Core to open source, międzyplatformowa struktura do tworzenia aplikacji sieci web nowoczesny, oparte na chmurze dla systemu Windows, system macOS lub Linux.</span><span class="sxs-lookup"><span data-stu-id="800fc-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="800fc-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="800fc-107">ASP.NET</span></span>

<span data-ttu-id="800fc-108">ASP.NET to dojrzały platforma, która zawiera wszystkich usług niezbędnych do tworzenia klasy korporacyjnej, na serwerze aplikacji sieci web w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="800fc-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="800fc-109">Które z nich jest dla mnie odpowiednia?</span><span class="sxs-lookup"><span data-stu-id="800fc-109">Which one is right for me?</span></span>

| <span data-ttu-id="800fc-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="800fc-110">ASP.NET Core</span></span> | <span data-ttu-id="800fc-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="800fc-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="800fc-112">Tworzenie dla systemu Windows, system macOS lub Linux</span><span class="sxs-lookup"><span data-stu-id="800fc-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="800fc-113">Kompilacji dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="800fc-113">Build for Windows</span></span>|
|<span data-ttu-id="800fc-114">[Stron razor](xref:mvc/razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web platformy ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="800fc-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="800fc-115">Zobacz też [MVC](xref:mvc/overview) i [interfejs API sieci Web](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="800fc-115">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="800fc-116">Użyj [sieci Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [interfejs API sieci Web](https://docs.microsoft.com/aspnet/web-api/), lub [stron sieci Web](https://docs.microsoft.com/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="800fc-116">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="800fc-117">Wiele wersji na maszynie</span><span class="sxs-lookup"><span data-stu-id="800fc-117">Multiple versions per machine</span></span>|<span data-ttu-id="800fc-118">Jednej wersji na maszynie</span><span class="sxs-lookup"><span data-stu-id="800fc-118">One version per machine</span></span>|
|<span data-ttu-id="800fc-119">Tworzenie za pomocą programu Visual Studio, [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), lub [Visual Studio Code](https://code.visualstudio.com/) przy użyciu języka C# lub języka F #</span><span class="sxs-lookup"><span data-stu-id="800fc-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="800fc-120">Tworzenie z programem Visual Studio za pomocą C#, VB i F #</span><span class="sxs-lookup"><span data-stu-id="800fc-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="800fc-121">Większą wydajność niż ASP.NET</span><span class="sxs-lookup"><span data-stu-id="800fc-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="800fc-122">Dobrą wydajność</span><span class="sxs-lookup"><span data-stu-id="800fc-122">Good performance</span></span>|
|[<span data-ttu-id="800fc-123">Wybierz środowisko uruchomieniowe .NET Framework lub .NET Core</span><span class="sxs-lookup"><span data-stu-id="800fc-123">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="800fc-124">Użyj środowiska uruchomieniowego .NET Framework</span><span class="sxs-lookup"><span data-stu-id="800fc-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="800fc-125">Scenariusze platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="800fc-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="800fc-126">[Stron razor](xref:mvc/razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web platformy ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="800fc-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="800fc-127">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="800fc-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="800fc-128">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="800fc-128">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="800fc-129">Scenariusze programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="800fc-129">ASP.NET scenarios</span></span>

* [<span data-ttu-id="800fc-130">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="800fc-130">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="800fc-131">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="800fc-131">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="800fc-132">W czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="800fc-132">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="800fc-133">Resources</span><span class="sxs-lookup"><span data-stu-id="800fc-133">Resources</span></span>

* [<span data-ttu-id="800fc-134">Wprowadzenie do programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="800fc-134">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="800fc-135">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="800fc-135">Introduction to ASP.NET Core</span></span>](xref:index)
