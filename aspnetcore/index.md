---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Zapoznaj się z wprowadzeniem do rozwiązania ASP.NET Core, czyli międzyplatformowej struktury typu open source o wysokiej wydajności służącej do tworzenia nowoczesnych aplikacji internetowych opartych na chmurze.
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: index
ms.openlocfilehash: e7c81ff82e5206a5aca217417f6cb1c339d72e89
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341410"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="4c946-103">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c946-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="4c946-104">[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) i [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="4c946-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="4c946-105">ASP.NET Core to międzyplatformowa struktura typu [open source](https://github.com/aspnet/home) o wysokiej wydajności służąca do tworzenia nowoczesnych aplikacji internetowych opartych na chmurze.</span><span class="sxs-lookup"><span data-stu-id="4c946-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="4c946-106">Platforma ASP.NET Core umożliwia:</span><span class="sxs-lookup"><span data-stu-id="4c946-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="4c946-107">Kompilowanie aplikacji i usług internetowych, aplikacji [IoT](https://www.microsoft.com/internet-of-things/) i zapleczy mobilnych.</span><span class="sxs-lookup"><span data-stu-id="4c946-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="4c946-108">Używanie ulubionych narzędzi programistycznych w systemach Windows, macOS i Linux.</span><span class="sxs-lookup"><span data-stu-id="4c946-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="4c946-109">Wdrażanie w chmurze lub lokalnie.</span><span class="sxs-lookup"><span data-stu-id="4c946-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="4c946-110">Uruchamianie aplikacji na platformie [.NET Core lub .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="4c946-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="4c946-111">Dlaczego warto korzystać z platformy ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="4c946-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="4c946-112">Miliony deweloperów korzystają z platformy [ASP.NET 4.x](/aspnet/overview) do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="4c946-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="4c946-113">ASP.NET Core to przeprojektowana platforma ASP.NET 4.x, w której wprowadzono zmiany architektoniczne w celu stworzenia bardziej zwartej i modułowej struktury.</span><span class="sxs-lookup"><span data-stu-id="4c946-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="4c946-114">Tworzenie internetowego interfejsu użytkownika i internetowych interfejsów API przy użyciu wzorca MVC platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c946-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="4c946-115">Platforma ASP.NET Core MVC udostępnia funkcje, które umożliwiają tworzenie [internetowych interfejsów API](xref:tutorials/first-web-api) i [aplikacji](xref:tutorials/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="4c946-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="4c946-116">Wzorzec [MVC (Model View Controller)](xref:mvc/overview) umożliwia testowanie internetowych interfejsów API i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4c946-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="4c946-117">[Razor Pages](xref:razor-pages/index) to oparty na stronach model programowania, który umożliwia łatwiejsze i bardziej wydajne tworzenie internetowego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4c946-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="4c946-118">[Składnia Razor](xref:mvc/views/razor) zapewnia wydajny język dla [stron Razor](xref:razor-pages/index) i [widoków MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="4c946-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="4c946-119">Specjalne funkcje pomocnicze (tzw. [pomocnicy tagów](xref:mvc/views/tag-helpers/intro)) umożliwiają tworzenie i renderowanie elementów HTML w plikach Razor z użyciem kodu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="4c946-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="4c946-120">Wbudowana obsługa [wielu formatów danych i negocjacji zawartości](xref:web-api/advanced/formatting) umożliwia internetowym interfejsom API nawiązywanie połączeń z szeroką gamą klientów (dotyczy to m.in. przeglądarek i urządzeń przenośnych).</span><span class="sxs-lookup"><span data-stu-id="4c946-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="4c946-121">[Powiązanie modelu](xref:mvc/models/model-binding) automatycznie mapuje dane z żądań HTTP na parametry metod akcji.</span><span class="sxs-lookup"><span data-stu-id="4c946-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="4c946-122">[Walidacja modelu](xref:mvc/models/validation) automatycznie przeprowadza walidację po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="4c946-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="4c946-123">Programowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="4c946-123">Client-side development</span></span>

<span data-ttu-id="4c946-124">Platforma ASP.NET Core bezproblemowo integruje się z popularnymi strukturami i bibliotekami po stronie klienta, takimi jak [Angular](xref:spa/angular), [React](xref:spa/react) czy [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="4c946-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="4c946-125">Aby uzyskać więcej informacji, zobacz [Programowanie po stronie klienta](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="4c946-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="4c946-126">Platforma ASP.NET Core ukierunkowana na .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4c946-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="4c946-127">Platforma ASP.NET Core 2.x umożliwia obsługę aplikacji dedykowanych rozwiązaniom .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4c946-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="4c946-128">Aplikacje platformy ASP.NET Core ukierunkowane na rozwiązanie .NET Framework nie są wieloplatformowe &mdash; działają tylko w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="4c946-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="4c946-129">Ogólnie rzecz biorąc, platforma ASP.NET Core 2.x jest zbudowana z bibliotek [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="4c946-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="4c946-130">Aplikacje napisane przy użyciu platformy .NET Standard 2.0 działają wszędzie tam, gdzie obsługiwana jest platforma .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="4c946-130">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="4c946-131">Platforma ASP.NET Core 2.x jest obsługiwana w wersjach .NET Framework zgodnych z .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="4c946-131">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="4c946-132">Zdecydowanie zaleca się wersję .NET Framework 4.7.1 lub nowszą.</span><span class="sxs-lookup"><span data-stu-id="4c946-132">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="4c946-133">Platforma .NET Framework 4.6.1 lub nowsza.</span><span class="sxs-lookup"><span data-stu-id="4c946-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="4c946-134">Platforma ASP.NET Core 3.0 i nowsze wersje będą działać tylko na platformie .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c946-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="4c946-135">Aby uzyskać więcej informacji o tej zmianie, zobacz [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (Pierwsze spojrzenie na zmiany wprowadzane na platformie ASP.NET Core 3.0).</span><span class="sxs-lookup"><span data-stu-id="4c946-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="4c946-136">Tworzenie rozwiązań ukierunkowanych na .NET Core jest korzystne z wielu względów, a przewaga tej platformy rośnie z każdym wydaniem.</span><span class="sxs-lookup"><span data-stu-id="4c946-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="4c946-137">Oto niektóre z czynników wpływających na przewagę platformy .NET Core nad .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="4c946-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="4c946-138">Wieloplatformowość.</span><span class="sxs-lookup"><span data-stu-id="4c946-138">Cross-platform.</span></span> <span data-ttu-id="4c946-139">Działanie w systemach macOS, Linux i Windows.</span><span class="sxs-lookup"><span data-stu-id="4c946-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="4c946-140">Większa wydajność</span><span class="sxs-lookup"><span data-stu-id="4c946-140">Improved performance</span></span>
* <span data-ttu-id="4c946-141">Równoległa obsługa różnych wersji</span><span class="sxs-lookup"><span data-stu-id="4c946-141">Side-by-side versioning</span></span>
* <span data-ttu-id="4c946-142">Nowe interfejsy API</span><span class="sxs-lookup"><span data-stu-id="4c946-142">New APIs</span></span>
* <span data-ttu-id="4c946-143">Kod open source</span><span class="sxs-lookup"><span data-stu-id="4c946-143">Open source</span></span>

<span data-ttu-id="4c946-144">Ciężko pracujemy nad zlikwidowaniem rozbieżności między interfejsami API platform .NET Framework i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c946-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="4c946-145">Pakiet [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) udostępnił tysiące interfejsów API specyficznych dla systemu Windows na platformie .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c946-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="4c946-146">Te interfejsy API nie były dostępne na platformie .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="4c946-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="4c946-147">Instrukcje: pobieranie pliku</span><span class="sxs-lookup"><span data-stu-id="4c946-147">How to download a sample</span></span>

<span data-ttu-id="4c946-148">Wiele artykułów i samouczków zawiera linki do przykładów kodu.</span><span class="sxs-lookup"><span data-stu-id="4c946-148">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="4c946-149">[Pobierz plik zip repozytorium ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="4c946-149">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="4c946-150">Rozpakuj plik *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="4c946-150">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="4c946-151">Przejdź do katalogu przykładu, korzystając z adresu URL linku przykładu.</span><span class="sxs-lookup"><span data-stu-id="4c946-151">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="4c946-152">Dyrektywy preprocesora w przykładowym kodzie</span><span class="sxs-lookup"><span data-stu-id="4c946-152">Preprocessor directives in sample code</span></span>

<span data-ttu-id="4c946-153">Aby zademonstrować wiele scenariuszy, przykładowe aplikacje używają instrukcji `#define` i `#if-#else/#elif-#endif` języka C# do selektywnego kompilowania i uruchamiania różnych sekcji przykładowego kodu.</span><span class="sxs-lookup"><span data-stu-id="4c946-153">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="4c946-154">Jeśli chcesz skorzystać z tej możliwości w tych przykładach, ustaw instrukcję `#define` w górnej części plików języka C# na symbol skojarzony ze scenariuszem, który chcesz uruchomić.</span><span class="sxs-lookup"><span data-stu-id="4c946-154">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="4c946-155">Niektóre przykłady mogą wymagać ustawienia symbolu w górnej części wielu plików, aby umożliwić uruchomienie scenariusza.</span><span class="sxs-lookup"><span data-stu-id="4c946-155">Some samples require setting the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="4c946-156">Na przykład następująca lista symboli `#define` wskazuje, że są dostępne cztery scenariusze (jeden scenariusz na symbol).</span><span class="sxs-lookup"><span data-stu-id="4c946-156">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="4c946-157">Aktualna konfiguracja przykładu powoduje uruchomienie scenariusza `TemplateCode`:</span><span class="sxs-lookup"><span data-stu-id="4c946-157">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="4c946-158">Aby zmienić przykład w celu uruchomienia scenariusza `ExpandDefault`, zdefiniuj symbol `ExpandDefault` i pozostaw pozostałe symbole jako przekształcone w komentarz:</span><span class="sxs-lookup"><span data-stu-id="4c946-158">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="4c946-159">Więcej informacji na temat używania [ dyrektyw preprocesora języka C#](/dotnet/csharp/language-reference/preprocessor-directives/) do selektywnego kompilowania sekcji kodu można znaleźć w tematach [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) (#define (dokumentacja języka C#)) i [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if) (#if (dokumentacja języka C#)).</span><span class="sxs-lookup"><span data-stu-id="4c946-159">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="4c946-160">Regiony w przykładowym kodzie</span><span class="sxs-lookup"><span data-stu-id="4c946-160">Regions in sample code</span></span>

<span data-ttu-id="4c946-161">Niektóre przykładowe aplikacje zawierają sekcje kodu ujęte w instrukcje [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) i [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) języka C#.</span><span class="sxs-lookup"><span data-stu-id="4c946-161">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# statements.</span></span> <span data-ttu-id="4c946-162">System tworzenia dokumentacji wstawia te regiony do renderowanych tematów dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="4c946-162">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="4c946-163">Nazwy regionów zwykle zawierają wyraz „snippet”.</span><span class="sxs-lookup"><span data-stu-id="4c946-163">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="4c946-164">W poniższym przykładzie pokazano region o nazwie `snippet_FilterInCode`:</span><span class="sxs-lookup"><span data-stu-id="4c946-164">The following example shows a region named `snippet_FilterInCode`:</span></span>

```csharp
#region snippet_FilterInCode
WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
        logging.AddFilter("System", LogLevel.Debug)
            .AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Trace))
            .Build();
#endregion
```

<span data-ttu-id="4c946-165">Wcześniejszy fragment kodu w języku C# jest przywoływany w pliku markdown tematu za pomocą następującego wiersza:</span><span class="sxs-lookup"><span data-stu-id="4c946-165">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

<span data-ttu-id="4c946-166">Możesz bezpiecznie zignorować (lub usunąć) instrukcje `#region` i `#endregion` otaczające kod.</span><span class="sxs-lookup"><span data-stu-id="4c946-166">You may safely ignore (or remove) the `#region` and `#endregion` statements that surround the code.</span></span> <span data-ttu-id="4c946-167">Jeśli planujesz uruchamiać przykładowy scenariusz opisany w temacie, nie zmieniaj kodu między tymi instrukcjami.</span><span class="sxs-lookup"><span data-stu-id="4c946-167">Don't alter the code within these statements if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="4c946-168">Kod możesz swobodnie modyfikować, eksperymentując z innymi scenariuszami.</span><span class="sxs-lookup"><span data-stu-id="4c946-168">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="4c946-169">Aby uzyskać więcej informacji, zobacz [Współtworzenie dokumentacji platformy ASP.NET: fragmenty kodu](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="4c946-169">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c946-170">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4c946-170">Next steps</span></span>

<span data-ttu-id="4c946-171">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="4c946-171">For more information, see the following resources:</span></span>

* [<span data-ttu-id="4c946-172">Wprowadzenie do korzystania ze stron Razor</span><span class="sxs-lookup"><span data-stu-id="4c946-172">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="4c946-173">Platforma ASP.NET Core — podstawy</span><span class="sxs-lookup"><span data-stu-id="4c946-173">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="4c946-174">[Cotygodniowe podsumowanie ASP.NET Community Standup](https://live.asp.net/) zawiera aktualności o postępach i planach zespołu.</span><span class="sxs-lookup"><span data-stu-id="4c946-174">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="4c946-175">Zawiera też informacje o polecanych blogach i oprogramowaniu innych firm.</span><span class="sxs-lookup"><span data-stu-id="4c946-175">It features new blogs and third-party software.</span></span>
