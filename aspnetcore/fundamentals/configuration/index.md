---
title: Konfiguracja w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core za pomocą interfejsu API konfiguracji.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 59ab0cd0f6975d15bd01ce7e4128521938182c24
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228627"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="8e806-103">Konfiguracja w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e806-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="8e806-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Michaelis znacznik](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8e806-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8e806-105">Interfejs API konfiguracji umożliwia konfigurowanie platformy ASP.NET Core aplikacji sieci web, w oparciu o listę par nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="8e806-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="8e806-106">Konfiguracja jest do odczytu w czasie wykonywania z wielu źródeł.</span><span class="sxs-lookup"><span data-stu-id="8e806-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="8e806-107">Pary nazwa wartość, można podzielić na wiele poziomu hierarchii.</span><span class="sxs-lookup"><span data-stu-id="8e806-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="8e806-108">Istnieją dostawców konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e806-108">There are configuration providers for:</span></span>

* <span data-ttu-id="8e806-109">Formaty plików (INI, JSON i XML).</span><span class="sxs-lookup"><span data-stu-id="8e806-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="8e806-110">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8e806-110">Command-line arguments.</span></span>
* <span data-ttu-id="8e806-111">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="8e806-111">Environment variables.</span></span>
* <span data-ttu-id="8e806-112">Obiekty .NET w pamięci.</span><span class="sxs-lookup"><span data-stu-id="8e806-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="8e806-113">Niezaszyfrowane [Menedżera klucz tajny](xref:security/app-secrets) magazynu.</span><span class="sxs-lookup"><span data-stu-id="8e806-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="8e806-114">Użytkownik zaszyfrowanych przechowywanych informacji, takich jak [usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="8e806-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="8e806-115">Dostawcy niestandardowi (zainstalowane lub utworzone).</span><span class="sxs-lookup"><span data-stu-id="8e806-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="8e806-116">Każda wartość konfiguracji mapuje klucza typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="8e806-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="8e806-117">Obsługuje wbudowanych powiązania do deserializacji ustawienia niestandardowego [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) obiektu (.NET klasą prostą przy użyciu właściwości).</span><span class="sxs-lookup"><span data-stu-id="8e806-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="8e806-118">Wzorzec opcje używa klas opcji do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="8e806-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="8e806-119">Aby uzyskać więcej informacji na temat korzystania z wzorca opcji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.</span><span class="sxs-lookup"><span data-stu-id="8e806-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="8e806-120">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8e806-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8e806-121">Przykłady podane w tym temacie opierają się na:</span><span class="sxs-lookup"><span data-stu-id="8e806-121">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="8e806-122">Ustawianie ścieżki podstawowej aplikacji za pomocą [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="8e806-122">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="8e806-123">`SetBasePath` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="8e806-123">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="8e806-124">Rozpoznawanie sekcji plików konfiguracji przy użyciu [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="8e806-124">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="8e806-125">`GetSection` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="8e806-125">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="8e806-126">Konfiguracja powiązania z [powiązać](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="8e806-126">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="8e806-127">`Bind` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="8e806-127">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="8e806-128">Te pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8e806-128">These packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8e806-129">Przykłady podane w tym temacie opierają się na:</span><span class="sxs-lookup"><span data-stu-id="8e806-129">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="8e806-130">Ustawianie ścieżki podstawowej aplikacji za pomocą [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="8e806-130">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="8e806-131">`SetBasePath` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="8e806-131">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="8e806-132">Rozpoznawanie sekcji plików konfiguracji przy użyciu [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="8e806-132">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="8e806-133">`GetSection` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="8e806-133">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="8e806-134">Konfiguracja powiązania z [powiązać](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="8e806-134">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="8e806-135">`Bind` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="8e806-135">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="8e806-136">Te pakiety są objęte [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="8e806-136">These packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="8e806-137">Przykłady podane w tym temacie opierają się na:</span><span class="sxs-lookup"><span data-stu-id="8e806-137">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="8e806-138">Ustawianie ścieżki podstawowej aplikacji za pomocą [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="8e806-138">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="8e806-139">`SetBasePath` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="8e806-139">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="8e806-140">Rozpoznawanie sekcji plików konfiguracji przy użyciu [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="8e806-140">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="8e806-141">`GetSection` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="8e806-141">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="8e806-142">Konfiguracja powiązania z [powiązać](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="8e806-142">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="8e806-143">`Bind` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="8e806-143">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

::: moniker-end

## <a name="json-configuration"></a><span data-ttu-id="8e806-144">Konfiguracja JSON</span><span class="sxs-lookup"><span data-stu-id="8e806-144">JSON configuration</span></span>

<span data-ttu-id="8e806-145">Następujące aplikacji konsolowej używa dostawcy konfiguracji JSON:</span><span class="sxs-lookup"><span data-stu-id="8e806-145">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="8e806-146">Aplikacja odczytuje i wyświetla następujące ustawienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e806-146">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="8e806-147">Konfiguracja obejmuje hierarchiczną listę par nazwa wartość, w których węzłach są oddzielone dwukropkiem (`:`).</span><span class="sxs-lookup"><span data-stu-id="8e806-147">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon (`:`).</span></span> <span data-ttu-id="8e806-148">Aby pobrać wartość, należy pobrać `Configuration` indeksatora kluczem odpowiadający element:</span><span class="sxs-lookup"><span data-stu-id="8e806-148">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="8e806-149">Aby pracować z tablic w formacie JSON konfiguracji źródła, należy użyć indeks tablicy jako część ciągu rozdzielone średnikami.</span><span class="sxs-lookup"><span data-stu-id="8e806-149">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="8e806-150">Poniższy przykład pobiera nazwę pierwszego elementu w poprzednim `wizards` tablicy:</span><span class="sxs-lookup"><span data-stu-id="8e806-150">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="8e806-151">Pary nazwa wartość zapisywane do wbudowanej [konfiguracji](/dotnet/api/microsoft.extensions.configuration) dostawcy są **nie** utrwalone.</span><span class="sxs-lookup"><span data-stu-id="8e806-151">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="8e806-152">Jednak można utworzyć niestandardowego dostawcę, który zapisuje wartości.</span><span class="sxs-lookup"><span data-stu-id="8e806-152">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="8e806-153">Zobacz [niestandardowego dostawcy konfiguracji](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="8e806-153">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="8e806-154">Poprzedni przykład używa konfiguracji indeksatora ma odczytać wartości.</span><span class="sxs-lookup"><span data-stu-id="8e806-154">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="8e806-155">Konfiguracja dostępu poza `Startup`, użyj *wzorzec opcje*.</span><span class="sxs-lookup"><span data-stu-id="8e806-155">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="8e806-156">Aby uzyskać więcej informacji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.</span><span class="sxs-lookup"><span data-stu-id="8e806-156">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="8e806-157">Plik XML konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e806-157">XML configuration</span></span>

<span data-ttu-id="8e806-158">Aby pracować z tablic w źródła konfiguracji w formacie XML, podaj `name` indeksu do każdego elementu.</span><span class="sxs-lookup"><span data-stu-id="8e806-158">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="8e806-159">Aby uzyskać dostęp do wartości, należy użyć indeksu:</span><span class="sxs-lookup"><span data-stu-id="8e806-159">Use the index to access the values:</span></span>

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

## <a name="configuration-by-environment"></a><span data-ttu-id="8e806-160">Konfiguracja według środowiska</span><span class="sxs-lookup"><span data-stu-id="8e806-160">Configuration by environment</span></span>

<span data-ttu-id="8e806-161">Jest to zazwyczaj mieć różne ustawienia konfiguracji dla różnych środowisk, na przykład, rozwoju, testowania i produkcji.</span><span class="sxs-lookup"><span data-stu-id="8e806-161">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="8e806-162">`CreateDefaultBuilder` Metody rozszerzenia w aplikacji ASP.NET Core 2.x (lub za pomocą `AddJsonFile` i `AddEnvironmentVariables` bezpośrednio w aplikacji ASP.NET Core 1.x) dodaje dostawcy konfiguracji do odczytywania źródeł konfiguracji systemu i pliki w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="8e806-162">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="8e806-163">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="8e806-163">*appsettings.json*</span></span>
* <span data-ttu-id="8e806-164">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="8e806-164">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="8e806-165">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="8e806-165">Environment variables</span></span>

<span data-ttu-id="8e806-166">Platforma ASP.NET Core 1.x aplikacji należy wywołać `AddJsonFile` i [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="8e806-166">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="8e806-167">Zobacz [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) objaśnienia dotyczące parametrów.</span><span class="sxs-lookup"><span data-stu-id="8e806-167">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="8e806-168">`reloadOnChange` jest obsługiwana tylko w programie ASP.NET Core 1.1 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="8e806-168">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="8e806-169">Źródła konfiguracji są do odczytu w kolejności, że jest określona.</span><span class="sxs-lookup"><span data-stu-id="8e806-169">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="8e806-170">W poprzednim kodzie zmienne środowiskowe są odczytywane ostatnio.</span><span class="sxs-lookup"><span data-stu-id="8e806-170">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="8e806-171">Wszelkie wartości konfiguracji ustawiać za pośrednictwem Zastąp środowiska w dwóch dostawców na poprzednim.</span><span class="sxs-lookup"><span data-stu-id="8e806-171">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="8e806-172">Należy wziąć pod uwagę następujące *appsettings. Staging.JSON* pliku:</span><span class="sxs-lookup"><span data-stu-id="8e806-172">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="8e806-173">W poniższym kodzie `Configure` odczytuje wartość `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="8e806-173">In the following code, `Configure` reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

<span data-ttu-id="8e806-174">Środowisko jest zazwyczaj równa `Development`, `Staging`, lub `Production`.</span><span class="sxs-lookup"><span data-stu-id="8e806-174">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="8e806-175">Aby uzyskać więcej informacji, zobacz [używanie wielu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="8e806-175">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="8e806-176">Zagadnienia dotyczące konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e806-176">Configuration considerations:</span></span>

* <span data-ttu-id="8e806-177">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) ponownie załadować dane konfiguracji po jej zmianie.</span><span class="sxs-lookup"><span data-stu-id="8e806-177">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) can reload configuration data when it changes.</span></span>
* <span data-ttu-id="8e806-178">Klucze konfiguracji **nie** uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="8e806-178">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="8e806-179">**Nigdy nie** przechowywanie haseł i innych poufnych danych w konfiguracji dostawcy kodu lub pliki konfiguracyjne w formacie zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="8e806-179">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="8e806-180">Nie używać kluczy tajnych w środowisku produkcyjnym w trakcie opracowywania lub środowisk testowych.</span><span class="sxs-lookup"><span data-stu-id="8e806-180">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="8e806-181">Określ klucze tajne poza projektem, nie może być przypadkowo wszelkich starań, by repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="8e806-181">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="8e806-182">Dowiedz się więcej o [jak używać wielu środowisk](xref:fundamentals/environments) i zarządzanie nimi [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="8e806-182">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets in development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="8e806-183">Wartości konfiguracji hierarchiczne określonych w zmiennych środowiskowych, dwukropek (`:`) może nie działać na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="8e806-183">For hierarchical config values specified in environment variables, a colon (`:`) may not work on all platforms.</span></span> <span data-ttu-id="8e806-184">Podwójnym podkreśleniem (`__`) jest obsługiwana przez wszystkie platformy.</span><span class="sxs-lookup"><span data-stu-id="8e806-184">Double underscore (`__`) is supported by all platforms.</span></span>
* <span data-ttu-id="8e806-185">Podczas interakcji z konfiguracji interfejsu API, dwukropek (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="8e806-185">When interacting with the configuration API, a colon (`:`) works on all platforms.</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="8e806-186">Dostawca w pamięci i wiązania klasy POCO</span><span class="sxs-lookup"><span data-stu-id="8e806-186">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="8e806-187">Poniższy przykład przedstawia sposób użycia dostawcy w pamięci i powiąż go z klasy:</span><span class="sxs-lookup"><span data-stu-id="8e806-187">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="8e806-188">Wartości konfiguracji są zwracane jako ciągi, ale powiązanie umożliwia konstrukcji obiektów.</span><span class="sxs-lookup"><span data-stu-id="8e806-188">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="8e806-189">Powiązanie umożliwia pobieranie obiektów POCO lub wykresy nawet cały obiekt.</span><span class="sxs-lookup"><span data-stu-id="8e806-189">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="8e806-190">GetValue</span><span class="sxs-lookup"><span data-stu-id="8e806-190">GetValue</span></span>

<span data-ttu-id="8e806-191">W poniższym przykładzie pokazano [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="8e806-191">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="8e806-192">ConfigurationBinder `GetValue<T>` metody umożliwia określenie wartości domyślnej (80 w przykładzie).</span><span class="sxs-lookup"><span data-stu-id="8e806-192">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="8e806-193">`GetValue<T>` Służy do prostych scenariuszy i nie jest powiązana całej sekcji.</span><span class="sxs-lookup"><span data-stu-id="8e806-193">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="8e806-194">`GetValue<T>` pobiera wartości skalarnych z `GetSection(key).Value` konwertowane do określonego typu.</span><span class="sxs-lookup"><span data-stu-id="8e806-194">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="8e806-195">Powiąż z wykresu obiektu</span><span class="sxs-lookup"><span data-stu-id="8e806-195">Bind to an object graph</span></span>

<span data-ttu-id="8e806-196">Każdy obiekt w klasie może być powiązany cyklicznie.</span><span class="sxs-lookup"><span data-stu-id="8e806-196">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="8e806-197">Należy wziąć pod uwagę następujące `AppSettings` klasy:</span><span class="sxs-lookup"><span data-stu-id="8e806-197">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="8e806-198">Poniższy przykład tworzy powiązanie z `AppSettings` klasy:</span><span class="sxs-lookup"><span data-stu-id="8e806-198">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="8e806-199">**Platforma ASP.NET Core 1.1** i nowszej można użyć `Get<T>`, który współdziała z całą sekcję.</span><span class="sxs-lookup"><span data-stu-id="8e806-199">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="8e806-200">`Get<T>` może być bardziej wygodne niż przy użyciu `Bind`.</span><span class="sxs-lookup"><span data-stu-id="8e806-200">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="8e806-201">Poniższy kod przedstawia sposób użycia `Get<T>` z poprzednim przykładem:</span><span class="sxs-lookup"><span data-stu-id="8e806-201">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="8e806-202">Za pomocą następujących *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="8e806-202">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="8e806-203">Ten program wyświetla `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="8e806-203">The program displays `Height 11`.</span></span>

<span data-ttu-id="8e806-204">Poniższy kod może służyć do jednostki konfiguracji testu:</span><span class="sxs-lookup"><span data-stu-id="8e806-204">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="8e806-205">Tworzenie niestandardowego dostawcy programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="8e806-205">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="8e806-206">W tej sekcji dostawcy konfiguracji podstawowej, która odczytuje pary nazwa wartość z bazy danych przy użyciu programu EF jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="8e806-206">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="8e806-207">Zdefiniuj `ConfigurationValue` jednostki do przechowywania wartości konfiguracji w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="8e806-207">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="8e806-208">Dodaj `ConfigurationContext` do przechowywania i uzyskać dostęp do skonfigurowanej wartości:</span><span class="sxs-lookup"><span data-stu-id="8e806-208">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="8e806-209">Utwórz klasę, która implementuje [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="8e806-209">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="8e806-210">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="8e806-210">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="8e806-211">Dostawca konfiguracji inicjuje bazy danych, gdy jest on pusty:</span><span class="sxs-lookup"><span data-stu-id="8e806-211">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="8e806-212">Wyróżnione wartości z bazy danych ("value_from_ef_1" i "value_from_ef_2") są wyświetlane po uruchomieniu przykładu.</span><span class="sxs-lookup"><span data-stu-id="8e806-212">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="8e806-213">`EFConfigSource` Metody rozszerzenia umożliwiające dodawanie źródło konfiguracji można użyć:</span><span class="sxs-lookup"><span data-stu-id="8e806-213">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="8e806-214">Poniższy kod przedstawia sposób używania niestandardowego `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="8e806-214">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="8e806-215">Przykładowa aplikacja dodaje niestandardową Uwaga `EFConfigProvider` po dostawcy JSON, więc wszystkie ustawienia z bazy danych spowoduje przesłonięcie ustawień z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="8e806-215">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="8e806-216">Za pomocą następujących *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="8e806-216">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="8e806-217">Są wyświetlane następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="8e806-217">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="8e806-218">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e806-218">CommandLine configuration provider</span></span>

<span data-ttu-id="8e806-219">[Dostawcę konfiguracji CommandLine](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) odbiera pary klucz wartość argumentu wiersza polecenia dla konfiguracji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8e806-219">The [CommandLine configuration provider](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="8e806-220">Wyświetlanie lub pobieranie przykładowych konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e806-220">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="8e806-221">Konfigurowanie i używanie dostawcę konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e806-221">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="8e806-222">Konfiguracja podstawowa</span><span class="sxs-lookup"><span data-stu-id="8e806-222">Basic Configuration</span></span>](#tab/basicconfiguration/)

<span data-ttu-id="8e806-223">Aby uaktywnić konfigurację wiersza polecenia, należy wywołać `AddCommandLine` metody rozszerzenia w wystąpieniu [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="8e806-223">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="8e806-224">Uruchamianie kodu, są wyświetlane następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="8e806-224">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="8e806-225">Przekazywanie argumentu pary klucz wartość w wierszu polecenia powoduje zmianę wartości `Profile:MachineName` i `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="8e806-225">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="8e806-226">W oknie konsoli zostaną wyświetlone:</span><span class="sxs-lookup"><span data-stu-id="8e806-226">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="8e806-227">Aby zastąpić konfiguracji udostępnianych przez innych dostawców konfiguracji przy użyciu wiersza polecenia konfiguracji, należy wywołać `AddCommandLine` ostatni `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8e806-227">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8e806-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8e806-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="8e806-229">Typowe aplikacje ASP.NET Core 2.x należy użyć metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta:</span><span class="sxs-lookup"><span data-stu-id="8e806-229">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="8e806-230">`CreateDefaultBuilder` ładuje opcjonalna konfiguracja z *appsettings.json*, *appsettings. { Środowisko} .json*, [wpisami tajnymi użytkowników](xref:security/app-secrets) (w `Development` środowiska), zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8e806-230">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="8e806-231">Dostawca konfiguracji wiersza polecenia nazywa się ostatnio.</span><span class="sxs-lookup"><span data-stu-id="8e806-231">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="8e806-232">Ostatnie wywoływania dostawcy umożliwia argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji o nazwie wcześniej.</span><span class="sxs-lookup"><span data-stu-id="8e806-232">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="8e806-233">Aby uzyskać *appsettings* pliki where:</span><span class="sxs-lookup"><span data-stu-id="8e806-233">For *appsettings* files where:</span></span>

* <span data-ttu-id="8e806-234">`reloadOnChange` jest włączone.</span><span class="sxs-lookup"><span data-stu-id="8e806-234">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="8e806-235">Zawierać tego samego ustawienia w argumentach wiersza polecenia i *appsettings* pliku.</span><span class="sxs-lookup"><span data-stu-id="8e806-235">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="8e806-236">*Appsettings* plik zawierający pasującego argumentu wiersza polecenia, to ulegnie zmianie po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e806-236">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="8e806-237">Jeśli wszystkie powyższe warunki są spełnione, zostaną zastąpione argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8e806-237">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="8e806-238">Można użyć aplikacji programu ASP.NET Core 2.x [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) zamiast `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8e806-238">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8e806-239">Korzystając z `WebHostBuilder`, ręcznie konfiguracji zestawu z [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="8e806-239">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="8e806-240">Zobacz kartę platformy ASP.NET Core 1.x, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="8e806-240">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8e806-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8e806-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="8e806-242">Tworzenie [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) i wywołać `AddCommandLine` metodę CommandLine dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e806-242">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="8e806-243">Ostatnie wywoływania dostawcy umożliwia argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji o nazwie wcześniej.</span><span class="sxs-lookup"><span data-stu-id="8e806-243">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="8e806-244">Zastosuj konfigurację do [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) z `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="8e806-244">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="8e806-245">Argumenty</span><span class="sxs-lookup"><span data-stu-id="8e806-245">Arguments</span></span>

<span data-ttu-id="8e806-246">Argumenty wiersza polecenia musi być zgodny z jednym z dwóch formatów pokazano w poniższej tabeli:</span><span class="sxs-lookup"><span data-stu-id="8e806-246">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="8e806-247">Format argumentu:</span><span class="sxs-lookup"><span data-stu-id="8e806-247">Argument format</span></span>                                                     | <span data-ttu-id="8e806-248">Przykład</span><span class="sxs-lookup"><span data-stu-id="8e806-248">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="8e806-249">Pojedynczy argument: pary klucz wartość oddzielone znak równości (`=`)</span><span class="sxs-lookup"><span data-stu-id="8e806-249">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="8e806-250">Sekwencja dwa argumenty: pary klucz wartość oddzielone spacją</span><span class="sxs-lookup"><span data-stu-id="8e806-250">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="8e806-251">**Jeden argument**</span><span class="sxs-lookup"><span data-stu-id="8e806-251">**Single argument**</span></span>

<span data-ttu-id="8e806-252">Wartość musi stosować się znak równości (`=`).</span><span class="sxs-lookup"><span data-stu-id="8e806-252">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="8e806-253">Może mieć wartości null (na przykład `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="8e806-253">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="8e806-254">Klucz może mieć prefiksu.</span><span class="sxs-lookup"><span data-stu-id="8e806-254">The key may have a prefix.</span></span>

| <span data-ttu-id="8e806-255">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="8e806-255">Key prefix</span></span>               | <span data-ttu-id="8e806-256">Przykład</span><span class="sxs-lookup"><span data-stu-id="8e806-256">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="8e806-257">Nie prefiksu</span><span class="sxs-lookup"><span data-stu-id="8e806-257">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="8e806-258">Pojedynczy dash (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="8e806-258">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="8e806-259">Dwa łączniki (`--`)</span><span class="sxs-lookup"><span data-stu-id="8e806-259">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="8e806-260">Kreski ułamkowej (`/`)</span><span class="sxs-lookup"><span data-stu-id="8e806-260">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="8e806-261">&#8224;Klucz z prefiksem jeden (`-`) musi być podana w [Przełącz mapowania](#switch-mappings), które zostały opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="8e806-261">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="8e806-262">Przykładowe polecenie:</span><span class="sxs-lookup"><span data-stu-id="8e806-262">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="8e806-263">Uwaga: Jeśli `-key2` nie jest obecny w [Przełącz mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="8e806-263">Note: If `-key2` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="8e806-264">**Sekwencja dwóch argumentów**</span><span class="sxs-lookup"><span data-stu-id="8e806-264">**Sequence of two arguments**</span></span>

<span data-ttu-id="8e806-265">Wartość nie może mieć wartości null i muszą być zgodne klucza, rozdzielonych spacją.</span><span class="sxs-lookup"><span data-stu-id="8e806-265">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="8e806-266">Klucz musi mieć prefiks.</span><span class="sxs-lookup"><span data-stu-id="8e806-266">The key must have a prefix.</span></span>

| <span data-ttu-id="8e806-267">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="8e806-267">Key prefix</span></span>               | <span data-ttu-id="8e806-268">Przykład</span><span class="sxs-lookup"><span data-stu-id="8e806-268">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="8e806-269">Pojedynczy dash (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="8e806-269">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="8e806-270">Dwa łączniki (`--`)</span><span class="sxs-lookup"><span data-stu-id="8e806-270">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="8e806-271">Kreski ułamkowej (`/`)</span><span class="sxs-lookup"><span data-stu-id="8e806-271">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="8e806-272">&#8224;Klucz z prefiksem jeden (`-`) musi być podana w [Przełącz mapowania](#switch-mappings), które zostały opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="8e806-272">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="8e806-273">Przykładowe polecenie:</span><span class="sxs-lookup"><span data-stu-id="8e806-273">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="8e806-274">Uwaga: Jeśli `-key1` nie jest obecny w [Przełącz mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="8e806-274">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="8e806-275">Zduplikowane klucze</span><span class="sxs-lookup"><span data-stu-id="8e806-275">Duplicate keys</span></span>

<span data-ttu-id="8e806-276">Jeśli zduplikowane klucze są dostarczane, używana jest ostatnia pary klucz wartość.</span><span class="sxs-lookup"><span data-stu-id="8e806-276">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="8e806-277">Przełącz mapowania</span><span class="sxs-lookup"><span data-stu-id="8e806-277">Switch mappings</span></span>

<span data-ttu-id="8e806-278">Podczas ręcznego tworzenia konfiguracji za pomocą `ConfigurationBuilder`, przełącznik słownik mapowań mogą być dodawane do `AddCommandLine` metody.</span><span class="sxs-lookup"><span data-stu-id="8e806-278">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="8e806-279">Przełącznik jest transformowanie na nazwę klucza wymiany logiki.</span><span class="sxs-lookup"><span data-stu-id="8e806-279">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="8e806-280">W przypadku przełącznika słownik mapowań słownika jest sprawdzane dla klucza, który pasuje do klucza, dostarczone przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8e806-280">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="8e806-281">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) jest przekazywana do ustawiania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e806-281">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="8e806-282">Mapowanie przełącznika jest wymagane dla dowolnego klucz wiersza polecenia poprzedzone kreską pojedynczego (`-`).</span><span class="sxs-lookup"><span data-stu-id="8e806-282">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="8e806-283">Przełącz mapowania słownika kluczowych reguł:</span><span class="sxs-lookup"><span data-stu-id="8e806-283">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="8e806-284">Przełączniki musi rozpoczynać się kreską (`-`) lub podwójne kreska (`--`).</span><span class="sxs-lookup"><span data-stu-id="8e806-284">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="8e806-285">Słownik mapowań przełącznik nie może zawierać zduplikowane klucze.</span><span class="sxs-lookup"><span data-stu-id="8e806-285">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="8e806-286">W poniższym przykładzie `GetSwitchMappings` metoda umożliwia argumenty wiersza polecenia, użyj jednej kreski (`-`) prefiks klucza i uniknąć wiodących prefiksy podklucza.</span><span class="sxs-lookup"><span data-stu-id="8e806-286">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="8e806-287">Bez podawania argumenty wiersza polecenia, udostępniane słownik `AddInMemoryCollection` ustawia wartości konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e806-287">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="8e806-288">Uruchom aplikację za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="8e806-288">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="8e806-289">W oknie konsoli zostaną wyświetlone:</span><span class="sxs-lookup"><span data-stu-id="8e806-289">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="8e806-290">Aby przekazać w ustawieniach konfiguracji, należy użyć następujących:</span><span class="sxs-lookup"><span data-stu-id="8e806-290">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="8e806-291">W oknie konsoli zostaną wyświetlone:</span><span class="sxs-lookup"><span data-stu-id="8e806-291">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="8e806-292">Po utworzeniu przełącznika słownik mapowań zawiera dane wyświetlane w poniższej tabeli:</span><span class="sxs-lookup"><span data-stu-id="8e806-292">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="8e806-293">Key</span><span class="sxs-lookup"><span data-stu-id="8e806-293">Key</span></span>            | <span data-ttu-id="8e806-294">Wartość</span><span class="sxs-lookup"><span data-stu-id="8e806-294">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="8e806-295">Aby zademonstrować, przełączanie klucza, używając słownika, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8e806-295">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="8e806-296">Klucze wiersza polecenia zostały zamienione.</span><span class="sxs-lookup"><span data-stu-id="8e806-296">The command-line keys are swapped.</span></span> <span data-ttu-id="8e806-297">W oknie konsoli zostaną wyświetlone wartości konfiguracji dla `Profile:MachineName` i `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="8e806-297">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="8e806-298">plik Web.config</span><span class="sxs-lookup"><span data-stu-id="8e806-298">web.config file</span></span>

<span data-ttu-id="8e806-299">A *web.config* plik jest wymagany w przypadku hostowania aplikacji w usługach IIS lub IIS Express.</span><span class="sxs-lookup"><span data-stu-id="8e806-299">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="8e806-300">Ustawienia w *web.config* Włącz [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) do uruchomienia aplikacji i skonfigurować inne ustawienia usług IIS i modułów.</span><span class="sxs-lookup"><span data-stu-id="8e806-300">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="8e806-301">Jeśli *web.config* plik nie jest obecny i plik projektu zawiera `<Project Sdk="Microsoft.NET.Sdk.Web">`, publikowania projektu tworzy *web.config* pliku w opublikowanych danych wyjściowych ( *publikowania* folder).</span><span class="sxs-lookup"><span data-stu-id="8e806-301">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="8e806-302">Aby uzyskać więcej informacji, zobacz [hosta ASP.NET Core na Windows z programem IIS](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="8e806-302">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="access-configuration-during-startup"></a><span data-ttu-id="8e806-303">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="8e806-303">Access configuration during startup</span></span>

<span data-ttu-id="8e806-304">Uzyskać dostępu do konfiguracji w ramach `ConfigureServices` lub `Configure` podczas uruchamiania, zapoznaj się z przykładami w [uruchamiania aplikacji](xref:fundamentals/startup) tematu.</span><span class="sxs-lookup"><span data-stu-id="8e806-304">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="adding-configuration-from-an-external-assembly"></a><span data-ttu-id="8e806-305">Dodawanie konfiguracji z zewnętrznego zestawu</span><span class="sxs-lookup"><span data-stu-id="8e806-305">Adding configuration from an external assembly</span></span>

<span data-ttu-id="8e806-306">[Interfejsu IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementacji umożliwia dodawanie rozszerzeń do aplikacji przy uruchamianiu z zewnętrznego zestawu poza aplikacji `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="8e806-306">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="8e806-307">Aby uzyskać więcej informacji, zobacz [zwiększanie możliwości aplikacji z zewnętrznego zestawu](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="8e806-307">For more information, see [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a><span data-ttu-id="8e806-308">Konfiguracja dostępu w widoku MVC lub strony Razor</span><span class="sxs-lookup"><span data-stu-id="8e806-308">Access configuration in a Razor Page or MVC view</span></span>

<span data-ttu-id="8e806-309">Aby uzyskać dostęp do ustawień konfiguracji na stronie stron Razor lub widoku MVC, należy dodać [użycie dyrektywy](xref:mvc/views/razor#using) ([odwołanie w C#: using — dyrektywa](/dotnet/csharp/language-reference/keywords/using-directive)) dla [Microsoft.Extensions.Configuration przestrzeni nazw ](/dotnet/api/microsoft.extensions.configuration) i wstawiać [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) do strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="8e806-309">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](/dotnet/api/microsoft.extensions.configuration) and inject [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) into the page or view.</span></span>

<span data-ttu-id="8e806-310">Na stronie stron Razor:</span><span class="sxs-lookup"><span data-stu-id="8e806-310">In a Razor Pages page:</span></span>

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

<span data-ttu-id="8e806-311">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="8e806-311">In an MVC view:</span></span>

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

## <a name="additional-notes"></a><span data-ttu-id="8e806-312">Dodatkowe uwagi</span><span class="sxs-lookup"><span data-stu-id="8e806-312">Additional notes</span></span>

* <span data-ttu-id="8e806-313">Wstrzykiwanie zależności (DI) nie została ustawiona w górę do momentu po `ConfigureServices` zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="8e806-313">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="8e806-314">System konfiguracji nie jest DI aware.</span><span class="sxs-lookup"><span data-stu-id="8e806-314">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="8e806-315">`IConfiguration` ma dwie specjalizacje:</span><span class="sxs-lookup"><span data-stu-id="8e806-315">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="8e806-316">`IConfigurationRoot` Używany do węzła głównego.</span><span class="sxs-lookup"><span data-stu-id="8e806-316">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="8e806-317">Można uruchomić ponownie załadować.</span><span class="sxs-lookup"><span data-stu-id="8e806-317">Can trigger a reload.</span></span>
  * <span data-ttu-id="8e806-318">`IConfigurationSection` Reprezentuje sekcję konfiguracji wartości.</span><span class="sxs-lookup"><span data-stu-id="8e806-318">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="8e806-319">`GetSection` i `GetChildren` metody zwracają `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="8e806-319">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="8e806-320">Użyj [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) podczas ponownego ładowania konfiguracji lub w celu uzyskania dostępu do poszczególnych dostawców.</span><span class="sxs-lookup"><span data-stu-id="8e806-320">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="8e806-321">Żadna z tych sytuacji nie występują.</span><span class="sxs-lookup"><span data-stu-id="8e806-321">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e806-322">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8e806-322">Additional resources</span></span>

* [<span data-ttu-id="8e806-323">Opcje</span><span class="sxs-lookup"><span data-stu-id="8e806-323">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="8e806-324">Używanie wielu środowisk</span><span class="sxs-lookup"><span data-stu-id="8e806-324">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="8e806-325">Bezpieczne przechowywanie wpisów tajnych aplikacji w czasie projektowania</span><span class="sxs-lookup"><span data-stu-id="8e806-325">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="8e806-326">Hosta w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e806-326">Host in ASP.NET Core</span></span>](xref:fundamentals/host/index)
* [<span data-ttu-id="8e806-327">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="8e806-327">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="8e806-328">Dostawca konfiguracji usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8e806-328">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
