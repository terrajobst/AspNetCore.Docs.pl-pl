---
title: Wzorzec opcje dla platformy ASP.NET Core
author: guardrex
description: "Wykryj sposób użycia wzorca opcje do reprezentowania grupy powiązanych ustawień w aplikacji platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: 7d89416626433bf737b63eda4b17e65b089ae142
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="5ea8a-103">Wzorzec opcje dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5ea8a-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="5ea8a-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5ea8a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5ea8a-105">Wzorzec opcje używa klas opcji do reprezentowania grupy ustawień.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="5ea8a-106">Ustawienia konfiguracji są izolowane przez funkcję klasom oddzielne opcje, aplikacja odpowiada dwóch zasad ważne engineering oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="5ea8a-107">[Zasady podział interfejsu (ISP)](http://deviq.com/interface-segregation-principle/): funkcje (klasy), które są zależne od ustawienia konfiguracji są zależne tylko od ustawienia konfiguracji, które korzystają z.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="5ea8a-108">[Separacji](http://deviq.com/separation-of-concerns/): ustawienia dla poszczególnych części aplikacji nie są zależne lub sprzężonego ze sobą.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="5ea8a-109">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) w tym artykule jest łatwiej postępuj zgodnie z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="5ea8a-110">Podstawowe opcje konfiguracji</span><span class="sxs-lookup"><span data-stu-id="5ea8a-110">Basic options configuration</span></span>

<span data-ttu-id="5ea8a-111">Podstawowe opcje konfiguracji przedstawiono przykład &num;1 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5ea8a-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5ea8a-112">Klasa opcji musi być typem abstrakcyjnym z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="5ea8a-113">Następujące klasy `MyOptions`, ma dwie właściwości `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="5ea8a-114">Ustawianie wartości domyślnych jest opcjonalne, ale konstruktora klasy w poniższym przykładzie ustawia domyślną wartość `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="5ea8a-115">`Option2`ma wartość domyślną, ustawione przez inicjowanie właściwość bezpośrednio (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="5ea8a-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="5ea8a-116">`MyOptions` Klasy jest dodawany do kontenera usługi z [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) i powiązane z konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="5ea8a-117">Następujące strony używa modelu [iniekcji zależności konstruktora](xref:fundamentals/dependency-injection#what-is-dependency-injection) z [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) uzyskać dostęp do ustawień (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5ea8a-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="5ea8a-118">Przykładowe *appsettings.json* pliku określa wartości `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="5ea8a-119">Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="5ea8a-120">Konfiguruj opcje proste delegata</span><span class="sxs-lookup"><span data-stu-id="5ea8a-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="5ea8a-121">Konfigurowanie opcji prostego z delegata przedstawiono przykład &num;2 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5ea8a-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5ea8a-122">Użyj delegata, aby ustawić opcje wartości.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-122">Use a delegate to set options values.</span></span> <span data-ttu-id="5ea8a-123">Przykładowe zastosowania aplikacji `MyOptionsWithDelegateConfig` klasy (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="5ea8a-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="5ea8a-124">W poniższym kodzie drugiej `IConfigureOptions<TOptions>` usługi jest dodawany do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="5ea8a-125">Używa delegata do konfigurowania wiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="5ea8a-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="5ea8a-127">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="5ea8a-128">W pakietach NuGet dostępnych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="5ea8a-129">Są one stosowane, że są one zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="5ea8a-130">Każde wywołanie [Konfiguruj&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) dodaje `IConfigureOptions<TOptions>` usługi do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="5ea8a-131">W powyższym przykładzie wartości `Option1` i `Option2` są określone w *appsettings.json*, ale wartości `Option1` i `Option2` zostały zastąpione przez skonfigurowany delegata.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="5ea8a-132">Po włączeniu więcej niż jedna usługa konfiguracji ostatniego źródło konfiguracji określone *wins* i ustawia wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="5ea8a-133">Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="5ea8a-134">Konfiguracja suboptions</span><span class="sxs-lookup"><span data-stu-id="5ea8a-134">Suboptions configuration</span></span>

<span data-ttu-id="5ea8a-135">Konfiguracja suboptions przedstawiono przykład &num;3 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5ea8a-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5ea8a-136">Aplikacje, należy utworzyć klasy opcje, które odnoszą się do określonych funkcji grupy (klasy) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="5ea8a-137">Części aplikacji, które wymagają wartości konfiguracji tylko powinien mieć dostęp do wartości konfiguracji, które używają.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="5ea8a-138">Podczas tworzenia wiązania opcje konfiguracji, każdej właściwości w typie opcje jest powiązany z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="5ea8a-139">Na przykład `MyOptions.Option1` właściwość jest powiązana z klucza `Option1`, które są odczytywane z `option1` właściwości w *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="5ea8a-140">W poniższym kodzie innej `IConfigureOptions<TOptions>` usługi jest dodawany do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="5ea8a-141">Tworzy ona powiązanie `MySubOptions` do sekcji `subsection` z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="5ea8a-142">`GetSection` — Metoda rozszerzenia wymaga [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="5ea8a-143">Jeśli aplikacja korzysta z [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, że pakiet jest automatycznie dołączane.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="5ea8a-144">Przykładowe *appsettings.json* plik definiuje `subsection` elementu członkowskiego z kluczami `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="5ea8a-145">`MySubOptions` Klasa definiuje właściwości `SubOption1` i `SubOption2`, do przechowywania wartości opcji podrzędne (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="5ea8a-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="5ea8a-146">Model strony `OnGet` metoda zwraca ciąg o wartości opcji podrzędne (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5ea8a-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="5ea8a-147">Gdy aplikacja jest uruchamiana, `OnGet` metoda zwraca ciąg przedstawiający opcji podrzędnego klasy wartości:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="5ea8a-148">Opcje przekazane przez model widoku lub widok bezpośredniego iniekcji</span><span class="sxs-lookup"><span data-stu-id="5ea8a-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="5ea8a-149">Opcje przekazane przez model widoku lub widok bezpośredniego iniekcji przedstawiono przykład &num;4 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5ea8a-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5ea8a-150">Opcje mogą być dostarczane w modelu widoku lub przez wstrzykiwanie `IOptions<TOptions>` bezpośrednio w widoku (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5ea8a-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="5ea8a-151">Bezpośrednie iniekcji wstrzyknąć `IOptions<MyOptions>` z `@inject` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="5ea8a-152">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Opcje wartości opcja 1: value1_from_json i Option2: -1 zostały załadowane z modelu i wstawiania do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="5ea8a-154">Załaduj ponownie dane konfiguracyjne z IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="5ea8a-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="5ea8a-155">Ponowne ładowanie danych konfiguracji z `IOptionsSnapshot` przedstawiono w przykładzie &num;5 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5ea8a-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5ea8a-156">*Wymaga platformy ASP.NET Core 1.1 lub nowszej.*</span><span class="sxs-lookup"><span data-stu-id="5ea8a-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="5ea8a-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) obsługuje ponowne ładowanie opcji z minimalnym przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="5ea8a-158">W programie ASP.NET Core 1.1 `IOptionsSnapshot` jest migawką [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) i aktualizacje automatyczne zawsze, gdy monitor wyzwala zmiany oparte na Zmiana źródła danych.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="5ea8a-159">W programie ASP.NET Core 2.0 lub nowszego oraz opcje są obliczane raz na każde żądanie, gdy dostępne i pamięci podręcznej przez czas ich istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="5ea8a-160">W poniższym przykładzie pokazano, jak nowy `IOptionsSnapshot` jest tworzony po *appsettings.json* zmiany (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="5ea8a-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="5ea8a-161">Wiele żądań do serwera zwracać wartości stałych przez *appsettings.json* pliku do momentu zmiany pliku i ponowne załadowanie konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="5ea8a-162">Na poniższej ilustracji przedstawiono początkowej `option1` i `option2` wartości załadowane z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="5ea8a-163">Zmień wartości w *appsettings.json* pliku `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="5ea8a-164">Zapisz *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="5ea8a-165">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji zostały zaktualizowane:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="5ea8a-166">O nazwie Opcje pomocy dotyczącej IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="5ea8a-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="5ea8a-167">Opcje obsługi o nazwie [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) przedstawiono przykład &num;6 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5ea8a-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5ea8a-168">*Wymaga platformy ASP.NET Core 2.0 lub nowszej.*</span><span class="sxs-lookup"><span data-stu-id="5ea8a-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="5ea8a-169">*O nazwie opcje* pomocy technicznej umożliwia rozróżnienia nazwanego opcje konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="5ea8a-170">W przykładowej aplikacji, opcje nazwanego został zadeklarowany z [ConfigureNamedOptions&lt;TOptions&gt;. Skonfiguruj](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) metody:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="5ea8a-171">Przykładowa aplikacja uzyskuje dostęp do opcji nazwanego za pomocą [IOptionsSnapshot&lt;TOptions&gt;. Pobierz](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5ea8a-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="5ea8a-172">Uruchomiona przykładowej aplikacji, zwracane są nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="5ea8a-173">`named_options_1`wartości są dostarczane z konfiguracji, które są ładowane z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="5ea8a-174">`named_options_2`wartości są dostarczane przez:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="5ea8a-175">`named_options_2` Delegowanie w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="5ea8a-176">Wartość domyślna dla `Option2` podał `MyOptions` klasy.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="5ea8a-177">Skonfiguruj wszystkie wystąpienia nazwanego opcje z [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) metody.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="5ea8a-178">Poniższy kod konfiguruje `Option1` wszystkie nazwane wystąpienia konfiguracji z wartością wspólnej.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="5ea8a-179">Dodaj następujący kod ręcznie do `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="5ea8a-180">Uruchamianie przykładowej aplikacji po dodaniu kod tworzy następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="5ea8a-181">W programie ASP.NET Core 2.0 lub nowszy wszystkie opcje są nazwane wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="5ea8a-182">Istniejące `IConfigureOption` wystąpienia są traktowane jak `Options.DefaultName` wystąpienia, która jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="5ea8a-183">`IConfigureNamedOptions`implementuje również `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="5ea8a-184">Domyślna implementacja [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([źródło odwołania](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) zawiera logikę używania każdego odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="5ea8a-185">`null` Nazwanego opcja jest używana do wszystkich wystąpień nazwanych zamiast określonego nazwanego wystąpienia docelowego ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) i [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) Użyj tę Konwencję).</span><span class="sxs-lookup"><span data-stu-id="5ea8a-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="5ea8a-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="5ea8a-186">IPostConfigureOptions</span></span>

<span data-ttu-id="5ea8a-187">*Wymaga platformy ASP.NET Core 2.0 lub nowszej.*</span><span class="sxs-lookup"><span data-stu-id="5ea8a-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="5ea8a-188">Ustaw postconfiguration z [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="5ea8a-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="5ea8a-189">Postconfiguration jest uruchamiana po wszystkich [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) występuje konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5ea8a-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) pozwala konfigurować po nazwanych opcje:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5ea8a-191">Użyj [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) po skonfigurować wszystkie nazwane wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="5ea8a-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="5ea8a-192">Opcje fabryki, monitorowania i pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="5ea8a-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="5ea8a-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) jest używany dla powiadomienia po `TOptions` wystąpienia zmiany.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="5ea8a-194">`IOptionsMonitor`obsługuje opcje instrument, zmień powiadomienia, a `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="5ea8a-195">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (platformy ASP.NET Core w wersji 2.0 lub nowszej) jest odpowiedzialny za tworzenie nowych opcji wystąpień.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="5ea8a-196">Ma on jeden [Utwórz](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) metody.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="5ea8a-197">Domyślna implementacja pobiera wszystkich zarejestrowanych `IConfigureOptions` i `IPostConfigureOptions` i uruchamia wszystkie konfiguruje się najpierw, a następnie po konfiguruje.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="5ea8a-198">Go rozróżnia `IConfigureNamedOptions` i `IConfigureOptions` i tylko wywołuje odpowiedniego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="5ea8a-199">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (platformy ASP.NET Core w wersji 2.0 lub nowszej) jest używany przez `IOptionsMonitor` do pamięci podręcznej `TOptions` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="5ea8a-200">`IOptionsMonitorCache` Unieważnia wystąpień opcje w monitorze tak, aby wartość jest przeliczane ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="5ea8a-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="5ea8a-201">Wartości mogą być ręcznie wprowadzone również z [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="5ea8a-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="5ea8a-202">[Wyczyść](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) metoda jest używana, gdy wszystkie wystąpienia nazwanego powinny zostać ponownie utworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="5ea8a-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="see-also"></a><span data-ttu-id="5ea8a-203">Zobacz także</span><span class="sxs-lookup"><span data-stu-id="5ea8a-203">See also</span></span>

* [<span data-ttu-id="5ea8a-204">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="5ea8a-204">Configuration</span></span>](xref:fundamentals/configuration/index)
