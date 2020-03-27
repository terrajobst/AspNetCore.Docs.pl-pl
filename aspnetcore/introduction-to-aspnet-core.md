---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Zapoznaj się z wprowadzeniem do rozwiązania ASP.NET Core, czyli międzyplatformowej struktury typu open source o wysokiej wydajności służącej do tworzenia nowoczesnych aplikacji internetowych opartych na chmurze.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: index
ms.openlocfilehash: fd7fa9dd70502f51222e457dd887ef668d377278
ms.sourcegitcommit: 4b166b49ec557a03f99f872dd069ca5e56faa524
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366660"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="54de3-103">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54de3-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="54de3-104">[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) i [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="54de3-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="54de3-105">ASP.NET Core to międzyplatformowa struktura typu [open source](https://github.com/aspnet/home) o wysokiej wydajności służąca do tworzenia nowoczesnych aplikacji internetowych opartych na chmurze.</span><span class="sxs-lookup"><span data-stu-id="54de3-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="54de3-106">Platforma ASP.NET Core umożliwia:</span><span class="sxs-lookup"><span data-stu-id="54de3-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="54de3-107">Kompilowanie aplikacji i usług internetowych, aplikacji [IoT](https://www.microsoft.com/internet-of-things/) i zapleczy mobilnych.</span><span class="sxs-lookup"><span data-stu-id="54de3-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="54de3-108">Używanie ulubionych narzędzi programistycznych w systemach Windows, macOS i Linux.</span><span class="sxs-lookup"><span data-stu-id="54de3-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="54de3-109">Wdrażanie w chmurze lub lokalnie.</span><span class="sxs-lookup"><span data-stu-id="54de3-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="54de3-110">Uruchamianie na platformie [.NET Core lub .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="54de3-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-choose-aspnet-core"></a><span data-ttu-id="54de3-111">Dlaczego warto wybrać ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="54de3-111">Why choose ASP.NET Core?</span></span>

<span data-ttu-id="54de3-112">Miliony deweloperów używają lub używały [ASP.NET 4. x](/aspnet/overview) do tworzenia aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="54de3-112">Millions of developers use or have used [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="54de3-113">ASP.NET Core to przeprojektowana platforma ASP.NET 4.x, w której wprowadzono zmiany architektoniczne w celu stworzenia bardziej zwartej i modułowej struktury.</span><span class="sxs-lookup"><span data-stu-id="54de3-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="54de3-114">Tworzenie internetowego interfejsu użytkownika i internetowych interfejsów API przy użyciu wzorca MVC platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54de3-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="54de3-115">Platforma ASP.NET Core MVC udostępnia funkcje, które umożliwiają tworzenie [internetowych interfejsów API](xref:tutorials/first-web-api) i [aplikacji internetowych](xref:tutorials/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="54de3-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="54de3-116">Wzorzec [MVC (Model View Controller)](xref:mvc/overview) ułatwia zapewnienie możliwości testowania aplikacji internetowych i internetowych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="54de3-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="54de3-117">[Razor Pages](xref:razor-pages/index) to oparty na stronach model programowania, który umożliwia łatwiejsze i bardziej wydajne tworzenie internetowego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="54de3-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="54de3-118">[Składnia Razor](xref:mvc/views/razor) zapewnia wydajny język dla [stron Razor](xref:razor-pages/index) i [widoków MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="54de3-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="54de3-119">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="54de3-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="54de3-120">Wbudowana obsługa [wiele formatów danych i negocjacji zawartości](xref:web-api/advanced/formatting) umożliwia internetowym interfejsom API obsługę szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="54de3-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="54de3-121">[Powiązanie modelu](xref:mvc/models/model-binding) automatycznie mapuje dane z żądań HTTP na parametry metod akcji.</span><span class="sxs-lookup"><span data-stu-id="54de3-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="54de3-122">[Walidacja modelu](xref:mvc/models/validation) automatycznie przeprowadza walidację po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="54de3-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="54de3-123">Programowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="54de3-123">Client-side development</span></span>

<span data-ttu-id="54de3-124">ASP.NET Core zapewnia bezproblemową integrację z popularnymi strukturami i bibliotekami po stronie klienta, takimi jak [Blazor](xref:blazor/index), [kątowy](xref:spa/angular), [reagowanie](xref:spa/react)i [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="54de3-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="54de3-125">Aby uzyskać więcej informacji, zobacz <xref:blazor/index> i Tematy pokrewne w obszarze *programowanie po stronie klienta*.</span><span class="sxs-lookup"><span data-stu-id="54de3-125">For more information, see <xref:blazor/index> and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="54de3-126">Platforma ASP.NET Core ukierunkowana na platformę .NET Framework</span><span class="sxs-lookup"><span data-stu-id="54de3-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="54de3-127">Platforma ASP.NET Core 2.x może jako cel mieć platformę .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="54de3-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="54de3-128">Aplikacje platformy ASP.NET Core ukierunkowane na platformę .NET Framework nie są wieloplatformowe &mdash; działają tylko w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="54de3-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="54de3-129">Ogólnie rzecz biorąc, platforma ASP.NET Core 2.x jest zbudowana z bibliotek [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="54de3-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="54de3-130">Biblioteki z .NET Standard 2,0 są uruchamiane na dowolnej [platformie .NET implementującej .NET Standard 2,0](/dotnet/standard/net-standard#net-implementation-support).</span><span class="sxs-lookup"><span data-stu-id="54de3-130">Libraries written with .NET Standard 2.0 run on any [.NET platform that implements .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span></span>

<span data-ttu-id="54de3-131">ASP.NET Core 2. x jest obsługiwana w wersjach .NET Framework, które implementują .NET Standard 2,0:</span><span class="sxs-lookup"><span data-stu-id="54de3-131">ASP.NET Core 2.x is supported on .NET Framework versions that implement .NET Standard 2.0:</span></span>

* <span data-ttu-id="54de3-132">.NET Framework Najnowsza wersja jest zdecydowanie zalecana.</span><span class="sxs-lookup"><span data-stu-id="54de3-132">.NET Framework latest version is strongly recommended.</span></span>
* <span data-ttu-id="54de3-133">Platforma .NET Framework 4.6.1 lub nowsza.</span><span class="sxs-lookup"><span data-stu-id="54de3-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="54de3-134">Platforma ASP.NET Core 3.0 i nowsze wersje będą działać tylko na platformie .NET Core.</span><span class="sxs-lookup"><span data-stu-id="54de3-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="54de3-135">Aby uzyskać więcej informacji o tej zmianie, zobacz [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (Pierwsze spojrzenie na zmiany wprowadzane na platformie ASP.NET Core 3.0).</span><span class="sxs-lookup"><span data-stu-id="54de3-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="54de3-136">Jest kilka zalet przyjmowania platformy .NET Core jako docelowej, a ich liczba rośnie z każdym wydaniem.</span><span class="sxs-lookup"><span data-stu-id="54de3-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="54de3-137">Niektóre z zalet platformy .NET Core nad platformą .NET Framework to:</span><span class="sxs-lookup"><span data-stu-id="54de3-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="54de3-138">Wieloplatformowość.</span><span class="sxs-lookup"><span data-stu-id="54de3-138">Cross-platform.</span></span> <span data-ttu-id="54de3-139">Działa w systemach macOS, Linux i Windows.</span><span class="sxs-lookup"><span data-stu-id="54de3-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="54de3-140">Większa wydajność</span><span class="sxs-lookup"><span data-stu-id="54de3-140">Improved performance</span></span>
* [<span data-ttu-id="54de3-141">Przechowywanie wersji równoległych</span><span class="sxs-lookup"><span data-stu-id="54de3-141">Side-by-side versioning</span></span>](/dotnet/standard/choosing-core-framework-server#a-need-for-side-by-side-of-net-versions-per-application-level)
* <span data-ttu-id="54de3-142">Nowe interfejsy API</span><span class="sxs-lookup"><span data-stu-id="54de3-142">New APIs</span></span>
* <span data-ttu-id="54de3-143">Kod open source</span><span class="sxs-lookup"><span data-stu-id="54de3-143">Open source</span></span>

<span data-ttu-id="54de3-144">Ciężko pracujemy nad zlikwidowaniem rozbieżności między interfejsami API platform .NET Framework i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="54de3-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="54de3-145">Pakiet [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) udostępnił tysiące interfejsów API specyficznych dla systemu Windows na platformie .NET Core.</span><span class="sxs-lookup"><span data-stu-id="54de3-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="54de3-146">Te interfejsy API nie były dostępne na platformie .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="54de3-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="54de3-147">Zalecana ścieżka szkoleniowa</span><span class="sxs-lookup"><span data-stu-id="54de3-147">Recommended learning path</span></span>

<span data-ttu-id="54de3-148">Przy rozpoczynaniu programowania aplikacji platformy ASP.NET Core zalecamy następujący zestaw samouczków i artykułów:</span><span class="sxs-lookup"><span data-stu-id="54de3-148">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="54de3-149">Wybierz samouczek odpowiedni dla typu aplikacji, którą chcesz programować lub konserwować:</span><span class="sxs-lookup"><span data-stu-id="54de3-149">Follow a tutorial for the type of app you want to develop or maintain:</span></span>

   |<span data-ttu-id="54de3-150">Typ aplikacji</span><span class="sxs-lookup"><span data-stu-id="54de3-150">App type</span></span>  |<span data-ttu-id="54de3-151">Scenariusz</span><span class="sxs-lookup"><span data-stu-id="54de3-151">Scenario</span></span>  |<span data-ttu-id="54de3-152">Samouczek</span><span class="sxs-lookup"><span data-stu-id="54de3-152">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="54de3-153">Aplikacja internetowa</span><span class="sxs-lookup"><span data-stu-id="54de3-153">Web app</span></span>                   | <span data-ttu-id="54de3-154">Programowanie od nowa</span><span class="sxs-lookup"><span data-stu-id="54de3-154">For new development</span></span>        |[<span data-ttu-id="54de3-155">Wprowadzenie do korzystania ze stron Razor</span><span class="sxs-lookup"><span data-stu-id="54de3-155">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="54de3-156">Aplikacja internetowa</span><span class="sxs-lookup"><span data-stu-id="54de3-156">Web app</span></span>                   | <span data-ttu-id="54de3-157">Konserwacja aplikacji MVC</span><span class="sxs-lookup"><span data-stu-id="54de3-157">For maintaining an MVC app</span></span> |[<span data-ttu-id="54de3-158">Wprowadzenie do wzorca MVC</span><span class="sxs-lookup"><span data-stu-id="54de3-158">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="54de3-159">Interfejs API sieci Web</span><span class="sxs-lookup"><span data-stu-id="54de3-159">Web API</span></span>                   |                            |<span data-ttu-id="54de3-160">[Tworzenie internetowego interfejsu API](xref:tutorials/first-web-api)\*</span><span class="sxs-lookup"><span data-stu-id="54de3-160">[Create a web API](xref:tutorials/first-web-api)\*</span></span>  |
   |<span data-ttu-id="54de3-161">Aplikacja czasu rzeczywistego</span><span class="sxs-lookup"><span data-stu-id="54de3-161">Real-time app</span></span>             |                            |[<span data-ttu-id="54de3-162">Wprowadzenie do usługi SignalR</span><span class="sxs-lookup"><span data-stu-id="54de3-162">Get started with SignalR</span></span>](xref:tutorials/signalr) |
   |<span data-ttu-id="54de3-163">Aplikacja Blazor</span><span class="sxs-lookup"><span data-stu-id="54de3-163">Blazor app</span></span>                |                            |[<span data-ttu-id="54de3-164">Wprowadzenie do Blazor</span><span class="sxs-lookup"><span data-stu-id="54de3-164">Get started with Blazor</span></span>](xref:blazor/get-started) |
   |<span data-ttu-id="54de3-165">Aplikacja zdalnego wywołania procedury</span><span class="sxs-lookup"><span data-stu-id="54de3-165">Remote Procedure Call app</span></span> |                            |[<span data-ttu-id="54de3-166">Wprowadzenie do usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="54de3-166">Get started with a gRPC service</span></span>](xref:tutorials/grpc/grpc-start) |

1. <span data-ttu-id="54de3-167">Wybierz samouczek pokazujący, jak uzyskiwać podstawowy dostęp do danych:</span><span class="sxs-lookup"><span data-stu-id="54de3-167">Follow a tutorial that shows how to do basic data access:</span></span>

   |<span data-ttu-id="54de3-168">Scenariusz</span><span class="sxs-lookup"><span data-stu-id="54de3-168">Scenario</span></span>  |<span data-ttu-id="54de3-169">Samouczek</span><span class="sxs-lookup"><span data-stu-id="54de3-169">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="54de3-170">Programowanie od nowa</span><span class="sxs-lookup"><span data-stu-id="54de3-170">For new development</span></span>        |[<span data-ttu-id="54de3-171">Platforma Razor Pages z platformą Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="54de3-171">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="54de3-172">Konserwacja aplikacji MVC</span><span class="sxs-lookup"><span data-stu-id="54de3-172">For maintaining an MVC app</span></span> |[<span data-ttu-id="54de3-173">Wzorzec MVC z platformą Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="54de3-173">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

1. <span data-ttu-id="54de3-174">Zapoznaj się z omówieniem funkcji platformy ASP.NET Core, które mają zastosowanie do wszystkich typów aplikacji:</span><span class="sxs-lookup"><span data-stu-id="54de3-174">Read an overview of ASP.NET Core features that apply to all app types:</span></span>

   * [<span data-ttu-id="54de3-175">Podstawowe założenia</span><span class="sxs-lookup"><span data-stu-id="54de3-175">Fundamentals</span></span>](xref:fundamentals/index)

1. <span data-ttu-id="54de3-176">Przeglądaj spis treści, aby znaleźć inne interesujące tematy.</span><span class="sxs-lookup"><span data-stu-id="54de3-176">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="54de3-177">\* Jest już dostępny nowy [samouczek internetowego interfejsu API, który można przejść w całości w przeglądarce](https://docs.microsoft.com/learn/modules/build-web-api-net-core), bez konieczności instalowania lokalnego środowiska IDE.</span><span class="sxs-lookup"><span data-stu-id="54de3-177">\* There is a new [web API tutorial that you follow entirely in the browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span>  <span data-ttu-id="54de3-178">Kod jest wykonywany w usłudze [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), a do testowania służy narzędzie [curl](https://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="54de3-178">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="migration-from-the-net-framework"></a><span data-ttu-id="54de3-179">Migracja z .NET Framework</span><span class="sxs-lookup"><span data-stu-id="54de3-179">Migration from the .NET Framework</span></span>

<span data-ttu-id="54de3-180">Przewodnik referencyjny dotyczący migrowania aplikacji ASP.NET do ASP.NET Core można znaleźć w temacie <xref:migration/proper-to-2x/index>.</span><span class="sxs-lookup"><span data-stu-id="54de3-180">For a reference guide to migrating ASP.NET apps to ASP.NET Core, see <xref:migration/proper-to-2x/index>.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="54de3-181">Instrukcje: pobieranie pliku</span><span class="sxs-lookup"><span data-stu-id="54de3-181">How to download a sample</span></span>

<span data-ttu-id="54de3-182">Wiele artykułów i samouczków zawiera linki do kodu przykładowego.</span><span class="sxs-lookup"><span data-stu-id="54de3-182">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="54de3-183">[Pobierz plik zip repozytorium ASP.NET](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="54de3-183">[Download the ASP.NET repository zip file](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master).</span></span>
1. <span data-ttu-id="54de3-184">Rozpakuj plik *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="54de3-184">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="54de3-185">Adres URL linku do przykładu pomoże Ci przejść do katalogu przykładu.</span><span class="sxs-lookup"><span data-stu-id="54de3-185">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="54de3-186">Dyrektywy preprocesora w przykładowym kodzie</span><span class="sxs-lookup"><span data-stu-id="54de3-186">Preprocessor directives in sample code</span></span>

<span data-ttu-id="54de3-187">Aby przedstawić wiele scenariuszy, przykładowe aplikacje używają dyrektyw preprocesora `#define` i `#if-#else/#elif-#endif`, aby wybiórczo kompilować i uruchamiać różne sekcje przykładowego kodu.</span><span class="sxs-lookup"><span data-stu-id="54de3-187">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` preprocessor directives to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="54de3-188">Dla tych przykładów, które korzystają z tego podejścia, ustaw `#define` dyrektywę w górnej części C# plików, aby zdefiniować symbol skojarzony z scenariuszem, który chcesz uruchomić.</span><span class="sxs-lookup"><span data-stu-id="54de3-188">For those samples that make use of this approach, set the `#define` directive at the top of the C# files to define the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="54de3-189">Niektóre przykłady wymagają zdefiniowania symbolu w górnej części wielu plików, aby można było uruchomić scenariusz.</span><span class="sxs-lookup"><span data-stu-id="54de3-189">Some samples require defining the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="54de3-190">Na przykład następująca lista symboli `#define` wskazuje, że są dostępne cztery scenariusze (jeden scenariusz na symbol).</span><span class="sxs-lookup"><span data-stu-id="54de3-190">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="54de3-191">Aktualna konfiguracja przykładu powoduje uruchomienie scenariusza `TemplateCode`:</span><span class="sxs-lookup"><span data-stu-id="54de3-191">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="54de3-192">Aby zmienić przykład w celu uruchomienia scenariusza `ExpandDefault`, zdefiniuj symbol `ExpandDefault` i pozostaw pozostałe symbole jako przekształcone w komentarz:</span><span class="sxs-lookup"><span data-stu-id="54de3-192">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="54de3-193">Więcej informacji na temat używania [ dyrektyw preprocesora języka C#](/dotnet/csharp/language-reference/preprocessor-directives/) do selektywnego kompilowania sekcji kodu można znaleźć w tematach [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) (#define (odwołanie w języku C#)) i [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if) (#if (odwołanie w języku C#)).</span><span class="sxs-lookup"><span data-stu-id="54de3-193">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="54de3-194">Regiony w przykładowym kodzie</span><span class="sxs-lookup"><span data-stu-id="54de3-194">Regions in sample code</span></span>

<span data-ttu-id="54de3-195">Niektóre przykładowe aplikacje zawierają sekcje kodu otoczone dyrektywami [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) i [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# .</span><span class="sxs-lookup"><span data-stu-id="54de3-195">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# directives.</span></span> <span data-ttu-id="54de3-196">System tworzenia dokumentacji wstawia te regiony do renderowanych tematów dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="54de3-196">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="54de3-197">Nazwy regionów zwykle zawierają wyraz „snippet”.</span><span class="sxs-lookup"><span data-stu-id="54de3-197">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="54de3-198">W poniższym przykładzie pokazano region o nazwie `snippet_WebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="54de3-198">The following example shows a region named `snippet_WebHostDefaults`:</span></span>

```csharp
#region snippet_WebHostDefaults
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
    });
#endregion
```

<span data-ttu-id="54de3-199">Wcześniejszy fragment kodu w języku C# jest przywoływany w pliku markdown tematu za pomocą następującego wiersza:</span><span class="sxs-lookup"><span data-stu-id="54de3-199">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_WebHostDefaults)]
```

<span data-ttu-id="54de3-200">Licencjobiorca może bezpiecznie ignorować (lub usuwać) `#region` i `#endregion` dyrektyw otaczających kod.</span><span class="sxs-lookup"><span data-stu-id="54de3-200">You may safely ignore (or remove) the `#region` and `#endregion` directives that surround the code.</span></span> <span data-ttu-id="54de3-201">Nie zmieniaj kodu w ramach tych dyrektyw, jeśli planujesz uruchamiać przykładowe scenariusze opisane w temacie.</span><span class="sxs-lookup"><span data-stu-id="54de3-201">Don't alter the code within these directives if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="54de3-202">Kod możesz swobodnie modyfikować, eksperymentując z innymi scenariuszami.</span><span class="sxs-lookup"><span data-stu-id="54de3-202">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="54de3-203">Aby uzyskać więcej informacji, zobacz [Współtworzenie dokumentacji platformy ASP.NET: fragmenty kodu](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="54de3-203">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="54de3-204">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="54de3-204">Next steps</span></span>

<span data-ttu-id="54de3-205">Więcej informacji zawierają następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="54de3-205">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="54de3-206">Platforma ASP.NET Core — podstawy</span><span class="sxs-lookup"><span data-stu-id="54de3-206">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="54de3-207">[Cotygodniowe podsumowanie ASP.NET Community Standup](https://live.asp.net/) zawiera aktualności o postępach i planach zespołu.</span><span class="sxs-lookup"><span data-stu-id="54de3-207">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="54de3-208">Zawiera też informacje o polecanych blogach i oprogramowaniu innych firm.</span><span class="sxs-lookup"><span data-stu-id="54de3-208">It features new blogs and third-party software.</span></span>
