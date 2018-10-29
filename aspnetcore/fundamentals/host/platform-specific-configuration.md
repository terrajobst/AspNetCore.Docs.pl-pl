---
title: Zwiększanie możliwości aplikacji z zewnętrznego zestawu w programie ASP.NET Core za pomocą interfejsu IHostingStartup
author: guardrex
description: Dowiedz się, jak poprawić aplikacji ASP.NET Core z zestawu zewnętrznego za pomocą interfejsu IHostingStartup implementację.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: a06c2da04c1631f5811a535c891ca5190b0d8864
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207566"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="1c836-103">Zwiększanie możliwości aplikacji z zewnętrznego zestawu w programie ASP.NET Core za pomocą interfejsu IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="1c836-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="1c836-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1c836-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1c836-105">[Interfejsu IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hostuje uruchamiania) implementacja dodaje rozszerzenia do aplikacji przy uruchamianiu z zestawu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="1c836-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="1c836-106">Na przykład zewnętrznej biblioteki służy hostingu implementacji uruchamiania zapewnienie dodatkowej konfiguracji dostawcy lub usług do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c836-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="1c836-107">`IHostingStartup` *jest dostępna w programie ASP.NET Core 2.0 lub nowszej.*</span><span class="sxs-lookup"><span data-stu-id="1c836-107">`IHostingStartup` *is available in ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="1c836-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1c836-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="1c836-109">Atrybut HostingStartup</span><span class="sxs-lookup"><span data-stu-id="1c836-109">HostingStartup attribute</span></span>

<span data-ttu-id="1c836-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut wskazuje obecność hostingu zestaw startowy można aktywować w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1c836-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="1c836-111">Zestaw wpisu lub zestawu zawierającego `Startup` automatycznie klasy jest skanowany pod kątem `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1c836-111">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="1c836-112">Lista zestawów, aby wyszukać `HostingStartup` atrybutów jest ładowany w czasie wykonywania z konfiguracji w [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="1c836-112">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="1c836-113">Lista zestawów do wykluczenia z odnajdywania jest ładowany z [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="1c836-113">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="1c836-114">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: hostingu zestawy startowe](xref:fundamentals/host/web-host#hosting-startup-assemblies) i [hosta sieci Web: hostingu uruchamiania wykluczyć zestawy](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="1c836-114">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="1c836-115">W poniższym przykładzie jest przestrzeń nazw w zestawie hostingu uruchamiania `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="1c836-115">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="1c836-116">Klasa zawierająca kod uruchamiający hostingu jest `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="1c836-116">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="1c836-117">`HostingStartup` Atrybutu znajduje się w zestawie uruchomienia hostingu `IHostingStartup` pliku z implementacją klasy.</span><span class="sxs-lookup"><span data-stu-id="1c836-117">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="1c836-118">Odkryj załadowanych zestawów uruchomienia hostingu</span><span class="sxs-lookup"><span data-stu-id="1c836-118">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="1c836-119">Aby odnaleźć załadowanych zestawów uruchomienia hostingu, Włącz rejestrowanie i zapoznać się z dziennikami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c836-119">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="1c836-120">Błędy występujące podczas ładowania zestawów są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="1c836-120">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="1c836-121">Załadowano hostingu zestawy startowe są rejestrowane na poziomie debugowania, a wszystkie błędy są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="1c836-121">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="1c836-122">Wyłącz automatyczne ładowanie hostingu zestawy startowe</span><span class="sxs-lookup"><span data-stu-id="1c836-122">Disable automatic loading of hosting startup assemblies</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1c836-123">Aby wyłączyć automatyczne ładowanie hostingu zestawy startowe, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="1c836-123">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="1c836-124">Aby zapobiec temu wszystkie zestawy startowe hostingu, ładowania, należy ustawić jedną z następujących czynności, aby `true` lub `1`:</span><span class="sxs-lookup"><span data-stu-id="1c836-124">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="1c836-125">[Zapobieganie uruchamianiu hostingu](xref:fundamentals/host/web-host#prevent-hosting-startup) hosta ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c836-125">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="1c836-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="1c836-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="1c836-127">Aby zapobiec określonych zestawów uruchomienia hostingu, ładowanie, ustawić jedną z następujących hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania ciągu rozdzielone średnikami:</span><span class="sxs-lookup"><span data-stu-id="1c836-127">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="1c836-128">[Hosting uruchamiania wykluczyć zestawy](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) hosta ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c836-128">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="1c836-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="1c836-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1c836-130">Aby wyłączyć automatyczne ładowanie hostingu zestawy startowe, ustawić jedną z następujących czynności, aby `true` lub `1`:</span><span class="sxs-lookup"><span data-stu-id="1c836-130">To disable automatic loading of hosting startup assemblies, set one of the following to `true` or `1`:</span></span>

* <span data-ttu-id="1c836-131">[Zapobieganie uruchamianiu hostingu](xref:fundamentals/host/web-host#prevent-hosting-startup) hosta ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c836-131">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="1c836-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="1c836-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

::: moniker-end

<span data-ttu-id="1c836-133">Jeśli ustawienia konfiguracji hosta i zmienną środowiskową są skonfigurowane, ustawienie hosta steruje zachowaniem.</span><span class="sxs-lookup"><span data-stu-id="1c836-133">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="1c836-134">Wyłączenie hostingu zestawy startowe przy użyciu zmiennej ustawienie lub środowisko hosta globalnie wyłącza zestawu i może wyłączyć pewne cechy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c836-134">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="1c836-135">Projekt</span><span class="sxs-lookup"><span data-stu-id="1c836-135">Project</span></span>

<span data-ttu-id="1c836-136">Tworzenie uruchomienia hostingu za pomocą jednej z poniższych typów projektów:</span><span class="sxs-lookup"><span data-stu-id="1c836-136">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="1c836-137">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="1c836-137">Class library</span></span>](#class-library)
* [<span data-ttu-id="1c836-138">Aplikacja konsoli bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="1c836-138">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="1c836-139">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="1c836-139">Class library</span></span>

<span data-ttu-id="1c836-140">Można podać hostingu ulepszenie uruchamiania w bibliotece klas.</span><span class="sxs-lookup"><span data-stu-id="1c836-140">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="1c836-141">Biblioteka zawiera `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1c836-141">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="1c836-142">[Przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) obejmuje aplikacją stron Razor *HostingStartupApp*i biblioteki klas, *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="1c836-142">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="1c836-143">Biblioteka klas:</span><span class="sxs-lookup"><span data-stu-id="1c836-143">The class library:</span></span>

* <span data-ttu-id="1c836-144">Zawiera klasę uruchomienia hostingu `ServiceKeyInjection`, który implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1c836-144">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="1c836-145">`ServiceKeyInjection` Dodaje parę ciągów usługi konfiguracji aplikacji przy użyciu dostawcy konfiguracji w pamięci ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="1c836-145">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="1c836-146">Obejmuje `HostingStartup` atrybut, który identyfikuje przestrzeni nazw i klasy uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="1c836-146">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="1c836-147">`ServiceKeyInjection` Klasy [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda używa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) na dodawanie rozszerzeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c836-147">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="1c836-148">`IHostingStartup.Configure` w hostingu uruchamiania zestawu jest wywoływana przez środowisko wykonawcze przed `Startup.Configure` w kodzie użytkownika, co pozwala kod użytkownika zastąpić żadnej konfiguracji zawartym w zestawie hostingu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1c836-148">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

<span data-ttu-id="1c836-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c836-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="1c836-150">Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez bibliotekę klas hostingu uruchamiania zestawu:</span><span class="sxs-lookup"><span data-stu-id="1c836-150">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="1c836-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c836-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="1c836-152">[Przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera także projekt pakietu NuGet, który zawiera osobne uruchomienia hostingu, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="1c836-152">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="1c836-153">Pakiet ma takie same charakterystyki biblioteki klas wcześniejszym opisem.</span><span class="sxs-lookup"><span data-stu-id="1c836-153">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="1c836-154">Pakiet:</span><span class="sxs-lookup"><span data-stu-id="1c836-154">The package:</span></span>

* <span data-ttu-id="1c836-155">Zawiera klasę uruchomienia hostingu `ServiceKeyInjection`, który implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1c836-155">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="1c836-156">`ServiceKeyInjection` dodaje dwa ciągi usługi do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c836-156">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="1c836-157">Obejmuje `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1c836-157">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="1c836-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c836-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="1c836-159">Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez pakiet hostingu uruchamiania zestawu:</span><span class="sxs-lookup"><span data-stu-id="1c836-159">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="1c836-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c836-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="1c836-161">Aplikacja konsoli bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="1c836-161">Console app without an entry point</span></span>

<span data-ttu-id="1c836-162">*Ta metoda jest dostępna tylko w przypadku aplikacji .NET Core, nie .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="1c836-162">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="1c836-163">Dynamiczne hostingu ulepszenie uruchamiania, która nie wymaga odwołania w czasie kompilacji w celu aktywacji można podać w aplikacji konsolowej bez punktu wejścia.</span><span class="sxs-lookup"><span data-stu-id="1c836-163">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point.</span></span> <span data-ttu-id="1c836-164">Ta aplikacja zawiera `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1c836-164">The app contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="1c836-165">Aby utworzyć dynamiczny uruchomienia hostingu:</span><span class="sxs-lookup"><span data-stu-id="1c836-165">To create a dynamic hosting startup:</span></span>

1. <span data-ttu-id="1c836-166">Implementacja biblioteki jest tworzony z klasy, która zawiera `IHostingStartup` implementacji.</span><span class="sxs-lookup"><span data-stu-id="1c836-166">An implementation library is created from the class that contains the `IHostingStartup` implementation.</span></span> <span data-ttu-id="1c836-167">Biblioteka implementacja jest traktowany jako normalny pakietu.</span><span class="sxs-lookup"><span data-stu-id="1c836-167">The implementation library is treated as a normal package.</span></span>
1. <span data-ttu-id="1c836-168">Aplikacja konsolowa bez punktu wejścia odwołuje się do wdrożenia pakietu biblioteki.</span><span class="sxs-lookup"><span data-stu-id="1c836-168">A console app without an entry point references the implementation library package.</span></span> <span data-ttu-id="1c836-169">Aplikacja konsoli jest używany, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="1c836-169">A console app is used because:</span></span>
   * <span data-ttu-id="1c836-170">Plik zależności jest trwały możliwy do uruchomienia aplikacji, więc biblioteki nie może dostarczyć pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="1c836-170">A dependencies file is a runnable app asset, so a library can't furnish a dependencies file.</span></span>
   * <span data-ttu-id="1c836-171">Nie można dodać bezpośrednio do biblioteki [Magazyn pakietu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store), co wymaga możliwy do uruchomienia projektu, który jest przeznaczony dla udostępnionego środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1c836-171">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>
1. <span data-ttu-id="1c836-172">Aplikacja konsoli jest publikowany uzyskać zależności uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="1c836-172">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="1c836-173">Konsekwencją publikowania aplikacji konsoli jest, że nieużywane zależności są usuwane z pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="1c836-173">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
1. <span data-ttu-id="1c836-174">Aplikacja i jej zależności pliku jest umieszczany w Magazyn pakietu środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1c836-174">The app and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="1c836-175">Do odnajdywania, hostingu zestaw startowy i jego zależności pliku, jest przywoływany w parze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="1c836-175">To discover the hosting startup assembly and its dependencies file, they're referenced in a pair of environment variables.</span></span>

<span data-ttu-id="1c836-176">Aplikacja konsoli odwołuje się do [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) pakietu:</span><span class="sxs-lookup"><span data-stu-id="1c836-176">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="1c836-177">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut określa klasę jako implementacja `IHostingStartup` w celu ładowania i wykonania podczas kompilowania [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="1c836-177">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="1c836-178">W poniższym przykładzie, przestrzeń nazw jest `StartupEnhancement`, a klasa jest `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="1c836-178">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="1c836-179">Klasa implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1c836-179">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="1c836-180">Klasa [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda używa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) na dodawanie rozszerzeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c836-180">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="1c836-181">`IHostingStartup.Configure` w hostingu uruchamiania zestawu jest wywoływana przez środowisko wykonawcze przed `Startup.Configure` w kodzie użytkownika, co pozwala kod użytkownika zastąpić żadnej konfiguracji zawartym w zestawie hostingu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1c836-181">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="1c836-182">Podczas kompilowania `IHostingStartup` projektu pliku zależności (*\*. deps.json*) ustawia `runtime` lokalizacji zestawu do *bin* folderu:</span><span class="sxs-lookup"><span data-stu-id="1c836-182">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="1c836-183">Jest wyświetlana tylko część pliku.</span><span class="sxs-lookup"><span data-stu-id="1c836-183">Only part of the file is shown.</span></span> <span data-ttu-id="1c836-184">Nazwa zestawu w przykładzie jest `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="1c836-184">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="1c836-185">Określ hostingu zestawu startowego</span><span class="sxs-lookup"><span data-stu-id="1c836-185">Specify the hosting startup assembly</span></span>

<span data-ttu-id="1c836-186">Dla klasy biblioteki — albo konsoli aplikacji dostarczanego hostingu uruchamiania, określić nazwę zestawu startowego hostingu w `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="1c836-186">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="1c836-187">Zmienna środowiskowa jest rozdzieloną średnikami listę zestawów.</span><span class="sxs-lookup"><span data-stu-id="1c836-187">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="1c836-188">Tylko hostingu zestawy startowe są skanowane pod kątem `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1c836-188">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="1c836-189">Dla przykładowej aplikacji *HostingStartupApp*, aby odnaleźć hostingu startupy, które opisano wcześniej, zmienna środowiskowa jest ustawiona na następującą wartość:</span><span class="sxs-lookup"><span data-stu-id="1c836-189">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="1c836-190">Zestaw startowy hostingu można również ustawić za pomocą [hostingu zestawy startowe](xref:fundamentals/host/web-host#hosting-startup-assemblies) hosta ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c836-190">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="1c836-191">Podczas uruchamiania wielu hostingu składa są obecne, ich [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metody są wykonywane w kolejności, czy zestawy są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="1c836-191">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="1c836-192">Aktywacja</span><span class="sxs-lookup"><span data-stu-id="1c836-192">Activation</span></span>

<span data-ttu-id="1c836-193">Dostępne są następujące opcje do obsługi aktywacji uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="1c836-193">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="1c836-194">[Środowisko uruchomieniowe magazynu](#runtime-store) &ndash; aktywacji nie wymaga odwołania w czasie kompilacji w celu aktywacji.</span><span class="sxs-lookup"><span data-stu-id="1c836-194">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="1c836-195">Przykładowa aplikacja umieszcza hostingu zestawu startowe i zależności pliki do folderu, *wdrożenia*w celu ułatwienia wdrażania hostingu uruchamiania w środowisku multimachine.</span><span class="sxs-lookup"><span data-stu-id="1c836-195">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="1c836-196">*Wdrożenia* folder zawiera także skrypt programu PowerShell, który tworzy lub modyfikuje zmienne środowiskowe w systemie wdrożenia, aby włączyć uruchamianie hostingu.</span><span class="sxs-lookup"><span data-stu-id="1c836-196">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="1c836-197">Dokumentacja kompilacji wymagany do aktywacji</span><span class="sxs-lookup"><span data-stu-id="1c836-197">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="1c836-198">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="1c836-198">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="1c836-199">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="1c836-199">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="1c836-200">Środowisko uruchomieniowe magazynu</span><span class="sxs-lookup"><span data-stu-id="1c836-200">Runtime store</span></span>

<span data-ttu-id="1c836-201">Hostingu implementacji uruchamiania jest umieszczany w [magazynu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="1c836-201">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="1c836-202">Odwołanie do zestawu kompilacji nie jest wymagane przez aplikację rozszerzone.</span><span class="sxs-lookup"><span data-stu-id="1c836-202">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="1c836-203">Po utworzeniu hostingu uruchamiania pliku projektu startowego hostingu służy jako plik manifestu pod kątem [magazynu dotnet](/dotnet/core/tools/dotnet-store) polecenia.</span><span class="sxs-lookup"><span data-stu-id="1c836-203">After the hosting startup is built, the hosting startup's project file serves as the manifest file for the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

<span data-ttu-id="1c836-204">To polecenie umieszcza zestawie hostingu uruchamiania i inne zależności, które nie są częścią udostępnionej platformy w magazynie środowiska uruchomieniowego profilu użytkownika na:</span><span class="sxs-lookup"><span data-stu-id="1c836-204">This command places the hosting startup assembly and other dependencies that aren't part of the shared framework in the user profile's runtime store at:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1c836-205">Windows</span><span class="sxs-lookup"><span data-stu-id="1c836-205">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1c836-206">macOS</span><span class="sxs-lookup"><span data-stu-id="1c836-206">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1c836-207">Linux</span><span class="sxs-lookup"><span data-stu-id="1c836-207">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="1c836-208">Jeśli wymagasz umieścić zestaw i zależności do użytku globalnego, należy dodać `-o|--output` opcję `dotnet store` polecenia z następującą ścieżką:</span><span class="sxs-lookup"><span data-stu-id="1c836-208">If you desire to place the assembly and dependencies for global use, add the `-o|--output` option to the `dotnet store` command with the following path:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1c836-209">Windows</span><span class="sxs-lookup"><span data-stu-id="1c836-209">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1c836-210">macOS</span><span class="sxs-lookup"><span data-stu-id="1c836-210">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1c836-211">Linux</span><span class="sxs-lookup"><span data-stu-id="1c836-211">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="1c836-212">**Zmodyfikuj i umieść plik zależności startowe hostingu**</span><span class="sxs-lookup"><span data-stu-id="1c836-212">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="1c836-213">Lokalizacja środowiska uruchomieniowego jest określona w  *\*. deps.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="1c836-213">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="1c836-214">Aby aktywować ulepszenie, `runtime` element musi określać lokalizację zestawu środowiska wykonawczego rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="1c836-214">To activate the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="1c836-215">Prefiks `runtime` lokalizacji za pomocą `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span><span class="sxs-lookup"><span data-stu-id="1c836-215">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="1c836-216">W przykładowym kodzie (*StartupDiagnostics* projektu), modyfikacji  *\*. deps.json* pliku odbywa się przez [PowerShell](/powershell/scripting/powershell-scripting) skryptu.</span><span class="sxs-lookup"><span data-stu-id="1c836-216">In the sample code (*StartupDiagnostics* project), modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="1c836-217">Skrypt programu PowerShell automatycznie jest wyzwalana przez element docelowy kompilacji, w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="1c836-217">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

<span data-ttu-id="1c836-218">Implementacja  *\*. deps.json* plik musi znajdować się w dostępnej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="1c836-218">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="1c836-219">Do użytku dla użytkownika, umieść plik w *additonalDeps* folderu profilu użytkownika `.dotnet` ustawienia:</span><span class="sxs-lookup"><span data-stu-id="1c836-219">For per-user use, place the file in the *additonalDeps* folder of the user profile's `.dotnet` settings:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1c836-220">Windows</span><span class="sxs-lookup"><span data-stu-id="1c836-220">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1c836-221">macOS</span><span class="sxs-lookup"><span data-stu-id="1c836-221">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1c836-222">Linux</span><span class="sxs-lookup"><span data-stu-id="1c836-222">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="1c836-223">Do użytku globalnego, umieść plik w *additonalDeps* folderu instalacji platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="1c836-223">For global use, place the file in the *additonalDeps* folder of the .NET Core installation:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1c836-224">Windows</span><span class="sxs-lookup"><span data-stu-id="1c836-224">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1c836-225">macOS</span><span class="sxs-lookup"><span data-stu-id="1c836-225">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1c836-226">Linux</span><span class="sxs-lookup"><span data-stu-id="1c836-226">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="1c836-227">Udostępnione framework w wersji odzwierciedla wersję udostępnionego środowiska uruchomieniowego, która korzysta z docelową aplikacją.</span><span class="sxs-lookup"><span data-stu-id="1c836-227">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="1c836-228">Udostępnione środowisko uruchomieniowe jest wyświetlany w  *\*. runtimeconfig.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="1c836-228">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="1c836-229">W przykładowej aplikacji (*HostingStartupApp*), udostępnionego środowiska uruchomieniowego jest określona w *HostingStartupApp.runtimeconfig.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="1c836-229">In the sample app (*HostingStartupApp*), the shared runtime is specified in the *HostingStartupApp.runtimeconfig.json* file.</span></span>

<span data-ttu-id="1c836-230">**Listy plików zależności startowe hostingu**</span><span class="sxs-lookup"><span data-stu-id="1c836-230">**List the hosting startup's dependencies file**</span></span>

<span data-ttu-id="1c836-231">Lokalizacja wdrożenia  *\*. deps.json* plik znajduje się w `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="1c836-231">The location of the implementation's *\*.deps.json* file is listed in the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="1c836-232">Jeśli plik zostanie umieszczony w profilu użytkownika *.dotnet* folder, ustaw wartość zmiennej środowiskowej:</span><span class="sxs-lookup"><span data-stu-id="1c836-232">If the file is placed in the user profile's *.dotnet* folder, set the environment variable's value to:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1c836-233">Windows</span><span class="sxs-lookup"><span data-stu-id="1c836-233">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1c836-234">macOS</span><span class="sxs-lookup"><span data-stu-id="1c836-234">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1c836-235">Linux</span><span class="sxs-lookup"><span data-stu-id="1c836-235">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

<span data-ttu-id="1c836-236">Jeśli plik zostanie umieszczony w instalacji programu .NET Core do użytku globalnego, należy podać pełną ścieżkę do pliku:</span><span class="sxs-lookup"><span data-stu-id="1c836-236">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1c836-237">Windows</span><span class="sxs-lookup"><span data-stu-id="1c836-237">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1c836-238">macOS</span><span class="sxs-lookup"><span data-stu-id="1c836-238">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1c836-239">Linux</span><span class="sxs-lookup"><span data-stu-id="1c836-239">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

<span data-ttu-id="1c836-240">Aby uzyskać przykładową aplikację (*HostingStartupApp*) można znaleźć pliku zależności (*HostingStartupApp.runtimeconfig.json*), plik zależności jest umieszczany w profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1c836-240">For the sample app (*HostingStartupApp*) to find the dependencies file (*HostingStartupApp.runtimeconfig.json*), the dependencies file is placed in the user's profile.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1c836-241">Windows</span><span class="sxs-lookup"><span data-stu-id="1c836-241">Windows</span></span>](#tab/windows)

<span data-ttu-id="1c836-242">Ustaw `DOTNET_ADDITIONAL_DEPS` zmienną środowiskową na następującą wartość:</span><span class="sxs-lookup"><span data-stu-id="1c836-242">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1c836-243">macOS</span><span class="sxs-lookup"><span data-stu-id="1c836-243">macOS</span></span>](#tab/macos)

<span data-ttu-id="1c836-244">Ustaw `DOTNET_ADDITIONAL_DEPS` zmienną środowiskową na następującą wartość:</span><span class="sxs-lookup"><span data-stu-id="1c836-244">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1c836-245">Linux</span><span class="sxs-lookup"><span data-stu-id="1c836-245">Linux</span></span>](#tab/linux)

<span data-ttu-id="1c836-246">Ustaw `DOTNET_ADDITIONAL_DEPS` zmienną środowiskową na następującą wartość:</span><span class="sxs-lookup"><span data-stu-id="1c836-246">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

<span data-ttu-id="1c836-247">Aby zapoznać się z przykładami sposobu ustawiania zmiennych środowiskowych dla różnych systemów operacyjnych, zobacz [używanie wielu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="1c836-247">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="1c836-248">**Wdrażanie**</span><span class="sxs-lookup"><span data-stu-id="1c836-248">**Deployment**</span></span>

<span data-ttu-id="1c836-249">W celu ułatwienia wdrażania hostingu uruchamiania w środowisku multimachine, przykładowa aplikacja tworzy *wdrożenia* folderu w opublikowanych danych wyjściowych, który zawiera:</span><span class="sxs-lookup"><span data-stu-id="1c836-249">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="1c836-250">Zestaw startowy hostingu.</span><span class="sxs-lookup"><span data-stu-id="1c836-250">The hosting startup assembly.</span></span>
* <span data-ttu-id="1c836-251">Plik hostingu zależności startowe.</span><span class="sxs-lookup"><span data-stu-id="1c836-251">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="1c836-252">Skrypt programu PowerShell, który tworzy lub modyfikuje `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` i `DOTNET_ADDITIONAL_DEPS` do obsługi aktywacji uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="1c836-252">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="1c836-253">Uruchom skrypt w administracyjnym wierszu polecenia programu PowerShell w systemie wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="1c836-253">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="1c836-254">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="1c836-254">NuGet package</span></span>

<span data-ttu-id="1c836-255">Można podać hostingu ulepszenie uruchamiania pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="1c836-255">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="1c836-256">Pakiet ma `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1c836-256">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="1c836-257">Hostingu typy uruchamiania, dostarczone przez pakiet są dostępne dla aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="1c836-257">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="1c836-258">Plik projektu aplikacji rozszerzone sprawia, że odwołania do pakietu do uruchomienia hostingu w pliku projektu aplikacji (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1c836-258">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="1c836-259">Za pomocą odwołania w czasie kompilacji w miejscu, zestawie hostingu uruchamiania i wszystkie jego zależności są włączone do pliku zależności aplikacji (*\*. deps.json*).</span><span class="sxs-lookup"><span data-stu-id="1c836-259">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="1c836-260">Takie podejście stosuje się do hostingu pakietu zestawu startowego publikowane w [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="1c836-260">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="1c836-261">Plik zależności startowe hostingu ma zostać udostępnione rozszerzone aplikacji zgodnie z opisem w [magazynu środowiska uruchomieniowego](#runtime-store) sekcji (bez odwołania do kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1c836-261">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="1c836-262">Aby uzyskać więcej informacji na temat pakietów NuGet i magazynem środowiska uruchomieniowego zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="1c836-262">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="1c836-263">Jak utworzyć pakiet NuGet za pomocą narzędzi międzyplatformowych</span><span class="sxs-lookup"><span data-stu-id="1c836-263">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="1c836-264">Publikowanie pakietów</span><span class="sxs-lookup"><span data-stu-id="1c836-264">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="1c836-265">Magazyn pakietu środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="1c836-265">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="1c836-266">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="1c836-266">Project bin folder</span></span>

<span data-ttu-id="1c836-267">Hostingu ulepszenie uruchamiania mogą być zapewniane przez *bin*-wdrożone zestawu w rozszerzonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c836-267">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="1c836-268">Typy uruchamiania hostingu, dostarczone przez zestaw są dostępne dla aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="1c836-268">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="1c836-269">Plik projektu aplikacji rozszerzone sprawia, że odwołanie do zestawu do uruchomienia hostingu (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1c836-269">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="1c836-270">Za pomocą odwołania w czasie kompilacji w miejscu, zestawie hostingu uruchamiania i wszystkie jego zależności są włączone do pliku zależności aplikacji (*\*. deps.json*).</span><span class="sxs-lookup"><span data-stu-id="1c836-270">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="1c836-271">Takie podejście ma zastosowanie, gdy scenariusz wdrażania wymaga, aby przenoszenia zestawu skompilowanego hostingu uruchamiania biblioteki (plik DLL) do konsumencki projektu lub do lokalizacji dostępne przez konsumencki projekt i kompilacji odnosi się do hostingu zestaw startowy firmy.</span><span class="sxs-lookup"><span data-stu-id="1c836-271">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="1c836-272">Plik zależności startowe hostingu ma zostać udostępnione rozszerzone aplikacji zgodnie z opisem w [magazynu środowiska uruchomieniowego](#runtime-store) sekcji (bez odwołania do kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1c836-272">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="1c836-273">Przykładowy kod</span><span class="sxs-lookup"><span data-stu-id="1c836-273">Sample code</span></span>

<span data-ttu-id="1c836-274">[Przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)) pokazuje hostingu scenariuszom dla programów próbnych implementacji:</span><span class="sxs-lookup"><span data-stu-id="1c836-274">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="1c836-275">Dwa hostingu zestawy startowe (bibliotek klas) Ustaw parę pary klucz wartość w pamięci konfiguracji poszczególnych:</span><span class="sxs-lookup"><span data-stu-id="1c836-275">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="1c836-276">Pakiet NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="1c836-276">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="1c836-277">Biblioteka klas (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="1c836-277">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="1c836-278">Uruchamianie hostingu została aktywowana w zestawie środowiska uruchomieniowego wdrożonych w magazynie (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="1c836-278">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="1c836-279">Zestaw dodaje dwa middlewares do aplikacji podczas uruchamiania, które zawierają informacje diagnostyczne dotyczące:</span><span class="sxs-lookup"><span data-stu-id="1c836-279">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="1c836-280">Zarejestrowane usługi</span><span class="sxs-lookup"><span data-stu-id="1c836-280">Registered services</span></span>
  * <span data-ttu-id="1c836-281">Adres (schematu, hosta, podstawa ścieżki, ścieżki, ciąg zapytania)</span><span class="sxs-lookup"><span data-stu-id="1c836-281">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="1c836-282">Połączenia (zdalnego adresu IP, port zdalny lokalnego adresu IP, port lokalny, certyfikatu klienta)</span><span class="sxs-lookup"><span data-stu-id="1c836-282">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="1c836-283">Nagłówki żądania</span><span class="sxs-lookup"><span data-stu-id="1c836-283">Request headers</span></span>
  * <span data-ttu-id="1c836-284">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="1c836-284">Environment variables</span></span>

<span data-ttu-id="1c836-285">Do uruchomienia przykładu:</span><span class="sxs-lookup"><span data-stu-id="1c836-285">To run the sample:</span></span>

<span data-ttu-id="1c836-286">**Aktywacja z pakietu NuGet**</span><span class="sxs-lookup"><span data-stu-id="1c836-286">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="1c836-287">Skompilować *HostingStartupPackage* pakietu z [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia.</span><span class="sxs-lookup"><span data-stu-id="1c836-287">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="1c836-288">Dodaj nazwę zestawu pakietu *HostingStartupPackage* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="1c836-288">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="1c836-289">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1c836-289">Compile and run the app.</span></span> <span data-ttu-id="1c836-290">Odwołanie do pakietu jest obecny w aplikacji rozszerzone (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1c836-290">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="1c836-291">A `<PropertyGroup>` w projekcie aplikacji plik Określa danych wyjściowych projektu pakietu (*... / HostingStartupPackage/bin/Debug*) jako źródło pakietów.</span><span class="sxs-lookup"><span data-stu-id="1c836-291">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="1c836-292">Umożliwia to aplikacji na użycie pakietu bez przekazywania pakietu, który ma [nuget.org](https://www.nuget.org/). Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="1c836-292">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="1c836-293">Sprawdź, czy wartości klucza konfiguracji usługi świadczone przez stronę indeksu odpowiadają wartości ustawione przy użyciu pakietu `ServiceKeyInjection.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="1c836-293">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="1c836-294">W przypadku wprowadzenia zmian do *HostingStartupPackage* projektu i ponownie ją kompilując, wyczyść pamięciach podręcznych pakietu NuGet, aby upewnić się, że *HostingStartupApp* otrzymuje zaktualizowany pakiet i nie nieodświeżone pakiet z lokalnej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="1c836-294">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="1c836-295">Aby wyczyścić pamięciach podręcznych NuGet, wykonaj następujące czynności, [lokalne nuget dotnet](/dotnet/core/tools/dotnet-nuget-locals) polecenia:</span><span class="sxs-lookup"><span data-stu-id="1c836-295">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="1c836-296">**Aktywacja z biblioteki klas**</span><span class="sxs-lookup"><span data-stu-id="1c836-296">**Activation from a class library**</span></span>

1. <span data-ttu-id="1c836-297">Skompilować *HostingStartupLibrary* biblioteki klas z [kompilacji dotnet](/dotnet/core/tools/dotnet-build) polecenia.</span><span class="sxs-lookup"><span data-stu-id="1c836-297">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="1c836-298">Dodaj nazwę zestawu biblioteki klas *HostingStartupLibrary* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="1c836-298">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="1c836-299">*pojemnik*— wdrażanie zestawu biblioteki klas w aplikacji, kopiując *HostingStartupLibrary.dll* pliku z biblioteki klas skompilowanych danych wyjściowych w aplikacji *bin/Debug* folderu.</span><span class="sxs-lookup"><span data-stu-id="1c836-299">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="1c836-300">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1c836-300">Compile and run the app.</span></span> <span data-ttu-id="1c836-301">`<ItemGroup>` w projekcie aplikacji plik odwołuje się do zestawu biblioteki klas (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1c836-301">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="1c836-302">Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="1c836-302">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="1c836-303">Sprawdź, czy wartości klucza konfiguracji usługi świadczone przez stronę indeksu odpowiadają wartości ustawione przez bibliotekę klas `ServiceKeyInjection.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="1c836-303">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="1c836-304">**Aktywacja w zestawie środowiska uruchomieniowego wdrożonych w magazynie**</span><span class="sxs-lookup"><span data-stu-id="1c836-304">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="1c836-305">*StartupDiagnostics* projekt używa [PowerShell](/powershell/scripting/powershell-scripting) do modyfikowania jej *StartupDiagnostics.deps.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="1c836-305">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="1c836-306">Program PowerShell jest instalowany domyślnie w systemie Windows, począwszy od Windows 7 z dodatkiem SP1 i Windows Server 2008 R2 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="1c836-306">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="1c836-307">Aby uzyskać programu PowerShell na innych platformach, zobacz [Instalowanie programu Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="1c836-307">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="1c836-308">Tworzenie *StartupDiagnostics* projektu.</span><span class="sxs-lookup"><span data-stu-id="1c836-308">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="1c836-309">Po projekt został utworzony, element docelowy kompilacji, w pliku projektu automatycznie:</span><span class="sxs-lookup"><span data-stu-id="1c836-309">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="1c836-310">Uruchamia skrypt programu PowerShell w celu zmodyfikowania *StartupDiagnostics.deps.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="1c836-310">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="1c836-311">Przenosi *StartupDiagnostics.deps.json* pliku do profilu użytkownika *additionalDeps* folderu.</span><span class="sxs-lookup"><span data-stu-id="1c836-311">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="1c836-312">Wykonaj `dotnet store` polecenia na command prompt w hostingu uruchamiania katalog do przechowywania zestawu i jego zależności w magazynie środowiska uruchomieniowego profilu użytkownika:</span><span class="sxs-lookup"><span data-stu-id="1c836-312">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="1c836-313">Windows, polecenie używa `win7-x64` [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="1c836-313">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="1c836-314">Podczas dostarczania hostingu uruchamiania różnych środowiska uruchomieniowego, podstawić poprawne identyfikatorów RID.</span><span class="sxs-lookup"><span data-stu-id="1c836-314">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="1c836-315">Ustaw zmienne środowiskowe:</span><span class="sxs-lookup"><span data-stu-id="1c836-315">Set the environment variables:</span></span>
   * <span data-ttu-id="1c836-316">Dodaj nazwę zestawu *StartupDiagnostics* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="1c836-316">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="1c836-317">Na Windows, ustaw `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej, aby `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span><span class="sxs-lookup"><span data-stu-id="1c836-317">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="1c836-318">W systemie macOS/Linux, należy ustawić `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej, aby `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, gdzie `<USER>` jest profil użytkownika, który zawiera uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="1c836-318">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="1c836-319">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="1c836-319">Run the sample app.</span></span>
1. <span data-ttu-id="1c836-320">Żądanie `/services` punktu końcowego, aby wyświetlić aplikację zarejestrowane usługi.</span><span class="sxs-lookup"><span data-stu-id="1c836-320">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="1c836-321">Żądanie `/diag` punktu końcowego, aby wyświetlić informacje diagnostyczne.</span><span class="sxs-lookup"><span data-stu-id="1c836-321">Request the `/diag` endpoint to see the diagnostic information.</span></span>
