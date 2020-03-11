---
title: Korzystanie z obsługi zestawów uruchamiania w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak poprawić aplikacji ASP.NET Core z zestawu zewnętrznego za pomocą interfejsu IHostingStartup implementację.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 71fd5cf1934b5374e0a393e055db23b98c03b62f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660399"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a><span data-ttu-id="1ae1e-103">Korzystanie z obsługi zestawów uruchamiania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ae1e-103">Use hosting startup assemblies in ASP.NET Core</span></span>

<span data-ttu-id="1ae1e-104">Autor [Pavel Krymets](https://github.com/pakrym)</span><span class="sxs-lookup"><span data-stu-id="1ae1e-104">By [Pavel Krymets](https://github.com/pakrym)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1ae1e-105">Implementacja <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (uruchamianie hostingu) dodaje usprawnienia do aplikacji podczas uruchamiania z zestawu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-105">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="1ae1e-106">Na przykład zewnętrznej biblioteki służy hostingu implementacji uruchamiania zapewnienie dodatkowej konfiguracji dostawcy lub usług do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="1ae1e-107">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1ae1e-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="1ae1e-108">Atrybut HostingStartup</span><span class="sxs-lookup"><span data-stu-id="1ae1e-108">HostingStartup attribute</span></span>

<span data-ttu-id="1ae1e-109">Atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) wskazuje obecność zestawu startowego hostingu do aktywowania w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-109">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="1ae1e-110">Zestaw wpisów lub zestaw zawierający klasę `Startup` jest automatycznie skanowany dla atrybutu `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-110">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="1ae1e-111">Lista zestawów do wyszukiwania atrybutów `HostingStartup` jest ładowana w czasie wykonywania z konfiguracji w [WebHostDefaults. HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-111">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span></span> <span data-ttu-id="1ae1e-112">Lista zestawów do wykluczenia z odnajdywania jest ładowana z [WebHostDefaults. HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-112">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span></span>

<span data-ttu-id="1ae1e-113">W poniższym przykładzie przestrzeń nazw zestawu startowego hostingu jest `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-113">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="1ae1e-114">Klasa zawierająca kod uruchamiania hostingu jest `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-114">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="1ae1e-115">Atrybut `HostingStartup` zwykle znajduje się w pliku klasy implementacji `IHostingStartup`, w którym znajduje się zestaw startowy.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-115">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="1ae1e-116">Odkryj załadowanych zestawów uruchomienia hostingu</span><span class="sxs-lookup"><span data-stu-id="1ae1e-116">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="1ae1e-117">Aby odnaleźć załadowanych zestawów uruchomienia hostingu, Włącz rejestrowanie i zapoznać się z dziennikami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-117">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="1ae1e-118">Błędy występujące podczas ładowania zestawów są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-118">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="1ae1e-119">Załadowano hostingu zestawy startowe są rejestrowane na poziomie debugowania, a wszystkie błędy są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-119">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="1ae1e-120">Wyłącz automatyczne ładowanie hostingu zestawy startowe</span><span class="sxs-lookup"><span data-stu-id="1ae1e-120">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="1ae1e-121">Aby wyłączyć automatyczne ładowanie hostingu zestawy startowe, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-121">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="1ae1e-122">Aby zapobiec ładowaniu wszystkich początkowych zestawów uruchamiania, należy ustawić jedną z następujących wartości `true` lub `1`:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-122">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>

  * <span data-ttu-id="1ae1e-123">Zapobiegaj ustawieniu konfiguracji hosta uruchamiania hostingu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-123">Prevent Hosting Startup host configuration setting:</span></span>

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseSetting(
                        WebHostDefaults.PreventHostingStartupKey, "true")
                    .UseStartup<Startup>();
            });
    ```

  * <span data-ttu-id="1ae1e-124">Zmienna środowiskowa `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-124">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

* <span data-ttu-id="1ae1e-125">Aby zapobiec określonych zestawów uruchomienia hostingu, ładowanie, ustawić jedną z następujących hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania ciągu rozdzielone średnikami:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-125">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>

  * <span data-ttu-id="1ae1e-126">Uruchamianie hostingu Wyklucz ustawienia konfiguracja hosta:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-126">Hosting Startup Exclude Assemblies host configuration setting:</span></span>

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseSetting(
                        WebHostDefaults.HostingStartupExcludeAssembliesKey, 
                        "{ASSEMBLY1;ASSEMBLY2; ...}")
                    .UseStartup<Startup>();
            });
    ```

  * <span data-ttu-id="1ae1e-127">Zmienna środowiskowa `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-127">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="1ae1e-128">Jeśli ustawienia konfiguracji hosta i zmienną środowiskową są skonfigurowane, ustawienie hosta steruje zachowaniem.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-128">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="1ae1e-129">Wyłączenie hostingu zestawy startowe przy użyciu zmiennej ustawienie lub środowisko hosta globalnie wyłącza zestawu i może wyłączyć pewne cechy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-129">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="1ae1e-130">Project</span><span class="sxs-lookup"><span data-stu-id="1ae1e-130">Project</span></span>

<span data-ttu-id="1ae1e-131">Tworzenie uruchomienia hostingu za pomocą jednej z poniższych typów projektów:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-131">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="1ae1e-132">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="1ae1e-132">Class library</span></span>](#class-library)
* [<span data-ttu-id="1ae1e-133">Aplikacja konsolowa bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="1ae1e-133">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="1ae1e-134">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="1ae1e-134">Class library</span></span>

<span data-ttu-id="1ae1e-135">Można podać hostingu ulepszenie uruchamiania w bibliotece klas.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-135">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="1ae1e-136">Biblioteka zawiera atrybut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-136">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="1ae1e-137">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera Razor Pages aplikacji, *HostingStartupApp*i biblioteki klas *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-137">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="1ae1e-138">Biblioteka klas:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-138">The class library:</span></span>

* <span data-ttu-id="1ae1e-139">Zawiera klasę uruchamiania hostingu, `ServiceKeyInjection`, która implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-139">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="1ae1e-140">`ServiceKeyInjection` dodaje parę ciągów usługi do konfiguracji aplikacji za pomocą dostawcy konfiguracji w pamięci ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-140">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span></span>
* <span data-ttu-id="1ae1e-141">Zawiera atrybut `HostingStartup`, który identyfikuje przestrzeń nazw i klasę uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-141">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="1ae1e-142">Metoda <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> klasy `ServiceKeyInjection` używa <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> do dodawania ulepszeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-142">The `ServiceKeyInjection` class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span>

<span data-ttu-id="1ae1e-143">*HostingStartupLibrary/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-143">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="1ae1e-144">Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez bibliotekę klas hostingu uruchamiania zestawu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-144">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="1ae1e-145">*HostingStartupApp/Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-145">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="1ae1e-146">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera również projekt pakietu NuGet, który zapewnia oddzielne uruchamianie hostingu, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-146">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="1ae1e-147">Pakiet ma takie same charakterystyki biblioteki klas wcześniejszym opisem.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-147">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="1ae1e-148">Pakiet:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-148">The package:</span></span>

* <span data-ttu-id="1ae1e-149">Zawiera klasę uruchamiania hostingu, `ServiceKeyInjection`, która implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-149">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="1ae1e-150">`ServiceKeyInjection` dodaje parę ciągów usługi do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-150">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="1ae1e-151">Zawiera atrybut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-151">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="1ae1e-152">*HostingStartupPackage/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-152">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="1ae1e-153">Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez pakiet hostingu uruchamiania zestawu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-153">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="1ae1e-154">*HostingStartupApp/Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-154">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="1ae1e-155">Aplikacja konsoli bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="1ae1e-155">Console app without an entry point</span></span>

<span data-ttu-id="1ae1e-156">*Ta metoda jest dostępna tylko dla aplikacji platformy .NET Core, a nie .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="1ae1e-156">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="1ae1e-157">Rozszerzenie dynamicznego uruchamiania hostingu, które nie wymaga odwołania do czasu kompilowania dla aktywacji, można dostarczyć w aplikacji konsolowej bez punktu wejścia, który zawiera atrybut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-157">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="1ae1e-158">Publikowanie aplikacji konsoli daje hostingu zestaw startowy, który mogą być używane w sklepie środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-158">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="1ae1e-159">Aplikacja konsolowa bez punktu wejścia jest używany w ramach tego procesu, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-159">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="1ae1e-160">Do korzystania z hostingu uruchamiania w zestawie hostingu uruchamiania jest wymagana pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-160">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="1ae1e-161">Plik zależności jest trwały możliwy do uruchomienia aplikacji, który jest wytwarzany przez opublikowanie aplikacji, a nie z biblioteki.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-161">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="1ae1e-162">Biblioteki nie można dodać bezpośrednio do [magazynu pakietów środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store), który wymaga projektu możliwy do uruchomienia, który jest przeznaczony dla udostępnionego środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-162">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="1ae1e-163">W przypadku tworzenia dynamicznego uruchamiania hostingu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-163">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="1ae1e-164">Zestaw startowy hostingu jest tworzony z aplikacji konsoli, bez punkt wejścia:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-164">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="1ae1e-165">Zawiera klasę, która zawiera implementację `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-165">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="1ae1e-166">Zawiera atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) do identyfikacji klasy implementacji `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-166">Includes a [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="1ae1e-167">Aplikacja konsoli jest publikowany uzyskać zależności uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-167">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="1ae1e-168">Konsekwencją publikowania aplikacji konsoli jest, że nieużywane zależności są usuwane z pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-168">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="1ae1e-169">Plik zależności jest modyfikowany do ustawiania lokalizacji środowiska uruchomieniowego, zestawu startowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-169">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="1ae1e-170">Zestawie hostingu uruchamiania i jego zależności pliku jest umieszczany w Magazyn pakietu środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-170">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="1ae1e-171">Aby odnaleźć zestawie hostingu uruchamiania i jego zależności pliku, są wyświetlane w pary zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-171">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="1ae1e-172">Aplikacja konsolowa odwołuje się do pakietu [Microsoft. AspNetCore. hosting. Abstracts](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) :</span><span class="sxs-lookup"><span data-stu-id="1ae1e-172">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.csproj)]

<span data-ttu-id="1ae1e-173">Atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) identyfikuje klasę jako implementację `IHostingStartup` do załadowania i wykonania podczas kompilowania <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-173">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span></span> <span data-ttu-id="1ae1e-174">W poniższym przykładzie przestrzeń nazw jest `StartupEnhancement`, a Klasa jest `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-174">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="1ae1e-175">Klasa implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-175">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="1ae1e-176">Metoda <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> klasy używa <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> do dodawania ulepszeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-176">The class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span> <span data-ttu-id="1ae1e-177">`IHostingStartup.Configure` w zestawie uruchamiania hostingu jest wywoływany przez środowisko uruchomieniowe przed `Startup.Configure` w kodzie użytkownika, dzięki czemu kod użytkownika może zastępować każdą konfigurację podaną przez zestaw startowy hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-177">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="1ae1e-178">Podczas kompilowania projektu `IHostingStartup` plik zależności ( *. deps. JSON*) ustawia lokalizację `runtime` zestawu do folderu *bin* :</span><span class="sxs-lookup"><span data-stu-id="1ae1e-178">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="1ae1e-179">Jest wyświetlana tylko część pliku.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-179">Only part of the file is shown.</span></span> <span data-ttu-id="1ae1e-180">Nazwa zestawu w przykładzie jest `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-180">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="1ae1e-181">Konfiguracja dostarczona przez uruchomienie hostingu</span><span class="sxs-lookup"><span data-stu-id="1ae1e-181">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="1ae1e-182">Istnieją dwa podejścia do obsługi konfiguracji w zależności od tego, czy konfiguracja uruchamiania hostingu ma mieć pierwszeństwo, czy konfiguracja aplikacji ma mieć pierwszeństwo:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-182">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="1ae1e-183">Podaj konfigurację aplikacji przy użyciu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*>, aby załadować konfigurację po wykonaniu delegatów <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-183">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="1ae1e-184">Hostowanie konfiguracji uruchamiania ma wyższy priorytet niż konfiguracja aplikacji przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-184">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="1ae1e-185">Podaj konfigurację aplikacji przy użyciu <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> do załadowania konfiguracji przed wykonaniem delegatów <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-185">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="1ae1e-186">Wartości konfiguracyjne aplikacji mają pierwszeństwo przed tymi, które są udostępniane przez uruchamianie hostingu przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-186">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

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
                    "From IHostingStartup: Higher priority " +
                    "than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority " +
                "than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="1ae1e-187">Określ hostingu zestawu startowego</span><span class="sxs-lookup"><span data-stu-id="1ae1e-187">Specify the hosting startup assembly</span></span>

<span data-ttu-id="1ae1e-188">W przypadku biblioteki klas lub uruchamiania hostingu obsługiwanej przez aplikację konsoli Określ nazwę zestawu startowego hostingu w zmiennej środowiskowej `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-188">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="1ae1e-189">Zmienna środowiskowa jest rozdzieloną średnikami listę zestawów.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-189">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="1ae1e-190">Tylko hosting zestawów startowych jest skanowany pod kątem atrybutu `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-190">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="1ae1e-191">W przypadku przykładowej aplikacji, *HostingStartupApp*, aby odnaleźć opisane wcześniej uruchomienia hostingu, zmienna środowiskowa jest ustawiona na następującą wartość:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-191">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="1ae1e-192">Zestaw startowy obsługujący hosting można również ustawić przy użyciu ustawienia konfiguracji hosta zestawów uruchamiania hostingu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-192">A hosting startup assembly can also be set using the Hosting Startup Assemblies host configuration setting:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseSetting(
                    WebHostDefaults.HostingStartupAssembliesKey, 
                    "{ASSEMBLY1;ASSEMBLY2; ...}")
                .UseStartup<Startup>();
        });
```

<span data-ttu-id="1ae1e-193">Gdy są obecne wiele asemblerów uruchamiania hostingu, ich metody <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> są wykonywane w kolejności, w jakiej są wymienione zestawy.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-193">When multiple hosting startup assembles are present, their <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="1ae1e-194">Aktywacja</span><span class="sxs-lookup"><span data-stu-id="1ae1e-194">Activation</span></span>

<span data-ttu-id="1ae1e-195">Dostępne są następujące opcje do obsługi aktywacji uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-195">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="1ae1e-196">Aktywacja [magazynu w środowisku uruchomieniowym](#runtime-store) &ndash; nie wymaga odwołania do czasu kompilacji na potrzeby aktywacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-196">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="1ae1e-197">Przykładowa aplikacja umieszcza zestaw startowy obsługujący i pliki zależności w folderze, *wdrożeniu*, aby ułatwić wdrożenie uruchamiania hostingu w środowisku wielomaszynowym.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-197">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="1ae1e-198">Folder *wdrożenia* zawiera także skrypt programu PowerShell, który tworzy lub modyfikuje zmienne środowiskowe w systemie wdrażania, aby umożliwić uruchamianie hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-198">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="1ae1e-199">Dokumentacja kompilacji wymagany do aktywacji</span><span class="sxs-lookup"><span data-stu-id="1ae1e-199">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="1ae1e-200">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="1ae1e-200">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="1ae1e-201">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="1ae1e-201">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="1ae1e-202">Środowisko uruchomieniowe magazynu</span><span class="sxs-lookup"><span data-stu-id="1ae1e-202">Runtime store</span></span>

<span data-ttu-id="1ae1e-203">Implementacja uruchamiania hostingu jest umieszczana w [magazynie w czasie wykonywania](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-203">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="1ae1e-204">Odwołanie do zestawu kompilacji nie jest wymagane przez aplikację rozszerzone.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-204">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="1ae1e-205">Po skompilowaniu uruchomienia hostingu magazyn środowiska uruchomieniowego jest generowany przy użyciu pliku projektu manifestu i polecenia [magazynu dotnet](/dotnet/core/tools/dotnet-store) .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-205">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="1ae1e-206">W przykładowej aplikacji (projekt*RuntimeStore* ) używane jest następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-206">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="1ae1e-207">Aby środowisko uruchomieniowe wykrywało magazyn środowiska uruchomieniowego, lokalizacja magazynu środowiska uruchomieniowego jest dodawana do zmiennej środowiskowej `DOTNET_SHARED_STORE`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-207">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="1ae1e-208">**Modyfikuj i umieść plik zależności uruchamiania hostingu**</span><span class="sxs-lookup"><span data-stu-id="1ae1e-208">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="1ae1e-209">Aby aktywować rozszerzanie bez odwołania do pakietu do rozszerzenia, określ dodatkowe zależności dla środowiska uruchomieniowego z `additionalDeps`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-209">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="1ae1e-210">`additionalDeps` umożliwia:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-210">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="1ae1e-211">Rozwiń Graf biblioteki aplikacji, dostarczając zestaw dodatkowych plików *. deps. JSON* do scalenia z własnym plikiem *. deps. JSON* podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-211">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="1ae1e-212">Należy hostingu zestawu startowego obciążana i prostsze do odnalezienia.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-212">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="1ae1e-213">Jest zalecane podejście do generowania pliku dodatkowe zależności:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-213">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="1ae1e-214">Wykonaj `dotnet publish` w pliku manifestu magazynu środowiska uruchomieniowego, do którego odwołuje się w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-214">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="1ae1e-215">Usuń odwołanie do manifestu z bibliotek i `runtime` w pliku *deps. JSON* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-215">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="1ae1e-216">W przykładowym projekcie Właściwość `store.manifest/1.0.0` jest usuwana z sekcji `targets` i `libraries`:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-216">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v3.0",
    "signature": ""
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v3.0": {
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
          "lib/netcoreapp3.0/StartupDiagnostics.dll": {
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
      "sha512": "sha512-xrhzuNSyM5/f4ZswhooJ9dmIYLP64wMnqUJSyTKVDKDVj5T+qtzypl8JmM/aFJLLpYrf0FYpVWvGujd7/FfMEw==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="1ae1e-217">Umieść plik *. deps. JSON* w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-217">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="1ae1e-218">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; lokalizacji dodanej do zmiennej środowiskowej `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-218">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="1ae1e-219">`{SHARED FRAMEWORK NAME}` &ndash; współdzielona struktura wymagana dla tego pliku zależności dodatkowych.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-219">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="1ae1e-220">`{SHARED FRAMEWORK VERSION}` &ndash; minimalnej udostępnionej wersji platformy.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-220">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="1ae1e-221">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; nazwę zestawu rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-221">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="1ae1e-222">W przykładowej aplikacji (projekt*RuntimeStore* ) plik dodatkowych zależności jest umieszczany w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-222">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/3.0.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="1ae1e-223">Aby środowisko uruchomieniowe wykrywało lokalizację magazynu środowiska uruchomieniowego, do zmiennej środowiskowej `DOTNET_ADDITIONAL_DEPS` jest dodawana lokalizacja pliku z dodatkowymi zależnościami.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-223">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="1ae1e-224">W przykładowej aplikacji (projekt*RuntimeStore* ) Kompilowanie magazynu środowiska uruchomieniowego i generowanie pliku dodatkowych zależności odbywa się przy użyciu skryptu [programu PowerShell](/powershell/scripting/powershell-scripting) .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-224">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="1ae1e-225">Przykłady sposobu ustawiania zmiennych środowiskowych dla różnych systemów operacyjnych znajdują się w temacie [Używanie wielu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-225">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="1ae1e-226">**Wdrożenie**</span><span class="sxs-lookup"><span data-stu-id="1ae1e-226">**Deployment**</span></span>

<span data-ttu-id="1ae1e-227">Aby ułatwić wdrożenie uruchamiania hostingu w środowisku wielokomputerowym, aplikacja Przykładowa tworzy folder *wdrożenia* w opublikowanych danych wyjściowych zawierający:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-227">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="1ae1e-228">Uruchamianie magazyn środowisko uruchomieniowe hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-228">The hosting startup runtime store.</span></span>
* <span data-ttu-id="1ae1e-229">Plik hostingu zależności startowe.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-229">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="1ae1e-230">Skrypt programu PowerShell, który tworzy lub modyfikuje `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`i `DOTNET_ADDITIONAL_DEPS` w celu obsługi aktywacji uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-230">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="1ae1e-231">Uruchom skrypt w administracyjnym wierszu polecenia programu PowerShell w systemie wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-231">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="1ae1e-232">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="1ae1e-232">NuGet package</span></span>

<span data-ttu-id="1ae1e-233">Można podać hostingu ulepszenie uruchamiania pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-233">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="1ae1e-234">Pakiet ma atrybut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-234">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="1ae1e-235">Hostingu typy uruchamiania, dostarczone przez pakiet są dostępne dla aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-235">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="1ae1e-236">Plik projektu aplikacji rozszerzone sprawia, że odwołania do pakietu do uruchomienia hostingu w pliku projektu aplikacji (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-236">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="1ae1e-237">Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji ( *. deps. JSON*).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-237">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="1ae1e-238">Takie podejście ma zastosowanie do pakietu zestawu startowego hostingu opublikowanego w [NuGet.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-238">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="1ae1e-239">Plik zależności uruchamiania hostingu jest udostępniany aplikacji rozszerzonej, zgodnie z opisem w sekcji [Magazyn środowiska uruchomieniowego](#runtime-store) (bez odwołania w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-239">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="1ae1e-240">Aby uzyskać więcej informacji na temat pakietów NuGet i magazynem środowiska uruchomieniowego zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-240">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="1ae1e-241">Jak utworzyć pakiet NuGet przy użyciu narzędzi międzyplatformowych</span><span class="sxs-lookup"><span data-stu-id="1ae1e-241">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="1ae1e-242">Publikowanie pakietów</span><span class="sxs-lookup"><span data-stu-id="1ae1e-242">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="1ae1e-243">Magazyn pakietu środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="1ae1e-243">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="1ae1e-244">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="1ae1e-244">Project bin folder</span></span>

<span data-ttu-id="1ae1e-245">Rozszerzanie uruchamiania hostingu może być dostarczone przez zestaw wdrożony przez *bin*w aplikacji rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-245">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="1ae1e-246">Typy uruchamiania hostingu udostępniane przez zestaw są udostępniane aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-246">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="1ae1e-247">Plik projektu aplikacji rozszerzone sprawia, że odwołanie do zestawu do uruchomienia hostingu (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-247">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="1ae1e-248">Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji ( *. deps. JSON*).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-248">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="1ae1e-249">Takie podejście ma zastosowanie, gdy scenariusz wdrażania wywoła odwołanie do zestawu uruchamiania hostingu (plik *. dll* ) i przenosząc zestaw do jednego z tych metod:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-249">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="1ae1e-250">Projekt zużywający.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-250">The consuming project.</span></span>
  * <span data-ttu-id="1ae1e-251">Lokalizacja dostępna przez projekt zużywający.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-251">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="1ae1e-252">Plik zależności uruchamiania hostingu jest udostępniany aplikacji rozszerzonej, zgodnie z opisem w sekcji [Magazyn środowiska uruchomieniowego](#runtime-store) (bez odwołania w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-252">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="1ae1e-253">Podczas określania .NET Framework, zestaw jest ładowany w domyślnym kontekście ładowania, który na .NET Framework oznacza, że zestaw znajduje się w jednej z następujących lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-253">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="1ae1e-254">Ścieżka bazowa aplikacji &ndash; folderze *bin* , w którym znajduje się plik wykonywalny ( *. exe*) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-254">Application base path &ndash; The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="1ae1e-255">Globalna pamięć podręczna zestawów (GAC) &ndash; magazyn GAC przechowuje zestawy, które są dostępne dla kilku aplikacji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-255">Global Assembly Cache (GAC) &ndash; The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="1ae1e-256">Aby uzyskać więcej informacji, zobacz [jak: Instalowanie zestawu w globalnej pamięci podręcznej zestawów](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) w dokumentacji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-256">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="1ae1e-257">Przykładowy kod</span><span class="sxs-lookup"><span data-stu-id="1ae1e-257">Sample code</span></span>

<span data-ttu-id="1ae1e-258">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([jak pobrać](xref:index#how-to-download-a-sample)) pokazuje hosting scenariuszy implementacji uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-258">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="1ae1e-259">Dwa hostingu zestawy startowe (bibliotek klas) Ustaw parę pary klucz wartość w pamięci konfiguracji poszczególnych:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-259">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="1ae1e-260">Pakiet NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="1ae1e-260">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="1ae1e-261">Biblioteka klas (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="1ae1e-261">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="1ae1e-262">Uruchamianie hostingu jest aktywowane z zestawu wdrożonego w sklepie uruchomieniowym (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-262">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="1ae1e-263">Zestaw dodaje dwa middlewares do aplikacji podczas uruchamiania, które zawierają informacje diagnostyczne dotyczące:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-263">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="1ae1e-264">Zarejestrowane usługi</span><span class="sxs-lookup"><span data-stu-id="1ae1e-264">Registered services</span></span>
  * <span data-ttu-id="1ae1e-265">Adres (schematu, hosta, podstawa ścieżki, ścieżki, ciąg zapytania)</span><span class="sxs-lookup"><span data-stu-id="1ae1e-265">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="1ae1e-266">Połączenia (zdalnego adresu IP, port zdalny lokalnego adresu IP, port lokalny, certyfikatu klienta)</span><span class="sxs-lookup"><span data-stu-id="1ae1e-266">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="1ae1e-267">Nagłówki żądań</span><span class="sxs-lookup"><span data-stu-id="1ae1e-267">Request headers</span></span>
  * <span data-ttu-id="1ae1e-268">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="1ae1e-268">Environment variables</span></span>

<span data-ttu-id="1ae1e-269">Do uruchomienia przykładu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-269">To run the sample:</span></span>

<span data-ttu-id="1ae1e-270">**Aktywacja z pakietu NuGet**</span><span class="sxs-lookup"><span data-stu-id="1ae1e-270">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="1ae1e-271">Skompiluj pakiet *HostingStartupPackage* przy użyciu polecenia [pakietu dotnet Pack](/dotnet/core/tools/dotnet-pack) .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-271">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="1ae1e-272">Dodaj nazwę zestawu pakietu *HostingStartupPackage* do zmiennej środowiskowej `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-272">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="1ae1e-273">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-273">Compile and run the app.</span></span> <span data-ttu-id="1ae1e-274">Odwołanie do pakietu jest obecny w aplikacji rozszerzone (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-274">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="1ae1e-275">`<PropertyGroup>` w pliku projektu aplikacji określa dane wyjściowe projektu pakietu ( *.. /HostingStartupPackage/bin/Debug*) jako źródło pakietu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-275">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="1ae1e-276">Dzięki temu aplikacja może korzystać z pakietu bez przekazywania pakietu do [NuGet.org](https://www.nuget.org/). Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-276">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="1ae1e-277">Zwróć uwagę, że wartości klucza konfiguracji usługi renderowane przez stronę indeksu są zgodne z wartościami ustawionymi przez metodę `ServiceKeyInjection.Configure` pakietu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-277">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="1ae1e-278">Jeśli wprowadzisz zmiany w projekcie *HostingStartupPackage* i skompilujesz go ponownie, wyczyść pamięć podręczną pakietów NuGet, aby upewnić się, że *HostingStartupApp* odbierze zaktualizowany pakiet, a nie stary pakiet z lokalnej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-278">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="1ae1e-279">Aby wyczyścić lokalne pamięci podręczne NuGet, należy wykonać następujące polecenie [locale NuGet programu dotnet](/dotnet/core/tools/dotnet-nuget-locals) :</span><span class="sxs-lookup"><span data-stu-id="1ae1e-279">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```dotnetcli
dotnet nuget locals all --clear
```

<span data-ttu-id="1ae1e-280">**Aktywacja z biblioteki klas**</span><span class="sxs-lookup"><span data-stu-id="1ae1e-280">**Activation from a class library**</span></span>

1. <span data-ttu-id="1ae1e-281">Skompiluj bibliotekę klas *HostingStartupLibrary* za pomocą polecenia [kompilacji dotnet](/dotnet/core/tools/dotnet-build) .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-281">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="1ae1e-282">Dodaj nazwę zestawu *HostingStartupLibrary* biblioteki klas do zmiennej środowiskowej `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-282">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="1ae1e-283">*bin*— Wdróż zestaw biblioteki klas w aplikacji przez skopiowanie pliku *HostingStartupLibrary. dll* z skompilowanych danych wyjściowych biblioteki klas do folderu *bin/debug* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-283">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="1ae1e-284">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-284">Compile and run the app.</span></span> <span data-ttu-id="1ae1e-285">`<ItemGroup>` w pliku projektu aplikacji odwołuje się do zestawu biblioteki klas ( *.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll*) (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-285">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="1ae1e-286">Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-286">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp3.0\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion> 
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="1ae1e-287">Zwróć uwagę, że wartości klucza konfiguracji usługi renderowane przez stronę indeksu są zgodne z wartościami ustawionymi przez metodę `ServiceKeyInjection.Configure` biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-287">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="1ae1e-288">**Aktywacja z zestawu wdrożonego w sklepie uruchomieniowym**</span><span class="sxs-lookup"><span data-stu-id="1ae1e-288">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="1ae1e-289">Projekt *StartupDiagnostics* używa [programu PowerShell](/powershell/scripting/powershell-scripting) do zmodyfikowania pliku *StartupDiagnostics. deps. JSON* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-289">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="1ae1e-290">Program PowerShell jest instalowany domyślnie w systemie Windows, począwszy od Windows 7 z dodatkiem SP1 i Windows Server 2008 R2 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-290">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="1ae1e-291">Aby uzyskać program PowerShell na innych platformach, zobacz [Instalowanie programu Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-291">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="1ae1e-292">Wykonaj skrypt *Build. ps1* w folderze *RuntimeStore* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-292">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="1ae1e-293">Skrypt:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-293">The script:</span></span>
   * <span data-ttu-id="1ae1e-294">Generuje pakiet `StartupDiagnostics` w folderze *obj\packages* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-294">Generates the `StartupDiagnostics` package in the *obj\packages* folder.</span></span>
   * <span data-ttu-id="1ae1e-295">Generuje magazyn środowiska uruchomieniowego dla `StartupDiagnostics` w folderze *magazynu* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-295">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="1ae1e-296">Polecenie `dotnet store` w skrypcie używa [identyfikatora środowiska uruchomieniowego `win7-x64` (RID)](/dotnet/core/rid-catalog) do uruchomienia hostingu wdrożonego w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-296">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="1ae1e-297">Podczas zapewniania uruchamiania hostingu dla innego środowiska uruchomieniowego należy zastąpić prawidłowy identyfikator RID w wierszu 37 skryptu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-297">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span> <span data-ttu-id="1ae1e-298">Magazyn środowiska uruchomieniowego dla `StartupDiagnostics` później zostanie przeniesiony do magazynu środowiska uruchomieniowego użytkownika lub systemu na komputerze, na którym zostanie użyty zestaw.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-298">The runtime store for `StartupDiagnostics` would later be moved to the user's or system's runtime store on the machine where the assembly will be consumed.</span></span> <span data-ttu-id="1ae1e-299">Lokalizacja instalacji magazynu środowiska uruchomieniowego użytkownika dla zestawu `StartupDiagnostics` to *. dotnet/Store/x64/netcoreapp 3.0/startupdiagnostics/1.0.0/lib/netcoreapp 3.0/startupdiagnostics. dll*.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-299">The user runtime store install location for the `StartupDiagnostics` assembly is *.dotnet/store/x64/netcoreapp3.0/startupdiagnostics/1.0.0/lib/netcoreapp3.0/StartupDiagnostics.dll*.</span></span>
   * <span data-ttu-id="1ae1e-300">Generuje `additionalDeps` dla `StartupDiagnostics` w folderze *additionalDeps* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-300">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps* folder.</span></span> <span data-ttu-id="1ae1e-301">Dodatkowe zależności byłyby później przenoszone do dodatkowych zależności użytkownika lub systemu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-301">The additional dependencies would later be moved to the user's or system's additional dependencies.</span></span> <span data-ttu-id="1ae1e-302">Użytkownik `StartupDiagnostics` dodatkowej lokalizacji instalacji jest *. dotnet/x64/additionalDeps/StartupDiagnostics/Shared/Microsoft. servicecore. app/3.0.0/StartupDiagnostics. deps. JSON*.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-302">The user `StartupDiagnostics` additional dependencies install location is *.dotnet/x64/additionalDeps/StartupDiagnostics/shared/Microsoft.NETCore.App/3.0.0/StartupDiagnostics.deps.json*.</span></span>
   * <span data-ttu-id="1ae1e-303">Umieszcza plik *Deploy. ps1* w folderze *Deployment* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-303">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="1ae1e-304">Uruchom skrypt *Deploy. ps1* w folderze *Deployment* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-304">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="1ae1e-305">Dołączenie skryptu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-305">The script appends:</span></span>
   * <span data-ttu-id="1ae1e-306">`StartupDiagnostics` ze zmienną środowiskową `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-306">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="1ae1e-307">Ścieżka zależności uruchamiania hostingu (w folderze *wdrażania* projektu RuntimeStore) do zmiennej środowiskowej `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-307">The hosting startup dependencies path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="1ae1e-308">Ścieżka magazynu środowiska uruchomieniowego (w folderze *wdrażania* projektu RuntimeStore) do zmiennej środowiskowej `DOTNET_SHARED_STORE`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-308">The runtime store path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="1ae1e-309">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-309">Run the sample app.</span></span>
1. <span data-ttu-id="1ae1e-310">Zażądaj punktu końcowego `/services`, aby zobaczyć zarejestrowane usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-310">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="1ae1e-311">Zażądaj punktu końcowego `/diag`, aby wyświetlić informacje diagnostyczne.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-311">Request the `/diag` endpoint to see the diagnostic information.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1ae1e-312">Implementacja <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (uruchamianie hostingu) dodaje usprawnienia do aplikacji podczas uruchamiania z zestawu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-312">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="1ae1e-313">Na przykład zewnętrznej biblioteki służy hostingu implementacji uruchamiania zapewnienie dodatkowej konfiguracji dostawcy lub usług do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-313">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="1ae1e-314">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1ae1e-314">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="1ae1e-315">Atrybut HostingStartup</span><span class="sxs-lookup"><span data-stu-id="1ae1e-315">HostingStartup attribute</span></span>

<span data-ttu-id="1ae1e-316">Atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) wskazuje obecność zestawu startowego hostingu do aktywowania w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-316">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="1ae1e-317">Zestaw wpisów lub zestaw zawierający klasę `Startup` jest automatycznie skanowany dla atrybutu `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-317">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="1ae1e-318">Lista zestawów do wyszukiwania atrybutów `HostingStartup` jest ładowana w czasie wykonywania z konfiguracji w [WebHostDefaults. HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-318">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span></span> <span data-ttu-id="1ae1e-319">Lista zestawów do wykluczenia z odnajdywania jest ładowana z [WebHostDefaults. HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-319">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span></span> <span data-ttu-id="1ae1e-320">Aby uzyskać więcej informacji, zobacz [host sieci Web: hosting zestawów startowych](xref:fundamentals/host/web-host#hosting-startup-assemblies) i [hosta sieci Web: hosting z wykluczeniem uruchomienia](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-320">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="1ae1e-321">W poniższym przykładzie przestrzeń nazw zestawu startowego hostingu jest `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-321">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="1ae1e-322">Klasa zawierająca kod uruchamiania hostingu jest `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-322">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="1ae1e-323">Atrybut `HostingStartup` zwykle znajduje się w pliku klasy implementacji `IHostingStartup`, w którym znajduje się zestaw startowy.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-323">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="1ae1e-324">Odkryj załadowanych zestawów uruchomienia hostingu</span><span class="sxs-lookup"><span data-stu-id="1ae1e-324">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="1ae1e-325">Aby odnaleźć załadowanych zestawów uruchomienia hostingu, Włącz rejestrowanie i zapoznać się z dziennikami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-325">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="1ae1e-326">Błędy występujące podczas ładowania zestawów są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-326">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="1ae1e-327">Załadowano hostingu zestawy startowe są rejestrowane na poziomie debugowania, a wszystkie błędy są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-327">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="1ae1e-328">Wyłącz automatyczne ładowanie hostingu zestawy startowe</span><span class="sxs-lookup"><span data-stu-id="1ae1e-328">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="1ae1e-329">Aby wyłączyć automatyczne ładowanie hostingu zestawy startowe, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-329">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="1ae1e-330">Aby zapobiec ładowaniu wszystkich początkowych zestawów uruchamiania, należy ustawić jedną z następujących wartości `true` lub `1`:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-330">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="1ae1e-331">[Zapobiegaj](xref:fundamentals/host/web-host#prevent-hosting-startup) ustawieniu konfiguracji hosta uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-331">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="1ae1e-332">Zmienna środowiskowa `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-332">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="1ae1e-333">Aby zapobiec określonych zestawów uruchomienia hostingu, ładowanie, ustawić jedną z następujących hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania ciągu rozdzielone średnikami:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-333">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="1ae1e-334">[Uruchamianie hostingu Wyklucz](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) ustawienia konfiguracja hosta.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-334">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="1ae1e-335">Zmienna środowiskowa `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-335">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="1ae1e-336">Jeśli ustawienia konfiguracji hosta i zmienną środowiskową są skonfigurowane, ustawienie hosta steruje zachowaniem.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-336">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="1ae1e-337">Wyłączenie hostingu zestawy startowe przy użyciu zmiennej ustawienie lub środowisko hosta globalnie wyłącza zestawu i może wyłączyć pewne cechy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-337">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="1ae1e-338">Project</span><span class="sxs-lookup"><span data-stu-id="1ae1e-338">Project</span></span>

<span data-ttu-id="1ae1e-339">Tworzenie uruchomienia hostingu za pomocą jednej z poniższych typów projektów:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-339">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="1ae1e-340">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="1ae1e-340">Class library</span></span>](#class-library)
* [<span data-ttu-id="1ae1e-341">Aplikacja konsolowa bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="1ae1e-341">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="1ae1e-342">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="1ae1e-342">Class library</span></span>

<span data-ttu-id="1ae1e-343">Można podać hostingu ulepszenie uruchamiania w bibliotece klas.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-343">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="1ae1e-344">Biblioteka zawiera atrybut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-344">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="1ae1e-345">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera Razor Pages aplikacji, *HostingStartupApp*i biblioteki klas *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-345">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="1ae1e-346">Biblioteka klas:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-346">The class library:</span></span>

* <span data-ttu-id="1ae1e-347">Zawiera klasę uruchamiania hostingu, `ServiceKeyInjection`, która implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-347">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="1ae1e-348">`ServiceKeyInjection` dodaje parę ciągów usługi do konfiguracji aplikacji za pomocą dostawcy konfiguracji w pamięci ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-348">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span></span>
* <span data-ttu-id="1ae1e-349">Zawiera atrybut `HostingStartup`, który identyfikuje przestrzeń nazw i klasę uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-349">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="1ae1e-350">Metoda <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> klasy `ServiceKeyInjection` używa <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> do dodawania ulepszeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-350">The `ServiceKeyInjection` class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span>

<span data-ttu-id="1ae1e-351">*HostingStartupLibrary/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-351">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="1ae1e-352">Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez bibliotekę klas hostingu uruchamiania zestawu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-352">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="1ae1e-353">*HostingStartupApp/Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-353">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="1ae1e-354">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera również projekt pakietu NuGet, który zapewnia oddzielne uruchamianie hostingu, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-354">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="1ae1e-355">Pakiet ma takie same charakterystyki biblioteki klas wcześniejszym opisem.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-355">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="1ae1e-356">Pakiet:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-356">The package:</span></span>

* <span data-ttu-id="1ae1e-357">Zawiera klasę uruchamiania hostingu, `ServiceKeyInjection`, która implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-357">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="1ae1e-358">`ServiceKeyInjection` dodaje parę ciągów usługi do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-358">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="1ae1e-359">Zawiera atrybut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-359">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="1ae1e-360">*HostingStartupPackage/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-360">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="1ae1e-361">Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez pakiet hostingu uruchamiania zestawu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-361">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="1ae1e-362">*HostingStartupApp/Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-362">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="1ae1e-363">Aplikacja konsoli bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="1ae1e-363">Console app without an entry point</span></span>

<span data-ttu-id="1ae1e-364">*Ta metoda jest dostępna tylko dla aplikacji platformy .NET Core, a nie .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="1ae1e-364">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="1ae1e-365">Rozszerzenie dynamicznego uruchamiania hostingu, które nie wymaga odwołania do czasu kompilowania dla aktywacji, można dostarczyć w aplikacji konsolowej bez punktu wejścia, który zawiera atrybut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-365">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="1ae1e-366">Publikowanie aplikacji konsoli daje hostingu zestaw startowy, który mogą być używane w sklepie środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-366">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="1ae1e-367">Aplikacja konsolowa bez punktu wejścia jest używany w ramach tego procesu, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-367">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="1ae1e-368">Do korzystania z hostingu uruchamiania w zestawie hostingu uruchamiania jest wymagana pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-368">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="1ae1e-369">Plik zależności jest trwały możliwy do uruchomienia aplikacji, który jest wytwarzany przez opublikowanie aplikacji, a nie z biblioteki.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-369">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="1ae1e-370">Biblioteki nie można dodać bezpośrednio do [magazynu pakietów środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store), który wymaga projektu możliwy do uruchomienia, który jest przeznaczony dla udostępnionego środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-370">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="1ae1e-371">W przypadku tworzenia dynamicznego uruchamiania hostingu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-371">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="1ae1e-372">Zestaw startowy hostingu jest tworzony z aplikacji konsoli, bez punkt wejścia:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-372">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="1ae1e-373">Zawiera klasę, która zawiera implementację `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-373">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="1ae1e-374">Zawiera atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) do identyfikacji klasy implementacji `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-374">Includes a [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="1ae1e-375">Aplikacja konsoli jest publikowany uzyskać zależności uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-375">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="1ae1e-376">Konsekwencją publikowania aplikacji konsoli jest, że nieużywane zależności są usuwane z pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-376">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="1ae1e-377">Plik zależności jest modyfikowany do ustawiania lokalizacji środowiska uruchomieniowego, zestawu startowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-377">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="1ae1e-378">Zestawie hostingu uruchamiania i jego zależności pliku jest umieszczany w Magazyn pakietu środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-378">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="1ae1e-379">Aby odnaleźć zestawie hostingu uruchamiania i jego zależności pliku, są wyświetlane w pary zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-379">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="1ae1e-380">Aplikacja konsolowa odwołuje się do pakietu [Microsoft. AspNetCore. hosting. Abstracts](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) :</span><span class="sxs-lookup"><span data-stu-id="1ae1e-380">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="1ae1e-381">Atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) identyfikuje klasę jako implementację `IHostingStartup` do załadowania i wykonania podczas kompilowania <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-381">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span></span> <span data-ttu-id="1ae1e-382">W poniższym przykładzie przestrzeń nazw jest `StartupEnhancement`, a Klasa jest `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-382">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="1ae1e-383">Klasa implementuje `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-383">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="1ae1e-384">Metoda <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> klasy używa <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> do dodawania ulepszeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-384">The class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span> <span data-ttu-id="1ae1e-385">`IHostingStartup.Configure` w zestawie uruchamiania hostingu jest wywoływany przez środowisko uruchomieniowe przed `Startup.Configure` w kodzie użytkownika, dzięki czemu kod użytkownika może zastępować każdą konfigurację podaną przez zestaw startowy hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-385">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="1ae1e-386">Podczas kompilowania projektu `IHostingStartup` plik zależności ( *. deps. JSON*) ustawia lokalizację `runtime` zestawu do folderu *bin* :</span><span class="sxs-lookup"><span data-stu-id="1ae1e-386">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="1ae1e-387">Jest wyświetlana tylko część pliku.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-387">Only part of the file is shown.</span></span> <span data-ttu-id="1ae1e-388">Nazwa zestawu w przykładzie jest `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-388">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="1ae1e-389">Konfiguracja dostarczona przez uruchomienie hostingu</span><span class="sxs-lookup"><span data-stu-id="1ae1e-389">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="1ae1e-390">Istnieją dwa podejścia do obsługi konfiguracji w zależności od tego, czy konfiguracja uruchamiania hostingu ma mieć pierwszeństwo, czy konfiguracja aplikacji ma mieć pierwszeństwo:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-390">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="1ae1e-391">Podaj konfigurację aplikacji przy użyciu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*>, aby załadować konfigurację po wykonaniu delegatów <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-391">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="1ae1e-392">Hostowanie konfiguracji uruchamiania ma wyższy priorytet niż konfiguracja aplikacji przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-392">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="1ae1e-393">Podaj konfigurację aplikacji przy użyciu <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> do załadowania konfiguracji przed wykonaniem delegatów <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-393">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="1ae1e-394">Wartości konfiguracyjne aplikacji mają pierwszeństwo przed tymi, które są udostępniane przez uruchamianie hostingu przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-394">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

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
                    "From IHostingStartup: Higher priority " +
                    "than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority " +
                "than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="1ae1e-395">Określ hostingu zestawu startowego</span><span class="sxs-lookup"><span data-stu-id="1ae1e-395">Specify the hosting startup assembly</span></span>

<span data-ttu-id="1ae1e-396">W przypadku biblioteki klas lub uruchamiania hostingu obsługiwanej przez aplikację konsoli Określ nazwę zestawu startowego hostingu w zmiennej środowiskowej `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-396">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="1ae1e-397">Zmienna środowiskowa jest rozdzieloną średnikami listę zestawów.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-397">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="1ae1e-398">Tylko hosting zestawów startowych jest skanowany pod kątem atrybutu `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-398">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="1ae1e-399">W przypadku przykładowej aplikacji, *HostingStartupApp*, aby odnaleźć opisane wcześniej uruchomienia hostingu, zmienna środowiskowa jest ustawiona na następującą wartość:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-399">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="1ae1e-400">Zestaw startowy obsługujący hosting można również ustawić przy użyciu ustawienia konfiguracji hosta [zestawów startowych](xref:fundamentals/host/web-host#hosting-startup-assemblies) .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-400">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="1ae1e-401">Gdy są obecne wiele asemblerów uruchamiania hostingu, ich metody <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> są wykonywane w kolejności, w jakiej są wymienione zestawy.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-401">When multiple hosting startup assembles are present, their <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="1ae1e-402">Aktywacja</span><span class="sxs-lookup"><span data-stu-id="1ae1e-402">Activation</span></span>

<span data-ttu-id="1ae1e-403">Dostępne są następujące opcje do obsługi aktywacji uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-403">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="1ae1e-404">Aktywacja [magazynu w środowisku uruchomieniowym](#runtime-store) &ndash; nie wymaga odwołania do czasu kompilacji na potrzeby aktywacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-404">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="1ae1e-405">Przykładowa aplikacja umieszcza zestaw startowy obsługujący i pliki zależności w folderze, *wdrożeniu*, aby ułatwić wdrożenie uruchamiania hostingu w środowisku wielomaszynowym.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-405">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="1ae1e-406">Folder *wdrożenia* zawiera także skrypt programu PowerShell, który tworzy lub modyfikuje zmienne środowiskowe w systemie wdrażania, aby umożliwić uruchamianie hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-406">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="1ae1e-407">Dokumentacja kompilacji wymagany do aktywacji</span><span class="sxs-lookup"><span data-stu-id="1ae1e-407">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="1ae1e-408">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="1ae1e-408">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="1ae1e-409">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="1ae1e-409">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="1ae1e-410">Środowisko uruchomieniowe magazynu</span><span class="sxs-lookup"><span data-stu-id="1ae1e-410">Runtime store</span></span>

<span data-ttu-id="1ae1e-411">Implementacja uruchamiania hostingu jest umieszczana w [magazynie w czasie wykonywania](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-411">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="1ae1e-412">Odwołanie do zestawu kompilacji nie jest wymagane przez aplikację rozszerzone.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-412">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="1ae1e-413">Po skompilowaniu uruchomienia hostingu magazyn środowiska uruchomieniowego jest generowany przy użyciu pliku projektu manifestu i polecenia [magazynu dotnet](/dotnet/core/tools/dotnet-store) .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-413">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="1ae1e-414">W przykładowej aplikacji (projekt*RuntimeStore* ) używane jest następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-414">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="1ae1e-415">Aby środowisko uruchomieniowe wykrywało magazyn środowiska uruchomieniowego, lokalizacja magazynu środowiska uruchomieniowego jest dodawana do zmiennej środowiskowej `DOTNET_SHARED_STORE`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-415">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="1ae1e-416">**Modyfikuj i umieść plik zależności uruchamiania hostingu**</span><span class="sxs-lookup"><span data-stu-id="1ae1e-416">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="1ae1e-417">Aby aktywować rozszerzanie bez odwołania do pakietu do rozszerzenia, określ dodatkowe zależności dla środowiska uruchomieniowego z `additionalDeps`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-417">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="1ae1e-418">`additionalDeps` umożliwia:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-418">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="1ae1e-419">Rozwiń Graf biblioteki aplikacji, dostarczając zestaw dodatkowych plików *. deps. JSON* do scalenia z własnym plikiem *. deps. JSON* podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-419">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="1ae1e-420">Należy hostingu zestawu startowego obciążana i prostsze do odnalezienia.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-420">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="1ae1e-421">Jest zalecane podejście do generowania pliku dodatkowe zależności:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-421">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="1ae1e-422">Wykonaj `dotnet publish` w pliku manifestu magazynu środowiska uruchomieniowego, do którego odwołuje się w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-422">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="1ae1e-423">Usuń odwołanie do manifestu z bibliotek i `runtime` w pliku *deps. JSON* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-423">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="1ae1e-424">W przykładowym projekcie Właściwość `store.manifest/1.0.0` jest usuwana z sekcji `targets` i `libraries`:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-424">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

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

<span data-ttu-id="1ae1e-425">Umieść plik *. deps. JSON* w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-425">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="1ae1e-426">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; lokalizacji dodanej do zmiennej środowiskowej `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-426">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="1ae1e-427">`{SHARED FRAMEWORK NAME}` &ndash; współdzielona struktura wymagana dla tego pliku zależności dodatkowych.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-427">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="1ae1e-428">`{SHARED FRAMEWORK VERSION}` &ndash; minimalnej udostępnionej wersji platformy.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-428">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="1ae1e-429">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; nazwę zestawu rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-429">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="1ae1e-430">W przykładowej aplikacji (projekt*RuntimeStore* ) plik dodatkowych zależności jest umieszczany w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-430">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="1ae1e-431">Aby środowisko uruchomieniowe wykrywało lokalizację magazynu środowiska uruchomieniowego, do zmiennej środowiskowej `DOTNET_ADDITIONAL_DEPS` jest dodawana lokalizacja pliku z dodatkowymi zależnościami.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-431">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="1ae1e-432">W przykładowej aplikacji (projekt*RuntimeStore* ) Kompilowanie magazynu środowiska uruchomieniowego i generowanie pliku dodatkowych zależności odbywa się przy użyciu skryptu [programu PowerShell](/powershell/scripting/powershell-scripting) .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-432">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="1ae1e-433">Przykłady sposobu ustawiania zmiennych środowiskowych dla różnych systemów operacyjnych znajdują się w temacie [Używanie wielu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-433">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="1ae1e-434">**Wdrożenie**</span><span class="sxs-lookup"><span data-stu-id="1ae1e-434">**Deployment**</span></span>

<span data-ttu-id="1ae1e-435">Aby ułatwić wdrożenie uruchamiania hostingu w środowisku wielokomputerowym, aplikacja Przykładowa tworzy folder *wdrożenia* w opublikowanych danych wyjściowych zawierający:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-435">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="1ae1e-436">Uruchamianie magazyn środowisko uruchomieniowe hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-436">The hosting startup runtime store.</span></span>
* <span data-ttu-id="1ae1e-437">Plik hostingu zależności startowe.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-437">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="1ae1e-438">Skrypt programu PowerShell, który tworzy lub modyfikuje `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`i `DOTNET_ADDITIONAL_DEPS` w celu obsługi aktywacji uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-438">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="1ae1e-439">Uruchom skrypt w administracyjnym wierszu polecenia programu PowerShell w systemie wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-439">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="1ae1e-440">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="1ae1e-440">NuGet package</span></span>

<span data-ttu-id="1ae1e-441">Można podać hostingu ulepszenie uruchamiania pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-441">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="1ae1e-442">Pakiet ma atrybut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-442">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="1ae1e-443">Hostingu typy uruchamiania, dostarczone przez pakiet są dostępne dla aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-443">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="1ae1e-444">Plik projektu aplikacji rozszerzone sprawia, że odwołania do pakietu do uruchomienia hostingu w pliku projektu aplikacji (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-444">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="1ae1e-445">Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji ( *. deps. JSON*).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-445">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="1ae1e-446">Takie podejście ma zastosowanie do pakietu zestawu startowego hostingu opublikowanego w [NuGet.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-446">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="1ae1e-447">Plik zależności uruchamiania hostingu jest udostępniany aplikacji rozszerzonej, zgodnie z opisem w sekcji [Magazyn środowiska uruchomieniowego](#runtime-store) (bez odwołania w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-447">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="1ae1e-448">Aby uzyskać więcej informacji na temat pakietów NuGet i magazynem środowiska uruchomieniowego zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-448">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="1ae1e-449">Jak utworzyć pakiet NuGet przy użyciu narzędzi międzyplatformowych</span><span class="sxs-lookup"><span data-stu-id="1ae1e-449">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="1ae1e-450">Publikowanie pakietów</span><span class="sxs-lookup"><span data-stu-id="1ae1e-450">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="1ae1e-451">Magazyn pakietu środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="1ae1e-451">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="1ae1e-452">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="1ae1e-452">Project bin folder</span></span>

<span data-ttu-id="1ae1e-453">Rozszerzanie uruchamiania hostingu może być dostarczone przez zestaw wdrożony przez *bin*w aplikacji rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-453">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="1ae1e-454">Typy uruchamiania hostingu udostępniane przez zestaw są udostępniane aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-454">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="1ae1e-455">Plik projektu aplikacji rozszerzone sprawia, że odwołanie do zestawu do uruchomienia hostingu (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-455">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="1ae1e-456">Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji ( *. deps. JSON*).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-456">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="1ae1e-457">Takie podejście ma zastosowanie, gdy scenariusz wdrażania wywoła odwołanie do zestawu uruchamiania hostingu (plik *. dll* ) i przenosząc zestaw do jednego z tych metod:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-457">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="1ae1e-458">Projekt zużywający.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-458">The consuming project.</span></span>
  * <span data-ttu-id="1ae1e-459">Lokalizacja dostępna przez projekt zużywający.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-459">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="1ae1e-460">Plik zależności uruchamiania hostingu jest udostępniany aplikacji rozszerzonej, zgodnie z opisem w sekcji [Magazyn środowiska uruchomieniowego](#runtime-store) (bez odwołania w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-460">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="1ae1e-461">Podczas określania .NET Framework, zestaw jest ładowany w domyślnym kontekście ładowania, który na .NET Framework oznacza, że zestaw znajduje się w jednej z następujących lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-461">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="1ae1e-462">Ścieżka bazowa aplikacji &ndash; folderze *bin* , w którym znajduje się plik wykonywalny ( *. exe*) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-462">Application base path &ndash; The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="1ae1e-463">Globalna pamięć podręczna zestawów (GAC) &ndash; magazyn GAC przechowuje zestawy, które są dostępne dla kilku aplikacji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-463">Global Assembly Cache (GAC) &ndash; The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="1ae1e-464">Aby uzyskać więcej informacji, zobacz [jak: Instalowanie zestawu w globalnej pamięci podręcznej zestawów](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) w dokumentacji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-464">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="1ae1e-465">Przykładowy kod</span><span class="sxs-lookup"><span data-stu-id="1ae1e-465">Sample code</span></span>

<span data-ttu-id="1ae1e-466">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([jak pobrać](xref:index#how-to-download-a-sample)) pokazuje hosting scenariuszy implementacji uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-466">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="1ae1e-467">Dwa hostingu zestawy startowe (bibliotek klas) Ustaw parę pary klucz wartość w pamięci konfiguracji poszczególnych:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-467">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="1ae1e-468">Pakiet NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="1ae1e-468">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="1ae1e-469">Biblioteka klas (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="1ae1e-469">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="1ae1e-470">Uruchamianie hostingu jest aktywowane z zestawu wdrożonego w sklepie uruchomieniowym (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-470">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="1ae1e-471">Zestaw dodaje dwa middlewares do aplikacji podczas uruchamiania, które zawierają informacje diagnostyczne dotyczące:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-471">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="1ae1e-472">Zarejestrowane usługi</span><span class="sxs-lookup"><span data-stu-id="1ae1e-472">Registered services</span></span>
  * <span data-ttu-id="1ae1e-473">Adres (schematu, hosta, podstawa ścieżki, ścieżki, ciąg zapytania)</span><span class="sxs-lookup"><span data-stu-id="1ae1e-473">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="1ae1e-474">Połączenia (zdalnego adresu IP, port zdalny lokalnego adresu IP, port lokalny, certyfikatu klienta)</span><span class="sxs-lookup"><span data-stu-id="1ae1e-474">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="1ae1e-475">Nagłówki żądań</span><span class="sxs-lookup"><span data-stu-id="1ae1e-475">Request headers</span></span>
  * <span data-ttu-id="1ae1e-476">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="1ae1e-476">Environment variables</span></span>

<span data-ttu-id="1ae1e-477">Do uruchomienia przykładu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-477">To run the sample:</span></span>

<span data-ttu-id="1ae1e-478">**Aktywacja z pakietu NuGet**</span><span class="sxs-lookup"><span data-stu-id="1ae1e-478">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="1ae1e-479">Skompiluj pakiet *HostingStartupPackage* przy użyciu polecenia [pakietu dotnet Pack](/dotnet/core/tools/dotnet-pack) .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-479">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="1ae1e-480">Dodaj nazwę zestawu pakietu *HostingStartupPackage* do zmiennej środowiskowej `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-480">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="1ae1e-481">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-481">Compile and run the app.</span></span> <span data-ttu-id="1ae1e-482">Odwołanie do pakietu jest obecny w aplikacji rozszerzone (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-482">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="1ae1e-483">`<PropertyGroup>` w pliku projektu aplikacji określa dane wyjściowe projektu pakietu ( *.. /HostingStartupPackage/bin/Debug*) jako źródło pakietu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-483">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="1ae1e-484">Dzięki temu aplikacja może korzystać z pakietu bez przekazywania pakietu do [NuGet.org](https://www.nuget.org/). Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-484">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="1ae1e-485">Zwróć uwagę, że wartości klucza konfiguracji usługi renderowane przez stronę indeksu są zgodne z wartościami ustawionymi przez metodę `ServiceKeyInjection.Configure` pakietu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-485">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="1ae1e-486">Jeśli wprowadzisz zmiany w projekcie *HostingStartupPackage* i skompilujesz go ponownie, wyczyść pamięć podręczną pakietów NuGet, aby upewnić się, że *HostingStartupApp* odbierze zaktualizowany pakiet, a nie stary pakiet z lokalnej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-486">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="1ae1e-487">Aby wyczyścić lokalne pamięci podręczne NuGet, należy wykonać następujące polecenie [locale NuGet programu dotnet](/dotnet/core/tools/dotnet-nuget-locals) :</span><span class="sxs-lookup"><span data-stu-id="1ae1e-487">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```dotnetcli
dotnet nuget locals all --clear
```

<span data-ttu-id="1ae1e-488">**Aktywacja z biblioteki klas**</span><span class="sxs-lookup"><span data-stu-id="1ae1e-488">**Activation from a class library**</span></span>

1. <span data-ttu-id="1ae1e-489">Skompiluj bibliotekę klas *HostingStartupLibrary* za pomocą polecenia [kompilacji dotnet](/dotnet/core/tools/dotnet-build) .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-489">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="1ae1e-490">Dodaj nazwę zestawu *HostingStartupLibrary* biblioteki klas do zmiennej środowiskowej `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-490">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="1ae1e-491">*bin*— Wdróż zestaw biblioteki klas w aplikacji przez skopiowanie pliku *HostingStartupLibrary. dll* z skompilowanych danych wyjściowych biblioteki klas do folderu *bin/debug* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-491">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="1ae1e-492">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-492">Compile and run the app.</span></span> <span data-ttu-id="1ae1e-493">`<ItemGroup>` w pliku projektu aplikacji odwołuje się do zestawu biblioteki klas ( *.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-493">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="1ae1e-494">Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-494">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp2.1\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="1ae1e-495">Zwróć uwagę, że wartości klucza konfiguracji usługi renderowane przez stronę indeksu są zgodne z wartościami ustawionymi przez metodę `ServiceKeyInjection.Configure` biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-495">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="1ae1e-496">**Aktywacja z zestawu wdrożonego w sklepie uruchomieniowym**</span><span class="sxs-lookup"><span data-stu-id="1ae1e-496">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="1ae1e-497">Projekt *StartupDiagnostics* używa [programu PowerShell](/powershell/scripting/powershell-scripting) do zmodyfikowania pliku *StartupDiagnostics. deps. JSON* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-497">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="1ae1e-498">Program PowerShell jest instalowany domyślnie w systemie Windows, począwszy od Windows 7 z dodatkiem SP1 i Windows Server 2008 R2 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-498">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="1ae1e-499">Aby uzyskać program PowerShell na innych platformach, zobacz [Instalowanie programu Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="1ae1e-499">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="1ae1e-500">Wykonaj skrypt *Build. ps1* w folderze *RuntimeStore* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-500">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="1ae1e-501">Skrypt:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-501">The script:</span></span>
   * <span data-ttu-id="1ae1e-502">Generuje pakiet `StartupDiagnostics` w folderze *obj\packages* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-502">Generates the `StartupDiagnostics` package in the *obj\packages* folder.</span></span>
   * <span data-ttu-id="1ae1e-503">Generuje magazyn środowiska uruchomieniowego dla `StartupDiagnostics` w folderze *magazynu* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-503">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="1ae1e-504">Polecenie `dotnet store` w skrypcie używa [identyfikatora środowiska uruchomieniowego `win7-x64` (RID)](/dotnet/core/rid-catalog) do uruchomienia hostingu wdrożonego w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-504">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="1ae1e-505">Podczas zapewniania uruchamiania hostingu dla innego środowiska uruchomieniowego należy zastąpić prawidłowy identyfikator RID w wierszu 37 skryptu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-505">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span> <span data-ttu-id="1ae1e-506">Magazyn środowiska uruchomieniowego dla `StartupDiagnostics` później zostanie przeniesiony do magazynu środowiska uruchomieniowego użytkownika lub systemu na komputerze, na którym zostanie użyty zestaw.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-506">The runtime store for `StartupDiagnostics` would later be moved to the user's or system's runtime store on the machine where the assembly will be consumed.</span></span> <span data-ttu-id="1ae1e-507">Lokalizacja instalacji magazynu środowiska uruchomieniowego użytkownika dla zestawu `StartupDiagnostics` to *. dotnet/Store/x64/netcoreapp 2.2/startupdiagnostics/1.0.0/lib/netcoreapp 2.2/startupdiagnostics. dll*.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-507">The user runtime store install location for the `StartupDiagnostics` assembly is *.dotnet/store/x64/netcoreapp2.2/startupdiagnostics/1.0.0/lib/netcoreapp2.2/StartupDiagnostics.dll*.</span></span>
   * <span data-ttu-id="1ae1e-508">Generuje `additionalDeps` dla `StartupDiagnostics` w folderze *additionalDeps* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-508">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps* folder.</span></span> <span data-ttu-id="1ae1e-509">Dodatkowe zależności byłyby później przenoszone do dodatkowych zależności użytkownika lub systemu.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-509">The additional dependencies would later be moved to the user's or system's additional dependencies.</span></span> <span data-ttu-id="1ae1e-510">Użytkownik `StartupDiagnostics` dodatkowej lokalizacji instalacji jest *. dotnet/x64/additionalDeps/StartupDiagnostics/Shared/Microsoft. servicecore. App/2.2.0/StartupDiagnostics. deps. JSON*.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-510">The user `StartupDiagnostics` additional dependencies install location is *.dotnet/x64/additionalDeps/StartupDiagnostics/shared/Microsoft.NETCore.App/2.2.0/StartupDiagnostics.deps.json*.</span></span>
   * <span data-ttu-id="1ae1e-511">Umieszcza plik *Deploy. ps1* w folderze *Deployment* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-511">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="1ae1e-512">Uruchom skrypt *Deploy. ps1* w folderze *Deployment* .</span><span class="sxs-lookup"><span data-stu-id="1ae1e-512">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="1ae1e-513">Dołączenie skryptu:</span><span class="sxs-lookup"><span data-stu-id="1ae1e-513">The script appends:</span></span>
   * <span data-ttu-id="1ae1e-514">`StartupDiagnostics` ze zmienną środowiskową `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-514">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="1ae1e-515">Ścieżka zależności uruchamiania hostingu (w folderze *wdrażania* projektu RuntimeStore) do zmiennej środowiskowej `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-515">The hosting startup dependencies path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="1ae1e-516">Ścieżka magazynu środowiska uruchomieniowego (w folderze *wdrażania* projektu RuntimeStore) do zmiennej środowiskowej `DOTNET_SHARED_STORE`.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-516">The runtime store path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="1ae1e-517">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-517">Run the sample app.</span></span>
1. <span data-ttu-id="1ae1e-518">Zażądaj punktu końcowego `/services`, aby zobaczyć zarejestrowane usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-518">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="1ae1e-519">Zażądaj punktu końcowego `/diag`, aby wyświetlić informacje diagnostyczne.</span><span class="sxs-lookup"><span data-stu-id="1ae1e-519">Request the `/diag` endpoint to see the diagnostic information.</span></span>

::: moniker-end
