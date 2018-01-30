---
title: Konfiguracja platformy ASP.NET Core
author: rick-anderson
description: "Interfejs API konfiguracji umożliwia konfigurowanie aplikacji platformy ASP.NET Core za pomocą wielu metod."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 6b9dcfcc2fa380b601eee56095f2e6a6dbe07732
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="1ec4b-103">Konfigurowanie aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ec4b-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="1ec4b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Michaelis znak](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Roth Danielowi](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1ec4b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1ec4b-105">Interfejs API konfiguracji umożliwia konfigurowanie platformy ASP.NET Core aplikacji sieci web na podstawie listy par nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="1ec4b-106">Konfiguracja jest do odczytu w czasie wykonywania z wielu źródeł.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="1ec4b-107">Pary nazwa wartość można grupować w wielopoziomowej hierarchii.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="1ec4b-108">Brak dostawcy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-108">There are configuration providers for:</span></span>

* <span data-ttu-id="1ec4b-109">Format pliku (INI, JSON i XML)</span><span class="sxs-lookup"><span data-stu-id="1ec4b-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="1ec4b-110">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1ec4b-110">Command-line arguments</span></span>
* <span data-ttu-id="1ec4b-111">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="1ec4b-111">Environment variables</span></span>
* <span data-ttu-id="1ec4b-112">Obiekty .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="1ec4b-112">In-memory .NET objects</span></span>
* <span data-ttu-id="1ec4b-113">Magazyn zaszyfrowanych użytkownika</span><span class="sxs-lookup"><span data-stu-id="1ec4b-113">An encrypted user store</span></span>
* [<span data-ttu-id="1ec4b-114">Usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1ec4b-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="1ec4b-115">Dostawców niestandardowych (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="1ec4b-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="1ec4b-116">Każda wartość konfiguracji jest mapowany na klucz ciągu.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="1ec4b-117">Brak obsługi wbudowanych powiązanie zdeserializować ustawień do niestandardowego [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) obiektu (.NET klasą prostą z właściwościami).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="1ec4b-118">Wzorzec opcje używa klas opcji do reprezentowania grupy ustawień.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="1ec4b-119">Aby uzyskać więcej informacji o wykorzystaniu wzorca opcji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="1ec4b-120">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1ec4b-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="1ec4b-121">JSON konfiguracji</span><span class="sxs-lookup"><span data-stu-id="1ec4b-121">JSON configuration</span></span>

<span data-ttu-id="1ec4b-122">Następujące aplikacji konsoli używa dostawcy konfiguracji JSON:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="1ec4b-123">Odczytuje i wyświetla następujące ustawienia konfiguracji aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="1ec4b-124">Konfiguracja obejmuje hierarchiczną listę par nazwa wartość, w których węzły są oddzielone dwukropkiem.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="1ec4b-125">Aby pobrać wartość, należy pobrać `Configuration` indeksatora kluczem odpowiedniego elementu:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="1ec4b-126">Aby pracować z tablic w formacie JSON konfiguracji źródła, należy użyć indeks tablicy jako część ciągu oddzielone dwukropkiem.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="1ec4b-127">Poniższy przykład pobiera nazwę pierwszego elementu w poprzednim `wizards` tablicy:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="1ec4b-128">Pary nazwa wartość zapisywane do wbudowanej [konfiguracji](/dotnet/api/microsoft.extensions.configuration) dostawcy są **nie** utrwalone.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="1ec4b-129">Jednak można utworzyć niestandardowego dostawcę, który zapisuje wartości.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="1ec4b-130">Zobacz [niestandardowego dostawcy konfiguracji](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="1ec4b-131">Powyższego przykładu używa indeksatora konfiguracji można odczytać wartości.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="1ec4b-132">Do konfiguracji dostępu poza `Startup`, użyj *wzorzec opcje*.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="1ec4b-133">Aby uzyskać więcej informacji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>


## <a name="configuration-by-environment"></a><span data-ttu-id="1ec4b-134">Konfiguracja przez środowisko</span><span class="sxs-lookup"><span data-stu-id="1ec4b-134">Configuration by environment</span></span>

<span data-ttu-id="1ec4b-135">Jest to typowe do innej konfiguracji ustawień dla różnych środowisk, na przykład programowania, testowania i produkcji.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-135">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="1ec4b-136">`CreateDefaultBuilder` — Metoda rozszerzenia w aplikacji platformy ASP.NET Core 2.x (lub przy użyciu `AddJsonFile` i `AddEnvironmentVariables` bezpośrednio w aplikacji platformy ASP.NET Core 1.x) dodaje dostawców konfiguracji do odczytywania źródeł konfiguracji systemu i pliki w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-136">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="1ec4b-137">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="1ec4b-137">*appsettings.json*</span></span>
* <span data-ttu-id="1ec4b-138">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="1ec4b-138">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="1ec4b-139">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="1ec4b-139">Environment variables</span></span>

<span data-ttu-id="1ec4b-140">Aplikacje 1.x platformy ASP.NET Core musi wywołać `AddJsonFile` i [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-140">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="1ec4b-141">Zobacz [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) dla objaśnienie parametrów.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-141">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="1ec4b-142">`reloadOnChange`jest obsługiwana tylko w ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-142">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="1ec4b-143">Konfiguracja źródła są do odczytu w kolejności, że jest określona.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-143">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="1ec4b-144">W powyższym kodzie zmienne środowiskowe są odczytywane ostatnio.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-144">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="1ec4b-145">Wartości konfiguracji ustawiana za pośrednictwem Zamień środowiska określone w poprzednich dwóch dostawców.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-145">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="1ec4b-146">Należy rozważyć *appsettings. Staging.JSON* pliku:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-146">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[Main](index/sample/appsettings.Staging.json)]

<span data-ttu-id="1ec4b-147">Jeśli środowisko jest równa `Staging`, następujące `Configure` metoda odczytuje wartość `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-147">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="1ec4b-148">Środowisko jest zwykle ustawiana na `Development`, `Staging`, lub `Production`.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-148">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="1ec4b-149">Aby uzyskać więcej informacji, zobacz [Praca w środowiskach wielu](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-149">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="1ec4b-150">Zagadnienia dotyczące konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-150">Configuration considerations:</span></span>

* <span data-ttu-id="1ec4b-151">`IOptionsSnapshot`można ponownie załadować dane konfiguracji, gdy zmienia.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-151">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="1ec4b-152">Aby uzyskać więcej informacji, zobacz [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span><span class="sxs-lookup"><span data-stu-id="1ec4b-152">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="1ec4b-153">Klucze konfiguracji **nie** z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-153">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="1ec4b-154">**Nigdy nie** przechowywania haseł i innych poufnych danych w konfiguracji dostawcy kodu lub pliki konfiguracyjne w formacie zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-154">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="1ec4b-155">Nie używasz produkcji kluczy tajnych w rozwoju lub testowania środowisk.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-155">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="1ec4b-156">Określ klucze tajne poza projektem, aby nie może być przypadkowo przekazane do repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-156">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="1ec4b-157">Dowiedz się więcej o [Praca w środowiskach wielu](xref:fundamentals/environments) i zarządzanie [bezpiecznego magazynu kluczy tajnych aplikacji podczas tworzenia](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-157">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="1ec4b-158">Jeśli dwukropkiem (`:`) nie może być używany w zmiennych środowiskowych w systemie, Zastąp dwukropkiem (`:`) o podwójnej precyzji znak podkreślenia (`__`).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-158">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="1ec4b-159">Dostawca w pamięci i powiązanie z klasą POCO</span><span class="sxs-lookup"><span data-stu-id="1ec4b-159">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="1ec4b-160">Poniższy przykład przedstawia sposób korzystają z dostawcy w pamięci i powiąż do klasy:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-160">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="1ec4b-161">Wartości konfiguracji są zwracane jako ciągi, ale powiązanie umożliwia konstrukcji obiektów.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-161">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="1ec4b-162">Powiązanie umożliwia pobieranie obiektów POCO lub wykresy nawet całego obiektu.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-162">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="1ec4b-163">GetValue</span><span class="sxs-lookup"><span data-stu-id="1ec4b-163">GetValue</span></span>

<span data-ttu-id="1ec4b-164">W poniższym przykładzie pokazano [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-164">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="1ec4b-165">ConfigurationBinder `GetValue<T>` metody umożliwia określenie wartości domyślnej (80 w próbce).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-165">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="1ec4b-166">`GetValue<T>`Służy do scenariuszy proste i nie należy powiązać całą sekcję.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-166">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="1ec4b-167">`GetValue<T>`pobiera wartości skalarnych z `GetSection(key).Value` przekonwertować dla określonego typu.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-167">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="1ec4b-168">Powiązanie do obiektu wykresu.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-168">Bind to an object graph</span></span>

<span data-ttu-id="1ec4b-169">Każdy obiekt w klasie można rekursywnie powiązany.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-169">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="1ec4b-170">Należy rozważyć `AppSettings` klasy:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-170">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="1ec4b-171">Poniższy przykład tworzy powiązanie z `AppSettings` klasy:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-171">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="1ec4b-172">**Platformy ASP.NET Core 1.1** i wyższe służy `Get<T>`, który współpracuje z całą sekcję.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-172">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="1ec4b-173">`Get<T>`może być wygodniejsze niż przy użyciu `Bind`.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-173">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="1ec4b-174">Poniższy kod przedstawia sposób użycia `Get<T>` z powyższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-174">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="1ec4b-175">Użycie następujących *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-175">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="1ec4b-176">Wyświetla program `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-176">The program displays `Height 11`.</span></span>

<span data-ttu-id="1ec4b-177">Poniższy kod może służyć do jednostki jest przetestowanie konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-177">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="1ec4b-178">Tworzenie niestandardowego dostawcy programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1ec4b-178">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="1ec4b-179">W tej sekcji tworzony jest odczytujący pary nazwa wartość z bazy danych przy użyciu EF dostawca konfiguracji podstawowej.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-179">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="1ec4b-180">Zdefiniuj `ConfigurationValue` jednostki do przechowywania wartości konfiguracji w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-180">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="1ec4b-181">Dodaj `ConfigurationContext` do przechowywania i uzyskać dostęp do skonfigurowanego wartości:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-181">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="1ec4b-182">Utwórz klasę, która implementuje [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="1ec4b-182">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="1ec4b-183">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-183">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="1ec4b-184">Dostawca konfiguracji inicjuje bazy danych, gdy nie jest pusty:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-184">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="1ec4b-185">Wyróżnione wartości z bazy danych ("value_from_ef_1" i "value_from_ef_2") są wyświetlane po uruchomieniu próbki.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-185">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="1ec4b-186">`EFConfigSource` Metody rozszerzenia umożliwiające dodawanie źródło konfiguracji można użyć:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-186">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="1ec4b-187">Poniższy kod przedstawia sposób użycia niestandardowego `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-187">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="1ec4b-188">Uwaga próbki dodaje niestandardowego `EFConfigProvider` po dostawcy JSON tak wszystkie ustawienia z bazy danych spowoduje zastąpienie ustawień z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-188">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="1ec4b-189">Użycie następujących *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-189">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="1ec4b-190">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-190">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="1ec4b-191">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1ec4b-191">CommandLine configuration provider</span></span>

<span data-ttu-id="1ec4b-192">[Dostawcę konfiguracji CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) odbiera pary klucz wartość argumentu wiersza polecenia dla konfiguracji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-192">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="1ec4b-193">Wyświetlanie lub pobieranie przykładowej konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1ec4b-193">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="1ec4b-194">Skonfigurować i stosować dostawcę konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1ec4b-194">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="1ec4b-195">Konfiguracja podstawowa</span><span class="sxs-lookup"><span data-stu-id="1ec4b-195">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="1ec4b-196">Aby aktywować konfigurację wiersza polecenia, należy wywołać `AddCommandLine` — metoda rozszerzenia w wystąpieniu [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="1ec4b-196">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="1ec4b-197">Wykonywanie kodu, wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-197">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="1ec4b-198">Przekazywanie argumentów pary klucz wartość w wierszu polecenia zmienia wartości `Profile:MachineName` i `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-198">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="1ec4b-199">Wyświetla okno konsoli:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-199">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="1ec4b-200">Aby zastąpić konfiguracji dostarczanych przez innych dostawców konfiguracji z wiersza polecenia konfiguracji, należy wywołać `AddCommandLine` ostatniego na `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-200">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ec4b-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1ec4b-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1ec4b-202">Typowe aplikacje 2.x platformy ASP.NET Core należy użyć metody statycznej wygody `CreateDefaultBuilder` tworzenie hosta:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-202">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="1ec4b-203">`CreateDefaultBuilder`ładuje konfiguracji opcjonalnej z *appsettings.json*, *appsettings. { Środowisko} JSON*, [kluczy tajnych użytkownika](xref:security/app-secrets) (w `Development` środowiska), zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-203">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="1ec4b-204">Dostawca konfiguracji wiersza polecenia nazywa się ostatnio.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-204">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="1ec4b-205">Ostatnio wywoływania dostawcy umożliwia wcześniej o nazwie argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-205">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="1ec4b-206">Aby uzyskać *appsettings* pliki gdzie:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-206">For *appsettings* files where:</span></span>

* <span data-ttu-id="1ec4b-207">`reloadOnChange`jest włączone.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-207">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="1ec4b-208">Zawiera tego samego ustawienia w argumentach wiersza polecenia i *appsettings* pliku.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-208">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="1ec4b-209">*Appsettings* plik zawierający pasujący argument wiersza polecenia zostaną zmienione po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-209">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="1ec4b-210">Jeśli wszystkie powyższe warunki są spełnione, argumenty wiersza polecenia są zastępowane.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-210">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="1ec4b-211">Można użyć aplikacji 2.x platformy ASP.NET Core [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) zamiast `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-211">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1ec4b-212">Korzystając z `WebHostBuilder`, ręcznie Konfiguracja zestawu z [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-212">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="1ec4b-213">Zobacz kartę 1.x platformy ASP.NET Core, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-213">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ec4b-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1ec4b-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1ec4b-215">Utwórz [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) i Wywołaj `AddCommandLine` — metoda korzysta z wiersza polecenia dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-215">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="1ec4b-216">Ostatnio wywoływania dostawcy umożliwia wcześniej o nazwie argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-216">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="1ec4b-217">Zastosuj konfigurację do [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) z `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-217">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="1ec4b-218">Argumenty</span><span class="sxs-lookup"><span data-stu-id="1ec4b-218">Arguments</span></span>

<span data-ttu-id="1ec4b-219">Argumenty wiersza polecenia musi być zgodna z jednym z dwóch formatów pokazano w poniższej tabeli:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-219">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="1ec4b-220">Format argumentu</span><span class="sxs-lookup"><span data-stu-id="1ec4b-220">Argument format</span></span>                                                     | <span data-ttu-id="1ec4b-221">Przykład</span><span class="sxs-lookup"><span data-stu-id="1ec4b-221">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="1ec4b-222">Pojedynczy argument: pary klucz wartość oddzielone znakiem równości (`=`)</span><span class="sxs-lookup"><span data-stu-id="1ec4b-222">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="1ec4b-223">Sekwencja dwa argumenty: pary klucz wartość oddzielone spacją</span><span class="sxs-lookup"><span data-stu-id="1ec4b-223">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="1ec4b-224">**Jeden argument**</span><span class="sxs-lookup"><span data-stu-id="1ec4b-224">**Single argument**</span></span>

<span data-ttu-id="1ec4b-225">Wartość musi występować po znaku równości (`=`).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-225">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="1ec4b-226">Wartość może mieć wartości null (na przykład `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-226">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="1ec4b-227">Klucz może mieć prefiksu.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-227">The key may have a prefix.</span></span>

| <span data-ttu-id="1ec4b-228">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="1ec4b-228">Key prefix</span></span>               | <span data-ttu-id="1ec4b-229">Przykład</span><span class="sxs-lookup"><span data-stu-id="1ec4b-229">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="1ec4b-230">Bez prefiksu</span><span class="sxs-lookup"><span data-stu-id="1ec4b-230">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="1ec4b-231">Pojedynczy dash (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="1ec4b-231">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="1ec4b-232">Dwa łączniki (`--`)</span><span class="sxs-lookup"><span data-stu-id="1ec4b-232">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="1ec4b-233">Ukośnika (`/`)</span><span class="sxs-lookup"><span data-stu-id="1ec4b-233">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="1ec4b-234">&#8224; Klucz z prefiksem pojedynczego dash (`-`) musi być wprowadzona w [przełącznika mapowania](#switch-mappings), które zostały opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-234">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="1ec4b-235">Przykładowe polecenie:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-235">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="1ec4b-236">Uwaga: Jeśli `-key1` nie jest obecny w [przełącznika mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-236">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="1ec4b-237">**Sekwencja dwa argumenty**</span><span class="sxs-lookup"><span data-stu-id="1ec4b-237">**Sequence of two arguments**</span></span>

<span data-ttu-id="1ec4b-238">Wartość nie może być pusta i musi występować po klucz rozdzielonych spacją.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-238">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="1ec4b-239">Klucz musi mieć prefiks.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-239">The key must have a prefix.</span></span>

| <span data-ttu-id="1ec4b-240">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="1ec4b-240">Key prefix</span></span>               | <span data-ttu-id="1ec4b-241">Przykład</span><span class="sxs-lookup"><span data-stu-id="1ec4b-241">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="1ec4b-242">Pojedynczy dash (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="1ec4b-242">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="1ec4b-243">Dwa łączniki (`--`)</span><span class="sxs-lookup"><span data-stu-id="1ec4b-243">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="1ec4b-244">Ukośnika (`/`)</span><span class="sxs-lookup"><span data-stu-id="1ec4b-244">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="1ec4b-245">&#8224; Klucz z prefiksem pojedynczego dash (`-`) musi być wprowadzona w [przełącznika mapowania](#switch-mappings), które zostały opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-245">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="1ec4b-246">Przykładowe polecenie:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-246">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="1ec4b-247">Uwaga: Jeśli `-key1` nie jest obecny w [przełącznika mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-247">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="1ec4b-248">Zduplikowane klucze</span><span class="sxs-lookup"><span data-stu-id="1ec4b-248">Duplicate keys</span></span>

<span data-ttu-id="1ec4b-249">Jeśli zduplikowane klucze są dostarczane, używana jest ostatnią parę klucz wartość.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-249">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="1ec4b-250">Mapowania przełącznika</span><span class="sxs-lookup"><span data-stu-id="1ec4b-250">Switch mappings</span></span>

<span data-ttu-id="1ec4b-251">Podczas ręcznego tworzenia konfiguracji o `ConfigurationBuilder`, słownik mapowań przełącznika mogą być dodawane do `AddCommandLine` metody.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-251">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="1ec4b-252">Mapowanie przełącznika umożliwić nazwę klucza wymiany logiki.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-252">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="1ec4b-253">W przypadku przełącznika słownik mapowań słownik jest sprawdzany pod kątem klucz pasujący do klucza dostarczonego przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-253">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="1ec4b-254">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) jest przekazywane z powrotem do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-254">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="1ec4b-255">Mapowanie przełącznika jest wymagane dla dowolnego klucz wiersza polecenia i jest poprzedzony prefiksem jeden pulpit (`-`).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-255">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="1ec4b-256">Przełącz zasady klucza słownika mapowania:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-256">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="1ec4b-257">Przełączniki musi rozpoczynać się kreską (`-`) lub kreska o podwójnej precyzji (`--`).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-257">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="1ec4b-258">Słownik mapowań przełącznik nie może zawierać zduplikowanych kluczy.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-258">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="1ec4b-259">W poniższym przykładzie `GetSwitchMappings` metoda pozwala argumenty wiersza polecenia użyć jednego dash (`-`) a uniknąć początkowe prefiksy podkluczy dla klucza prefiks.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-259">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="1ec4b-260">Bez konieczności podawania argumenty wiersza polecenia, podany słownik `AddInMemoryCollection` ustawia wartości konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-260">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="1ec4b-261">Uruchom aplikację za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-261">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="1ec4b-262">Wyświetla okno konsoli:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-262">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="1ec4b-263">Aby przekazać w ustawieniach konfiguracji, skorzystaj z następujących:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-263">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="1ec4b-264">Wyświetla okno konsoli:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-264">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="1ec4b-265">Po utworzeniu przełącznika słownik mapowań zawiera dane wyświetlane w poniższej tabeli:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-265">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="1ec4b-266">Key</span><span class="sxs-lookup"><span data-stu-id="1ec4b-266">Key</span></span>            | <span data-ttu-id="1ec4b-267">Wartość</span><span class="sxs-lookup"><span data-stu-id="1ec4b-267">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="1ec4b-268">Aby zademonstrować przełączanie klucza za pomocą słownika, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-268">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="1ec4b-269">Klucze wiersza polecenia są zamienione.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-269">The command-line keys are swapped.</span></span> <span data-ttu-id="1ec4b-270">W oknie konsoli wyświetla wartości konfiguracji `Profile:MachineName` i `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-270">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="1ec4b-271">Plik web.config</span><span class="sxs-lookup"><span data-stu-id="1ec4b-271">The web.config file</span></span>

<span data-ttu-id="1ec4b-272">A *web.config* hosting aplikacji w usługach IIS lub usług IIS Express jest wymagany plik.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-272">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="1ec4b-273">Ustawienia w *web.config* włączyć [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) do uruchomienia aplikacji i skonfigurować inne ustawienia usług IIS i modułów.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-273">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="1ec4b-274">Jeśli *web.config* plik nie jest obecny i plik projektu zawiera `<Project Sdk="Microsoft.NET.Sdk.Web">`, publikowania projektu tworzy *web.config* plików publikowanych danych wyjściowych ( *publikowania* folderu).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-274">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="1ec4b-275">Aby uzyskać więcej informacji, zobacz [hosta platformy ASP.NET Core w systemie Windows z programem IIS](xref:host-and-deploy/iis/index#webconfig).</span><span class="sxs-lookup"><span data-stu-id="1ec4b-275">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="1ec4b-276">Uzyskiwanie dostępu do konfiguracji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="1ec4b-276">Accessing configuration during startup</span></span>

<span data-ttu-id="1ec4b-277">Do konfiguracji dostępu w ramach `ConfigureServices` lub `Configure` podczas uruchamiania, zobacz przykłady w [uruchamiania aplikacji](xref:fundamentals/startup) tematu.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-277">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="1ec4b-278">Dodatkowe uwagi</span><span class="sxs-lookup"><span data-stu-id="1ec4b-278">Additional notes</span></span>

* <span data-ttu-id="1ec4b-279">Zależności Iniekcja nie został skonfigurowany do po `ConfigureServices` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-279">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="1ec4b-280">System konfiguracji nie jest świadome Podpisane.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-280">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="1ec4b-281">`IConfiguration`ma dwa specjalizacje:</span><span class="sxs-lookup"><span data-stu-id="1ec4b-281">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="1ec4b-282">`IConfigurationRoot`Użyty dla węzła głównego.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-282">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="1ec4b-283">Może spowodować ponowne załadowanie.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-283">Can trigger a reload.</span></span>
  * <span data-ttu-id="1ec4b-284">`IConfigurationSection`Reprezentuje sekcję konfiguracji wartości.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-284">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="1ec4b-285">`GetSection` i `GetChildren` metody zwracają `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-285">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="1ec4b-286">Użyj [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) podczas ponownego ładowania konfiguracji lub dostęp do każdego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-286">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="1ec4b-287">Żadna z tych sytuacji są często używane.</span><span class="sxs-lookup"><span data-stu-id="1ec4b-287">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ec4b-288">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1ec4b-288">Additional resources</span></span>

* [<span data-ttu-id="1ec4b-289">Opcje</span><span class="sxs-lookup"><span data-stu-id="1ec4b-289">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="1ec4b-290">Praca w środowiskach wielu</span><span class="sxs-lookup"><span data-stu-id="1ec4b-290">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="1ec4b-291">Bezpieczne przechowywanie kluczy tajnych aplikacji w czasie projektowania</span><span class="sxs-lookup"><span data-stu-id="1ec4b-291">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="1ec4b-292">Hosting w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ec4b-292">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="1ec4b-293">Iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="1ec4b-293">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="1ec4b-294">Dostawca konfiguracji usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1ec4b-294">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
