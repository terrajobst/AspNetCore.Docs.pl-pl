---
title: Konfiguracja w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core za pomocą interfejsu API konfiguracji.
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 6f0378ffc4f9a1efa95c8f70d70e7799abef130b
ms.sourcegitcommit: 1872d2e6f299093c78a6795a486929ffb0bbffff
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53216901"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="220d9-103">Konfiguracja w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="220d9-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="220d9-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="220d9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="220d9-105">Konfiguracja aplikacji w programie ASP.NET Core opiera się na parach klucz wartość ustanowione przez *dostawcy konfiguracji*.</span><span class="sxs-lookup"><span data-stu-id="220d9-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="220d9-106">Dostawcy konfiguracji odczytania danych konfiguracyjnych do pary klucz wartość z wielu źródeł w konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="220d9-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="220d9-107">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="220d9-107">Azure Key Vault</span></span>
* <span data-ttu-id="220d9-108">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-108">Command-line arguments</span></span>
* <span data-ttu-id="220d9-109">Niestandardowi dostawcy (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="220d9-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="220d9-110">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="220d9-110">Directory files</span></span>
* <span data-ttu-id="220d9-111">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="220d9-111">Environment variables</span></span>
* <span data-ttu-id="220d9-112">Obiekty platformy .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="220d9-112">In-memory .NET objects</span></span>
* <span data-ttu-id="220d9-113">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="220d9-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="220d9-114">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="220d9-114">Azure Key Vault</span></span>
* <span data-ttu-id="220d9-115">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-115">Command-line arguments</span></span>
* <span data-ttu-id="220d9-116">Niestandardowi dostawcy (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="220d9-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="220d9-117">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="220d9-117">Environment variables</span></span>
* <span data-ttu-id="220d9-118">Obiekty platformy .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="220d9-118">In-memory .NET objects</span></span>
* <span data-ttu-id="220d9-119">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="220d9-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="220d9-120">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-120">Command-line arguments</span></span>
* <span data-ttu-id="220d9-121">Niestandardowi dostawcy (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="220d9-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="220d9-122">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="220d9-122">Environment variables</span></span>
* <span data-ttu-id="220d9-123">Obiekty platformy .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="220d9-123">In-memory .NET objects</span></span>
* <span data-ttu-id="220d9-124">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="220d9-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="220d9-125">*Wzorzec opcje* jest rozszerzeniem pojęć związanych z konfiguracją opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="220d9-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="220d9-126">Opcje używa klas do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="220d9-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="220d9-127">Aby uzyskać więcej informacji na temat korzystania z wzorca opcji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="220d9-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="220d9-128">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="220d9-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="220d9-129">Przykłady podane w tym temacie opierają się na:</span><span class="sxs-lookup"><span data-stu-id="220d9-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="220d9-130">Ustawianie ścieżki podstawowej aplikacji za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="220d9-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="220d9-131">`SetBasePath` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="220d9-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="220d9-132">Rozpoznawanie sekcji plików konfiguracji przy użyciu <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="220d9-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="220d9-133">`GetSection` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="220d9-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="220d9-134">Konfiguracja powiązania do .NET klasy za pomocą <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> i [uzyskać&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="220d9-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="220d9-135">`Bind` i `Get<T>` są dostępne dla aplikacji, odwołując się do [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="220d9-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="220d9-136">`Get<T>` jest dostępna w programie ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="220d9-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="220d9-137">Te trzy pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="220d9-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="220d9-138">Te trzy pakiety są objęte [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="220d9-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="220d9-139">Hostowanie i konfiguracji aplikacji</span><span class="sxs-lookup"><span data-stu-id="220d9-139">Host vs. app configuration</span></span>

<span data-ttu-id="220d9-140">Zanim aplikacja jest skonfigurowana i uruchomiona, *hosta* skonfigurowany i uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="220d9-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="220d9-141">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="220d9-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="220d9-142">Zarówno aplikacja, jak i hosta są skonfigurowane przy użyciu dostawcy konfiguracji opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="220d9-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="220d9-143">Pary klucz wartość konfiguracji hosta stają się częścią globalnej konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="220d9-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="220d9-144">Aby uzyskać więcej informacji na temat sposobu konfiguracji dostawcy są używane, gdy host jest wbudowana i wpływ źródła konfiguracji hosta konfiguracji, zobacz <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="220d9-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="220d9-145">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="220d9-145">Security</span></span>

<span data-ttu-id="220d9-146">Przyjmuje następujące najlepsze rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="220d9-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="220d9-147">Nigdy nie przechowują hasła lub innych danych poufnych w kodzie dostawcy konfiguracji lub w plikach konfiguracyjnych w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="220d9-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="220d9-148">Nie używać kluczy tajnych w środowisku produkcyjnym w trakcie opracowywania lub środowisk testowych.</span><span class="sxs-lookup"><span data-stu-id="220d9-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="220d9-149">Określ klucze tajne poza projektem, nie może być przypadkowo wszelkich starań, by repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="220d9-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="220d9-150">Dowiedz się więcej o [jak używać wielu środowisk](xref:fundamentals/environments) i zarządzanie nimi [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie programowania przy użyciu klucza tajnego Menedżera](xref:security/app-secrets) (zawiera wskazówki dotyczące używania zmiennych środowiskowych do przechowywania dane poufne).</span><span class="sxs-lookup"><span data-stu-id="220d9-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="220d9-151">Menedżer klucz tajny używa dostawcy konfiguracji plików do przechowywania kluczy tajnych użytkownika w pliku JSON w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="220d9-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="220d9-152">Dostawca konfiguracji pliku jest opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="220d9-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="220d9-153">[Usługa Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stanowi jedną z opcji dla bezpieczne przechowywanie kluczy tajnych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="220d9-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="220d9-154">Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="220d9-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="220d9-155">Dane hierarchiczne konfiguracji</span><span class="sxs-lookup"><span data-stu-id="220d9-155">Hierarchical configuration data</span></span>

<span data-ttu-id="220d9-156">Interfejs API konfiguracji jest zdolny do utrzymywania dane hierarchiczne konfiguracji spłaszczając dane hierarchiczne przy użyciu ogranicznika w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="220d9-157">W następującym pliku JSON cztery klucze, istnieją w ze strukturą hierarchii dwie sekcje:</span><span class="sxs-lookup"><span data-stu-id="220d9-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="220d9-158">Gdy plik jest do odczytu do konfiguracji, unikatowe klucze są tworzone do obsługi struktury danych hierarchicznych oryginalnego źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="220d9-159">Sekcji i kluczy są spłaszczone przy użyciu dwukropek (`:`) do pierwotnej struktury obsługi:</span><span class="sxs-lookup"><span data-stu-id="220d9-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="220d9-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="220d9-160">section0:key0</span></span>
* <span data-ttu-id="220d9-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="220d9-161">section0:key1</span></span>
* <span data-ttu-id="220d9-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="220d9-162">section1:key0</span></span>
* <span data-ttu-id="220d9-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="220d9-163">section1:key1</span></span>

<span data-ttu-id="220d9-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> metody są dostępne do izolowania sekcje i elementy podrzędne do sekcji w danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="220d9-165">Te metody są opisane w dalszej części w [GetSection GetChildren i Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="220d9-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="220d9-166">Konwencje</span><span class="sxs-lookup"><span data-stu-id="220d9-166">Conventions</span></span>

<span data-ttu-id="220d9-167">Przy uruchamianiu aplikacji źródła konfiguracji są do odczytu w kolejności, że podano ich dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="220d9-168">Dostawcy konfiguracji pliku mają możliwość Załaduj ponownie konfigurację, gdy podstawowy plik ustawień zostanie zmieniona po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="220d9-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="220d9-169">Dostawca konfiguracji pliku jest opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="220d9-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="220d9-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> jest dostępna w aplikacji [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="220d9-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="220d9-171">Dostawcy konfiguracji nie może wykorzystywać DI, ponieważ nie jest dostępna podczas konfigurowania one przez hosta.</span><span class="sxs-lookup"><span data-stu-id="220d9-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="220d9-172">Klucze konfiguracji przyjęcie następujących konwencji:</span><span class="sxs-lookup"><span data-stu-id="220d9-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="220d9-173">Kluczy jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="220d9-173">Keys are case-insensitive.</span></span> <span data-ttu-id="220d9-174">Na przykład `ConnectionString` i `connectionstring` są traktowane jako równoważne kluczy.</span><span class="sxs-lookup"><span data-stu-id="220d9-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="220d9-175">Jeśli ten sam klucz jest ustawiany przez dostawców o tej samej lub innej konfiguracji, ostatnia wartość klucza jest wartość.</span><span class="sxs-lookup"><span data-stu-id="220d9-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="220d9-176">Klucze hierarchicznych</span><span class="sxs-lookup"><span data-stu-id="220d9-176">Hierarchical keys</span></span>
  * <span data-ttu-id="220d9-177">W ramach konfiguracji interfejsu API, separator dwukropka (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="220d9-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="220d9-178">W zmiennych środowiskowych separator dwukropek, może nie działać na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="220d9-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="220d9-179">Podwójnym podkreśleniem (`__`) jest obsługiwana przez wszystkie platformy i jest konwertowany na dwukropka.</span><span class="sxs-lookup"><span data-stu-id="220d9-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="220d9-180">W usłudze Azure Key Vault, klucze hierarchiczne, użyj `--` (dwa łączniki) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="220d9-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="220d9-181">Należy podać kod, aby zastąpić kresek dwukropka po wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="220d9-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="220d9-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Obsługuje tablice powiązania do obiektów przy użyciu tablicy indeksów w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="220d9-183">Powiązanie tablicy jest opisana w [tablicy należy powiązać klasę](#bind-an-array-to-a-class) sekcji.</span><span class="sxs-lookup"><span data-stu-id="220d9-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="220d9-184">Wartości konfiguracji przyjęcie następujących konwencji:</span><span class="sxs-lookup"><span data-stu-id="220d9-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="220d9-185">Wartości są ciągami.</span><span class="sxs-lookup"><span data-stu-id="220d9-185">Values are strings.</span></span>
* <span data-ttu-id="220d9-186">Wartości null nie przechowywanych w konfiguracji lub powiązane z obiektami.</span><span class="sxs-lookup"><span data-stu-id="220d9-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="220d9-187">dostawcy</span><span class="sxs-lookup"><span data-stu-id="220d9-187">Providers</span></span>

<span data-ttu-id="220d9-188">W poniższej tabeli przedstawiono dostępne dla aplikacji platformy ASP.NET Core dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="220d9-189">Dostawca</span><span class="sxs-lookup"><span data-stu-id="220d9-189">Provider</span></span> | <span data-ttu-id="220d9-190">Udostępnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="220d9-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="220d9-191">[Dostawca konfiguracji magazynu w usłudze Azure klucza](xref:security/key-vault-configuration) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="220d9-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="220d9-192">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="220d9-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="220d9-193">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="220d9-194">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-194">Command-line parameters</span></span> |
| [<span data-ttu-id="220d9-195">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="220d9-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="220d9-196">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="220d9-196">Custom source</span></span> |
| [<span data-ttu-id="220d9-197">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="220d9-197">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="220d9-198">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="220d9-198">Environment variables</span></span> |
| [<span data-ttu-id="220d9-199">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="220d9-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="220d9-200">Pliki INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="220d9-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="220d9-201">Dostawca klucza każdego pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="220d9-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="220d9-202">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="220d9-202">Directory files</span></span> |
| [<span data-ttu-id="220d9-203">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="220d9-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="220d9-204">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="220d9-204">In-memory collections</span></span> |
| <span data-ttu-id="220d9-205">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="220d9-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="220d9-206">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="220d9-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="220d9-207">Dostawca</span><span class="sxs-lookup"><span data-stu-id="220d9-207">Provider</span></span> | <span data-ttu-id="220d9-208">Udostępnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="220d9-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="220d9-209">[Dostawca konfiguracji magazynu w usłudze Azure klucza](xref:security/key-vault-configuration) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="220d9-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="220d9-210">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="220d9-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="220d9-211">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="220d9-212">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-212">Command-line parameters</span></span> |
| [<span data-ttu-id="220d9-213">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="220d9-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="220d9-214">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="220d9-214">Custom source</span></span> |
| [<span data-ttu-id="220d9-215">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="220d9-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="220d9-216">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="220d9-216">Environment variables</span></span> |
| [<span data-ttu-id="220d9-217">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="220d9-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="220d9-218">Pliki INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="220d9-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="220d9-219">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="220d9-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="220d9-220">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="220d9-220">In-memory collections</span></span> |
| <span data-ttu-id="220d9-221">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="220d9-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="220d9-222">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="220d9-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="220d9-223">Dostawca</span><span class="sxs-lookup"><span data-stu-id="220d9-223">Provider</span></span> | <span data-ttu-id="220d9-224">Udostępnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="220d9-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="220d9-225">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="220d9-226">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-226">Command-line parameters</span></span> |
| [<span data-ttu-id="220d9-227">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="220d9-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="220d9-228">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="220d9-228">Custom source</span></span> |
| [<span data-ttu-id="220d9-229">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="220d9-229">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="220d9-230">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="220d9-230">Environment variables</span></span> |
| [<span data-ttu-id="220d9-231">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="220d9-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="220d9-232">Pliki INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="220d9-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="220d9-233">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="220d9-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="220d9-234">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="220d9-234">In-memory collections</span></span> |
| <span data-ttu-id="220d9-235">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="220d9-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="220d9-236">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="220d9-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="220d9-237">Źródła konfiguracji są do odczytu w kolejności określono ich dostawców konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="220d9-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="220d9-238">Dostawcy konfiguracji opisanych w tym temacie są opisane w kolejności alfabetycznej, a nie w kolejności, Twój kod może zorganizować ich.</span><span class="sxs-lookup"><span data-stu-id="220d9-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="220d9-239">Kolejność dostawców konfiguracji w kodzie do własnych priorytetów do bazowego źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="220d9-240">Jest zwykle kolejność dostawców konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="220d9-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="220d9-241">Pliki (*appsettings.json*, *appsettings. { Środowisko} .json*, gdzie `{Environment}` jest bieżące środowisko hostingu aplikacji)</span><span class="sxs-lookup"><span data-stu-id="220d9-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="220d9-242">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="220d9-242">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="220d9-243">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym tylko)</span><span class="sxs-lookup"><span data-stu-id="220d9-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="220d9-244">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="220d9-244">Environment variables</span></span>
1. <span data-ttu-id="220d9-245">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-245">Command-line arguments</span></span>

<span data-ttu-id="220d9-246">Jest to powszechną praktyką do ostatniej pozycji dostawcę konfiguracji wiersza polecenia w serii dostawcy, aby zezwolić na argumenty wiersza polecenia zastąpić konfiguracji ustawione przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="220d9-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="220d9-247">Ta sekwencja dostawcy są umieszczane w miejscu, podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="220d9-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="220d9-248">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="220d9-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-249">Ta sekwencja dostawcy mogą być tworzone dla aplikacji (nie hosta) za pomocą <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> i wywołanie w celu jego <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in Class metoda `Startup`:</span><span class="sxs-lookup"><span data-stu-id="220d9-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="220d9-250">W powyższym przykładzie nazwa środowiska (`env.EnvironmentName`) i nazwę zestawu aplikacji (`env.ApplicationName`) są dostarczane przez <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="220d9-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="220d9-251">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="220d9-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="220d9-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="220d9-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="220d9-253">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta sieci Web, aby określić aplikacji dostawcy konfiguracji oprócz tych dodawane automatycznie przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="220d9-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="220d9-254">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="220d9-255"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Ładuje konfiguracji z pary klucz wartość argumentu wiersza polecenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="220d9-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="220d9-256">Aby aktywować wiersza polecenia konfiguracji, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metoda rozszerzająca zostanie wywołana w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="220d9-257">`AddCommandLine` jest wywoływana automatycznie podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="220d9-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="220d9-258">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="220d9-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="220d9-259">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="220d9-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="220d9-260">Konfiguracja opcjonalna z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="220d9-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="220d9-261">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="220d9-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="220d9-262">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="220d9-262">Environment variables.</span></span>

<span data-ttu-id="220d9-263">`CreateDefaultBuilder` dodaje ostatniego wiersza polecenia dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="220d9-264">Argumenty wiersza polecenia przekazywane w czasie wykonywania zastąpić konfiguracji ustawione przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="220d9-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="220d9-265">`CreateDefaultBuilder` działa, gdy host jest konstruowany.</span><span class="sxs-lookup"><span data-stu-id="220d9-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="220d9-266">W związku z tym, wiersza polecenia konfiguracji aktywowany przez `CreateDefaultBuilder` mogą mieć wpływ na sposób host jest skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="220d9-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="220d9-267">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="220d9-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="220d9-268">`AddCommandLine` zostały już wywołane `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="220d9-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="220d9-269">Jeśli zachodzi potrzeba zapewniają konfigurację aplikacji i nadal będzie można przesłonić tę konfigurację za pomocą argumentów wiersza polecenia, należy wywołać dodatkowych dostawców aplikacji <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> i wywołać `AddCommandLine` ostatni.</span><span class="sxs-lookup"><span data-stu-id="220d9-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="220d9-270">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="220d9-271">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody.</span><span class="sxs-lookup"><span data-stu-id="220d9-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="220d9-272">`AddCommandLine` zostały już wywołane `CreateDefaultBuilder` podczas `UseConfiguration` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="220d9-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="220d9-273">Jeśli zachodzi potrzeba zapewniają konfigurację aplikacji i nadal będzie można przesłonić tę konfigurację za pomocą argumentów wiersza polecenia, należy wywołać dodatkowych dostawców aplikacji w `ConfigurationBuilder` i wywołać `AddCommandLine` ostatni.</span><span class="sxs-lookup"><span data-stu-id="220d9-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="220d9-274">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-275">Aby uaktywnić konfigurację wiersza polecenia, należy wywołać <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="220d9-276">Wywołanie dostawcy ostatnio, aby umożliwić argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="220d9-277">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="220d9-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="220d9-278">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="220d9-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="220d9-279">2.x Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta, który zawiera wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="220d9-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-280">Wywołania aplikacji przykładowej 1.x <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> na <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="220d9-281">Otwórz wiersz polecenia w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="220d9-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="220d9-282">Argument wiersza polecenia, aby podać `dotnet run` polecenia `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="220d9-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="220d9-283">Po uruchomieniu aplikacji otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="220d9-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="220d9-284">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla argumentu wiersza polecenia konfiguracji udostępnionego `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="220d9-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="220d9-285">Argumenty</span><span class="sxs-lookup"><span data-stu-id="220d9-285">Arguments</span></span>

<span data-ttu-id="220d9-286">Wartość musi stosować się znak równości (`=`), lub klucz musi mieć prefiks (`--` lub `/`) Jeśli wartość jest zgodna spację.</span><span class="sxs-lookup"><span data-stu-id="220d9-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="220d9-287">Wartość może być null, jeśli jest używany znak równości (na przykład `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="220d9-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="220d9-288">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="220d9-288">Key prefix</span></span>               | <span data-ttu-id="220d9-289">Przykład</span><span class="sxs-lookup"><span data-stu-id="220d9-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="220d9-290">Nie prefiksu</span><span class="sxs-lookup"><span data-stu-id="220d9-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="220d9-291">Dwa łączniki (`--`)</span><span class="sxs-lookup"><span data-stu-id="220d9-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="220d9-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="220d9-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="220d9-293">Kreski ułamkowej (`/`)</span><span class="sxs-lookup"><span data-stu-id="220d9-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="220d9-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="220d9-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="220d9-295">W tym samym poleceniu nie Mieszaj argument wiersza polecenia pary klucz wartość, które znakiem równości za pomocą pary klucz wartość, korzystających z przestrzeni.</span><span class="sxs-lookup"><span data-stu-id="220d9-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="220d9-296">Przykładowe polecenia:</span><span class="sxs-lookup"><span data-stu-id="220d9-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="220d9-297">Przełącz mapowania</span><span class="sxs-lookup"><span data-stu-id="220d9-297">Switch mappings</span></span>

<span data-ttu-id="220d9-298">Przełącznik jest transformowanie na nazwę klucza wymiany logiki.</span><span class="sxs-lookup"><span data-stu-id="220d9-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="220d9-299">Podczas ręcznego tworzenia konfiguracji za pomocą <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, może zapewnić słownika zastępstw przełącznika <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metody.</span><span class="sxs-lookup"><span data-stu-id="220d9-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="220d9-300">W przypadku przełącznika słownik mapowań słownika jest sprawdzane dla klucza, który pasuje do klucza, dostarczone przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="220d9-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="220d9-301">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) jest przekazywana do zestawu pary klucz wartość do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="220d9-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="220d9-302">Mapowanie przełącznika jest wymagane dla dowolnego klucz wiersza polecenia poprzedzone kreską pojedynczego (`-`).</span><span class="sxs-lookup"><span data-stu-id="220d9-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="220d9-303">Przełącz mapowania słownika kluczowych reguł:</span><span class="sxs-lookup"><span data-stu-id="220d9-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="220d9-304">Przełączniki musi rozpoczynać się kreską (`-`) lub podwójne kreska (`--`).</span><span class="sxs-lookup"><span data-stu-id="220d9-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="220d9-305">Słownik mapowań przełącznik nie może zawierać zduplikowane klucze.</span><span class="sxs-lookup"><span data-stu-id="220d9-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="220d9-306">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="220d9-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="220d9-307">Jak pokazano w powyższym przykładzie wywołanie `CreateDefaultBuilder` nie należy przekazywać argumenty, gdy przełącznik mapowania są używane.</span><span class="sxs-lookup"><span data-stu-id="220d9-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="220d9-308">`CreateDefaultBuilder` metody `AddCommandLine` wywołania nie zawiera zamapowanego przełączników, a nie istnieje sposób do przekazania słownik mapowania przełącznika `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="220d9-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="220d9-309">Jeśli argumenty zamapowanego przełącznik dołączania i są przekazywane do `CreateDefaultBuilder`, jego `AddCommandLine` dostawcy nie uda się zainicjowanie z <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="220d9-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="220d9-310">Rozwiązanie nie jest do przekazywania argumentów do `CreateDefaultBuilder` , ale zamiast tego umożliwiające `ConfigurationBuilder` metody `AddCommandLine` metody do przetwarzania argumentów i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="220d9-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

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

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="220d9-311">Jak pokazano w powyższym przykładzie wywołanie `CreateDefaultBuilder` nie należy przekazywać argumenty, gdy przełącznik mapowania są używane.</span><span class="sxs-lookup"><span data-stu-id="220d9-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="220d9-312">`CreateDefaultBuilder` metody `AddCommandLine` wywołania nie zawiera zamapowanego przełączników, a nie istnieje sposób do przekazania słownik mapowania przełącznika `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="220d9-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="220d9-313">Jeśli argumenty zamapowanego przełącznik dołączania i są przekazywane do `CreateDefaultBuilder`, jego `AddCommandLine` dostawcy nie uda się zainicjowanie z <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="220d9-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="220d9-314">Rozwiązanie nie jest do przekazywania argumentów do `CreateDefaultBuilder` , ale zamiast tego umożliwiające `ConfigurationBuilder` metody `AddCommandLine` metody do przetwarzania argumentów i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="220d9-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="220d9-315">Po utworzeniu przełącznika słownik mapowań zawiera dane wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="220d9-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="220d9-316">Key</span><span class="sxs-lookup"><span data-stu-id="220d9-316">Key</span></span>       | <span data-ttu-id="220d9-317">Wartość</span><span class="sxs-lookup"><span data-stu-id="220d9-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="220d9-318">Jeśli mapowane przełącznika klucze są używane podczas uruchamiania aplikacji, konfiguracji odbiera wartości konfiguracji na kluczu pochodzącego ze słownika:</span><span class="sxs-lookup"><span data-stu-id="220d9-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="220d9-319">Po uruchomieniu polecenia poprzedniej konfiguracji zawiera wartości podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="220d9-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="220d9-320">Key</span><span class="sxs-lookup"><span data-stu-id="220d9-320">Key</span></span>               | <span data-ttu-id="220d9-321">Wartość</span><span class="sxs-lookup"><span data-stu-id="220d9-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="220d9-322">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="220d9-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="220d9-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Ładuje konfiguracji ze środowiska zmiennej pary klucz wartość w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="220d9-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="220d9-324">Aby aktywować środowisko zmiennych konfiguracji, należy wywołać <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="220d9-325">Podczas pracy z kluczami hierarchiczne w zmiennych środowiskowych, separator dwukropek (`:`) może nie działać na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="220d9-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="220d9-326">Podwójnym podkreśleniem (`__`) jest obsługiwana przez wszystkie platformy i zastępuje dwukropka.</span><span class="sxs-lookup"><span data-stu-id="220d9-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="220d9-327">[Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) pozwala na Ustawianie zmiennych środowiskowych w portalu Azure, które mogą zastąpić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="220d9-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="220d9-328">Aby uzyskać więcej informacji, zobacz [aplikacje platformy Azure: Zastąpienie konfiguracji aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="220d9-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="220d9-329">`AddEnvironmentVariables` jest wywoływana automatycznie dla zmiennych środowiskowych prefiksem `ASPNETCORE_` podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-329">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="220d9-330">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="220d9-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="220d9-331">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="220d9-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="220d9-332">Konfiguracja aplikacji ze zmiennych środowiskowych unprefixed przez wywołanie metody `AddEnvironmentVariables` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="220d9-332">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="220d9-333">Konfiguracja opcjonalna z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="220d9-333">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="220d9-334">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="220d9-334">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="220d9-335">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="220d9-335">Command-line arguments.</span></span>

<span data-ttu-id="220d9-336">Dostawca konfiguracji zmiennych środowiskowych jest wywoływana po konfiguracji zostanie nawiązane z wpisami tajnymi użytkowników i *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="220d9-336">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="220d9-337">W tym miejscu podczas wywoływania dostawcy umożliwia zmienne środowiskowe są odczytywane w czasie wykonywania do zastępowania konfiguracji ustawione przez użytkownika hasła i *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="220d9-337">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="220d9-338">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="220d9-338">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="220d9-339">Jeśli konieczne jest zapewnienie konfiguracji aplikacji z dodatkowe zmienne środowiskowe, wywołaj dodatkowych dostawców aplikacji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> i wywołać `AddEnvironmentVariables` z prefiksem.</span><span class="sxs-lookup"><span data-stu-id="220d9-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="220d9-340">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="220d9-341">Wywołaj `AddEnvironmentVariables` metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="220d9-342">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody.</span><span class="sxs-lookup"><span data-stu-id="220d9-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="220d9-343">Jeśli konieczne jest zapewnienie konfiguracji aplikacji z dodatkowe zmienne środowiskowe, wywołaj dodatkowych dostawców aplikacji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> i wywołać `AddEnvironmentVariables` z prefiksem.</span><span class="sxs-lookup"><span data-stu-id="220d9-343">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="220d9-344">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-344">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-345">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="220d9-345">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="220d9-346">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="220d9-346">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="220d9-347">2.x Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta, który zawiera wywołanie `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="220d9-347">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-348">Wywołania aplikacji przykładowej 1.x `AddEnvironmentVariables` na `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="220d9-348">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="220d9-349">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="220d9-349">Run the sample app.</span></span> <span data-ttu-id="220d9-350">Otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="220d9-350">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="220d9-351">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla zmiennej środowiskowej `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="220d9-351">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="220d9-352">Wartość odzwierciedla środowisko, w którym aplikacja jest uruchomiona, zwykle `Development` podczas uruchamiania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="220d9-352">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="220d9-353">Przechowywać listę zmiennych środowiskowych renderowany przez aplikację krótki, aplikacji filtry zmienne środowiskowe do tych rozpoczynających się od następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="220d9-353">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="220d9-354">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="220d9-354">ASPNETCORE_</span></span>
* <span data-ttu-id="220d9-355">adresy URL</span><span class="sxs-lookup"><span data-stu-id="220d9-355">urls</span></span>
* <span data-ttu-id="220d9-356">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="220d9-356">Logging</span></span>
* <span data-ttu-id="220d9-357">ŚRODOWISKO</span><span class="sxs-lookup"><span data-stu-id="220d9-357">ENVIRONMENT</span></span>
* <span data-ttu-id="220d9-358">contentRoot</span><span class="sxs-lookup"><span data-stu-id="220d9-358">contentRoot</span></span>
* <span data-ttu-id="220d9-359">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="220d9-359">AllowedHosts</span></span>
* <span data-ttu-id="220d9-360">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="220d9-360">applicationName</span></span>
* <span data-ttu-id="220d9-361">Wiersz polecenia</span><span class="sxs-lookup"><span data-stu-id="220d9-361">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="220d9-362">Jeśli chcesz udostępnić wszystkie zmienne środowiskowe, dostępne dla aplikacji, zmień `FilteredConfiguration` w *Pages/Index.cshtml.cs* do następującego:</span><span class="sxs-lookup"><span data-stu-id="220d9-362">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-363">Jeśli chcesz udostępnić wszystkie zmienne środowiskowe, dostępne dla aplikacji, zmień `FilteredConfiguration` w *Controllers/HomeController.cs* do następującego:</span><span class="sxs-lookup"><span data-stu-id="220d9-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="220d9-364">Prefiksy</span><span class="sxs-lookup"><span data-stu-id="220d9-364">Prefixes</span></span>

<span data-ttu-id="220d9-365">Zmienne środowiskowe ładowane z konfiguracji aplikacji są filtrowane podczas podawania prefiks `AddEnvironmentVariables` metody.</span><span class="sxs-lookup"><span data-stu-id="220d9-365">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="220d9-366">Na przykład, aby filtr zmiennych środowiskowych na prefiksie `CUSTOM_`, podaj prefiks, który dostawca konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="220d9-366">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="220d9-367">Prefiks jest odłączany podczas tworzenia pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-367">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="220d9-368">Statyczne wygodna metoda `CreateDefaultBuilder` tworzy <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> Aby ustanowić hosta aplikacji.</span><span class="sxs-lookup"><span data-stu-id="220d9-368">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="220d9-369">Gdy `WebHostBuilder` jest tworzony, znajdzie jego konfigurację hosta w zmiennych środowiskowych z prefiksem `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="220d9-369">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="220d9-370">**Prefiksy ciąg połączenia**</span><span class="sxs-lookup"><span data-stu-id="220d9-370">**Connection string prefixes**</span></span>

<span data-ttu-id="220d9-371">Interfejs API konfiguracji zawiera reguły jest przetwarzana w specjalny cztery połączenia ciągu zmiennych środowiskowych zajmujących się Konfigurowanie parametrów połączenia platformy Azure dla środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="220d9-371">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="220d9-372">Zmienne środowiskowe z prefiksami pokazano w tabeli są ładowane do aplikacji, jeśli nie dostarczono żadnego prefiksu do `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="220d9-372">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="220d9-373">Prefiks ciągu połączenia</span><span class="sxs-lookup"><span data-stu-id="220d9-373">Connection string prefix</span></span> | <span data-ttu-id="220d9-374">Dostawca</span><span class="sxs-lookup"><span data-stu-id="220d9-374">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="220d9-375">Dostawcy niestandardowego</span><span class="sxs-lookup"><span data-stu-id="220d9-375">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="220d9-376">MySQL</span><span class="sxs-lookup"><span data-stu-id="220d9-376">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="220d9-377">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="220d9-377">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="220d9-378">SQL Server</span><span class="sxs-lookup"><span data-stu-id="220d9-378">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="220d9-379">Gdy zmienna środowiskowa są odnajdywane i ładowane do konfiguracji za pomocą dowolnego z czterech prefiksy z tabelą:</span><span class="sxs-lookup"><span data-stu-id="220d9-379">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="220d9-380">Klucz konfiguracji jest tworzony, usuwając prefiks zmiennej środowiska i dodając kluczowych sekcję konfiguracji (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="220d9-380">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="220d9-381">Tworzony jest nową parę klucz wartość konfiguracji, który reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, który nie ma określonego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="220d9-381">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="220d9-382">Klucz zmiennych środowiska</span><span class="sxs-lookup"><span data-stu-id="220d9-382">Environment variable key</span></span> | <span data-ttu-id="220d9-383">Klucz konfiguracji przekonwertowany</span><span class="sxs-lookup"><span data-stu-id="220d9-383">Converted configuration key</span></span> | <span data-ttu-id="220d9-384">Pozycja konfiguracji dostawcy</span><span class="sxs-lookup"><span data-stu-id="220d9-384">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="220d9-385">Wpis konfiguracyjny nie utworzony.</span><span class="sxs-lookup"><span data-stu-id="220d9-385">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="220d9-386">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="220d9-386">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="220d9-387">Wartość:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="220d9-387">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="220d9-388">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="220d9-388">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="220d9-389">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="220d9-389">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="220d9-390">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="220d9-390">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="220d9-391">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="220d9-391">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="220d9-392">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="220d9-392">File Configuration Provider</span></span>

<span data-ttu-id="220d9-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową dla ładowania konfiguracji z systemu plików.</span><span class="sxs-lookup"><span data-stu-id="220d9-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="220d9-394">Następujących dostawców konfiguracji są przeznaczone dla określonych typów plików:</span><span class="sxs-lookup"><span data-stu-id="220d9-394">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="220d9-395">Dostawca konfiguracji INI</span><span class="sxs-lookup"><span data-stu-id="220d9-395">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="220d9-396">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="220d9-396">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="220d9-397">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="220d9-397">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="220d9-398">Dostawca konfiguracji INI</span><span class="sxs-lookup"><span data-stu-id="220d9-398">INI Configuration Provider</span></span>

<span data-ttu-id="220d9-399"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Ładuje konfiguracji z pary klucz wartość pliku INI w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="220d9-399">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="220d9-400">Aby aktywować konfiguracji pliku INI, należy wywołać <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-400">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="220d9-401">Dwukropek może służyć do jako ogranicznik sekcji w konfiguracji pliku INI.</span><span class="sxs-lookup"><span data-stu-id="220d9-401">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="220d9-402">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="220d9-402">Overloads permit specifying:</span></span>

* <span data-ttu-id="220d9-403">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="220d9-403">Whether the file is optional.</span></span>
* <span data-ttu-id="220d9-404">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="220d9-404">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="220d9-405"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="220d9-405">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="220d9-406">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="220d9-406">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="220d9-407">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-407">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="220d9-408">Podczas wywoływania `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-408">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="220d9-409">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-409">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-410">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="220d9-410">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="220d9-411">Przykładowy plik konfiguracji INI:</span><span class="sxs-lookup"><span data-stu-id="220d9-411">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="220d9-412">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="220d9-412">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="220d9-413">section0:key0</span><span class="sxs-lookup"><span data-stu-id="220d9-413">section0:key0</span></span>
* <span data-ttu-id="220d9-414">section0:key1</span><span class="sxs-lookup"><span data-stu-id="220d9-414">section0:key1</span></span>
* <span data-ttu-id="220d9-415">section1:Subsection:key</span><span class="sxs-lookup"><span data-stu-id="220d9-415">section1:subsection:key</span></span>
* <span data-ttu-id="220d9-416">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="220d9-416">section2:subsection0:key</span></span>
* <span data-ttu-id="220d9-417">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="220d9-417">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="220d9-418">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="220d9-418">JSON Configuration Provider</span></span>

<span data-ttu-id="220d9-419"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Ładuje konfiguracji z pary klucz wartość plik JSON podczas wykonywania.</span><span class="sxs-lookup"><span data-stu-id="220d9-419">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="220d9-420">Aby aktywować konfiguracji pliku JSON, należy wywołać <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-420">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="220d9-421">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="220d9-421">Overloads permit specifying:</span></span>

* <span data-ttu-id="220d9-422">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="220d9-422">Whether the file is optional.</span></span>
* <span data-ttu-id="220d9-423">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="220d9-423">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="220d9-424"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="220d9-424">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="220d9-425">`AddJsonFile` jest wywoływana automatycznie, dwa razy, podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="220d9-425">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="220d9-426">Metoda jest wywoływana, aby załadować konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="220d9-426">The method is called to load configuration from:</span></span>

* <span data-ttu-id="220d9-427">*appSettings.JSON* &ndash; ten plik jest najpierw do odczytu.</span><span class="sxs-lookup"><span data-stu-id="220d9-427">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="220d9-428">Wersja środowiska pliku można zastąpić wartości dostarczone przez *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="220d9-428">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="220d9-429">*appSettings. {Środowiska} .json* &ndash; środowiska wersję pliku zostanie załadowany na podstawie [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="220d9-429">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="220d9-430">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="220d9-430">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="220d9-431">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="220d9-431">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="220d9-432">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="220d9-432">Environment variables.</span></span>
* <span data-ttu-id="220d9-433">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="220d9-433">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="220d9-434">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="220d9-434">Command-line arguments.</span></span>

<span data-ttu-id="220d9-435">Dostawca konfiguracji JSON jest ustanawiane, najpierw.</span><span class="sxs-lookup"><span data-stu-id="220d9-435">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="220d9-436">W związku z tym, wpisami tajnymi użytkowników, zmienne środowiskowe i argumenty wiersza polecenia zastępuje konfiguracji ustawione przez *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="220d9-436">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="220d9-437">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji dla plików innych niż *appsettings.json* i *appsettings. { Środowisko} .json*:</span><span class="sxs-lookup"><span data-stu-id="220d9-437">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="220d9-438">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-438">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="220d9-439">Możesz również bezpośrednio wywołać `AddJsonFile` metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-439">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="220d9-440">Podczas wywoływania `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-440">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="220d9-441">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-441">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-442">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="220d9-442">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="220d9-443">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="220d9-443">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="220d9-444">2.x Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do hosta, który obejmuje dwa wywołania do tworzenia `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="220d9-444">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="220d9-445">Konfiguracja została załadowana z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="220d9-445">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-446">Wywołania aplikacji przykładowej 1.x `AddJsonFile` dwa razy na `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="220d9-446">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="220d9-447">Konfiguracja została załadowana z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="220d9-447">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="220d9-448">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="220d9-448">Run the sample app.</span></span> <span data-ttu-id="220d9-449">Otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="220d9-449">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="220d9-450">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla konfiguracji, jak pokazano w tabeli, w zależności od środowiska.</span><span class="sxs-lookup"><span data-stu-id="220d9-450">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="220d9-451">Klucze konfiguracji rejestrowania, użyj dwukropka (`:`) jako separator hierarchicznej.</span><span class="sxs-lookup"><span data-stu-id="220d9-451">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="220d9-452">Key</span><span class="sxs-lookup"><span data-stu-id="220d9-452">Key</span></span>                        | <span data-ttu-id="220d9-453">Wartość rozwoju</span><span class="sxs-lookup"><span data-stu-id="220d9-453">Development Value</span></span> | <span data-ttu-id="220d9-454">Wartość produkcji</span><span class="sxs-lookup"><span data-stu-id="220d9-454">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="220d9-455">Rejestrowanie: LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="220d9-455">Logging:LogLevel:System</span></span>    | <span data-ttu-id="220d9-456">Informacje</span><span class="sxs-lookup"><span data-stu-id="220d9-456">Information</span></span>       | <span data-ttu-id="220d9-457">Informacje</span><span class="sxs-lookup"><span data-stu-id="220d9-457">Information</span></span>      |
| <span data-ttu-id="220d9-458">Rejestrowanie: LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="220d9-458">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="220d9-459">Informacje</span><span class="sxs-lookup"><span data-stu-id="220d9-459">Information</span></span>       | <span data-ttu-id="220d9-460">Informacje</span><span class="sxs-lookup"><span data-stu-id="220d9-460">Information</span></span>      |
| <span data-ttu-id="220d9-461">Rejestrowanie: LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="220d9-461">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="220d9-462">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="220d9-462">Debug</span></span>             | <span data-ttu-id="220d9-463">Błąd</span><span class="sxs-lookup"><span data-stu-id="220d9-463">Error</span></span>            |
| <span data-ttu-id="220d9-464">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="220d9-464">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="220d9-465">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="220d9-465">XML Configuration Provider</span></span>

<span data-ttu-id="220d9-466"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Ładuje konfiguracji z pary klucz wartość plik XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="220d9-466">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="220d9-467">Aby aktywować Konfiguracja pliku XML, należy wywołać <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-467">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="220d9-468">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="220d9-468">Overloads permit specifying:</span></span>

* <span data-ttu-id="220d9-469">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="220d9-469">Whether the file is optional.</span></span>
* <span data-ttu-id="220d9-470">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="220d9-470">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="220d9-471"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="220d9-471">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="220d9-472">Węzeł główny plik konfiguracji jest ignorowany podczas tworzenia pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-472">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="220d9-473">Określać definicji typu dokumentu (DTD) lub przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="220d9-473">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="220d9-474">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="220d9-474">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="220d9-475">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-475">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="220d9-476">Podczas wywoływania `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-476">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="220d9-477">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-477">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-478">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="220d9-478">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="220d9-479">Pliki konfiguracji XML można użyć nazw unikatowych elementów dla powtarzających się sekcje:</span><span class="sxs-lookup"><span data-stu-id="220d9-479">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="220d9-480">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="220d9-480">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="220d9-481">section0:key0</span><span class="sxs-lookup"><span data-stu-id="220d9-481">section0:key0</span></span>
* <span data-ttu-id="220d9-482">section0:key1</span><span class="sxs-lookup"><span data-stu-id="220d9-482">section0:key1</span></span>
* <span data-ttu-id="220d9-483">section1:key0</span><span class="sxs-lookup"><span data-stu-id="220d9-483">section1:key0</span></span>
* <span data-ttu-id="220d9-484">section1:key1</span><span class="sxs-lookup"><span data-stu-id="220d9-484">section1:key1</span></span>

<span data-ttu-id="220d9-485">Powtarzające się elementy, które używają takiej samej nazwie elementu pracy, jeśli `name` atrybut jest używany do odróżniania elementów:</span><span class="sxs-lookup"><span data-stu-id="220d9-485">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="220d9-486">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="220d9-486">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="220d9-487">sekcja: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="220d9-487">section:section0:key:key0</span></span>
* <span data-ttu-id="220d9-488">sekcja: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="220d9-488">section:section0:key:key1</span></span>
* <span data-ttu-id="220d9-489">sekcja: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="220d9-489">section:section1:key:key0</span></span>
* <span data-ttu-id="220d9-490">sekcja: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="220d9-490">section:section1:key:key1</span></span>

<span data-ttu-id="220d9-491">Atrybuty można podać wartości:</span><span class="sxs-lookup"><span data-stu-id="220d9-491">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="220d9-492">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="220d9-492">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="220d9-493">klucz: atrybut</span><span class="sxs-lookup"><span data-stu-id="220d9-493">key:attribute</span></span>
* <span data-ttu-id="220d9-494">sekcja: klucz: atrybut</span><span class="sxs-lookup"><span data-stu-id="220d9-494">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="220d9-495">Dostawca klucza każdego pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="220d9-495">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="220d9-496"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Korzysta z plików w katalogu jako pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-496">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="220d9-497">Klucz jest nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="220d9-497">The key is the file name.</span></span> <span data-ttu-id="220d9-498">Wartość zawiera zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="220d9-498">The value contains the file's contents.</span></span> <span data-ttu-id="220d9-499">Dostawca konfiguracji klucza każdego pliku jest używany w scenariuszach hostingu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="220d9-499">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="220d9-500">Aby aktywować klucza dla pliku konfiguracji, należy wywołać <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-500">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="220d9-501">`directoryPath` Do plików musi być ścieżką bezwzględną.</span><span class="sxs-lookup"><span data-stu-id="220d9-501">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="220d9-502">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="220d9-502">Overloads permit specifying:</span></span>

* <span data-ttu-id="220d9-503">`Action<KeyPerFileConfigurationSource>` Delegat, który konfiguruje źródło.</span><span class="sxs-lookup"><span data-stu-id="220d9-503">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="220d9-504">Czy katalog jest opcjonalna i ścieżkę do katalogu.</span><span class="sxs-lookup"><span data-stu-id="220d9-504">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="220d9-505">Podwójne podkreślenie (`__`) jest używany jako ogranicznika klucza konfiguracji w nazwach plików.</span><span class="sxs-lookup"><span data-stu-id="220d9-505">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="220d9-506">Na przykład, nazwa_pliku `Logging__LogLevel__System` generuje klucz konfiguracji `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="220d9-506">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="220d9-507">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="220d9-507">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="220d9-508">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-508">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="220d9-509">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="220d9-509">Memory Configuration Provider</span></span>

<span data-ttu-id="220d9-510"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Korzysta z kolekcji w pamięci jako pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-510">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="220d9-511">Aby aktywować konfiguracji kolekcji w pamięci, należy wywołać <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="220d9-511">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="220d9-512">Dostawca konfiguracji mogą być zainicjowane z `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="220d9-512">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="220d9-513">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="220d9-513">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="220d9-514">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-514">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="220d9-515">Podczas wywoływania `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-515">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="220d9-516">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="220d9-516">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-517">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="220d9-517">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="220d9-518">GetValue</span><span class="sxs-lookup"><span data-stu-id="220d9-518">GetValue</span></span>

<span data-ttu-id="220d9-519">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnianie wartości z konfiguracji z określonym kluczem i konwertuje je do określonego typu.</span><span class="sxs-lookup"><span data-stu-id="220d9-519">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="220d9-520">Przeciążenie umożliwia podanie wartości domyślnej, jeśli nie odnaleziono klucza.</span><span class="sxs-lookup"><span data-stu-id="220d9-520">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="220d9-521">Poniższy przykład wyodrębnia wartość ciągu z konfiguracji przy użyciu klucza `NumberKey`, typy wartości jako `int`i przechowuje wartość w zmiennej `intValue`.</span><span class="sxs-lookup"><span data-stu-id="220d9-521">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="220d9-522">Jeśli `NumberKey` nie zostanie odnaleziona w klucze konfiguracji `intValue` odbiera wartość domyślną `99`:</span><span class="sxs-lookup"><span data-stu-id="220d9-522">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="220d9-523">GetSection, GetChildren i istnieje</span><span class="sxs-lookup"><span data-stu-id="220d9-523">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="220d9-524">Aby uzyskać przykłady, które należy wykonać należy wziąć pod uwagę następujący plik JSON.</span><span class="sxs-lookup"><span data-stu-id="220d9-524">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="220d9-525">Cztery klucze znajdują się na dwie sekcje, z których jedna zawiera parę podsekcje:</span><span class="sxs-lookup"><span data-stu-id="220d9-525">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="220d9-526">Gdy plik jest do odczytu do konfiguracji, następujące klucze unikatowe hierarchiczne są tworzone do przechowywania wartości konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="220d9-526">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="220d9-527">section0:key0</span><span class="sxs-lookup"><span data-stu-id="220d9-527">section0:key0</span></span>
* <span data-ttu-id="220d9-528">section0:key1</span><span class="sxs-lookup"><span data-stu-id="220d9-528">section0:key1</span></span>
* <span data-ttu-id="220d9-529">section1:key0</span><span class="sxs-lookup"><span data-stu-id="220d9-529">section1:key0</span></span>
* <span data-ttu-id="220d9-530">section1:key1</span><span class="sxs-lookup"><span data-stu-id="220d9-530">section1:key1</span></span>
* <span data-ttu-id="220d9-531">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="220d9-531">section2:subsection0:key0</span></span>
* <span data-ttu-id="220d9-532">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="220d9-532">section2:subsection0:key1</span></span>
* <span data-ttu-id="220d9-533">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="220d9-533">section2:subsection1:key0</span></span>
* <span data-ttu-id="220d9-534">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="220d9-534">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="220d9-535">GetSection</span><span class="sxs-lookup"><span data-stu-id="220d9-535">GetSection</span></span>

<span data-ttu-id="220d9-536">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcji konfiguracji z kluczem określonym podsekcji.</span><span class="sxs-lookup"><span data-stu-id="220d9-536">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="220d9-537">Aby zwrócić <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz wartość w `section1`, wywołaj `GetSection` i podaj nazwę sekcji:</span><span class="sxs-lookup"><span data-stu-id="220d9-537">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="220d9-538">Podobnie Aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę do sekcji:</span><span class="sxs-lookup"><span data-stu-id="220d9-538">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="220d9-539">`GetSection` nigdy nie zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="220d9-539">`GetSection` never returns `null`.</span></span> <span data-ttu-id="220d9-540">Jeśli nie zostanie znaleziona pasująca sekcja, pusta `IConfigurationSection` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="220d9-540">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="220d9-541">GetChildren —</span><span class="sxs-lookup"><span data-stu-id="220d9-541">GetChildren</span></span>

<span data-ttu-id="220d9-542">Wywołanie [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) na `section2` uzyskuje `IEnumerable<IConfigurationSection>` zawierającej:</span><span class="sxs-lookup"><span data-stu-id="220d9-542">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="220d9-543">Exists</span><span class="sxs-lookup"><span data-stu-id="220d9-543">Exists</span></span>

<span data-ttu-id="220d9-544">Użyj [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) do ustalenia, czy istnieje sekcji konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="220d9-544">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="220d9-545">Podane dane przykładowe `sectionExists` jest `false` , ponieważ nie ma `section2:subsection2` sekcji w danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-545">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="220d9-546">Powiąż z klasy</span><span class="sxs-lookup"><span data-stu-id="220d9-546">Bind to a class</span></span>

<span data-ttu-id="220d9-547">Konfiguracja może być powiązana z klas, które reprezentują grupy powiązane ustawienia za pomocą *wzorzec opcje*.</span><span class="sxs-lookup"><span data-stu-id="220d9-547">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="220d9-548">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="220d9-548">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="220d9-549">Wartości konfiguracji są zwracane jako ciągi, ale wywoływania <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) obiektów.</span><span class="sxs-lookup"><span data-stu-id="220d9-549">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="220d9-550">Przykładowa aplikacja zawiera `Starship` modelu (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="220d9-550">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="220d9-551">`starship` Części *starship.json* plików umożliwia utworzenie konfiguracji podczas Przykładowa aplikacja używa dostawcy konfiguracji JSON można załadować konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="220d9-551">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="220d9-552">Tworzone są następujące pary klucz wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="220d9-552">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="220d9-553">Key</span><span class="sxs-lookup"><span data-stu-id="220d9-553">Key</span></span>                   | <span data-ttu-id="220d9-554">Wartość</span><span class="sxs-lookup"><span data-stu-id="220d9-554">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="220d9-555">starship: Nazwa</span><span class="sxs-lookup"><span data-stu-id="220d9-555">starship:name</span></span>         | <span data-ttu-id="220d9-556">USS przedsiębiorstwa</span><span class="sxs-lookup"><span data-stu-id="220d9-556">USS Enterprise</span></span>                                    |
| <span data-ttu-id="220d9-557">starship: rejestru</span><span class="sxs-lookup"><span data-stu-id="220d9-557">starship:registry</span></span>     | <span data-ttu-id="220d9-558">KCF 1701</span><span class="sxs-lookup"><span data-stu-id="220d9-558">NCC-1701</span></span>                                          |
| <span data-ttu-id="220d9-559">starship: klasy</span><span class="sxs-lookup"><span data-stu-id="220d9-559">starship:class</span></span>        | <span data-ttu-id="220d9-560">Tworzenia</span><span class="sxs-lookup"><span data-stu-id="220d9-560">Constitution</span></span>                                      |
| <span data-ttu-id="220d9-561">starship: długość</span><span class="sxs-lookup"><span data-stu-id="220d9-561">starship:length</span></span>       | <span data-ttu-id="220d9-562">304.8</span><span class="sxs-lookup"><span data-stu-id="220d9-562">304.8</span></span>                                             |
| <span data-ttu-id="220d9-563">starship: upoważnione</span><span class="sxs-lookup"><span data-stu-id="220d9-563">starship:commissioned</span></span> | <span data-ttu-id="220d9-564">False</span><span class="sxs-lookup"><span data-stu-id="220d9-564">False</span></span>                                             |
| <span data-ttu-id="220d9-565">Znak towarowy</span><span class="sxs-lookup"><span data-stu-id="220d9-565">trademark</span></span>             | <span data-ttu-id="220d9-566">Paramount obrazy Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="220d9-566">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="220d9-567">Wywołania aplikacji przykładowej `GetSection` z `starship` klucza.</span><span class="sxs-lookup"><span data-stu-id="220d9-567">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="220d9-568">`starship` Pary klucz wartość są izolowane.</span><span class="sxs-lookup"><span data-stu-id="220d9-568">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="220d9-569">`Bind` Metoda jest wywoływana w podsekcji, przekazując wystąpienie `Starship` klasy.</span><span class="sxs-lookup"><span data-stu-id="220d9-569">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="220d9-570">Po powiązaniu wartości wystąpienia, wystąpienie jest przypisany do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="220d9-570">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="220d9-571">Powiąż z wykresu obiektu</span><span class="sxs-lookup"><span data-stu-id="220d9-571">Bind to an object graph</span></span>

<span data-ttu-id="220d9-572"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> jest w stanie całego grafu obiektów POCO powiązania.</span><span class="sxs-lookup"><span data-stu-id="220d9-572"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="220d9-573">Przykładowy zawiera `TvShow` model zawiera wykres obiektu, którego `Metadata` i `Actors` klasy (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="220d9-573">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="220d9-574">Przykładowa aplikacja ma *tvshow.xml* plik zawierający dane konfiguracyjne:</span><span class="sxs-lookup"><span data-stu-id="220d9-574">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="220d9-575">Konfiguracja jest powiązany z całej `TvShow` wykres obiektu z `Bind` metody.</span><span class="sxs-lookup"><span data-stu-id="220d9-575">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="220d9-576">Powiązane wystąpienia jest przypisywany do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="220d9-576">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="220d9-577">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) wiąże i zwraca wartość określonego typu.</span><span class="sxs-lookup"><span data-stu-id="220d9-577">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="220d9-578">`Get<T>` jest bardziej wygodne niż przy użyciu `Bind`.</span><span class="sxs-lookup"><span data-stu-id="220d9-578">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="220d9-579">Poniższy kod przedstawia sposób użycia `Get<T>` w poprzednim przykładzie, co umożliwia wystąpienie powiązanych, którego można przypisać bezpośrednio do właściwości, używany do renderowania:</span><span class="sxs-lookup"><span data-stu-id="220d9-579">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="220d9-580">Powiąż tablicę do klasy</span><span class="sxs-lookup"><span data-stu-id="220d9-580">Bind an array to a class</span></span>

<span data-ttu-id="220d9-581">*Przykładowa aplikacja pokazuje pojęcia opisane w tej sekcji.*</span><span class="sxs-lookup"><span data-stu-id="220d9-581">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="220d9-582"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Obsługuje tablice powiązania do obiektów przy użyciu tablicy indeksów w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-582">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="220d9-583">Dowolnym formacie tablicy, który udostępnia liczbowych segment klucza (`:0:`, `:1:`, &hellip; `:{n}:`) jest w stanie tablicy powiązania z macierzą POCO klasy.</span><span class="sxs-lookup"><span data-stu-id="220d9-583">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="220d9-584">Powiązanie jest zapewniana przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="220d9-584">Binding is provided by convention.</span></span> <span data-ttu-id="220d9-585">Konfiguracja niestandardowa dostawców nie są wymagane do zaimplementowania tablicy powiązania.</span><span class="sxs-lookup"><span data-stu-id="220d9-585">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="220d9-586">**Przetwarzanie w pamięci tablica**</span><span class="sxs-lookup"><span data-stu-id="220d9-586">**In-memory array processing**</span></span>

<span data-ttu-id="220d9-587">Należy wziąć pod uwagę kluczy i wartości konfiguracji pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="220d9-587">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="220d9-588">Key</span><span class="sxs-lookup"><span data-stu-id="220d9-588">Key</span></span>             | <span data-ttu-id="220d9-589">Wartość</span><span class="sxs-lookup"><span data-stu-id="220d9-589">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="220d9-590">Macierz: wpisów: 0</span><span class="sxs-lookup"><span data-stu-id="220d9-590">array:entries:0</span></span> | <span data-ttu-id="220d9-591">value0</span><span class="sxs-lookup"><span data-stu-id="220d9-591">value0</span></span> |
| <span data-ttu-id="220d9-592">Macierz: wpisów: 1</span><span class="sxs-lookup"><span data-stu-id="220d9-592">array:entries:1</span></span> | <span data-ttu-id="220d9-593">Wartość1</span><span class="sxs-lookup"><span data-stu-id="220d9-593">value1</span></span> |
| <span data-ttu-id="220d9-594">Macierz: wpisów: 2</span><span class="sxs-lookup"><span data-stu-id="220d9-594">array:entries:2</span></span> | <span data-ttu-id="220d9-595">Wartość2</span><span class="sxs-lookup"><span data-stu-id="220d9-595">value2</span></span> |
| <span data-ttu-id="220d9-596">Macierz: wpisów: 4</span><span class="sxs-lookup"><span data-stu-id="220d9-596">array:entries:4</span></span> | <span data-ttu-id="220d9-597">Wartość4</span><span class="sxs-lookup"><span data-stu-id="220d9-597">value4</span></span> |
| <span data-ttu-id="220d9-598">Macierz: wpisów: 5</span><span class="sxs-lookup"><span data-stu-id="220d9-598">array:entries:5</span></span> | <span data-ttu-id="220d9-599">Wartość5</span><span class="sxs-lookup"><span data-stu-id="220d9-599">value5</span></span> |

<span data-ttu-id="220d9-600">Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:</span><span class="sxs-lookup"><span data-stu-id="220d9-600">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="220d9-601">Tablica pomija wartość indeksu &num;3.</span><span class="sxs-lookup"><span data-stu-id="220d9-601">The array skips a value for index &num;3.</span></span> <span data-ttu-id="220d9-602">Konfiguracja obiektu wiążącego nie jest zdolny do powiązania wartości null lub tworzenie wartości null wpisów w powiązanych obiektów, które stają się w chwili, gdy przedstawiono wynik powiązania tej tablicy do obiektu.</span><span class="sxs-lookup"><span data-stu-id="220d9-602">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="220d9-603">W przykładowej aplikacji do przechowywania danych konfiguracji powiązanej dostępnej klasy POCO:</span><span class="sxs-lookup"><span data-stu-id="220d9-603">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="220d9-604">Dane konfiguracji jest powiązana z obiektem:</span><span class="sxs-lookup"><span data-stu-id="220d9-604">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="220d9-605">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) składni można również użyć, która skutkuje bardziej kompaktowy kod:</span><span class="sxs-lookup"><span data-stu-id="220d9-605">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="220d9-606">Powiązany obiekt, wystąpienie `ArrayExample`, odbiera dane tablicy z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-606">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="220d9-607">`ArrayExample.Entries` Indeks</span><span class="sxs-lookup"><span data-stu-id="220d9-607">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="220d9-608">`ArrayExample.Entries` Wartość</span><span class="sxs-lookup"><span data-stu-id="220d9-608">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="220d9-609">0</span><span class="sxs-lookup"><span data-stu-id="220d9-609">0</span></span>                            | <span data-ttu-id="220d9-610">value0</span><span class="sxs-lookup"><span data-stu-id="220d9-610">value0</span></span>                       |
| <span data-ttu-id="220d9-611">1</span><span class="sxs-lookup"><span data-stu-id="220d9-611">1</span></span>                            | <span data-ttu-id="220d9-612">Wartość1</span><span class="sxs-lookup"><span data-stu-id="220d9-612">value1</span></span>                       |
| <span data-ttu-id="220d9-613">2</span><span class="sxs-lookup"><span data-stu-id="220d9-613">2</span></span>                            | <span data-ttu-id="220d9-614">Wartość2</span><span class="sxs-lookup"><span data-stu-id="220d9-614">value2</span></span>                       |
| <span data-ttu-id="220d9-615">3</span><span class="sxs-lookup"><span data-stu-id="220d9-615">3</span></span>                            | <span data-ttu-id="220d9-616">Wartość4</span><span class="sxs-lookup"><span data-stu-id="220d9-616">value4</span></span>                       |
| <span data-ttu-id="220d9-617">4</span><span class="sxs-lookup"><span data-stu-id="220d9-617">4</span></span>                            | <span data-ttu-id="220d9-618">Wartość5</span><span class="sxs-lookup"><span data-stu-id="220d9-618">value5</span></span>                       |

<span data-ttu-id="220d9-619">Indeks &num;3 w powiązany obiekt przechowuje dane konfiguracyjne `array:4` klucz konfiguracji i jego wartość `value4`.</span><span class="sxs-lookup"><span data-stu-id="220d9-619">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="220d9-620">Gdy dane konfiguracji, które zawiera tablicę jest powiązana, indeksy tablicy w klucze konfiguracji jedynie służą do iteracji dane konfiguracji, podczas tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="220d9-620">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="220d9-621">Wartość null nie mogą być przechowywane w danych konfiguracji, a wpis o wartości null nie jest tworzona w powiązany obiekt, w przypadku tablicy w konfiguracji kluczy Pomiń jeden lub więcej indeksów.</span><span class="sxs-lookup"><span data-stu-id="220d9-621">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="220d9-622">Brak elementu konfiguracji dla indeksu &num;3 mogą być dostarczane przed powiązanie `ArrayExample` wystąpienie przez dowolnego dostawcę konfiguracji, tworzącego prawidłowe pary klucz wartość w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-622">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="220d9-623">Jeśli przykład uwzględniony dodatkowego dostawcę konfiguracji JSON z Brak pary klucz wartość `ArrayExample.Entries` pasuje do tablicy kompletna konfiguracja:</span><span class="sxs-lookup"><span data-stu-id="220d9-623">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="220d9-624">*missing_value.JSON*:</span><span class="sxs-lookup"><span data-stu-id="220d9-624">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="220d9-625">W <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="220d9-625">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="220d9-626">W `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="220d9-626">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="220d9-627">Pary klucz wartość, jak pokazano w tabeli są ładowane do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-627">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="220d9-628">Key</span><span class="sxs-lookup"><span data-stu-id="220d9-628">Key</span></span>             | <span data-ttu-id="220d9-629">Wartość</span><span class="sxs-lookup"><span data-stu-id="220d9-629">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="220d9-630">Macierz: wpisów: 3</span><span class="sxs-lookup"><span data-stu-id="220d9-630">array:entries:3</span></span> | <span data-ttu-id="220d9-631">Wartość3</span><span class="sxs-lookup"><span data-stu-id="220d9-631">value3</span></span> |

<span data-ttu-id="220d9-632">Jeśli `ArrayExample` wystąpienia klasy jest powiązana po dostawcę konfiguracji JSON zawiera wpis dla indeksu &num;3, `ArrayExample.Entries` tablicy zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="220d9-632">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="220d9-633">`ArrayExample.Entries` Indeks</span><span class="sxs-lookup"><span data-stu-id="220d9-633">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="220d9-634">`ArrayExample.Entries` Wartość</span><span class="sxs-lookup"><span data-stu-id="220d9-634">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="220d9-635">0</span><span class="sxs-lookup"><span data-stu-id="220d9-635">0</span></span>                            | <span data-ttu-id="220d9-636">value0</span><span class="sxs-lookup"><span data-stu-id="220d9-636">value0</span></span>                       |
| <span data-ttu-id="220d9-637">1</span><span class="sxs-lookup"><span data-stu-id="220d9-637">1</span></span>                            | <span data-ttu-id="220d9-638">Wartość1</span><span class="sxs-lookup"><span data-stu-id="220d9-638">value1</span></span>                       |
| <span data-ttu-id="220d9-639">2</span><span class="sxs-lookup"><span data-stu-id="220d9-639">2</span></span>                            | <span data-ttu-id="220d9-640">Wartość2</span><span class="sxs-lookup"><span data-stu-id="220d9-640">value2</span></span>                       |
| <span data-ttu-id="220d9-641">3</span><span class="sxs-lookup"><span data-stu-id="220d9-641">3</span></span>                            | <span data-ttu-id="220d9-642">Wartość3</span><span class="sxs-lookup"><span data-stu-id="220d9-642">value3</span></span>                       |
| <span data-ttu-id="220d9-643">4</span><span class="sxs-lookup"><span data-stu-id="220d9-643">4</span></span>                            | <span data-ttu-id="220d9-644">Wartość4</span><span class="sxs-lookup"><span data-stu-id="220d9-644">value4</span></span>                       |
| <span data-ttu-id="220d9-645">5</span><span class="sxs-lookup"><span data-stu-id="220d9-645">5</span></span>                            | <span data-ttu-id="220d9-646">Wartość5</span><span class="sxs-lookup"><span data-stu-id="220d9-646">value5</span></span>                       |

<span data-ttu-id="220d9-647">**Przetwarzanie tablicy JSON**</span><span class="sxs-lookup"><span data-stu-id="220d9-647">**JSON array processing**</span></span>

<span data-ttu-id="220d9-648">Jeśli plik JSON zawiera tablicę, klucze konfiguracji zostaną utworzone dla elementów tablicy przy użyciu indeksu zaczynającego się od zera sekcji.</span><span class="sxs-lookup"><span data-stu-id="220d9-648">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="220d9-649">W następującym pliku konfiguracji `subsection` jest tablicą:</span><span class="sxs-lookup"><span data-stu-id="220d9-649">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="220d9-650">Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz wartość:</span><span class="sxs-lookup"><span data-stu-id="220d9-650">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="220d9-651">Key</span><span class="sxs-lookup"><span data-stu-id="220d9-651">Key</span></span>                     | <span data-ttu-id="220d9-652">Wartość</span><span class="sxs-lookup"><span data-stu-id="220d9-652">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="220d9-653">json_array:key</span><span class="sxs-lookup"><span data-stu-id="220d9-653">json_array:key</span></span>          | <span data-ttu-id="220d9-654">Wartośća</span><span class="sxs-lookup"><span data-stu-id="220d9-654">valueA</span></span> |
| <span data-ttu-id="220d9-655">json_array:Subsection:0</span><span class="sxs-lookup"><span data-stu-id="220d9-655">json_array:subsection:0</span></span> | <span data-ttu-id="220d9-656">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="220d9-656">valueB</span></span> |
| <span data-ttu-id="220d9-657">json_array:Subsection:1</span><span class="sxs-lookup"><span data-stu-id="220d9-657">json_array:subsection:1</span></span> | <span data-ttu-id="220d9-658">valueC</span><span class="sxs-lookup"><span data-stu-id="220d9-658">valueC</span></span> |
| <span data-ttu-id="220d9-659">json_array:Subsection:2</span><span class="sxs-lookup"><span data-stu-id="220d9-659">json_array:subsection:2</span></span> | <span data-ttu-id="220d9-660">zwracającej</span><span class="sxs-lookup"><span data-stu-id="220d9-660">valueD</span></span> |

<span data-ttu-id="220d9-661">W przykładowej aplikacji następującej klasy POCO jest dostępny dla powiązania pary klucz wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="220d9-661">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="220d9-662">Po powiązaniu `JsonArrayExample.Key` przechowuje wartość `valueA`.</span><span class="sxs-lookup"><span data-stu-id="220d9-662">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="220d9-663">Wartości podsekcji są przechowywane we właściwości tablicy POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="220d9-663">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="220d9-664">`JsonArrayExample.Subsection` Indeks</span><span class="sxs-lookup"><span data-stu-id="220d9-664">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="220d9-665">`JsonArrayExample.Subsection` Wartość</span><span class="sxs-lookup"><span data-stu-id="220d9-665">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="220d9-666">0</span><span class="sxs-lookup"><span data-stu-id="220d9-666">0</span></span>                                   | <span data-ttu-id="220d9-667">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="220d9-667">valueB</span></span>                              |
| <span data-ttu-id="220d9-668">1</span><span class="sxs-lookup"><span data-stu-id="220d9-668">1</span></span>                                   | <span data-ttu-id="220d9-669">valueC</span><span class="sxs-lookup"><span data-stu-id="220d9-669">valueC</span></span>                              |
| <span data-ttu-id="220d9-670">2</span><span class="sxs-lookup"><span data-stu-id="220d9-670">2</span></span>                                   | <span data-ttu-id="220d9-671">zwracającej</span><span class="sxs-lookup"><span data-stu-id="220d9-671">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="220d9-672">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="220d9-672">Custom configuration provider</span></span>

<span data-ttu-id="220d9-673">Przykładowa aplikacja pokazuje, jak utworzyć dostawcę konfiguracji podstawowej, który odczytuje pary klucz wartość konfiguracji z bazy danych za pomocą [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="220d9-673">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="220d9-674">Dostawcę ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="220d9-674">The provider has the following characteristics:</span></span>

* <span data-ttu-id="220d9-675">Bazy danych w pamięci programu EF jest używana do celów demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="220d9-675">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="220d9-676">Aby użyć bazy danych, która wymaga parametrów połączenia, należy zaimplementować pomocniczy `ConfigurationBuilder` podawać parametrów połączenia z innego dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="220d9-676">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="220d9-677">Dostawca odczytuje tabelę bazy danych do konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="220d9-677">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="220d9-678">Dostawca nie kwerendy bazy danych na podstawie-key.</span><span class="sxs-lookup"><span data-stu-id="220d9-678">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="220d9-679">Załaduj ponownie przy zmianie nie jest zaimplementowany, więc zaktualizowanie bazy danych, po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="220d9-679">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="220d9-680">Zdefiniuj `EFConfigurationValue` jednostki do przechowywania wartości konfiguracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="220d9-680">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="220d9-681">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="220d9-681">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="220d9-682">Dodaj `EFConfigurationContext` do przechowywania i uzyskiwania dostępu skonfigurowane wartości.</span><span class="sxs-lookup"><span data-stu-id="220d9-682">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="220d9-683">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="220d9-683">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="220d9-684">Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="220d9-684">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="220d9-685">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="220d9-685">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="220d9-686">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="220d9-686">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="220d9-687">Dostawca konfiguracji inicjuje bazy danych, gdy jest on pusty.</span><span class="sxs-lookup"><span data-stu-id="220d9-687">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="220d9-688">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="220d9-688">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="220d9-689">`AddEFConfiguration` Metody rozszerzenia, który pozwala na dodawanie źródła konfiguracji do `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="220d9-689">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="220d9-690">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="220d9-690">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="220d9-691">Poniższy kod przedstawia sposób używania niestandardowego `EFConfigurationProvider` w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="220d9-691">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="220d9-692">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="220d9-692">Access configuration during startup</span></span>

<span data-ttu-id="220d9-693">Wstrzykiwanie `IConfiguration` do `Startup` konstruktora do dostępu do wartości konfiguracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="220d9-693">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="220d9-694">Uzyskać dostępu do konfiguracji w `Startup.Configure`, albo wstrzyknąć `IConfiguration` bezpośrednio do metody lub użyj wystąpienia z konstruktora:</span><span class="sxs-lookup"><span data-stu-id="220d9-694">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="220d9-695">Na przykład uzyskiwania dostępu do konfiguracji za pomocą metod jako udogodnienie uruchamiania zobacz [uruchamiania aplikacji: Wygodne metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="220d9-695">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="220d9-696">Konfiguracja dostępu na stronie stron Razor lub w widoku MVC</span><span class="sxs-lookup"><span data-stu-id="220d9-696">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="220d9-697">Aby uzyskać dostęp do ustawień konfiguracji na stronie stron Razor lub widoku MVC, należy dodać [użycie dyrektywy](xref:mvc/views/razor#using) ([odwołanie w C#: using — dyrektywa](/dotnet/csharp/language-reference/keywords/using-directive)) dla [Microsoft.Extensions.Configuration przestrzeni nazw ](xref:Microsoft.Extensions.Configuration) i wstawiać <xref:Microsoft.Extensions.Configuration.IConfiguration> do strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="220d9-697">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="220d9-698">Na stronie stron Razor:</span><span class="sxs-lookup"><span data-stu-id="220d9-698">In a Razor Pages page:</span></span>

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

<span data-ttu-id="220d9-699">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="220d9-699">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="220d9-700">Dodaj konfigurację z zewnętrznego zestawu</span><span class="sxs-lookup"><span data-stu-id="220d9-700">Add configuration from an external assembly</span></span>

<span data-ttu-id="220d9-701"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Implementacji umożliwia dodawanie rozszerzeń do aplikacji przy uruchamianiu z zewnętrznego zestawu poza aplikacji `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="220d9-701">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="220d9-702">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="220d9-702">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="220d9-703">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="220d9-703">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="220d9-704">Głębszej analizy konfiguracji firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="220d9-704">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
