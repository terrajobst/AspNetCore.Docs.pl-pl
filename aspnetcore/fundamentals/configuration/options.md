---
title: Wzorzec opcje w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak użyć wzorca opcje do reprezentowania grup powiązanych ustawień w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: c553062bbec31ba5bd437eb0bd29e007ae93c65e
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756615"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="21ac0-103">Wzorzec opcje w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21ac0-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="21ac0-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="21ac0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="21ac0-105">Wzorzec opcje używa klas do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="21ac0-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="21ac0-106">Jeśli ustawienia konfiguracji są izolowane przez funkcję w osobnych klas, aplikacja działa zgodnie z dwóch zasad ważne inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="21ac0-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="21ac0-107">[Zasady podziału interfejsu (ISP)](http://deviq.com/interface-segregation-principle/): funkcji (grupy), które są zależne od ustawień konfiguracji tylko zależą od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="21ac0-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="21ac0-108">[Separacji](http://deviq.com/separation-of-concerns/): ustawienia dla poszczególnych części aplikacji nie są zależnych lub powiązanych ze sobą.</span><span class="sxs-lookup"><span data-stu-id="21ac0-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="21ac0-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) ten artykuł stanowi łatwiejszej do obserwowania z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21ac0-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="21ac0-110">Prerequisites</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="21ac0-111">Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="21ac0-111">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="21ac0-112">Odwołanie [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="21ac0-112">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="21ac0-113">Dodaj odwołanie do pakietu [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="21ac0-113">Add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

## <a name="basic-options-configuration"></a><span data-ttu-id="21ac0-114">Podstawowe opcje konfiguracji</span><span class="sxs-lookup"><span data-stu-id="21ac0-114">Basic options configuration</span></span>

<span data-ttu-id="21ac0-115">Opcje podstawowe konfiguracja jest przedstawiona jako przykład &num;1 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="21ac0-115">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="21ac0-116">Klasa opcji musi być nieabstrakcyjnej przy użyciu publicznego konstruktora bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="21ac0-116">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="21ac0-117">Następujące klasy `MyOptions`, ma dwie właściwości `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-117">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="21ac0-118">Ustawianie wartości domyślnych jest opcjonalne, ale konstruktora klasy w poniższym przykładzie ustawiono wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-118">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="21ac0-119">`Option2` ma wartość domyślną, ustawiane przez bezpośrednie inicjowanie właściwości (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="21ac0-119">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="21ac0-120">`MyOptions` Klasy zostanie dodany do kontenera usługi za pomocą [Konfiguruj&lt;TOptions&gt;(IServiceCollection, wartości IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) i powiązane z konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="21ac0-120">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="21ac0-121">Stronie używa modelu [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) można uzyskać dostęp do ustawień (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="21ac0-121">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="21ac0-122">Przykładowe *appsettings.json* plik Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="21ac0-122">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="21ac0-123">Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="21ac0-123">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="21ac0-124">Korzystając z niestandardowego [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) można załadować konfiguracji opcji z pliku ustawień, upewnij się, że ścieżka podstawowa jest prawidłowo:</span><span class="sxs-lookup"><span data-stu-id="21ac0-124">When using a custom [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="21ac0-125">Jawne ustawianie ścieżka podstawowa nie jest wymagana podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="21ac0-125">Explicitly setting the base path isn't required when loading options configuration from the settings file via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="21ac0-126">Skonfiguruj opcje prostego za pomocą delegata</span><span class="sxs-lookup"><span data-stu-id="21ac0-126">Configure simple options with a delegate</span></span>

<span data-ttu-id="21ac0-127">Konfigurowanie opcji prostego z delegatem przedstawiono przykład &num;2 [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="21ac0-127">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="21ac0-128">Użycie delegowania do ustawiania wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-128">Use a delegate to set options values.</span></span> <span data-ttu-id="21ac0-129">Ta aplikacja używa przykładowych `MyOptionsWithDelegateConfig` klasy (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="21ac0-129">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="21ac0-130">W poniższym kodzie sekundy `IConfigureOptions<TOptions>` usługi zostanie dodany do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="21ac0-130">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="21ac0-131">Używa ona delegata do konfigurowania wiązania za pomocą `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="21ac0-131">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="21ac0-132">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="21ac0-132">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="21ac0-133">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-133">You can add multiple configuration providers.</span></span> <span data-ttu-id="21ac0-134">Dostawcy konfiguracji są dostępne w pakietach NuGet.</span><span class="sxs-lookup"><span data-stu-id="21ac0-134">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="21ac0-135">Są one stosowane w celu zapewnienia ich rejestracji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-135">They're applied in order that they're registered.</span></span>

<span data-ttu-id="21ac0-136">Każde wywołanie [Konfiguruj&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) dodaje `IConfigureOptions<TOptions>` usługi service container.</span><span class="sxs-lookup"><span data-stu-id="21ac0-136">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="21ac0-137">W powyższym przykładzie wartości `Option1` i `Option2` określono zarówno w *appsettings.json*, ale wartości `Option1` i `Option2` są zastępowane przez delegata skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="21ac0-137">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="21ac0-138">Po włączeniu więcej niż jednej konfiguracji usługi ostatni źródło konfiguracji określone *wins* i ustawia wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-138">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="21ac0-139">Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="21ac0-139">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="21ac0-140">Konfiguracja suboptions</span><span class="sxs-lookup"><span data-stu-id="21ac0-140">Suboptions configuration</span></span>

<span data-ttu-id="21ac0-141">Suboptions konfiguracja jest przedstawiona jako przykład &num;3 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="21ac0-141">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="21ac0-142">Aplikacje powinny zostać utworzone opcje klasy, które odnoszą się do określonych funkcji grupy (klasy) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-142">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="21ac0-143">Części aplikacji, które wymagają wartości konfiguracji powinien mieć tylko dostęp do wartości konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="21ac0-143">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="21ac0-144">Podczas tworzenia powiązania opcje konfiguracji, każdej właściwości w typie opcji jest powiązany z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-144">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="21ac0-145">Na przykład `MyOptions.Option1` właściwość jest powiązana z klucza `Option1`, które są odczytywane z `option1` właściwość *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="21ac0-145">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="21ac0-146">W poniższym kodzie, a trzeci `IConfigureOptions<TOptions>` usługi zostanie dodany do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="21ac0-146">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="21ac0-147">Powiąże `MySubOptions` do sekcji `subsection` z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="21ac0-147">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="21ac0-148">`GetSection` — Metoda rozszerzenia wymaga [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="21ac0-148">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="21ac0-149">Jeśli aplikacja korzysta z [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 lub nowszej), pakiet jest automatycznie dołączane.</span><span class="sxs-lookup"><span data-stu-id="21ac0-149">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="21ac0-150">Przykładowe *appsettings.json* plik definiuje `subsection` członka za pomocą kluczy `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="21ac0-150">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="21ac0-151">`MySubOptions` Klasy definiuje właściwości, `SubOption1` i `SubOption2`, aby przechowywać wartości opcji (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="21ac0-151">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="21ac0-152">Model strony `OnGet` metoda zwraca ciąg zawierający wartości opcji (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="21ac0-152">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="21ac0-153">Gdy aplikacja jest uruchamiana, `OnGet` metoda zwraca ciąg przedstawiający podrzędnych opcję wartości klasy:</span><span class="sxs-lookup"><span data-stu-id="21ac0-153">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="21ac0-154">Opcje dostarczane przez model widoku lub przy użyciu iniekcji bezpośrednie widoku</span><span class="sxs-lookup"><span data-stu-id="21ac0-154">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="21ac0-155">Opcje dostarczane przez model widoku lub przy użyciu iniekcji bezpośrednie widoku przedstawiono przykład &num;4 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="21ac0-155">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="21ac0-156">Opcje mogą być podawane w modelu widoku lub przez iniekcję `IOptions<TOptions>` bezpośrednio w widoku (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="21ac0-156">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="21ac0-157">Do bezpośredniej iniekcji należy wstrzyknąć `IOptions<MyOptions>` z `@inject` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="21ac0-157">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="21ac0-158">Gdy aplikacja jest uruchamiana, wartości opcje są wyświetlane na renderowanej stronie:</span><span class="sxs-lookup"><span data-stu-id="21ac0-158">When the app is run, the options values are shown in the rendered page:</span></span>

![Opcje wartości opcja1: value1_from_json i opcja2: -1 są ładowane z modelu, jak również iniekcję do widoku.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="21ac0-160">Załaduj ponownie dane konfiguracyjne z IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="21ac0-160">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="21ac0-161">Ponowne załadowanie danych konfiguracji z `IOptionsSnapshot` przedstawiono w przykładzie &num;5 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="21ac0-161">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="21ac0-162">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) obsługuje ponownego ładowania opcji z minimalnym przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="21ac0-162">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="21ac0-163">Opcje są obliczane raz na każde żądanie, gdy dostęp do pamięci podręcznej i okresu istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="21ac0-163">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="21ac0-164">`IOptionsSnapshot` jest migawką [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) i aktualizacje automatyczne zawsze, gdy monitor wyzwolenie zmienia się zależnie od zmiany źródła danych.</span><span class="sxs-lookup"><span data-stu-id="21ac0-164">`IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="21ac0-165">W poniższym przykładzie pokazano, jak nowy `IOptionsSnapshot` został utworzony po *appsettings.json* zmiany (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="21ac0-165">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="21ac0-166">Wiele żądań do serwera zwracają stałe wartości, dostarczone przez *appsettings.json* pliku do momentu zmiany pliku i ponowne załadowanie konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-166">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="21ac0-167">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="21ac0-167">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="21ac0-168">Zmień wartości w *appsettings.json* plik `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-168">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="21ac0-169">Zapisz *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="21ac0-169">Save the *appsettings.json* file.</span></span> <span data-ttu-id="21ac0-170">Odśwież przeglądarkę, aby zobaczyć, że zostaną zaktualizowane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="21ac0-170">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="21ac0-171">Opcje pomocy technicznej, za pomocą IConfigureNamedOptions nazwane</span><span class="sxs-lookup"><span data-stu-id="21ac0-171">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="21ac0-172">Opcje pomocy technicznej, o nazwie [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) przedstawiono przykład &num;6 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="21ac0-172">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="21ac0-173">*O nazwie opcje* pomocy technicznej zezwala aplikacji na rozróżnienie między nazwane opcje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-173">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="21ac0-174">W przykładowej aplikacji o nazwie opcje są uznane za pomocą [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(Akcja IServiceCollection, String,&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure)który z kolei wywołuje metodę rozszerzenia [ConfigureNamedOptions&lt;TOptions&gt;. Konfigurowanie](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) metody:</span><span class="sxs-lookup"><span data-stu-id="21ac0-174">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="21ac0-175">Przykładowa aplikacja uzyskuje dostęp do opcji o nazwie za pomocą [IOptionsSnapshot&lt;TOptions&gt;. Pobierz](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="21ac0-175">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="21ac0-176">Uruchamianie przykładowej aplikacji, zwracane są nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="21ac0-176">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="21ac0-177">`named_options_1` wartości są dostarczane z konfiguracji, które są ładowane z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="21ac0-177">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="21ac0-178">`named_options_2` wartości są dostarczane przez:</span><span class="sxs-lookup"><span data-stu-id="21ac0-178">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="21ac0-179">`named_options_2` Delegowanie w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-179">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="21ac0-180">Wartością domyślną dla `Option2` dostarczone przez `MyOptions` klasy.</span><span class="sxs-lookup"><span data-stu-id="21ac0-180">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="21ac0-181">Skonfiguruj wszystkie wystąpienia nazwanego opcje z [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) metody.</span><span class="sxs-lookup"><span data-stu-id="21ac0-181">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="21ac0-182">Poniższy kod służy do konfigurowania `Option1` wszystkie nazwane wystąpienia konfiguracji, za pomocą wspólnej wartości.</span><span class="sxs-lookup"><span data-stu-id="21ac0-182">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="21ac0-183">Dodaj następujący kod ręcznie do `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="21ac0-183">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="21ac0-184">Uruchamianie przykładowej aplikacji po dodaniu kod generuje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="21ac0-184">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="21ac0-185">Wszystkie opcje są nazwane wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="21ac0-185">All options are named instances.</span></span> <span data-ttu-id="21ac0-186">Istniejące `IConfigureOption` wystąpienia są traktowane jako docelowy `Options.DefaultName` wystąpienia, co jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-186">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="21ac0-187">`IConfigureNamedOptions` implementuje również `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-187">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="21ac0-188">Domyślna implementacja klasy [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([źródło odwołania](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) zawiera logikę w celu używania każdego odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="21ac0-188">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="21ac0-189">`null` Nazwane opcja jest używana do Docieraj do wszystkich wystąpień nazwanych, zamiast określonego nazwanego wystąpienia ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) i [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) ta Konwencja).</span><span class="sxs-lookup"><span data-stu-id="21ac0-189">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="21ac0-190">Opcje weryfikacji</span><span class="sxs-lookup"><span data-stu-id="21ac0-190">Options validation</span></span>

<span data-ttu-id="21ac0-191">Opcje weryfikacji umożliwia sprawdzenie poprawności opcji, po skonfigurowaniu opcji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-191">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="21ac0-192">Wywołaj `Validate` przy użyciu metody sprawdzania poprawności, która zwraca `true` Jeśli opcje są prawidłowe i `false` Jeśli nie są prawidłowe:</span><span class="sxs-lookup"><span data-stu-id="21ac0-192">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");
        
// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
} 
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

<span data-ttu-id="21ac0-193">Poprzedni przykład ustawia wystąpienia nazwanego opcji `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-193">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="21ac0-194">Wystąpienie domyślne opcje to `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-194">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="21ac0-195">Sprawdzanie poprawności jest uruchamiany, gdy tworzone jest wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-195">Validation runs when the options instance is created.</span></span> <span data-ttu-id="21ac0-196">Wystąpienie opcji jest gwarantowane do przekazania razem pierwszego sprawdzania poprawności, gdy jest on dostępny.</span><span class="sxs-lookup"><span data-stu-id="21ac0-196">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21ac0-197">Opcje sprawdzania poprawności nie je przed nieprzewidzianymi uniemożliwiającą modyfikacje opcje po opcji są wstępnie skonfigurowane i zweryfikowane.</span><span class="sxs-lookup"><span data-stu-id="21ac0-197">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="21ac0-198">`Validate` Metoda przyjmuje `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-198">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="21ac0-199">Aby w pełni dostosować sprawdzanie poprawności, należy zaimplementować `IValidateOptions<TOptions>`, co umożliwia:</span><span class="sxs-lookup"><span data-stu-id="21ac0-199">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="21ac0-200">Sprawdzanie poprawności wielu typów opcji: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="21ac0-200">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="21ac0-201">Sprawdzanie poprawności, który jest zależny od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="21ac0-201">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span></span>

<span data-ttu-id="21ac0-202">`IValidateOptions` sprawdza poprawność:</span><span class="sxs-lookup"><span data-stu-id="21ac0-202">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="21ac0-203">Określonego nazwanego wystąpienia opcje.</span><span class="sxs-lookup"><span data-stu-id="21ac0-203">A specific named options instance.</span></span>
* <span data-ttu-id="21ac0-204">Wszystkie opcje, kiedy `name` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-204">All options when `name` is `null`.</span></span>

<span data-ttu-id="21ac0-205">Zwróć `ValidateOptionsResult` z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="21ac0-205">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="21ac0-206">Weryfikacja eager (po awarii szybkie przy uruchamianiu) i sprawdzanie poprawności danych na podstawie adnotacji są planowane w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-206">Eager validation (fail fast at startup) and data annotation-based validation are scheduled for a future release.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="ipostconfigureoptions"></a><span data-ttu-id="21ac0-207">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="21ac0-207">IPostConfigureOptions</span></span>

<span data-ttu-id="21ac0-208">Ustaw postconfiguration z [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="21ac0-208">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="21ac0-209">Postconfiguration jest uruchamiany po wszystkich [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) występuje konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="21ac0-209">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="21ac0-210">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) pozwala konfigurować po opcji o nazwie:</span><span class="sxs-lookup"><span data-stu-id="21ac0-210">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="21ac0-211">Użyj [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) po Konfiguracja wszystkich nazwanego wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="21ac0-211">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="21ac0-212">Opcje fabryki, monitorowanie i pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="21ac0-212">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="21ac0-213">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) jest używany dla powiadomień podczas `TOptions` wystąpienia zmiany.</span><span class="sxs-lookup"><span data-stu-id="21ac0-213">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="21ac0-214">`IOptionsMonitor` obsługuje opcje ładowany ponownie zmienić powiadomienia, i `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-214">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="21ac0-215">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) odpowiada za tworzenie nowych opcji wystąpień.</span><span class="sxs-lookup"><span data-stu-id="21ac0-215">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) is responsible for creating new options instances.</span></span> <span data-ttu-id="21ac0-216">Ma on jeden [Utwórz](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) metody.</span><span class="sxs-lookup"><span data-stu-id="21ac0-216">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="21ac0-217">Domyślna implementacja pobiera wszystkie zarejestrowane `IConfigureOptions` i `IPostConfigureOptions` i umożliwia uruchamianie wszystkich konfiguruje się najpierw, a następnie konfiguruje używane po tej operacji.</span><span class="sxs-lookup"><span data-stu-id="21ac0-217">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="21ac0-218">Jego rozróżnia `IConfigureNamedOptions` i `IConfigureOptions` i tylko wywołuje odpowiedniego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="21ac0-218">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="21ac0-219">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) jest używany przez `IOptionsMonitor` do pamięci podręcznej `TOptions` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="21ac0-219">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="21ac0-220">`IOptionsMonitorCache` Unieważnia opcje wystąpień w monitorze, tak aby wartość jest ponownie przeliczana ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="21ac0-220">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="21ac0-221">Wartości można ręcznie wprowadzić również z [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="21ac0-221">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="21ac0-222">[Wyczyść](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) metoda jest używana, gdy wszystkie wystąpienia nazwanego, należy odtworzyć na żądanie.</span><span class="sxs-lookup"><span data-stu-id="21ac0-222">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

::: moniker-end

## <a name="accessing-options-during-startup"></a><span data-ttu-id="21ac0-223">Uzyskiwanie dostępu do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="21ac0-223">Accessing options during startup</span></span>

<span data-ttu-id="21ac0-224">`IOptions` mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed `Configure` metoda jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="21ac0-224">`IOptions` can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

<span data-ttu-id="21ac0-225">`IOptions` nie powinny być używane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="21ac0-225">`IOptions` shouldn't be used in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="21ac0-226">Stan opcje niezgodne mogą występować z powodu zamawiania rejestracji usługi.</span><span class="sxs-lookup"><span data-stu-id="21ac0-226">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="21ac0-227">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="21ac0-227">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
