---
title: Konfiguracja w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core przy użyciu interfejsu API konfiguracji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/29/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: b4fa082c5a53bc9ecb3c7b8ddcbf243ef0d94ba7
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989693"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="8e1b9-103">Konfiguracja w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e1b9-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="8e1b9-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Kirka Larkin](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="8e1b9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8e1b9-105">Konfiguracja w ASP.NET Core jest wykonywana przy użyciu co najmniej jednego [dostawcy konfiguracji](#cp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-105">Configuration in ASP.NET Core is performed using one or more [configuration providers](#cp).</span></span> <span data-ttu-id="8e1b9-106">Dostawcy konfiguracji odczytują dane konfiguracji z par klucz-wartość przy użyciu różnych źródeł konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-106">Configuration providers read configuration data from key-value pairs using a variety of configuration sources:</span></span>

* <span data-ttu-id="8e1b9-107">Pliki ustawień, takie jak *appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="8e1b9-107">Settings files, such as *appsettings.json*</span></span>
* <span data-ttu-id="8e1b9-108">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="8e1b9-108">Environment variables</span></span>
* <span data-ttu-id="8e1b9-109">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8e1b9-109">Azure Key Vault</span></span>
* <span data-ttu-id="8e1b9-110">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="8e1b9-110">Azure App Configuration</span></span>
* <span data-ttu-id="8e1b9-111">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-111">Command-line arguments</span></span>
* <span data-ttu-id="8e1b9-112">Niestandardowi dostawcy, instalowani lub utworzony</span><span class="sxs-lookup"><span data-stu-id="8e1b9-112">Custom providers, installed or created</span></span>
* <span data-ttu-id="8e1b9-113">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="8e1b9-113">Directory files</span></span>
* <span data-ttu-id="8e1b9-114">Obiekty w pamięci .NET</span><span class="sxs-lookup"><span data-stu-id="8e1b9-114">In-memory .NET objects</span></span>

<span data-ttu-id="8e1b9-115">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8e1b9-115">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<a name="default"></a>

## <a name="default-configuration"></a><span data-ttu-id="8e1b9-116">Konfiguracja domyślna</span><span class="sxs-lookup"><span data-stu-id="8e1b9-116">Default configuration</span></span>

<span data-ttu-id="8e1b9-117">ASP.NET Core aplikacje sieci Web utworzone za pomocą programu [dotnet New](/dotnet/core/tools/dotnet-new) lub Visual Studio generują następujący kod:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-117">ASP.NET Core web apps created with [dotnet new](/dotnet/core/tools/dotnet-new) or Visual Studio generate the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet&highlight=9)]

 <span data-ttu-id="8e1b9-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

1. <span data-ttu-id="8e1b9-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) :  Dodaje istniejący `IConfiguration` jako źródło.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) :  Adds an existing `IConfiguration` as a source.</span></span> <span data-ttu-id="8e1b9-120">W przypadku konfiguracji domyślnej program dodaje konfigurację [hosta](#hvac) i ustawia ją jako pierwsze źródło konfiguracji _aplikacji_ .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-120">In the default configuration case, adds the [host](#hvac) configuration and setting it as the first source for the _app_ configuration.</span></span>
1. <span data-ttu-id="8e1b9-121">[appSettings. JSON](#appsettingsjson) przy użyciu [dostawcy konfiguracji JSON](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-121">[appsettings.json](#appsettingsjson) using the [JSON configuration provider](#file-configuration-provider).</span></span>
1. <span data-ttu-id="8e1b9-122">*appSettings.* plik`Environment` *. JSON* przy użyciu [dostawcy konfiguracji JSON](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-122">*appsettings.*`Environment`*.json* using the [JSON configuration provider](#file-configuration-provider).</span></span> <span data-ttu-id="8e1b9-123">Na przykład *AppSettings*. ***Środowisko produkcyjne***. *JSON* i *AppSettings*. ***Programowanie***. *kod JSON*.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-123">For example, *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json*.</span></span>
1. <span data-ttu-id="8e1b9-124">Wpisy [tajne aplikacji](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development`u.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-124">[App secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
1. <span data-ttu-id="8e1b9-125">Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#evcp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-125">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="8e1b9-126">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-126">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="8e1b9-127">Dostawcy konfiguracji, którzy zostaną dodani później przesłaniają poprzednie ustawienia klucza.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-127">Configuration providers that are added later override previous key settings.</span></span> <span data-ttu-id="8e1b9-128">Na przykład jeśli `MyKey` jest ustawiona w pliku *appSettings. JSON* i środowisku, zostanie użyta wartość środowiska.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-128">For example, if `MyKey` is set in both *appsettings.json* and the environment, the environment value is used.</span></span> <span data-ttu-id="8e1b9-129">Przy użyciu domyślnych dostawców konfiguracji [dostawca konfiguracji wiersza polecenia](#command-line-configuration-provider) zastępuje wszystkich innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-129">Using the default configuration providers, the  [Command-line configuration provider](#command-line-configuration-provider) overrides all other providers.</span></span>

<span data-ttu-id="8e1b9-130">Aby uzyskać więcej informacji na `CreateDefaultBuilder`, zobacz [ustawienia domyślnego konstruktora](xref:fundamentals/host/generic-host#default-builder-settings).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-130">For more information on `CreateDefaultBuilder`, see [Default builder settings](xref:fundamentals/host/generic-host#default-builder-settings).</span></span>

<span data-ttu-id="8e1b9-131">Poniższy kod wyświetla dostawców konfiguracji włączonych w kolejności, w jakiej zostały dodane:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-131">The following code displays the enabled configuration providers in the order they were added:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Index2.cshtml.cs?name=snippet)]

### <a name="appsettingsjson"></a><span data-ttu-id="8e1b9-132">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="8e1b9-132">appsettings.json</span></span>

<span data-ttu-id="8e1b9-133">Rozważmy następujący plik *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="8e1b9-133">Consider the following *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="8e1b9-134">Poniższy kod z [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) wyświetla kilka powyższych ustawień konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-134">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-135">Domyślna <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> ładuje konfigurację w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-135">The default <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration in the following order:</span></span>

1. <span data-ttu-id="8e1b9-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="8e1b9-136">*appsettings.json*</span></span>
1. <span data-ttu-id="8e1b9-137">*appSettings.* plik`Environment` *. JSON* : Na przykład, *AppSettings*. ***Środowisko produkcyjne***. *JSON* i *AppSettings*. ***Programowanie***. pliki *JSON* .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-137">*appsettings.*`Environment`*.json* : For example, the *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json* files.</span></span> <span data-ttu-id="8e1b9-138">Wersja środowiska pliku jest ładowana na podstawie [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-138">The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="8e1b9-139">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-139">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="8e1b9-140">*AppSettings*.`Environment`. wartości *JSON* przesłaniają klucze w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-140">*appsettings*.`Environment`.*json* values override keys in *appsettings.json*.</span></span> <span data-ttu-id="8e1b9-141">Na przykład domyślnie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-141">For example, by default:</span></span>

* <span data-ttu-id="8e1b9-142">W obszarze programowanie, *AppSettings*. ***Programowanie***. Konfiguracja *JSON* zastępuje wartości Znalezione w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-142">In development, *appsettings*.***Development***.*json* configuration overwrites values found in *appsettings.json*.</span></span>
* <span data-ttu-id="8e1b9-143">W obszarze produkcja, *AppSettings*. ***Środowisko produkcyjne***. Konfiguracja *JSON* zastępuje wartości Znalezione w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-143">In production, *appsettings*.***Production***.*json* configuration overwrites values found in *appsettings.json*.</span></span> <span data-ttu-id="8e1b9-144">Na przykład podczas wdrażania aplikacji na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-144">For example, when deploying the app to Azure.</span></span>

<a name="optpat"></a>

#### <a name="bind-hierarchical-configuration-data-using-the-options-pattern"></a><span data-ttu-id="8e1b9-145">Powiązywanie hierarchicznych danych konfiguracji przy użyciu wzorca opcji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-145">Bind hierarchical configuration data using the options pattern</span></span>

<span data-ttu-id="8e1b9-146">Preferowanym sposobem odczytywania pokrewnych wartości konfiguracji jest użycie [wzorca opcji](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-146">The preferred way to read related configuration values is using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="8e1b9-147">Na przykład, aby odczytać następujące wartości konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-147">For example, to read the following configuration values:</span></span>

```json
  "Position": {
    "Title": "Editor",
    "Name": "Joe Smith"
  }
```

<span data-ttu-id="8e1b9-148">Utwórz następującą klasę `PositionOptions`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-148">Create the following `PositionOptions` class:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Options/PositionOptions.cs?name=snippet)]

<span data-ttu-id="8e1b9-149">Wszystkie publiczne właściwości odczytu i zapisu typu są powiązane.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-149">All the public read-write properties of the type are bound.</span></span> <span data-ttu-id="8e1b9-150">Pola ***nie*** są powiązane.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-150">Fields are ***not*** bound.</span></span>

<span data-ttu-id="8e1b9-151">Następujący kod:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-151">The following code:</span></span>

* <span data-ttu-id="8e1b9-152">Wywołuje [ConfigurationBinder. bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) , aby powiązać klasę `PositionOptions` z sekcją `Position`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-152">Calls [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) to bind the `PositionOptions` class to the `Position` section.</span></span>
* <span data-ttu-id="8e1b9-153">Wyświetla dane konfiguracji `Position`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-153">Displays the `Position` configuration data.</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test22.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-154">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) tworzy powiązania i zwraca określony typ.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-154">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="8e1b9-155">`ConfigurationBinder.Get<T>` mogą być wygodniejsze niż korzystanie z `ConfigurationBinder.Bind`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-155">`ConfigurationBinder.Get<T>` may be more convenient than using `ConfigurationBinder.Bind`.</span></span> <span data-ttu-id="8e1b9-156">Poniższy kod przedstawia sposób użycia `ConfigurationBinder.Get<T>` z klasą `PositionOptions`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-156">The following code shows how to use `ConfigurationBinder.Get<T>` with the `PositionOptions` class:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test21.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-157">Alternatywne podejście w przypadku używania ***wzorca opcji*** ma na celu powiązanie sekcji `Position` i dodanie jej do [kontenera usługi iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-157">An alternative approach when using the ***options pattern*** is to bind the `Position` section and add it to the [dependency injection service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="8e1b9-158">W poniższym kodzie `PositionOptions` jest dodawane do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązane z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-158">In the following code, `PositionOptions` is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Startup.cs?name=snippet)]

<span data-ttu-id="8e1b9-159">Korzystając z powyższego kodu, poniższy kod odczytuje opcje pozycji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-159">Using the preceding code, the following code reads the position options:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test2.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-160">Przy użyciu konfiguracji [domyślnej](#default) pliki *appSettings. JSON* i *appSettings.* `Environment` *. JSON* są włączone z [reloadOnChange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-160">Using the [default](#default) configuration, the *appsettings.json* and *appsettings.*`Environment`*.json* files are enabled with [reloadOnChange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75).</span></span> <span data-ttu-id="8e1b9-161">Zmiany wprowadzone w pliku *appSettings. JSON* i *appSettings.* `Environment` *. JSON* ***po*** uruchomieniu aplikacji są odczytywane przez [dostawcę konfiguracji JSON](#jcp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-161">Changes made to the *appsettings.json* and *appsettings.*`Environment`*.json* file ***after*** the app starts are read by the [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="8e1b9-162">Aby uzyskać informacje na temat dodawania dodatkowych plików konfiguracji JSON, zobacz [dostawca konfiguracji JSON](#jcp) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-162">See [JSON configuration provider](#jcp) in this document for information on adding additional JSON configuration files.</span></span>

<a name="security"></a>

## <a name="security-and-secret-manager"></a><span data-ttu-id="8e1b9-163">Security and Secret Manager</span><span class="sxs-lookup"><span data-stu-id="8e1b9-163">Security and secret manager</span></span>

<span data-ttu-id="8e1b9-164">Wskazówki dotyczące danych konfiguracyjnych:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-164">Configuration data guidelines:</span></span>

* <span data-ttu-id="8e1b9-165">Nie należy przechowywać haseł ani innych danych poufnych w kodzie dostawcy konfiguracji ani w plikach konfiguracji zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-165">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="8e1b9-166">Za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets) można przechowywać wpisy tajne.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-166">The [Secret manager](xref:security/app-secrets) can be used to store secrets in development.</span></span>
* <span data-ttu-id="8e1b9-167">Nie używaj tajemnic produkcyjnych w środowiskach deweloperskich i testowych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-167">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="8e1b9-168">Określ wpisy tajne poza projektem, aby nie mogły zostać przypadkowo przekazane do repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-168">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="8e1b9-169">[Domyślnie](#default)program [Secret Manager](xref:security/app-secrets) odczytuje ustawienia konfiguracji po pliku *appSettings. JSON* i *appSettings.* `Environment` *. JSON*.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-169">By [default](#default), [Secret manager](xref:security/app-secrets) reads configuration settings after *appsettings.json* and *appsettings.*`Environment`*.json*.</span></span>

<span data-ttu-id="8e1b9-170">Aby uzyskać więcej informacji na temat przechowywania haseł lub innych poufnych danych:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-170">For more information on storing passwords or other sensitive data:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="8e1b9-171"><xref:security/app-secrets>:  Zawiera porady dotyczące używania zmiennych środowiskowych do przechowywania poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-171"><xref:security/app-secrets>:  Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="8e1b9-172">Menedżer wpisów tajnych używa [dostawcy konfiguracji plików](#fcp) do przechowywania wpisów tajnych użytkownika w pliku JSON w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-172">The Secret Manager uses the [File configuration provider](#fcp) to store user secrets in a JSON file on the local system.</span></span>

<span data-ttu-id="8e1b9-173">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) bezpieczne przechowywanie wpisów tajnych aplikacji dla ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-173">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="8e1b9-174">Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-174">For more information, see <xref:security/key-vault-configuration>.</span></span>

<a name="evcp"></a>

## <a name="environment-variables"></a><span data-ttu-id="8e1b9-175">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="8e1b9-175">Environment variables</span></span>

<span data-ttu-id="8e1b9-176">Korzystając z konfiguracji [domyślnej](#default) , <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> ładuje konfigurację ze par klucz-wartość zmiennej środowiskowej po odczytywaniu pliku *appSettings. JSON*, *appSettings.* `Environment` *. JSON*i [Menedżera wpisów tajnych](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-176">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs after reading *appsettings.json*, *appsettings.*`Environment`*.json*, and [Secret manager](xref:security/app-secrets).</span></span> <span data-ttu-id="8e1b9-177">W związku z tym kluczowe wartości odczytywane ze środowiska zastępują wartości odczytywane z pliku *appSettings. JSON*, *appSettings.* `Environment` *. JSON*i Menedżera wpisów tajnych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-177">Therefore, key values read from the environment override values read from *appsettings.json*, *appsettings.*`Environment`*.json*, and Secret manager.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="8e1b9-178">Następujące polecenia `set`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-178">The following `set` commands:</span></span>

* <span data-ttu-id="8e1b9-179">Ustaw klucze środowiska i wartości z [poprzedniego przykładu](#appsettingsjson) w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-179">Set the environment keys and values of the [preceding example](#appsettingsjson) on Windows.</span></span>
* <span data-ttu-id="8e1b9-180">Przetestuj ustawienia przy użyciu pobranego [przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-180">Test the settings when using the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample).</span></span> <span data-ttu-id="8e1b9-181">Polecenie `dotnet run` musi być uruchamiane w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-181">The `dotnet run` command must be run in the project directory.</span></span>

```dotnetcli
set MyKey="My key from Environment"
set Position__Title=Environment_Editor
set Position__Name=Environment_Rick
dotnet run
```

<span data-ttu-id="8e1b9-182">Poprzednie ustawienia środowiska:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-182">The preceding environment settings:</span></span>

* <span data-ttu-id="8e1b9-183">Są ustawiane tylko w ramach procesów uruchomionych z poziomu okna polecenia, które zostały ustawione w.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-183">Are only set in processes launched from the command window they were set in.</span></span>
* <span data-ttu-id="8e1b9-184">Nie będą odczytywane przez przeglądarki uruchomione przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-184">Won't be read by browsers launched with Visual Studio.</span></span>

<span data-ttu-id="8e1b9-185">Następujące polecenia [setx](/windows-server/administration/windows-commands/setx) mogą służyć do ustawiania kluczy środowiskowych i wartości w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-185">The following [setx](/windows-server/administration/windows-commands/setx) commands can be used to set the environment keys and values on Windows.</span></span> <span data-ttu-id="8e1b9-186">W przeciwieństwie do `set`ustawienia `setx` są utrwalane.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-186">Unlike `set`, `setx` settings are persisted.</span></span> <span data-ttu-id="8e1b9-187">`/M` ustawia zmienną w środowisku systemowym.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-187">`/M` sets the variable in the system environment.</span></span> <span data-ttu-id="8e1b9-188">Jeśli przełącznik `/M` nie jest używany, zmienna środowiskowa użytkownika zostanie ustawiona.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-188">If the `/M` switch isn't used, a user environment variable is set.</span></span>

```cmd
setx MyKey "My key from setx Environment" /M
setx Position__Title Setx_Environment_Editor /M
setx Position__Name Environment_Rick /M
```

<span data-ttu-id="8e1b9-189">Aby sprawdzić, czy poprzednie polecenia przesłaniają plik *appSettings. JSON* i *appSettings.* `Environment` *. JSON*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-189">To test that the preceding commands override *appsettings.json* and *appsettings.*`Environment`*.json*:</span></span>

* <span data-ttu-id="8e1b9-190">Za pomocą programu Visual Studio: Zamknij i uruchom ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-190">With Visual Studio: Exit and restart Visual Studio.</span></span>
* <span data-ttu-id="8e1b9-191">Za pomocą interfejsu wiersza polecenia: Uruchom nowe okno polecenia i wprowadź `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-191">With the CLI: Start a new command window and enter `dotnet run`.</span></span>

<span data-ttu-id="8e1b9-192">Wywołaj <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> z ciągiem, aby określić prefiks dla zmiennych środowiskowych:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-192">Call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> with a string to specify a prefix for environment variables:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet4&highlight=12)]

<span data-ttu-id="8e1b9-193">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-193">In the preceding code:</span></span>

* <span data-ttu-id="8e1b9-194">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")` jest dodawany po [domyślnych dostawcach konfiguracji](#default).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-194">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="8e1b9-195">Przykład określania kolejności dostawców konfiguracji można znaleźć w temacie [dostawca konfiguracji JSON](#jcp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-195">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>
* <span data-ttu-id="8e1b9-196">Zmienne środowiskowe ustawione przy użyciu prefiksu `MyCustomPrefix_` przesłaniają [domyślnych dostawców konfiguracji](#default).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-196">Environment variables set with the `MyCustomPrefix_` prefix override the [default configuration providers](#default).</span></span> <span data-ttu-id="8e1b9-197">Obejmuje to zmienne środowiskowe bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-197">This includes environment variables without the prefix.</span></span>

<span data-ttu-id="8e1b9-198">Prefiks jest usuwany, gdy pary klucz konfiguracji-wartość są odczytywane.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-198">The prefix is stripped off when the configuration key-value pairs are read.</span></span>

<span data-ttu-id="8e1b9-199">Następujące polecenia testują prefiks niestandardowy:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-199">The following commands test the custom prefix:</span></span>

```dotnetcli
set MyCustomPrefix_MyKey="My key with MyCustomPrefix_ Environment"
set MyCustomPrefix_Position__Title=Editor_with_customPrefix
set MyCustomPrefix_Position__Name=Environment_Rick_cp
dotnet run
```

<span data-ttu-id="8e1b9-200">[Konfiguracja domyślna](#default) ładuje zmienne środowiskowe i argumenty wiersza polecenia poprzedzone `DOTNET_` i `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-200">The [default configuration](#default) loads environment variables and command line arguments prefixed with `DOTNET_` and `ASPNETCORE_`.</span></span> <span data-ttu-id="8e1b9-201">`DOTNET_` i `ASPNETCORE_` prefiksy są używane przez ASP.NET Core do [konfiguracji hosta i aplikacji](xref:fundamentals/host/generic-host#host-configuration), ale nie do konfiguracji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-201">The `DOTNET_` and `ASPNETCORE_` prefixes are used by ASP.NET Core for [host and app configuration](xref:fundamentals/host/generic-host#host-configuration), but not for user configuration.</span></span> <span data-ttu-id="8e1b9-202">Aby uzyskać więcej informacji na temat konfiguracji hosta i aplikacji, zobacz [host ogólny programu .NET](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-202">For more information on host and app configuration, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

<span data-ttu-id="8e1b9-203">Na [Azure App Service](https://azure.microsoft.com/services/app-service/)wybierz pozycję **nowe ustawienie aplikacji** na stronie **Konfiguracja > Ustawienia** .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-203">On [Azure App Service](https://azure.microsoft.com/services/app-service/), select **New application setting** on the **Settings > Configuration** page.</span></span> <span data-ttu-id="8e1b9-204">Ustawienia aplikacji Azure App Service są następujące:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-204">Azure App Service application settings are:</span></span>

* <span data-ttu-id="8e1b9-205">Szyfrowane i przesyłane przez zaszyfrowanego kanału.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-205">Encrypted at rest and transmitted over an encrypted channel.</span></span>
* <span data-ttu-id="8e1b9-206">Uwidocznione jako zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-206">Exposed as environment variables.</span></span>

<span data-ttu-id="8e1b9-207">Aby uzyskać więcej informacji, zobacz [Azure Apps: Przesłoń konfigurację aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-207">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="8e1b9-208">Aby uzyskać informacje na temat parametrów połączenia z usługą Azure Database, zobacz [prefiksy parametrów połączenia](#constr) .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-208">See [Connection string prefixes](#constr) for information on Azure database connection strings.</span></span>

<a name="clcp"></a>

## <a name="command-line"></a><span data-ttu-id="8e1b9-209">Wiersz polecenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-209">Command-line</span></span>

<span data-ttu-id="8e1b9-210">Przy użyciu konfiguracji [domyślnej](#default) <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> ładuje konfigurację z par klucz-wartość argumentu wiersza polecenia po następujących źródłach konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-210">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs after the following configuration sources:</span></span>

* <span data-ttu-id="8e1b9-211">*appSettings. JSON* i *AppSettings*.`Environment`. pliki *JSON* .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-211">*appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="8e1b9-212">Wpisy [tajne aplikacji (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-212">[App secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="8e1b9-213">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-213">Environment variables.</span></span>

<span data-ttu-id="8e1b9-214">[Domyślnie](#default)wartości konfiguracji ustawione w wierszu polecenia przesłaniają wartości konfiguracyjne ustawione dla wszystkich innych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-214">By [default](#default), configuration values set on the command-line override configuration values set with all the other configuration providers.</span></span>

### <a name="command-line-arguments"></a><span data-ttu-id="8e1b9-215">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-215">Command-line arguments</span></span>

<span data-ttu-id="8e1b9-216">Następujące polecenie ustawia klucze i wartości przy użyciu `=`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-216">The following command sets keys and values using `=`:</span></span>

```dotnetcli
dotnet run MyKey="My key from command line" Position:Title=Cmd Position:Name=Cmd_Rick
```

<span data-ttu-id="8e1b9-217">Następujące polecenie ustawia klucze i wartości przy użyciu `/`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-217">The following command sets keys and values using `/`:</span></span>

```dotnetcli
dotnet run /MyKey "Using /" /Position:Title=Cmd_ /Position:Name=Cmd_Rick
```

<span data-ttu-id="8e1b9-218">Następujące polecenie ustawia klucze i wartości przy użyciu `--`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-218">The following command sets keys and values using `--`:</span></span>

```dotnetcli
dotnet run --MyKey "Using --" --Position:Title=Cmd-- --Position:Name=Cmd--Rick
```

<span data-ttu-id="8e1b9-219">Wartość klucza:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-219">The key value:</span></span>

* <span data-ttu-id="8e1b9-220">Należy przestrzegać `=`lub klucz musi mieć prefiks `--` lub `/`, gdy wartość znajduje się w miejscu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-220">Must follow `=`, or the key must have a prefix of `--` or `/` when the value follows a space.</span></span>
* <span data-ttu-id="8e1b9-221">Nie jest wymagane, jeśli użyto `=`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-221">Isn't required if `=` is used.</span></span> <span data-ttu-id="8e1b9-222">Na przykład `MySetting=`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-222">For example, `MySetting=`.</span></span>

<span data-ttu-id="8e1b9-223">W tym samym poleceniu nie należy mieszać par klucz-wartość argumentu wiersza polecenia, które używają `=` z parami klucz-wartość, które używają spacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-223">Within the same command, don't mix command-line argument key-value pairs that use `=` with key-value pairs that use a space.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="8e1b9-224">Mapowanie przełączników</span><span class="sxs-lookup"><span data-stu-id="8e1b9-224">Switch mappings</span></span>

<span data-ttu-id="8e1b9-225">Mapowania przełączników Zezwalaj na logikę zamiany nazwy **klucza** .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-225">Switch mappings allow **key** name replacement logic.</span></span> <span data-ttu-id="8e1b9-226">Udostępnij słownik przemieszczeń przełączeń do metody <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-226">Provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="8e1b9-227">Gdy jest używany słownik mapowania przełączników, słownik jest sprawdzany dla klucza, który pasuje do klucza dostarczonego przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-227">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="8e1b9-228">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika zostanie przeniesiona z powrotem, aby ustawić parę klucz-wartość w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-228">If the command-line key is found in the dictionary, the dictionary value is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="8e1b9-229">Mapowanie przełącznika jest wymagane dla każdego klucza wiersza polecenia poprzedzonego jedną kreską (`-`).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-229">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="8e1b9-230">Przełącz reguły klucza słownika mapowania:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-230">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="8e1b9-231">Przełączniki muszą zaczynać się od `-` lub `--`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-231">Switches must start with `-` or `--`.</span></span>
* <span data-ttu-id="8e1b9-232">Słownik mapowania przełącznika nie może zawierać zduplikowanych kluczy.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-232">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="8e1b9-233">Aby użyć słownika mapowania przełączników, przekaż go do wywołania `AddCommandLine`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-233">To use a switch mappings dictionary, pass it into the call to `AddCommandLine`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramSwitch.cs?name=snippet&highlight=10-18,23)]

<span data-ttu-id="8e1b9-234">Poniższy kod przedstawia wartości kluczy zastępowanych kluczy:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-234">The following code shows the key values for the replaced keys:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test3.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-235">Uruchom następujące polecenie, aby przetestować zastąpienie klucza:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-235">Run the following command to test the key replacement:</span></span>

```dotnetcli
dotnet run -k1=value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="8e1b9-236">Uwaga: Obecnie `=` nie można użyć do ustawiania wartości zastępczych klucza z jedną kreską `-`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-236">Note: Currently, `=` cannot be used to set key-replacement values with a single dash `-`.</span></span> <span data-ttu-id="8e1b9-237">Zobacz [ten problem](https://github.com/dotnet/extensions/issues/3059)w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-237">See [this GitHub issue](https://github.com/dotnet/extensions/issues/3059).</span></span>

<span data-ttu-id="8e1b9-238">Następujące polecenie działa w celu zastąpienia klucza testowego:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-238">The following command works to test key replacement:</span></span>

```dotnetcli
dotnet run -k1 value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="8e1b9-239">W przypadku aplikacji korzystających z mapowań przełączników wywołanie `CreateDefaultBuilder` nie powinno przekazywać argumentów.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-239">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="8e1b9-240">Wywołanie `AddCommandLine` metody `CreateDefaultBuilder` nie obejmuje zamapowanych przełączników i nie ma sposobu przekazywania słownika mapowania przełącznika do `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-240">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch-mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8e1b9-241">Rozwiązanie nie przekazuje argumentów do `CreateDefaultBuilder`, ale zamiast zezwalać `AddCommandLine` metody `ConfigurationBuilder` na przetwarzanie zarówno argumentów, jak i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-241">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch-mapping dictionary.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="8e1b9-242">Hierarchiczne dane konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-242">Hierarchical configuration data</span></span>

<span data-ttu-id="8e1b9-243">Interfejs API konfiguracji odczytuje hierarchiczne dane konfiguracji przez spłaszczonie danych hierarchicznych przy użyciu ogranicznika w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-243">The Configuration API is reads hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="8e1b9-244">[Pobieranie próbek](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) zawiera następujący plik *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="8e1b9-244">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="8e1b9-245">Poniższy kod z [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) wyświetla kilka ustawień konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-245">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-246">Preferowanym sposobem odczytywania hierarchicznych danych konfiguracji jest użycie wzorca opcji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-246">The preferred way to read hierarchical configuration data is using the options pattern.</span></span> <span data-ttu-id="8e1b9-247">Aby uzyskać więcej informacji, zobacz [Powiązywanie hierarchicznych danych konfiguracji](#optpat) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-247">For more information, see [Bind hierarchical configuration data](#optpat) in this document.</span></span>

<span data-ttu-id="8e1b9-248">Metody <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> są dostępne do izolowania sekcji i elementów podrzędnych sekcji w danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-248"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="8e1b9-249">Te metody są opisane w dalszej [części GetSection, GetChildren i EXISTS](#getsection).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-249">These methods are described later in [GetSection, GetChildren, and Exists](#getsection).</span></span>

<!--
[Azure Key Vault configuration provider](xref:security/key-vault-configuration) implement change detection.
-->

## <a name="configuration-keys-and-values"></a><span data-ttu-id="8e1b9-250">Klucze i wartości konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-250">Configuration keys and values</span></span>

<span data-ttu-id="8e1b9-251">Klucze konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-251">Configuration keys:</span></span>

* <span data-ttu-id="8e1b9-252">Bez uwzględniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-252">Are case-insensitive.</span></span> <span data-ttu-id="8e1b9-253">Na przykład `ConnectionString` i `connectionstring` są traktowane jako klucze równoważne.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-253">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="8e1b9-254">Jeśli klucz i wartość są ustawione w więcej niż jednym dostawcy konfiguracji, zostanie użyta wartość z ostatniego dodawanego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-254">If a key and value is set in more than one configuration providers, the value from the last provider added is used.</span></span> <span data-ttu-id="8e1b9-255">Aby uzyskać więcej informacji, zobacz [Konfiguracja domyślna](#default).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-255">For more information, see [Default configuration](#default).</span></span>
* <span data-ttu-id="8e1b9-256">Klucze hierarchiczne</span><span class="sxs-lookup"><span data-stu-id="8e1b9-256">Hierarchical keys</span></span>
  * <span data-ttu-id="8e1b9-257">W interfejsie API konfiguracji, separator dwukropek (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-257">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="8e1b9-258">W zmiennych środowiskowych separator dwukropek może nie zadziałał na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-258">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="8e1b9-259">Podwójne podkreślenie, `__`, jest obsługiwane przez wszystkie platformy i automatycznie konwertowane na dwu`:`kropek.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-259">A double underscore, `__`, is supported by all platforms and is automatically converted into a colon `:`.</span></span>
  * <span data-ttu-id="8e1b9-260">W Azure Key Vault klucze hierarchiczne używają `--` jako separatora.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-260">In Azure Key Vault, hierarchical keys use `--` as a separator.</span></span> <span data-ttu-id="8e1b9-261">Napisz kod, aby zastąpić `--` przy użyciu `:` po załadowaniu wpisów tajnych do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-261">Write code to replace the `--` with a `:` when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="8e1b9-262"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-262">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="8e1b9-263">Powiązanie tablicowe zostało opisane w sekcji [Powiązywanie tablicy z klasą](#boa) .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-263">Array binding is described in the [Bind an array to a class](#boa) section.</span></span>

<span data-ttu-id="8e1b9-264">Wartości konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-264">Configuration values:</span></span>

* <span data-ttu-id="8e1b9-265">Są ciągami.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-265">Are strings.</span></span>
* <span data-ttu-id="8e1b9-266">Wartości null nie można przechowywać w konfiguracji ani powiązana z obiektami.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-266">Null values can't be stored in configuration or bound to objects.</span></span>

<a name="cp"></a>

## <a name="configuration-providers"></a><span data-ttu-id="8e1b9-267">Dostawcy konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-267">Configuration providers</span></span>

<span data-ttu-id="8e1b9-268">W poniższej tabeli przedstawiono dostawców konfiguracji dostępnych do ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-268">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="8e1b9-269">Dostawca</span><span class="sxs-lookup"><span data-stu-id="8e1b9-269">Provider</span></span> | <span data-ttu-id="8e1b9-270">Zapewnia konfigurację z</span><span class="sxs-lookup"><span data-stu-id="8e1b9-270">Provides configuration from</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="8e1b9-271">Dostawca konfiguracji usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8e1b9-271">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration) | <span data-ttu-id="8e1b9-272">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8e1b9-272">Azure Key Vault</span></span> |
| [<span data-ttu-id="8e1b9-273">Dostawca konfiguracji aplikacji platformy Azure</span><span class="sxs-lookup"><span data-stu-id="8e1b9-273">Azure App configuration provider</span></span>](/azure/azure-app-configuration/quickstart-aspnet-core-app) | <span data-ttu-id="8e1b9-274">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="8e1b9-274">Azure App Configuration</span></span> |
| [<span data-ttu-id="8e1b9-275">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-275">Command-line configuration provider</span></span>](#clcp) | <span data-ttu-id="8e1b9-276">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-276">Command-line parameters</span></span> |
| [<span data-ttu-id="8e1b9-277">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-277">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="8e1b9-278">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="8e1b9-278">Custom source</span></span> |
| [<span data-ttu-id="8e1b9-279">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="8e1b9-279">Environment Variables configuration provider</span></span>](#evcp) | <span data-ttu-id="8e1b9-280">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="8e1b9-280">Environment variables</span></span> |
| [<span data-ttu-id="8e1b9-281">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="8e1b9-281">File configuration provider</span></span>](#file-configuration-provider) | <span data-ttu-id="8e1b9-282">Pliki INI, JSON i XML</span><span class="sxs-lookup"><span data-stu-id="8e1b9-282">INI, JSON, and XML files</span></span> |
| [<span data-ttu-id="8e1b9-283">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="8e1b9-283">Key-per-file configuration provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="8e1b9-284">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="8e1b9-284">Directory files</span></span> |
| [<span data-ttu-id="8e1b9-285">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="8e1b9-285">Memory configuration provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="8e1b9-286">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="8e1b9-286">In-memory collections</span></span> |
| [<span data-ttu-id="8e1b9-287">Menedżer wpisów tajnych</span><span class="sxs-lookup"><span data-stu-id="8e1b9-287">Secret Manager</span></span>](xref:security/app-secrets)  | <span data-ttu-id="8e1b9-288">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="8e1b9-288">File in the user profile directory</span></span> |

<span data-ttu-id="8e1b9-289">Źródła konfiguracji są odczytywane w kolejności, w jakiej zostały określone dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-289">Configuration sources are read in the order that their configuration providers are specified.</span></span> <span data-ttu-id="8e1b9-290">Zamów dostawców konfiguracji w kodzie, aby odpowiadały priorytetom źródłowych źródeł konfiguracji wymaganych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-290">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="8e1b9-291">Typową sekwencją dostawców konfiguracji jest:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-291">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="8e1b9-292">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="8e1b9-292">*appsettings.json*</span></span>
1. <span data-ttu-id="8e1b9-293">*AppSettings*.`Environment`. *kod JSON*</span><span class="sxs-lookup"><span data-stu-id="8e1b9-293">*appsettings*.`Environment`.*json*</span></span>
1. [<span data-ttu-id="8e1b9-294">Menedżer wpisów tajnych</span><span class="sxs-lookup"><span data-stu-id="8e1b9-294">Secret Manager</span></span>](xref:security/app-secrets)
1. <span data-ttu-id="8e1b9-295">Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#evcp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-295">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="8e1b9-296">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-296">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="8e1b9-297">Typowym celem jest dodanie dostawcy konfiguracji wiersza polecenia w ciągu kilku dostawców, aby zezwolić na argumenty wiersza polecenia, aby przesłonić konfigurację ustawioną przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-297">A common practice is to add the Command-line configuration provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="8e1b9-298">Poprzednia sekwencja dostawców jest używana w [konfiguracji domyślnej](#default).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-298">The preceding sequence of providers is used in the [default configuration](#default).</span></span>

<a name="constr"></a>

### <a name="connection-string-prefixes"></a><span data-ttu-id="8e1b9-299">Prefiksy parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-299">Connection string prefixes</span></span>

<span data-ttu-id="8e1b9-300">Interfejs API konfiguracji ma specjalne reguły przetwarzania dla czterech zmiennych środowiskowych parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-300">The Configuration API has special processing rules for four connection string environment variables.</span></span> <span data-ttu-id="8e1b9-301">Te parametry połączenia są związane z konfigurowaniem parametrów połączenia platformy Azure dla środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-301">These connection strings are involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="8e1b9-302">Zmienne środowiskowe z prefiksami podanymi w tabeli są ładowane do aplikacji z [konfiguracją domyślną](#default) lub jeśli nie podano prefiksu do `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-302">Environment variables with the prefixes shown in the table are loaded into the app with the [default configuration](#default) or when no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="8e1b9-303">Prefiks parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-303">Connection string prefix</span></span> | <span data-ttu-id="8e1b9-304">Dostawca</span><span class="sxs-lookup"><span data-stu-id="8e1b9-304">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="8e1b9-305">Dostawca niestandardowy</span><span class="sxs-lookup"><span data-stu-id="8e1b9-305">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="8e1b9-306">MySQL</span><span class="sxs-lookup"><span data-stu-id="8e1b9-306">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="8e1b9-307">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="8e1b9-307">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="8e1b9-308">SQL Server</span><span class="sxs-lookup"><span data-stu-id="8e1b9-308">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="8e1b9-309">Gdy zmienna środowiskowa zostanie odnaleziona i załadowana do konfiguracji z dowolnymi z czterech prefiksów pokazanych w tabeli:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-309">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="8e1b9-310">Klucz konfiguracji jest tworzony przez usunięcie prefiksu zmiennej środowiskowej i dodanie sekcji klucza konfiguracji (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-310">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="8e1b9-311">Zostanie utworzona nowa para klucz-wartość konfiguracji, która reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, które nie ma określonego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-311">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="8e1b9-312">Klucz zmiennej środowiskowej</span><span class="sxs-lookup"><span data-stu-id="8e1b9-312">Environment variable key</span></span> | <span data-ttu-id="8e1b9-313">Przekonwertowany klucz konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-313">Converted configuration key</span></span> | <span data-ttu-id="8e1b9-314">Wpis konfiguracji dostawcy</span><span class="sxs-lookup"><span data-stu-id="8e1b9-314">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="8e1b9-315">Wpis konfiguracji nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-315">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="8e1b9-316">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-316">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="8e1b9-317">Wartość:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-317">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="8e1b9-318">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-318">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="8e1b9-319">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-319">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="8e1b9-320">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-320">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="8e1b9-321">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-321">Value: `System.Data.SqlClient`</span></span>  |

<a name="jcp"></a>

### <a name="json-configuration-provider"></a><span data-ttu-id="8e1b9-322">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="8e1b9-322">JSON configuration provider</span></span>

<span data-ttu-id="8e1b9-323"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku JSON.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-323">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs.</span></span>

<span data-ttu-id="8e1b9-324">Przeciążenia mogą określać:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-324">Overloads can specify:</span></span>

* <span data-ttu-id="8e1b9-325">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-325">Whether the file is optional.</span></span>
* <span data-ttu-id="8e1b9-326">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-326">Whether the configuration is reloaded if the file changes.</span></span>

<span data-ttu-id="8e1b9-327">Rozważmy następujący kod:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-327">Consider the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON.cs?name=snippet&highlight=12-14)]

<span data-ttu-id="8e1b9-328">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-328">The preceding code:</span></span>

* <span data-ttu-id="8e1b9-329">Konfiguruje dostawcę konfiguracji JSON w celu załadowania pliku *. JSON* z następującymi opcjami:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-329">Configures the JSON configuration provider to load the *MyConfig.json* file with the following options:</span></span>
  * <span data-ttu-id="8e1b9-330">`optional: true`: Plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-330">`optional: true`: The file is optional.</span></span>
  * <span data-ttu-id="8e1b9-331">`reloadOnChange: true` : Plik zostanie ponownie załadowany po zapisaniu zmian.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-331">`reloadOnChange: true` : The file is reloaded when changes are saved.</span></span>
* <span data-ttu-id="8e1b9-332">Odczytuje [domyślnych dostawców konfiguracji](#default) przed plikiem moja *config. JSON* .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-332">Reads the [default configuration providers](#default) before the *MyConfig.json* file.</span></span> <span data-ttu-id="8e1b9-333">Ustawienia w pliku *config. JSON* przesłaniają ustawienie w domyślnych dostawcach konfiguracji, w tym [dostawcę konfiguracji zmienne środowiskowe](#evcp) i [dostawca konfiguracji wiersza polecenia](#clcp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-333">Settings in the *MyConfig.json* file override setting in the default configuration providers, including the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="8e1b9-334">Zwykle ***nie*** chcesz, aby niestandardowy plik JSON zastępujący wartości ustawione w [zmiennej środowiskowej dostawcy konfiguracji](#evcp) i [dostawcy konfiguracji wiersza polecenia](#clcp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-334">You typically ***don't*** want a custom JSON file overriding values set in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="8e1b9-335">Poniższy kod czyści wszystkich dostawców konfiguracji i dodaje kilku dostawców konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-335">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON2.cs?name=snippet)]

<span data-ttu-id="8e1b9-336">W powyższym kodzie ustawienia w *pliku config. JSON* i *konfiguracji*.`Environment`. pliki *JSON* :</span><span class="sxs-lookup"><span data-stu-id="8e1b9-336">In the preceding code, settings in the *MyConfig.json* and  *MyConfig*.`Environment`.*json* files:</span></span>

* <span data-ttu-id="8e1b9-337">Zastąp ustawienia w pliku *appSettings. JSON* i *AppSettings*.`Environment`. pliki *JSON* .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-337">Override settings in the *appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="8e1b9-338">Są zastępowane przez ustawienia [dostawcy konfiguracji zmiennych środowiskowych](#evcp) i [dostawcy konfiguracji wiersza polecenia](#clcp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-338">Are overridden by settings in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="8e1b9-339">[Pobieranie próbek](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) zawiera następujący plik *. JSON* :</span><span class="sxs-lookup"><span data-stu-id="8e1b9-339">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *MyConfig.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyConfig.json)]

<span data-ttu-id="8e1b9-340">Poniższy kod z [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) wyświetla kilka powyższych ustawień konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-340">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<a name="fcp"></a>

## <a name="file-configuration-provider"></a><span data-ttu-id="8e1b9-341">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="8e1b9-341">File configuration provider</span></span>

<span data-ttu-id="8e1b9-342"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową do ładowania konfiguracji z systemu plików.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-342"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="8e1b9-343">Następujący dostawcy konfiguracji pochodzą z `FileConfigurationProvider`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-343">The following configuration providers derive from `FileConfigurationProvider`:</span></span>

* [<span data-ttu-id="8e1b9-344">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="8e1b9-344">INI configuration provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="8e1b9-345">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="8e1b9-345">JSON configuration provider</span></span>](#jcp)
* [<span data-ttu-id="8e1b9-346">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="8e1b9-346">XML configuration provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="8e1b9-347">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="8e1b9-347">INI configuration provider</span></span>

<span data-ttu-id="8e1b9-348"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku INI w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-348">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="8e1b9-349">Poniższy kod czyści wszystkich dostawców konfiguracji i dodaje kilku dostawców konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-349">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramINI.cs?name=snippet&highlight=10-30)]

<span data-ttu-id="8e1b9-350">W powyższym kodzie ustawienia w *MyIniConfig. ini* i *MyIniConfig*.`Environment`. pliki *ini* są zastępowane przez ustawienia w:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-350">In the preceding code, settings in the *MyIniConfig.ini* and  *MyIniConfig*.`Environment`.*ini* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="8e1b9-351">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="8e1b9-351">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="8e1b9-352">[Dostawca konfiguracji wiersza polecenia](#clcp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-352">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="8e1b9-353">[Pobieranie próbek](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) zawiera następujący plik *MyIniConfig. ini* :</span><span class="sxs-lookup"><span data-stu-id="8e1b9-353">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyIniConfig.ini* file:</span></span>

[!code-ini[](index/samples/3.x/ConfigSample/MyIniConfig.ini)]

<span data-ttu-id="8e1b9-354">Poniższy kod z [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) wyświetla kilka powyższych ustawień konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-354">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

### <a name="xml-configuration-provider"></a><span data-ttu-id="8e1b9-355">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="8e1b9-355">XML configuration provider</span></span>

<span data-ttu-id="8e1b9-356"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-356">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="8e1b9-357">Poniższy kod czyści wszystkich dostawców konfiguracji i dodaje kilku dostawców konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-357">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramXML.cs?name=snippet)]

<span data-ttu-id="8e1b9-358">W powyższym kodzie ustawienia w *MyXMLFile. XML* i *MyXMLFile*.`Environment`. pliki *XML* są zastępowane przez ustawienia w:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-358">In the preceding code, settings in the *MyXMLFile.xml* and  *MyXMLFile*.`Environment`.*xml* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="8e1b9-359">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="8e1b9-359">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="8e1b9-360">[Dostawca konfiguracji wiersza polecenia](#clcp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-360">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="8e1b9-361">[Pobieranie próbek](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) zawiera następujący plik *MyXMLFile. XML* :</span><span class="sxs-lookup"><span data-stu-id="8e1b9-361">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyXMLFile.xml* file:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile.xml)]

<span data-ttu-id="8e1b9-362">Poniższy kod z [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) wyświetla kilka powyższych ustawień konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-362">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-363">Powtarzalne elementy, które używają tej samej nazwy elementu, działają, jeśli atrybut `name` jest używany do odróżnienia elementów:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-363">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile3.xml)]

<span data-ttu-id="8e1b9-364">Poniższy kod odczytuje poprzedni plik konfiguracji i wyświetla klucze i wartości:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-364">The following code reads the previous configuration file and displays the keys and values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/XML/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-365">Atrybuty mogą służyć do dostarczania wartości:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-365">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="8e1b9-366">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-366">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8e1b9-367">Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="8e1b9-367">key:attribute</span></span>
* <span data-ttu-id="8e1b9-368">sekcja: Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="8e1b9-368">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="8e1b9-369">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="8e1b9-369">Key-per-file configuration provider</span></span>

<span data-ttu-id="8e1b9-370"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> używa plików katalogu jako par klucz konfiguracji i wartość.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-370">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="8e1b9-371">Kluczem jest nazwa pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-371">The key is the file name.</span></span> <span data-ttu-id="8e1b9-372">Wartość zawiera zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-372">The value contains the file's contents.</span></span> <span data-ttu-id="8e1b9-373">Dostawca konfiguracji klucza dla plików jest używany w scenariuszach hostingu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-373">The Key-per-file configuration provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="8e1b9-374">Aby uaktywnić konfigurację klucza dla plików, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-374">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="8e1b9-375">`directoryPath` plików musi być ścieżką bezwzględną.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-375">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="8e1b9-376">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-376">Overloads permit specifying:</span></span>

* <span data-ttu-id="8e1b9-377">Delegat `Action<KeyPerFileConfigurationSource>`, który konfiguruje źródło.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-377">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="8e1b9-378">Określa, czy katalog jest opcjonalny, i ścieżkę do katalogu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-378">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="8e1b9-379">Podwójny znak podkreślenia (`__`) jest używany jako ogranicznik klucza konfiguracji w nazwach plików.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-379">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="8e1b9-380">Na przykład nazwa pliku `Logging__LogLevel__System` generuje klucz konfiguracji `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-380">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="8e1b9-381">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-381">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

<a name="mcp"></a>

## <a name="memory-configuration-provider"></a><span data-ttu-id="8e1b9-382">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="8e1b9-382">Memory configuration provider</span></span>

<span data-ttu-id="8e1b9-383"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> używa kolekcji w pamięci jako par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-383">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="8e1b9-384">Poniższy kod dodaje kolekcję pamięci do systemu konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-384">The following code adds a memory collection to the configuration system:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet6)]

<span data-ttu-id="8e1b9-385">Poniższy kod z [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) wyświetla poprzednie ustawienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-385">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-386">W poprzednim kodzie `config.AddInMemoryCollection(Dict)` jest dodawany po [domyślnych dostawcach konfiguracji](#default).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-386">In the preceding code, `config.AddInMemoryCollection(Dict)` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="8e1b9-387">Przykład określania kolejności dostawców konfiguracji można znaleźć w temacie [dostawca konfiguracji JSON](#jcp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-387">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="8e1b9-388">Przykład określania kolejności dostawców konfiguracji można znaleźć w temacie [dostawca konfiguracji JSON](#jcp).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-388">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="8e1b9-389">Zobacz [Powiąż tablicę](#boa) z innym przykładem przy użyciu `MemoryConfigurationProvider`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-389">See [Bind an array](#boa) for another example using `MemoryConfigurationProvider`.</span></span>

## <a name="getvalue"></a><span data-ttu-id="8e1b9-390">GetValue</span><span class="sxs-lookup"><span data-stu-id="8e1b9-390">GetValue</span></span>

<span data-ttu-id="8e1b9-391">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnia jedną wartość z konfiguracji z określonym kluczem i konwertuje ją na określony typ:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-391">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified type:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestNum.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-392">Jeśli w powyższym kodzie nie znaleziono `NumberKey`, zostanie użyta wartość domyślna `99`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-392">In the preceding code,  if `NumberKey` isn't found in the configuration, the default value of `99` is used.</span></span>

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="8e1b9-393">GetSection, GetChildren i EXISTS</span><span class="sxs-lookup"><span data-stu-id="8e1b9-393">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="8e1b9-394">W poniższych przykładach należy wziąć pod uwagę następujący plik *MySubsection. JSON* :</span><span class="sxs-lookup"><span data-stu-id="8e1b9-394">For the examples that follow, consider the following *MySubsection.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MySubsection.json)]

<span data-ttu-id="8e1b9-395">Poniższy kod dodaje *MySubsection. JSON* do dostawców konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-395">The following code adds *MySubsection.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONsection.cs?name=snippet)]

### <a name="getsection"></a><span data-ttu-id="8e1b9-396">GetSection</span><span class="sxs-lookup"><span data-stu-id="8e1b9-396">GetSection</span></span>

<span data-ttu-id="8e1b9-397">[IConfiguration. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) zwraca podsekcję konfiguracji z określonym kluczem podsekcji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-397">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) returns a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="8e1b9-398">Poniższy kod zwraca wartości dla `section1`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-398">The following code returns values for `section1`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-399">Poniższy kod zwraca wartości dla `section2:subsection0`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-399">The following code returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection2.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-400">`GetSection` nigdy nie zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-400">`GetSection` never returns `null`.</span></span> <span data-ttu-id="8e1b9-401">Jeśli nie znaleziono pasującej sekcji, zwracany jest pusty `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-401">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="8e1b9-402">Gdy `GetSection` zwraca pasującą sekcję, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-402">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="8e1b9-403"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> i <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> są zwracane, gdy istnieje sekcja.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-403">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren-and-exists"></a><span data-ttu-id="8e1b9-404">GetChildren i istnieje</span><span class="sxs-lookup"><span data-stu-id="8e1b9-404">GetChildren and Exists</span></span>

<span data-ttu-id="8e1b9-405">Poniższy kod wywołuje [iConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) i zwraca wartości dla `section2:subsection0`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-405">The following code calls [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) and returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection4.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-406">Poprzedni kod wywołuje [ConfigurationExtensions. Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) , aby sprawdzić, czy sekcja istnieje:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-406">The preceding code calls [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to verify the  section exists:</span></span>

 <a name="boa"></a>

## <a name="bind-an-array"></a><span data-ttu-id="8e1b9-407">Powiąż tablicę</span><span class="sxs-lookup"><span data-stu-id="8e1b9-407">Bind an array</span></span>

<span data-ttu-id="8e1b9-408">[ConfigurationBinder. bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) obsługuje tablice powiązań z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-408">The [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="8e1b9-409">Każdy format tablicy, który ujawnia segment klucza numerycznego, jest w stanie powiązać powiązanie tablicy z tablicą klas [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-409">Any array format that exposes a numeric key segment is capable of array binding to a [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) class array.</span></span>

<span data-ttu-id="8e1b9-410">Weź pod uwagę plik *webarray. JSON* z [przykładowego pobrania](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample):</span><span class="sxs-lookup"><span data-stu-id="8e1b9-410">Consider *MyArray.json* from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample):</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyArray.json)]

<span data-ttu-id="8e1b9-411">Poniższy kod dodaje plik *webarray. JSON* do dostawców konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-411">The following code adds *MyArray.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONarray.cs?name=snippet)]

<span data-ttu-id="8e1b9-412">Poniższy kod odczytuje konfigurację i wyświetla wartości:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-412">The following code reads the configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-413">Poprzedni kod zwraca następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-413">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value00
Index: 1  Value: value10
Index: 2  Value: value20
Index: 3  Value: value40
Index: 4  Value: value50
```

<span data-ttu-id="8e1b9-414">W poprzednich danych wyjściowych indeks 3 ma wartość `value40`, odpowiadając `"4": "value40",` w pliku *webarray. JSON*.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-414">In the preceding output, Index 3 has value `value40`, corresponding to `"4": "value40",` in *MyArray.json*.</span></span> <span data-ttu-id="8e1b9-415">Powiązane indeksy tablicy są ciągłe i nie są powiązane z indeksem klucza konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-415">The bound array indices are continuous and not bound to the configuration key index.</span></span> <span data-ttu-id="8e1b9-416">Obiekt tworzący konfigurację nie jest w stanie powiązać wartości null ani tworzyć wpisów o wartości null dla obiektów powiązanych</span><span class="sxs-lookup"><span data-stu-id="8e1b9-416">The configuration binder isn't capable of binding null values or creating null entries in bound objects</span></span>

<span data-ttu-id="8e1b9-417">Poniższy kod ładuje konfigurację `array:entries` przy użyciu metody rozszerzenia <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*>:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-417">The  following code loads the `array:entries` configuration with the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet)]

<span data-ttu-id="8e1b9-418">Poniższy kod odczytuje konfigurację w `arrayDict` `Dictionary` i wyświetla wartości:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-418">The following code reads the configuration in the `arrayDict` `Dictionary` and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-419">Poprzedni kod zwraca następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-419">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value4
Index: 4  Value: value5
```

<span data-ttu-id="8e1b9-420">Indeks &num;3 w obiekcie powiązanym zawiera dane konfiguracyjne `array:4` klucza konfiguracji i jego wartość `value4`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-420">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="8e1b9-421">Gdy dane konfiguracji zawierające tablicę są powiązane, indeksy tablic w kluczach konfiguracji są używane do iteracji danych konfiguracji podczas tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-421">When configuration data containing an array is bound, the array indices in the configuration keys are used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="8e1b9-422">Wartości null nie można zachować w danych konfiguracyjnych, a wpis o wartości null nie jest tworzony w obiekcie powiązanym, gdy tablica w kluczach konfiguracji pomija jeden lub więcej indeksów.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-422">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="8e1b9-423">Brakujący element konfiguracji dla indeksu &num;3 można dostarczyć przed powiązaniem do wystąpienia `ArrayExample` przez dowolnego dostawcę konfiguracji, który odczytuje parę klucz-wartość indeksu &num;3.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-423">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that reads the index &num;3 key/value pair.</span></span> <span data-ttu-id="8e1b9-424">Rozważmy następujący plik *Wartość3. JSON* z przykładowego pobrania:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-424">Consider the following *Value3.json* file from the sample download:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/Value3.json)]

<span data-ttu-id="8e1b9-425">Poniższy kod zawiera konfigurację dla *Wartość3. JSON* i `Dictionary``arrayDict`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-425">The following code includes configuration for *Value3.json* and the `arrayDict` `Dictionary`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet2)]

<span data-ttu-id="8e1b9-426">Poniższy kod odczytuje poprzednią konfigurację i wyświetla wartości:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-426">The following code reads the preceding configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="8e1b9-427">Poprzedni kod zwraca następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-427">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value3
Index: 4  Value: value4
Index: 5  Value: value5
```

<span data-ttu-id="8e1b9-428">Niestandardowi dostawcy konfiguracji nie muszą implementować powiązania tablicy.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-428">Custom configuration providers aren't required to implement array binding.</span></span>

## <a name="custom-configuration-provider"></a><span data-ttu-id="8e1b9-429">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-429">Custom configuration provider</span></span>

<span data-ttu-id="8e1b9-430">Przykładowa aplikacja pokazuje, jak utworzyć podstawowego dostawcę konfiguracji, który odczytuje pary klucz-wartość konfiguracji z bazy danych przy użyciu [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-430">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="8e1b9-431">Dostawca ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-431">The provider has the following characteristics:</span></span>

* <span data-ttu-id="8e1b9-432">Baza danych EF w pamięci jest używana w celach demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-432">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="8e1b9-433">Aby użyć bazy danych, która wymaga parametrów połączenia, zaimplementuj pomocniczą `ConfigurationBuilder`, aby podać parametry połączenia od innego dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-433">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="8e1b9-434">Dostawca odczytuje tabelę bazy danych w konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-434">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="8e1b9-435">Dostawca nie wykonuje zapytania do bazy danych w oparciu o klucz.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-435">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="8e1b9-436">Ponowne załadowanie nie zostało zaimplementowane, więc aktualizacja bazy danych po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-436">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="8e1b9-437">Zdefiniuj jednostkę `EFConfigurationValue` do przechowywania wartości konfiguracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-437">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="8e1b9-438">*Modele/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-438">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="8e1b9-439">Dodaj `EFConfigurationContext` do przechowywania skonfigurowanych wartości i uzyskiwania do nich dostępu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-439">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="8e1b9-440">*EFConfigurationProvider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-440">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="8e1b9-441">Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-441">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="8e1b9-442">*EFConfigurationProvider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-442">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="8e1b9-443">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-443">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="8e1b9-444">Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-444">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="8e1b9-445">Ponieważ w [kluczach konfiguracji jest rozróżniana wielkość liter](#keys), słownik używany do inicjowania bazy danych jest tworzony przy użyciu funkcji porównującej bez uwzględniania wielkości liter ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-445">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="8e1b9-446">*EFConfigurationProvider/EFConfigurationProvider. cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-446">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="8e1b9-447">Metoda rozszerzenia `AddEFConfiguration` zezwala na Dodawanie źródła konfiguracji do `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-447">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="8e1b9-448">*Rozszerzenia/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-448">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="8e1b9-449">Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-449">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

<a name="acs"></a>

## <a name="access-configuration-in-startup"></a><span data-ttu-id="8e1b9-450">Konfiguracja dostępu w programie startowym</span><span class="sxs-lookup"><span data-stu-id="8e1b9-450">Access configuration in Startup</span></span>

<span data-ttu-id="8e1b9-451">Poniższy kod przedstawia dane konfiguracji w metodach `Startup`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-451">The following code displays configuration data in `Startup` methods:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/StartupKey.cs?name=snippet&highlight=13,18)]

<span data-ttu-id="8e1b9-452">Aby zapoznać się z przykładem uzyskiwania dostępu do konfiguracji przy użyciu metod uruchamiania, zobacz [uruchamiania aplikacji: Wygodne metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-452">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-razor-pages"></a><span data-ttu-id="8e1b9-453">Konfiguracja dostępu w Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8e1b9-453">Access configuration in Razor Pages</span></span>

<span data-ttu-id="8e1b9-454">Poniższy kod przedstawia dane konfiguracji na stronie Razor:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-454">The following code displays configuration data in a Razor Page:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Pages/Test5.cshtml)]

## <a name="access-configuration-in-a-mvc-view-file"></a><span data-ttu-id="8e1b9-455">Konfiguracja dostępu w pliku widoku MVC</span><span class="sxs-lookup"><span data-stu-id="8e1b9-455">Access configuration in a MVC view file</span></span>

<span data-ttu-id="8e1b9-456">Poniższy kod przedstawia dane konfiguracji w widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-456">The following code displays configuration data in a MVC view:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Views/Home2/Index.cshtml)]

<a name="hvac"></a>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="8e1b9-457">Host a konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-457">Host versus app configuration</span></span>

<span data-ttu-id="8e1b9-458">Przed skonfigurowaniem i uruchomieniem aplikacji *host* zostanie skonfigurowany i uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-458">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="8e1b9-459">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-459">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="8e1b9-460">Zarówno aplikacja, jak i Host są konfigurowane przy użyciu dostawców konfiguracji opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-460">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="8e1b9-461">Klucz konfiguracji hosta — pary wartości są również uwzględnione w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-461">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="8e1b9-462">Aby uzyskać więcej informacji na temat tego, jak dostawcy konfiguracji są używani podczas kompilowania hosta i jak źródła konfiguracji wpływają na konfigurację hosta, zobacz <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-462">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

<a name="dhc"></a>

## <a name="default-host-configuration"></a><span data-ttu-id="8e1b9-463">Domyślna konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="8e1b9-463">Default host configuration</span></span>

<span data-ttu-id="8e1b9-464">Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej podczas korzystania z [hosta sieci Web](xref:fundamentals/host/web-host), zobacz [wersję ASP.NET Core 2,2 tego tematu](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-464">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="8e1b9-465">Konfiguracja hosta jest poświadczona z:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-465">Host configuration is provided from:</span></span>
  * <span data-ttu-id="8e1b9-466">Zmienne środowiskowe poprzedzone prefiksem `DOTNET_` (na przykład `DOTNET_ENVIRONMENT`) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-466">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables configuration provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="8e1b9-467">Prefiks (`DOTNET_`) jest usuwany podczas ładowania par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-467">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="8e1b9-468">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-468">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="8e1b9-469">Konfiguracja domyślna hosta sieci Web (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="8e1b9-469">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="8e1b9-470">Kestrel jest używany jako serwer sieci Web i konfigurowany przy użyciu dostawców konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-470">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="8e1b9-471">Dodaj oprogramowanie pośredniczące do filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-471">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="8e1b9-472">Dodaj przekierowane nagłówki — oprogramowanie pośredniczące, jeśli zmienna środowiskowa `ASPNETCORE_FORWARDEDHEADERS_ENABLED` jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-472">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="8e1b9-473">Włącz integrację usług IIS.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-473">Enable IIS integration.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="8e1b9-474">Inna konfiguracja</span><span class="sxs-lookup"><span data-stu-id="8e1b9-474">Other configuration</span></span>

<span data-ttu-id="8e1b9-475">Ten temat dotyczy tylko *konfiguracji aplikacji*.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-475">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="8e1b9-476">Inne aspekty uruchamiania i hostowania aplikacji ASP.NET Core są konfigurowane przy użyciu plików konfiguracji nieuwzględnionych w tym temacie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-476">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="8e1b9-477">plik *Launch. json*/*profilu launchsettings. JSON* to pliki konfiguracji narzędzi dla środowiska programistycznego, opisane w temacie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-477">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="8e1b9-478">W <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-478">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="8e1b9-479">W zestawie dokumentacji, w której pliki są używane do konfigurowania ASP.NET Core aplikacji na potrzeby scenariuszy programistycznych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-479">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="8e1b9-480">*Web. config* to plik konfiguracji serwera opisany w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-480">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="8e1b9-481">Aby uzyskać więcej informacji na temat migrowania konfiguracji aplikacji z wcześniejszych wersji programu ASP.NET, zobacz <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-481">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="8e1b9-482">Dodawanie konfiguracji z zestawu zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="8e1b9-482">Add configuration from an external assembly</span></span>

<span data-ttu-id="8e1b9-483">Implementacja <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zestawu zewnętrznego poza klasą `Startup` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-483">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="8e1b9-484">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-484">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e1b9-485">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8e1b9-485">Additional resources</span></span>

* [<span data-ttu-id="8e1b9-486">Kod źródłowy konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-486">Configuration source code</span></span>](https://github.com/dotnet/extensions/tree/master/src/Configuration)
* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8e1b9-487">Konfiguracja aplikacji w ASP.NET Core jest oparta na parach klucz-wartość określonych przez *dostawców konfiguracji*.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-487">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="8e1b9-488">Dostawcy konfiguracji odczytują dane konfiguracji do par klucz-wartość z różnych źródeł konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-488">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="8e1b9-489">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8e1b9-489">Azure Key Vault</span></span>
* <span data-ttu-id="8e1b9-490">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="8e1b9-490">Azure App Configuration</span></span>
* <span data-ttu-id="8e1b9-491">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-491">Command-line arguments</span></span>
* <span data-ttu-id="8e1b9-492">Dostawcy niestandardowi (instalowani lub utworzony)</span><span class="sxs-lookup"><span data-stu-id="8e1b9-492">Custom providers (installed or created)</span></span>
* <span data-ttu-id="8e1b9-493">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="8e1b9-493">Directory files</span></span>
* <span data-ttu-id="8e1b9-494">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="8e1b9-494">Environment variables</span></span>
* <span data-ttu-id="8e1b9-495">Obiekty w pamięci .NET</span><span class="sxs-lookup"><span data-stu-id="8e1b9-495">In-memory .NET objects</span></span>
* <span data-ttu-id="8e1b9-496">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="8e1b9-496">Settings files</span></span>

<span data-ttu-id="8e1b9-497">Pakiety konfiguracyjne dla typowych scenariuszy dostawcy konfiguracji ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-497">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="8e1b9-498">Przykłady kodu, które obserwują i w przykładowej aplikacji używają przestrzeni nazw <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-498">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="8e1b9-499">*Wzorzec opcji* jest rozszerzeniem pojęć konfiguracyjnych opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-499">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="8e1b9-500">Opcje używają klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-500">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="8e1b9-501">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-501">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="8e1b9-502">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8e1b9-502">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="8e1b9-503">Host a konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-503">Host versus app configuration</span></span>

<span data-ttu-id="8e1b9-504">Przed skonfigurowaniem i uruchomieniem aplikacji *host* zostanie skonfigurowany i uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-504">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="8e1b9-505">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-505">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="8e1b9-506">Zarówno aplikacja, jak i Host są konfigurowane przy użyciu dostawców konfiguracji opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-506">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="8e1b9-507">Klucz konfiguracji hosta — pary wartości są również uwzględnione w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-507">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="8e1b9-508">Aby uzyskać więcej informacji na temat tego, jak dostawcy konfiguracji są używani podczas kompilowania hosta i jak źródła konfiguracji wpływają na konfigurację hosta, zobacz <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-508">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="8e1b9-509">Inna konfiguracja</span><span class="sxs-lookup"><span data-stu-id="8e1b9-509">Other configuration</span></span>

<span data-ttu-id="8e1b9-510">Ten temat dotyczy tylko *konfiguracji aplikacji*.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-510">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="8e1b9-511">Inne aspekty uruchamiania i hostowania aplikacji ASP.NET Core są konfigurowane przy użyciu plików konfiguracji nieuwzględnionych w tym temacie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-511">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="8e1b9-512">plik *Launch. json*/*profilu launchsettings. JSON* to pliki konfiguracji narzędzi dla środowiska programistycznego, opisane w temacie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-512">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="8e1b9-513">W <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-513">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="8e1b9-514">W zestawie dokumentacji, w której pliki są używane do konfigurowania ASP.NET Core aplikacji na potrzeby scenariuszy programistycznych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-514">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="8e1b9-515">*Web. config* to plik konfiguracji serwera opisany w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-515">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="8e1b9-516">Aby uzyskać więcej informacji na temat migrowania konfiguracji aplikacji z wcześniejszych wersji programu ASP.NET, zobacz <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-516">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="8e1b9-517">Konfiguracja domyślna</span><span class="sxs-lookup"><span data-stu-id="8e1b9-517">Default configuration</span></span>

<span data-ttu-id="8e1b9-518">Aplikacje sieci Web oparte na ASP.NET Core [nowym szablonów dotnet](/dotnet/core/tools/dotnet-new) <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-518">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="8e1b9-519">`CreateDefaultBuilder` zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-519">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="8e1b9-520">Poniższe zasady dotyczą aplikacji korzystających z [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-520">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="8e1b9-521">Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej w przypadku korzystania z [hosta ogólnego](xref:fundamentals/host/generic-host), zobacz [najnowszą wersję tego tematu](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-521">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="8e1b9-522">Konfiguracja hosta jest poświadczona z:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-522">Host configuration is provided from:</span></span>
  * <span data-ttu-id="8e1b9-523">Zmienne środowiskowe poprzedzone prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-523">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="8e1b9-524">Prefiks (`ASPNETCORE_`) jest usuwany podczas ładowania par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-524">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="8e1b9-525">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-525">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="8e1b9-526">Podano konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-526">App configuration is provided from:</span></span>
  * <span data-ttu-id="8e1b9-527">*appSettings. JSON* przy użyciu [dostawcy konfiguracji plików](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-527">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="8e1b9-528">*appSettings. {Environment}. JSON* przy użyciu [dostawcy konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-528">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="8e1b9-529">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development` przy użyciu zestawu wpisów.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-529">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="8e1b9-530">Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-530">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="8e1b9-531">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-531">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="8e1b9-532">Bezpieczeństwo</span><span class="sxs-lookup"><span data-stu-id="8e1b9-532">Security</span></span>

<span data-ttu-id="8e1b9-533">Aby zabezpieczyć poufne dane konfiguracji, należy zastosować następujące rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-533">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="8e1b9-534">Nie należy przechowywać haseł ani innych danych poufnych w kodzie dostawcy konfiguracji ani w plikach konfiguracji zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-534">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="8e1b9-535">Nie używaj tajemnic produkcyjnych w środowiskach deweloperskich i testowych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-535">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="8e1b9-536">Określ wpisy tajne poza projektem, aby nie mogły zostać przypadkowo przekazane do repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-536">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="8e1b9-537">Więcej informacji znajduje się w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-537">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="8e1b9-538"><xref:security/app-secrets> &ndash; zawiera porady dotyczące używania zmiennych środowiskowych do przechowywania poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-538"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="8e1b9-539">Menedżer wpisów tajnych używa dostawcy konfiguracji plików do przechowywania wpisów tajnych użytkownika w pliku JSON w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-539">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="8e1b9-540">Dostawca konfiguracji plików został opisany w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-540">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="8e1b9-541">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) bezpieczne przechowywanie wpisów tajnych aplikacji dla ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-541">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="8e1b9-542">Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-542">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="8e1b9-543">Hierarchiczne dane konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-543">Hierarchical configuration data</span></span>

<span data-ttu-id="8e1b9-544">Interfejs API konfiguracji jest w stanie utrzymywać hierarchiczne dane konfiguracji przez spłaszczonie danych hierarchicznych przy użyciu ogranicznika w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-544">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="8e1b9-545">W poniższym pliku JSON cztery klucze istnieją w hierarchii strukturalnej dwóch sekcji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-545">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="8e1b9-546">Gdy plik jest odczytywany do konfiguracji, są tworzone unikatowe klucze, aby zachować oryginalną hierarchiczną strukturę danych źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-546">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="8e1b9-547">Sekcje i klucze są spłaszczone przy użyciu dwukropka (`:`), aby zachować oryginalną strukturę:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-547">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="8e1b9-548">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-548">section0:key0</span></span>
* <span data-ttu-id="8e1b9-549">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-549">section0:key1</span></span>
* <span data-ttu-id="8e1b9-550">section1:key0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-550">section1:key0</span></span>
* <span data-ttu-id="8e1b9-551">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-551">section1:key1</span></span>

<span data-ttu-id="8e1b9-552">Metody <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> są dostępne do izolowania sekcji i elementów podrzędnych sekcji w danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-552"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="8e1b9-553">Te metody są opisane w dalszej [części GetSection, GetChildren i EXISTS](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-553">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="8e1b9-554">Konwencje</span><span class="sxs-lookup"><span data-stu-id="8e1b9-554">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="8e1b9-555">Źródła i dostawcy</span><span class="sxs-lookup"><span data-stu-id="8e1b9-555">Sources and providers</span></span>

<span data-ttu-id="8e1b9-556">Podczas uruchamiania aplikacji źródła konfiguracji są odczytywane w kolejności, w jakiej są określone przez ich dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-556">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="8e1b9-557">Dostawcy konfiguracji implementujący funkcję wykrywania zmian mają możliwość ponownego załadowania konfiguracji, gdy ustawienie podstawowe zostanie zmienione.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-557">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="8e1b9-558">Na przykład dostawca konfiguracji plików (opisany w dalszej części tego tematu) i [dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) implementują wykrywanie zmian.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-558">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="8e1b9-559"><xref:Microsoft.Extensions.Configuration.IConfiguration> jest dostępny w kontenerze [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-559"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="8e1b9-560"><xref:Microsoft.Extensions.Configuration.IConfiguration> można wstrzyknąć do Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> lub MVC <xref:Microsoft.AspNetCore.Mvc.Controller>, aby uzyskać konfigurację dla klasy.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-560"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="8e1b9-561">W poniższych przykładach pole `_config` służy do uzyskiwania dostępu do wartości konfiguracyjnych:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-561">In the following examples, the `_config` field is used to access configuration values:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

<span data-ttu-id="8e1b9-562">Dostawcy konfiguracji nie mogą używać DI, ponieważ są niedostępne, gdy są skonfigurowane przez hosta.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-562">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="8e1b9-563">Klucze</span><span class="sxs-lookup"><span data-stu-id="8e1b9-563">Keys</span></span>

<span data-ttu-id="8e1b9-564">Klucze konfiguracji przyjmują następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-564">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="8e1b9-565">W kluczach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-565">Keys are case-insensitive.</span></span> <span data-ttu-id="8e1b9-566">Na przykład `ConnectionString` i `connectionstring` są traktowane jako klucze równoważne.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-566">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="8e1b9-567">Jeśli wartość tego samego klucza jest ustawiana przez tych samych lub różnych dostawców konfiguracji, Ostatnia wartość ustawiona w tym kluczu jest używana.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-567">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="8e1b9-568">Klucze hierarchiczne</span><span class="sxs-lookup"><span data-stu-id="8e1b9-568">Hierarchical keys</span></span>
  * <span data-ttu-id="8e1b9-569">W interfejsie API konfiguracji, separator dwukropek (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-569">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="8e1b9-570">W zmiennych środowiskowych separator dwukropek może nie zadziałał na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-570">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="8e1b9-571">Podwójne podkreślenie (`__`) jest obsługiwane przez wszystkie platformy i automatycznie konwertowane na dwukropek.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-571">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="8e1b9-572">W Azure Key Vault klucze hierarchiczne używają `--` (dwóch kresek) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-572">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="8e1b9-573">Napisz kod, aby zastąpić łączniki dwukropkiem, gdy wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-573">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="8e1b9-574"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-574">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="8e1b9-575">Powiązanie tablicowe zostało opisane w sekcji [Powiązywanie tablicy z klasą](#bind-an-array-to-a-class) .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-575">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="8e1b9-576">Wartości</span><span class="sxs-lookup"><span data-stu-id="8e1b9-576">Values</span></span>

<span data-ttu-id="8e1b9-577">Wartości konfiguracyjne przyjmują następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-577">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="8e1b9-578">Wartości są ciągami.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-578">Values are strings.</span></span>
* <span data-ttu-id="8e1b9-579">Wartości null nie można przechowywać w konfiguracji ani powiązana z obiektami.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-579">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="8e1b9-580">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="8e1b9-580">Providers</span></span>

<span data-ttu-id="8e1b9-581">W poniższej tabeli przedstawiono dostawców konfiguracji dostępnych do ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-581">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="8e1b9-582">Dostawca</span><span class="sxs-lookup"><span data-stu-id="8e1b9-582">Provider</span></span> | <span data-ttu-id="8e1b9-583">Zapewnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="8e1b9-583">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="8e1b9-584">[Dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) (tematy dotyczące*zabezpieczeń* )</span><span class="sxs-lookup"><span data-stu-id="8e1b9-584">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="8e1b9-585">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8e1b9-585">Azure Key Vault</span></span> |
| <span data-ttu-id="8e1b9-586">[Dostawca konfiguracji aplikacji platformy Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (dokumentacja platformy Azure)</span><span class="sxs-lookup"><span data-stu-id="8e1b9-586">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="8e1b9-587">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="8e1b9-587">Azure App Configuration</span></span> |
| [<span data-ttu-id="8e1b9-588">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-588">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="8e1b9-589">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-589">Command-line parameters</span></span> |
| [<span data-ttu-id="8e1b9-590">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-590">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="8e1b9-591">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="8e1b9-591">Custom source</span></span> |
| [<span data-ttu-id="8e1b9-592">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="8e1b9-592">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="8e1b9-593">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="8e1b9-593">Environment variables</span></span> |
| [<span data-ttu-id="8e1b9-594">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="8e1b9-594">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="8e1b9-595">Pliki (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="8e1b9-595">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="8e1b9-596">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="8e1b9-596">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="8e1b9-597">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="8e1b9-597">Directory files</span></span> |
| [<span data-ttu-id="8e1b9-598">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="8e1b9-598">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="8e1b9-599">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="8e1b9-599">In-memory collections</span></span> |
| <span data-ttu-id="8e1b9-600">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tematy dotyczące*zabezpieczeń* )</span><span class="sxs-lookup"><span data-stu-id="8e1b9-600">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="8e1b9-601">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="8e1b9-601">File in the user profile directory</span></span> |

<span data-ttu-id="8e1b9-602">Źródła konfiguracji są odczytywane w kolejności, w jakiej dostawcy konfiguracji są określeni podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-602">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="8e1b9-603">Dostawcy konfiguracji opisane w tym temacie są opisane w kolejności alfabetycznej, a nie w kolejności, w jakiej kod ich rozmieszcza.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-603">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="8e1b9-604">Zamów dostawców konfiguracji w kodzie, aby odpowiadały priorytetom źródłowych źródeł konfiguracji wymaganych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-604">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="8e1b9-605">Typową sekwencją dostawców konfiguracji jest:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-605">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="8e1b9-606">Pliki (*appSettings. JSON*, *appSettings. { Environment}. JSON*, gdzie `{Environment}` jest bieżącym środowiskiem hostingu aplikacji)</span><span class="sxs-lookup"><span data-stu-id="8e1b9-606">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="8e1b9-607">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8e1b9-607">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="8e1b9-608">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tylko środowisko programistyczne)</span><span class="sxs-lookup"><span data-stu-id="8e1b9-608">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="8e1b9-609">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="8e1b9-609">Environment variables</span></span>
1. <span data-ttu-id="8e1b9-610">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-610">Command-line arguments</span></span>

<span data-ttu-id="8e1b9-611">Typowym celem jest umieszczenie dostawcy konfiguracji wiersza polecenia jako ostatni w serii dostawców, aby zezwolić na argumenty wiersza polecenia, aby przesłonić konfigurację ustawioną przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-611">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="8e1b9-612">Poprzednia sekwencja dostawców jest używana, gdy nowy Konstruktor hosta zostanie zainicjowany przy użyciu `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-612">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8e1b9-613">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-613">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="8e1b9-614">Konfigurowanie konstruktora hostów za pomocą UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="8e1b9-614">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="8e1b9-615">Aby skonfigurować konstruktora hostów, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> w konstruktorze hosta z konfiguracją.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-615">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

## <a name="configureappconfiguration"></a><span data-ttu-id="8e1b9-616">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="8e1b9-616">ConfigureAppConfiguration</span></span>

<span data-ttu-id="8e1b9-617">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić dostawców konfiguracji aplikacji oprócz tych dodanych automatycznie przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-617">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="8e1b9-618">Zastąp poprzednią konfigurację argumentami wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-618">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="8e1b9-619">Aby podać konfigurację aplikacji, którą można zastąpić za pomocą argumentów wiersza polecenia, wywołaj `AddCommandLine` Last:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-619">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="8e1b9-620">Usuń dostawców dodanych przez CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="8e1b9-620">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="8e1b9-621">Aby usunąć dostawców dodanych przez `CreateDefaultBuilder`, wywołaj najpierw polecenie [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) w [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="8e1b9-621">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="8e1b9-622">Użyj konfiguracji podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-622">Consume configuration during app startup</span></span>

<span data-ttu-id="8e1b9-623">Konfiguracja dostarczona do aplikacji w `ConfigureAppConfiguration` jest dostępna podczas uruchamiania aplikacji, w tym `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-623">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8e1b9-624">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja dostępu podczas uruchamiania](#access-configuration-during-startup) .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-624">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="8e1b9-625">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-625">Command-line Configuration Provider</span></span>

<span data-ttu-id="8e1b9-626"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> ładuje konfigurację z par klucz-wartość argumentu wiersza polecenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-626">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="8e1b9-627">Aby uaktywnić konfigurację wiersza polecenia, Metoda rozszerzenia <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> jest wywoływana w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-627">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8e1b9-628">`AddCommandLine` jest wywoływana automatycznie, gdy zostanie wywołana `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-628">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="8e1b9-629">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-629">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="8e1b9-630">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-630">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="8e1b9-631">Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-631">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="8e1b9-632">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-632">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="8e1b9-633">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-633">Environment variables.</span></span>

<span data-ttu-id="8e1b9-634">`CreateDefaultBuilder` dodaje dostawcę konfiguracji wiersza polecenia Last.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-634">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="8e1b9-635">Argumenty wiersza polecenia przekazane w czasie wykonywania zastępują konfigurację ustawioną przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-635">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="8e1b9-636">`CreateDefaultBuilder` działa, gdy host jest skonstruowany.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-636">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="8e1b9-637">W związku z tym konfiguracja wiersza polecenia aktywowana przez `CreateDefaultBuilder` może mieć wpływ na sposób konfigurowania hosta.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-637">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="8e1b9-638">W przypadku aplikacji opartych na ASP.NET Core szablonach `AddCommandLine` została już wywołana przez `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-638">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8e1b9-639">Aby dodać kolejnych dostawców konfiguracji i zachować możliwość przesłonięcia konfiguracji od tych dostawców za pomocą argumentów wiersza polecenia, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddCommandLine` Last.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-639">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="8e1b9-640">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="8e1b9-640">**Example**</span></span>

<span data-ttu-id="8e1b9-641">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-641">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="8e1b9-642">Otwórz wiersz polecenia w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-642">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="8e1b9-643">Podaj argument wiersza polecenia do polecenia `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-643">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="8e1b9-644">Po uruchomieniu aplikacji otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-644">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="8e1b9-645">Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla argumentu wiersza polecenia konfiguracji dostarczonego do `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-645">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="8e1b9-646">Argumenty</span><span class="sxs-lookup"><span data-stu-id="8e1b9-646">Arguments</span></span>

<span data-ttu-id="8e1b9-647">Wartość musi następować po znaku równości (`=`) lub klucz musi mieć prefiks (`--` lub `/`), gdy wartość znajduje się w miejscu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-647">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="8e1b9-648">Wartość nie jest wymagana, jeśli jest używany znak równości (na przykład `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-648">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="8e1b9-649">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="8e1b9-649">Key prefix</span></span>               | <span data-ttu-id="8e1b9-650">Przykład</span><span class="sxs-lookup"><span data-stu-id="8e1b9-650">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="8e1b9-651">Brak prefiksu</span><span class="sxs-lookup"><span data-stu-id="8e1b9-651">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="8e1b9-652">Dwie kreski (`--`)</span><span class="sxs-lookup"><span data-stu-id="8e1b9-652">Two dashes (`--`)</span></span>        | <span data-ttu-id="8e1b9-653">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-653">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="8e1b9-654">Ukośnik (`/`)</span><span class="sxs-lookup"><span data-stu-id="8e1b9-654">Forward slash (`/`)</span></span>      | <span data-ttu-id="8e1b9-655">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-655">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="8e1b9-656">W tym samym poleceniu nie należy mieszać par klucz-wartość argumentu wiersza polecenia, które używają znaku równości z parami klucz-wartość, które używają spacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-656">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="8e1b9-657">Przykładowe polecenia:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-657">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="8e1b9-658">Mapowanie przełączników</span><span class="sxs-lookup"><span data-stu-id="8e1b9-658">Switch mappings</span></span>

<span data-ttu-id="8e1b9-659">Mapowania przełączników Zezwalaj na logikę zamiany nazwy klucza.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-659">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="8e1b9-660">Podczas ręcznego kompilowania konfiguracji przy użyciu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>należy udostępnić słownik przemieszczeń Switch do metody <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-660">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="8e1b9-661">Gdy jest używany słownik mapowania przełączników, słownik jest sprawdzany dla klucza, który pasuje do klucza dostarczonego przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-661">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="8e1b9-662">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) zostanie przeniesiona z powrotem, aby ustawić parę klucz-wartość w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-662">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="8e1b9-663">Mapowanie przełącznika jest wymagane dla każdego klucza wiersza polecenia poprzedzonego jedną kreską (`-`).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-663">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="8e1b9-664">Przełącz reguły klucza słownika mapowania:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-664">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="8e1b9-665">Przełączniki muszą zaczynać się kreską (`-`) lub podwójną kreską (`--`).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-665">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="8e1b9-666">Słownik mapowania przełącznika nie może zawierać zduplikowanych kluczy.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-666">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="8e1b9-667">Utwórz słownik mapowań mapowania.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-667">Create a switch mappings dictionary.</span></span> <span data-ttu-id="8e1b9-668">W poniższym przykładzie są tworzone dwa mapowania przełączników:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-668">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="8e1b9-669">Po skompilowaniu hosta Wywołaj `AddCommandLine` przy użyciu słownika mapowania przełączników:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-669">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="8e1b9-670">W przypadku aplikacji korzystających z mapowań przełączników wywołanie `CreateDefaultBuilder` nie powinno przekazywać argumentów.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-670">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="8e1b9-671">Wywołanie `AddCommandLine` metody `CreateDefaultBuilder` nie obejmuje zamapowanych przełączników i nie ma sposobu przekazywania słownika mapowania przełącznika do `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-671">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8e1b9-672">Rozwiązanie nie przekazuje argumentów do `CreateDefaultBuilder`, ale zamiast zezwalać `AddCommandLine` metody `ConfigurationBuilder` do przetwarzania zarówno argumentów, jak i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-672">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="8e1b9-673">Po utworzeniu słownika mapowań przełączników zawiera dane przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-673">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="8e1b9-674">Klucz</span><span class="sxs-lookup"><span data-stu-id="8e1b9-674">Key</span></span>       | <span data-ttu-id="8e1b9-675">Wartość</span><span class="sxs-lookup"><span data-stu-id="8e1b9-675">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="8e1b9-676">Jeśli klucze mapowane przez przełącznik są używane podczas uruchamiania aplikacji, konfiguracja otrzymuje wartość konfiguracji klucza dostarczonego przez słownik:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-676">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="8e1b9-677">Po uruchomieniu poprzedniego polecenia Konfiguracja zawiera wartości pokazane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-677">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="8e1b9-678">Klucz</span><span class="sxs-lookup"><span data-stu-id="8e1b9-678">Key</span></span>               | <span data-ttu-id="8e1b9-679">Wartość</span><span class="sxs-lookup"><span data-stu-id="8e1b9-679">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="8e1b9-680">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="8e1b9-680">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="8e1b9-681"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> ładuje konfigurację ze par klucz-wartość zmiennej środowiskowej w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-681">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="8e1b9-682">Aby uaktywnić konfigurację zmiennych środowiskowych, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-682">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="8e1b9-683">[Azure App Service](https://azure.microsoft.com/services/app-service/) umożliwia ustawianie zmiennych środowiskowych w witrynie Azure Portal, które mogą przesłonić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-683">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="8e1b9-684">Aby uzyskać więcej informacji, zobacz [Azure Apps: Przesłoń konfigurację aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-684">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="8e1b9-685">`AddEnvironmentVariables` jest używany do ładowania zmiennych środowiskowych, które są poprzedzone `ASPNETCORE_` na potrzeby [konfiguracji hosta](#host-versus-app-configuration) , gdy nowy Konstruktor hosta zostanie zainicjowany przy użyciu [hosta sieci Web](xref:fundamentals/host/web-host) i zostanie wywołane `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-685">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="8e1b9-686">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-686">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="8e1b9-687">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-687">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="8e1b9-688">Konfiguracja aplikacji z nieoznaczonych zmiennych środowiskowych przez wywołanie `AddEnvironmentVariables` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-688">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="8e1b9-689">Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-689">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="8e1b9-690">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-690">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="8e1b9-691">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-691">Command-line arguments.</span></span>

<span data-ttu-id="8e1b9-692">Dostawca konfiguracji zmiennych środowiskowych jest wywoływany po ustanowieniu konfiguracji z poziomu kluczy tajnych użytkownika i plików *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-692">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="8e1b9-693">Wywołanie dostawcy w tym miejscu pozwala odczytywać zmienne środowiskowe w czasie wykonywania w celu przesłania konfiguracji ustawionych przez klucze tajne użytkownika i pliki *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-693">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="8e1b9-694">Aby zapewnić konfigurację aplikacji na podstawie dodatkowych zmiennych środowiskowych, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddEnvironmentVariables` z prefiksem:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-694">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="8e1b9-695">Wywołaj `AddEnvironmentVariables` ostatnią, aby zezwolić na zmienne środowiskowe z danym prefiksem w celu przesłonięcia wartości z innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-695">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="8e1b9-696">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="8e1b9-696">**Example**</span></span>

<span data-ttu-id="8e1b9-697">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-697">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="8e1b9-698">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-698">Run the sample app.</span></span> <span data-ttu-id="8e1b9-699">Otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-699">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="8e1b9-700">Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla zmiennej środowiskowej `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-700">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="8e1b9-701">Wartość odzwierciedla środowisko, w którym jest uruchomiona aplikacja, zwykle `Development` podczas uruchamiania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-701">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="8e1b9-702">Aby zachować listę zmiennych środowiskowych renderowanych przez aplikację, aplikacja filtruje zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-702">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="8e1b9-703">Zapoznaj się z plikiem przykładowej *strony aplikacji/index. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-703">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="8e1b9-704">Aby udostępnić wszystkie zmienne środowiskowe dostępne dla aplikacji, należy zmienić `FilteredConfiguration` na *stronie/index. cshtml. cs* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-704">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="8e1b9-705">Prefiksy</span><span class="sxs-lookup"><span data-stu-id="8e1b9-705">Prefixes</span></span>

<span data-ttu-id="8e1b9-706">Zmienne środowiskowe ładowane do konfiguracji aplikacji są filtrowane podczas dostarczania prefiksu do metody `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-706">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="8e1b9-707">Na przykład aby filtrować zmienne środowiskowe na prefiksie `CUSTOM_`, podaj prefiks dla dostawcy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-707">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="8e1b9-708">Prefiks jest usuwany podczas tworzenia par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-708">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="8e1b9-709">Podczas tworzenia konstruktora hostów Konfiguracja hosta jest zapewniana przez zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-709">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="8e1b9-710">Aby uzyskać więcej informacji na temat prefiksu używanego dla tych zmiennych środowiskowych, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-710">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="8e1b9-711">**Prefiksy parametrów połączenia**</span><span class="sxs-lookup"><span data-stu-id="8e1b9-711">**Connection string prefixes**</span></span>

<span data-ttu-id="8e1b9-712">Interfejs API konfiguracji ma specjalne reguły przetwarzania dla czterech zmiennych środowiskowych parametrów połączenia związanych z konfigurowaniem parametrów połączenia platformy Azure dla środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-712">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="8e1b9-713">Zmienne środowiskowe z prefiksami podanymi w tabeli są ładowane do aplikacji, jeśli nie podano prefiksu do `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-713">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="8e1b9-714">Prefiks parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="8e1b9-714">Connection string prefix</span></span> | <span data-ttu-id="8e1b9-715">Dostawca</span><span class="sxs-lookup"><span data-stu-id="8e1b9-715">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="8e1b9-716">Dostawca niestandardowy</span><span class="sxs-lookup"><span data-stu-id="8e1b9-716">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="8e1b9-717">MySQL</span><span class="sxs-lookup"><span data-stu-id="8e1b9-717">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="8e1b9-718">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="8e1b9-718">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="8e1b9-719">SQL Server</span><span class="sxs-lookup"><span data-stu-id="8e1b9-719">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="8e1b9-720">Gdy zmienna środowiskowa zostanie odnaleziona i załadowana do konfiguracji z dowolnymi z czterech prefiksów pokazanych w tabeli:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-720">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="8e1b9-721">Klucz konfiguracji jest tworzony przez usunięcie prefiksu zmiennej środowiskowej i dodanie sekcji klucza konfiguracji (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-721">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="8e1b9-722">Zostanie utworzona nowa para klucz-wartość konfiguracji, która reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, które nie ma określonego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-722">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="8e1b9-723">Klucz zmiennej środowiskowej</span><span class="sxs-lookup"><span data-stu-id="8e1b9-723">Environment variable key</span></span> | <span data-ttu-id="8e1b9-724">Przekonwertowany klucz konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-724">Converted configuration key</span></span> | <span data-ttu-id="8e1b9-725">Wpis konfiguracji dostawcy</span><span class="sxs-lookup"><span data-stu-id="8e1b9-725">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="8e1b9-726">Wpis konfiguracji nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-726">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="8e1b9-727">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-727">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="8e1b9-728">Wartość:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-728">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="8e1b9-729">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-729">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="8e1b9-730">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-730">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="8e1b9-731">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-731">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="8e1b9-732">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-732">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="8e1b9-733">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="8e1b9-733">**Example**</span></span>

<span data-ttu-id="8e1b9-734">Na serwerze zostanie utworzona niestandardowa zmienna środowiskowa parametrów połączenia:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-734">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="8e1b9-735">Nazwa &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-735">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="8e1b9-736">`Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True` &ndash; wartości</span><span class="sxs-lookup"><span data-stu-id="8e1b9-736">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="8e1b9-737">Jeśli `IConfiguration` zostanie dodany i przypisany do pola o nazwie `_config`, przeczytaj wartość:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-737">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="8e1b9-738">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="8e1b9-738">File Configuration Provider</span></span>

<span data-ttu-id="8e1b9-739"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową do ładowania konfiguracji z systemu plików.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-739"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="8e1b9-740">Następujący dostawcy konfiguracji są przydzielone do określonych typów plików:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-740">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="8e1b9-741">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="8e1b9-741">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="8e1b9-742">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="8e1b9-742">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="8e1b9-743">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="8e1b9-743">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="8e1b9-744">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="8e1b9-744">INI Configuration Provider</span></span>

<span data-ttu-id="8e1b9-745"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku INI w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-745">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="8e1b9-746">Aby uaktywnić konfigurację pliku INI, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-746">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8e1b9-747">Dwukropek może służyć jako ogranicznik sekcji w konfiguracji pliku INI.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-747">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="8e1b9-748">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-748">Overloads permit specifying:</span></span>

* <span data-ttu-id="8e1b9-749">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-749">Whether the file is optional.</span></span>
* <span data-ttu-id="8e1b9-750">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-750">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="8e1b9-751"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-751">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="8e1b9-752">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-752">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="8e1b9-753">Ogólny przykład pliku konfiguracji INI:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-753">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="8e1b9-754">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-754">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8e1b9-755">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-755">section0:key0</span></span>
* <span data-ttu-id="8e1b9-756">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-756">section0:key1</span></span>
* <span data-ttu-id="8e1b9-757">Section1: podsekcja: Key</span><span class="sxs-lookup"><span data-stu-id="8e1b9-757">section1:subsection:key</span></span>
* <span data-ttu-id="8e1b9-758">section2: subsection0: klucz</span><span class="sxs-lookup"><span data-stu-id="8e1b9-758">section2:subsection0:key</span></span>
* <span data-ttu-id="8e1b9-759">section2: subsection1: klucz</span><span class="sxs-lookup"><span data-stu-id="8e1b9-759">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="8e1b9-760">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="8e1b9-760">JSON Configuration Provider</span></span>

<span data-ttu-id="8e1b9-761"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku JSON podczas środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-761">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="8e1b9-762">Aby uaktywnić konfigurację pliku JSON, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-762">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8e1b9-763">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-763">Overloads permit specifying:</span></span>

* <span data-ttu-id="8e1b9-764">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-764">Whether the file is optional.</span></span>
* <span data-ttu-id="8e1b9-765">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-765">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="8e1b9-766"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-766">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="8e1b9-767">`AddJsonFile` jest automatycznie wywoływana dwukrotnie, gdy nowy Konstruktor hosta zostanie zainicjowany z `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-767">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8e1b9-768">Metoda jest wywoływana w celu załadowania konfiguracji z:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-768">The method is called to load configuration from:</span></span>

* <span data-ttu-id="8e1b9-769">*appSettings. json* &ndash; ten plik jest najpierw odczytywany.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-769">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="8e1b9-770">Wersja środowiska pliku może przesłonić wartości dostarczone przez plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-770">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="8e1b9-771">*appSettings. {Environment}. JSON* &ndash; wersja środowiska pliku jest ładowana na podstawie [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-771">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="8e1b9-772">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-772">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="8e1b9-773">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-773">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="8e1b9-774">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-774">Environment variables.</span></span>
* <span data-ttu-id="8e1b9-775">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-775">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="8e1b9-776">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-776">Command-line arguments.</span></span>

<span data-ttu-id="8e1b9-777">Dostawca konfiguracji JSON został ustanowiony jako pierwszy.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-777">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="8e1b9-778">W związku z tym klucze tajne użytkownika, zmienne środowiskowe i argumenty wiersza polecenia przesłaniają konfigurację ustawioną przez pliki *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-778">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="8e1b9-779">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji dla plików innych niż *appSettings. JSON* i *appSettings. { Environment}. JSON*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-779">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="8e1b9-780">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="8e1b9-780">**Example**</span></span>

<span data-ttu-id="8e1b9-781">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje dwa wywołania `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-781">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="8e1b9-782">Pierwsze wywołanie `AddJsonFile` ładuje konfigurację z pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-782">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="8e1b9-783">Drugie wywołanie `AddJsonFile` ładuje konfigurację z *appSettings. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-783">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="8e1b9-784">Dla *appSettings. Plik Development. JSON* w przykładowej aplikacji jest ładowany:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-784">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="8e1b9-785">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-785">Run the sample app.</span></span> <span data-ttu-id="8e1b9-786">Otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-786">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="8e1b9-787">Dane wyjściowe zawierają pary klucz-wartość dla konfiguracji w oparciu o środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-787">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="8e1b9-788">Poziom dziennika dla klucza `Logging:LogLevel:Default` jest `Debug` podczas uruchamiania aplikacji w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-788">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="8e1b9-789">Ponownie uruchom przykładową aplikację w środowisku produkcyjnym:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-789">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="8e1b9-790">Otwórz plik *Properties/profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="8e1b9-790">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="8e1b9-791">W profilu `ConfigurationSample` Zmień wartość zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` na `Production`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-791">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="8e1b9-792">Zapisz plik i uruchom aplikację za pomocą `dotnet run` w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-792">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="8e1b9-793">Ustawienia w *appSettings. Plik Development. JSON* nie zastępuje już ustawień w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-793">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="8e1b9-794">Poziom dziennika `Logging:LogLevel:Default` jest `Warning`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-794">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="8e1b9-795">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="8e1b9-795">XML Configuration Provider</span></span>

<span data-ttu-id="8e1b9-796"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-796">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="8e1b9-797">Aby uaktywnić konfigurację pliku XML, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-797">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8e1b9-798">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-798">Overloads permit specifying:</span></span>

* <span data-ttu-id="8e1b9-799">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-799">Whether the file is optional.</span></span>
* <span data-ttu-id="8e1b9-800">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-800">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="8e1b9-801"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-801">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="8e1b9-802">Węzeł główny pliku konfiguracji jest ignorowany, gdy są tworzone pary klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-802">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="8e1b9-803">Nie określaj definicji typu dokumentu (DTD) ani przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-803">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="8e1b9-804">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-804">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="8e1b9-805">Pliki konfiguracji XML mogą używać odrębnych nazw elementów dla powtarzających się sekcji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-805">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="8e1b9-806">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-806">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8e1b9-807">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-807">section0:key0</span></span>
* <span data-ttu-id="8e1b9-808">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-808">section0:key1</span></span>
* <span data-ttu-id="8e1b9-809">section1:key0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-809">section1:key0</span></span>
* <span data-ttu-id="8e1b9-810">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-810">section1:key1</span></span>

<span data-ttu-id="8e1b9-811">Powtarzalne elementy, które używają tej samej nazwy elementu, działają, jeśli atrybut `name` jest używany do odróżnienia elementów:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-811">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="8e1b9-812">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-812">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8e1b9-813">sekcja: section0: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-813">section:section0:key:key0</span></span>
* <span data-ttu-id="8e1b9-814">sekcja: section0: Key: Klucz1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-814">section:section0:key:key1</span></span>
* <span data-ttu-id="8e1b9-815">sekcja: Section1: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-815">section:section1:key:key0</span></span>
* <span data-ttu-id="8e1b9-816">sekcja: Section1: Key: Klucz1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-816">section:section1:key:key1</span></span>

<span data-ttu-id="8e1b9-817">Atrybuty mogą służyć do dostarczania wartości:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-817">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="8e1b9-818">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-818">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8e1b9-819">Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="8e1b9-819">key:attribute</span></span>
* <span data-ttu-id="8e1b9-820">sekcja: Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="8e1b9-820">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="8e1b9-821">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="8e1b9-821">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="8e1b9-822"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> używa plików katalogu jako par klucz konfiguracji i wartość.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-822">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="8e1b9-823">Kluczem jest nazwa pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-823">The key is the file name.</span></span> <span data-ttu-id="8e1b9-824">Wartość zawiera zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-824">The value contains the file's contents.</span></span> <span data-ttu-id="8e1b9-825">Dostawca konfiguracji klucza dla plików jest używany w scenariuszach hostingu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-825">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="8e1b9-826">Aby uaktywnić konfigurację klucza dla plików, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-826">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="8e1b9-827">`directoryPath` plików musi być ścieżką bezwzględną.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-827">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="8e1b9-828">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-828">Overloads permit specifying:</span></span>

* <span data-ttu-id="8e1b9-829">Delegat `Action<KeyPerFileConfigurationSource>`, który konfiguruje źródło.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-829">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="8e1b9-830">Określa, czy katalog jest opcjonalny, i ścieżkę do katalogu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-830">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="8e1b9-831">Podwójny znak podkreślenia (`__`) jest używany jako ogranicznik klucza konfiguracji w nazwach plików.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-831">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="8e1b9-832">Na przykład nazwa pliku `Logging__LogLevel__System` generuje klucz konfiguracji `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-832">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="8e1b9-833">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-833">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="8e1b9-834">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="8e1b9-834">Memory Configuration Provider</span></span>

<span data-ttu-id="8e1b9-835"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> używa kolekcji w pamięci jako par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-835">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="8e1b9-836">Aby uaktywnić konfigurację kolekcji w pamięci, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-836">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8e1b9-837">Dostawcę konfiguracji można zainicjować przy użyciu `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-837">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="8e1b9-838">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-838">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="8e1b9-839">W poniższym przykładzie tworzony jest słownik konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-839">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="8e1b9-840">Słownik jest używany z wywołaniem `AddInMemoryCollection`, aby zapewnić konfigurację:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-840">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="8e1b9-841">GetValue</span><span class="sxs-lookup"><span data-stu-id="8e1b9-841">GetValue</span></span>

<span data-ttu-id="8e1b9-842">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnia jedną wartość z konfiguracji z określonym kluczem i konwertuje ją na określony typ niekolekcje.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-842">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="8e1b9-843">Przeciążenie akceptuje wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-843">An overload accepts a default value.</span></span>

<span data-ttu-id="8e1b9-844">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-844">The following example:</span></span>

* <span data-ttu-id="8e1b9-845">Wyodrębnia wartość ciągu z konfiguracji z kluczem `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-845">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="8e1b9-846">Jeśli nie można odnaleźć `NumberKey` w kluczach konfiguracji, zostanie użyta wartość domyślna `99`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-846">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="8e1b9-847">Typ wartości jako `int`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-847">Types the value as an `int`.</span></span>
* <span data-ttu-id="8e1b9-848">Przechowuje wartość we właściwości `NumberConfig` do użycia na stronie.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-848">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="8e1b9-849">GetSection, GetChildren i EXISTS</span><span class="sxs-lookup"><span data-stu-id="8e1b9-849">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="8e1b9-850">W poniższych przykładach należy wziąć pod uwagę następujący plik JSON.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-850">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="8e1b9-851">Cztery klucze są dostępne w dwóch sekcjach, z których jedna zawiera parę podsekcji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-851">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="8e1b9-852">Gdy plik jest odczytywany do konfiguracji, następujące unikatowe klucze hierarchiczne są tworzone w celu przechowywania wartości konfiguracyjnych:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-852">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="8e1b9-853">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-853">section0:key0</span></span>
* <span data-ttu-id="8e1b9-854">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-854">section0:key1</span></span>
* <span data-ttu-id="8e1b9-855">section1:key0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-855">section1:key0</span></span>
* <span data-ttu-id="8e1b9-856">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-856">section1:key1</span></span>
* <span data-ttu-id="8e1b9-857">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-857">section2:subsection0:key0</span></span>
* <span data-ttu-id="8e1b9-858">section2: subsection0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-858">section2:subsection0:key1</span></span>
* <span data-ttu-id="8e1b9-859">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-859">section2:subsection1:key0</span></span>
* <span data-ttu-id="8e1b9-860">section2: subsection1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-860">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="8e1b9-861">GetSection</span><span class="sxs-lookup"><span data-stu-id="8e1b9-861">GetSection</span></span>

<span data-ttu-id="8e1b9-862">[IConfiguration. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcję konfiguracji z określonym kluczem podsekcji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-862">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="8e1b9-863">Aby zwrócić <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz-wartość w `section1`, wywołaj `GetSection` i podaj nazwę sekcji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-863">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="8e1b9-864">`configSection` nie ma wartości, tylko klucza i ścieżki.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-864">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="8e1b9-865">Podobnie, aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę sekcji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-865">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="8e1b9-866">`GetSection` nigdy nie zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-866">`GetSection` never returns `null`.</span></span> <span data-ttu-id="8e1b9-867">Jeśli nie znaleziono pasującej sekcji, zwracany jest pusty `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-867">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="8e1b9-868">Gdy `GetSection` zwraca pasującą sekcję, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-868">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="8e1b9-869"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> i <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> są zwracane, gdy istnieje sekcja.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-869">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="8e1b9-870">GetChildren</span><span class="sxs-lookup"><span data-stu-id="8e1b9-870">GetChildren</span></span>

<span data-ttu-id="8e1b9-871">Wywołanie [iConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) w `section2` uzyskuje `IEnumerable<IConfigurationSection>` obejmujący:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-871">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="8e1b9-872">Exists</span><span class="sxs-lookup"><span data-stu-id="8e1b9-872">Exists</span></span>

<span data-ttu-id="8e1b9-873">Użyj [ConfigurationExtensions. istnieje](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) , aby określić, czy istnieje sekcja konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-873">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="8e1b9-874">Dane przykładowe `sectionExists` jest `false`, ponieważ w danych konfiguracyjnych nie ma `section2:subsection2` sekcji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-874">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="8e1b9-875">Powiąż z grafem obiektów</span><span class="sxs-lookup"><span data-stu-id="8e1b9-875">Bind to an object graph</span></span>

<span data-ttu-id="8e1b9-876"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> jest w stanie powiązać cały Graf obiektów POCO.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-876"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="8e1b9-877">Podobnie jak w przypadku powiązania prostego obiektu, powiązane są tylko publiczne właściwości odczytu i zapisu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-877">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="8e1b9-878">Przykład zawiera model `TvShow`, którego wykres obiektu zawiera klasy `Metadata` i `Actors` (*modele/TvShow. cs*):</span><span class="sxs-lookup"><span data-stu-id="8e1b9-878">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="8e1b9-879">Przykładowa aplikacja zawiera plik *tvshow. XML* zawierający dane konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-879">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="8e1b9-880">Konfiguracja jest powiązana z całym grafem obiektu `TvShow` za pomocą metody `Bind`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-880">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="8e1b9-881">Powiązane wystąpienie jest przypisane do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-881">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="8e1b9-882">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) tworzy powiązania i zwraca określony typ.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-882">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="8e1b9-883">`Get<T>` jest wygodniejszy niż korzystanie z `Bind`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-883">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="8e1b9-884">Poniższy kod ilustruje sposób używania `Get<T>` w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-884">The following code shows how to use `Get<T>` with the preceding example:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="8e1b9-885">Powiąż tablicę z klasą</span><span class="sxs-lookup"><span data-stu-id="8e1b9-885">Bind an array to a class</span></span>

<span data-ttu-id="8e1b9-886">*Przykładowa aplikacja pokazuje Koncepcje opisane w tej sekcji.*</span><span class="sxs-lookup"><span data-stu-id="8e1b9-886">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="8e1b9-887"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-887">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="8e1b9-888">Każdy format tablicy, który ujawnia segment klucza numerycznego (`:0:`, `:1:`, &hellip; `:{n}:`), jest w stanie powiązać powiązanie tablicową z tablicą klas POCO.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-888">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="8e1b9-889">Powiązanie jest dostarczane według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-889">Binding is provided by convention.</span></span> <span data-ttu-id="8e1b9-890">Niestandardowi dostawcy konfiguracji nie muszą implementować powiązania tablicy.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-890">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="8e1b9-891">**Przetwarzanie tablicy w pamięci**</span><span class="sxs-lookup"><span data-stu-id="8e1b9-891">**In-memory array processing**</span></span>

<span data-ttu-id="8e1b9-892">Należy wziąć pod uwagę klucze konfiguracji i wartości podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-892">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="8e1b9-893">Klucz</span><span class="sxs-lookup"><span data-stu-id="8e1b9-893">Key</span></span>             | <span data-ttu-id="8e1b9-894">Wartość</span><span class="sxs-lookup"><span data-stu-id="8e1b9-894">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="8e1b9-895">Tablica: wpisy: 0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-895">array:entries:0</span></span> | <span data-ttu-id="8e1b9-896">value0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-896">value0</span></span> |
| <span data-ttu-id="8e1b9-897">Tablica: wpisy: 1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-897">array:entries:1</span></span> | <span data-ttu-id="8e1b9-898">Sekwencj</span><span class="sxs-lookup"><span data-stu-id="8e1b9-898">value1</span></span> |
| <span data-ttu-id="8e1b9-899">Tablica: wpisy: 2</span><span class="sxs-lookup"><span data-stu-id="8e1b9-899">array:entries:2</span></span> | <span data-ttu-id="8e1b9-900">Wartość2</span><span class="sxs-lookup"><span data-stu-id="8e1b9-900">value2</span></span> |
| <span data-ttu-id="8e1b9-901">Tablica: wpisy: 4</span><span class="sxs-lookup"><span data-stu-id="8e1b9-901">array:entries:4</span></span> | <span data-ttu-id="8e1b9-902">value4</span><span class="sxs-lookup"><span data-stu-id="8e1b9-902">value4</span></span> |
| <span data-ttu-id="8e1b9-903">Tablica: wpisy: 5</span><span class="sxs-lookup"><span data-stu-id="8e1b9-903">array:entries:5</span></span> | <span data-ttu-id="8e1b9-904">value5</span><span class="sxs-lookup"><span data-stu-id="8e1b9-904">value5</span></span> |

<span data-ttu-id="8e1b9-905">Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-905">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="8e1b9-906">Tablica pomija wartość indeksu &num;3.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-906">The array skips a value for index &num;3.</span></span> <span data-ttu-id="8e1b9-907">Segregator konfiguracji nie może powiązać wartości null ani tworzyć wpisów o wartości null w obiektach powiązanych, co oznacza, że w chwili pojawi się wynik powiązania tej tablicy z obiektem.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-907">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="8e1b9-908">W przykładowej aplikacji jest dostępna Klasa POCO, która przechowuje powiązane dane konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-908">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="8e1b9-909">Dane konfiguracji są powiązane z obiektem:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-909">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="8e1b9-910">można również użyć składni [`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) , co powoduje zwiększenie kodu kompaktowego:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-910">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="8e1b9-911">Obiekt powiązany, wystąpienie `ArrayExample`, otrzymuje dane tablicy z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-911">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="8e1b9-912">Indeks `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-912">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="8e1b9-913">Wartość `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-913">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="8e1b9-914">0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-914">0</span></span>                            | <span data-ttu-id="8e1b9-915">value0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-915">value0</span></span>                       |
| <span data-ttu-id="8e1b9-916">1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-916">1</span></span>                            | <span data-ttu-id="8e1b9-917">Sekwencj</span><span class="sxs-lookup"><span data-stu-id="8e1b9-917">value1</span></span>                       |
| <span data-ttu-id="8e1b9-918">2</span><span class="sxs-lookup"><span data-stu-id="8e1b9-918">2</span></span>                            | <span data-ttu-id="8e1b9-919">Wartość2</span><span class="sxs-lookup"><span data-stu-id="8e1b9-919">value2</span></span>                       |
| <span data-ttu-id="8e1b9-920">3</span><span class="sxs-lookup"><span data-stu-id="8e1b9-920">3</span></span>                            | <span data-ttu-id="8e1b9-921">value4</span><span class="sxs-lookup"><span data-stu-id="8e1b9-921">value4</span></span>                       |
| <span data-ttu-id="8e1b9-922">4</span><span class="sxs-lookup"><span data-stu-id="8e1b9-922">4</span></span>                            | <span data-ttu-id="8e1b9-923">value5</span><span class="sxs-lookup"><span data-stu-id="8e1b9-923">value5</span></span>                       |

<span data-ttu-id="8e1b9-924">Indeks &num;3 w obiekcie powiązanym zawiera dane konfiguracyjne `array:4` klucza konfiguracji i jego wartość `value4`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-924">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="8e1b9-925">Gdy dane konfiguracji zawierające tablicę są powiązane, indeksy tablic w kluczach konfiguracji są używane tylko do iteracji danych konfiguracji podczas tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-925">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="8e1b9-926">Wartości null nie można zachować w danych konfiguracyjnych, a wpis o wartości null nie jest tworzony w obiekcie powiązanym, gdy tablica w kluczach konfiguracji pomija jeden lub więcej indeksów.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-926">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="8e1b9-927">Brakujący element konfiguracji dla indeksu &num;3 można dostarczyć przed powiązaniem do wystąpienia `ArrayExample` przez dowolnego dostawcę konfiguracji, który wygeneruje poprawną parę klucz-wartość w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-927">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="8e1b9-928">Jeśli przykład zawiera dodatkowego dostawcę konfiguracji JSON z brakującą parą klucz-wartość, `ArrayExample.Entries` pasuje do kompletnej tablicy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-928">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="8e1b9-929">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-929">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="8e1b9-930">W pliku `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-930">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="8e1b9-931">Para klucz-wartość pokazana w tabeli jest ładowana do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-931">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="8e1b9-932">Klucz</span><span class="sxs-lookup"><span data-stu-id="8e1b9-932">Key</span></span>             | <span data-ttu-id="8e1b9-933">Wartość</span><span class="sxs-lookup"><span data-stu-id="8e1b9-933">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="8e1b9-934">Tablica: wpisy: 3</span><span class="sxs-lookup"><span data-stu-id="8e1b9-934">array:entries:3</span></span> | <span data-ttu-id="8e1b9-935">Wartość3</span><span class="sxs-lookup"><span data-stu-id="8e1b9-935">value3</span></span> |

<span data-ttu-id="8e1b9-936">Jeśli wystąpienie klasy `ArrayExample` jest powiązane, gdy dostawca konfiguracji JSON zawiera wpis dla indeksu &num;3, tablica `ArrayExample.Entries` zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-936">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="8e1b9-937">Indeks `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-937">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="8e1b9-938">Wartość `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-938">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="8e1b9-939">0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-939">0</span></span>                            | <span data-ttu-id="8e1b9-940">value0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-940">value0</span></span>                       |
| <span data-ttu-id="8e1b9-941">1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-941">1</span></span>                            | <span data-ttu-id="8e1b9-942">Sekwencj</span><span class="sxs-lookup"><span data-stu-id="8e1b9-942">value1</span></span>                       |
| <span data-ttu-id="8e1b9-943">2</span><span class="sxs-lookup"><span data-stu-id="8e1b9-943">2</span></span>                            | <span data-ttu-id="8e1b9-944">Wartość2</span><span class="sxs-lookup"><span data-stu-id="8e1b9-944">value2</span></span>                       |
| <span data-ttu-id="8e1b9-945">3</span><span class="sxs-lookup"><span data-stu-id="8e1b9-945">3</span></span>                            | <span data-ttu-id="8e1b9-946">Wartość3</span><span class="sxs-lookup"><span data-stu-id="8e1b9-946">value3</span></span>                       |
| <span data-ttu-id="8e1b9-947">4</span><span class="sxs-lookup"><span data-stu-id="8e1b9-947">4</span></span>                            | <span data-ttu-id="8e1b9-948">value4</span><span class="sxs-lookup"><span data-stu-id="8e1b9-948">value4</span></span>                       |
| <span data-ttu-id="8e1b9-949">5</span><span class="sxs-lookup"><span data-stu-id="8e1b9-949">5</span></span>                            | <span data-ttu-id="8e1b9-950">value5</span><span class="sxs-lookup"><span data-stu-id="8e1b9-950">value5</span></span>                       |

<span data-ttu-id="8e1b9-951">**Przetwarzanie tablicy JSON**</span><span class="sxs-lookup"><span data-stu-id="8e1b9-951">**JSON array processing**</span></span>

<span data-ttu-id="8e1b9-952">Jeśli plik JSON zawiera tablicę, klucze konfiguracji są tworzone dla elementów tablicy z indeksem sekcji o wartości zero.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-952">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="8e1b9-953">W poniższym pliku konfiguracyjnym `subsection` jest tablicą:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-953">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="8e1b9-954">Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz-wartość:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-954">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="8e1b9-955">Klucz</span><span class="sxs-lookup"><span data-stu-id="8e1b9-955">Key</span></span>                     | <span data-ttu-id="8e1b9-956">Wartość</span><span class="sxs-lookup"><span data-stu-id="8e1b9-956">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="8e1b9-957">json_array: klucz</span><span class="sxs-lookup"><span data-stu-id="8e1b9-957">json_array:key</span></span>          | <span data-ttu-id="8e1b9-958">wartośća</span><span class="sxs-lookup"><span data-stu-id="8e1b9-958">valueA</span></span> |
| <span data-ttu-id="8e1b9-959">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-959">json_array:subsection:0</span></span> | <span data-ttu-id="8e1b9-960">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="8e1b9-960">valueB</span></span> |
| <span data-ttu-id="8e1b9-961">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-961">json_array:subsection:1</span></span> | <span data-ttu-id="8e1b9-962">valueC</span><span class="sxs-lookup"><span data-stu-id="8e1b9-962">valueC</span></span> |
| <span data-ttu-id="8e1b9-963">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="8e1b9-963">json_array:subsection:2</span></span> | <span data-ttu-id="8e1b9-964">Znajdując</span><span class="sxs-lookup"><span data-stu-id="8e1b9-964">valueD</span></span> |

<span data-ttu-id="8e1b9-965">W przykładowej aplikacji jest dostępna następująca Klasa POCO z powiązaniem par klucz-wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-965">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="8e1b9-966">Po powiązaniu `JsonArrayExample.Key` utrzymuje `valueA`wartości.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-966">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="8e1b9-967">Wartości podsekcji są przechowywane we właściwości tablicy POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-967">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="8e1b9-968">Indeks `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-968">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="8e1b9-969">Wartość `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="8e1b9-969">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="8e1b9-970">0</span><span class="sxs-lookup"><span data-stu-id="8e1b9-970">0</span></span>                                   | <span data-ttu-id="8e1b9-971">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="8e1b9-971">valueB</span></span>                              |
| <span data-ttu-id="8e1b9-972">1</span><span class="sxs-lookup"><span data-stu-id="8e1b9-972">1</span></span>                                   | <span data-ttu-id="8e1b9-973">valueC</span><span class="sxs-lookup"><span data-stu-id="8e1b9-973">valueC</span></span>                              |
| <span data-ttu-id="8e1b9-974">2</span><span class="sxs-lookup"><span data-stu-id="8e1b9-974">2</span></span>                                   | <span data-ttu-id="8e1b9-975">Znajdując</span><span class="sxs-lookup"><span data-stu-id="8e1b9-975">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="8e1b9-976">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8e1b9-976">Custom configuration provider</span></span>

<span data-ttu-id="8e1b9-977">Przykładowa aplikacja pokazuje, jak utworzyć podstawowego dostawcę konfiguracji, który odczytuje pary klucz-wartość konfiguracji z bazy danych przy użyciu [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-977">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="8e1b9-978">Dostawca ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-978">The provider has the following characteristics:</span></span>

* <span data-ttu-id="8e1b9-979">Baza danych EF w pamięci jest używana w celach demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-979">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="8e1b9-980">Aby użyć bazy danych, która wymaga parametrów połączenia, zaimplementuj pomocniczą `ConfigurationBuilder`, aby podać parametry połączenia od innego dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-980">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="8e1b9-981">Dostawca odczytuje tabelę bazy danych w konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-981">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="8e1b9-982">Dostawca nie wykonuje zapytania do bazy danych w oparciu o klucz.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-982">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="8e1b9-983">Ponowne załadowanie nie zostało zaimplementowane, więc aktualizacja bazy danych po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-983">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="8e1b9-984">Zdefiniuj jednostkę `EFConfigurationValue` do przechowywania wartości konfiguracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-984">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="8e1b9-985">*Modele/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-985">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="8e1b9-986">Dodaj `EFConfigurationContext` do przechowywania skonfigurowanych wartości i uzyskiwania do nich dostępu.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-986">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="8e1b9-987">*EFConfigurationProvider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-987">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="8e1b9-988">Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-988">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="8e1b9-989">*EFConfigurationProvider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-989">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="8e1b9-990">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-990">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="8e1b9-991">Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-991">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="8e1b9-992">*EFConfigurationProvider/EFConfigurationProvider. cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-992">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="8e1b9-993">Metoda rozszerzenia `AddEFConfiguration` zezwala na Dodawanie źródła konfiguracji do `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-993">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="8e1b9-994">*Rozszerzenia/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-994">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="8e1b9-995">Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-995">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="8e1b9-996">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="8e1b9-996">Access configuration during startup</span></span>

<span data-ttu-id="8e1b9-997">Wsuń `IConfiguration` do konstruktora `Startup`, aby uzyskać dostęp do wartości konfiguracyjnych w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-997">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8e1b9-998">Aby uzyskać dostęp do konfiguracji w `Startup.Configure`, należy wstrzyknąć `IConfiguration` bezpośrednio do metody lub użyć wystąpienia z konstruktora:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-998">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="8e1b9-999">Aby zapoznać się z przykładem uzyskiwania dostępu do konfiguracji przy użyciu metod uruchamiania, zobacz [uruchamiania aplikacji: Wygodne metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="8e1b9-999">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="8e1b9-1000">Konfiguracja dostępu na stronie Razor Pages lub widoku MVC</span><span class="sxs-lookup"><span data-stu-id="8e1b9-1000">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="8e1b9-1001">Aby uzyskać dostęp do ustawień konfiguracji na stronie Razor Pages lub widoku MVC, Dodaj [dyrektywę using](xref:mvc/views/razor#using) ([ C# odwołanie: Using](/dotnet/csharp/language-reference/keywords/using-directive)) dla [przestrzeni nazw Microsoft. Extensions. Configuration](xref:Microsoft.Extensions.Configuration) i wsuń <xref:Microsoft.Extensions.Configuration.IConfiguration> do strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-1001">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="8e1b9-1002">Na stronie Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-1002">In a Razor Pages page:</span></span>

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="8e1b9-1003">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="8e1b9-1003">In an MVC view:</span></span>

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="8e1b9-1004">Dodawanie konfiguracji z zestawu zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="8e1b9-1004">Add configuration from an external assembly</span></span>

<span data-ttu-id="8e1b9-1005">Implementacja <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zestawu zewnętrznego poza klasą `Startup` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-1005">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="8e1b9-1006">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="8e1b9-1006">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e1b9-1007">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8e1b9-1007">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
