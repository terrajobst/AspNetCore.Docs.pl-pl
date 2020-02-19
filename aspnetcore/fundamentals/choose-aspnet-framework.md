---
title: Wybieranie między ASP.NET 4.x i ASP.NET Core
author: rick-anderson
description: Wyjaśnia ASP.NET Core a ASP.NET 4. x oraz jak wybierać między nimi.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 02/12/2020
no-loc:
- SignalR
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: a7280b59578ee1d96edeeccf9c9df0b0e4eb4eb8
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447298"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="04540-103">Wybieranie między ASP.NET 4.x i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04540-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="04540-104">Platforma ASP.NET Core to przeprojektowana platforma ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="04540-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="04540-105">W tym artykule przedstawiono różnice między nimi.</span><span class="sxs-lookup"><span data-stu-id="04540-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="04540-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04540-106">ASP.NET Core</span></span>

<span data-ttu-id="04540-107">ASP.NET Core to architektura typu open source i między platformami do tworzenia aplikacji sieci web Nowoczesna, oparta na chmurze systemie Windows, macOS lub Linux.</span><span class="sxs-lookup"><span data-stu-id="04540-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="04540-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="04540-108">ASP.NET 4.x</span></span>

<span data-ttu-id="04540-109">ASP.NET 4.x to dojrzała platforma, która zapewnia usługi potrzebne do tworzenia przeznaczonych dla przedsiębiorstw oparte na serwerze aplikacji sieci web na Windows.</span><span class="sxs-lookup"><span data-stu-id="04540-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="04540-110">Wybór Framework</span><span class="sxs-lookup"><span data-stu-id="04540-110">Framework selection</span></span>

<span data-ttu-id="04540-111">W poniższej tabeli porównano platformy ASP.NET Core, platformy ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="04540-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="04540-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04540-112">ASP.NET Core</span></span> | <span data-ttu-id="04540-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="04540-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="04540-114">Tworzenie dla systemu Windows, system macOS lub Linux</span><span class="sxs-lookup"><span data-stu-id="04540-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="04540-115">Tworzenie dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="04540-115">Build for Windows</span></span>|
|<span data-ttu-id="04540-116">[Razor Pages](xref:razor-pages/index) jest zalecanym podejściem do tworzenia interfejsu użytkownika sieci Web w przypadku ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="04540-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="04540-117">Zobacz również [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api)i [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="04540-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="04540-118">Korzystanie z [formularzy sieci Web](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), internetowego [interfejsu API](/aspnet/web-api/), elementów [webhook](/aspnet/webhooks/)lub [stron sieci Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="04540-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="04540-119">Wiele wersji na maszynie</span><span class="sxs-lookup"><span data-stu-id="04540-119">Multiple versions per machine</span></span>|<span data-ttu-id="04540-120">Jedna wersja na maszynie</span><span class="sxs-lookup"><span data-stu-id="04540-120">One version per machine</span></span>|
|<span data-ttu-id="04540-121">Programowanie za pomocą [programu Visual Studio](https://visualstudio.microsoft.com/vs/), [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/)lub [Visual Studio Code](https://code.visualstudio.com/) przy użyciu C# lubF#</span><span class="sxs-lookup"><span data-stu-id="04540-121">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/), [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="04540-122">Programowanie w programie [Visual Studio](https://visualstudio.microsoft.com/vs/) przy użyciu C#programu, VB lubF#</span><span class="sxs-lookup"><span data-stu-id="04540-122">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/) using C#, VB, or F#</span></span>|
|<span data-ttu-id="04540-123">Wyższą wydajność niż ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="04540-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="04540-124">Dobra wydajność</span><span class="sxs-lookup"><span data-stu-id="04540-124">Good performance</span></span>|
|[<span data-ttu-id="04540-125">Korzystanie z środowiska uruchomieniowego platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="04540-125">Use .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="04540-126">Używasz środowiska uruchomieniowego .NET Framework</span><span class="sxs-lookup"><span data-stu-id="04540-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="04540-127">Aby uzyskać informacje na temat obsługi programu ASP.NET Core 2. x w systemie .NET Framework, zobacz [ASP.NET Core określania celu .NET Framework](xref:index#target-framework) .</span><span class="sxs-lookup"><span data-stu-id="04540-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="04540-128">Scenariusze platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04540-128">ASP.NET Core scenarios</span></span>

* [<span data-ttu-id="04540-129">Zaufany</span><span class="sxs-lookup"><span data-stu-id="04540-129">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="04540-130">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="04540-130">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="04540-131">W czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="04540-131">Real-time</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="04540-132">Wdrażanie aplikacji ASP.NET Core na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="04540-132">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="04540-133">Scenariusze 4.x ASP.NET</span><span class="sxs-lookup"><span data-stu-id="04540-133">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="04540-134">Zaufany</span><span class="sxs-lookup"><span data-stu-id="04540-134">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="04540-135">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="04540-135">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="04540-136">W czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="04540-136">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="04540-137">Tworzenie aplikacji sieci Web ASP.NET 4. x na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="04540-137">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="04540-138">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="04540-138">Additional resources</span></span>

* [<span data-ttu-id="04540-139">Wprowadzenie do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="04540-139">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="04540-140">Wprowadzenie do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04540-140">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>
