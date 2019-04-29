---
title: Wybieranie między ASP.NET 4.x i ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono vs platformy ASP.NET Core. ASP.NET 4.x i jak dokonać wyboru między nimi.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: a89af3ba8d95c80356439ee89601bda00d171d3b
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165144"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="740f4-103">Wybieranie między ASP.NET 4.x i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="740f4-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="740f4-104">Platforma ASP.NET Core to przeprojektowana platforma ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="740f4-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="740f4-105">W tym artykule przedstawiono różnice między nimi.</span><span class="sxs-lookup"><span data-stu-id="740f4-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="740f4-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="740f4-106">ASP.NET Core</span></span>

<span data-ttu-id="740f4-107">ASP.NET Core to architektura typu open source i między platformami do tworzenia aplikacji sieci web Nowoczesna, oparta na chmurze systemie Windows, macOS lub Linux.</span><span class="sxs-lookup"><span data-stu-id="740f4-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="740f4-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="740f4-108">ASP.NET 4.x</span></span>

<span data-ttu-id="740f4-109">ASP.NET 4.x to dojrzała platforma, która zapewnia usługi potrzebne do tworzenia przeznaczonych dla przedsiębiorstw oparte na serwerze aplikacji sieci web na Windows.</span><span class="sxs-lookup"><span data-stu-id="740f4-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="740f4-110">Wybór Framework</span><span class="sxs-lookup"><span data-stu-id="740f4-110">Framework selection</span></span>

<span data-ttu-id="740f4-111">W poniższej tabeli porównano platformy ASP.NET Core, platformy ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="740f4-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="740f4-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="740f4-112">ASP.NET Core</span></span> | <span data-ttu-id="740f4-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="740f4-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="740f4-114">Tworzenie dla systemu Windows, system macOS lub Linux</span><span class="sxs-lookup"><span data-stu-id="740f4-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="740f4-115">Tworzenie dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="740f4-115">Build for Windows</span></span>|
|<span data-ttu-id="740f4-116">[Strony razor](xref:razor-pages/index) przedstawia zalecane podejście do tworzenia interfejsu użytkownika sieci Web począwszy od platformy ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="740f4-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="740f4-117">Zobacz też [MVC](xref:mvc/overview), [interfejs API sieci Web](xref:tutorials/first-web-api), i [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="740f4-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="740f4-118">Użyj [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [interfejs API sieci Web](/aspnet/web-api/), [elementów Webhook](/aspnet/webhooks/), lub [stron sieci Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="740f4-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="740f4-119">Wiele wersji na maszynie</span><span class="sxs-lookup"><span data-stu-id="740f4-119">Multiple versions per machine</span></span>|<span data-ttu-id="740f4-120">Jedna wersja na maszynie</span><span class="sxs-lookup"><span data-stu-id="740f4-120">One version per machine</span></span>|
|<span data-ttu-id="740f4-121">Programowanie za pomocą programu Visual Studio [programu Visual Studio dla komputerów Mac](https://www.visualstudio.com/vs/visual-studio-mac/), lub [programu Visual Studio Code](https://code.visualstudio.com/) przy użyciu C# lubF#</span><span class="sxs-lookup"><span data-stu-id="740f4-121">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="740f4-122">Programowanie za pomocą programu Visual Studio przy użyciu C#, VB, lubF#</span><span class="sxs-lookup"><span data-stu-id="740f4-122">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="740f4-123">Wyższą wydajność niż ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="740f4-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="740f4-124">Dobra wydajność</span><span class="sxs-lookup"><span data-stu-id="740f4-124">Good performance</span></span>|
|[<span data-ttu-id="740f4-125">Wybierz środowisko uruchomieniowe platformy .NET Framework lub .NET Core</span><span class="sxs-lookup"><span data-stu-id="740f4-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="740f4-126">Używasz środowiska uruchomieniowego .NET Framework</span><span class="sxs-lookup"><span data-stu-id="740f4-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="740f4-127">Zobacz [platformy ASP.NET Core przeznaczone dla .NET Framework](xref:index#target-framework) informacje na temat platformy ASP.NET Core 2.x obsługi w programie .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="740f4-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="740f4-128">Scenariusze platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="740f4-128">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="740f4-129">[Strony razor](xref:razor-pages/index) przedstawia zalecane podejście do tworzenia interfejsu użytkownika sieci Web począwszy od platformy ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="740f4-129">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="740f4-130">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="740f4-130">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="740f4-131">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="740f4-131">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="740f4-132">W czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="740f4-132">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="740f4-133">Wdrażanie aplikacji ASP.NET Core na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="740f4-133">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="740f4-134">Scenariusze 4.x ASP.NET</span><span class="sxs-lookup"><span data-stu-id="740f4-134">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="740f4-135">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="740f4-135">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="740f4-136">Interfejsy API</span><span class="sxs-lookup"><span data-stu-id="740f4-136">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="740f4-137">W czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="740f4-137">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="740f4-138">Tworzenie aplikacji sieci web platformy ASP.NET 4.x na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="740f4-138">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="740f4-139">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="740f4-139">Additional resources</span></span>

* [<span data-ttu-id="740f4-140">Wprowadzenie do platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="740f4-140">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="740f4-141">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="740f4-141">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>
