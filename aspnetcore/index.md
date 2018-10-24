---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Zapoznaj się z wprowadzeniem do rozwiązania ASP.NET Core, czyli międzyplatformowej struktury typu open source o wysokiej wydajności służącej do tworzenia nowoczesnych aplikacji internetowych opartych na chmurze.
ms.author: riande
ms.date: 9/28/2018
uid: index
ms.openlocfilehash: 69ab702e9d9f8d746b7bc546d4f2bbb831ff59c7
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911706"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="ed390-103">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed390-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="ed390-104">[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) i [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="ed390-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="ed390-105">ASP.NET Core to międzyplatformowa struktura typu [open source](https://github.com/aspnet/home) o wysokiej wydajności służąca do tworzenia nowoczesnych aplikacji internetowych opartych na chmurze.</span><span class="sxs-lookup"><span data-stu-id="ed390-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="ed390-106">Platforma ASP.NET Core umożliwia:</span><span class="sxs-lookup"><span data-stu-id="ed390-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="ed390-107">Kompilowanie aplikacji i usług internetowych, aplikacji [IoT](https://www.microsoft.com/internet-of-things/) i zapleczy mobilnych.</span><span class="sxs-lookup"><span data-stu-id="ed390-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="ed390-108">Używanie ulubionych narzędzi programistycznych w systemach Windows, macOS i Linux.</span><span class="sxs-lookup"><span data-stu-id="ed390-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="ed390-109">Wdrażanie w chmurze lub lokalnie.</span><span class="sxs-lookup"><span data-stu-id="ed390-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="ed390-110">Uruchamianie na platformie [.NET Core lub .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="ed390-110">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="ed390-111">Dlaczego warto korzystać z platformy ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="ed390-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="ed390-112">Miliony deweloperów korzystają z platformy [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="ed390-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="ed390-113">ASP.NET Core to przeprojektowana platforma ASP.NET 4.x, w której wprowadzono zmiany architektoniczne w celu stworzenia bardziej zwartej i modułowej struktury.</span><span class="sxs-lookup"><span data-stu-id="ed390-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="ed390-114">Tworzenie internetowego interfejsu użytkownika i internetowych interfejsów API przy użyciu wzorca MVC platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed390-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="ed390-115">Platforma ASP.NET Core MVC udostępnia funkcje, które umożliwiają tworzenie [internetowych interfejsów API](xref:tutorials/index#build-web-apis) i [aplikacji internetowych](xref:tutorials/index#build-web-apps):</span><span class="sxs-lookup"><span data-stu-id="ed390-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#build-web-apis) and [web apps](xref:tutorials/index#build-web-apps):</span></span>

* <span data-ttu-id="ed390-116">Wzorzec [MVC (Model View Controller)](xref:mvc/overview) ułatwia zapewnienie [możliwości testowania](xref:test/index) aplikacji internetowych i internetowych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="ed390-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](xref:test/index).</span></span>
* <span data-ttu-id="ed390-117">[Strony Razor](xref:razor-pages/index) (nowość w ASP.NET Core 2.0) to oparty na stronach model programowania, który umożliwia łatwiejsze i bardziej wydajne tworzenie internetowego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ed390-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="ed390-118">[Składnia Razor](xref:mvc/views/razor) zapewnia wydajny język dla [stron Razor](xref:razor-pages/index) i [widoków MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="ed390-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="ed390-119">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="ed390-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="ed390-120">Wbudowana obsługa [wiele formatów danych i negocjacji zawartości](xref:web-api/advanced/formatting) umożliwia internetowym interfejsom API obsługę szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="ed390-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="ed390-121">[Powiązanie modelu](xref:mvc/models/model-binding) automatycznie mapuje dane z żądań HTTP na parametry metod akcji.</span><span class="sxs-lookup"><span data-stu-id="ed390-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="ed390-122">[Walidacja modelu](xref:mvc/models/validation) automatycznie przeprowadza walidację po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="ed390-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="ed390-123">Programowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="ed390-123">Client-side development</span></span>

<span data-ttu-id="ed390-124">Platforma ASP.NET Core bezproblemowo integruje się z popularnymi strukturami i bibliotekami po stronie klienta, takimi jak [Angular](xref:spa/angular), [React](xref:spa/react) czy [Bootstrap](xref:client-side/bootstrap).</span><span class="sxs-lookup"><span data-stu-id="ed390-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="ed390-125">Aby uzyskać więcej informacji, zobacz [Programowanie po stronie klienta](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="ed390-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="ed390-126">Platforma ASP.NET Core ukierunkowana na platformę .NET Framework</span><span class="sxs-lookup"><span data-stu-id="ed390-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="ed390-127">Platforma ASP.NET Core może jako cel mieć platformę .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ed390-127">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="ed390-128">Aplikacje platformy ASP.NET Core ukierunkowane na platformę .NET Framework nie są wieloplatformowe &mdash; działają tylko w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="ed390-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="ed390-129">Nie planuje się usunięcia z platformy ASP.NET Core obsługi przyjmowania jako celu platformy .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ed390-129">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="ed390-130">Ogólnie rzecz biorąc, platforma ASP.NET Core zbudowana jest z bibliotek [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="ed390-130">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="ed390-131">Aplikacje napisane przy użyciu platformy .NET Standard 2.0 działają wszędzie tam, gdzie obsługiwana jest platforma .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="ed390-131">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="ed390-132">Platforma ASP.NET Core 2.x jest obsługiwana w wersjach platformy .NET Framework zgodnych z platformą .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="ed390-132">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="ed390-133">Zdecydowanie zaleca się platformę .NET Framework 4.7.1 lub nowszą.</span><span class="sxs-lookup"><span data-stu-id="ed390-133">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="ed390-134">Platforma .NET Framework 4.6.1 lub nowsza.</span><span class="sxs-lookup"><span data-stu-id="ed390-134">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="ed390-135">Jest kilka zalet przyjmowania platformy .NET Core jako docelowej, a ich liczba rośnie z każdym wydaniem.</span><span class="sxs-lookup"><span data-stu-id="ed390-135">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="ed390-136">Niektóre z zalet platformy .NET Core nad platformą .NET Framework to:</span><span class="sxs-lookup"><span data-stu-id="ed390-136">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="ed390-137">Wieloplatformowość.</span><span class="sxs-lookup"><span data-stu-id="ed390-137">Cross-platform.</span></span> <span data-ttu-id="ed390-138">Działa w systemach macOS, Linux i Windows.</span><span class="sxs-lookup"><span data-stu-id="ed390-138">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="ed390-139">Większa wydajność</span><span class="sxs-lookup"><span data-stu-id="ed390-139">Improved performance</span></span>
* <span data-ttu-id="ed390-140">Przechowywanie wersji obok siebie</span><span class="sxs-lookup"><span data-stu-id="ed390-140">Side-by-side versioning</span></span>
* <span data-ttu-id="ed390-141">Nowe interfejsy API</span><span class="sxs-lookup"><span data-stu-id="ed390-141">New APIs</span></span>
* <span data-ttu-id="ed390-142">Kod open source</span><span class="sxs-lookup"><span data-stu-id="ed390-142">Open source</span></span>

<span data-ttu-id="ed390-143">Ciężko pracujemy nad zlikwidowaniem rozbieżności między interfejsami API platform .NET Framework i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed390-143">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="ed390-144">Pakiet [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) udostępnił tysiące interfejsów API specyficznych dla systemu Windows na platformie .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed390-144">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="ed390-145">Te interfejsy API nie były dostępne na platformie .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="ed390-145">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed390-146">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ed390-146">Next steps</span></span>

<span data-ttu-id="ed390-147">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="ed390-147">For more information, see the following resources:</span></span>

* [<span data-ttu-id="ed390-148">Wprowadzenie do korzystania ze stron Razor</span><span class="sxs-lookup"><span data-stu-id="ed390-148">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="ed390-149">Samouczki platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed390-149">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="ed390-150">Platforma ASP.NET Core — podstawy</span><span class="sxs-lookup"><span data-stu-id="ed390-150">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="ed390-151">[Cotygodniowe podsumowanie ASP.NET Community Standup](https://live.asp.net/) zawiera aktualności o postępach i planach zespołu.</span><span class="sxs-lookup"><span data-stu-id="ed390-151">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="ed390-152">Zawiera też informacje o polecanych blogach i oprogramowaniu innych firm.</span><span class="sxs-lookup"><span data-stu-id="ed390-152">It features new blogs and third-party software.</span></span>
