---
title: Zwiększanie możliwości aplikacji z zewnętrznego zestawu w programie ASP.NET Core za pomocą interfejsu IHostingStartup
author: guardrex
description: Dowiedz się, jak poprawić aplikacji ASP.NET Core z zestawu zewnętrznego za pomocą interfejsu IHostingStartup implementację.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/22/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: ef3b48dc72f294a783d789c4c9a796e3498a91d9
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299459"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="cd4c5-103">Zwiększanie możliwości aplikacji z zewnętrznego zestawu w programie ASP.NET Core za pomocą interfejsu IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="cd4c5-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="cd4c5-104">Przez [Luke Latham](https://github.com/guardrex) i [Pavel Krymets](https://github.com/pakrym)</span><span class="sxs-lookup"><span data-stu-id="cd4c5-104">By [Luke Latham](https://github.com/guardrex) and [Pavel Krymets](https://github.com/pakrym)</span></span>

<span data-ttu-id="cd4c5-105">[Interfejsu IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hostuje uruchamiania) implementacja dodaje rozszerzenia do aplikacji przy uruchamianiu z zestawu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="cd4c5-106">Na przykład zewnętrznej biblioteki służy hostingu implementacji uruchamiania zapewnienie dodatkowej konfiguracji dostawcy lub usług do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="cd4c5-107">`IHostingStartup` *jest dostępna w programie ASP.NET Core 2.0 lub nowszej.*</span><span class="sxs-lookup"><span data-stu-id="cd4c5-107">`IHostingStartup` *is available in ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="cd4c5-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cd4c5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="cd4c5-109">Atrybut HostingStartup</span><span class="sxs-lookup"><span data-stu-id="cd4c5-109">HostingStartup attribute</span></span>

<span data-ttu-id="cd4c5-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut wskazuje obecność hostingu zestaw startowy można aktywować w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="cd4c5-111">Zestaw wpisu lub zestawu zawierającego `Startup` automatycznie klasy jest skanowany pod kątem `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-111">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="cd4c5-112">Lista zestawów, aby wyszukać `HostingStartup` atrybutów jest ładowany w czasie wykonywania z konfiguracji w [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-112">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="cd4c5-113">Lista zestawów do wykluczenia z odnajdywania jest ładowany z [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-113">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="cd4c5-114">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: hostingu zestawy startowe](xref:fundamentals/host/web-host#hosting-startup-assemblies) i [hosta sieci Web: hostingu uruchamiania wykluczyć zestawy](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-114">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="cd4c5-115">W poniższym przykładzie jest przestrzeń nazw w zestawie hostingu uruchamiania `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-115">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="cd4c5-116">Klasa zawierająca kod uruchamiający hostingu jest `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-116">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="cd4c5-117">`HostingStartup` Atrybutu znajduje się w zestawie uruchomienia hostingu `IHostingStartup` pliku z implementacją klasy.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-117">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="cd4c5-118">Odkryj załadowanych zestawów uruchomienia hostingu</span><span class="sxs-lookup"><span data-stu-id="cd4c5-118">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="cd4c5-119">Aby odnaleźć załadowanych zestawów uruchomienia hostingu, Włącz rejestrowanie i zapoznać się z dziennikami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-119">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="cd4c5-120">Błędy występujące podczas ładowania zestawów są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-120">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="cd4c5-121">Załadowano hostingu zestawy startowe są rejestrowane na poziomie debugowania, a wszystkie błędy są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-121">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="cd4c5-122">Wyłącz automatyczne ładowanie hostingu zestawy startowe</span><span class="sxs-lookup"><span data-stu-id="cd4c5-122">Disable automatic loading of hosting startup assemblies</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="cd4c5-123">Aby wyłączyć automatyczne ładowanie hostingu zestawy startowe, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-123">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="cd4c5-124">Aby zapobiec temu wszystkie zestawy startowe hostingu, ładowania, należy ustawić jedną z następujących czynności, aby `true` lub `1`:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-124">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="cd4c5-125">[Zapobieganie uruchamianiu hostingu](xref:fundamentals/host/web-host#prevent-hosting-startup) hosta ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-125">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="cd4c5-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="cd4c5-127">Aby zapobiec określonych zestawów uruchomienia hostingu, ładowanie, ustawić jedną z następujących hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania ciągu rozdzielone średnikami:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-127">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="cd4c5-128">[Hosting uruchamiania wykluczyć zestawy](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) hosta ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-128">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="cd4c5-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="cd4c5-130">Aby wyłączyć automatyczne ładowanie hostingu zestawy startowe, ustawić jedną z następujących czynności, aby `true` lub `1`:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-130">To disable automatic loading of hosting startup assemblies, set one of the following to `true` or `1`:</span></span>

* <span data-ttu-id="cd4c5-131">[Zapobieganie uruchamianiu hostingu](xref:fundamentals/host/web-host#prevent-hosting-startup) hosta ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-131">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="cd4c5-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

::: moniker-end

<span data-ttu-id="cd4c5-133">Jeśli ustawienia konfiguracji hosta i zmienną środowiskową są skonfigurowane, ustawienie hosta steruje zachowaniem.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-133">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="cd4c5-134">Wyłączenie hostingu zestawy startowe przy użyciu zmiennej ustawienie lub środowisko hosta globalnie wyłącza zestawu i może wyłączyć pewne cechy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-134">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="cd4c5-135">Projekt</span><span class="sxs-lookup"><span data-stu-id="cd4c5-135">Project</span></span>

<span data-ttu-id="cd4c5-136">Tworzenie uruchomienia hostingu za pomocą jednej z poniższych typów projektów:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-136">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="cd4c5-137">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="cd4c5-137">Class library</span></span>](#class-library)
* [<span data-ttu-id="cd4c5-138">Aplikacja konsoli bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="cd4c5-138">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="cd4c5-139">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="cd4c5-139">Class library</span></span>

<span data-ttu-id="cd4c5-140">Można podać hostingu ulepszenie uruchamiania w bibliotece klas.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-140">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="cd4c5-141">Biblioteka zawiera `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-141">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="cd4c5-142">[Przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) obejmuje aplikacją stron Razor *HostingStartupApp*i biblioteki klas, *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-142">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="cd4c5-143">Biblioteka klas:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-143">The class library:</span></span>

* <span data-ttu-id="cd4c5-144">Zawiera klasę uruchomienia hostingu `ServiceKeyInjection`, który implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-144">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="cd4c5-145">`ServiceKeyInjection` Dodaje parę ciągów usługi konfiguracji aplikacji przy użyciu dostawcy konfiguracji w pamięci ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-145">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="cd4c5-146">Obejmuje `HostingStartup` atrybut, który identyfikuje przestrzeni nazw i klasy uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-146">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="cd4c5-147">`ServiceKeyInjection` Klasy [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda używa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) na dodawanie rozszerzeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-147">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="cd4c5-148">`IHostingStartup.Configure` w hostingu uruchamiania zestawu jest wywoływana przez środowisko wykonawcze przed `Startup.Configure` w kodzie użytkownika, co pozwala kod użytkownika zastąpić żadnej konfiguracji zawartym w zestawie hostingu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-148">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

<span data-ttu-id="cd4c5-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="cd4c5-150">Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez bibliotekę klas hostingu uruchamiania zestawu:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-150">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="cd4c5-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="cd4c5-152">[Przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera także projekt pakietu NuGet, który zawiera osobne uruchomienia hostingu, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-152">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="cd4c5-153">Pakiet ma takie same charakterystyki biblioteki klas wcześniejszym opisem.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-153">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="cd4c5-154">Pakiet:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-154">The package:</span></span>

* <span data-ttu-id="cd4c5-155">Zawiera klasę uruchomienia hostingu `ServiceKeyInjection`, który implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-155">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="cd4c5-156">`ServiceKeyInjection` dodaje dwa ciągi usługi do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-156">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="cd4c5-157">Obejmuje `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-157">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="cd4c5-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="cd4c5-159">Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez pakiet hostingu uruchamiania zestawu:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-159">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="cd4c5-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="cd4c5-161">Aplikacja konsoli bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="cd4c5-161">Console app without an entry point</span></span>

<span data-ttu-id="cd4c5-162">*Ta metoda jest dostępna tylko w przypadku aplikacji .NET Core, nie .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="cd4c5-162">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="cd4c5-163">Dynamiczne hostingu ulepszenie uruchamiania, która nie wymaga odwołania w czasie kompilacji w celu aktywacji można podać w aplikacji konsolowej bez punktu wejścia, który zawiera `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-163">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="cd4c5-164">Publikowanie aplikacji konsoli daje hostingu zestaw startowy, który mogą być używane w sklepie środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-164">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="cd4c5-165">Aplikacja konsolowa bez punktu wejścia jest używany w ramach tego procesu, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-165">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="cd4c5-166">Do korzystania z hostingu uruchamiania w zestawie hostingu uruchamiania jest wymagana pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-166">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="cd4c5-167">Plik zależności jest trwały możliwy do uruchomienia aplikacji, który jest wytwarzany przez opublikowanie aplikacji, a nie z biblioteki.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-167">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="cd4c5-168">Nie można dodać bezpośrednio do biblioteki [Magazyn pakietu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store), co wymaga możliwy do uruchomienia projektu, który jest przeznaczony dla udostępnionego środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-168">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="cd4c5-169">W przypadku tworzenia dynamicznego uruchamiania hostingu:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-169">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="cd4c5-170">Zestaw startowy hostingu jest tworzony z aplikacji konsoli, bez punkt wejścia:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-170">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="cd4c5-171">Zawiera klasę, która zawiera `IHostingStartup` implementacji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-171">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="cd4c5-172">Obejmuje [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut do identyfikowania `IHostingStartup` implementacji klasy.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-172">Includes a [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="cd4c5-173">Aplikacja konsoli jest publikowany uzyskać zależności uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-173">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="cd4c5-174">Konsekwencją publikowania aplikacji konsoli jest, że nieużywane zależności są usuwane z pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-174">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="cd4c5-175">Plik zależności jest modyfikowany do ustawiania lokalizacji środowiska uruchomieniowego, zestawu startowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-175">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="cd4c5-176">Zestawie hostingu uruchamiania i jego zależności pliku jest umieszczany w Magazyn pakietu środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-176">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="cd4c5-177">Aby odnaleźć zestawie hostingu uruchamiania i jego zależności pliku, są wyświetlane w pary zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-177">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="cd4c5-178">Aplikacja konsoli odwołuje się do [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) pakietu:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-178">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="cd4c5-179">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut określa klasę jako implementacja `IHostingStartup` w celu ładowania i wykonania podczas kompilowania [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-179">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="cd4c5-180">W poniższym przykładzie, przestrzeń nazw jest `StartupEnhancement`, a klasa jest `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-180">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="cd4c5-181">Klasa implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-181">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="cd4c5-182">Klasa [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda używa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) na dodawanie rozszerzeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-182">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="cd4c5-183">`IHostingStartup.Configure` w hostingu uruchamiania zestawu jest wywoływana przez środowisko wykonawcze przed `Startup.Configure` w kodzie użytkownika, co pozwala kod użytkownika zastąpić żadnej konfiguracji zawartym w zestawie hostingu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-183">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="cd4c5-184">Podczas kompilowania `IHostingStartup` projektu pliku zależności (*\*. deps.json*) ustawia `runtime` lokalizacji zestawu do *bin* folderu:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-184">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="cd4c5-185">Jest wyświetlana tylko część pliku.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-185">Only part of the file is shown.</span></span> <span data-ttu-id="cd4c5-186">Nazwa zestawu w przykładzie jest `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-186">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="cd4c5-187">Określ hostingu zestawu startowego</span><span class="sxs-lookup"><span data-stu-id="cd4c5-187">Specify the hosting startup assembly</span></span>

<span data-ttu-id="cd4c5-188">Dla klasy biblioteki — albo konsoli aplikacji dostarczanego hostingu uruchamiania, określić nazwę zestawu startowego hostingu w `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-188">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="cd4c5-189">Zmienna środowiskowa jest rozdzieloną średnikami listę zestawów.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-189">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="cd4c5-190">Tylko hostingu zestawy startowe są skanowane pod kątem `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-190">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="cd4c5-191">Dla przykładowej aplikacji *HostingStartupApp*, aby odnaleźć hostingu startupy, które opisano wcześniej, zmienna środowiskowa jest ustawiona na następującą wartość:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-191">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="cd4c5-192">Zestaw startowy hostingu można również ustawić za pomocą [hostingu zestawy startowe](xref:fundamentals/host/web-host#hosting-startup-assemblies) hosta ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-192">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="cd4c5-193">Podczas uruchamiania wielu hostingu składa są obecne, ich [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metody są wykonywane w kolejności, czy zestawy są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-193">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="cd4c5-194">Aktywacja</span><span class="sxs-lookup"><span data-stu-id="cd4c5-194">Activation</span></span>

<span data-ttu-id="cd4c5-195">Dostępne są następujące opcje do obsługi aktywacji uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-195">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="cd4c5-196">[Środowisko uruchomieniowe magazynu](#runtime-store) &ndash; aktywacji nie wymaga odwołania w czasie kompilacji w celu aktywacji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-196">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="cd4c5-197">Przykładowa aplikacja umieszcza hostingu zestawu startowe i zależności pliki do folderu, *wdrożenia*w celu ułatwienia wdrażania hostingu uruchamiania w środowisku multimachine.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-197">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="cd4c5-198">*Wdrożenia* folder zawiera także skrypt programu PowerShell, który tworzy lub modyfikuje zmienne środowiskowe w systemie wdrożenia, aby włączyć uruchamianie hostingu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-198">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="cd4c5-199">Dokumentacja kompilacji wymagany do aktywacji</span><span class="sxs-lookup"><span data-stu-id="cd4c5-199">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="cd4c5-200">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="cd4c5-200">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="cd4c5-201">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="cd4c5-201">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="cd4c5-202">Środowisko uruchomieniowe magazynu</span><span class="sxs-lookup"><span data-stu-id="cd4c5-202">Runtime store</span></span>

<span data-ttu-id="cd4c5-203">Hostingu implementacji uruchamiania jest umieszczany w [magazynu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-203">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="cd4c5-204">Odwołanie do zestawu kompilacji nie jest wymagane przez aplikację rozszerzone.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-204">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="cd4c5-205">Po utworzeniu hostingu uruchamiania magazynu środowiska uruchomieniowego jest generowana z użyciem pliku manifestu projektu i [magazynu dotnet](/dotnet/core/tools/dotnet-store) polecenia.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-205">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="cd4c5-206">W przykładowej aplikacji (*RuntimeStore* projektu) służy następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-206">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

``` console
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="cd4c5-207">Dla środowiska uruchomieniowego odnaleźć magazyn środowiska uruchomieniowego, lokalizacja magazynu środowiska uruchomieniowego jest dodawana do `DOTNET_SHARED_STORE` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-207">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="cd4c5-208">**Zmodyfikuj i umieść plik zależności startowe hostingu**</span><span class="sxs-lookup"><span data-stu-id="cd4c5-208">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="cd4c5-209">Aby aktywować rozszerzenie bez pakietu odwołania do poprawienia, określ dodatkowe zależności środowiska uruchomieniowego za pomocą `additionalDeps`.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-209">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="cd4c5-210">`additionalDeps` Umożliwia:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-210">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="cd4c5-211">Rozszerz wykres biblioteki aplikacji przez udostępnienie zestawu dodatkowe  *\*. deps.json* pliki do scalania z własnych aplikacji  *\*. deps.json* pliku podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-211">Extend the app's library graph by providing a set of additional *\*.deps.json* files to merge with the app's own *\*.deps.json* file on startup.</span></span>
* <span data-ttu-id="cd4c5-212">Należy hostingu zestawu startowego obciążana i prostsze do odnalezienia.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-212">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="cd4c5-213">Jest zalecane podejście do generowania pliku dodatkowe zależności:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-213">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="cd4c5-214">Wykonaj `dotnet publish` w pliku manifestu sklepu środowiska uruchomieniowego, do którego odwołuje się w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-214">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="cd4c5-215">Usuń odwołanie do manifestu z bibliotek i `runtime` części wynikowy  *\*deps.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-215">Remove the manifest reference from libraries and the `runtime` section of the resulting *\*deps.json* file.</span></span>

<span data-ttu-id="cd4c5-216">W przykładowym projekcie `store.manifest/1.0.0` właściwości zostanie usunięta z `targets` i `libraries` sekcji:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-216">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v2.1",
    "signature": "4ea77c7b75ad1895ae1ea65e6ba2399010514f99"
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v2.1": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp2.1/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-oiQr60vBQW7+nBTmgKLSldj06WNLRTdhOZpAdEbCuapoZ+M2DJH2uQbRLvFT8EGAAv4TAKzNtcztpx5YOgBXQQ==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="cd4c5-217">Miejsce  *\*. deps.json* plik w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-217">Place the *\*.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="cd4c5-218">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Dodane do lokalizacji `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-218">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="cd4c5-219">`{SHARED FRAMEWORK NAME}` &ndash; Udostępnione framework jest wymagany dla tego pliku dodatkowe zależności.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-219">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="cd4c5-220">`{SHARED FRAMEWORK VERSION}` &ndash; Wersja minimalna udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-220">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="cd4c5-221">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; Rozszerzenie nazwy zestawu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-221">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="cd4c5-222">W przykładowej aplikacji (*RuntimeStore* projektu), plik dodatkowe zależności jest umieszczany w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-222">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="cd4c5-223">Dla środowiska uruchomieniowego odnaleźć lokalizacji magazynu środowiska uruchomieniowego, lokalizacja pliku dodatkowe zależności jest dodawana do `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-223">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="cd4c5-224">W przykładowej aplikacji (*RuntimeStore* projektu), tworzenie magazynu środowiska uruchomieniowego i generuje dodatkowe zależności pliku odbywa się przy użyciu [PowerShell](/powershell/scripting/powershell-scripting) skryptu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-224">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="cd4c5-225">Aby zapoznać się z przykładami sposobu ustawiania zmiennych środowiskowych dla różnych systemów operacyjnych, zobacz [używanie wielu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-225">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="cd4c5-226">**Wdrażanie**</span><span class="sxs-lookup"><span data-stu-id="cd4c5-226">**Deployment**</span></span>

<span data-ttu-id="cd4c5-227">W celu ułatwienia wdrażania hostingu uruchamiania w środowisku multimachine, przykładowa aplikacja tworzy *wdrożenia* folderu w opublikowanych danych wyjściowych, który zawiera:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-227">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="cd4c5-228">Uruchamianie magazyn środowisko uruchomieniowe hostingu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-228">The hosting startup runtime store.</span></span>
* <span data-ttu-id="cd4c5-229">Plik hostingu zależności startowe.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-229">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="cd4c5-230">Skrypt programu PowerShell, który tworzy lub modyfikuje `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, i `DOTNET_ADDITIONAL_DEPS` do obsługi aktywacji uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-230">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="cd4c5-231">Uruchom skrypt w administracyjnym wierszu polecenia programu PowerShell w systemie wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-231">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="cd4c5-232">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="cd4c5-232">NuGet package</span></span>

<span data-ttu-id="cd4c5-233">Można podać hostingu ulepszenie uruchamiania pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-233">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="cd4c5-234">Pakiet ma `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-234">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="cd4c5-235">Hostingu typy uruchamiania, dostarczone przez pakiet są dostępne dla aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-235">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="cd4c5-236">Plik projektu aplikacji rozszerzone sprawia, że odwołania do pakietu do uruchomienia hostingu w pliku projektu aplikacji (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-236">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="cd4c5-237">Za pomocą odwołania w czasie kompilacji w miejscu, zestawie hostingu uruchamiania i wszystkie jego zależności są włączone do pliku zależności aplikacji (*\*. deps.json*).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-237">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="cd4c5-238">Takie podejście stosuje się do hostingu pakietu zestawu startowego publikowane w [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-238">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="cd4c5-239">Plik zależności startowe hostingu ma zostać udostępnione rozszerzone aplikacji zgodnie z opisem w [magazynu środowiska uruchomieniowego](#runtime-store) sekcji (bez odwołania do kompilacji).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-239">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="cd4c5-240">Aby uzyskać więcej informacji na temat pakietów NuGet i magazynem środowiska uruchomieniowego zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-240">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="cd4c5-241">Jak utworzyć pakiet NuGet za pomocą narzędzi międzyplatformowych</span><span class="sxs-lookup"><span data-stu-id="cd4c5-241">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="cd4c5-242">Publikowanie pakietów</span><span class="sxs-lookup"><span data-stu-id="cd4c5-242">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="cd4c5-243">Magazyn pakietu środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="cd4c5-243">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="cd4c5-244">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="cd4c5-244">Project bin folder</span></span>

<span data-ttu-id="cd4c5-245">Hostingu ulepszenie uruchamiania mogą być zapewniane przez *bin*-wdrożone zestawu w rozszerzonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-245">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="cd4c5-246">Typy uruchamiania hostingu, dostarczone przez zestaw są dostępne dla aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-246">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="cd4c5-247">Plik projektu aplikacji rozszerzone sprawia, że odwołanie do zestawu do uruchomienia hostingu (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-247">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="cd4c5-248">Za pomocą odwołania w czasie kompilacji w miejscu, zestawie hostingu uruchamiania i wszystkie jego zależności są włączone do pliku zależności aplikacji (*\*. deps.json*).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-248">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="cd4c5-249">Takie podejście ma zastosowanie, gdy scenariusz wdrażania wymaga, aby przenoszenia zestawu skompilowanego hostingu uruchamiania biblioteki (plik DLL) do konsumencki projektu lub do lokalizacji dostępne przez konsumencki projekt i kompilacji odnosi się do hostingu zestaw startowy firmy.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-249">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="cd4c5-250">Plik zależności startowe hostingu ma zostać udostępnione rozszerzone aplikacji zgodnie z opisem w [magazynu środowiska uruchomieniowego](#runtime-store) sekcji (bez odwołania do kompilacji).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-250">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="cd4c5-251">Przykładowy kod</span><span class="sxs-lookup"><span data-stu-id="cd4c5-251">Sample code</span></span>

<span data-ttu-id="cd4c5-252">[Przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)) pokazuje hostingu scenariuszom dla programów próbnych implementacji:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-252">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="cd4c5-253">Dwa hostingu zestawy startowe (bibliotek klas) Ustaw parę pary klucz wartość w pamięci konfiguracji poszczególnych:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-253">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="cd4c5-254">Pakiet NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="cd4c5-254">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="cd4c5-255">Biblioteka klas (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="cd4c5-255">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="cd4c5-256">Uruchamianie hostingu została aktywowana w zestawie środowiska uruchomieniowego wdrożonych w magazynie (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-256">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="cd4c5-257">Zestaw dodaje dwa middlewares do aplikacji podczas uruchamiania, które zawierają informacje diagnostyczne dotyczące:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-257">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="cd4c5-258">Zarejestrowane usługi</span><span class="sxs-lookup"><span data-stu-id="cd4c5-258">Registered services</span></span>
  * <span data-ttu-id="cd4c5-259">Adres (schematu, hosta, podstawa ścieżki, ścieżki, ciąg zapytania)</span><span class="sxs-lookup"><span data-stu-id="cd4c5-259">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="cd4c5-260">Połączenia (zdalnego adresu IP, port zdalny lokalnego adresu IP, port lokalny, certyfikatu klienta)</span><span class="sxs-lookup"><span data-stu-id="cd4c5-260">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="cd4c5-261">Nagłówki żądania</span><span class="sxs-lookup"><span data-stu-id="cd4c5-261">Request headers</span></span>
  * <span data-ttu-id="cd4c5-262">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="cd4c5-262">Environment variables</span></span>

<span data-ttu-id="cd4c5-263">Do uruchomienia przykładu:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-263">To run the sample:</span></span>

<span data-ttu-id="cd4c5-264">**Aktywacja z pakietu NuGet**</span><span class="sxs-lookup"><span data-stu-id="cd4c5-264">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="cd4c5-265">Skompilować *HostingStartupPackage* pakietu z [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-265">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="cd4c5-266">Dodaj nazwę zestawu pakietu *HostingStartupPackage* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-266">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="cd4c5-267">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-267">Compile and run the app.</span></span> <span data-ttu-id="cd4c5-268">Odwołanie do pakietu jest obecny w aplikacji rozszerzone (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-268">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="cd4c5-269">A `<PropertyGroup>` w projekcie aplikacji plik Określa danych wyjściowych projektu pakietu (*... / HostingStartupPackage/bin/Debug*) jako źródło pakietów.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-269">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="cd4c5-270">Umożliwia to aplikacji na użycie pakietu bez przekazywania pakietu, który ma [nuget.org](https://www.nuget.org/). Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-270">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="cd4c5-271">Sprawdź, czy wartości klucza konfiguracji usługi świadczone przez stronę indeksu odpowiadają wartości ustawione przy użyciu pakietu `ServiceKeyInjection.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-271">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="cd4c5-272">W przypadku wprowadzenia zmian do *HostingStartupPackage* projektu i ponownie ją kompilując, wyczyść pamięciach podręcznych pakietu NuGet, aby upewnić się, że *HostingStartupApp* otrzymuje zaktualizowany pakiet i nie nieodświeżone pakiet z lokalnej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-272">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="cd4c5-273">Aby wyczyścić pamięciach podręcznych NuGet, wykonaj następujące czynności, [lokalne nuget dotnet](/dotnet/core/tools/dotnet-nuget-locals) polecenia:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-273">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="cd4c5-274">**Aktywacja z biblioteki klas**</span><span class="sxs-lookup"><span data-stu-id="cd4c5-274">**Activation from a class library**</span></span>

1. <span data-ttu-id="cd4c5-275">Skompilować *HostingStartupLibrary* biblioteki klas z [kompilacji dotnet](/dotnet/core/tools/dotnet-build) polecenia.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-275">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="cd4c5-276">Dodaj nazwę zestawu biblioteki klas *HostingStartupLibrary* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-276">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="cd4c5-277">*pojemnik*— wdrażanie zestawu biblioteki klas w aplikacji, kopiując *HostingStartupLibrary.dll* pliku z biblioteki klas skompilowanych danych wyjściowych w aplikacji *bin/Debug* folderu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-277">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="cd4c5-278">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-278">Compile and run the app.</span></span> <span data-ttu-id="cd4c5-279">`<ItemGroup>` w projekcie aplikacji plik odwołuje się do zestawu biblioteki klas (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-279">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="cd4c5-280">Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-280">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="cd4c5-281">Sprawdź, czy wartości klucza konfiguracji usługi świadczone przez stronę indeksu odpowiadają wartości ustawione przez bibliotekę klas `ServiceKeyInjection.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-281">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="cd4c5-282">**Aktywacja w zestawie środowiska uruchomieniowego wdrożonych w magazynie**</span><span class="sxs-lookup"><span data-stu-id="cd4c5-282">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="cd4c5-283">*StartupDiagnostics* projekt używa [PowerShell](/powershell/scripting/powershell-scripting) do modyfikowania jej *StartupDiagnostics.deps.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-283">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="cd4c5-284">Program PowerShell jest instalowany domyślnie w systemie Windows, począwszy od Windows 7 z dodatkiem SP1 i Windows Server 2008 R2 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-284">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="cd4c5-285">Aby uzyskać programu PowerShell na innych platformach, zobacz [Instalowanie programu Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-285">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="cd4c5-286">Tworzenie *StartupDiagnostics* projektu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-286">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="cd4c5-287">Po projekt został utworzony, element docelowy kompilacji, w pliku projektu automatycznie:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-287">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="cd4c5-288">Uruchamia skrypt programu PowerShell w celu zmodyfikowania *StartupDiagnostics.deps.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-288">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="cd4c5-289">Przenosi *StartupDiagnostics.deps.json* pliku do profilu użytkownika *additionalDeps* folderu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-289">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="cd4c5-290">Wykonaj `dotnet store` polecenia na command prompt w hostingu uruchamiania katalog do przechowywania zestawu i jego zależności w magazynie środowiska uruchomieniowego profilu użytkownika:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-290">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="cd4c5-291">Windows, polecenie używa `win7-x64` [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="cd4c5-291">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="cd4c5-292">Podczas dostarczania hostingu uruchamiania różnych środowiska uruchomieniowego, podstawić poprawne identyfikatorów RID.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-292">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="cd4c5-293">Ustaw zmienne środowiskowe:</span><span class="sxs-lookup"><span data-stu-id="cd4c5-293">Set the environment variables:</span></span>
   * <span data-ttu-id="cd4c5-294">Dodaj nazwę zestawu *StartupDiagnostics* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-294">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="cd4c5-295">Na Windows, ustaw `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej, aby `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-295">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="cd4c5-296">W systemie macOS/Linux, należy ustawić `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej, aby `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, gdzie `<USER>` jest profil użytkownika, który zawiera uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-296">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="cd4c5-297">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-297">Run the sample app.</span></span>
1. <span data-ttu-id="cd4c5-298">Żądanie `/services` punktu końcowego, aby wyświetlić aplikację zarejestrowane usługi.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-298">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="cd4c5-299">Żądanie `/diag` punktu końcowego, aby wyświetlić informacje diagnostyczne.</span><span class="sxs-lookup"><span data-stu-id="cd4c5-299">Request the `/diag` endpoint to see the diagnostic information.</span></span>
