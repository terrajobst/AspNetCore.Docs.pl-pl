---
title: Wzorzec opcje w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak użyć wzorca opcje do reprezentowania grup powiązanych ustawień w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: c996ac6ab05b98bcca72d0993fe412f553b58106
ms.sourcegitcommit: 19cbda409bdbbe42553dc385ea72d2a8e246509c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992967"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="b7e17-103">Wzorzec opcje w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b7e17-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="b7e17-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b7e17-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b7e17-105">Wzorzec opcje używa klas do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="b7e17-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b7e17-106">Jeśli ustawienia konfiguracji są izolowane przez funkcję w osobnych klas, aplikacja działa zgodnie z dwóch zasad ważne inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="b7e17-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="b7e17-107">[Zasady podziału interfejsu (ISP)](http://deviq.com/interface-segregation-principle/): funkcji (grupy), które są zależne od ustawień konfiguracji tylko zależą od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="b7e17-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="b7e17-108">[Separacji](http://deviq.com/separation-of-concerns/): ustawienia dla poszczególnych części aplikacji nie są zależnych lub powiązanych ze sobą.</span><span class="sxs-lookup"><span data-stu-id="b7e17-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="b7e17-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) ten artykuł stanowi łatwiejszej do obserwowania z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b7e17-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="b7e17-110">Podstawowe opcje konfiguracji</span><span class="sxs-lookup"><span data-stu-id="b7e17-110">Basic options configuration</span></span>

<span data-ttu-id="b7e17-111">Opcje podstawowe konfiguracja jest przedstawiona jako przykład &num;1 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b7e17-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b7e17-112">Klasa opcji musi być nieabstrakcyjnej przy użyciu publicznego konstruktora bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="b7e17-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="b7e17-113">Następujące klasy `MyOptions`, ma dwie właściwości `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="b7e17-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="b7e17-114">Ustawianie wartości domyślnych jest opcjonalne, ale konstruktora klasy w poniższym przykładzie ustawiono wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b7e17-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="b7e17-115">`Option2` ma wartość domyślną, ustawiane przez bezpośrednie inicjowanie właściwości (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="b7e17-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="b7e17-116">`MyOptions` Klasy zostanie dodany do kontenera usługi za pomocą [Konfiguruj&lt;TOptions&gt;(IServiceCollection, wartości IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) i powiązane z konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="b7e17-116">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="b7e17-117">Stronie używa modelu [iniekcji zależności konstruktora](xref:fundamentals/dependency-injection#what-is-dependency-injection) z [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) można uzyskać dostęp do ustawień (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b7e17-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="b7e17-118">Przykładowe *appsettings.json* plik Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="b7e17-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="b7e17-119">Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b7e17-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="b7e17-120">Korzystając z niestandardowego [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) można załadować konfiguracji opcji z pliku ustawień, upewnij się, że ścieżka podstawowa jest prawidłowo:</span><span class="sxs-lookup"><span data-stu-id="b7e17-120">When using a custom [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> <span data-ttu-id="b7e17-121">Jawne ustawianie ścieżka podstawowa nie jest wymagana podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="b7e17-121">Explicitly setting the base path isn't required when loading options configuration from the settings file via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="b7e17-122">Skonfiguruj opcje prostego za pomocą delegata</span><span class="sxs-lookup"><span data-stu-id="b7e17-122">Configure simple options with a delegate</span></span>

<span data-ttu-id="b7e17-123">Konfigurowanie opcji prostego z delegatem przedstawiono przykład &num;2 [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b7e17-123">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b7e17-124">Użycie delegowania do ustawiania wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="b7e17-124">Use a delegate to set options values.</span></span> <span data-ttu-id="b7e17-125">Ta aplikacja używa przykładowych `MyOptionsWithDelegateConfig` klasy (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="b7e17-125">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="b7e17-126">W poniższym kodzie sekundy `IConfigureOptions<TOptions>` usługi zostanie dodany do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="b7e17-126">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="b7e17-127">Używa ona delegata do konfigurowania wiązania za pomocą `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="b7e17-127">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="b7e17-128">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b7e17-128">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="b7e17-129">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b7e17-129">You can add multiple configuration providers.</span></span> <span data-ttu-id="b7e17-130">Dostawcy konfiguracji są dostępne w pakietach NuGet.</span><span class="sxs-lookup"><span data-stu-id="b7e17-130">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="b7e17-131">Są one stosowane w celu zapewnienia ich rejestracji.</span><span class="sxs-lookup"><span data-stu-id="b7e17-131">They're applied in order that they're registered.</span></span>

<span data-ttu-id="b7e17-132">Każde wywołanie [Konfiguruj&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) dodaje `IConfigureOptions<TOptions>` usługi service container.</span><span class="sxs-lookup"><span data-stu-id="b7e17-132">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="b7e17-133">W powyższym przykładzie wartości `Option1` i `Option2` określono zarówno w *appsettings.json*, ale wartości `Option1` i `Option2` są zastępowane przez delegata skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="b7e17-133">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="b7e17-134">Po włączeniu więcej niż jednej konfiguracji usługi ostatni źródło konfiguracji określone *wins* i ustawia wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b7e17-134">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="b7e17-135">Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b7e17-135">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="b7e17-136">Konfiguracja suboptions</span><span class="sxs-lookup"><span data-stu-id="b7e17-136">Suboptions configuration</span></span>

<span data-ttu-id="b7e17-137">Suboptions konfiguracja jest przedstawiona jako przykład &num;3 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b7e17-137">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b7e17-138">Aplikacje powinny zostać utworzone opcje klasy, które odnoszą się do określonych funkcji grupy (klasy) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b7e17-138">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="b7e17-139">Części aplikacji, które wymagają wartości konfiguracji powinien mieć tylko dostęp do wartości konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="b7e17-139">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="b7e17-140">Podczas tworzenia powiązania opcje konfiguracji, każdej właściwości w typie opcji jest powiązany z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="b7e17-140">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="b7e17-141">Na przykład `MyOptions.Option1` właściwość jest powiązana z klucza `Option1`, które są odczytywane z `option1` właściwość *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b7e17-141">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="b7e17-142">W poniższym kodzie, a trzeci `IConfigureOptions<TOptions>` usługi zostanie dodany do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="b7e17-142">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="b7e17-143">Powiąże `MySubOptions` do sekcji `subsection` z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="b7e17-143">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="b7e17-144">`GetSection` — Metoda rozszerzenia wymaga [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="b7e17-144">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="b7e17-145">Jeśli aplikacja korzysta z [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 lub nowszej), pakiet jest automatycznie dołączane.</span><span class="sxs-lookup"><span data-stu-id="b7e17-145">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="b7e17-146">Przykładowe *appsettings.json* plik definiuje `subsection` członka za pomocą kluczy `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="b7e17-146">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="b7e17-147">`MySubOptions` Klasy definiuje właściwości, `SubOption1` i `SubOption2`, do przechowywania wartości opcji podrzędnych (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="b7e17-147">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="b7e17-148">Model strony `OnGet` metoda zwraca ciąg zawierający wartości opcji podrzędnych (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b7e17-148">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="b7e17-149">Gdy aplikacja jest uruchamiana, `OnGet` metoda zwraca ciąg przedstawiający podrzędnych opcję wartości klasy:</span><span class="sxs-lookup"><span data-stu-id="b7e17-149">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="b7e17-150">Opcje dostarczane przez model widoku lub przy użyciu iniekcji bezpośrednie widoku</span><span class="sxs-lookup"><span data-stu-id="b7e17-150">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="b7e17-151">Opcje dostarczane przez model widoku lub przy użyciu iniekcji bezpośrednie widoku przedstawiono przykład &num;4 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b7e17-151">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b7e17-152">Opcje mogą być podawane w modelu widoku lub przez iniekcję `IOptions<TOptions>` bezpośrednio w widoku (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b7e17-152">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="b7e17-153">Do bezpośredniej iniekcji należy wstrzyknąć `IOptions<MyOptions>` z `@inject` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="b7e17-153">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="b7e17-154">Gdy aplikacja jest uruchamiana, wartości opcji są wyświetlane na renderowanej stronie:</span><span class="sxs-lookup"><span data-stu-id="b7e17-154">When the app is run, the option values are shown in the rendered page:</span></span>

![Opcje wartości opcja1: value1_from_json i opcja2: -1 są ładowane z modelu, jak również iniekcję do widoku.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="b7e17-156">Załaduj ponownie dane konfiguracyjne z IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="b7e17-156">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="b7e17-157">Ponowne załadowanie danych konfiguracji z `IOptionsSnapshot` przedstawiono w przykładzie &num;5 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b7e17-157">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b7e17-158">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) obsługuje ponownego ładowania opcji z minimalnym przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="b7e17-158">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b7e17-159">Opcje są obliczane raz na każde żądanie, gdy dostęp do pamięci podręcznej i okresu istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="b7e17-159">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b7e17-160">`IOptionsSnapshot` jest migawką [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) i aktualizacje automatyczne zawsze, gdy monitor wyzwolenie zmienia się zależnie od zmiany źródła danych.</span><span class="sxs-lookup"><span data-stu-id="b7e17-160">`IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="b7e17-161">W poniższym przykładzie pokazano, jak nowy `IOptionsSnapshot` został utworzony po *appsettings.json* zmiany (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="b7e17-161">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="b7e17-162">Wiele żądań do serwera zwracają stałe wartości, dostarczone przez *appsettings.json* pliku do momentu zmiany pliku i ponowne załadowanie konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b7e17-162">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="b7e17-163">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="b7e17-163">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="b7e17-164">Zmień wartości w *appsettings.json* plik `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="b7e17-164">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="b7e17-165">Zapisz *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="b7e17-165">Save the *appsettings.json* file.</span></span> <span data-ttu-id="b7e17-166">Odśwież przeglądarkę, aby zobaczyć, że zostaną zaktualizowane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="b7e17-166">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="b7e17-167">Opcje pomocy technicznej, za pomocą IConfigureNamedOptions nazwane</span><span class="sxs-lookup"><span data-stu-id="b7e17-167">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="b7e17-168">Opcje pomocy technicznej, o nazwie [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) przedstawiono przykład &num;6 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b7e17-168">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b7e17-169">*O nazwie opcje* pomocy technicznej zezwala aplikacji na rozróżnienie między nazwane opcje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b7e17-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="b7e17-170">W przykładowej aplikacji o nazwie opcje są uznane za pomocą [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(Akcja IServiceCollection, String,&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure)który z kolei wywołuje metodę rozszerzenia [ConfigureNamedOptions&lt;TOptions&gt;. Konfigurowanie](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) metody:</span><span class="sxs-lookup"><span data-stu-id="b7e17-170">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="b7e17-171">Przykładowa aplikacja uzyskuje dostęp do opcji o nazwie za pomocą [IOptionsSnapshot&lt;TOptions&gt;. Pobierz](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b7e17-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="b7e17-172">Uruchamianie przykładowej aplikacji, zwracane są nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="b7e17-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="b7e17-173">`named_options_1` wartości są dostarczane z konfiguracji, które są ładowane z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="b7e17-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="b7e17-174">`named_options_2` wartości są dostarczane przez:</span><span class="sxs-lookup"><span data-stu-id="b7e17-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="b7e17-175">`named_options_2` Delegowanie w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b7e17-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="b7e17-176">Wartością domyślną dla `Option2` dostarczone przez `MyOptions` klasy.</span><span class="sxs-lookup"><span data-stu-id="b7e17-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="b7e17-177">Skonfiguruj wszystkie wystąpienia nazwanego opcje z [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) metody.</span><span class="sxs-lookup"><span data-stu-id="b7e17-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="b7e17-178">Poniższy kod służy do konfigurowania `Option1` wszystkie nazwane wystąpienia konfiguracji, za pomocą wspólnej wartości.</span><span class="sxs-lookup"><span data-stu-id="b7e17-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="b7e17-179">Dodaj następujący kod ręcznie do `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="b7e17-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="b7e17-180">Uruchamianie przykładowej aplikacji po dodaniu kod generuje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="b7e17-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="b7e17-181">Wszystkie opcje są nazwane wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="b7e17-181">All options are named instances.</span></span> <span data-ttu-id="b7e17-182">Istniejące `IConfigureOption` wystąpienia są traktowane jako docelowy `Options.DefaultName` wystąpienia, co jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b7e17-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="b7e17-183">`IConfigureNamedOptions` implementuje również `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="b7e17-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="b7e17-184">Domyślna implementacja klasy [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([źródło odwołania](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) zawiera logikę w celu używania każdego odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="b7e17-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="b7e17-185">`null` Nazwane opcja jest używana do Docieraj do wszystkich wystąpień nazwanych, zamiast określonego nazwanego wystąpienia ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) i [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) ta Konwencja).</span><span class="sxs-lookup"><span data-stu-id="b7e17-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="b7e17-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="b7e17-186">IPostConfigureOptions</span></span>

<span data-ttu-id="b7e17-187">Ustaw postconfiguration z [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="b7e17-187">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="b7e17-188">Postconfiguration jest uruchamiany po wszystkich [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) występuje konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="b7e17-188">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b7e17-189">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) pozwala konfigurować po opcji o nazwie:</span><span class="sxs-lookup"><span data-stu-id="b7e17-189">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b7e17-190">Użyj [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) po Konfiguracja wszystkich nazwanego wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="b7e17-190">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="b7e17-191">Opcje fabryki, monitorowanie i pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="b7e17-191">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="b7e17-192">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) jest używany dla powiadomień podczas `TOptions` wystąpienia zmiany.</span><span class="sxs-lookup"><span data-stu-id="b7e17-192">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="b7e17-193">`IOptionsMonitor` obsługuje opcje ładowany ponownie zmienić powiadomienia, i `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="b7e17-193">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b7e17-194">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) odpowiada za tworzenie nowych opcji wystąpień.</span><span class="sxs-lookup"><span data-stu-id="b7e17-194">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) is responsible for creating new options instances.</span></span> <span data-ttu-id="b7e17-195">Ma on jeden [Utwórz](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) metody.</span><span class="sxs-lookup"><span data-stu-id="b7e17-195">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="b7e17-196">Domyślna implementacja pobiera wszystkie zarejestrowane `IConfigureOptions` i `IPostConfigureOptions` i umożliwia uruchamianie wszystkich konfiguruje się najpierw, a następnie konfiguruje używane po tej operacji.</span><span class="sxs-lookup"><span data-stu-id="b7e17-196">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="b7e17-197">Jego rozróżnia `IConfigureNamedOptions` i `IConfigureOptions` i tylko wywołuje odpowiedniego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b7e17-197">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="b7e17-198">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) jest używany przez `IOptionsMonitor` do pamięci podręcznej `TOptions` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="b7e17-198">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="b7e17-199">`IOptionsMonitorCache` Unieważnia opcje wystąpień w monitorze, tak aby wartość jest ponownie przeliczana ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="b7e17-199">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="b7e17-200">Wartości można ręcznie wprowadzić również z [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="b7e17-200">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="b7e17-201">[Wyczyść](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) metoda jest używana, gdy wszystkie wystąpienia nazwanego, należy odtworzyć na żądanie.</span><span class="sxs-lookup"><span data-stu-id="b7e17-201">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

::: moniker-end

## <a name="accessing-options-during-startup"></a><span data-ttu-id="b7e17-202">Uzyskiwanie dostępu do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="b7e17-202">Accessing options during startup</span></span>

<span data-ttu-id="b7e17-203">`IOptions` mogą być używane w `Configure`, ponieważ usługi są kompilowane przed `Configure` metoda jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="b7e17-203">`IOptions` can be used in `Configure`, since services are built before the `Configure` method executes.</span></span> <span data-ttu-id="b7e17-204">Jeśli dostawca usług jest wbudowana `ConfigureServices` uzyskać dostęp do opcji, go nie ma zawierać opcje konfiguracjami dostarczonymi po utworzeniu dostawcy usługi.</span><span class="sxs-lookup"><span data-stu-id="b7e17-204">If a service provider is built in `ConfigureServices` to access options, it wouldn't contain any options configurations provided after the service provider is built.</span></span> <span data-ttu-id="b7e17-205">W związku z tym do stanu opcje niezgodne może istnieć z powodu zamawiania rejestracji usługi.</span><span class="sxs-lookup"><span data-stu-id="b7e17-205">Therefore, an inconsistent options state may exist due to the ordering of service registrations.</span></span>

<span data-ttu-id="b7e17-206">Ponieważ opcje są zwykle ładowane z konfiguracji, konfiguracji mogą być używane w uruchamiania w obu `Configure` i `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b7e17-206">Since options are typically loaded from configuration, configuration can be used in startup in both `Configure` and `ConfigureServices`.</span></span> <span data-ttu-id="b7e17-207">Przykłady korzystania z konfiguracji podczas uruchamiania, zobacz [uruchamiania aplikacji](xref:fundamentals/startup) tematu.</span><span class="sxs-lookup"><span data-stu-id="b7e17-207">For examples of using configuration during startup, see the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="see-also"></a><span data-ttu-id="b7e17-208">Zobacz także</span><span class="sxs-lookup"><span data-stu-id="b7e17-208">See also</span></span>

* [<span data-ttu-id="b7e17-209">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="b7e17-209">Configuration</span></span>](xref:fundamentals/configuration/index)
