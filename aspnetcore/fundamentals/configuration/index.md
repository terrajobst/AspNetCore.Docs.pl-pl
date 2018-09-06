---
title: Konfiguracja w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core za pomocą interfejsu API konfiguracji.
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 288f8ba5b45cdecd8c9eae060fee2c2c25dec7f9
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893249"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="11701-103">Konfiguracja w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="11701-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="11701-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="11701-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="11701-105">Konfiguracja aplikacji w programie ASP.NET Core opiera się na parach klucz wartość ustanowione przez *dostawcy konfiguracji*.</span><span class="sxs-lookup"><span data-stu-id="11701-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="11701-106">Dostawcy konfiguracji odczytania danych konfiguracyjnych do pary klucz wartość z wielu źródeł w konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="11701-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="11701-107">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="11701-107">Azure Key Vault</span></span>
* <span data-ttu-id="11701-108">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-108">Command-line arguments</span></span>
* <span data-ttu-id="11701-109">Niestandardowi dostawcy (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="11701-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="11701-110">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="11701-110">Directory files</span></span>
* <span data-ttu-id="11701-111">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="11701-111">Environment variables</span></span>
* <span data-ttu-id="11701-112">Obiekty platformy .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="11701-112">In-memory .NET objects</span></span>
* <span data-ttu-id="11701-113">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="11701-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="11701-114">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="11701-114">Azure Key Vault</span></span>
* <span data-ttu-id="11701-115">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-115">Command-line arguments</span></span>
* <span data-ttu-id="11701-116">Niestandardowi dostawcy (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="11701-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="11701-117">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="11701-117">Environment variables</span></span>
* <span data-ttu-id="11701-118">Obiekty platformy .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="11701-118">In-memory .NET objects</span></span>
* <span data-ttu-id="11701-119">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="11701-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="11701-120">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-120">Command-line arguments</span></span>
* <span data-ttu-id="11701-121">Niestandardowi dostawcy (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="11701-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="11701-122">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="11701-122">Environment variables</span></span>
* <span data-ttu-id="11701-123">Obiekty platformy .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="11701-123">In-memory .NET objects</span></span>
* <span data-ttu-id="11701-124">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="11701-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="11701-125">*Wzorzec opcje* jest rozszerzeniem pojęć związanych z konfiguracją opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="11701-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="11701-126">Opcje używa klas do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="11701-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="11701-127">Aby uzyskać więcej informacji na temat korzystania z wzorca opcji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="11701-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="11701-128">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="11701-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="11701-129">Przykłady podane w tym temacie opierają się na:</span><span class="sxs-lookup"><span data-stu-id="11701-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="11701-130">Ustawianie ścieżki podstawowej aplikacji za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="11701-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="11701-131">`SetBasePath` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="11701-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="11701-132">Rozpoznawanie sekcji plików konfiguracji przy użyciu <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="11701-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="11701-133">`GetSection` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="11701-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="11701-134">Konfiguracja powiązania do .NET klasy za pomocą <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> i [uzyskać&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="11701-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="11701-135">`Bind` i `Get<T>` są dostępne dla aplikacji, odwołując się do [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="11701-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="11701-136">`Get<T>` jest dostępna w programie ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="11701-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="11701-137">Te trzy pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="11701-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="11701-138">Te trzy pakiety są objęte [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="11701-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="11701-139">Hostowanie i konfiguracji aplikacji</span><span class="sxs-lookup"><span data-stu-id="11701-139">Host vs. app configuration</span></span>

<span data-ttu-id="11701-140">Zanim aplikacja jest skonfigurowana i uruchomiona, *hosta* skonfigurowany i uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="11701-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="11701-141">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11701-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="11701-142">Zarówno aplikacja, jak i hosta są skonfigurowane przy użyciu dostawcy konfiguracji opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="11701-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="11701-143">Pary klucz wartość konfiguracji hosta stają się częścią globalnej konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11701-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="11701-144">Aby uzyskać więcej informacji na temat sposobu konfiguracji dostawcy są używane, gdy host jest wbudowana i wpływ źródła konfiguracji hosta konfiguracji, zobacz <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="11701-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="11701-145">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="11701-145">Security</span></span>

<span data-ttu-id="11701-146">Przyjmuje następujące najlepsze rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="11701-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="11701-147">Nigdy nie przechowują hasła lub innych danych poufnych w kodzie dostawcy konfiguracji lub w plikach konfiguracyjnych w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="11701-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="11701-148">Nie używać kluczy tajnych w środowisku produkcyjnym w trakcie opracowywania lub środowisk testowych.</span><span class="sxs-lookup"><span data-stu-id="11701-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="11701-149">Określ klucze tajne poza projektem, nie może być przypadkowo wszelkich starań, by repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="11701-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="11701-150">Dowiedz się więcej o [jak używać wielu środowisk](xref:fundamentals/environments) i zarządzanie nimi [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie programowania przy użyciu klucza tajnego Menedżera](xref:security/app-secrets) (zawiera wskazówki dotyczące używania zmiennych środowiskowych do przechowywania dane poufne).</span><span class="sxs-lookup"><span data-stu-id="11701-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="11701-151">Menedżer klucz tajny używa dostawcy konfiguracji plików do przechowywania kluczy tajnych użytkownika w pliku JSON w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="11701-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="11701-152">Dostawca konfiguracji pliku jest opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="11701-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="11701-153">[Usługa Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stanowi jedną z opcji dla bezpieczne przechowywanie kluczy tajnych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11701-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="11701-154">Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="11701-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="11701-155">Dane hierarchiczne konfiguracji</span><span class="sxs-lookup"><span data-stu-id="11701-155">Hierarchical configuration data</span></span>

<span data-ttu-id="11701-156">Interfejs API konfiguracji jest zdolny do utrzymywania dane hierarchiczne konfiguracji spłaszczając dane hierarchiczne przy użyciu ogranicznika w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="11701-157">W następującym pliku JSON cztery klucze, istnieją w ze strukturą hierarchii dwie sekcje:</span><span class="sxs-lookup"><span data-stu-id="11701-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="11701-158">Gdy plik jest do odczytu do konfiguracji, unikatowe klucze są tworzone do obsługi struktury danych hierarchicznych oryginalnego źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="11701-159">Sekcji i kluczy są spłaszczone przy użyciu dwukropek (`:`) do pierwotnej struktury obsługi:</span><span class="sxs-lookup"><span data-stu-id="11701-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="11701-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="11701-160">section0:key0</span></span>
* <span data-ttu-id="11701-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="11701-161">section0:key1</span></span>
* <span data-ttu-id="11701-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="11701-162">section1:key0</span></span>
* <span data-ttu-id="11701-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="11701-163">section1:key1</span></span>

<span data-ttu-id="11701-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> metody są dostępne do izolowania sekcje i elementy podrzędne do sekcji w danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="11701-165">Te metody są opisane w dalszej części w [GetSection GetChildren i Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="11701-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="11701-166">Konwencje</span><span class="sxs-lookup"><span data-stu-id="11701-166">Conventions</span></span>

<span data-ttu-id="11701-167">Przy uruchamianiu aplikacji źródła konfiguracji są do odczytu w kolejności, że podano ich dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="11701-168">Dostawcy konfiguracji pliku mają możliwość Załaduj ponownie konfigurację, gdy podstawowy plik ustawień zostanie zmieniona po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11701-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="11701-169">Dostawca konfiguracji pliku jest opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="11701-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="11701-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> jest dostępna w aplikacji [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="11701-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="11701-171">Dostawcy konfiguracji nie może wykorzystywać DI, ponieważ nie jest dostępna podczas konfigurowania one przez hosta.</span><span class="sxs-lookup"><span data-stu-id="11701-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="11701-172">Klucze konfiguracji przyjęcie następujących konwencji:</span><span class="sxs-lookup"><span data-stu-id="11701-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="11701-173">Kluczy jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="11701-173">Keys are case-insensitive.</span></span> <span data-ttu-id="11701-174">Na przykład `ConnectionString` i `connectionstring` są traktowane jako równoważne kluczy.</span><span class="sxs-lookup"><span data-stu-id="11701-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="11701-175">Jeśli ten sam klucz jest ustawiany przez dostawców o tej samej lub innej konfiguracji, ostatnia wartość klucza jest wartość.</span><span class="sxs-lookup"><span data-stu-id="11701-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="11701-176">Klucze hierarchicznych</span><span class="sxs-lookup"><span data-stu-id="11701-176">Hierarchical keys</span></span>
  * <span data-ttu-id="11701-177">W ramach konfiguracji interfejsu API, separator dwukropka (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="11701-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="11701-178">W zmiennych środowiskowych separator dwukropek, może nie działać na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="11701-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="11701-179">Podwójnym podkreśleniem (`__`) jest obsługiwana przez wszystkie platformy i jest konwertowany na dwukropka.</span><span class="sxs-lookup"><span data-stu-id="11701-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="11701-180">W usłudze Azure Key Vault, klucze hierarchiczne, użyj `--` (dwa łączniki) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="11701-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="11701-181">Należy podać kod, aby zastąpić kresek dwukropka po wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11701-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="11701-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Obsługuje tablice powiązania do obiektów przy użyciu tablicy indeksów w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="11701-183">Powiązanie tablicy jest opisana w [tablicy należy powiązać klasę](#bind-an-array-to-a-class) sekcji.</span><span class="sxs-lookup"><span data-stu-id="11701-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="11701-184">Wartości konfiguracji przyjęcie następujących konwencji:</span><span class="sxs-lookup"><span data-stu-id="11701-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="11701-185">Wartości są ciągami.</span><span class="sxs-lookup"><span data-stu-id="11701-185">Values are strings.</span></span>
* <span data-ttu-id="11701-186">Wartości null nie przechowywanych w konfiguracji lub powiązane z obiektami.</span><span class="sxs-lookup"><span data-stu-id="11701-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="11701-187">dostawcy</span><span class="sxs-lookup"><span data-stu-id="11701-187">Providers</span></span>

<span data-ttu-id="11701-188">W poniższej tabeli przedstawiono dostępne dla aplikacji platformy ASP.NET Core dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="11701-189">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="11701-189">Provider</span></span> | <span data-ttu-id="11701-190">Udostępnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="11701-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="11701-191">[Dostawca konfiguracji magazynu w usłudze Azure klucza](xref:security/key-vault-configuration) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="11701-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="11701-192">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="11701-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="11701-193">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="11701-194">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-194">Command-line parameters</span></span> |
| [<span data-ttu-id="11701-195">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="11701-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="11701-196">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="11701-196">Custom source</span></span> |
| [<span data-ttu-id="11701-197">Zmienne środowiskowe dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="11701-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="11701-198">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="11701-198">Environment variables</span></span> |
| [<span data-ttu-id="11701-199">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="11701-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="11701-200">Pliki INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="11701-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="11701-201">Dostawca klucza każdego pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="11701-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="11701-202">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="11701-202">Directory files</span></span> |
| [<span data-ttu-id="11701-203">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="11701-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="11701-204">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="11701-204">In-memory collections</span></span> |
| <span data-ttu-id="11701-205">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="11701-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="11701-206">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="11701-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="11701-207">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="11701-207">Provider</span></span> | <span data-ttu-id="11701-208">Udostępnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="11701-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="11701-209">[Dostawca konfiguracji magazynu w usłudze Azure klucza](xref:security/key-vault-configuration) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="11701-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="11701-210">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="11701-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="11701-211">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="11701-212">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-212">Command-line parameters</span></span> |
| [<span data-ttu-id="11701-213">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="11701-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="11701-214">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="11701-214">Custom source</span></span> |
| [<span data-ttu-id="11701-215">Zmienne środowiskowe dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="11701-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="11701-216">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="11701-216">Environment variables</span></span> |
| [<span data-ttu-id="11701-217">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="11701-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="11701-218">Pliki INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="11701-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="11701-219">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="11701-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="11701-220">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="11701-220">In-memory collections</span></span> |
| <span data-ttu-id="11701-221">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="11701-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="11701-222">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="11701-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="11701-223">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="11701-223">Provider</span></span> | <span data-ttu-id="11701-224">Udostępnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="11701-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="11701-225">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="11701-226">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-226">Command-line parameters</span></span> |
| [<span data-ttu-id="11701-227">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="11701-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="11701-228">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="11701-228">Custom source</span></span> |
| [<span data-ttu-id="11701-229">Zmienne środowiskowe dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="11701-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="11701-230">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="11701-230">Environment variables</span></span> |
| [<span data-ttu-id="11701-231">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="11701-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="11701-232">Pliki INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="11701-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="11701-233">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="11701-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="11701-234">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="11701-234">In-memory collections</span></span> |
| <span data-ttu-id="11701-235">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="11701-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="11701-236">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="11701-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="11701-237">Źródła konfiguracji są do odczytu w kolejności określono ich dostawców konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="11701-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="11701-238">Dostawcy konfiguracji opisanych w tym temacie są opisane w kolejności alfabetycznej, a nie w kolejności, Twój kod może zorganizować ich.</span><span class="sxs-lookup"><span data-stu-id="11701-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="11701-239">Kolejność dostawców konfiguracji w kodzie do własnych priorytetów do bazowego źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="11701-240">Jest zwykle kolejność dostawców konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="11701-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="11701-241">Pliki (*appsettings.json*, *appsettings.&lt; Środowisko&gt;.json*, gdzie `<Environment>` jest bieżące środowisko hostingu aplikacji)</span><span class="sxs-lookup"><span data-stu-id="11701-241">Files (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, where `<Environment>` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="11701-242">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym tylko)</span><span class="sxs-lookup"><span data-stu-id="11701-242">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="11701-243">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="11701-243">Environment variables</span></span>
1. <span data-ttu-id="11701-244">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-244">Command-line arguments</span></span>

<span data-ttu-id="11701-245">Jest to powszechną praktyką do ostatniej pozycji dostawcę konfiguracji wiersza polecenia w serii dostawcy, aby zezwolić na argumenty wiersza polecenia zastąpić konfiguracji ustawione przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="11701-245">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-246">Ta sekwencja dostawcy są umieszczane w miejscu, podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="11701-246">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="11701-247">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="11701-247">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="11701-248">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta sieci Web w celu określenia dostawcy konfiguracji aplikacji:</span><span class="sxs-lookup"><span data-stu-id="11701-248">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="11701-249">`ConfigureAppConfiguration` *jest dostępna w programie ASP.NET Core 2.1 lub nowszej.*</span><span class="sxs-lookup"><span data-stu-id="11701-249">`ConfigureAppConfiguration` *is available in ASP.NET Core 2.1 or later.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-250">Ta sekwencja dostawcy mogą być tworzone dla aplikacji (nie hosta) za pomocą <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> i wywołanie w celu jego <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in Class metoda `Startup`:</span><span class="sxs-lookup"><span data-stu-id="11701-250">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

<span data-ttu-id="11701-251">W powyższym przykładzie nazwa środowiska (`env.EnvironmentName`) i nazwę zestawu aplikacji (`env.ApplicationName`) są dostarczane przez <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="11701-251">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="11701-252">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="11701-252">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="11701-253">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-253">Command-line Configuration Provider</span></span>

<span data-ttu-id="11701-254"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Ładuje konfiguracji z pary klucz wartość argumentu wiersza polecenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="11701-254">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-255">Aby uaktywnić konfigurację wiersza polecenia, należy wywołać <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="11701-255">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="11701-256">`AddCommandLine` jest wywoływana automatycznie podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="11701-256">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="11701-257">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="11701-257">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="11701-258">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="11701-258">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="11701-259">Konfiguracja opcjonalna z *appsettings.json* i *appsettings.&lt; Środowisko&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="11701-259">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="11701-260">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="11701-260">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="11701-261">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="11701-261">Environment variables.</span></span>

<span data-ttu-id="11701-262">`CreateDefaultBuilder` dodaje ostatniego wiersza polecenia dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-262">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="11701-263">Argumenty wiersza polecenia przekazywane w czasie wykonywania zastąpić konfiguracji ustawione przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="11701-263">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="11701-264">`CreateDefaultBuilder` działa, gdy host jest konstruowany.</span><span class="sxs-lookup"><span data-stu-id="11701-264">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="11701-265">W związku z tym, wiersza polecenia konfiguracji aktywowany przez `CreateDefaultBuilder` mogą mieć wpływ na sposób host jest skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="11701-265">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="11701-266">Podczas tworzenia hosta ręcznie i nie wywołuje metody `CreateDefaultBuilder`, wywołaj `AddCommandLine` metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="11701-266">When building the host manually and not calling `CreateDefaultBuilder`, call the `AddCommandLine` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-267">Aby uaktywnić konfigurację wiersza polecenia, należy wywołać <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="11701-267">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="11701-268">Wywołanie dostawcy ostatnio, aby umożliwić argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-268">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="11701-269">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="11701-269">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="11701-270">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="11701-270">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-271">2.x Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta, który zawiera wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="11701-271">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-272">Wywołania aplikacji przykładowej 1.x <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> na <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="11701-272">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="11701-273">Otwórz wiersz polecenia w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="11701-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="11701-274">Argument wiersza polecenia, aby podać `dotnet run` polecenia `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="11701-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="11701-275">Po uruchomieniu aplikacji otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="11701-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="11701-276">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla argumentu wiersza polecenia konfiguracji udostępnionego `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="11701-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="11701-277">Argumenty</span><span class="sxs-lookup"><span data-stu-id="11701-277">Arguments</span></span>

<span data-ttu-id="11701-278">Wartość musi stosować się znak równości (`=`), lub klucz musi mieć prefiks (`--` lub `/`) Jeśli wartość jest zgodna spację.</span><span class="sxs-lookup"><span data-stu-id="11701-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="11701-279">Wartość może być null, jeśli jest używany znak równości (na przykład `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="11701-279">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="11701-280">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="11701-280">Key prefix</span></span>               | <span data-ttu-id="11701-281">Przykład</span><span class="sxs-lookup"><span data-stu-id="11701-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="11701-282">Nie prefiksu</span><span class="sxs-lookup"><span data-stu-id="11701-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="11701-283">Dwa łączniki (`--`)</span><span class="sxs-lookup"><span data-stu-id="11701-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="11701-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="11701-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="11701-285">Kreski ułamkowej (`/`)</span><span class="sxs-lookup"><span data-stu-id="11701-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="11701-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="11701-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="11701-287">W tym samym poleceniu nie Mieszaj argument wiersza polecenia pary klucz wartość, które znakiem równości za pomocą pary klucz wartość, korzystających z przestrzeni.</span><span class="sxs-lookup"><span data-stu-id="11701-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="11701-288">Przykładowe polecenia:</span><span class="sxs-lookup"><span data-stu-id="11701-288">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="11701-289">Przełącz mapowania</span><span class="sxs-lookup"><span data-stu-id="11701-289">Switch mappings</span></span>

<span data-ttu-id="11701-290">Przełącznik jest transformowanie na nazwę klucza wymiany logiki.</span><span class="sxs-lookup"><span data-stu-id="11701-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="11701-291">Podczas ręcznego tworzenia konfiguracji za pomocą <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, może zapewnić słownika zastępstw przełącznika <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metody.</span><span class="sxs-lookup"><span data-stu-id="11701-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="11701-292">W przypadku przełącznika słownik mapowań słownika jest sprawdzane dla klucza, który pasuje do klucza, dostarczone przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="11701-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="11701-293">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) jest przekazywana do zestawu pary klucz wartość do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11701-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="11701-294">Mapowanie przełącznika jest wymagane dla dowolnego klucz wiersza polecenia poprzedzone kreską pojedynczego (`-`).</span><span class="sxs-lookup"><span data-stu-id="11701-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="11701-295">Przełącz mapowania słownika kluczowych reguł:</span><span class="sxs-lookup"><span data-stu-id="11701-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="11701-296">Przełączniki musi rozpoczynać się kreską (`-`) lub podwójne kreska (`--`).</span><span class="sxs-lookup"><span data-stu-id="11701-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="11701-297">Słownik mapowań przełącznik nie może zawierać zduplikowane klucze.</span><span class="sxs-lookup"><span data-stu-id="11701-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="11701-298">Jak pokazano w powyższym przykładzie wywołanie `CreateDefaultBuilder` nie należy przekazywać argumenty, gdy przełącznik mapowania są używane.</span><span class="sxs-lookup"><span data-stu-id="11701-298">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="11701-299">`CreateDefaultBuilder` metody `AddCommandLine` wywołania nie zawiera zamapowanego przełączników, a nie istnieje sposób do przekazania słownik mapowania przełącznika `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="11701-299">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="11701-300">Jeśli argumenty zamapowanego przełącznik dołączania i są przekazywane do `CreateDefaultBuilder`, jego `AddCommandLine` dostawcy nie uda się zainicjowanie z <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="11701-300">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="11701-301">Rozwiązanie nie ma przekazywania argumentów do `CreateDefaultBuilder` , ale zamiast tego umożliwiające `ConfigurationBuilder` metody `AddCommandLine` metody do przetwarzania argumentów i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="11701-301">The solution is not to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

<span data-ttu-id="11701-302">Po utworzeniu przełącznika słownik mapowań zawiera dane wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="11701-302">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="11701-303">Key</span><span class="sxs-lookup"><span data-stu-id="11701-303">Key</span></span>       | <span data-ttu-id="11701-304">Wartość</span><span class="sxs-lookup"><span data-stu-id="11701-304">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="11701-305">Jeśli mapowane przełącznika klucze są używane podczas uruchamiania aplikacji, konfiguracji odbiera wartości konfiguracji na kluczu pochodzącego ze słownika:</span><span class="sxs-lookup"><span data-stu-id="11701-305">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="11701-306">Po uruchomieniu polecenia poprzedniej konfiguracji zawiera wartości podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="11701-306">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="11701-307">Key</span><span class="sxs-lookup"><span data-stu-id="11701-307">Key</span></span>               | <span data-ttu-id="11701-308">Wartość</span><span class="sxs-lookup"><span data-stu-id="11701-308">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="11701-309">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="11701-309">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="11701-310"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Ładuje konfiguracji ze środowiska zmiennej pary klucz wartość w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="11701-310">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="11701-311">Aby aktywować środowisko zmiennych konfiguracji, należy wywołać <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="11701-311">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="11701-312">Podczas pracy z kluczami hierarchiczne w zmiennych środowiskowych, separator dwukropek (`:`) może nie działać na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="11701-312">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="11701-313">Podwójnym podkreśleniem (`__`) jest obsługiwana przez wszystkie platformy i zastępuje dwukropka.</span><span class="sxs-lookup"><span data-stu-id="11701-313">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="11701-314">[Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) pozwala na Ustawianie zmiennych środowiskowych w portalu Azure, które mogą zastąpić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="11701-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="11701-315">Aby uzyskać więcej informacji, zobacz [aplikacje platformy Azure: zastępczą konfigurację aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="11701-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-316">`AddEnvironmentVariables` jest wywoływana automatycznie podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="11701-316">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="11701-317">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="11701-317">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="11701-318">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="11701-318">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="11701-319">Konfiguracja opcjonalna z *appsettings.json* i *appsettings.&lt; Środowisko&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="11701-319">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="11701-320">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="11701-320">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="11701-321">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="11701-321">Command-line arguments.</span></span>

<span data-ttu-id="11701-322">Dostawca konfiguracji zmiennych środowiskowych jest wywoływana po konfiguracji zostanie nawiązane z wpisami tajnymi użytkowników i *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="11701-322">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="11701-323">W tym miejscu podczas wywoływania dostawcy umożliwia zmienne środowiskowe są odczytywane w czasie wykonywania do zastępowania konfiguracji ustawione przez użytkownika hasła i *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="11701-323">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="11701-324">Możesz również bezpośrednio wywołać `AddEnvironmentVariables` metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="11701-324">You can also directly call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-325">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="11701-325">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="11701-326">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="11701-326">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-327">2.x Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta, który zawiera wywołanie `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="11701-327">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-328">Wywołania aplikacji przykładowej 1.x `AddEnvironmentVariables` na `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="11701-328">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="11701-329">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="11701-329">Run the sample app.</span></span> <span data-ttu-id="11701-330">Otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="11701-330">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="11701-331">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla zmiennej środowiskowej `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="11701-331">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="11701-332">Wartość odzwierciedla środowisko, w którym aplikacja jest uruchomiona, zwykle `Development` podczas uruchamiania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="11701-332">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="11701-333">Przechowywać listę zmiennych środowiskowych renderowany przez aplikację krótki, aplikacji filtry zmienne środowiskowe do tych rozpoczynających się od następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="11701-333">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="11701-334">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="11701-334">ASPNETCORE_</span></span>
* <span data-ttu-id="11701-335">adresy URL</span><span class="sxs-lookup"><span data-stu-id="11701-335">urls</span></span>
* <span data-ttu-id="11701-336">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="11701-336">Logging</span></span>
* <span data-ttu-id="11701-337">ŚRODOWISKO</span><span class="sxs-lookup"><span data-stu-id="11701-337">ENVIRONMENT</span></span>
* <span data-ttu-id="11701-338">contentRoot</span><span class="sxs-lookup"><span data-stu-id="11701-338">contentRoot</span></span>
* <span data-ttu-id="11701-339">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="11701-339">AllowedHosts</span></span>
* <span data-ttu-id="11701-340">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="11701-340">applicationName</span></span>
* <span data-ttu-id="11701-341">Wiersz polecenia</span><span class="sxs-lookup"><span data-stu-id="11701-341">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-342">Jeśli chcesz udostępnić wszystkie zmienne środowiskowe, dostępne dla aplikacji, zmień `FilteredConfiguration` w *Pages/Index.cshtml.cs* do następującego:</span><span class="sxs-lookup"><span data-stu-id="11701-342">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-343">Jeśli chcesz udostępnić wszystkie zmienne środowiskowe, dostępne dla aplikacji, zmień `FilteredConfiguration` w *Controllers/HomeController.cs* do następującego:</span><span class="sxs-lookup"><span data-stu-id="11701-343">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="11701-344">Prefiksy</span><span class="sxs-lookup"><span data-stu-id="11701-344">Prefixes</span></span>

<span data-ttu-id="11701-345">Zmienne środowiskowe ładowane z konfiguracji aplikacji są filtrowane podczas podawania prefiks `AddEnvironmentVariables` metody.</span><span class="sxs-lookup"><span data-stu-id="11701-345">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="11701-346">Na przykład, aby filtr zmiennych środowiskowych na prefiksie `CUSTOM_`, podaj prefiks, który dostawca konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="11701-346">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="11701-347">Prefiks jest odłączany podczas tworzenia pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-347">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-348">Statyczne wygodna metoda `CreateDefaultBuilder` tworzy <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> Aby ustanowić hosta aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11701-348">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="11701-349">Gdy `WebHostBuilder` jest tworzony, znajdzie jego konfigurację hosta w zmiennych środowiskowych z prefiksem `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="11701-349">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="11701-350">**Prefiksy ciąg połączenia**</span><span class="sxs-lookup"><span data-stu-id="11701-350">**Connection string prefixes**</span></span>

<span data-ttu-id="11701-351">Interfejs API konfiguracji zawiera reguły jest przetwarzana w specjalny cztery połączenia ciągu zmiennych środowiskowych zajmujących się Konfigurowanie parametrów połączenia platformy Azure dla środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11701-351">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="11701-352">Zmienne środowiskowe z prefiksami pokazano w tabeli są ładowane do aplikacji, jeśli nie dostarczono żadnego prefiksu do `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="11701-352">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="11701-353">Prefiks ciągu połączenia</span><span class="sxs-lookup"><span data-stu-id="11701-353">Connection string prefix</span></span> | <span data-ttu-id="11701-354">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="11701-354">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="11701-355">Dostawcy niestandardowego</span><span class="sxs-lookup"><span data-stu-id="11701-355">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="11701-356">MySQL</span><span class="sxs-lookup"><span data-stu-id="11701-356">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="11701-357">Usługa Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="11701-357">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="11701-358">SQL Server</span><span class="sxs-lookup"><span data-stu-id="11701-358">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="11701-359">Gdy zmienna środowiskowa są odnajdywane i ładowane do konfiguracji za pomocą dowolnego z czterech prefiksy z tabelą:</span><span class="sxs-lookup"><span data-stu-id="11701-359">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="11701-360">Klucz konfiguracji jest tworzony, usuwając prefiks zmiennej środowiska i dodając kluczowych sekcję konfiguracji (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="11701-360">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="11701-361">Tworzony jest nową parę klucz wartość konfiguracji, który reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, który nie ma określonego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="11701-361">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="11701-362">Klucz zmiennych środowiska</span><span class="sxs-lookup"><span data-stu-id="11701-362">Environment variable key</span></span> | <span data-ttu-id="11701-363">Klucz konfiguracji przekonwertowany</span><span class="sxs-lookup"><span data-stu-id="11701-363">Converted configuration key</span></span> | <span data-ttu-id="11701-364">Pozycja konfiguracji dostawcy</span><span class="sxs-lookup"><span data-stu-id="11701-364">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="11701-365">Wpis konfiguracyjny nie utworzony.</span><span class="sxs-lookup"><span data-stu-id="11701-365">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="11701-366">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="11701-366">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="11701-367">Wartość: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="11701-367">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="11701-368">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="11701-368">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="11701-369">Wartość: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="11701-369">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="11701-370">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="11701-370">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="11701-371">Wartość: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="11701-371">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="11701-372">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="11701-372">File Configuration Provider</span></span>

<span data-ttu-id="11701-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową dla ładowania konfiguracji z systemu plików.</span><span class="sxs-lookup"><span data-stu-id="11701-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="11701-374">Następujących dostawców konfiguracji są przeznaczone dla określonych typów plików:</span><span class="sxs-lookup"><span data-stu-id="11701-374">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="11701-375">Dostawca konfiguracji INI</span><span class="sxs-lookup"><span data-stu-id="11701-375">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="11701-376">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="11701-376">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="11701-377">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="11701-377">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="11701-378">Dostawca konfiguracji INI</span><span class="sxs-lookup"><span data-stu-id="11701-378">INI Configuration Provider</span></span>

<span data-ttu-id="11701-379"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Ładuje konfiguracji z pary klucz wartość pliku INI w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="11701-379">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="11701-380">Aby aktywować konfiguracji pliku INI, należy wywołać <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="11701-380">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="11701-381">Dwukropek może służyć do jako ogranicznik sekcji w konfiguracji pliku INI.</span><span class="sxs-lookup"><span data-stu-id="11701-381">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="11701-382">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="11701-382">Overloads permit specifying:</span></span>

* <span data-ttu-id="11701-383">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="11701-383">Whether the file is optional.</span></span>
* <span data-ttu-id="11701-384">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="11701-384">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="11701-385"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="11701-385">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-386">Podczas wywoływania `CreateDefaultBuilder`, wywołaj `UseConfiguration` z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="11701-386">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="11701-387">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc `UseConfiguration` z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="11701-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-388">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="11701-388">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="11701-389">Przykładowy plik konfiguracji INI:</span><span class="sxs-lookup"><span data-stu-id="11701-389">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="11701-390">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="11701-390">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="11701-391">section0:key0</span><span class="sxs-lookup"><span data-stu-id="11701-391">section0:key0</span></span>
* <span data-ttu-id="11701-392">section0:key1</span><span class="sxs-lookup"><span data-stu-id="11701-392">section0:key1</span></span>
* <span data-ttu-id="11701-393">section1:Subsection:key</span><span class="sxs-lookup"><span data-stu-id="11701-393">section1:subsection:key</span></span>
* <span data-ttu-id="11701-394">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="11701-394">section2:subsection0:key</span></span>
* <span data-ttu-id="11701-395">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="11701-395">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="11701-396">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="11701-396">JSON Configuration Provider</span></span>

<span data-ttu-id="11701-397"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Ładuje konfiguracji z pary klucz wartość plik JSON podczas wykonywania.</span><span class="sxs-lookup"><span data-stu-id="11701-397">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="11701-398">Aby aktywować konfiguracji pliku JSON, należy wywołać <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="11701-398">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="11701-399">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="11701-399">Overloads permit specifying:</span></span>

* <span data-ttu-id="11701-400">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="11701-400">Whether the file is optional.</span></span>
* <span data-ttu-id="11701-401">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="11701-401">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="11701-402"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="11701-402">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-403">`AddJsonFile` jest wywoływana automatycznie, dwa razy, podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="11701-403">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="11701-404">Metoda jest wywoływana, aby załadować konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="11701-404">The method is called to load configuration from:</span></span>

* <span data-ttu-id="11701-405">*appSettings.JSON* &ndash; ten plik jest najpierw do odczytu.</span><span class="sxs-lookup"><span data-stu-id="11701-405">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="11701-406">Wersja środowiska pliku można zastąpić wartości dostarczone przez *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="11701-406">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="11701-407">*appSettings. &lt;Środowiska&gt;.json* &ndash; środowiska wersję pliku zostanie załadowany na podstawie [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="11701-407">*appsettings.&lt;Environment&gt;.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="11701-408">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="11701-408">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="11701-409">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="11701-409">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="11701-410">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="11701-410">Environment variables.</span></span>
* <span data-ttu-id="11701-411">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="11701-411">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="11701-412">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="11701-412">Command-line arguments.</span></span>

<span data-ttu-id="11701-413">Dostawca konfiguracji JSON jest ustanawiane, najpierw.</span><span class="sxs-lookup"><span data-stu-id="11701-413">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="11701-414">W związku z tym, wpisami tajnymi użytkowników, zmienne środowiskowe i argumenty wiersza polecenia zastępuje konfiguracji ustawione przez *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="11701-414">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="11701-415">Możesz również bezpośrednio wywołać `AddJsonFile` metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="11701-415">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="11701-416">Podczas wywoływania `CreateDefaultBuilder`, wywołaj `UseConfiguration` z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="11701-416">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="11701-417">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc `UseConfiguration` z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="11701-417">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-418">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="11701-418">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="11701-419">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="11701-419">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-420">2.x Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do hosta, który obejmuje dwa wywołania do tworzenia `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="11701-420">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="11701-421">Konfiguracja została załadowana z *appsettings.json* i *appsettings.&lt; Środowisko&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="11701-421">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-422">Wywołania aplikacji przykładowej 1.x `AddJsonFile` dwa razy na `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="11701-422">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="11701-423">Konfiguracja została załadowana z *appsettings.json* i *appsettings.&lt; Środowisko&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="11701-423">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="11701-424">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="11701-424">Run the sample app.</span></span> <span data-ttu-id="11701-425">Otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="11701-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="11701-426">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla konfiguracji, jak pokazano w tabeli, w zależności od środowiska.</span><span class="sxs-lookup"><span data-stu-id="11701-426">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="11701-427">Klucze konfiguracji rejestrowania, użyj dwukropka (`:`) jako separator hierarchicznej.</span><span class="sxs-lookup"><span data-stu-id="11701-427">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="11701-428">Key</span><span class="sxs-lookup"><span data-stu-id="11701-428">Key</span></span>                        | <span data-ttu-id="11701-429">Wartość rozwoju</span><span class="sxs-lookup"><span data-stu-id="11701-429">Development Value</span></span> | <span data-ttu-id="11701-430">Wartość produkcji</span><span class="sxs-lookup"><span data-stu-id="11701-430">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="11701-431">Rejestrowanie: LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="11701-431">Logging:LogLevel:System</span></span>    | <span data-ttu-id="11701-432">Informacje</span><span class="sxs-lookup"><span data-stu-id="11701-432">Information</span></span>       | <span data-ttu-id="11701-433">Informacje</span><span class="sxs-lookup"><span data-stu-id="11701-433">Information</span></span>      |
| <span data-ttu-id="11701-434">Rejestrowanie: LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="11701-434">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="11701-435">Informacje</span><span class="sxs-lookup"><span data-stu-id="11701-435">Information</span></span>       | <span data-ttu-id="11701-436">Informacje</span><span class="sxs-lookup"><span data-stu-id="11701-436">Information</span></span>      |
| <span data-ttu-id="11701-437">Rejestrowanie: LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="11701-437">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="11701-438">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="11701-438">Debug</span></span>             | <span data-ttu-id="11701-439">Błąd</span><span class="sxs-lookup"><span data-stu-id="11701-439">Error</span></span>            |
| <span data-ttu-id="11701-440">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="11701-440">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="11701-441">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="11701-441">XML Configuration Provider</span></span>

<span data-ttu-id="11701-442"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Ładuje konfiguracji z pary klucz wartość plik XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="11701-442">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="11701-443">Aby aktywować Konfiguracja pliku XML, należy wywołać <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="11701-443">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="11701-444">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="11701-444">Overloads permit specifying:</span></span>

* <span data-ttu-id="11701-445">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="11701-445">Whether the file is optional.</span></span>
* <span data-ttu-id="11701-446">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="11701-446">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="11701-447"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="11701-447">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="11701-448">Węzeł główny plik konfiguracji jest ignorowany podczas tworzenia pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-448">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="11701-449">Określać definicji typu dokumentu (DTD) lub przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="11701-449">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-450">Podczas wywoływania `CreateDefaultBuilder`, wywołaj `UseConfiguration` z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="11701-450">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="11701-451">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc `UseConfiguration` z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="11701-451">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-452">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="11701-452">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="11701-453">Pliki konfiguracji XML można użyć nazw unikatowych elementów dla powtarzających się sekcje:</span><span class="sxs-lookup"><span data-stu-id="11701-453">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="11701-454">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="11701-454">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="11701-455">section0:key0</span><span class="sxs-lookup"><span data-stu-id="11701-455">section0:key0</span></span>
* <span data-ttu-id="11701-456">section0:key1</span><span class="sxs-lookup"><span data-stu-id="11701-456">section0:key1</span></span>
* <span data-ttu-id="11701-457">section1:key0</span><span class="sxs-lookup"><span data-stu-id="11701-457">section1:key0</span></span>
* <span data-ttu-id="11701-458">section1:key1</span><span class="sxs-lookup"><span data-stu-id="11701-458">section1:key1</span></span>

<span data-ttu-id="11701-459">Powtarzające się elementy, które używają takiej samej nazwie elementu pracy, jeśli `name` atrybut jest używany do odróżniania elementów:</span><span class="sxs-lookup"><span data-stu-id="11701-459">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="11701-460">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="11701-460">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="11701-461">sekcja: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="11701-461">section:section0:key:key0</span></span>
* <span data-ttu-id="11701-462">sekcja: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="11701-462">section:section0:key:key1</span></span>
* <span data-ttu-id="11701-463">sekcja: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="11701-463">section:section1:key:key0</span></span>
* <span data-ttu-id="11701-464">sekcja: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="11701-464">section:section1:key:key1</span></span>

<span data-ttu-id="11701-465">Atrybuty można podać wartości:</span><span class="sxs-lookup"><span data-stu-id="11701-465">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="11701-466">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="11701-466">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="11701-467">klucz: atrybut</span><span class="sxs-lookup"><span data-stu-id="11701-467">key:attribute</span></span>
* <span data-ttu-id="11701-468">sekcja: klucz: atrybut</span><span class="sxs-lookup"><span data-stu-id="11701-468">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="11701-469">Dostawca klucza każdego pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="11701-469">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="11701-470"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Korzysta z plików w katalogu jako pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-470">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="11701-471">Klucz jest nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="11701-471">The key is the file name.</span></span> <span data-ttu-id="11701-472">Wartość zawiera zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="11701-472">The value contains the file's contents.</span></span> <span data-ttu-id="11701-473">Dostawca konfiguracji klucza każdego pliku jest używany w scenariuszach hostingu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="11701-473">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="11701-474">Aby aktywować klucza dla pliku konfiguracji, należy wywołać <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="11701-474">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="11701-475">`directoryPath` Do plików musi być ścieżką bezwzględną.</span><span class="sxs-lookup"><span data-stu-id="11701-475">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="11701-476">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="11701-476">Overloads permit specifying:</span></span>

* <span data-ttu-id="11701-477">`Action<KeyPerFileConfigurationSource>` Delegat, który konfiguruje źródło.</span><span class="sxs-lookup"><span data-stu-id="11701-477">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="11701-478">Czy katalog jest opcjonalna i ścieżkę do katalogu.</span><span class="sxs-lookup"><span data-stu-id="11701-478">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="11701-479">Podczas wywoływania `CreateDefaultBuilder`, wywołaj `UseConfiguration` z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="11701-479">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
        var config = new ConfigurationBuilder()
            .AddKeyPerFile(directoryPath: path, optional: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="11701-480">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc `UseConfiguration` z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="11701-480">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a><span data-ttu-id="11701-481">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="11701-481">Memory Configuration Provider</span></span>

<span data-ttu-id="11701-482"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Korzysta z kolekcji w pamięci jako pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-482">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="11701-483">Aby aktywować konfiguracji kolekcji w pamięci, należy wywołać <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="11701-483">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="11701-484">Dostawca konfiguracji mogą być zainicjowane z `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="11701-484">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-485">Podczas wywoływania `CreateDefaultBuilder`, wywołaj `UseConfiguration` z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="11701-485">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

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
}
```

<span data-ttu-id="11701-486">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc `UseConfiguration` z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="11701-486">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-487">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="11701-487">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a><span data-ttu-id="11701-488">GetValue</span><span class="sxs-lookup"><span data-stu-id="11701-488">GetValue</span></span>

<span data-ttu-id="11701-489">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnianie wartości z konfiguracji z określonym kluczem i konwertuje je do określonego typu.</span><span class="sxs-lookup"><span data-stu-id="11701-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="11701-490">Przeciążenie umożliwia podanie wartości domyślnej, jeśli nie odnaleziono klucza.</span><span class="sxs-lookup"><span data-stu-id="11701-490">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="11701-491">Poniższy przykład wyodrębnia wartość ciągu z konfiguracji przy użyciu klucza `NumberKey`, typy wartości jako `int`i przechowuje wartość w zmiennej `intValue`.</span><span class="sxs-lookup"><span data-stu-id="11701-491">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="11701-492">Jeśli `NumberKey` nie zostanie odnaleziona w klucze konfiguracji `intValue` odbiera wartość domyślną `99`:</span><span class="sxs-lookup"><span data-stu-id="11701-492">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="11701-493">GetSection, GetChildren i istnieje</span><span class="sxs-lookup"><span data-stu-id="11701-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="11701-494">Aby uzyskać przykłady, które należy wykonać należy wziąć pod uwagę następujący plik JSON.</span><span class="sxs-lookup"><span data-stu-id="11701-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="11701-495">Cztery klucze znajdują się na dwie sekcje, z których jedna zawiera parę podsekcje:</span><span class="sxs-lookup"><span data-stu-id="11701-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="11701-496">Gdy plik jest do odczytu do konfiguracji, następujące klucze unikatowe hierarchiczne są tworzone do przechowywania wartości konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="11701-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="11701-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="11701-497">section0:key0</span></span>
* <span data-ttu-id="11701-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="11701-498">section0:key1</span></span>
* <span data-ttu-id="11701-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="11701-499">section1:key0</span></span>
* <span data-ttu-id="11701-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="11701-500">section1:key1</span></span>
* <span data-ttu-id="11701-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="11701-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="11701-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="11701-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="11701-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="11701-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="11701-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="11701-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="11701-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="11701-505">GetSection</span></span>

<span data-ttu-id="11701-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcji konfiguracji z kluczem określonym podsekcji.</span><span class="sxs-lookup"><span data-stu-id="11701-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="11701-507">Aby zwrócić <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz wartość w `section1`, wywołaj `GetSection` i podaj nazwę sekcji:</span><span class="sxs-lookup"><span data-stu-id="11701-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="11701-508">Podobnie Aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę do sekcji:</span><span class="sxs-lookup"><span data-stu-id="11701-508">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="11701-509">`GetSection` nigdy nie zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="11701-509">`GetSection` never returns `null`.</span></span> <span data-ttu-id="11701-510">Jeśli nie zostanie znaleziona pasująca sekcja, pusta `IConfigurationSection` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="11701-510">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="11701-511">GetChildren —</span><span class="sxs-lookup"><span data-stu-id="11701-511">GetChildren</span></span>

<span data-ttu-id="11701-512">Wywołanie [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) na `section2` uzyskuje `IEnumerable<IConfigurationSection>` zawierającej:</span><span class="sxs-lookup"><span data-stu-id="11701-512">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="11701-513">Istnieje</span><span class="sxs-lookup"><span data-stu-id="11701-513">Exists</span></span>

<span data-ttu-id="11701-514">Użyj [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) do ustalenia, czy istnieje sekcji konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="11701-514">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="11701-515">Podane dane przykładowe `sectionExists` jest `false` , ponieważ nie ma `section2:subsection2` sekcji w danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-515">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="11701-516">Powiąż z klasy</span><span class="sxs-lookup"><span data-stu-id="11701-516">Bind to a class</span></span>

<span data-ttu-id="11701-517">Konfiguracja może być powiązana z klas, które reprezentują grupy powiązane ustawienia za pomocą *wzorzec opcje*.</span><span class="sxs-lookup"><span data-stu-id="11701-517">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="11701-518">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="11701-518">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="11701-519">Wartości konfiguracji są zwracane jako ciągi, ale wywoływania <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) obiektów.</span><span class="sxs-lookup"><span data-stu-id="11701-519">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="11701-520">Przykładowa aplikacja zawiera `Starship` modelu (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="11701-520">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="11701-521">`starship` Części *starship.json* plików umożliwia utworzenie konfiguracji podczas Przykładowa aplikacja używa dostawcy konfiguracji JSON można załadować konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="11701-521">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="11701-522">Tworzone są następujące pary klucz wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="11701-522">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="11701-523">Key</span><span class="sxs-lookup"><span data-stu-id="11701-523">Key</span></span>                   | <span data-ttu-id="11701-524">Wartość</span><span class="sxs-lookup"><span data-stu-id="11701-524">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="11701-525">starship: Nazwa</span><span class="sxs-lookup"><span data-stu-id="11701-525">starship:name</span></span>         | <span data-ttu-id="11701-526">USS przedsiębiorstwa</span><span class="sxs-lookup"><span data-stu-id="11701-526">USS Enterprise</span></span>                                    |
| <span data-ttu-id="11701-527">starship: rejestru</span><span class="sxs-lookup"><span data-stu-id="11701-527">starship:registry</span></span>     | <span data-ttu-id="11701-528">KCF 1701</span><span class="sxs-lookup"><span data-stu-id="11701-528">NCC-1701</span></span>                                          |
| <span data-ttu-id="11701-529">starship: klasy</span><span class="sxs-lookup"><span data-stu-id="11701-529">starship:class</span></span>        | <span data-ttu-id="11701-530">Tworzenia</span><span class="sxs-lookup"><span data-stu-id="11701-530">Constitution</span></span>                                      |
| <span data-ttu-id="11701-531">starship: długość</span><span class="sxs-lookup"><span data-stu-id="11701-531">starship:length</span></span>       | <span data-ttu-id="11701-532">304.8</span><span class="sxs-lookup"><span data-stu-id="11701-532">304.8</span></span>                                             |
| <span data-ttu-id="11701-533">starship: upoważnione</span><span class="sxs-lookup"><span data-stu-id="11701-533">starship:commissioned</span></span> | <span data-ttu-id="11701-534">False</span><span class="sxs-lookup"><span data-stu-id="11701-534">False</span></span>                                             |
| <span data-ttu-id="11701-535">Znak towarowy</span><span class="sxs-lookup"><span data-stu-id="11701-535">trademark</span></span>             | <span data-ttu-id="11701-536">Paramount obrazy Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="11701-536">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="11701-537">Wywołania aplikacji przykładowej `GetSection` z `starship` klucza.</span><span class="sxs-lookup"><span data-stu-id="11701-537">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="11701-538">`starship` Pary klucz wartość są izolowane.</span><span class="sxs-lookup"><span data-stu-id="11701-538">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="11701-539">`Bind` Metoda jest wywoływana w podsekcji, przekazując wystąpienie `Starship` klasy.</span><span class="sxs-lookup"><span data-stu-id="11701-539">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="11701-540">Po powiązaniu wartości wystąpienia, wystąpienie jest przypisany do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="11701-540">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="11701-541">Powiąż z wykresu obiektu</span><span class="sxs-lookup"><span data-stu-id="11701-541">Bind to an object graph</span></span>

<span data-ttu-id="11701-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> jest w stanie całego grafu obiektów POCO powiązania.</span><span class="sxs-lookup"><span data-stu-id="11701-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="11701-543">Przykładowy zawiera `TvShow` model zawiera wykres obiektu, którego `Metadata` i `Actors` klasy (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="11701-543">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="11701-544">Przykładowa aplikacja ma *tvshow.xml* plik zawierający dane konfiguracyjne:</span><span class="sxs-lookup"><span data-stu-id="11701-544">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="11701-545">Konfiguracja jest powiązany z całej `TvShow` wykres obiektu z `Bind` metody.</span><span class="sxs-lookup"><span data-stu-id="11701-545">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="11701-546">Powiązane wystąpienia jest przypisywany do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="11701-546">The bound instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="11701-547">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) wiąże i zwraca wartość określonego typu.</span><span class="sxs-lookup"><span data-stu-id="11701-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="11701-548">`Get<T>` jest bardziej wygodne niż przy użyciu `Bind`.</span><span class="sxs-lookup"><span data-stu-id="11701-548">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="11701-549">Poniższy kod przedstawia sposób użycia `Get<T>` w poprzednim przykładzie, co umożliwia wystąpienie powiązanych, którego można przypisać bezpośrednio do właściwości, używany do renderowania:</span><span class="sxs-lookup"><span data-stu-id="11701-549">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="11701-550">Powiąż tablicę do klasy</span><span class="sxs-lookup"><span data-stu-id="11701-550">Bind an array to a class</span></span>

<span data-ttu-id="11701-551">*Przykładowa aplikacja pokazuje pojęcia opisane w tej sekcji.*</span><span class="sxs-lookup"><span data-stu-id="11701-551">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="11701-552"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Obsługuje tablice powiązania do obiektów przy użyciu tablicy indeksów w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-552">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="11701-553">Dowolnym formacie tablicy, który udostępnia liczbowych segment klucza (`:0:`, `:1:`, &hellip; `:{n}:`) jest w stanie tablicy powiązania z macierzą POCO klasy.</span><span class="sxs-lookup"><span data-stu-id="11701-553">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="11701-554">Powiązanie jest zapewniana przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="11701-554">Binding is provided by convention.</span></span> <span data-ttu-id="11701-555">Konfiguracja niestandardowa dostawców nie są wymagane do zaimplementowania tablicy powiązania.</span><span class="sxs-lookup"><span data-stu-id="11701-555">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="11701-556">**Przetwarzanie w pamięci tablica**</span><span class="sxs-lookup"><span data-stu-id="11701-556">**In-memory array processing**</span></span>

<span data-ttu-id="11701-557">Należy wziąć pod uwagę kluczy i wartości konfiguracji pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="11701-557">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="11701-558">Key</span><span class="sxs-lookup"><span data-stu-id="11701-558">Key</span></span>     | <span data-ttu-id="11701-559">Wartość</span><span class="sxs-lookup"><span data-stu-id="11701-559">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="11701-560">Macierz: 0</span><span class="sxs-lookup"><span data-stu-id="11701-560">array:0</span></span> | <span data-ttu-id="11701-561">value0</span><span class="sxs-lookup"><span data-stu-id="11701-561">value0</span></span> |
| <span data-ttu-id="11701-562">Macierz: 1</span><span class="sxs-lookup"><span data-stu-id="11701-562">array:1</span></span> | <span data-ttu-id="11701-563">Wartość1</span><span class="sxs-lookup"><span data-stu-id="11701-563">value1</span></span> |
| <span data-ttu-id="11701-564">Macierz: 2</span><span class="sxs-lookup"><span data-stu-id="11701-564">array:2</span></span> | <span data-ttu-id="11701-565">Wartość2</span><span class="sxs-lookup"><span data-stu-id="11701-565">value2</span></span> |
| <span data-ttu-id="11701-566">Macierz: 4</span><span class="sxs-lookup"><span data-stu-id="11701-566">array:4</span></span> | <span data-ttu-id="11701-567">Wartość4</span><span class="sxs-lookup"><span data-stu-id="11701-567">value4</span></span> |
| <span data-ttu-id="11701-568">Macierz: 5</span><span class="sxs-lookup"><span data-stu-id="11701-568">array:5</span></span> | <span data-ttu-id="11701-569">Wartość5</span><span class="sxs-lookup"><span data-stu-id="11701-569">value5</span></span> |

<span data-ttu-id="11701-570">Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:</span><span class="sxs-lookup"><span data-stu-id="11701-570">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="11701-571">Tablica pomija wartość indeksu &num;3.</span><span class="sxs-lookup"><span data-stu-id="11701-571">The array skips a value for index &num;3.</span></span> <span data-ttu-id="11701-572">Konfiguracja obiektu wiążącego nie jest zdolny do powiązania wartości null lub tworzenie wartości null wpisów w powiązanych obiektów, które stają się w chwili, gdy przedstawiono wynik powiązania tej tablicy do obiektu.</span><span class="sxs-lookup"><span data-stu-id="11701-572">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="11701-573">W przykładowej aplikacji do przechowywania danych konfiguracji powiązanej dostępnej klasy POCO:</span><span class="sxs-lookup"><span data-stu-id="11701-573">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="11701-574">Dane konfiguracji jest powiązana z obiektem:</span><span class="sxs-lookup"><span data-stu-id="11701-574">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="11701-575">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) składni można również użyć, która skutkuje bardziej kompaktowy kod:</span><span class="sxs-lookup"><span data-stu-id="11701-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="11701-576">Powiązany obiekt, wystąpienie `ArrayExample`, odbiera dane tablicy z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-576">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="11701-577">`ArrayExamples.Entries` Indeks</span><span class="sxs-lookup"><span data-stu-id="11701-577">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="11701-578">`ArrayExamples.Entries` Wartość</span><span class="sxs-lookup"><span data-stu-id="11701-578">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="11701-579">0</span><span class="sxs-lookup"><span data-stu-id="11701-579">0</span></span>                             | <span data-ttu-id="11701-580">value0</span><span class="sxs-lookup"><span data-stu-id="11701-580">value0</span></span>                        |
| <span data-ttu-id="11701-581">1</span><span class="sxs-lookup"><span data-stu-id="11701-581">1</span></span>                             | <span data-ttu-id="11701-582">Wartość1</span><span class="sxs-lookup"><span data-stu-id="11701-582">value1</span></span>                        |
| <span data-ttu-id="11701-583">2</span><span class="sxs-lookup"><span data-stu-id="11701-583">2</span></span>                             | <span data-ttu-id="11701-584">Wartość2</span><span class="sxs-lookup"><span data-stu-id="11701-584">value2</span></span>                        |
| <span data-ttu-id="11701-585">3</span><span class="sxs-lookup"><span data-stu-id="11701-585">3</span></span>                             | <span data-ttu-id="11701-586">Wartość4</span><span class="sxs-lookup"><span data-stu-id="11701-586">value4</span></span>                        |
| <span data-ttu-id="11701-587">4</span><span class="sxs-lookup"><span data-stu-id="11701-587">4</span></span>                             | <span data-ttu-id="11701-588">Wartość5</span><span class="sxs-lookup"><span data-stu-id="11701-588">value5</span></span>                        |

<span data-ttu-id="11701-589">Indeks &num;3 w powiązany obiekt przechowuje dane konfiguracyjne `array:4` klucz konfiguracji i jego wartość `value4`.</span><span class="sxs-lookup"><span data-stu-id="11701-589">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="11701-590">Gdy dane konfiguracji, które zawiera tablicę jest powiązana, indeksy tablicy w klucze konfiguracji jedynie służą do iteracji dane konfiguracji, podczas tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="11701-590">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="11701-591">Wartość null nie mogą być przechowywane w danych konfiguracji, a wpis o wartości null nie jest tworzona w powiązany obiekt, w przypadku tablicy w konfiguracji kluczy Pomiń jeden lub więcej indeksów.</span><span class="sxs-lookup"><span data-stu-id="11701-591">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="11701-592">Brak elementu konfiguracji dla indeksu &num;3 mogą być dostarczane przed powiązanie `ArrayExamples` wystąpienie przez dowolnego dostawcę konfiguracji, tworzącego prawidłowe pary klucz wartość w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-592">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="11701-593">Jeśli przykład uwzględniony dodatkowego dostawcę konfiguracji JSON z Brak pary klucz wartość `ArrayExamples.Entries` pasuje do tablicy kompletna konfiguracja:</span><span class="sxs-lookup"><span data-stu-id="11701-593">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="11701-594">*missing_value.JSON*:</span><span class="sxs-lookup"><span data-stu-id="11701-594">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11701-595">W `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="11701-595">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="11701-596">W `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="11701-596">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="11701-597">Pary klucz wartość, jak pokazano w tabeli są ładowane do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-597">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="11701-598">Key</span><span class="sxs-lookup"><span data-stu-id="11701-598">Key</span></span>             | <span data-ttu-id="11701-599">Wartość</span><span class="sxs-lookup"><span data-stu-id="11701-599">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="11701-600">Macierz: wpisów: 3</span><span class="sxs-lookup"><span data-stu-id="11701-600">array:entries:3</span></span> | <span data-ttu-id="11701-601">Wartość3</span><span class="sxs-lookup"><span data-stu-id="11701-601">value3</span></span> |

<span data-ttu-id="11701-602">Jeśli `ArrayExamples` wystąpienia klasy jest powiązana po dostawcę konfiguracji JSON zawiera wpis dla indeksu &num;3, `ArrayExamples.Entries` tablicy zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="11701-602">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="11701-603">`ArrayExamples.Entries` Indeks</span><span class="sxs-lookup"><span data-stu-id="11701-603">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="11701-604">`ArrayExamples.Entries` Wartość</span><span class="sxs-lookup"><span data-stu-id="11701-604">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="11701-605">0</span><span class="sxs-lookup"><span data-stu-id="11701-605">0</span></span>                             | <span data-ttu-id="11701-606">value0</span><span class="sxs-lookup"><span data-stu-id="11701-606">value0</span></span>                        |
| <span data-ttu-id="11701-607">1</span><span class="sxs-lookup"><span data-stu-id="11701-607">1</span></span>                             | <span data-ttu-id="11701-608">Wartość1</span><span class="sxs-lookup"><span data-stu-id="11701-608">value1</span></span>                        |
| <span data-ttu-id="11701-609">2</span><span class="sxs-lookup"><span data-stu-id="11701-609">2</span></span>                             | <span data-ttu-id="11701-610">Wartość2</span><span class="sxs-lookup"><span data-stu-id="11701-610">value2</span></span>                        |
| <span data-ttu-id="11701-611">3</span><span class="sxs-lookup"><span data-stu-id="11701-611">3</span></span>                             | <span data-ttu-id="11701-612">Wartość3</span><span class="sxs-lookup"><span data-stu-id="11701-612">value3</span></span>                        |
| <span data-ttu-id="11701-613">4</span><span class="sxs-lookup"><span data-stu-id="11701-613">4</span></span>                             | <span data-ttu-id="11701-614">Wartość4</span><span class="sxs-lookup"><span data-stu-id="11701-614">value4</span></span>                        |
| <span data-ttu-id="11701-615">5</span><span class="sxs-lookup"><span data-stu-id="11701-615">5</span></span>                             | <span data-ttu-id="11701-616">Wartość5</span><span class="sxs-lookup"><span data-stu-id="11701-616">value5</span></span>                        |

<span data-ttu-id="11701-617">**Przetwarzanie tablicy JSON**</span><span class="sxs-lookup"><span data-stu-id="11701-617">**JSON array processing**</span></span>

<span data-ttu-id="11701-618">Jeśli plik JSON zawiera tablicę, klucze konfiguracji zostaną utworzone dla elementów tablicy przy użyciu indeksu zaczynającego się od zera sekcji.</span><span class="sxs-lookup"><span data-stu-id="11701-618">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="11701-619">W następującym pliku konfiguracji `subsection` jest tablicą:</span><span class="sxs-lookup"><span data-stu-id="11701-619">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="11701-620">Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz wartość:</span><span class="sxs-lookup"><span data-stu-id="11701-620">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="11701-621">Key</span><span class="sxs-lookup"><span data-stu-id="11701-621">Key</span></span>                     | <span data-ttu-id="11701-622">Wartość</span><span class="sxs-lookup"><span data-stu-id="11701-622">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="11701-623">json_array:key</span><span class="sxs-lookup"><span data-stu-id="11701-623">json_array:key</span></span>          | <span data-ttu-id="11701-624">Wartośća</span><span class="sxs-lookup"><span data-stu-id="11701-624">valueA</span></span> |
| <span data-ttu-id="11701-625">json_array:Subsection:0</span><span class="sxs-lookup"><span data-stu-id="11701-625">json_array:subsection:0</span></span> | <span data-ttu-id="11701-626">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="11701-626">valueB</span></span> |
| <span data-ttu-id="11701-627">json_array:Subsection:1</span><span class="sxs-lookup"><span data-stu-id="11701-627">json_array:subsection:1</span></span> | <span data-ttu-id="11701-628">valueC</span><span class="sxs-lookup"><span data-stu-id="11701-628">valueC</span></span> |
| <span data-ttu-id="11701-629">json_array:Subsection:2</span><span class="sxs-lookup"><span data-stu-id="11701-629">json_array:subsection:2</span></span> | <span data-ttu-id="11701-630">zwracającej</span><span class="sxs-lookup"><span data-stu-id="11701-630">valueD</span></span> |

<span data-ttu-id="11701-631">W przykładowej aplikacji następującej klasy POCO jest dostępny dla powiązania pary klucz wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="11701-631">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="11701-632">Po powiązaniu `JsonArrayExample.Key` przechowuje wartość `valueA`.</span><span class="sxs-lookup"><span data-stu-id="11701-632">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="11701-633">Wartości podsekcji są przechowywane we właściwości tablicy POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="11701-633">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="11701-634">`JsonArrayExample.Subsection` Indeks</span><span class="sxs-lookup"><span data-stu-id="11701-634">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="11701-635">`JsonArrayExample.Subsection` Wartość</span><span class="sxs-lookup"><span data-stu-id="11701-635">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="11701-636">0</span><span class="sxs-lookup"><span data-stu-id="11701-636">0</span></span>                                   | <span data-ttu-id="11701-637">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="11701-637">valueB</span></span>                              |
| <span data-ttu-id="11701-638">1</span><span class="sxs-lookup"><span data-stu-id="11701-638">1</span></span>                                   | <span data-ttu-id="11701-639">valueC</span><span class="sxs-lookup"><span data-stu-id="11701-639">valueC</span></span>                              |
| <span data-ttu-id="11701-640">2</span><span class="sxs-lookup"><span data-stu-id="11701-640">2</span></span>                                   | <span data-ttu-id="11701-641">zwracającej</span><span class="sxs-lookup"><span data-stu-id="11701-641">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="11701-642">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="11701-642">Custom configuration provider</span></span>

<span data-ttu-id="11701-643">Przykładowa aplikacja pokazuje, jak utworzyć dostawcę konfiguracji podstawowej, który odczytuje pary klucz wartość konfiguracji z bazy danych za pomocą [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="11701-643">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="11701-644">Dostawcę ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="11701-644">The provider has the following characteristics:</span></span>

* <span data-ttu-id="11701-645">Bazy danych w pamięci programu EF jest używana do celów demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="11701-645">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="11701-646">Aby użyć bazy danych, która wymaga parametrów połączenia, należy zaimplementować pomocniczy `ConfigurationBuilder` podawać parametrów połączenia z innego dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="11701-646">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="11701-647">Dostawca odczytuje tabelę bazy danych do konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="11701-647">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="11701-648">Dostawca nie kwerendy bazy danych na podstawie-key.</span><span class="sxs-lookup"><span data-stu-id="11701-648">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="11701-649">Załaduj ponownie przy zmianie nie jest zaimplementowany, więc zaktualizowanie bazy danych, po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11701-649">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="11701-650">Zdefiniuj `EFConfigurationValue` jednostki do przechowywania wartości konfiguracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="11701-650">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="11701-651">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="11701-651">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="11701-652">Dodaj `EFConfigurationContext` do przechowywania i uzyskiwania dostępu skonfigurowane wartości.</span><span class="sxs-lookup"><span data-stu-id="11701-652">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="11701-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="11701-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="11701-654">Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="11701-654">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="11701-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="11701-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="11701-656">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="11701-656">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="11701-657">Dostawca konfiguracji inicjuje bazy danych, gdy jest on pusty.</span><span class="sxs-lookup"><span data-stu-id="11701-657">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="11701-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="11701-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="11701-659">`AddEFConfiguration` Metody rozszerzenia, który pozwala na dodawanie źródła konfiguracji do `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="11701-659">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="11701-660">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="11701-660">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="11701-661">Poniższy kod przedstawia sposób używania niestandardowego `EFConfigurationProvider` w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="11701-661">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="11701-662">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="11701-662">Access configuration during startup</span></span>

<span data-ttu-id="11701-663">Wstrzykiwanie `IConfiguration` do `Startup` konstruktora do dostępu do wartości konfiguracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="11701-663">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="11701-664">Uzyskać dostępu do konfiguracji w `Startup.Configure`, albo wstrzyknąć `IConfiguration` bezpośrednio do metody lub użyj wystąpienia z konstruktora:</span><span class="sxs-lookup"><span data-stu-id="11701-664">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="11701-665">Na przykład uzyskiwania dostępu do konfiguracji za pomocą metod jako udogodnienie uruchamiania zobacz [uruchamiania aplikacji: wygodne metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="11701-665">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="11701-666">Konfiguracja dostępu na stronie stron Razor lub w widoku MVC</span><span class="sxs-lookup"><span data-stu-id="11701-666">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="11701-667">Aby uzyskać dostęp do ustawień konfiguracji na stronie stron Razor lub widoku MVC, należy dodać [użycie dyrektywy](xref:mvc/views/razor#using) ([odwołanie w C#: using — dyrektywa](/dotnet/csharp/language-reference/keywords/using-directive)) dla [Microsoft.Extensions.Configuration przestrzeni nazw ](xref:Microsoft.Extensions.Configuration) i wstawiać <xref:Microsoft.Extensions.Configuration.IConfiguration> do strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="11701-667">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="11701-668">Na stronie stron Razor:</span><span class="sxs-lookup"><span data-stu-id="11701-668">In a Razor Pages page:</span></span>

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

<span data-ttu-id="11701-669">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="11701-669">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="11701-670">Dodaj konfigurację z zewnętrznego zestawu</span><span class="sxs-lookup"><span data-stu-id="11701-670">Add configuration from an external assembly</span></span>

<span data-ttu-id="11701-671"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Implementacji umożliwia dodawanie rozszerzeń do aplikacji przy uruchamianiu z zewnętrznego zestawu poza aplikacji `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="11701-671">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="11701-672">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="11701-672">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11701-673">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="11701-673">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="11701-674">Głębszej analizy konfiguracji firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="11701-674">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
