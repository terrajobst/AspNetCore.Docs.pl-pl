---
title: Korzystanie z obsługi zestawów uruchamiania w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak poprawić aplikacji ASP.NET Core z zestawu zewnętrznego za pomocą interfejsu IHostingStartup implementację.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/02/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 3be036d9b4fc6c9898faf14e8a60a8cc7a8683b7
ms.sourcegitcommit: b5e63714afc26e94be49a92619586df5189ed93a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739548"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a><span data-ttu-id="f09dd-103">Korzystanie z obsługi zestawów uruchamiania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f09dd-103">Use hosting startup assemblies in ASP.NET Core</span></span>

<span data-ttu-id="f09dd-104">Przez [Luke Latham](https://github.com/guardrex) i [Pavel Krymets](https://github.com/pakrym)</span><span class="sxs-lookup"><span data-stu-id="f09dd-104">By [Luke Latham](https://github.com/guardrex) and [Pavel Krymets](https://github.com/pakrym)</span></span>

<span data-ttu-id="f09dd-105">[Interfejsu IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hostuje uruchamiania) implementacja dodaje rozszerzenia do aplikacji przy uruchamianiu z zestawu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="f09dd-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="f09dd-106">Na przykład zewnętrznej biblioteki służy hostingu implementacji uruchamiania zapewnienie dodatkowej konfiguracji dostawcy lub usług do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="f09dd-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f09dd-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="f09dd-108">Atrybut HostingStartup</span><span class="sxs-lookup"><span data-stu-id="f09dd-108">HostingStartup attribute</span></span>

<span data-ttu-id="f09dd-109">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut wskazuje obecność hostingu zestaw startowy można aktywować w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="f09dd-109">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="f09dd-110">Zestaw wpisu lub zestawu zawierającego `Startup` automatycznie klasy jest skanowany pod kątem `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-110">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="f09dd-111">Lista zestawów, aby wyszukać `HostingStartup` atrybutów jest ładowany w czasie wykonywania z konfiguracji w [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="f09dd-111">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="f09dd-112">Lista zestawów do wykluczenia z odnajdywania jest ładowany z [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="f09dd-112">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="f09dd-113">Aby uzyskać więcej informacji, [Zobacz host sieci Web: Hostowanie zestawów](xref:fundamentals/host/web-host#hosting-startup-assemblies) startowych i [hosta sieci Web: Hostowanie wykluczania zestawów](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="f09dd-113">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="f09dd-114">W poniższym przykładzie jest przestrzeń nazw w zestawie hostingu uruchamiania `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="f09dd-114">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="f09dd-115">Klasa zawierająca kod uruchamiający hostingu jest `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="f09dd-115">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="f09dd-116">`HostingStartup` Atrybutu znajduje się w zestawie uruchomienia hostingu `IHostingStartup` pliku z implementacją klasy.</span><span class="sxs-lookup"><span data-stu-id="f09dd-116">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="f09dd-117">Odkryj załadowanych zestawów uruchomienia hostingu</span><span class="sxs-lookup"><span data-stu-id="f09dd-117">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="f09dd-118">Aby odnaleźć załadowanych zestawów uruchomienia hostingu, Włącz rejestrowanie i zapoznać się z dziennikami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-118">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="f09dd-119">Błędy występujące podczas ładowania zestawów są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="f09dd-119">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="f09dd-120">Załadowano hostingu zestawy startowe są rejestrowane na poziomie debugowania, a wszystkie błędy są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="f09dd-120">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="f09dd-121">Wyłącz automatyczne ładowanie hostingu zestawy startowe</span><span class="sxs-lookup"><span data-stu-id="f09dd-121">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="f09dd-122">Aby wyłączyć automatyczne ładowanie hostingu zestawy startowe, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="f09dd-122">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="f09dd-123">Aby zapobiec temu wszystkie zestawy startowe hostingu, ładowania, należy ustawić jedną z następujących czynności, aby `true` lub `1`:</span><span class="sxs-lookup"><span data-stu-id="f09dd-123">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="f09dd-124">[Zapobieganie uruchamianiu hostingu](xref:fundamentals/host/web-host#prevent-hosting-startup) hosta ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-124">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="f09dd-125">`ASPNETCORE_PREVENTHOSTINGSTARTUP` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-125">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="f09dd-126">Aby zapobiec określonych zestawów uruchomienia hostingu, ładowanie, ustawić jedną z następujących hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania ciągu rozdzielone średnikami:</span><span class="sxs-lookup"><span data-stu-id="f09dd-126">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="f09dd-127">[Hosting uruchamiania wykluczyć zestawy](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) hosta ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-127">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="f09dd-128">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-128">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="f09dd-129">Jeśli ustawienia konfiguracji hosta i zmienną środowiskową są skonfigurowane, ustawienie hosta steruje zachowaniem.</span><span class="sxs-lookup"><span data-stu-id="f09dd-129">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="f09dd-130">Wyłączenie hostingu zestawy startowe przy użyciu zmiennej ustawienie lub środowisko hosta globalnie wyłącza zestawu i może wyłączyć pewne cechy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-130">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="f09dd-131">Projekt</span><span class="sxs-lookup"><span data-stu-id="f09dd-131">Project</span></span>

<span data-ttu-id="f09dd-132">Tworzenie uruchomienia hostingu za pomocą jednej z poniższych typów projektów:</span><span class="sxs-lookup"><span data-stu-id="f09dd-132">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="f09dd-133">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="f09dd-133">Class library</span></span>](#class-library)
* [<span data-ttu-id="f09dd-134">Aplikacja konsoli bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="f09dd-134">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="f09dd-135">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="f09dd-135">Class library</span></span>

<span data-ttu-id="f09dd-136">Można podać hostingu ulepszenie uruchamiania w bibliotece klas.</span><span class="sxs-lookup"><span data-stu-id="f09dd-136">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="f09dd-137">Biblioteka zawiera `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-137">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="f09dd-138">[Przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) obejmuje aplikacją stron Razor *HostingStartupApp*i biblioteki klas, *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="f09dd-138">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="f09dd-139">Biblioteka klas:</span><span class="sxs-lookup"><span data-stu-id="f09dd-139">The class library:</span></span>

* <span data-ttu-id="f09dd-140">Zawiera klasę uruchomienia hostingu `ServiceKeyInjection`, który implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f09dd-140">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="f09dd-141">`ServiceKeyInjection` Dodaje parę ciągów usługi konfiguracji aplikacji przy użyciu dostawcy konfiguracji w pamięci ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="f09dd-141">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="f09dd-142">Obejmuje `HostingStartup` atrybut, który identyfikuje przestrzeni nazw i klasy uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-142">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="f09dd-143">`ServiceKeyInjection` Klasy [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda używa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) na dodawanie rozszerzeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-143">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span>

<span data-ttu-id="f09dd-144">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="f09dd-144">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="f09dd-145">Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez bibliotekę klas hostingu uruchamiania zestawu:</span><span class="sxs-lookup"><span data-stu-id="f09dd-145">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="f09dd-146">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f09dd-146">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="f09dd-147">[Przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera także projekt pakietu NuGet, który zawiera osobne uruchomienia hostingu, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="f09dd-147">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="f09dd-148">Pakiet ma takie same charakterystyki biblioteki klas wcześniejszym opisem.</span><span class="sxs-lookup"><span data-stu-id="f09dd-148">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="f09dd-149">Pakiet:</span><span class="sxs-lookup"><span data-stu-id="f09dd-149">The package:</span></span>

* <span data-ttu-id="f09dd-150">Zawiera klasę uruchomienia hostingu `ServiceKeyInjection`, który implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f09dd-150">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="f09dd-151">`ServiceKeyInjection` dodaje dwa ciągi usługi do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-151">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="f09dd-152">Obejmuje `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-152">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="f09dd-153">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="f09dd-153">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="f09dd-154">Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez pakiet hostingu uruchamiania zestawu:</span><span class="sxs-lookup"><span data-stu-id="f09dd-154">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="f09dd-155">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f09dd-155">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="f09dd-156">Aplikacja konsoli bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="f09dd-156">Console app without an entry point</span></span>

<span data-ttu-id="f09dd-157">*Ta metoda jest dostępna tylko w przypadku aplikacji .NET Core, nie .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="f09dd-157">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="f09dd-158">Dynamiczne hostingu ulepszenie uruchamiania, która nie wymaga odwołania w czasie kompilacji w celu aktywacji można podać w aplikacji konsolowej bez punktu wejścia, który zawiera `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-158">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="f09dd-159">Publikowanie aplikacji konsoli daje hostingu zestaw startowy, który mogą być używane w sklepie środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="f09dd-159">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="f09dd-160">Aplikacja konsolowa bez punktu wejścia jest używany w ramach tego procesu, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="f09dd-160">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="f09dd-161">Do korzystania z hostingu uruchamiania w zestawie hostingu uruchamiania jest wymagana pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="f09dd-161">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="f09dd-162">Plik zależności jest trwały możliwy do uruchomienia aplikacji, który jest wytwarzany przez opublikowanie aplikacji, a nie z biblioteki.</span><span class="sxs-lookup"><span data-stu-id="f09dd-162">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="f09dd-163">Nie można dodać bezpośrednio do biblioteki [Magazyn pakietu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store), co wymaga możliwy do uruchomienia projektu, który jest przeznaczony dla udostępnionego środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="f09dd-163">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="f09dd-164">W przypadku tworzenia dynamicznego uruchamiania hostingu:</span><span class="sxs-lookup"><span data-stu-id="f09dd-164">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="f09dd-165">Zestaw startowy hostingu jest tworzony z aplikacji konsoli, bez punkt wejścia:</span><span class="sxs-lookup"><span data-stu-id="f09dd-165">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="f09dd-166">Zawiera klasę, która zawiera `IHostingStartup` implementacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-166">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="f09dd-167">Obejmuje [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut do identyfikowania `IHostingStartup` implementacji klasy.</span><span class="sxs-lookup"><span data-stu-id="f09dd-167">Includes a [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="f09dd-168">Aplikacja konsoli jest publikowany uzyskać zależności uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-168">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="f09dd-169">Konsekwencją publikowania aplikacji konsoli jest, że nieużywane zależności są usuwane z pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="f09dd-169">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="f09dd-170">Plik zależności jest modyfikowany do ustawiania lokalizacji środowiska uruchomieniowego, zestawu startowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-170">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="f09dd-171">Zestawie hostingu uruchamiania i jego zależności pliku jest umieszczany w Magazyn pakietu środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="f09dd-171">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="f09dd-172">Aby odnaleźć zestawie hostingu uruchamiania i jego zależności pliku, są wyświetlane w pary zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="f09dd-172">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="f09dd-173">Aplikacja konsoli odwołuje się do [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) pakietu:</span><span class="sxs-lookup"><span data-stu-id="f09dd-173">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="f09dd-174">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut określa klasę jako implementacja `IHostingStartup` w celu ładowania i wykonania podczas kompilowania [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="f09dd-174">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="f09dd-175">W poniższym przykładzie, przestrzeń nazw jest `StartupEnhancement`, a klasa jest `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="f09dd-175">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="f09dd-176">Klasa implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f09dd-176">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="f09dd-177">Klasa [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda używa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) na dodawanie rozszerzeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-177">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="f09dd-178">`IHostingStartup.Configure` w hostingu uruchamiania zestawu jest wywoływana przez środowisko wykonawcze przed `Startup.Configure` w kodzie użytkownika, co pozwala kod użytkownika zastąpić żadnej konfiguracji zawartym w zestawie hostingu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="f09dd-178">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="f09dd-179">Podczas kompilowania `IHostingStartup` projektu plik zależności ( *. deps. JSON*) ustawia `runtime` lokalizację zestawu do folderu *bin* :</span><span class="sxs-lookup"><span data-stu-id="f09dd-179">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="f09dd-180">Jest wyświetlana tylko część pliku.</span><span class="sxs-lookup"><span data-stu-id="f09dd-180">Only part of the file is shown.</span></span> <span data-ttu-id="f09dd-181">Nazwa zestawu w przykładzie jest `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="f09dd-181">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="f09dd-182">Konfiguracja dostarczona przez uruchomienie hostingu</span><span class="sxs-lookup"><span data-stu-id="f09dd-182">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="f09dd-183">Istnieją dwa podejścia do obsługi konfiguracji w zależności od tego, czy konfiguracja uruchamiania hostingu ma mieć pierwszeństwo, czy konfiguracja aplikacji ma mieć pierwszeństwo:</span><span class="sxs-lookup"><span data-stu-id="f09dd-183">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="f09dd-184">Podaj konfigurację aplikacji przy użyciu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> programu, aby załadować konfigurację po wykonaniu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegatów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-184">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="f09dd-185">Hostowanie konfiguracji uruchamiania ma wyższy priorytet niż konfiguracja aplikacji przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="f09dd-185">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="f09dd-186">Podaj konfigurację aplikacji przy użyciu <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> programu, aby załadować konfigurację przed wykonaniem <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegatów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-186">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="f09dd-187">Wartości konfiguracyjne aplikacji mają pierwszeństwo przed tymi, które są udostępniane przez uruchamianie hostingu przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="f09dd-187">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="f09dd-188">Określ hostingu zestawu startowego</span><span class="sxs-lookup"><span data-stu-id="f09dd-188">Specify the hosting startup assembly</span></span>

<span data-ttu-id="f09dd-189">Dla klasy biblioteki — albo konsoli aplikacji dostarczanego hostingu uruchamiania, określić nazwę zestawu startowego hostingu w `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-189">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="f09dd-190">Zmienna środowiskowa jest rozdzieloną średnikami listę zestawów.</span><span class="sxs-lookup"><span data-stu-id="f09dd-190">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="f09dd-191">Tylko hostingu zestawy startowe są skanowane pod kątem `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-191">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="f09dd-192">Dla przykładowej aplikacji *HostingStartupApp*, aby odnaleźć hostingu startupy, które opisano wcześniej, zmienna środowiskowa jest ustawiona na następującą wartość:</span><span class="sxs-lookup"><span data-stu-id="f09dd-192">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="f09dd-193">Zestaw startowy hostingu można również ustawić za pomocą [hostingu zestawy startowe](xref:fundamentals/host/web-host#hosting-startup-assemblies) hosta ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-193">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="f09dd-194">Podczas uruchamiania wielu hostingu składa są obecne, ich [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metody są wykonywane w kolejności, czy zestawy są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="f09dd-194">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="f09dd-195">Aktywacja</span><span class="sxs-lookup"><span data-stu-id="f09dd-195">Activation</span></span>

<span data-ttu-id="f09dd-196">Dostępne są następujące opcje do obsługi aktywacji uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="f09dd-196">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="f09dd-197">[Środowisko uruchomieniowe magazynu](#runtime-store) &ndash; aktywacji nie wymaga odwołania w czasie kompilacji w celu aktywacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-197">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="f09dd-198">Przykładowa aplikacja umieszcza hostingu zestawu startowe i zależności pliki do folderu, *wdrożenia*w celu ułatwienia wdrażania hostingu uruchamiania w środowisku multimachine.</span><span class="sxs-lookup"><span data-stu-id="f09dd-198">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="f09dd-199">*Wdrożenia* folder zawiera także skrypt programu PowerShell, który tworzy lub modyfikuje zmienne środowiskowe w systemie wdrożenia, aby włączyć uruchamianie hostingu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-199">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="f09dd-200">Dokumentacja kompilacji wymagany do aktywacji</span><span class="sxs-lookup"><span data-stu-id="f09dd-200">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="f09dd-201">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="f09dd-201">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="f09dd-202">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="f09dd-202">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="f09dd-203">Środowisko uruchomieniowe magazynu</span><span class="sxs-lookup"><span data-stu-id="f09dd-203">Runtime store</span></span>

<span data-ttu-id="f09dd-204">Hostingu implementacji uruchamiania jest umieszczany w [magazynu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="f09dd-204">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="f09dd-205">Odwołanie do zestawu kompilacji nie jest wymagane przez aplikację rozszerzone.</span><span class="sxs-lookup"><span data-stu-id="f09dd-205">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="f09dd-206">Po utworzeniu hostingu uruchamiania magazynu środowiska uruchomieniowego jest generowana z użyciem pliku manifestu projektu i [magazynu dotnet](/dotnet/core/tools/dotnet-store) polecenia.</span><span class="sxs-lookup"><span data-stu-id="f09dd-206">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="f09dd-207">W przykładowej aplikacji (*RuntimeStore* projektu) służy następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f09dd-207">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

``` console
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="f09dd-208">Dla środowiska uruchomieniowego odnaleźć magazyn środowiska uruchomieniowego, lokalizacja magazynu środowiska uruchomieniowego jest dodawana do `DOTNET_SHARED_STORE` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-208">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="f09dd-209">**Zmodyfikuj i umieść plik zależności startowe hostingu**</span><span class="sxs-lookup"><span data-stu-id="f09dd-209">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="f09dd-210">Aby aktywować rozszerzenie bez pakietu odwołania do poprawienia, określ dodatkowe zależności środowiska uruchomieniowego za pomocą `additionalDeps`.</span><span class="sxs-lookup"><span data-stu-id="f09dd-210">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="f09dd-211">`additionalDeps` Umożliwia:</span><span class="sxs-lookup"><span data-stu-id="f09dd-211">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="f09dd-212">Rozwiń Graf biblioteki aplikacji, dostarczając zestaw dodatkowych plików *. deps. JSON* do scalenia z własnym plikiem *. deps. JSON* podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="f09dd-212">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="f09dd-213">Należy hostingu zestawu startowego obciążana i prostsze do odnalezienia.</span><span class="sxs-lookup"><span data-stu-id="f09dd-213">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="f09dd-214">Jest zalecane podejście do generowania pliku dodatkowe zależności:</span><span class="sxs-lookup"><span data-stu-id="f09dd-214">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="f09dd-215">Wykonaj `dotnet publish` w pliku manifestu sklepu środowiska uruchomieniowego, do którego odwołuje się w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-215">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="f09dd-216">Usuń odwołanie do manifestu z bibliotek i `runtime` sekcji pliku z wynikiem *. deps. JSON* .</span><span class="sxs-lookup"><span data-stu-id="f09dd-216">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="f09dd-217">W przykładowym projekcie `store.manifest/1.0.0` właściwości zostanie usunięta z `targets` i `libraries` sekcji:</span><span class="sxs-lookup"><span data-stu-id="f09dd-217">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

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

<span data-ttu-id="f09dd-218">Umieść plik *. deps. JSON* w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="f09dd-218">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="f09dd-219">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Dodane do lokalizacji `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-219">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="f09dd-220">`{SHARED FRAMEWORK NAME}` &ndash; Udostępnione framework jest wymagany dla tego pliku dodatkowe zależności.</span><span class="sxs-lookup"><span data-stu-id="f09dd-220">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="f09dd-221">`{SHARED FRAMEWORK VERSION}` &ndash; Wersja minimalna udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="f09dd-221">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="f09dd-222">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; Rozszerzenie nazwy zestawu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-222">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="f09dd-223">W przykładowej aplikacji (*RuntimeStore* projektu), plik dodatkowe zależności jest umieszczany w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="f09dd-223">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="f09dd-224">Dla środowiska uruchomieniowego odnaleźć lokalizacji magazynu środowiska uruchomieniowego, lokalizacja pliku dodatkowe zależności jest dodawana do `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-224">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="f09dd-225">W przykładowej aplikacji (*RuntimeStore* projektu), tworzenie magazynu środowiska uruchomieniowego i generuje dodatkowe zależności pliku odbywa się przy użyciu [PowerShell](/powershell/scripting/powershell-scripting) skryptu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-225">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="f09dd-226">Aby zapoznać się z przykładami sposobu ustawiania zmiennych środowiskowych dla różnych systemów operacyjnych, zobacz [używanie wielu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="f09dd-226">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="f09dd-227">**Wdrażanie**</span><span class="sxs-lookup"><span data-stu-id="f09dd-227">**Deployment**</span></span>

<span data-ttu-id="f09dd-228">W celu ułatwienia wdrażania hostingu uruchamiania w środowisku multimachine, przykładowa aplikacja tworzy *wdrożenia* folderu w opublikowanych danych wyjściowych, który zawiera:</span><span class="sxs-lookup"><span data-stu-id="f09dd-228">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="f09dd-229">Uruchamianie magazyn środowisko uruchomieniowe hostingu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-229">The hosting startup runtime store.</span></span>
* <span data-ttu-id="f09dd-230">Plik hostingu zależności startowe.</span><span class="sxs-lookup"><span data-stu-id="f09dd-230">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="f09dd-231">Skrypt programu PowerShell, który tworzy lub modyfikuje `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, i `DOTNET_ADDITIONAL_DEPS` do obsługi aktywacji uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-231">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="f09dd-232">Uruchom skrypt w administracyjnym wierszu polecenia programu PowerShell w systemie wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="f09dd-232">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="f09dd-233">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="f09dd-233">NuGet package</span></span>

<span data-ttu-id="f09dd-234">Można podać hostingu ulepszenie uruchamiania pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="f09dd-234">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="f09dd-235">Pakiet ma `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-235">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="f09dd-236">Hostingu typy uruchamiania, dostarczone przez pakiet są dostępne dla aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="f09dd-236">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="f09dd-237">Plik projektu aplikacji rozszerzone sprawia, że odwołania do pakietu do uruchomienia hostingu w pliku projektu aplikacji (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="f09dd-237">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="f09dd-238">Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji ( *. deps. JSON*).</span><span class="sxs-lookup"><span data-stu-id="f09dd-238">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="f09dd-239">Takie podejście stosuje się do hostingu pakietu zestawu startowego publikowane w [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="f09dd-239">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="f09dd-240">Plik zależności startowe hostingu ma zostać udostępnione rozszerzone aplikacji zgodnie z opisem w [magazynu środowiska uruchomieniowego](#runtime-store) sekcji (bez odwołania do kompilacji).</span><span class="sxs-lookup"><span data-stu-id="f09dd-240">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="f09dd-241">Aby uzyskać więcej informacji na temat pakietów NuGet i magazynem środowiska uruchomieniowego zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="f09dd-241">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="f09dd-242">Jak utworzyć pakiet NuGet za pomocą narzędzi międzyplatformowych</span><span class="sxs-lookup"><span data-stu-id="f09dd-242">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="f09dd-243">Publikowanie pakietów</span><span class="sxs-lookup"><span data-stu-id="f09dd-243">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="f09dd-244">Magazyn pakietu środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="f09dd-244">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="f09dd-245">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="f09dd-245">Project bin folder</span></span>

<span data-ttu-id="f09dd-246">Hostingu ulepszenie uruchamiania mogą być zapewniane przez *bin*-wdrożone zestawu w rozszerzonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-246">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="f09dd-247">Typy uruchamiania hostingu udostępniane przez zestaw są udostępniane aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="f09dd-247">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="f09dd-248">Plik projektu aplikacji rozszerzone sprawia, że odwołanie do zestawu do uruchomienia hostingu (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="f09dd-248">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="f09dd-249">Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji ( *. deps. JSON*).</span><span class="sxs-lookup"><span data-stu-id="f09dd-249">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="f09dd-250">Takie podejście ma zastosowanie, gdy scenariusz wdrażania wywoła odwołanie do zestawu uruchamiania hostingu (plik *. dll* ) i przenosząc zestaw do jednego z tych metod:</span><span class="sxs-lookup"><span data-stu-id="f09dd-250">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="f09dd-251">Projekt zużywający.</span><span class="sxs-lookup"><span data-stu-id="f09dd-251">The consuming project.</span></span>
  * <span data-ttu-id="f09dd-252">Lokalizacja dostępna przez projekt zużywający.</span><span class="sxs-lookup"><span data-stu-id="f09dd-252">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="f09dd-253">Plik zależności startowe hostingu ma zostać udostępnione rozszerzone aplikacji zgodnie z opisem w [magazynu środowiska uruchomieniowego](#runtime-store) sekcji (bez odwołania do kompilacji).</span><span class="sxs-lookup"><span data-stu-id="f09dd-253">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="f09dd-254">Podczas określania .NET Framework, zestaw jest ładowany w domyślnym kontekście ładowania, który na .NET Framework oznacza, że zestaw znajduje się w jednej z następujących lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="f09dd-254">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="f09dd-255">Ścieżka &ndash; podstawowa aplikacji folder *bin* , w którym znajduje się plik wykonywalny ( *. exe*) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-255">Application base path &ndash; The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="f09dd-256">Globalna pamięć podręczna zestawów &ndash; (GAC) przechowuje zestawy, które udostępniają kilka .NET Framework aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f09dd-256">Global Assembly Cache (GAC) &ndash; The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="f09dd-257">Aby uzyskać więcej informacji, zobacz [jak: Zainstaluj zestaw w globalnej pamięci podręcznej](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) zestawów w dokumentacji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f09dd-257">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="f09dd-258">Przykładowy kod</span><span class="sxs-lookup"><span data-stu-id="f09dd-258">Sample code</span></span>

<span data-ttu-id="f09dd-259">[Przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)) pokazuje hostingu scenariuszom dla programów próbnych implementacji:</span><span class="sxs-lookup"><span data-stu-id="f09dd-259">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="f09dd-260">Dwa hostingu zestawy startowe (bibliotek klas) Ustaw parę pary klucz wartość w pamięci konfiguracji poszczególnych:</span><span class="sxs-lookup"><span data-stu-id="f09dd-260">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="f09dd-261">Pakiet NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="f09dd-261">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="f09dd-262">Biblioteka klas (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="f09dd-262">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="f09dd-263">Uruchamianie hostingu została aktywowana w zestawie środowiska uruchomieniowego wdrożonych w magazynie (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="f09dd-263">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="f09dd-264">Zestaw dodaje dwa middlewares do aplikacji podczas uruchamiania, które zawierają informacje diagnostyczne dotyczące:</span><span class="sxs-lookup"><span data-stu-id="f09dd-264">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="f09dd-265">Zarejestrowane usługi</span><span class="sxs-lookup"><span data-stu-id="f09dd-265">Registered services</span></span>
  * <span data-ttu-id="f09dd-266">Adres (schematu, hosta, podstawa ścieżki, ścieżki, ciąg zapytania)</span><span class="sxs-lookup"><span data-stu-id="f09dd-266">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="f09dd-267">Połączenia (zdalnego adresu IP, port zdalny lokalnego adresu IP, port lokalny, certyfikatu klienta)</span><span class="sxs-lookup"><span data-stu-id="f09dd-267">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="f09dd-268">Nagłówki żądań</span><span class="sxs-lookup"><span data-stu-id="f09dd-268">Request headers</span></span>
  * <span data-ttu-id="f09dd-269">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="f09dd-269">Environment variables</span></span>

<span data-ttu-id="f09dd-270">Do uruchomienia przykładu:</span><span class="sxs-lookup"><span data-stu-id="f09dd-270">To run the sample:</span></span>

<span data-ttu-id="f09dd-271">**Aktywacja z pakietu NuGet**</span><span class="sxs-lookup"><span data-stu-id="f09dd-271">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="f09dd-272">Skompilować *HostingStartupPackage* pakietu z [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia.</span><span class="sxs-lookup"><span data-stu-id="f09dd-272">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="f09dd-273">Dodaj nazwę zestawu pakietu *HostingStartupPackage* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-273">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="f09dd-274">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="f09dd-274">Compile and run the app.</span></span> <span data-ttu-id="f09dd-275">Odwołanie do pakietu jest obecny w aplikacji rozszerzone (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="f09dd-275">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="f09dd-276">A `<PropertyGroup>` w projekcie aplikacji plik Określa danych wyjściowych projektu pakietu ( *... / HostingStartupPackage/bin/Debug*) jako źródło pakietów.</span><span class="sxs-lookup"><span data-stu-id="f09dd-276">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="f09dd-277">Umożliwia to aplikacji na użycie pakietu bez przekazywania pakietu, który ma [nuget.org](https://www.nuget.org/). Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="f09dd-277">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="f09dd-278">Sprawdź, czy wartości klucza konfiguracji usługi świadczone przez stronę indeksu odpowiadają wartości ustawione przy użyciu pakietu `ServiceKeyInjection.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="f09dd-278">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="f09dd-279">W przypadku wprowadzenia zmian do *HostingStartupPackage* projektu i ponownie ją kompilując, wyczyść pamięciach podręcznych pakietu NuGet, aby upewnić się, że *HostingStartupApp* otrzymuje zaktualizowany pakiet i nie nieodświeżone pakiet z lokalnej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-279">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="f09dd-280">Aby wyczyścić pamięciach podręcznych NuGet, wykonaj następujące czynności, [lokalne nuget dotnet](/dotnet/core/tools/dotnet-nuget-locals) polecenia:</span><span class="sxs-lookup"><span data-stu-id="f09dd-280">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="f09dd-281">**Aktywacja z biblioteki klas**</span><span class="sxs-lookup"><span data-stu-id="f09dd-281">**Activation from a class library**</span></span>

1. <span data-ttu-id="f09dd-282">Skompilować *HostingStartupLibrary* biblioteki klas z [kompilacji dotnet](/dotnet/core/tools/dotnet-build) polecenia.</span><span class="sxs-lookup"><span data-stu-id="f09dd-282">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="f09dd-283">Dodaj nazwę zestawu biblioteki klas *HostingStartupLibrary* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-283">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="f09dd-284">*pojemnik*— wdrażanie zestawu biblioteki klas w aplikacji, kopiując *HostingStartupLibrary.dll* pliku z biblioteki klas skompilowanych danych wyjściowych w aplikacji *bin/Debug* folderu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-284">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="f09dd-285">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="f09dd-285">Compile and run the app.</span></span> <span data-ttu-id="f09dd-286">`<ItemGroup>` w projekcie aplikacji plik odwołuje się do zestawu biblioteki klas ( *.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="f09dd-286">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="f09dd-287">Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="f09dd-287">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="f09dd-288">Sprawdź, czy wartości klucza konfiguracji usługi świadczone przez stronę indeksu odpowiadają wartości ustawione przez bibliotekę klas `ServiceKeyInjection.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="f09dd-288">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="f09dd-289">**Aktywacja w zestawie środowiska uruchomieniowego wdrożonych w magazynie**</span><span class="sxs-lookup"><span data-stu-id="f09dd-289">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="f09dd-290">*StartupDiagnostics* projekt używa [PowerShell](/powershell/scripting/powershell-scripting) do modyfikowania jej *StartupDiagnostics.deps.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="f09dd-290">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="f09dd-291">Program PowerShell jest instalowany domyślnie w systemie Windows, począwszy od Windows 7 z dodatkiem SP1 i Windows Server 2008 R2 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="f09dd-291">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="f09dd-292">Aby uzyskać programu PowerShell na innych platformach, zobacz [Instalowanie programu Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="f09dd-292">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="f09dd-293">Wykonaj skrypt *Build. ps1* w folderze *RuntimeStore* .</span><span class="sxs-lookup"><span data-stu-id="f09dd-293">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="f09dd-294">Skrypt:</span><span class="sxs-lookup"><span data-stu-id="f09dd-294">The script:</span></span>
   * <span data-ttu-id="f09dd-295">`StartupDiagnostics` Generuje pakiet.</span><span class="sxs-lookup"><span data-stu-id="f09dd-295">Generates the `StartupDiagnostics` package.</span></span>
   * <span data-ttu-id="f09dd-296">Generuje magazyn środowiska uruchomieniowego `StartupDiagnostics` dla programu w folderze *Store* .</span><span class="sxs-lookup"><span data-stu-id="f09dd-296">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="f09dd-297">Polecenie w skrypcie `win7-x64` używa [identyfikatora środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) do uruchomienia hostingu wdrożonego w systemie Windows. `dotnet store`</span><span class="sxs-lookup"><span data-stu-id="f09dd-297">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="f09dd-298">Podczas zapewniania uruchamiania hostingu dla innego środowiska uruchomieniowego należy zastąpić prawidłowy identyfikator RID w wierszu 37 skryptu.</span><span class="sxs-lookup"><span data-stu-id="f09dd-298">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span>
   * <span data-ttu-id="f09dd-299">Generuje element `StartupDiagnostics`forw programie *additionalDeps/Shared/Microsoft. AspNetCore. app/{Shared Framework Version}/folder.* `additionalDeps`</span><span class="sxs-lookup"><span data-stu-id="f09dd-299">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps/shared/Microsoft.AspNetCore.App/{Shared Framework Version}/* folder.</span></span>
   * <span data-ttu-id="f09dd-300">Umieszcza plik *Deploy. ps1* w folderze *Deployment* .</span><span class="sxs-lookup"><span data-stu-id="f09dd-300">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="f09dd-301">Uruchom skrypt *Deploy. ps1* w folderze *Deployment* .</span><span class="sxs-lookup"><span data-stu-id="f09dd-301">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="f09dd-302">Dołączenie skryptu:</span><span class="sxs-lookup"><span data-stu-id="f09dd-302">The script appends:</span></span>
   * <span data-ttu-id="f09dd-303">`StartupDiagnostics`do zmiennej `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-303">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="f09dd-304">Ścieżka zależności uruchamiania hostingu do `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-304">The hosting startup dependencies path to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="f09dd-305">Ścieżka do magazynu środowiska uruchomieniowego `DOTNET_SHARED_STORE` do zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f09dd-305">The runtime store path to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="f09dd-306">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="f09dd-306">Run the sample app.</span></span>
1. <span data-ttu-id="f09dd-307">Żądanie `/services` punktu końcowego, aby wyświetlić aplikację zarejestrowane usługi.</span><span class="sxs-lookup"><span data-stu-id="f09dd-307">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="f09dd-308">Żądanie `/diag` punktu końcowego, aby wyświetlić informacje diagnostyczne.</span><span class="sxs-lookup"><span data-stu-id="f09dd-308">Request the `/diag` endpoint to see the diagnostic information.</span></span>
