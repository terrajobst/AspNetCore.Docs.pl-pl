---
title: Konfiguracja w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core za pomocą interfejsu API konfiguracji.
ms.author: riande
ms.custom: mvc
ms.date: 01/25/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 2465570e469020ae2855508bd1bfc8528e188ebb
ms.sourcegitcommit: ca5f03210bedc61c6639a734ae5674bfe095dee8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55073169"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="c2962-103">Konfiguracja w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2962-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="c2962-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c2962-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c2962-105">Konfiguracja aplikacji w programie ASP.NET Core opiera się na parach klucz wartość ustanowione przez *dostawcy konfiguracji*.</span><span class="sxs-lookup"><span data-stu-id="c2962-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="c2962-106">Dostawcy konfiguracji odczytania danych konfiguracyjnych do pary klucz wartość z wielu źródeł w konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c2962-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="c2962-107">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c2962-107">Azure Key Vault</span></span>
* <span data-ttu-id="c2962-108">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-108">Command-line arguments</span></span>
* <span data-ttu-id="c2962-109">Niestandardowi dostawcy (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="c2962-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="c2962-110">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="c2962-110">Directory files</span></span>
* <span data-ttu-id="c2962-111">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c2962-111">Environment variables</span></span>
* <span data-ttu-id="c2962-112">Obiekty platformy .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="c2962-112">In-memory .NET objects</span></span>
* <span data-ttu-id="c2962-113">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="c2962-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="c2962-114">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c2962-114">Azure Key Vault</span></span>
* <span data-ttu-id="c2962-115">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-115">Command-line arguments</span></span>
* <span data-ttu-id="c2962-116">Niestandardowi dostawcy (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="c2962-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="c2962-117">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c2962-117">Environment variables</span></span>
* <span data-ttu-id="c2962-118">Obiekty platformy .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="c2962-118">In-memory .NET objects</span></span>
* <span data-ttu-id="c2962-119">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="c2962-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="c2962-120">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-120">Command-line arguments</span></span>
* <span data-ttu-id="c2962-121">Niestandardowi dostawcy (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="c2962-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="c2962-122">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c2962-122">Environment variables</span></span>
* <span data-ttu-id="c2962-123">Obiekty platformy .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="c2962-123">In-memory .NET objects</span></span>
* <span data-ttu-id="c2962-124">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="c2962-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="c2962-125">*Wzorzec opcje* jest rozszerzeniem pojęć związanych z konfiguracją opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="c2962-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="c2962-126">Opcje używa klas do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="c2962-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="c2962-127">Aby uzyskać więcej informacji na temat korzystania z wzorca opcji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="c2962-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="c2962-128">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c2962-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c2962-129">Te trzy pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-129">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c2962-130">Te trzy pakiety są objęte [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="c2962-130">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="c2962-131">Hostowanie i konfiguracji aplikacji</span><span class="sxs-lookup"><span data-stu-id="c2962-131">Host vs. app configuration</span></span>

<span data-ttu-id="c2962-132">Zanim aplikacja jest skonfigurowana i uruchomiona, *hosta* skonfigurowany i uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="c2962-132">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="c2962-133">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-133">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="c2962-134">Zarówno aplikacja, jak i hosta są skonfigurowane przy użyciu dostawcy konfiguracji opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="c2962-134">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="c2962-135">Pary klucz wartość konfiguracji hosta stają się częścią globalnej konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-135">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="c2962-136">Aby uzyskać więcej informacji na temat sposobu konfiguracji dostawcy są używane, gdy host jest wbudowana i wpływ źródła konfiguracji hosta konfiguracji, zobacz <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="c2962-136">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="c2962-137">Konfiguracja domyślna</span><span class="sxs-lookup"><span data-stu-id="c2962-137">Default configuration</span></span>

<span data-ttu-id="c2962-138">Aplikacje oparte na platformy ASP.NET Core sieci Web [dotnet nowe](/dotnet/core/tools/dotnet-new) wywołania szablonów <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> podczas tworzenia na hoście.</span><span class="sxs-lookup"><span data-stu-id="c2962-138">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="c2962-139">`CreateDefaultBuilder` udostępnia domyślną konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-139">`CreateDefaultBuilder` provides default configuration for the app.</span></span>

* <span data-ttu-id="c2962-140">Konfiguracja hosta jest dostarczana z:</span><span class="sxs-lookup"><span data-stu-id="c2962-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="c2962-141">Zmienne środowiskowe prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`) przy użyciu [dostawcę konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c2962-141">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="c2962-142">Argumenty wiersza polecenia przy użyciu [dostawcę konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c2962-142">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="c2962-143">Konfiguracja aplikacji jest dostarczana z (w następującej kolejności):</span><span class="sxs-lookup"><span data-stu-id="c2962-143">App configuration is provided from (in the following order):</span></span>
  * <span data-ttu-id="c2962-144">*appSettings.JSON* przy użyciu [dostawcę konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c2962-144">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="c2962-145">*appSettings. {Środowiska} .json* przy użyciu [dostawcę konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c2962-145">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="c2962-146">[Klucz tajny Menedżera](xref:security/app-secrets) uruchamiania aplikacji `Development` środowisko przy użyciu zestawu wpisu.</span><span class="sxs-lookup"><span data-stu-id="c2962-146">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="c2962-147">Zmienne środowiskowe za pomocą [dostawcę konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c2962-147">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="c2962-148">Argumenty wiersza polecenia przy użyciu [dostawcę konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c2962-148">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="c2962-149">Dostawcy konfiguracji zostały omówione w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="c2962-149">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="c2962-150">Aby uzyskać więcej informacji na hoście i `CreateDefaultBuilder`, zobacz <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="c2962-150">For more information on the host and `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="c2962-151">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="c2962-151">Security</span></span>

<span data-ttu-id="c2962-152">Przyjmuje następujące najlepsze rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="c2962-152">Adopt the following best practices:</span></span>

* <span data-ttu-id="c2962-153">Nigdy nie przechowują hasła lub innych danych poufnych w kodzie dostawcy konfiguracji lub w plikach konfiguracyjnych w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="c2962-153">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="c2962-154">Nie używać kluczy tajnych w środowisku produkcyjnym w trakcie opracowywania lub środowisk testowych.</span><span class="sxs-lookup"><span data-stu-id="c2962-154">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="c2962-155">Określ klucze tajne poza projektem, nie może być przypadkowo wszelkich starań, by repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="c2962-155">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="c2962-156">Dowiedz się więcej o [jak używać wielu środowisk](xref:fundamentals/environments) i zarządzanie nimi [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie programowania przy użyciu klucza tajnego Menedżera](xref:security/app-secrets) (zawiera wskazówki dotyczące używania zmiennych środowiskowych do przechowywania dane poufne).</span><span class="sxs-lookup"><span data-stu-id="c2962-156">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="c2962-157">Menedżer klucz tajny używa dostawcy konfiguracji plików do przechowywania kluczy tajnych użytkownika w pliku JSON w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="c2962-157">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="c2962-158">Dostawca konfiguracji pliku jest opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="c2962-158">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="c2962-159">[Usługa Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stanowi jedną z opcji dla bezpieczne przechowywanie kluczy tajnych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="c2962-160">Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c2962-160">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="c2962-161">Dane hierarchiczne konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c2962-161">Hierarchical configuration data</span></span>

<span data-ttu-id="c2962-162">Interfejs API konfiguracji jest zdolny do utrzymywania dane hierarchiczne konfiguracji spłaszczając dane hierarchiczne przy użyciu ogranicznika w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-162">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="c2962-163">W następującym pliku JSON cztery klucze, istnieją w ze strukturą hierarchii dwie sekcje:</span><span class="sxs-lookup"><span data-stu-id="c2962-163">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="c2962-164">Gdy plik jest do odczytu do konfiguracji, unikatowe klucze są tworzone do obsługi struktury danych hierarchicznych oryginalnego źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-164">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="c2962-165">Sekcji i kluczy są spłaszczone przy użyciu dwukropek (`:`) do pierwotnej struktury obsługi:</span><span class="sxs-lookup"><span data-stu-id="c2962-165">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="c2962-166">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c2962-166">section0:key0</span></span>
* <span data-ttu-id="c2962-167">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c2962-167">section0:key1</span></span>
* <span data-ttu-id="c2962-168">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c2962-168">section1:key0</span></span>
* <span data-ttu-id="c2962-169">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c2962-169">section1:key1</span></span>

<span data-ttu-id="c2962-170"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> metody są dostępne do izolowania sekcje i elementy podrzędne do sekcji w danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-170"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="c2962-171">Te metody są opisane w dalszej części w [GetSection GetChildren i Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="c2962-171">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="c2962-172">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-172">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="c2962-173">Konwencje</span><span class="sxs-lookup"><span data-stu-id="c2962-173">Conventions</span></span>

<span data-ttu-id="c2962-174">Przy uruchamianiu aplikacji źródła konfiguracji są do odczytu w kolejności, że podano ich dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-174">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="c2962-175">Dostawcy konfiguracji pliku mają możliwość Załaduj ponownie konfigurację, gdy podstawowy plik ustawień zostanie zmieniona po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-175">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="c2962-176">Dostawca konfiguracji pliku jest opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="c2962-176">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="c2962-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> jest dostępna w aplikacji [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="c2962-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="c2962-178">Dostawcy konfiguracji nie może wykorzystywać DI, ponieważ nie jest dostępna podczas konfigurowania one przez hosta.</span><span class="sxs-lookup"><span data-stu-id="c2962-178">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="c2962-179">Klucze konfiguracji przyjęcie następujących konwencji:</span><span class="sxs-lookup"><span data-stu-id="c2962-179">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="c2962-180">Kluczy jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="c2962-180">Keys are case-insensitive.</span></span> <span data-ttu-id="c2962-181">Na przykład `ConnectionString` i `connectionstring` są traktowane jako równoważne kluczy.</span><span class="sxs-lookup"><span data-stu-id="c2962-181">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="c2962-182">Jeśli ten sam klucz jest ustawiany przez dostawców o tej samej lub innej konfiguracji, ostatnia wartość klucza jest wartość.</span><span class="sxs-lookup"><span data-stu-id="c2962-182">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="c2962-183">Klucze hierarchicznych</span><span class="sxs-lookup"><span data-stu-id="c2962-183">Hierarchical keys</span></span>
  * <span data-ttu-id="c2962-184">W ramach konfiguracji interfejsu API, separator dwukropka (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="c2962-184">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="c2962-185">W zmiennych środowiskowych separator dwukropek, może nie działać na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="c2962-185">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="c2962-186">Podwójnym podkreśleniem (`__`) jest obsługiwana przez wszystkie platformy i jest konwertowany na dwukropka.</span><span class="sxs-lookup"><span data-stu-id="c2962-186">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="c2962-187">W usłudze Azure Key Vault, klucze hierarchiczne, użyj `--` (dwa łączniki) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="c2962-187">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="c2962-188">Należy podać kod, aby zastąpić kresek dwukropka po wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-188">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="c2962-189"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Obsługuje tablice powiązania do obiektów przy użyciu tablicy indeksów w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-189">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="c2962-190">Powiązanie tablicy jest opisana w [tablicy należy powiązać klasę](#bind-an-array-to-a-class) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2962-190">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="c2962-191">Wartości konfiguracji przyjęcie następujących konwencji:</span><span class="sxs-lookup"><span data-stu-id="c2962-191">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="c2962-192">Wartości są ciągami.</span><span class="sxs-lookup"><span data-stu-id="c2962-192">Values are strings.</span></span>
* <span data-ttu-id="c2962-193">Wartości null nie przechowywanych w konfiguracji lub powiązane z obiektami.</span><span class="sxs-lookup"><span data-stu-id="c2962-193">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="c2962-194">dostawcy</span><span class="sxs-lookup"><span data-stu-id="c2962-194">Providers</span></span>

<span data-ttu-id="c2962-195">W poniższej tabeli przedstawiono dostępne dla aplikacji platformy ASP.NET Core dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-195">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="c2962-196">Dostawca</span><span class="sxs-lookup"><span data-stu-id="c2962-196">Provider</span></span> | <span data-ttu-id="c2962-197">Udostępnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="c2962-197">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="c2962-198">[Dostawca konfiguracji magazynu w usłudze Azure klucza](xref:security/key-vault-configuration) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="c2962-198">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="c2962-199">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c2962-199">Azure Key Vault</span></span> |
| [<span data-ttu-id="c2962-200">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-200">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="c2962-201">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-201">Command-line parameters</span></span> |
| [<span data-ttu-id="c2962-202">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="c2962-202">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="c2962-203">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="c2962-203">Custom source</span></span> |
| [<span data-ttu-id="c2962-204">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="c2962-204">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="c2962-205">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c2962-205">Environment variables</span></span> |
| [<span data-ttu-id="c2962-206">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="c2962-206">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="c2962-207">Pliki INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="c2962-207">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="c2962-208">Dostawca klucza każdego pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c2962-208">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="c2962-209">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="c2962-209">Directory files</span></span> |
| [<span data-ttu-id="c2962-210">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="c2962-210">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="c2962-211">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="c2962-211">In-memory collections</span></span> |
| <span data-ttu-id="c2962-212">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="c2962-212">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="c2962-213">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="c2962-213">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="c2962-214">Dostawca</span><span class="sxs-lookup"><span data-stu-id="c2962-214">Provider</span></span> | <span data-ttu-id="c2962-215">Udostępnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="c2962-215">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="c2962-216">[Dostawca konfiguracji magazynu w usłudze Azure klucza](xref:security/key-vault-configuration) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="c2962-216">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="c2962-217">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c2962-217">Azure Key Vault</span></span> |
| [<span data-ttu-id="c2962-218">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-218">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="c2962-219">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-219">Command-line parameters</span></span> |
| [<span data-ttu-id="c2962-220">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="c2962-220">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="c2962-221">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="c2962-221">Custom source</span></span> |
| [<span data-ttu-id="c2962-222">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="c2962-222">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="c2962-223">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c2962-223">Environment variables</span></span> |
| [<span data-ttu-id="c2962-224">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="c2962-224">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="c2962-225">Pliki INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="c2962-225">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="c2962-226">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="c2962-226">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="c2962-227">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="c2962-227">In-memory collections</span></span> |
| <span data-ttu-id="c2962-228">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="c2962-228">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="c2962-229">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="c2962-229">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="c2962-230">Dostawca</span><span class="sxs-lookup"><span data-stu-id="c2962-230">Provider</span></span> | <span data-ttu-id="c2962-231">Udostępnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="c2962-231">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="c2962-232">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-232">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="c2962-233">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-233">Command-line parameters</span></span> |
| [<span data-ttu-id="c2962-234">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="c2962-234">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="c2962-235">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="c2962-235">Custom source</span></span> |
| [<span data-ttu-id="c2962-236">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="c2962-236">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="c2962-237">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c2962-237">Environment variables</span></span> |
| [<span data-ttu-id="c2962-238">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="c2962-238">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="c2962-239">Pliki INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="c2962-239">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="c2962-240">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="c2962-240">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="c2962-241">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="c2962-241">In-memory collections</span></span> |
| <span data-ttu-id="c2962-242">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="c2962-242">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="c2962-243">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="c2962-243">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="c2962-244">Źródła konfiguracji są do odczytu w kolejności określono ich dostawców konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="c2962-244">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="c2962-245">Dostawcy konfiguracji opisanych w tym temacie są opisane w kolejności alfabetycznej, a nie w kolejności, Twój kod może zorganizować ich.</span><span class="sxs-lookup"><span data-stu-id="c2962-245">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="c2962-246">Kolejność dostawców konfiguracji w kodzie do własnych priorytetów do bazowego źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-246">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="c2962-247">Jest zwykle kolejność dostawców konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c2962-247">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="c2962-248">Pliki (*appsettings.json*, *appsettings. { Środowisko} .json*, gdzie `{Environment}` jest bieżące środowisko hostingu aplikacji)</span><span class="sxs-lookup"><span data-stu-id="c2962-248">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="c2962-249">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c2962-249">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="c2962-250">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym tylko)</span><span class="sxs-lookup"><span data-stu-id="c2962-250">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="c2962-251">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c2962-251">Environment variables</span></span>
1. <span data-ttu-id="c2962-252">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-252">Command-line arguments</span></span>

<span data-ttu-id="c2962-253">Jest to powszechną praktyką do ostatniej pozycji dostawcę konfiguracji wiersza polecenia w serii dostawcy, aby zezwolić na argumenty wiersza polecenia zastąpić konfiguracji ustawione przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="c2962-253">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2962-254">Ta sekwencja dostawcy są umieszczane w miejscu, podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-254">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="c2962-255">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c2962-255">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-256">Ta sekwencja dostawcy mogą być tworzone dla aplikacji (nie hosta) za pomocą <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> i wywołanie w celu jego <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in Class metoda `Startup`:</span><span class="sxs-lookup"><span data-stu-id="c2962-256">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="c2962-257">W powyższym przykładzie nazwa środowiska (`env.EnvironmentName`) i nazwę zestawu aplikacji (`env.ApplicationName`) są dostarczane przez <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="c2962-257">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="c2962-258">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="c2962-258">For more information, see <xref:fundamentals/environments>.</span></span> <span data-ttu-id="c2962-259">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-259">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c2962-260">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-260">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
<span data-ttu-id="c2962-261">.</span><span class="sxs-lookup"><span data-stu-id="c2962-261">.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="c2962-262">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="c2962-262">ConfigureAppConfiguration</span></span>

<span data-ttu-id="c2962-263">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta do określenia aplikacji dostawcy konfiguracji oprócz tych dodawane automatycznie przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="c2962-263">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="c2962-264">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-264">Command-line Configuration Provider</span></span>

<span data-ttu-id="c2962-265"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Ładuje konfiguracji z pary klucz wartość argumentu wiersza polecenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c2962-265">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="c2962-266">Aby aktywować wiersza polecenia konfiguracji, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metoda rozszerzająca zostanie wywołana w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-266">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2962-267">`AddCommandLine` jest wywoływana automatycznie podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-267">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="c2962-268">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c2962-268">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="c2962-269">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c2962-269">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c2962-270">Konfiguracja opcjonalna z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="c2962-270">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="c2962-271">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="c2962-271">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="c2962-272">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="c2962-272">Environment variables.</span></span>

<span data-ttu-id="c2962-273">`CreateDefaultBuilder` dodaje ostatniego wiersza polecenia dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-273">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="c2962-274">Argumenty wiersza polecenia przekazywane w czasie wykonywania zastąpić konfiguracji ustawione przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="c2962-274">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="c2962-275">`CreateDefaultBuilder` działa, gdy host jest konstruowany.</span><span class="sxs-lookup"><span data-stu-id="c2962-275">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="c2962-276">W związku z tym, wiersza polecenia konfiguracji aktywowany przez `CreateDefaultBuilder` mogą mieć wpływ na sposób host jest skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="c2962-276">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c2962-277">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-277">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="c2962-278">`AddCommandLine` zostały już wywołane `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c2962-278">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c2962-279">Jeśli zachodzi potrzeba zapewniają konfigurację aplikacji i nadal będzie można przesłonić tę konfigurację za pomocą argumentów wiersza polecenia, należy wywołać dodatkowych dostawców aplikacji <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> i wywołać `AddCommandLine` ostatni.</span><span class="sxs-lookup"><span data-stu-id="c2962-279">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="c2962-280">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-280">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c2962-281">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody.</span><span class="sxs-lookup"><span data-stu-id="c2962-281">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="c2962-282">`AddCommandLine` zostały już wywołane `CreateDefaultBuilder` podczas `UseConfiguration` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c2962-282">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="c2962-283">Jeśli zachodzi potrzeba zapewniają konfigurację aplikacji i nadal będzie można przesłonić tę konfigurację za pomocą argumentów wiersza polecenia, należy wywołać dodatkowych dostawców aplikacji w `ConfigurationBuilder` i wywołać `AddCommandLine` ostatni.</span><span class="sxs-lookup"><span data-stu-id="c2962-283">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="c2962-284">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-284">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-285">Aby uaktywnić konfigurację wiersza polecenia, należy wywołać <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-285">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c2962-286">Wywołanie dostawcy ostatnio, aby umożliwić argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-286">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="c2962-287">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="c2962-287">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="c2962-288">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="c2962-288">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2962-289">2.x Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta, który zawiera wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-289">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-290">Wywołania aplikacji przykładowej 1.x <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> na <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-290">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="c2962-291">Otwórz wiersz polecenia w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="c2962-291">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="c2962-292">Argument wiersza polecenia, aby podać `dotnet run` polecenia `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="c2962-292">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="c2962-293">Po uruchomieniu aplikacji otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c2962-293">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c2962-294">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla argumentu wiersza polecenia konfiguracji udostępnionego `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c2962-294">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="c2962-295">Argumenty</span><span class="sxs-lookup"><span data-stu-id="c2962-295">Arguments</span></span>

<span data-ttu-id="c2962-296">Wartość musi stosować się znak równości (`=`), lub klucz musi mieć prefiks (`--` lub `/`) Jeśli wartość jest zgodna spację.</span><span class="sxs-lookup"><span data-stu-id="c2962-296">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="c2962-297">Wartość może być null, jeśli jest używany znak równości (na przykład `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="c2962-297">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="c2962-298">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="c2962-298">Key prefix</span></span>               | <span data-ttu-id="c2962-299">Przykład</span><span class="sxs-lookup"><span data-stu-id="c2962-299">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="c2962-300">Nie prefiksu</span><span class="sxs-lookup"><span data-stu-id="c2962-300">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="c2962-301">Dwa łączniki (`--`)</span><span class="sxs-lookup"><span data-stu-id="c2962-301">Two dashes (`--`)</span></span>        | <span data-ttu-id="c2962-302">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="c2962-302">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="c2962-303">Kreski ułamkowej (`/`)</span><span class="sxs-lookup"><span data-stu-id="c2962-303">Forward slash (`/`)</span></span>      | <span data-ttu-id="c2962-304">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="c2962-304">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="c2962-305">W tym samym poleceniu nie Mieszaj argument wiersza polecenia pary klucz wartość, które znakiem równości za pomocą pary klucz wartość, korzystających z przestrzeni.</span><span class="sxs-lookup"><span data-stu-id="c2962-305">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="c2962-306">Przykładowe polecenia:</span><span class="sxs-lookup"><span data-stu-id="c2962-306">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="c2962-307">Przełącz mapowania</span><span class="sxs-lookup"><span data-stu-id="c2962-307">Switch mappings</span></span>

<span data-ttu-id="c2962-308">Przełącznik jest transformowanie na nazwę klucza wymiany logiki.</span><span class="sxs-lookup"><span data-stu-id="c2962-308">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="c2962-309">Podczas ręcznego tworzenia konfiguracji za pomocą <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, może zapewnić słownika zastępstw przełącznika <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metody.</span><span class="sxs-lookup"><span data-stu-id="c2962-309">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="c2962-310">W przypadku przełącznika słownik mapowań słownika jest sprawdzane dla klucza, który pasuje do klucza, dostarczone przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c2962-310">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="c2962-311">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) jest przekazywana do zestawu pary klucz wartość do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-311">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="c2962-312">Mapowanie przełącznika jest wymagane dla dowolnego klucz wiersza polecenia poprzedzone kreską pojedynczego (`-`).</span><span class="sxs-lookup"><span data-stu-id="c2962-312">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="c2962-313">Przełącz mapowania słownika kluczowych reguł:</span><span class="sxs-lookup"><span data-stu-id="c2962-313">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="c2962-314">Przełączniki musi rozpoczynać się kreską (`-`) lub podwójne kreska (`--`).</span><span class="sxs-lookup"><span data-stu-id="c2962-314">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="c2962-315">Słownik mapowań przełącznik nie może zawierać zduplikowane klucze.</span><span class="sxs-lookup"><span data-stu-id="c2962-315">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c2962-316">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c2962-316">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c2962-317">Jak pokazano w powyższym przykładzie wywołanie `CreateDefaultBuilder` nie należy przekazywać argumenty, gdy przełącznik mapowania są używane.</span><span class="sxs-lookup"><span data-stu-id="c2962-317">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="c2962-318">`CreateDefaultBuilder` metody `AddCommandLine` wywołania nie zawiera zamapowanego przełączników, a nie istnieje sposób do przekazania słownik mapowania przełącznika `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c2962-318">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c2962-319">Jeśli argumenty zamapowanego przełącznik dołączania i są przekazywane do `CreateDefaultBuilder`, jego `AddCommandLine` dostawcy nie uda się zainicjowanie z <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="c2962-319">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="c2962-320">Rozwiązanie nie jest do przekazywania argumentów do `CreateDefaultBuilder` , ale zamiast tego umożliwiające `ConfigurationBuilder` metody `AddCommandLine` metody do przetwarzania argumentów i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="c2962-320">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="c2962-321">Jak pokazano w powyższym przykładzie wywołanie `CreateDefaultBuilder` nie należy przekazywać argumenty, gdy przełącznik mapowania są używane.</span><span class="sxs-lookup"><span data-stu-id="c2962-321">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="c2962-322">`CreateDefaultBuilder` metody `AddCommandLine` wywołania nie zawiera zamapowanego przełączników, a nie istnieje sposób do przekazania słownik mapowania przełącznika `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c2962-322">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c2962-323">Jeśli argumenty zamapowanego przełącznik dołączania i są przekazywane do `CreateDefaultBuilder`, jego `AddCommandLine` dostawcy nie uda się zainicjowanie z <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="c2962-323">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="c2962-324">Rozwiązanie nie jest do przekazywania argumentów do `CreateDefaultBuilder` , ale zamiast tego umożliwiające `ConfigurationBuilder` metody `AddCommandLine` metody do przetwarzania argumentów i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="c2962-324">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="c2962-325">Po utworzeniu przełącznika słownik mapowań zawiera dane wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c2962-325">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="c2962-326">Key</span><span class="sxs-lookup"><span data-stu-id="c2962-326">Key</span></span>       | <span data-ttu-id="c2962-327">Wartość</span><span class="sxs-lookup"><span data-stu-id="c2962-327">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="c2962-328">Jeśli mapowane przełącznika klucze są używane podczas uruchamiania aplikacji, konfiguracji odbiera wartości konfiguracji na kluczu pochodzącego ze słownika:</span><span class="sxs-lookup"><span data-stu-id="c2962-328">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="c2962-329">Po uruchomieniu polecenia poprzedniej konfiguracji zawiera wartości podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c2962-329">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="c2962-330">Key</span><span class="sxs-lookup"><span data-stu-id="c2962-330">Key</span></span>               | <span data-ttu-id="c2962-331">Wartość</span><span class="sxs-lookup"><span data-stu-id="c2962-331">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="c2962-332">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="c2962-332">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="c2962-333"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Ładuje konfiguracji ze środowiska zmiennej pary klucz wartość w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c2962-333">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="c2962-334">Aby aktywować środowisko zmiennych konfiguracji, należy wywołać <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-334">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c2962-335">Podczas pracy z kluczami hierarchiczne w zmiennych środowiskowych, separator dwukropek (`:`) może nie działać na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="c2962-335">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="c2962-336">Podwójnym podkreśleniem (`__`) jest obsługiwana przez wszystkie platformy i zastępuje dwukropka.</span><span class="sxs-lookup"><span data-stu-id="c2962-336">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="c2962-337">[Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) pozwala na Ustawianie zmiennych środowiskowych w portalu Azure, które mogą zastąpić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="c2962-337">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="c2962-338">Aby uzyskać więcej informacji, zobacz [aplikacje platformy Azure: Zastąpienie konfiguracji aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="c2962-338">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2962-339">`AddEnvironmentVariables` jest wywoływana automatycznie dla zmiennych środowiskowych prefiksem `ASPNETCORE_` podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-339">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="c2962-340">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c2962-340">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="c2962-341">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c2962-341">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c2962-342">Konfiguracja aplikacji ze zmiennych środowiskowych unprefixed przez wywołanie metody `AddEnvironmentVariables` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="c2962-342">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="c2962-343">Konfiguracja opcjonalna z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="c2962-343">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="c2962-344">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="c2962-344">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="c2962-345">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c2962-345">Command-line arguments.</span></span>

<span data-ttu-id="c2962-346">Dostawca konfiguracji zmiennych środowiskowych jest wywoływana po konfiguracji zostanie nawiązane z wpisami tajnymi użytkowników i *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="c2962-346">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="c2962-347">W tym miejscu podczas wywoływania dostawcy umożliwia zmienne środowiskowe są odczytywane w czasie wykonywania do zastępowania konfiguracji ustawione przez użytkownika hasła i *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="c2962-347">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c2962-348">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-348">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="c2962-349">Jeśli konieczne jest zapewnienie konfiguracji aplikacji z dodatkowe zmienne środowiskowe, wywołaj dodatkowych dostawców aplikacji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> i wywołać `AddEnvironmentVariables` z prefiksem.</span><span class="sxs-lookup"><span data-stu-id="c2962-349">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="c2962-350">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-350">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c2962-351">Wywołaj `AddEnvironmentVariables` metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-351">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="c2962-352">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody.</span><span class="sxs-lookup"><span data-stu-id="c2962-352">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="c2962-353">Jeśli konieczne jest zapewnienie konfiguracji aplikacji z dodatkowe zmienne środowiskowe, wywołaj dodatkowych dostawców aplikacji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> i wywołać `AddEnvironmentVariables` z prefiksem.</span><span class="sxs-lookup"><span data-stu-id="c2962-353">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="c2962-354">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-354">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-355">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="c2962-355">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="c2962-356">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="c2962-356">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2962-357">2.x Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta, który zawiera wywołanie `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="c2962-357">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-358">Wywołania aplikacji przykładowej 1.x `AddEnvironmentVariables` na `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c2962-358">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="c2962-359">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="c2962-359">Run the sample app.</span></span> <span data-ttu-id="c2962-360">Otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c2962-360">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c2962-361">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla zmiennej środowiskowej `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="c2962-361">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="c2962-362">Wartość odzwierciedla środowisko, w którym aplikacja jest uruchomiona, zwykle `Development` podczas uruchamiania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="c2962-362">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="c2962-363">Przechowywać listę zmiennych środowiskowych renderowany przez aplikację krótki, aplikacji filtry zmienne środowiskowe do tych rozpoczynających się od następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c2962-363">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="c2962-364">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="c2962-364">ASPNETCORE_</span></span>
* <span data-ttu-id="c2962-365">adresy URL</span><span class="sxs-lookup"><span data-stu-id="c2962-365">urls</span></span>
* <span data-ttu-id="c2962-366">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="c2962-366">Logging</span></span>
* <span data-ttu-id="c2962-367">ŚRODOWISKO</span><span class="sxs-lookup"><span data-stu-id="c2962-367">ENVIRONMENT</span></span>
* <span data-ttu-id="c2962-368">contentRoot</span><span class="sxs-lookup"><span data-stu-id="c2962-368">contentRoot</span></span>
* <span data-ttu-id="c2962-369">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="c2962-369">AllowedHosts</span></span>
* <span data-ttu-id="c2962-370">applicationName</span><span class="sxs-lookup"><span data-stu-id="c2962-370">applicationName</span></span>
* <span data-ttu-id="c2962-371">Wiersz polecenia</span><span class="sxs-lookup"><span data-stu-id="c2962-371">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2962-372">Jeśli chcesz udostępnić wszystkie zmienne środowiskowe, dostępne dla aplikacji, zmień `FilteredConfiguration` w *Pages/Index.cshtml.cs* do następującego:</span><span class="sxs-lookup"><span data-stu-id="c2962-372">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-373">Jeśli chcesz udostępnić wszystkie zmienne środowiskowe, dostępne dla aplikacji, zmień `FilteredConfiguration` w *Controllers/HomeController.cs* do następującego:</span><span class="sxs-lookup"><span data-stu-id="c2962-373">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="c2962-374">Prefiksy</span><span class="sxs-lookup"><span data-stu-id="c2962-374">Prefixes</span></span>

<span data-ttu-id="c2962-375">Zmienne środowiskowe ładowane z konfiguracji aplikacji są filtrowane podczas podawania prefiks `AddEnvironmentVariables` metody.</span><span class="sxs-lookup"><span data-stu-id="c2962-375">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="c2962-376">Na przykład, aby filtr zmiennych środowiskowych na prefiksie `CUSTOM_`, podaj prefiks, który dostawca konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c2962-376">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="c2962-377">Prefiks jest odłączany podczas tworzenia pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-377">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2962-378">Statyczne wygodna metoda `CreateDefaultBuilder` tworzy <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> Aby ustanowić hosta aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-378">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="c2962-379">Gdy `WebHostBuilder` jest tworzony, znajdzie jego konfigurację hosta w zmiennych środowiskowych z prefiksem `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="c2962-379">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="c2962-380">**Prefiksy ciąg połączenia**</span><span class="sxs-lookup"><span data-stu-id="c2962-380">**Connection string prefixes**</span></span>

<span data-ttu-id="c2962-381">Interfejs API konfiguracji zawiera reguły jest przetwarzana w specjalny cztery połączenia ciągu zmiennych środowiskowych zajmujących się Konfigurowanie parametrów połączenia platformy Azure dla środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-381">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="c2962-382">Zmienne środowiskowe z prefiksami pokazano w tabeli są ładowane do aplikacji, jeśli nie dostarczono żadnego prefiksu do `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="c2962-382">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="c2962-383">Prefiks ciągu połączenia</span><span class="sxs-lookup"><span data-stu-id="c2962-383">Connection string prefix</span></span> | <span data-ttu-id="c2962-384">Dostawca</span><span class="sxs-lookup"><span data-stu-id="c2962-384">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="c2962-385">Dostawcy niestandardowego</span><span class="sxs-lookup"><span data-stu-id="c2962-385">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="c2962-386">MySQL</span><span class="sxs-lookup"><span data-stu-id="c2962-386">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="c2962-387">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c2962-387">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="c2962-388">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c2962-388">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="c2962-389">Gdy zmienna środowiskowa są odnajdywane i ładowane do konfiguracji za pomocą dowolnego z czterech prefiksy z tabelą:</span><span class="sxs-lookup"><span data-stu-id="c2962-389">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="c2962-390">Klucz konfiguracji jest tworzony, usuwając prefiks zmiennej środowiska i dodając kluczowych sekcję konfiguracji (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="c2962-390">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="c2962-391">Tworzony jest nową parę klucz wartość konfiguracji, który reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, który nie ma określonego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="c2962-391">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="c2962-392">Klucz zmiennych środowiska</span><span class="sxs-lookup"><span data-stu-id="c2962-392">Environment variable key</span></span> | <span data-ttu-id="c2962-393">Klucz konfiguracji przekonwertowany</span><span class="sxs-lookup"><span data-stu-id="c2962-393">Converted configuration key</span></span> | <span data-ttu-id="c2962-394">Pozycja konfiguracji dostawcy</span><span class="sxs-lookup"><span data-stu-id="c2962-394">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c2962-395">Wpis konfiguracyjny nie utworzony.</span><span class="sxs-lookup"><span data-stu-id="c2962-395">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c2962-396">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="c2962-396">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="c2962-397">Wartość:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="c2962-397">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c2962-398">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="c2962-398">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="c2962-399">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="c2962-399">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c2962-400">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="c2962-400">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="c2962-401">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="c2962-401">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="c2962-402">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="c2962-402">File Configuration Provider</span></span>

<span data-ttu-id="c2962-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową dla ładowania konfiguracji z systemu plików.</span><span class="sxs-lookup"><span data-stu-id="c2962-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="c2962-404">Następujących dostawców konfiguracji są przeznaczone dla określonych typów plików:</span><span class="sxs-lookup"><span data-stu-id="c2962-404">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="c2962-405">Dostawca konfiguracji INI</span><span class="sxs-lookup"><span data-stu-id="c2962-405">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="c2962-406">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="c2962-406">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="c2962-407">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="c2962-407">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="c2962-408">Dostawca konfiguracji INI</span><span class="sxs-lookup"><span data-stu-id="c2962-408">INI Configuration Provider</span></span>

<span data-ttu-id="c2962-409"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Ładuje konfiguracji z pary klucz wartość pliku INI w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c2962-409">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="c2962-410">Aby aktywować konfiguracji pliku INI, należy wywołać <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-410">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c2962-411">Dwukropek może służyć do jako ogranicznik sekcji w konfiguracji pliku INI.</span><span class="sxs-lookup"><span data-stu-id="c2962-411">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="c2962-412">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c2962-412">Overloads permit specifying:</span></span>

* <span data-ttu-id="c2962-413">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c2962-413">Whether the file is optional.</span></span>
* <span data-ttu-id="c2962-414">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="c2962-414">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c2962-415"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="c2962-415">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c2962-416">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c2962-416">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c2962-417">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-417">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c2962-418">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-418">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-419">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-419">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c2962-420">Podczas wywoływania `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-420">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c2962-421">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-421">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c2962-422">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-422">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-423">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-423">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-424">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="c2962-424">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="c2962-425">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-425">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c2962-426">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-426">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-427">Przykładowy plik konfiguracji INI:</span><span class="sxs-lookup"><span data-stu-id="c2962-427">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="c2962-428">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c2962-428">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c2962-429">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c2962-429">section0:key0</span></span>
* <span data-ttu-id="c2962-430">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c2962-430">section0:key1</span></span>
* <span data-ttu-id="c2962-431">section1:Subsection:key</span><span class="sxs-lookup"><span data-stu-id="c2962-431">section1:subsection:key</span></span>
* <span data-ttu-id="c2962-432">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="c2962-432">section2:subsection0:key</span></span>
* <span data-ttu-id="c2962-433">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="c2962-433">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="c2962-434">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="c2962-434">JSON Configuration Provider</span></span>

<span data-ttu-id="c2962-435"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Ładuje konfiguracji z pary klucz wartość plik JSON podczas wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c2962-435">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="c2962-436">Aby aktywować konfiguracji pliku JSON, należy wywołać <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-436">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c2962-437">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c2962-437">Overloads permit specifying:</span></span>

* <span data-ttu-id="c2962-438">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c2962-438">Whether the file is optional.</span></span>
* <span data-ttu-id="c2962-439">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="c2962-439">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c2962-440"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="c2962-440">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2962-441">`AddJsonFile` jest wywoływana automatycznie, dwa razy, podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-441">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="c2962-442">Metoda jest wywoływana, aby załadować konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c2962-442">The method is called to load configuration from:</span></span>

* <span data-ttu-id="c2962-443">*appSettings.JSON* &ndash; ten plik jest najpierw do odczytu.</span><span class="sxs-lookup"><span data-stu-id="c2962-443">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="c2962-444">Wersja środowiska pliku można zastąpić wartości dostarczone przez *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="c2962-444">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="c2962-445">*appSettings. {Środowiska} .json* &ndash; środowiska wersję pliku zostanie załadowany na podstawie [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="c2962-445">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="c2962-446">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c2962-446">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="c2962-447">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c2962-447">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c2962-448">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="c2962-448">Environment variables.</span></span>
* <span data-ttu-id="c2962-449">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="c2962-449">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="c2962-450">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c2962-450">Command-line arguments.</span></span>

<span data-ttu-id="c2962-451">Dostawca konfiguracji JSON jest ustanawiane, najpierw.</span><span class="sxs-lookup"><span data-stu-id="c2962-451">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="c2962-452">W związku z tym, wpisami tajnymi użytkowników, zmienne środowiskowe i argumenty wiersza polecenia zastępuje konfiguracji ustawione przez *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="c2962-452">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c2962-453">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji dla plików innych niż *appsettings.json* i *appsettings. { Środowisko} .json*:</span><span class="sxs-lookup"><span data-stu-id="c2962-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="c2962-454">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c2962-455">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-456">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c2962-457">Możesz również bezpośrednio wywołać `AddJsonFile` metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-457">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c2962-458">Podczas wywoływania `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-458">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c2962-459">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-459">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c2962-460">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-460">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-461">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-461">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-462">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="c2962-462">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="c2962-463">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-463">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c2962-464">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-464">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-465">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="c2962-465">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2962-466">2.x Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do hosta, który obejmuje dwa wywołania do tworzenia `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="c2962-466">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="c2962-467">Konfiguracja została załadowana z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="c2962-467">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-468">Wywołania aplikacji przykładowej 1.x `AddJsonFile` dwa razy na `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c2962-468">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="c2962-469">Konfiguracja została załadowana z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="c2962-469">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="c2962-470">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="c2962-470">Run the sample app.</span></span> <span data-ttu-id="c2962-471">Otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c2962-471">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c2962-472">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla konfiguracji, jak pokazano w tabeli, w zależności od środowiska.</span><span class="sxs-lookup"><span data-stu-id="c2962-472">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="c2962-473">Klucze konfiguracji rejestrowania, użyj dwukropka (`:`) jako separator hierarchicznej.</span><span class="sxs-lookup"><span data-stu-id="c2962-473">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="c2962-474">Key</span><span class="sxs-lookup"><span data-stu-id="c2962-474">Key</span></span>                        | <span data-ttu-id="c2962-475">Wartość rozwoju</span><span class="sxs-lookup"><span data-stu-id="c2962-475">Development Value</span></span> | <span data-ttu-id="c2962-476">Wartość produkcji</span><span class="sxs-lookup"><span data-stu-id="c2962-476">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="c2962-477">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="c2962-477">Logging:LogLevel:System</span></span>    | <span data-ttu-id="c2962-478">Informacje</span><span class="sxs-lookup"><span data-stu-id="c2962-478">Information</span></span>       | <span data-ttu-id="c2962-479">Informacje</span><span class="sxs-lookup"><span data-stu-id="c2962-479">Information</span></span>      |
| <span data-ttu-id="c2962-480">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="c2962-480">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="c2962-481">Informacje</span><span class="sxs-lookup"><span data-stu-id="c2962-481">Information</span></span>       | <span data-ttu-id="c2962-482">Informacje</span><span class="sxs-lookup"><span data-stu-id="c2962-482">Information</span></span>      |
| <span data-ttu-id="c2962-483">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="c2962-483">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="c2962-484">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="c2962-484">Debug</span></span>             | <span data-ttu-id="c2962-485">Błąd</span><span class="sxs-lookup"><span data-stu-id="c2962-485">Error</span></span>            |
| <span data-ttu-id="c2962-486">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="c2962-486">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="c2962-487">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="c2962-487">XML Configuration Provider</span></span>

<span data-ttu-id="c2962-488"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Ładuje konfiguracji z pary klucz wartość plik XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c2962-488">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="c2962-489">Aby aktywować Konfiguracja pliku XML, należy wywołać <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-489">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c2962-490">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c2962-490">Overloads permit specifying:</span></span>

* <span data-ttu-id="c2962-491">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c2962-491">Whether the file is optional.</span></span>
* <span data-ttu-id="c2962-492">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="c2962-492">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c2962-493"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="c2962-493">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="c2962-494">Węzeł główny plik konfiguracji jest ignorowany podczas tworzenia pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-494">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="c2962-495">Określać definicji typu dokumentu (DTD) lub przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="c2962-495">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c2962-496">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c2962-496">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c2962-497">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-497">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c2962-498">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-498">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-499">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-499">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c2962-500">Podczas wywoływania `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-500">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c2962-501">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-501">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c2962-502">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-502">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-503">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-503">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-504">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="c2962-504">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="c2962-505">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-505">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c2962-506">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-506">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-507">Pliki konfiguracji XML można użyć nazw unikatowych elementów dla powtarzających się sekcje:</span><span class="sxs-lookup"><span data-stu-id="c2962-507">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="c2962-508">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c2962-508">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c2962-509">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c2962-509">section0:key0</span></span>
* <span data-ttu-id="c2962-510">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c2962-510">section0:key1</span></span>
* <span data-ttu-id="c2962-511">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c2962-511">section1:key0</span></span>
* <span data-ttu-id="c2962-512">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c2962-512">section1:key1</span></span>

<span data-ttu-id="c2962-513">Powtarzające się elementy, które używają takiej samej nazwie elementu pracy, jeśli `name` atrybut jest używany do odróżniania elementów:</span><span class="sxs-lookup"><span data-stu-id="c2962-513">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="c2962-514">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c2962-514">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c2962-515">sekcja: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="c2962-515">section:section0:key:key0</span></span>
* <span data-ttu-id="c2962-516">sekcja: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="c2962-516">section:section0:key:key1</span></span>
* <span data-ttu-id="c2962-517">sekcja: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="c2962-517">section:section1:key:key0</span></span>
* <span data-ttu-id="c2962-518">sekcja: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="c2962-518">section:section1:key:key1</span></span>

<span data-ttu-id="c2962-519">Atrybuty można podać wartości:</span><span class="sxs-lookup"><span data-stu-id="c2962-519">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="c2962-520">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c2962-520">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c2962-521">klucz: atrybut</span><span class="sxs-lookup"><span data-stu-id="c2962-521">key:attribute</span></span>
* <span data-ttu-id="c2962-522">sekcja: klucz: atrybut</span><span class="sxs-lookup"><span data-stu-id="c2962-522">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="c2962-523">Dostawca klucza każdego pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c2962-523">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="c2962-524"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Korzysta z plików w katalogu jako pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-524">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="c2962-525">Klucz jest nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="c2962-525">The key is the file name.</span></span> <span data-ttu-id="c2962-526">Wartość zawiera zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="c2962-526">The value contains the file's contents.</span></span> <span data-ttu-id="c2962-527">Dostawca konfiguracji klucza każdego pliku jest używany w scenariuszach hostingu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="c2962-527">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="c2962-528">Aby aktywować klucza dla pliku konfiguracji, należy wywołać <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-528">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="c2962-529">`directoryPath` Do plików musi być ścieżką bezwzględną.</span><span class="sxs-lookup"><span data-stu-id="c2962-529">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="c2962-530">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c2962-530">Overloads permit specifying:</span></span>

* <span data-ttu-id="c2962-531">`Action<KeyPerFileConfigurationSource>` Delegat, który konfiguruje źródło.</span><span class="sxs-lookup"><span data-stu-id="c2962-531">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="c2962-532">Czy katalog jest opcjonalna i ścieżkę do katalogu.</span><span class="sxs-lookup"><span data-stu-id="c2962-532">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="c2962-533">Podwójne podkreślenie (`__`) jest używany jako ogranicznika klucza konfiguracji w nazwach plików.</span><span class="sxs-lookup"><span data-stu-id="c2962-533">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="c2962-534">Na przykład, nazwa_pliku `Logging__LogLevel__System` generuje klucz konfiguracji `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="c2962-534">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="c2962-535">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c2962-535">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c2962-536">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c2962-536">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c2962-537">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-537">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-538">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-538">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="c2962-539">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="c2962-539">Memory Configuration Provider</span></span>

<span data-ttu-id="c2962-540"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Korzysta z kolekcji w pamięci jako pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-540">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="c2962-541">Aby aktywować konfiguracji kolekcji w pamięci, należy wywołać <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c2962-541">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c2962-542">Dostawca konfiguracji mogą być zainicjowane z `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="c2962-542">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c2962-543">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c2962-543">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c2962-544">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-544">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c2962-545">Podczas wywoływania `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-545">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c2962-546">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c2962-546">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-547">Zastosuj konfigurację do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="c2962-547">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="c2962-548">GetValue</span><span class="sxs-lookup"><span data-stu-id="c2962-548">GetValue</span></span>

<span data-ttu-id="c2962-549">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnianie wartości z konfiguracji z określonym kluczem i konwertuje je do określonego typu.</span><span class="sxs-lookup"><span data-stu-id="c2962-549">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="c2962-550">Przeciążenie umożliwia podanie wartości domyślnej, jeśli nie odnaleziono klucza.</span><span class="sxs-lookup"><span data-stu-id="c2962-550">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="c2962-551">Poniższy przykład wyodrębnia wartość ciągu z konfiguracji przy użyciu klucza `NumberKey`, typy wartości jako `int`i przechowuje wartość w zmiennej `intValue`.</span><span class="sxs-lookup"><span data-stu-id="c2962-551">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="c2962-552">Jeśli `NumberKey` nie zostanie odnaleziona w klucze konfiguracji `intValue` odbiera wartość domyślną `99`:</span><span class="sxs-lookup"><span data-stu-id="c2962-552">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="c2962-553">GetSection, GetChildren i istnieje</span><span class="sxs-lookup"><span data-stu-id="c2962-553">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="c2962-554">Aby uzyskać przykłady, które należy wykonać należy wziąć pod uwagę następujący plik JSON.</span><span class="sxs-lookup"><span data-stu-id="c2962-554">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="c2962-555">Cztery klucze znajdują się na dwie sekcje, z których jedna zawiera parę podsekcje:</span><span class="sxs-lookup"><span data-stu-id="c2962-555">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="c2962-556">Gdy plik jest do odczytu do konfiguracji, następujące klucze unikatowe hierarchiczne są tworzone do przechowywania wartości konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c2962-556">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="c2962-557">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c2962-557">section0:key0</span></span>
* <span data-ttu-id="c2962-558">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c2962-558">section0:key1</span></span>
* <span data-ttu-id="c2962-559">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c2962-559">section1:key0</span></span>
* <span data-ttu-id="c2962-560">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c2962-560">section1:key1</span></span>
* <span data-ttu-id="c2962-561">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="c2962-561">section2:subsection0:key0</span></span>
* <span data-ttu-id="c2962-562">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="c2962-562">section2:subsection0:key1</span></span>
* <span data-ttu-id="c2962-563">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="c2962-563">section2:subsection1:key0</span></span>
* <span data-ttu-id="c2962-564">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="c2962-564">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="c2962-565">GetSection</span><span class="sxs-lookup"><span data-stu-id="c2962-565">GetSection</span></span>

<span data-ttu-id="c2962-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcji konfiguracji z kluczem określonym podsekcji.</span><span class="sxs-lookup"><span data-stu-id="c2962-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="c2962-567">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-567">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-568">Aby zwrócić <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz wartość w `section1`, wywołaj `GetSection` i podaj nazwę sekcji:</span><span class="sxs-lookup"><span data-stu-id="c2962-568">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="c2962-569">`configSection` Nie ma wartości, tylko klucz i ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="c2962-569">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="c2962-570">Podobnie Aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę do sekcji:</span><span class="sxs-lookup"><span data-stu-id="c2962-570">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="c2962-571">`GetSection` nigdy nie zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="c2962-571">`GetSection` never returns `null`.</span></span> <span data-ttu-id="c2962-572">Jeśli nie zostanie znaleziona pasująca sekcja, pusta `IConfigurationSection` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="c2962-572">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="c2962-573">Gdy `GetSection` zwraca pasujących sekcji <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione.</span><span class="sxs-lookup"><span data-stu-id="c2962-573">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="c2962-574">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> i <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> są zwracane, gdy istnieje sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2962-574">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="c2962-575">GetChildren</span><span class="sxs-lookup"><span data-stu-id="c2962-575">GetChildren</span></span>

<span data-ttu-id="c2962-576">Wywołanie [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) na `section2` uzyskuje `IEnumerable<IConfigurationSection>` zawierającej:</span><span class="sxs-lookup"><span data-stu-id="c2962-576">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="c2962-577">Exists</span><span class="sxs-lookup"><span data-stu-id="c2962-577">Exists</span></span>

<span data-ttu-id="c2962-578">Użyj [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) do ustalenia, czy istnieje sekcji konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c2962-578">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="c2962-579">Podane dane przykładowe `sectionExists` jest `false` , ponieważ nie ma `section2:subsection2` sekcji w danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-579">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="c2962-580">Powiąż z klasy</span><span class="sxs-lookup"><span data-stu-id="c2962-580">Bind to a class</span></span>

<span data-ttu-id="c2962-581">Konfiguracja może być powiązana z klas, które reprezentują grupy powiązane ustawienia za pomocą *wzorzec opcje*.</span><span class="sxs-lookup"><span data-stu-id="c2962-581">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="c2962-582">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="c2962-582">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="c2962-583">Wartości konfiguracji są zwracane jako ciągi, ale wywoływania <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) obiektów.</span><span class="sxs-lookup"><span data-stu-id="c2962-583">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="c2962-584">`Bind` Trwa [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-584">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-585">Przykładowa aplikacja zawiera `Starship` modelu (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="c2962-585">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c2962-586">`starship` Części *starship.json* plików umożliwia utworzenie konfiguracji podczas Przykładowa aplikacja używa dostawcy konfiguracji JSON można załadować konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c2962-586">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="c2962-587">Tworzone są następujące pary klucz wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c2962-587">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="c2962-588">Key</span><span class="sxs-lookup"><span data-stu-id="c2962-588">Key</span></span>                   | <span data-ttu-id="c2962-589">Wartość</span><span class="sxs-lookup"><span data-stu-id="c2962-589">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="c2962-590">starship: Nazwa</span><span class="sxs-lookup"><span data-stu-id="c2962-590">starship:name</span></span>         | <span data-ttu-id="c2962-591">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="c2962-591">USS Enterprise</span></span>                                    |
| <span data-ttu-id="c2962-592">starship: rejestru</span><span class="sxs-lookup"><span data-stu-id="c2962-592">starship:registry</span></span>     | <span data-ttu-id="c2962-593">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="c2962-593">NCC-1701</span></span>                                          |
| <span data-ttu-id="c2962-594">starship: klasy</span><span class="sxs-lookup"><span data-stu-id="c2962-594">starship:class</span></span>        | <span data-ttu-id="c2962-595">Tworzenia</span><span class="sxs-lookup"><span data-stu-id="c2962-595">Constitution</span></span>                                      |
| <span data-ttu-id="c2962-596">starship: długość</span><span class="sxs-lookup"><span data-stu-id="c2962-596">starship:length</span></span>       | <span data-ttu-id="c2962-597">304.8</span><span class="sxs-lookup"><span data-stu-id="c2962-597">304.8</span></span>                                             |
| <span data-ttu-id="c2962-598">starship: upoważnione</span><span class="sxs-lookup"><span data-stu-id="c2962-598">starship:commissioned</span></span> | <span data-ttu-id="c2962-599">False</span><span class="sxs-lookup"><span data-stu-id="c2962-599">False</span></span>                                             |
| <span data-ttu-id="c2962-600">Znak towarowy</span><span class="sxs-lookup"><span data-stu-id="c2962-600">trademark</span></span>             | <span data-ttu-id="c2962-601">Paramount obrazy Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="c2962-601">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="c2962-602">Wywołania aplikacji przykładowej `GetSection` z `starship` klucza.</span><span class="sxs-lookup"><span data-stu-id="c2962-602">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="c2962-603">`starship` Pary klucz wartość są izolowane.</span><span class="sxs-lookup"><span data-stu-id="c2962-603">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="c2962-604">`Bind` Metoda jest wywoływana w podsekcji, przekazując wystąpienie `Starship` klasy.</span><span class="sxs-lookup"><span data-stu-id="c2962-604">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="c2962-605">Po powiązaniu wartości wystąpienia, wystąpienie jest przypisany do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="c2962-605">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

<span data-ttu-id="c2962-606">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-606">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="c2962-607">Powiąż z wykresu obiektu</span><span class="sxs-lookup"><span data-stu-id="c2962-607">Bind to an object graph</span></span>

<span data-ttu-id="c2962-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> jest w stanie całego grafu obiektów POCO powiązania.</span><span class="sxs-lookup"><span data-stu-id="c2962-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="c2962-609">`Bind` Trwa [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-609">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c2962-610">Przykładowy zawiera `TvShow` model zawiera wykres obiektu, którego `Metadata` i `Actors` klasy (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="c2962-610">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c2962-611">Przykładowa aplikacja ma *tvshow.xml* plik zawierający dane konfiguracyjne:</span><span class="sxs-lookup"><span data-stu-id="c2962-611">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="c2962-612">Konfiguracja jest powiązany z całej `TvShow` wykres obiektu z `Bind` metody.</span><span class="sxs-lookup"><span data-stu-id="c2962-612">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="c2962-613">Powiązane wystąpienia jest przypisywany do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="c2962-613">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="c2962-614">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) wiąże i zwraca wartość określonego typu.</span><span class="sxs-lookup"><span data-stu-id="c2962-614">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="c2962-615">`Get<T>` jest bardziej wygodne niż przy użyciu `Bind`.</span><span class="sxs-lookup"><span data-stu-id="c2962-615">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="c2962-616">Poniższy kod przedstawia sposób użycia `Get<T>` w poprzednim przykładzie, co umożliwia wystąpienie powiązanych, którego można przypisać bezpośrednio do właściwości, używany do renderowania:</span><span class="sxs-lookup"><span data-stu-id="c2962-616">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

<span data-ttu-id="c2962-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> Trwa [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="c2962-618">`Get<T>` jest dostępna w programie ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="c2962-618">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="c2962-619">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-619">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="c2962-620">Powiąż tablicę do klasy</span><span class="sxs-lookup"><span data-stu-id="c2962-620">Bind an array to a class</span></span>

<span data-ttu-id="c2962-621">*Przykładowa aplikacja pokazuje pojęcia opisane w tej sekcji.*</span><span class="sxs-lookup"><span data-stu-id="c2962-621">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="c2962-622"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Obsługuje tablice powiązania do obiektów przy użyciu tablicy indeksów w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-622">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="c2962-623">Dowolnym formacie tablicy, który udostępnia liczbowych segment klucza (`:0:`, `:1:`, &hellip; `:{n}:`) jest w stanie tablicy powiązania z macierzą POCO klasy.</span><span class="sxs-lookup"><span data-stu-id="c2962-623">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="c2962-624">"Związać" znajduje się w [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-624">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="c2962-625">Powiązanie jest zapewniana przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="c2962-625">Binding is provided by convention.</span></span> <span data-ttu-id="c2962-626">Konfiguracja niestandardowa dostawców nie są wymagane do zaimplementowania tablicy powiązania.</span><span class="sxs-lookup"><span data-stu-id="c2962-626">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="c2962-627">**Przetwarzanie w pamięci tablica**</span><span class="sxs-lookup"><span data-stu-id="c2962-627">**In-memory array processing**</span></span>

<span data-ttu-id="c2962-628">Należy wziąć pod uwagę kluczy i wartości konfiguracji pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c2962-628">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="c2962-629">Key</span><span class="sxs-lookup"><span data-stu-id="c2962-629">Key</span></span>             | <span data-ttu-id="c2962-630">Wartość</span><span class="sxs-lookup"><span data-stu-id="c2962-630">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="c2962-631">Macierz: wpisów: 0</span><span class="sxs-lookup"><span data-stu-id="c2962-631">array:entries:0</span></span> | <span data-ttu-id="c2962-632">value0</span><span class="sxs-lookup"><span data-stu-id="c2962-632">value0</span></span> |
| <span data-ttu-id="c2962-633">Macierz: wpisów: 1</span><span class="sxs-lookup"><span data-stu-id="c2962-633">array:entries:1</span></span> | <span data-ttu-id="c2962-634">value1</span><span class="sxs-lookup"><span data-stu-id="c2962-634">value1</span></span> |
| <span data-ttu-id="c2962-635">Macierz: wpisów: 2</span><span class="sxs-lookup"><span data-stu-id="c2962-635">array:entries:2</span></span> | <span data-ttu-id="c2962-636">value2</span><span class="sxs-lookup"><span data-stu-id="c2962-636">value2</span></span> |
| <span data-ttu-id="c2962-637">Macierz: wpisów: 4</span><span class="sxs-lookup"><span data-stu-id="c2962-637">array:entries:4</span></span> | <span data-ttu-id="c2962-638">value4</span><span class="sxs-lookup"><span data-stu-id="c2962-638">value4</span></span> |
| <span data-ttu-id="c2962-639">Macierz: wpisów: 5</span><span class="sxs-lookup"><span data-stu-id="c2962-639">array:entries:5</span></span> | <span data-ttu-id="c2962-640">value5</span><span class="sxs-lookup"><span data-stu-id="c2962-640">value5</span></span> |

<span data-ttu-id="c2962-641">Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:</span><span class="sxs-lookup"><span data-stu-id="c2962-641">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="c2962-642">Tablica pomija wartość indeksu &num;3.</span><span class="sxs-lookup"><span data-stu-id="c2962-642">The array skips a value for index &num;3.</span></span> <span data-ttu-id="c2962-643">Konfiguracja obiektu wiążącego nie jest zdolny do powiązania wartości null lub tworzenie wartości null wpisów w powiązanych obiektów, które stają się w chwili, gdy przedstawiono wynik powiązania tej tablicy do obiektu.</span><span class="sxs-lookup"><span data-stu-id="c2962-643">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="c2962-644">W przykładowej aplikacji do przechowywania danych konfiguracji powiązanej dostępnej klasy POCO:</span><span class="sxs-lookup"><span data-stu-id="c2962-644">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c2962-645">Dane konfiguracji jest powiązana z obiektem:</span><span class="sxs-lookup"><span data-stu-id="c2962-645">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="c2962-646">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2962-646">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="c2962-647">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) składni można również użyć, która skutkuje bardziej kompaktowy kod:</span><span class="sxs-lookup"><span data-stu-id="c2962-647">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="c2962-648">Powiązany obiekt, wystąpienie `ArrayExample`, odbiera dane tablicy z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-648">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="c2962-649">`ArrayExample.Entries` Indeks</span><span class="sxs-lookup"><span data-stu-id="c2962-649">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="c2962-650">`ArrayExample.Entries` Wartość</span><span class="sxs-lookup"><span data-stu-id="c2962-650">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="c2962-651">0</span><span class="sxs-lookup"><span data-stu-id="c2962-651">0</span></span>                            | <span data-ttu-id="c2962-652">value0</span><span class="sxs-lookup"><span data-stu-id="c2962-652">value0</span></span>                       |
| <span data-ttu-id="c2962-653">1</span><span class="sxs-lookup"><span data-stu-id="c2962-653">1</span></span>                            | <span data-ttu-id="c2962-654">value1</span><span class="sxs-lookup"><span data-stu-id="c2962-654">value1</span></span>                       |
| <span data-ttu-id="c2962-655">2</span><span class="sxs-lookup"><span data-stu-id="c2962-655">2</span></span>                            | <span data-ttu-id="c2962-656">value2</span><span class="sxs-lookup"><span data-stu-id="c2962-656">value2</span></span>                       |
| <span data-ttu-id="c2962-657">3</span><span class="sxs-lookup"><span data-stu-id="c2962-657">3</span></span>                            | <span data-ttu-id="c2962-658">value4</span><span class="sxs-lookup"><span data-stu-id="c2962-658">value4</span></span>                       |
| <span data-ttu-id="c2962-659">4</span><span class="sxs-lookup"><span data-stu-id="c2962-659">4</span></span>                            | <span data-ttu-id="c2962-660">value5</span><span class="sxs-lookup"><span data-stu-id="c2962-660">value5</span></span>                       |

<span data-ttu-id="c2962-661">Indeks &num;3 w powiązany obiekt przechowuje dane konfiguracyjne `array:4` klucz konfiguracji i jego wartość `value4`.</span><span class="sxs-lookup"><span data-stu-id="c2962-661">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="c2962-662">Gdy dane konfiguracji, które zawiera tablicę jest powiązana, indeksy tablicy w klucze konfiguracji jedynie służą do iteracji dane konfiguracji, podczas tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="c2962-662">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="c2962-663">Wartość null nie mogą być przechowywane w danych konfiguracji, a wpis o wartości null nie jest tworzona w powiązany obiekt, w przypadku tablicy w konfiguracji kluczy Pomiń jeden lub więcej indeksów.</span><span class="sxs-lookup"><span data-stu-id="c2962-663">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="c2962-664">Brak elementu konfiguracji dla indeksu &num;3 mogą być dostarczane przed powiązanie `ArrayExample` wystąpienie przez dowolnego dostawcę konfiguracji, tworzącego prawidłowe pary klucz wartość w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-664">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="c2962-665">Jeśli przykład uwzględniony dodatkowego dostawcę konfiguracji JSON z Brak pary klucz wartość `ArrayExample.Entries` pasuje do tablicy kompletna konfiguracja:</span><span class="sxs-lookup"><span data-stu-id="c2962-665">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="c2962-666">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="c2962-666">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2962-667">W <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="c2962-667">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2962-668">W `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="c2962-668">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="c2962-669">Pary klucz wartość, jak pokazano w tabeli są ładowane do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-669">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="c2962-670">Key</span><span class="sxs-lookup"><span data-stu-id="c2962-670">Key</span></span>             | <span data-ttu-id="c2962-671">Wartość</span><span class="sxs-lookup"><span data-stu-id="c2962-671">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="c2962-672">Macierz: wpisów: 3</span><span class="sxs-lookup"><span data-stu-id="c2962-672">array:entries:3</span></span> | <span data-ttu-id="c2962-673">value3</span><span class="sxs-lookup"><span data-stu-id="c2962-673">value3</span></span> |

<span data-ttu-id="c2962-674">Jeśli `ArrayExample` wystąpienia klasy jest powiązana po dostawcę konfiguracji JSON zawiera wpis dla indeksu &num;3, `ArrayExample.Entries` tablicy zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="c2962-674">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="c2962-675">`ArrayExample.Entries` Indeks</span><span class="sxs-lookup"><span data-stu-id="c2962-675">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="c2962-676">`ArrayExample.Entries` Wartość</span><span class="sxs-lookup"><span data-stu-id="c2962-676">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="c2962-677">0</span><span class="sxs-lookup"><span data-stu-id="c2962-677">0</span></span>                            | <span data-ttu-id="c2962-678">value0</span><span class="sxs-lookup"><span data-stu-id="c2962-678">value0</span></span>                       |
| <span data-ttu-id="c2962-679">1</span><span class="sxs-lookup"><span data-stu-id="c2962-679">1</span></span>                            | <span data-ttu-id="c2962-680">value1</span><span class="sxs-lookup"><span data-stu-id="c2962-680">value1</span></span>                       |
| <span data-ttu-id="c2962-681">2</span><span class="sxs-lookup"><span data-stu-id="c2962-681">2</span></span>                            | <span data-ttu-id="c2962-682">value2</span><span class="sxs-lookup"><span data-stu-id="c2962-682">value2</span></span>                       |
| <span data-ttu-id="c2962-683">3</span><span class="sxs-lookup"><span data-stu-id="c2962-683">3</span></span>                            | <span data-ttu-id="c2962-684">value3</span><span class="sxs-lookup"><span data-stu-id="c2962-684">value3</span></span>                       |
| <span data-ttu-id="c2962-685">4</span><span class="sxs-lookup"><span data-stu-id="c2962-685">4</span></span>                            | <span data-ttu-id="c2962-686">value4</span><span class="sxs-lookup"><span data-stu-id="c2962-686">value4</span></span>                       |
| <span data-ttu-id="c2962-687">5</span><span class="sxs-lookup"><span data-stu-id="c2962-687">5</span></span>                            | <span data-ttu-id="c2962-688">value5</span><span class="sxs-lookup"><span data-stu-id="c2962-688">value5</span></span>                       |

<span data-ttu-id="c2962-689">**Przetwarzanie tablicy JSON**</span><span class="sxs-lookup"><span data-stu-id="c2962-689">**JSON array processing**</span></span>

<span data-ttu-id="c2962-690">Jeśli plik JSON zawiera tablicę, klucze konfiguracji zostaną utworzone dla elementów tablicy przy użyciu indeksu zaczynającego się od zera sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2962-690">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="c2962-691">W następującym pliku konfiguracji `subsection` jest tablicą:</span><span class="sxs-lookup"><span data-stu-id="c2962-691">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="c2962-692">Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz wartość:</span><span class="sxs-lookup"><span data-stu-id="c2962-692">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="c2962-693">Key</span><span class="sxs-lookup"><span data-stu-id="c2962-693">Key</span></span>                     | <span data-ttu-id="c2962-694">Wartość</span><span class="sxs-lookup"><span data-stu-id="c2962-694">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="c2962-695">json_array:key</span><span class="sxs-lookup"><span data-stu-id="c2962-695">json_array:key</span></span>          | <span data-ttu-id="c2962-696">valueA</span><span class="sxs-lookup"><span data-stu-id="c2962-696">valueA</span></span> |
| <span data-ttu-id="c2962-697">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="c2962-697">json_array:subsection:0</span></span> | <span data-ttu-id="c2962-698">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="c2962-698">valueB</span></span> |
| <span data-ttu-id="c2962-699">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="c2962-699">json_array:subsection:1</span></span> | <span data-ttu-id="c2962-700">valueC</span><span class="sxs-lookup"><span data-stu-id="c2962-700">valueC</span></span> |
| <span data-ttu-id="c2962-701">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="c2962-701">json_array:subsection:2</span></span> | <span data-ttu-id="c2962-702">valueD</span><span class="sxs-lookup"><span data-stu-id="c2962-702">valueD</span></span> |

<span data-ttu-id="c2962-703">W przykładowej aplikacji następującej klasy POCO jest dostępny dla powiązania pary klucz wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c2962-703">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c2962-704">Po powiązaniu `JsonArrayExample.Key` przechowuje wartość `valueA`.</span><span class="sxs-lookup"><span data-stu-id="c2962-704">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="c2962-705">Wartości podsekcji są przechowywane we właściwości tablicy POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="c2962-705">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="c2962-706">`JsonArrayExample.Subsection` Indeks</span><span class="sxs-lookup"><span data-stu-id="c2962-706">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="c2962-707">`JsonArrayExample.Subsection` Wartość</span><span class="sxs-lookup"><span data-stu-id="c2962-707">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="c2962-708">0</span><span class="sxs-lookup"><span data-stu-id="c2962-708">0</span></span>                                   | <span data-ttu-id="c2962-709">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="c2962-709">valueB</span></span>                              |
| <span data-ttu-id="c2962-710">1</span><span class="sxs-lookup"><span data-stu-id="c2962-710">1</span></span>                                   | <span data-ttu-id="c2962-711">valueC</span><span class="sxs-lookup"><span data-stu-id="c2962-711">valueC</span></span>                              |
| <span data-ttu-id="c2962-712">2</span><span class="sxs-lookup"><span data-stu-id="c2962-712">2</span></span>                                   | <span data-ttu-id="c2962-713">valueD</span><span class="sxs-lookup"><span data-stu-id="c2962-713">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="c2962-714">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="c2962-714">Custom configuration provider</span></span>

<span data-ttu-id="c2962-715">Przykładowa aplikacja pokazuje, jak utworzyć dostawcę konfiguracji podstawowej, który odczytuje pary klucz wartość konfiguracji z bazy danych za pomocą [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="c2962-715">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="c2962-716">Dostawcę ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="c2962-716">The provider has the following characteristics:</span></span>

* <span data-ttu-id="c2962-717">Bazy danych w pamięci programu EF jest używana do celów demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="c2962-717">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="c2962-718">Aby użyć bazy danych, która wymaga parametrów połączenia, należy zaimplementować pomocniczy `ConfigurationBuilder` podawać parametrów połączenia z innego dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c2962-718">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="c2962-719">Dostawca odczytuje tabelę bazy danych do konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="c2962-719">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="c2962-720">Dostawca nie kwerendy bazy danych na podstawie-key.</span><span class="sxs-lookup"><span data-stu-id="c2962-720">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="c2962-721">Załaduj ponownie przy zmianie nie jest zaimplementowany, więc zaktualizowanie bazy danych, po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2962-721">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="c2962-722">Zdefiniuj `EFConfigurationValue` jednostki do przechowywania wartości konfiguracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c2962-722">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="c2962-723">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2962-723">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c2962-724">Dodaj `EFConfigurationContext` do przechowywania i uzyskiwania dostępu skonfigurowane wartości.</span><span class="sxs-lookup"><span data-stu-id="c2962-724">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="c2962-725">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2962-725">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c2962-726">Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="c2962-726">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="c2962-727">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2962-727">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c2962-728">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="c2962-728">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="c2962-729">Dostawca konfiguracji inicjuje bazy danych, gdy jest on pusty.</span><span class="sxs-lookup"><span data-stu-id="c2962-729">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="c2962-730">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2962-730">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c2962-731">`AddEFConfiguration` Metody rozszerzenia, który pozwala na dodawanie źródła konfiguracji do `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c2962-731">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="c2962-732">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2962-732">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c2962-733">Poniższy kod przedstawia sposób używania niestandardowego `EFConfigurationProvider` w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2962-733">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="c2962-734">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="c2962-734">Access configuration during startup</span></span>

<span data-ttu-id="c2962-735">Wstrzykiwanie `IConfiguration` do `Startup` konstruktora do dostępu do wartości konfiguracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c2962-735">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c2962-736">Uzyskać dostępu do konfiguracji w `Startup.Configure`, albo wstrzyknąć `IConfiguration` bezpośrednio do metody lub użyj wystąpienia z konstruktora:</span><span class="sxs-lookup"><span data-stu-id="c2962-736">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="c2962-737">Na przykład uzyskiwania dostępu do konfiguracji za pomocą metod jako udogodnienie uruchamiania zobacz [uruchamiania aplikacji: Wygodne metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="c2962-737">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="c2962-738">Konfiguracja dostępu na stronie stron Razor lub w widoku MVC</span><span class="sxs-lookup"><span data-stu-id="c2962-738">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="c2962-739">Aby uzyskać dostęp do ustawień konfiguracji na stronie stron Razor lub widoku MVC, należy dodać [użycie dyrektywy](xref:mvc/views/razor#using) ([odwołanie w C#: using — dyrektywa](/dotnet/csharp/language-reference/keywords/using-directive)) dla [Microsoft.Extensions.Configuration przestrzeni nazw ](xref:Microsoft.Extensions.Configuration) i wstawiać <xref:Microsoft.Extensions.Configuration.IConfiguration> do strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="c2962-739">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="c2962-740">Na stronie stron Razor:</span><span class="sxs-lookup"><span data-stu-id="c2962-740">In a Razor Pages page:</span></span>

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

<span data-ttu-id="c2962-741">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="c2962-741">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="c2962-742">Dodaj konfigurację z zewnętrznego zestawu</span><span class="sxs-lookup"><span data-stu-id="c2962-742">Add configuration from an external assembly</span></span>

<span data-ttu-id="c2962-743"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Implementacji umożliwia dodawanie rozszerzeń do aplikacji przy uruchamianiu z zewnętrznego zestawu poza aplikacji `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="c2962-743">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="c2962-744">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c2962-744">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2962-745">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c2962-745">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="c2962-746">Głębszej analizy konfiguracji firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="c2962-746">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
