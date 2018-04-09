---
title: Konfiguracja platformy ASP.NET Core
author: rick-anderson
description: Interfejs API konfiguracji umożliwia konfigurowanie aplikacji platformy ASP.NET Core za pomocą wielu metod.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: f272f9629ab1f9e7f7643cafd0d45f19340d5284
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="92ea5-103">Konfiguracja platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92ea5-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="92ea5-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Michaelis znak](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Roth Danielowi](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="92ea5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="92ea5-105">Interfejs API konfiguracji umożliwia konfigurowanie platformy ASP.NET Core aplikacji sieci web na podstawie listy par nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="92ea5-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="92ea5-106">Konfiguracja jest do odczytu w czasie wykonywania z wielu źródeł.</span><span class="sxs-lookup"><span data-stu-id="92ea5-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="92ea5-107">Pary nazwa wartość można grupować w wielopoziomowej hierarchii.</span><span class="sxs-lookup"><span data-stu-id="92ea5-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="92ea5-108">Brak dostawcy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="92ea5-108">There are configuration providers for:</span></span>

* <span data-ttu-id="92ea5-109">Format pliku (INI, JSON i XML).</span><span class="sxs-lookup"><span data-stu-id="92ea5-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="92ea5-110">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="92ea5-110">Command-line arguments.</span></span>
* <span data-ttu-id="92ea5-111">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="92ea5-111">Environment variables.</span></span>
* <span data-ttu-id="92ea5-112">Obiekty .NET w pamięci.</span><span class="sxs-lookup"><span data-stu-id="92ea5-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="92ea5-113">Niezaszyfrowane [Manager klucz tajny](xref:security/app-secrets) magazynu.</span><span class="sxs-lookup"><span data-stu-id="92ea5-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="92ea5-114">Przechowywać zaszyfrowane użytkownika, takich jak [usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="92ea5-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="92ea5-115">Dostawców niestandardowych (zainstalowane lub utworzone).</span><span class="sxs-lookup"><span data-stu-id="92ea5-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="92ea5-116">Każda wartość konfiguracji jest mapowany na klucz ciągu.</span><span class="sxs-lookup"><span data-stu-id="92ea5-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="92ea5-117">Brak obsługi wbudowanych powiązanie zdeserializować ustawień do niestandardowego [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) obiektu (.NET klasą prostą z właściwościami).</span><span class="sxs-lookup"><span data-stu-id="92ea5-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="92ea5-118">Wzorzec opcje używa klas opcji do reprezentowania grupy ustawień.</span><span class="sxs-lookup"><span data-stu-id="92ea5-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="92ea5-119">Aby uzyskać więcej informacji o wykorzystaniu wzorca opcji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.</span><span class="sxs-lookup"><span data-stu-id="92ea5-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="92ea5-120">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="92ea5-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="92ea5-121">JSON konfiguracji</span><span class="sxs-lookup"><span data-stu-id="92ea5-121">JSON configuration</span></span>

<span data-ttu-id="92ea5-122">Następujące aplikacji konsoli używa dostawcy konfiguracji JSON:</span><span class="sxs-lookup"><span data-stu-id="92ea5-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="92ea5-123">Odczytuje i wyświetla następujące ustawienia konfiguracji aplikacji:</span><span class="sxs-lookup"><span data-stu-id="92ea5-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="92ea5-124">Konfiguracja obejmuje hierarchiczną listę par nazwa wartość, w których węzły są oddzielone dwukropkiem (`:`).</span><span class="sxs-lookup"><span data-stu-id="92ea5-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon (`:`).</span></span> <span data-ttu-id="92ea5-125">Aby pobrać wartość, należy pobrać `Configuration` indeksatora kluczem odpowiedniego elementu:</span><span class="sxs-lookup"><span data-stu-id="92ea5-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="92ea5-126">Aby pracować z tablic w formacie JSON konfiguracji źródła, należy użyć indeks tablicy jako część ciągu oddzielone dwukropkiem.</span><span class="sxs-lookup"><span data-stu-id="92ea5-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="92ea5-127">Poniższy przykład pobiera nazwę pierwszego elementu w poprzednim `wizards` tablicy:</span><span class="sxs-lookup"><span data-stu-id="92ea5-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="92ea5-128">Pary nazwa wartość zapisywane do wbudowanej [konfiguracji](/dotnet/api/microsoft.extensions.configuration) dostawcy są **nie** utrwalone.</span><span class="sxs-lookup"><span data-stu-id="92ea5-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="92ea5-129">Jednak można utworzyć niestandardowego dostawcę, który zapisuje wartości.</span><span class="sxs-lookup"><span data-stu-id="92ea5-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="92ea5-130">Zobacz [niestandardowego dostawcy konfiguracji](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="92ea5-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="92ea5-131">Powyższego przykładu używa indeksatora konfiguracji można odczytać wartości.</span><span class="sxs-lookup"><span data-stu-id="92ea5-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="92ea5-132">Do konfiguracji dostępu poza `Startup`, użyj *wzorzec opcje*.</span><span class="sxs-lookup"><span data-stu-id="92ea5-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="92ea5-133">Aby uzyskać więcej informacji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.</span><span class="sxs-lookup"><span data-stu-id="92ea5-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="92ea5-134">Konfiguracja XML</span><span class="sxs-lookup"><span data-stu-id="92ea5-134">XML configuration</span></span>

<span data-ttu-id="92ea5-135">Aby pracować z tablic w formacie XML konfiguracji źródła, należy podać `name` indeksu do każdego elementu.</span><span class="sxs-lookup"><span data-stu-id="92ea5-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="92ea5-136">Użyj indeksu, aby uzyskać dostęp do wartości:</span><span class="sxs-lookup"><span data-stu-id="92ea5-136">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="92ea5-137">Konfiguracja przez środowisko</span><span class="sxs-lookup"><span data-stu-id="92ea5-137">Configuration by environment</span></span>

<span data-ttu-id="92ea5-138">Jest to typowe do innej konfiguracji ustawień dla różnych środowisk, na przykład programowania, testowania i produkcji.</span><span class="sxs-lookup"><span data-stu-id="92ea5-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="92ea5-139">`CreateDefaultBuilder` — Metoda rozszerzenia w aplikacji platformy ASP.NET Core 2.x (lub przy użyciu `AddJsonFile` i `AddEnvironmentVariables` bezpośrednio w aplikacji platformy ASP.NET Core 1.x) dodaje dostawców konfiguracji do odczytywania źródeł konfiguracji systemu i pliki w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="92ea5-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="92ea5-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="92ea5-140">*appsettings.json*</span></span>
* <span data-ttu-id="92ea5-141">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="92ea5-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="92ea5-142">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="92ea5-142">Environment variables</span></span>

<span data-ttu-id="92ea5-143">Aplikacje 1.x platformy ASP.NET Core musi wywołać `AddJsonFile` i [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="92ea5-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="92ea5-144">Zobacz [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) dla objaśnienie parametrów.</span><span class="sxs-lookup"><span data-stu-id="92ea5-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="92ea5-145">`reloadOnChange` jest obsługiwana tylko w ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="92ea5-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="92ea5-146">Konfiguracja źródła są do odczytu w kolejności, że jest określona.</span><span class="sxs-lookup"><span data-stu-id="92ea5-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="92ea5-147">W powyższym kodzie zmienne środowiskowe są odczytywane ostatnio.</span><span class="sxs-lookup"><span data-stu-id="92ea5-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="92ea5-148">Wartości konfiguracji ustawiana za pośrednictwem Zamień środowiska określone w poprzednich dwóch dostawców.</span><span class="sxs-lookup"><span data-stu-id="92ea5-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="92ea5-149">Należy rozważyć *appsettings. Staging.JSON* pliku:</span><span class="sxs-lookup"><span data-stu-id="92ea5-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="92ea5-150">Jeśli środowisko jest równa `Staging`, następujące `Configure` metoda odczytuje wartość `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="92ea5-150">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

<span data-ttu-id="92ea5-151">Środowisko jest zwykle ustawiana na `Development`, `Staging`, lub `Production`.</span><span class="sxs-lookup"><span data-stu-id="92ea5-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="92ea5-152">Aby uzyskać więcej informacji, zobacz [pracy w środowiskach wielu](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="92ea5-152">For more information, see [Work with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="92ea5-153">Zagadnienia dotyczące konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="92ea5-153">Configuration considerations:</span></span>

* <span data-ttu-id="92ea5-154">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) można ponownie załadować dane konfiguracji, gdy zmienia.</span><span class="sxs-lookup"><span data-stu-id="92ea5-154">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) can reload configuration data when it changes.</span></span>
* <span data-ttu-id="92ea5-155">Klucze konfiguracji **nie** z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="92ea5-155">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="92ea5-156">**Nigdy nie** przechowywania haseł i innych poufnych danych w konfiguracji dostawcy kodu lub pliki konfiguracyjne w formacie zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="92ea5-156">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="92ea5-157">Nie używasz produkcji kluczy tajnych w rozwoju lub testowania środowisk.</span><span class="sxs-lookup"><span data-stu-id="92ea5-157">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="92ea5-158">Określ klucze tajne poza projektem, aby nie może być przypadkowo przekazane do repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="92ea5-158">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="92ea5-159">Dowiedz się więcej o [pracy w środowiskach wielu](xref:fundamentals/environments) i zarządzanie [bezpiecznego magazynu kluczy tajnych aplikacji do rozwoju](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="92ea5-159">Learn more about [how to work with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets in development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="92ea5-160">Dla wartości hierarchiczna konfiguracji określonych w zmiennych środowiskowych, dwukropek (`:`) może nie działać na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="92ea5-160">For hierarchical config values specified in environment variables, a colon (`:`) may not work on all platforms.</span></span> <span data-ttu-id="92ea5-161">Podwójne podkreślenia (`__`) jest obsługiwana przez wszystkie platformy.</span><span class="sxs-lookup"><span data-stu-id="92ea5-161">Double underscore (`__`) is supported by all platforms.</span></span>
* <span data-ttu-id="92ea5-162">Gdy korzysta z konfiguracji interfejsu API, dwukropek (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="92ea5-162">When interacting with the configuration API, a colon (`:`) works on all platforms.</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="92ea5-163">Dostawca w pamięci i powiązanie z klasą POCO</span><span class="sxs-lookup"><span data-stu-id="92ea5-163">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="92ea5-164">Poniższy przykład przedstawia sposób korzystają z dostawcy w pamięci i powiąż do klasy:</span><span class="sxs-lookup"><span data-stu-id="92ea5-164">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="92ea5-165">Wartości konfiguracji są zwracane jako ciągi, ale powiązanie umożliwia konstrukcji obiektów.</span><span class="sxs-lookup"><span data-stu-id="92ea5-165">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="92ea5-166">Powiązanie umożliwia pobieranie obiektów POCO lub wykresy nawet całego obiektu.</span><span class="sxs-lookup"><span data-stu-id="92ea5-166">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="92ea5-167">GetValue</span><span class="sxs-lookup"><span data-stu-id="92ea5-167">GetValue</span></span>

<span data-ttu-id="92ea5-168">W poniższym przykładzie pokazano [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="92ea5-168">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="92ea5-169">ConfigurationBinder `GetValue<T>` metody umożliwia określenie wartości domyślnej (80 w próbce).</span><span class="sxs-lookup"><span data-stu-id="92ea5-169">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="92ea5-170">`GetValue<T>` Służy do scenariuszy proste i nie należy powiązać całą sekcję.</span><span class="sxs-lookup"><span data-stu-id="92ea5-170">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="92ea5-171">`GetValue<T>` pobiera wartości skalarnych z `GetSection(key).Value` przekonwertować dla określonego typu.</span><span class="sxs-lookup"><span data-stu-id="92ea5-171">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="92ea5-172">Powiązanie do obiektu wykresu.</span><span class="sxs-lookup"><span data-stu-id="92ea5-172">Bind to an object graph</span></span>

<span data-ttu-id="92ea5-173">Każdy obiekt w klasie można rekursywnie powiązany.</span><span class="sxs-lookup"><span data-stu-id="92ea5-173">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="92ea5-174">Należy rozważyć `AppSettings` klasy:</span><span class="sxs-lookup"><span data-stu-id="92ea5-174">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="92ea5-175">Poniższy przykład tworzy powiązanie z `AppSettings` klasy:</span><span class="sxs-lookup"><span data-stu-id="92ea5-175">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="92ea5-176">**Platformy ASP.NET Core 1.1** i wyższe służy `Get<T>`, który współpracuje z całą sekcję.</span><span class="sxs-lookup"><span data-stu-id="92ea5-176">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="92ea5-177">`Get<T>` może być wygodniejsze niż przy użyciu `Bind`.</span><span class="sxs-lookup"><span data-stu-id="92ea5-177">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="92ea5-178">Poniższy kod przedstawia sposób użycia `Get<T>` z powyższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="92ea5-178">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="92ea5-179">Użycie następujących *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="92ea5-179">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="92ea5-180">Wyświetla program `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="92ea5-180">The program displays `Height 11`.</span></span>

<span data-ttu-id="92ea5-181">Poniższy kod może służyć do jednostki jest przetestowanie konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="92ea5-181">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="92ea5-182">Tworzenie niestandardowego dostawcy programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="92ea5-182">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="92ea5-183">W tej sekcji tworzony jest odczytujący pary nazwa wartość z bazy danych przy użyciu EF dostawca konfiguracji podstawowej.</span><span class="sxs-lookup"><span data-stu-id="92ea5-183">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="92ea5-184">Zdefiniuj `ConfigurationValue` jednostki do przechowywania wartości konfiguracji w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="92ea5-184">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="92ea5-185">Dodaj `ConfigurationContext` do przechowywania i uzyskać dostęp do skonfigurowanego wartości:</span><span class="sxs-lookup"><span data-stu-id="92ea5-185">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="92ea5-186">Utwórz klasę, która implementuje [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="92ea5-186">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="92ea5-187">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="92ea5-187">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="92ea5-188">Dostawca konfiguracji inicjuje bazy danych, gdy nie jest pusty:</span><span class="sxs-lookup"><span data-stu-id="92ea5-188">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="92ea5-189">Wyróżnione wartości z bazy danych ("value_from_ef_1" i "value_from_ef_2") są wyświetlane po uruchomieniu próbki.</span><span class="sxs-lookup"><span data-stu-id="92ea5-189">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="92ea5-190">`EFConfigSource` Metody rozszerzenia umożliwiające dodawanie źródło konfiguracji można użyć:</span><span class="sxs-lookup"><span data-stu-id="92ea5-190">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="92ea5-191">Poniższy kod przedstawia sposób użycia niestandardowego `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="92ea5-191">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="92ea5-192">Uwaga próbki dodaje niestandardowego `EFConfigProvider` po dostawcy JSON tak wszystkie ustawienia z bazy danych spowoduje zastąpienie ustawień z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="92ea5-192">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="92ea5-193">Użycie następujących *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="92ea5-193">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="92ea5-194">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="92ea5-194">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="92ea5-195">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="92ea5-195">CommandLine configuration provider</span></span>

<span data-ttu-id="92ea5-196">[Dostawcę konfiguracji CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) odbiera pary klucz wartość argumentu wiersza polecenia dla konfiguracji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="92ea5-196">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="92ea5-197">Wyświetlanie lub pobieranie przykładowej konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="92ea5-197">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="92ea5-198">Skonfigurować i stosować dostawcę konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="92ea5-198">Setup and use the CommandLine configuration provider</span></span>

#### <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="92ea5-199">Konfiguracja podstawowa</span><span class="sxs-lookup"><span data-stu-id="92ea5-199">Basic Configuration</span></span>](#tab/basicconfiguration/)
<span data-ttu-id="92ea5-200">Aby aktywować konfigurację wiersza polecenia, należy wywołać `AddCommandLine` — metoda rozszerzenia w wystąpieniu [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="92ea5-200">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="92ea5-201">Wykonywanie kodu, wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="92ea5-201">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="92ea5-202">Przekazywanie argumentów pary klucz wartość w wierszu polecenia zmienia wartości `Profile:MachineName` i `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="92ea5-202">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="92ea5-203">Wyświetla okno konsoli:</span><span class="sxs-lookup"><span data-stu-id="92ea5-203">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="92ea5-204">Aby zastąpić konfiguracji dostarczanych przez innych dostawców konfiguracji z wiersza polecenia konfiguracji, należy wywołać `AddCommandLine` ostatniego na `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="92ea5-204">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92ea5-205">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92ea5-205">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="92ea5-206">Typowe aplikacje 2.x platformy ASP.NET Core należy użyć metody statycznej wygody `CreateDefaultBuilder` tworzenie hosta:</span><span class="sxs-lookup"><span data-stu-id="92ea5-206">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="92ea5-207">`CreateDefaultBuilder` ładuje konfiguracji opcjonalnej z *appsettings.json*, *appsettings. { Środowisko} JSON*, [kluczy tajnych użytkownika](xref:security/app-secrets) (w `Development` środowiska), zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="92ea5-207">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="92ea5-208">Dostawca konfiguracji wiersza polecenia nazywa się ostatnio.</span><span class="sxs-lookup"><span data-stu-id="92ea5-208">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="92ea5-209">Ostatnio wywoływania dostawcy umożliwia wcześniej o nazwie argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="92ea5-209">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="92ea5-210">Aby uzyskać *appsettings* pliki gdzie:</span><span class="sxs-lookup"><span data-stu-id="92ea5-210">For *appsettings* files where:</span></span>

* <span data-ttu-id="92ea5-211">`reloadOnChange` jest włączone.</span><span class="sxs-lookup"><span data-stu-id="92ea5-211">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="92ea5-212">Zawiera tego samego ustawienia w argumentach wiersza polecenia i *appsettings* pliku.</span><span class="sxs-lookup"><span data-stu-id="92ea5-212">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="92ea5-213">*Appsettings* plik zawierający pasujący argument wiersza polecenia zostaną zmienione po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92ea5-213">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="92ea5-214">Jeśli wszystkie powyższe warunki są spełnione, argumenty wiersza polecenia są zastępowane.</span><span class="sxs-lookup"><span data-stu-id="92ea5-214">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="92ea5-215">Można użyć aplikacji 2.x platformy ASP.NET Core [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) zamiast `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="92ea5-215">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="92ea5-216">Korzystając z `WebHostBuilder`, ręcznie Konfiguracja zestawu z [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="92ea5-216">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="92ea5-217">Zobacz kartę 1.x platformy ASP.NET Core, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="92ea5-217">See the ASP.NET Core 1.x tab for more information.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92ea5-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92ea5-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="92ea5-219">Utwórz [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) i Wywołaj `AddCommandLine` — metoda korzysta z wiersza polecenia dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="92ea5-219">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="92ea5-220">Ostatnio wywoływania dostawcy umożliwia wcześniej o nazwie argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="92ea5-220">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="92ea5-221">Zastosuj konfigurację do [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) z `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="92ea5-221">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

* * *
### <a name="arguments"></a><span data-ttu-id="92ea5-222">Argumenty</span><span class="sxs-lookup"><span data-stu-id="92ea5-222">Arguments</span></span>

<span data-ttu-id="92ea5-223">Argumenty wiersza polecenia musi być zgodna z jednym z dwóch formatów pokazano w poniższej tabeli:</span><span class="sxs-lookup"><span data-stu-id="92ea5-223">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="92ea5-224">Format argumentu</span><span class="sxs-lookup"><span data-stu-id="92ea5-224">Argument format</span></span>                                                     | <span data-ttu-id="92ea5-225">Przykład</span><span class="sxs-lookup"><span data-stu-id="92ea5-225">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="92ea5-226">Pojedynczy argument: pary klucz wartość oddzielone znakiem równości (`=`)</span><span class="sxs-lookup"><span data-stu-id="92ea5-226">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="92ea5-227">Sekwencja dwa argumenty: pary klucz wartość oddzielone spacją</span><span class="sxs-lookup"><span data-stu-id="92ea5-227">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="92ea5-228">**Jeden argument**</span><span class="sxs-lookup"><span data-stu-id="92ea5-228">**Single argument**</span></span>

<span data-ttu-id="92ea5-229">Wartość musi występować po znaku równości (`=`).</span><span class="sxs-lookup"><span data-stu-id="92ea5-229">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="92ea5-230">Wartość może mieć wartości null (na przykład `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="92ea5-230">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="92ea5-231">Klucz może mieć prefiksu.</span><span class="sxs-lookup"><span data-stu-id="92ea5-231">The key may have a prefix.</span></span>

| <span data-ttu-id="92ea5-232">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="92ea5-232">Key prefix</span></span>               | <span data-ttu-id="92ea5-233">Przykład</span><span class="sxs-lookup"><span data-stu-id="92ea5-233">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="92ea5-234">Bez prefiksu</span><span class="sxs-lookup"><span data-stu-id="92ea5-234">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="92ea5-235">Pojedynczy dash (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="92ea5-235">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="92ea5-236">Dwa łączniki (`--`)</span><span class="sxs-lookup"><span data-stu-id="92ea5-236">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="92ea5-237">Ukośnika (`/`)</span><span class="sxs-lookup"><span data-stu-id="92ea5-237">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="92ea5-238">&#8224;Klucz z prefiksem pojedynczego dash (`-`) musi być wprowadzona w [przełącznika mapowania](#switch-mappings), które zostały opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="92ea5-238">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="92ea5-239">Przykładowe polecenie:</span><span class="sxs-lookup"><span data-stu-id="92ea5-239">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="92ea5-240">Uwaga: Jeśli `-key1` nie jest obecny w [przełącznika mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="92ea5-240">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="92ea5-241">**Sekwencja dwa argumenty**</span><span class="sxs-lookup"><span data-stu-id="92ea5-241">**Sequence of two arguments**</span></span>

<span data-ttu-id="92ea5-242">Wartość nie może być pusta i musi występować po klucz rozdzielonych spacją.</span><span class="sxs-lookup"><span data-stu-id="92ea5-242">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="92ea5-243">Klucz musi mieć prefiks.</span><span class="sxs-lookup"><span data-stu-id="92ea5-243">The key must have a prefix.</span></span>

| <span data-ttu-id="92ea5-244">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="92ea5-244">Key prefix</span></span>               | <span data-ttu-id="92ea5-245">Przykład</span><span class="sxs-lookup"><span data-stu-id="92ea5-245">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="92ea5-246">Pojedynczy dash (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="92ea5-246">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="92ea5-247">Dwa łączniki (`--`)</span><span class="sxs-lookup"><span data-stu-id="92ea5-247">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="92ea5-248">Ukośnika (`/`)</span><span class="sxs-lookup"><span data-stu-id="92ea5-248">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="92ea5-249">&#8224;Klucz z prefiksem pojedynczego dash (`-`) musi być wprowadzona w [przełącznika mapowania](#switch-mappings), które zostały opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="92ea5-249">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="92ea5-250">Przykładowe polecenie:</span><span class="sxs-lookup"><span data-stu-id="92ea5-250">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="92ea5-251">Uwaga: Jeśli `-key1` nie jest obecny w [przełącznika mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="92ea5-251">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="92ea5-252">Zduplikowane klucze</span><span class="sxs-lookup"><span data-stu-id="92ea5-252">Duplicate keys</span></span>

<span data-ttu-id="92ea5-253">Jeśli zduplikowane klucze są dostarczane, używana jest ostatnią parę klucz wartość.</span><span class="sxs-lookup"><span data-stu-id="92ea5-253">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="92ea5-254">Mapowania przełącznika</span><span class="sxs-lookup"><span data-stu-id="92ea5-254">Switch mappings</span></span>

<span data-ttu-id="92ea5-255">Podczas ręcznego tworzenia konfiguracji o `ConfigurationBuilder`, słownik mapowań przełącznika mogą być dodawane do `AddCommandLine` metody.</span><span class="sxs-lookup"><span data-stu-id="92ea5-255">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="92ea5-256">Mapowanie przełącznika umożliwić nazwę klucza wymiany logiki.</span><span class="sxs-lookup"><span data-stu-id="92ea5-256">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="92ea5-257">W przypadku przełącznika słownik mapowań słownik jest sprawdzany pod kątem klucz pasujący do klucza dostarczonego przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="92ea5-257">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="92ea5-258">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) jest przekazywane z powrotem do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="92ea5-258">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="92ea5-259">Mapowanie przełącznika jest wymagane dla dowolnego klucz wiersza polecenia i jest poprzedzony prefiksem jeden pulpit (`-`).</span><span class="sxs-lookup"><span data-stu-id="92ea5-259">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="92ea5-260">Przełącz zasady klucza słownika mapowania:</span><span class="sxs-lookup"><span data-stu-id="92ea5-260">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="92ea5-261">Przełączniki musi rozpoczynać się kreską (`-`) lub kreska o podwójnej precyzji (`--`).</span><span class="sxs-lookup"><span data-stu-id="92ea5-261">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="92ea5-262">Słownik mapowań przełącznik nie może zawierać zduplikowanych kluczy.</span><span class="sxs-lookup"><span data-stu-id="92ea5-262">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="92ea5-263">W poniższym przykładzie `GetSwitchMappings` metoda pozwala argumenty wiersza polecenia użyć jednego dash (`-`) a uniknąć początkowe prefiksy podkluczy dla klucza prefiks.</span><span class="sxs-lookup"><span data-stu-id="92ea5-263">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="92ea5-264">Bez konieczności podawania argumenty wiersza polecenia, podany słownik `AddInMemoryCollection` ustawia wartości konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="92ea5-264">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="92ea5-265">Uruchom aplikację za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="92ea5-265">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="92ea5-266">Wyświetla okno konsoli:</span><span class="sxs-lookup"><span data-stu-id="92ea5-266">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="92ea5-267">Aby przekazać w ustawieniach konfiguracji, skorzystaj z następujących:</span><span class="sxs-lookup"><span data-stu-id="92ea5-267">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="92ea5-268">Wyświetla okno konsoli:</span><span class="sxs-lookup"><span data-stu-id="92ea5-268">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="92ea5-269">Po utworzeniu przełącznika słownik mapowań zawiera dane wyświetlane w poniższej tabeli:</span><span class="sxs-lookup"><span data-stu-id="92ea5-269">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="92ea5-270">Key</span><span class="sxs-lookup"><span data-stu-id="92ea5-270">Key</span></span>            | <span data-ttu-id="92ea5-271">Wartość</span><span class="sxs-lookup"><span data-stu-id="92ea5-271">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="92ea5-272">Aby zademonstrować przełączanie klucza za pomocą słownika, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="92ea5-272">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="92ea5-273">Klucze wiersza polecenia są zamienione.</span><span class="sxs-lookup"><span data-stu-id="92ea5-273">The command-line keys are swapped.</span></span> <span data-ttu-id="92ea5-274">W oknie konsoli wyświetla wartości konfiguracji `Profile:MachineName` i `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="92ea5-274">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="92ea5-275">plik Web.config</span><span class="sxs-lookup"><span data-stu-id="92ea5-275">web.config file</span></span>

<span data-ttu-id="92ea5-276">A *web.config* hosting aplikacji w usługach IIS lub usług IIS Express jest wymagany plik.</span><span class="sxs-lookup"><span data-stu-id="92ea5-276">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="92ea5-277">Ustawienia w *web.config* włączyć [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) do uruchomienia aplikacji i skonfigurować inne ustawienia usług IIS i modułów.</span><span class="sxs-lookup"><span data-stu-id="92ea5-277">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="92ea5-278">Jeśli *web.config* plik nie jest obecny i plik projektu zawiera `<Project Sdk="Microsoft.NET.Sdk.Web">`, publikowania projektu tworzy *web.config* plików publikowanych danych wyjściowych ( *publikowania* folderu).</span><span class="sxs-lookup"><span data-stu-id="92ea5-278">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="92ea5-279">Aby uzyskać więcej informacji, zobacz [hosta platformy ASP.NET Core w systemie Windows z programem IIS](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="92ea5-279">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="access-configuration-during-startup"></a><span data-ttu-id="92ea5-280">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="92ea5-280">Access configuration during startup</span></span>

<span data-ttu-id="92ea5-281">Do konfiguracji dostępu w ramach `ConfigureServices` lub `Configure` podczas uruchamiania, zobacz przykłady w [uruchamiania aplikacji](xref:fundamentals/startup) tematu.</span><span class="sxs-lookup"><span data-stu-id="92ea5-281">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a><span data-ttu-id="92ea5-282">Konfiguracja dostępu w widoku Razor strony lub MVC</span><span class="sxs-lookup"><span data-stu-id="92ea5-282">Access configuration in a Razor Page or MVC view</span></span>

<span data-ttu-id="92ea5-283">Aby uzyskać dostęp do ustawień konfiguracji na stronie aparatu Razor strony lub widok MVC, Dodaj [dyrektywa using](xref:mvc/views/razor#using) ([odwołanie w C#: dyrektywa using](/dotnet/csharp/language-reference/keywords/using-directive)) dla [Microsoft.Extensions.Configuration przestrzeni nazw ](/dotnet/api/microsoft.extensions.configuration) i wstrzyknąć [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) do strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="92ea5-283">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](/dotnet/api/microsoft.extensions.configuration) and inject [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) into the page or view.</span></span>

<span data-ttu-id="92ea5-284">Na stronie aparatu Razor strony:</span><span class="sxs-lookup"><span data-stu-id="92ea5-284">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="92ea5-285">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="92ea5-285">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a><span data-ttu-id="92ea5-286">Dodatkowe uwagi</span><span class="sxs-lookup"><span data-stu-id="92ea5-286">Additional notes</span></span>

* <span data-ttu-id="92ea5-287">Zależności Iniekcja nie został skonfigurowany do po `ConfigureServices` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="92ea5-287">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="92ea5-288">System konfiguracji nie jest świadome Podpisane.</span><span class="sxs-lookup"><span data-stu-id="92ea5-288">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="92ea5-289">`IConfiguration` ma dwa specjalizacje:</span><span class="sxs-lookup"><span data-stu-id="92ea5-289">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="92ea5-290">`IConfigurationRoot` Użyty dla węzła głównego.</span><span class="sxs-lookup"><span data-stu-id="92ea5-290">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="92ea5-291">Może spowodować ponowne załadowanie.</span><span class="sxs-lookup"><span data-stu-id="92ea5-291">Can trigger a reload.</span></span>
  * <span data-ttu-id="92ea5-292">`IConfigurationSection` Reprezentuje sekcję konfiguracji wartości.</span><span class="sxs-lookup"><span data-stu-id="92ea5-292">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="92ea5-293">`GetSection` i `GetChildren` metody zwracają `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="92ea5-293">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="92ea5-294">Użyj [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) podczas ponownego ładowania konfiguracji lub dostęp do każdego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="92ea5-294">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="92ea5-295">Żadna z tych sytuacji są często używane.</span><span class="sxs-lookup"><span data-stu-id="92ea5-295">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="92ea5-296">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="92ea5-296">Additional resources</span></span>

* [<span data-ttu-id="92ea5-297">Opcje</span><span class="sxs-lookup"><span data-stu-id="92ea5-297">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="92ea5-298">Praca w środowiskach wielu</span><span class="sxs-lookup"><span data-stu-id="92ea5-298">Work with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="92ea5-299">Bezpieczne przechowywanie klucze tajne aplikacji do rozwoju</span><span class="sxs-lookup"><span data-stu-id="92ea5-299">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="92ea5-300">Hosting w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92ea5-300">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="92ea5-301">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="92ea5-301">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="92ea5-302">Dostawca konfiguracji usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="92ea5-302">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
