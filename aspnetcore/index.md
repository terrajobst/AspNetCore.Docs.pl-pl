---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="6bf5c-104">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6bf5c-104">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="6bf5c-105">[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) i [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="6bf5c-105">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="6bf5c-106">ASP.NET Core to międzyplatformowa struktura typu [open source](https://github.com/aspnet/home) o wysokiej wydajności służąca do tworzenia nowoczesnych aplikacji internetowych opartych na chmurze.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-106">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="6bf5c-107">Platforma ASP.NET Core umożliwia:</span><span class="sxs-lookup"><span data-stu-id="6bf5c-107">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="6bf5c-108">Kompilowanie aplikacji i usług internetowych, aplikacji [IoT](https://www.microsoft.com/en-us/internet-of-things/) i zapleczy mobilnych.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-108">Build web apps and services, [IoT](https://www.microsoft.com/en-us/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="6bf5c-109">Używanie ulubionych narzędzi programistycznych w systemach Windows, macOS i Linux.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-109">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="6bf5c-110">Wdrażanie w chmurze i lokalne</span><span class="sxs-lookup"><span data-stu-id="6bf5c-110">Deploy to the cloud or on-premises</span></span>
* <span data-ttu-id="6bf5c-111">Uruchamianie na platformie [.NET Core lub .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="6bf5c-111">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="6bf5c-112">Dlaczego warto korzystać z platformy ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="6bf5c-112">Why use ASP.NET Core?</span></span>

<span data-ttu-id="6bf5c-113">Miliony deweloperów używają platformy ASP.NET do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-113">Millions of developers have used (and continue to use) ASP.NET to create web apps.</span></span> <span data-ttu-id="6bf5c-114">Platforma ASP.NET Core to przeprojektowana platforma ASP.NET, w której wprowadzono zmiany architektoniczne w celu stworzenia bardziej zwartej i modułowej struktury.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-114">ASP.NET Core is a redesign of ASP.NET, with architectural changes that result in a leaner and modular framework.</span></span>

<span data-ttu-id="6bf5c-115">Platforma ASP. NET Core oferuje następujące zalety:</span><span class="sxs-lookup"><span data-stu-id="6bf5c-115">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="6bf5c-116">Ujednolicony scenariusz na potrzeby tworzenia internetowego interfejsu użytkownika i internetowych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-116">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="6bf5c-117">Integracja [nowoczesnych struktur po stronie klienta](xref:client-side/index) i przepływów pracy programowania.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-117">Integration of [modern client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="6bf5c-118">Gotowy do pracy w chmurze, oparty na środowisku [system konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6bf5c-118">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="6bf5c-119">Wbudowane [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6bf5c-119">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="6bf5c-120">Uproszczony, zapewniający wysoką wydajność, modułowy potok żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-120">A lightweight, high-performance, and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="6bf5c-121">Możliwość hostowania w ramach usług IIS lub samodzielnego hostowania we własnym procesie.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-121">Ability to host on IIS or self-host in your own process.</span></span>
* <span data-ttu-id="6bf5c-122">Możliwość działania na platformie [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), która obsługuje prawdziwe równoległe używanie wielu wersji obok siebie.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-122">Can run on [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), which supports true side-by-side app versioning.</span></span>
* <span data-ttu-id="6bf5c-123">Narzędzia, które upraszczają tworzenie nowoczesnych aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-123">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="6bf5c-124">Możliwość kompilowania i uruchamiania w systemach Windows, macOS i Linux.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-124">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="6bf5c-125">Open source i koncentracja na społeczności.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-125">Open-source and community-focused.</span></span>

<span data-ttu-id="6bf5c-126">Platforma ASP.NET Core jest dostarczana w całości w postaci pakietów [NuGet](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="6bf5c-126">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="6bf5c-127">Dzięki temu można zoptymalizować aplikację w celu uwzględnienia tylko tych pakietów NuGet, które są potrzebne.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-127">This allows you to optimize your app to include just the NuGet packages you need.</span></span> <span data-ttu-id="6bf5c-128">Korzyści z mniejszego obszaru powierzchni aplikacji obejmują: większe bezpieczeństwo, ograniczenie obsługi i lepszą wydajność.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-128">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="6bf5c-129">Tworzenie internetowego interfejsu użytkownika i internetowych interfejsów API przy użyciu wzorca MVC platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6bf5c-129">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="6bf5c-130">Platforma ASP.NET Core MVC udostępnia funkcje, które ułatwiają tworzenie [internetowych interfejsów API](xref:tutorials/index#building-web-apis) i [aplikacji internetowych](xref:tutorials/index#building-web-applications):</span><span class="sxs-lookup"><span data-stu-id="6bf5c-130">ASP.NET Core MVC provides features that help you build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="6bf5c-131">Wzorzec [MVC (Model View Controller)](xref:mvc/overview) ułatwia zapewnienie [możliwości testowania](testing/index.md) aplikacji internetowych i internetowych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-131">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="6bf5c-132">[Strony Razor](xref:mvc/razor-pages/index) (nowość w wersji 2.0) to oparty na stronach model programowania, który umożliwia łatwiejsze i bardziej wydajne tworzenie internetowego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-132">[Razor Pages](xref:mvc/razor-pages/index) (new in 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="6bf5c-133">[Składnia Razor](xref:mvc/views/razor) zapewnia wydajny język dla [stron Razor](xref:mvc/razor-pages/index) i [widoków MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="6bf5c-133">[Razor syntax](xref:mvc/views/razor) provides a productive language for [Razor Pages](xref:mvc/razor-pages/index) and [MVC Views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="6bf5c-134">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-134">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="6bf5c-135">Wbudowana obsługa [wiele formatów danych i negocjacji zawartości](mvc/models/formatting.md) umożliwia internetowym interfejsom API obsługę szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-135">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="6bf5c-136">[Wiązanie modelu](xref:mvc/models/model-binding) automatycznie mapuje dane z żądań HTTP do parametrów metod akcji.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-136">[Model Binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="6bf5c-137">[Walidacja modelu](xref:mvc/models/validation) automatycznie przeprowadza weryfikację (walidację) po stronie klienta i po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-137">[Model Validation](xref:mvc/models/validation) automatically performs client and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="6bf5c-138">Programowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="6bf5c-138">Client-side development</span></span>

<span data-ttu-id="6bf5c-139">Platformę ASP.NET Core zaprojektowano w celu bezproblemowej integracji z rozmaitymi platformami po stronie klienta, w tym [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) i [Bootstrap](xref:client-side/bootstrap).</span><span class="sxs-lookup"><span data-stu-id="6bf5c-139">ASP.NET Core is designed to integrate seamlessly with a variety of client-side frameworks, including [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="6bf5c-140">Zobacz [Programowanie po stronie klienta](client-side/index.md), aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-140">See [Client-side development](client-side/index.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6bf5c-141">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="6bf5c-141">Next steps</span></span>

<span data-ttu-id="6bf5c-142">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="6bf5c-142">For more information, see the following resources:</span></span>

* [<span data-ttu-id="6bf5c-143">Samouczki platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6bf5c-143">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="6bf5c-144">Platforma ASP.NET Core — podstawy</span><span class="sxs-lookup"><span data-stu-id="6bf5c-144">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="6bf5c-145">[Cotygodniowe podsumowanie ASP.NET Community Standup](https://live.asp.net/) obejmuje informacje o postępie i planach zespołu, poleca nowe blogi i oprogramowanie innych firm.</span><span class="sxs-lookup"><span data-stu-id="6bf5c-145">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans and features new blogs and third-party software.</span></span>
