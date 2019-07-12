---
title: Konfiguracja w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core za pomocą interfejsu API konfiguracji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 3351ab743ce38b78b1c5857e52020fdeda12cbe7
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855813"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="c77ff-103">Konfiguracja w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c77ff-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="c77ff-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c77ff-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c77ff-105">Konfiguracja aplikacji w programie ASP.NET Core opiera się na parach klucz wartość ustanowione przez *dostawcy konfiguracji*.</span><span class="sxs-lookup"><span data-stu-id="c77ff-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="c77ff-106">Dostawcy konfiguracji odczytania danych konfiguracyjnych do pary klucz wartość z wielu źródeł w konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="c77ff-107">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c77ff-107">Azure Key Vault</span></span>
* <span data-ttu-id="c77ff-108">Konfiguracja aplikacji platformy Azure</span><span class="sxs-lookup"><span data-stu-id="c77ff-108">Azure App Configuration</span></span>
* <span data-ttu-id="c77ff-109">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c77ff-109">Command-line arguments</span></span>
* <span data-ttu-id="c77ff-110">Niestandardowi dostawcy (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="c77ff-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="c77ff-111">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="c77ff-111">Directory files</span></span>
* <span data-ttu-id="c77ff-112">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c77ff-112">Environment variables</span></span>
* <span data-ttu-id="c77ff-113">Obiekty platformy .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="c77ff-113">In-memory .NET objects</span></span>
* <span data-ttu-id="c77ff-114">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="c77ff-114">Settings files</span></span>

<span data-ttu-id="c77ff-115">Pakiety konfiguracji dla typowych scenariuszy dostawcy konfiguracji znajdują się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-115">Configuration packages for common configuration provider scenarios are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="c77ff-116">Przykłady, które należy wykonać i w przykładowej aplikacji kodu <xref:Microsoft.Extensions.Configuration> przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="c77ff-116">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="c77ff-117">*Wzorzec opcje* jest rozszerzeniem pojęć związanych z konfiguracją opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="c77ff-117">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="c77ff-118">Opcje używa klas do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="c77ff-118">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="c77ff-119">Aby uzyskać więcej informacji na temat korzystania z wzorca opcji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-119">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="c77ff-120">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c77ff-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="c77ff-121">Hostowanie w zależności od konfiguracji aplikacji</span><span class="sxs-lookup"><span data-stu-id="c77ff-121">Host versus app configuration</span></span>

<span data-ttu-id="c77ff-122">Zanim aplikacja jest skonfigurowana i uruchomiona, *hosta* skonfigurowany i uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="c77ff-122">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="c77ff-123">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-123">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="c77ff-124">Zarówno aplikacja, jak i hosta są skonfigurowane przy użyciu dostawcy konfiguracji opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="c77ff-124">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="c77ff-125">Pary klucz wartość konfiguracji hosta stają się częścią globalnej konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-125">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="c77ff-126">Aby uzyskać więcej informacji na temat sposobu konfiguracji dostawcy są używane, gdy host jest wbudowana i wpływ źródła konfiguracji hosta konfiguracji, zobacz [hosta](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="c77ff-126">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="c77ff-127">Konfiguracja domyślna</span><span class="sxs-lookup"><span data-stu-id="c77ff-127">Default configuration</span></span>

<span data-ttu-id="c77ff-128">Aplikacje oparte na platformy ASP.NET Core sieci Web [dotnet nowe](/dotnet/core/tools/dotnet-new) wywołania szablonów <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> podczas tworzenia na hoście.</span><span class="sxs-lookup"><span data-stu-id="c77ff-128">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="c77ff-129"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> udostępnia domyślną konfigurację aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="c77ff-129"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="c77ff-130">Konfiguracja hosta jest dostarczana z:</span><span class="sxs-lookup"><span data-stu-id="c77ff-130">Host configuration is provided from:</span></span>
  * <span data-ttu-id="c77ff-131">Zmienne środowiskowe prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`) przy użyciu [dostawcę konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c77ff-131">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="c77ff-132">Prefiks (`ASPNETCORE_`) jest usuwany po załadowaniu pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-132">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="c77ff-133">Argumenty wiersza polecenia przy użyciu [dostawcę konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c77ff-133">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="c77ff-134">Konfiguracja aplikacji jest dostarczana z:</span><span class="sxs-lookup"><span data-stu-id="c77ff-134">App configuration is provided from:</span></span>
  * <span data-ttu-id="c77ff-135">*appSettings.JSON* przy użyciu [dostawcę konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c77ff-135">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="c77ff-136">*appSettings. {Środowiska} .json* przy użyciu [dostawcę konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c77ff-136">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="c77ff-137">[Klucz tajny Menedżera](xref:security/app-secrets) uruchamiania aplikacji `Development` środowisko przy użyciu zestawu wpisu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-137">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="c77ff-138">Zmienne środowiskowe za pomocą [dostawcę konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c77ff-138">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="c77ff-139">Jeśli jest używany prefiks niestandardowy (na przykład `PREFIX_` z `.AddEnvironmentVariables(prefix: "PREFIX_")`), prefiks jest usuwany po załadowaniu pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-139">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="c77ff-140">Argumenty wiersza polecenia przy użyciu [dostawcę konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c77ff-140">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="c77ff-141">Dostawcy konfiguracji zostały omówione w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-141">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="c77ff-142">Aby uzyskać więcej informacji na hoście i <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, zobacz <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-142">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="c77ff-143">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="c77ff-143">Security</span></span>

<span data-ttu-id="c77ff-144">Przyjmuje następujące najlepsze rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="c77ff-144">Adopt the following best practices:</span></span>

* <span data-ttu-id="c77ff-145">Nigdy nie przechowują hasła lub innych danych poufnych w kodzie dostawcy konfiguracji lub w plikach konfiguracyjnych w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-145">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="c77ff-146">Nie używać kluczy tajnych w środowisku produkcyjnym w trakcie opracowywania lub środowisk testowych.</span><span class="sxs-lookup"><span data-stu-id="c77ff-146">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="c77ff-147">Określ klucze tajne poza projektem, nie może być przypadkowo wszelkich starań, by repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="c77ff-147">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="c77ff-148">Dowiedz się więcej o [jak używać wielu środowisk](xref:fundamentals/environments) i zarządzanie nimi [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie programowania przy użyciu klucza tajnego Menedżera](xref:security/app-secrets) (zawiera wskazówki dotyczące używania zmiennych środowiskowych do przechowywania dane poufne).</span><span class="sxs-lookup"><span data-stu-id="c77ff-148">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="c77ff-149">Menedżer klucz tajny używa dostawcy konfiguracji plików do przechowywania kluczy tajnych użytkownika w pliku JSON w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="c77ff-149">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="c77ff-150">Dostawca konfiguracji pliku jest opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-150">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="c77ff-151">[Usługa Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stanowi jedną z opcji dla bezpieczne przechowywanie kluczy tajnych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-151">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="c77ff-152">Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-152">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="c77ff-153">Dane hierarchiczne konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c77ff-153">Hierarchical configuration data</span></span>

<span data-ttu-id="c77ff-154">Interfejs API konfiguracji jest zdolny do utrzymywania dane hierarchiczne konfiguracji spłaszczając dane hierarchiczne przy użyciu ogranicznika w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-154">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="c77ff-155">W następującym pliku JSON cztery klucze, istnieją w ze strukturą hierarchii dwie sekcje:</span><span class="sxs-lookup"><span data-stu-id="c77ff-155">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="c77ff-156">Gdy plik jest do odczytu do konfiguracji, unikatowe klucze są tworzone do obsługi struktury danych hierarchicznych oryginalnego źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-156">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="c77ff-157">Sekcji i kluczy są spłaszczone przy użyciu dwukropek (`:`) do pierwotnej struktury obsługi:</span><span class="sxs-lookup"><span data-stu-id="c77ff-157">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="c77ff-158">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c77ff-158">section0:key0</span></span>
* <span data-ttu-id="c77ff-159">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c77ff-159">section0:key1</span></span>
* <span data-ttu-id="c77ff-160">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c77ff-160">section1:key0</span></span>
* <span data-ttu-id="c77ff-161">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c77ff-161">section1:key1</span></span>

<span data-ttu-id="c77ff-162"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> metody są dostępne do izolowania sekcje i elementy podrzędne do sekcji w danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-162"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="c77ff-163">Te metody są opisane w dalszej części w [GetSection GetChildren i Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="c77ff-163">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="c77ff-164">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-164">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="c77ff-165">Konwencje</span><span class="sxs-lookup"><span data-stu-id="c77ff-165">Conventions</span></span>

<span data-ttu-id="c77ff-166">Przy uruchamianiu aplikacji źródła konfiguracji są do odczytu w kolejności, że podano ich dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-166">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="c77ff-167">Dostawcy konfiguracji, które implementują wykrywania zmian mają możliwość Załaduj ponownie konfigurację, gdy podstawowy ustawienie to ulegnie zmianie.</span><span class="sxs-lookup"><span data-stu-id="c77ff-167">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="c77ff-168">Na przykład dostawca konfiguracji pliku (opisanych w dalszej części tego tematu) i [dostawcy konfiguracji magazynu kluczy Azure](xref:security/key-vault-configuration) zaimplementować wykrywania zmian.</span><span class="sxs-lookup"><span data-stu-id="c77ff-168">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="c77ff-169"><xref:Microsoft.Extensions.Configuration.IConfiguration> jest dostępna w aplikacji [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="c77ff-169"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="c77ff-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> być dodane do stron Razor <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> można uzyskać konfiguracji dla klasy:</span><span class="sxs-lookup"><span data-stu-id="c77ff-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    // The _config local variable is used to obtain configuration 
    // throughout the class.
}
```

<span data-ttu-id="c77ff-171">Dostawcy konfiguracji nie może wykorzystywać DI, ponieważ nie jest dostępna podczas konfigurowania one przez hosta.</span><span class="sxs-lookup"><span data-stu-id="c77ff-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="c77ff-172">Klucze konfiguracji przyjęcie następujących konwencji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="c77ff-173">Kluczy jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="c77ff-173">Keys are case-insensitive.</span></span> <span data-ttu-id="c77ff-174">Na przykład `ConnectionString` i `connectionstring` są traktowane jako równoważne kluczy.</span><span class="sxs-lookup"><span data-stu-id="c77ff-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="c77ff-175">Jeśli ten sam klucz jest ustawiany przez dostawców o tej samej lub innej konfiguracji, ostatnia wartość klucza jest wartość.</span><span class="sxs-lookup"><span data-stu-id="c77ff-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="c77ff-176">Klucze hierarchicznych</span><span class="sxs-lookup"><span data-stu-id="c77ff-176">Hierarchical keys</span></span>
  * <span data-ttu-id="c77ff-177">W ramach konfiguracji interfejsu API, separator dwukropka (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="c77ff-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="c77ff-178">W zmiennych środowiskowych separator dwukropek, może nie działać na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="c77ff-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="c77ff-179">Podwójnym podkreśleniem (`__`) jest obsługiwana przez wszystkie platformy i jest konwertowany na dwukropka.</span><span class="sxs-lookup"><span data-stu-id="c77ff-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="c77ff-180">W usłudze Azure Key Vault, klucze hierarchiczne, użyj `--` (dwa łączniki) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="c77ff-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="c77ff-181">Należy podać kod, aby zastąpić kresek dwukropka po wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="c77ff-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Obsługuje tablice powiązania do obiektów przy użyciu tablicy indeksów w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="c77ff-183">Powiązanie tablicy jest opisana w [tablicy należy powiązać klasę](#bind-an-array-to-a-class) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="c77ff-184">Wartości konfiguracji przyjęcie następujących konwencji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="c77ff-185">Wartości są ciągami.</span><span class="sxs-lookup"><span data-stu-id="c77ff-185">Values are strings.</span></span>
* <span data-ttu-id="c77ff-186">Wartości null nie przechowywanych w konfiguracji lub powiązane z obiektami.</span><span class="sxs-lookup"><span data-stu-id="c77ff-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="c77ff-187">dostawcy</span><span class="sxs-lookup"><span data-stu-id="c77ff-187">Providers</span></span>

<span data-ttu-id="c77ff-188">W poniższej tabeli przedstawiono dostępne dla aplikacji platformy ASP.NET Core dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="c77ff-189">Dostawca</span><span class="sxs-lookup"><span data-stu-id="c77ff-189">Provider</span></span> | <span data-ttu-id="c77ff-190">Udostępnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="c77ff-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="c77ff-191">[Dostawca konfiguracji magazynu w usłudze Azure klucza](xref:security/key-vault-configuration) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="c77ff-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="c77ff-192">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c77ff-192">Azure Key Vault</span></span> |
| <span data-ttu-id="c77ff-193">[Dostawca konfiguracji usługi Azure App](/azure/azure-app-configuration/quickstart-aspnet-core-app) (dokumentacja platformy Azure)</span><span class="sxs-lookup"><span data-stu-id="c77ff-193">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="c77ff-194">Konfiguracja aplikacji platformy Azure</span><span class="sxs-lookup"><span data-stu-id="c77ff-194">Azure App Configuration</span></span> |
| [<span data-ttu-id="c77ff-195">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c77ff-195">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="c77ff-196">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c77ff-196">Command-line parameters</span></span> |
| [<span data-ttu-id="c77ff-197">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="c77ff-197">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="c77ff-198">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="c77ff-198">Custom source</span></span> |
| [<span data-ttu-id="c77ff-199">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="c77ff-199">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="c77ff-200">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c77ff-200">Environment variables</span></span> |
| [<span data-ttu-id="c77ff-201">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="c77ff-201">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="c77ff-202">Pliki INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="c77ff-202">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="c77ff-203">Dostawca klucza każdego pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c77ff-203">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="c77ff-204">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="c77ff-204">Directory files</span></span> |
| [<span data-ttu-id="c77ff-205">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="c77ff-205">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="c77ff-206">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="c77ff-206">In-memory collections</span></span> |
| <span data-ttu-id="c77ff-207">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="c77ff-207">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="c77ff-208">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="c77ff-208">File in the user profile directory</span></span> |

<span data-ttu-id="c77ff-209">Źródła konfiguracji są do odczytu w kolejności określono ich dostawców konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="c77ff-209">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="c77ff-210">Dostawcy konfiguracji opisanych w tym temacie są opisane w kolejności alfabetycznej, a nie w kolejności, Twój kod może zorganizować ich.</span><span class="sxs-lookup"><span data-stu-id="c77ff-210">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="c77ff-211">Kolejność dostawców konfiguracji w kodzie do własnych priorytetów do bazowego źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-211">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="c77ff-212">Jest zwykle kolejność dostawców konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-212">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="c77ff-213">Pliki (*appsettings.json*, *appsettings. { Środowisko} .json*, gdzie `{Environment}` jest bieżące środowisko hostingu aplikacji)</span><span class="sxs-lookup"><span data-stu-id="c77ff-213">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="c77ff-214">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c77ff-214">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="c77ff-215">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym tylko)</span><span class="sxs-lookup"><span data-stu-id="c77ff-215">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="c77ff-216">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c77ff-216">Environment variables</span></span>
1. <span data-ttu-id="c77ff-217">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c77ff-217">Command-line arguments</span></span>

<span data-ttu-id="c77ff-218">Jest to powszechną praktyką do ostatniej pozycji dostawcę konfiguracji wiersza polecenia w serii dostawcy, aby zezwolić na argumenty wiersza polecenia zastąpić konfiguracji ustawione przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="c77ff-218">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="c77ff-219">Ta sekwencja dostawcy są umieszczane w miejscu, podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-219">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="c77ff-220">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c77ff-220">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="c77ff-221">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="c77ff-221">ConfigureAppConfiguration</span></span>

<span data-ttu-id="c77ff-222">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta do określenia aplikacji dostawcy konfiguracji oprócz tych dodawane automatycznie przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="c77ff-222">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

<span data-ttu-id="c77ff-223">Dostarczony do aplikacji w konfiguracji <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> jest dostępna podczas uruchamiania aplikacji, w tym `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-223">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c77ff-224">Aby uzyskać więcej informacji, zobacz [konfiguracji dostępu podczas uruchamiania](#access-configuration-during-startup) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-224">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="c77ff-225">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c77ff-225">Command-line Configuration Provider</span></span>

<span data-ttu-id="c77ff-226"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Ładuje konfiguracji z pary klucz wartość argumentu wiersza polecenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c77ff-226">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="c77ff-227">Aby aktywować wiersza polecenia konfiguracji, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metoda rozszerzająca zostanie wywołana w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-227">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c77ff-228">`AddCommandLine` jest wywoływana automatycznie podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-228">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="c77ff-229">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c77ff-229">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="c77ff-230">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c77ff-230">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c77ff-231">Konfiguracja opcjonalna z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="c77ff-231">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="c77ff-232">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="c77ff-232">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="c77ff-233">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="c77ff-233">Environment variables.</span></span>

<span data-ttu-id="c77ff-234">`CreateDefaultBuilder` dodaje ostatniego wiersza polecenia dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-234">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="c77ff-235">Argumenty wiersza polecenia przekazywane w czasie wykonywania zastąpić konfiguracji ustawione przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="c77ff-235">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="c77ff-236">`CreateDefaultBuilder` działa, gdy host jest konstruowany.</span><span class="sxs-lookup"><span data-stu-id="c77ff-236">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="c77ff-237">W związku z tym, wiersza polecenia konfiguracji aktywowany przez `CreateDefaultBuilder` mogą mieć wpływ na sposób host jest skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="c77ff-237">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="c77ff-238">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-238">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="c77ff-239">`AddCommandLine` zostały już wywołane `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-239">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c77ff-240">Jeśli zachodzi potrzeba zapewniają konfigurację aplikacji i nadal będzie można przesłonić tę konfigurację za pomocą argumentów wiersza polecenia, należy wywołać dodatkowych dostawców aplikacji <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> i wywołać `AddCommandLine` ostatni.</span><span class="sxs-lookup"><span data-stu-id="c77ff-240">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="c77ff-241">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c77ff-241">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c77ff-242">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="c77ff-242">**Example**</span></span>

<span data-ttu-id="c77ff-243">Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta, który zawiera wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-243">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="c77ff-244">Otwórz wiersz polecenia w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-244">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="c77ff-245">Argument wiersza polecenia, aby podać `dotnet run` polecenia `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-245">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="c77ff-246">Po uruchomieniu aplikacji otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-246">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c77ff-247">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla argumentu wiersza polecenia konfiguracji udostępnionego `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-247">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="c77ff-248">Argumenty</span><span class="sxs-lookup"><span data-stu-id="c77ff-248">Arguments</span></span>

<span data-ttu-id="c77ff-249">Wartość musi stosować się znak równości (`=`), lub klucz musi mieć prefiks (`--` lub `/`) Jeśli wartość jest zgodna spację.</span><span class="sxs-lookup"><span data-stu-id="c77ff-249">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="c77ff-250">Wartość może być null, jeśli jest używany znak równości (na przykład `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="c77ff-250">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="c77ff-251">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="c77ff-251">Key prefix</span></span>               | <span data-ttu-id="c77ff-252">Przykład</span><span class="sxs-lookup"><span data-stu-id="c77ff-252">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="c77ff-253">Nie prefiksu</span><span class="sxs-lookup"><span data-stu-id="c77ff-253">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="c77ff-254">Dwa łączniki (`--`)</span><span class="sxs-lookup"><span data-stu-id="c77ff-254">Two dashes (`--`)</span></span>        | <span data-ttu-id="c77ff-255">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="c77ff-255">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="c77ff-256">Kreski ułamkowej (`/`)</span><span class="sxs-lookup"><span data-stu-id="c77ff-256">Forward slash (`/`)</span></span>      | <span data-ttu-id="c77ff-257">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="c77ff-257">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="c77ff-258">W tym samym poleceniu nie Mieszaj argument wiersza polecenia pary klucz wartość, które znakiem równości za pomocą pary klucz wartość, korzystających z przestrzeni.</span><span class="sxs-lookup"><span data-stu-id="c77ff-258">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="c77ff-259">Przykładowe polecenia:</span><span class="sxs-lookup"><span data-stu-id="c77ff-259">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="c77ff-260">Przełącz mapowania</span><span class="sxs-lookup"><span data-stu-id="c77ff-260">Switch mappings</span></span>

<span data-ttu-id="c77ff-261">Przełącznik jest transformowanie na nazwę klucza wymiany logiki.</span><span class="sxs-lookup"><span data-stu-id="c77ff-261">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="c77ff-262">Podczas ręcznego tworzenia konfiguracji za pomocą <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, może zapewnić słownika zastępstw przełącznika <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metody.</span><span class="sxs-lookup"><span data-stu-id="c77ff-262">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="c77ff-263">W przypadku przełącznika słownik mapowań słownika jest sprawdzane dla klucza, który pasuje do klucza, dostarczone przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c77ff-263">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="c77ff-264">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) jest przekazywana do zestawu pary klucz wartość do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-264">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="c77ff-265">Mapowanie przełącznika jest wymagane dla dowolnego klucz wiersza polecenia poprzedzone kreską pojedynczego (`-`).</span><span class="sxs-lookup"><span data-stu-id="c77ff-265">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="c77ff-266">Przełącz mapowania słownika kluczowych reguł:</span><span class="sxs-lookup"><span data-stu-id="c77ff-266">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="c77ff-267">Przełączniki musi rozpoczynać się kreską (`-`) lub podwójne kreska (`--`).</span><span class="sxs-lookup"><span data-stu-id="c77ff-267">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="c77ff-268">Słownik mapowań przełącznik nie może zawierać zduplikowane klucze.</span><span class="sxs-lookup"><span data-stu-id="c77ff-268">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="c77ff-269">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-269">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c77ff-270">Jak pokazano w powyższym przykładzie wywołanie `CreateDefaultBuilder` nie należy przekazywać argumenty, gdy przełącznik mapowania są używane.</span><span class="sxs-lookup"><span data-stu-id="c77ff-270">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="c77ff-271">`CreateDefaultBuilder` metody `AddCommandLine` wywołania nie zawiera zamapowanego przełączników, a nie istnieje sposób do przekazania słownik mapowania przełącznika `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-271">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c77ff-272">Jeśli argumenty zamapowanego przełącznik dołączania i są przekazywane do `CreateDefaultBuilder`, jego `AddCommandLine` dostawcy nie uda się zainicjowanie z <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-272">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="c77ff-273">Rozwiązanie nie jest do przekazywania argumentów do `CreateDefaultBuilder` , ale zamiast tego umożliwiające `ConfigurationBuilder` metody `AddCommandLine` metody do przetwarzania argumentów i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="c77ff-273">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="c77ff-274">Po utworzeniu przełącznika słownik mapowań zawiera dane wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c77ff-274">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="c77ff-275">Key</span><span class="sxs-lookup"><span data-stu-id="c77ff-275">Key</span></span>       | <span data-ttu-id="c77ff-276">Wartość</span><span class="sxs-lookup"><span data-stu-id="c77ff-276">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="c77ff-277">Jeśli mapowane przełącznika klucze są używane podczas uruchamiania aplikacji, konfiguracji odbiera wartości konfiguracji na kluczu pochodzącego ze słownika:</span><span class="sxs-lookup"><span data-stu-id="c77ff-277">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="c77ff-278">Po uruchomieniu polecenia poprzedniej konfiguracji zawiera wartości podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c77ff-278">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="c77ff-279">Key</span><span class="sxs-lookup"><span data-stu-id="c77ff-279">Key</span></span>               | <span data-ttu-id="c77ff-280">Wartość</span><span class="sxs-lookup"><span data-stu-id="c77ff-280">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="c77ff-281">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="c77ff-281">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="c77ff-282"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Ładuje konfiguracji ze środowiska zmiennej pary klucz wartość w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c77ff-282">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="c77ff-283">Aby aktywować środowisko zmiennych konfiguracji, należy wywołać <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-283">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="c77ff-284">[Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) pozwala na Ustawianie zmiennych środowiskowych w portalu Azure, które mogą zastąpić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="c77ff-284">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="c77ff-285">Aby uzyskać więcej informacji, zobacz [aplikacje platformy Azure: Zastąpienie konfiguracji aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="c77ff-285">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="c77ff-286">`AddEnvironmentVariables` Służy do ładowania zmienne środowiskowe z prefiksem `ASPNETCORE_` dla [konfiguracji hosta](#host-versus-app-configuration) gdy nowy <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> został zainicjowany.</span><span class="sxs-lookup"><span data-stu-id="c77ff-286">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is initialized.</span></span> <span data-ttu-id="c77ff-287">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c77ff-287">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="c77ff-288">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c77ff-288">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c77ff-289">Konfiguracja aplikacji ze zmiennych środowiskowych unprefixed przez wywołanie metody `AddEnvironmentVariables` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-289">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="c77ff-290">Konfiguracja opcjonalna z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="c77ff-290">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="c77ff-291">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="c77ff-291">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="c77ff-292">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c77ff-292">Command-line arguments.</span></span>

<span data-ttu-id="c77ff-293">Dostawca konfiguracji zmiennych środowiskowych jest wywoływana po konfiguracji zostanie nawiązane z wpisami tajnymi użytkowników i *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="c77ff-293">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="c77ff-294">W tym miejscu podczas wywoływania dostawcy umożliwia zmienne środowiskowe są odczytywane w czasie wykonywania do zastępowania konfiguracji ustawione przez użytkownika hasła i *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="c77ff-294">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="c77ff-295">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-295">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="c77ff-296">Jeśli konieczne jest zapewnienie konfiguracji aplikacji z dodatkowe zmienne środowiskowe, wywołaj dodatkowych dostawców aplikacji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> i wywołać `AddEnvironmentVariables` z prefiksem.</span><span class="sxs-lookup"><span data-stu-id="c77ff-296">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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
                // Call AddEnvironmentVariables last if you need to allow
                // environment variables to override values from other 
                // providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="c77ff-297">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c77ff-297">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="c77ff-298">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="c77ff-298">**Example**</span></span>

<span data-ttu-id="c77ff-299">Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta, który zawiera wywołanie `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-299">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="c77ff-300">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="c77ff-300">Run the sample app.</span></span> <span data-ttu-id="c77ff-301">Otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-301">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c77ff-302">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla zmiennej środowiskowej `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-302">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="c77ff-303">Wartość odzwierciedla środowisko, w którym aplikacja jest uruchomiona, zwykle `Development` podczas uruchamiania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="c77ff-303">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="c77ff-304">Przechowywać listę zmiennych środowiskowych renderowany przez aplikację krótki, aplikacji filtry zmienne środowiskowe do tych rozpoczynających się od następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c77ff-304">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="c77ff-305">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="c77ff-305">ASPNETCORE_</span></span>
* <span data-ttu-id="c77ff-306">adresy URL</span><span class="sxs-lookup"><span data-stu-id="c77ff-306">urls</span></span>
* <span data-ttu-id="c77ff-307">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="c77ff-307">Logging</span></span>
* <span data-ttu-id="c77ff-308">ŚRODOWISKO</span><span class="sxs-lookup"><span data-stu-id="c77ff-308">ENVIRONMENT</span></span>
* <span data-ttu-id="c77ff-309">contentRoot</span><span class="sxs-lookup"><span data-stu-id="c77ff-309">contentRoot</span></span>
* <span data-ttu-id="c77ff-310">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="c77ff-310">AllowedHosts</span></span>
* <span data-ttu-id="c77ff-311">applicationName</span><span class="sxs-lookup"><span data-stu-id="c77ff-311">applicationName</span></span>
* <span data-ttu-id="c77ff-312">Wiersz polecenia</span><span class="sxs-lookup"><span data-stu-id="c77ff-312">CommandLine</span></span>

<span data-ttu-id="c77ff-313">Jeśli chcesz udostępnić wszystkie zmienne środowiskowe, dostępne dla aplikacji, zmień `FilteredConfiguration` w *Pages/Index.cshtml.cs* do następującego:</span><span class="sxs-lookup"><span data-stu-id="c77ff-313">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="c77ff-314">Prefiksy</span><span class="sxs-lookup"><span data-stu-id="c77ff-314">Prefixes</span></span>

<span data-ttu-id="c77ff-315">Zmienne środowiskowe ładowane z konfiguracji aplikacji są filtrowane podczas podawania prefiks `AddEnvironmentVariables` metody.</span><span class="sxs-lookup"><span data-stu-id="c77ff-315">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="c77ff-316">Na przykład, aby filtr zmiennych środowiskowych na prefiksie `CUSTOM_`, podaj prefiks, który dostawca konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-316">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="c77ff-317">Prefiks jest odłączany podczas tworzenia pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-317">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="c77ff-318">Statyczne wygodna metoda `CreateDefaultBuilder` tworzy <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> Aby ustanowić hosta aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-318">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="c77ff-319">Gdy `WebHostBuilder` jest tworzony, znajdzie jego konfigurację hosta w zmiennych środowiskowych z prefiksem `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-319">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="c77ff-320">**Prefiksy ciąg połączenia**</span><span class="sxs-lookup"><span data-stu-id="c77ff-320">**Connection string prefixes**</span></span>

<span data-ttu-id="c77ff-321">Interfejs API konfiguracji zawiera reguły jest przetwarzana w specjalny cztery połączenia ciągu zmiennych środowiskowych zajmujących się Konfigurowanie parametrów połączenia platformy Azure dla środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-321">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="c77ff-322">Zmienne środowiskowe z prefiksami pokazano w tabeli są ładowane do aplikacji, jeśli nie dostarczono żadnego prefiksu do `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-322">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="c77ff-323">Prefiks ciągu połączenia</span><span class="sxs-lookup"><span data-stu-id="c77ff-323">Connection string prefix</span></span> | <span data-ttu-id="c77ff-324">Dostawca</span><span class="sxs-lookup"><span data-stu-id="c77ff-324">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="c77ff-325">Dostawcy niestandardowego</span><span class="sxs-lookup"><span data-stu-id="c77ff-325">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="c77ff-326">MySQL</span><span class="sxs-lookup"><span data-stu-id="c77ff-326">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="c77ff-327">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c77ff-327">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="c77ff-328">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c77ff-328">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="c77ff-329">Gdy zmienna środowiskowa są odnajdywane i ładowane do konfiguracji za pomocą dowolnego z czterech prefiksy z tabelą:</span><span class="sxs-lookup"><span data-stu-id="c77ff-329">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="c77ff-330">Klucz konfiguracji jest tworzony, usuwając prefiks zmiennej środowiska i dodając kluczowych sekcję konfiguracji (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="c77ff-330">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="c77ff-331">Tworzony jest nową parę klucz wartość konfiguracji, który reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, który nie ma określonego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="c77ff-331">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="c77ff-332">Klucz zmiennych środowiska</span><span class="sxs-lookup"><span data-stu-id="c77ff-332">Environment variable key</span></span> | <span data-ttu-id="c77ff-333">Klucz konfiguracji przekonwertowany</span><span class="sxs-lookup"><span data-stu-id="c77ff-333">Converted configuration key</span></span> | <span data-ttu-id="c77ff-334">Pozycja konfiguracji dostawcy</span><span class="sxs-lookup"><span data-stu-id="c77ff-334">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c77ff-335">Wpis konfiguracyjny nie utworzony.</span><span class="sxs-lookup"><span data-stu-id="c77ff-335">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c77ff-336">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="c77ff-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="c77ff-337">Wartość:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="c77ff-337">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c77ff-338">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="c77ff-338">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="c77ff-339">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="c77ff-339">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c77ff-340">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="c77ff-340">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="c77ff-341">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="c77ff-341">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="c77ff-342">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="c77ff-342">File Configuration Provider</span></span>

<span data-ttu-id="c77ff-343"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową dla ładowania konfiguracji z systemu plików.</span><span class="sxs-lookup"><span data-stu-id="c77ff-343"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="c77ff-344">Następujących dostawców konfiguracji są przeznaczone dla określonych typów plików:</span><span class="sxs-lookup"><span data-stu-id="c77ff-344">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="c77ff-345">Dostawca konfiguracji INI</span><span class="sxs-lookup"><span data-stu-id="c77ff-345">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="c77ff-346">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="c77ff-346">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="c77ff-347">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="c77ff-347">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="c77ff-348">Dostawca konfiguracji INI</span><span class="sxs-lookup"><span data-stu-id="c77ff-348">INI Configuration Provider</span></span>

<span data-ttu-id="c77ff-349"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Ładuje konfiguracji z pary klucz wartość pliku INI w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c77ff-349">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="c77ff-350">Aby aktywować konfiguracji pliku INI, należy wywołać <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-350">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c77ff-351">Dwukropek może służyć do jako ogranicznik sekcji w konfiguracji pliku INI.</span><span class="sxs-lookup"><span data-stu-id="c77ff-351">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="c77ff-352">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c77ff-352">Overloads permit specifying:</span></span>

* <span data-ttu-id="c77ff-353">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c77ff-353">Whether the file is optional.</span></span>
* <span data-ttu-id="c77ff-354">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="c77ff-354">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c77ff-355"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="c77ff-355">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="c77ff-356">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-356">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddIniFile(
                    "config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="c77ff-357">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-357">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c77ff-358">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-358">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c77ff-359">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c77ff-359">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c77ff-360">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-360">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c77ff-361">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-361">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c77ff-362">Przykładowy plik konfiguracji INI:</span><span class="sxs-lookup"><span data-stu-id="c77ff-362">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="c77ff-363">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c77ff-363">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c77ff-364">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c77ff-364">section0:key0</span></span>
* <span data-ttu-id="c77ff-365">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c77ff-365">section0:key1</span></span>
* <span data-ttu-id="c77ff-366">section1:Subsection:key</span><span class="sxs-lookup"><span data-stu-id="c77ff-366">section1:subsection:key</span></span>
* <span data-ttu-id="c77ff-367">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="c77ff-367">section2:subsection0:key</span></span>
* <span data-ttu-id="c77ff-368">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="c77ff-368">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="c77ff-369">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="c77ff-369">JSON Configuration Provider</span></span>

<span data-ttu-id="c77ff-370"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Ładuje konfiguracji z pary klucz wartość plik JSON podczas wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c77ff-370">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="c77ff-371">Aby aktywować konfiguracji pliku JSON, należy wywołać <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-371">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c77ff-372">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c77ff-372">Overloads permit specifying:</span></span>

* <span data-ttu-id="c77ff-373">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c77ff-373">Whether the file is optional.</span></span>
* <span data-ttu-id="c77ff-374">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="c77ff-374">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c77ff-375"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="c77ff-375">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="c77ff-376">`AddJsonFile` jest wywoływana automatycznie, dwa razy, podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-376">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="c77ff-377">Metoda jest wywoływana, aby załadować konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-377">The method is called to load configuration from:</span></span>

* <span data-ttu-id="c77ff-378">*appSettings.JSON* &ndash; ten plik jest najpierw do odczytu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-378">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="c77ff-379">Wersja środowiska pliku można zastąpić wartości dostarczone przez *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="c77ff-379">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="c77ff-380">*appSettings. {Środowiska} .json* &ndash; środowiska wersję pliku zostanie załadowany na podstawie [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="c77ff-380">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="c77ff-381">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c77ff-381">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="c77ff-382">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c77ff-382">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c77ff-383">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="c77ff-383">Environment variables.</span></span>
* <span data-ttu-id="c77ff-384">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="c77ff-384">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="c77ff-385">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c77ff-385">Command-line arguments.</span></span>

<span data-ttu-id="c77ff-386">Dostawca konfiguracji JSON jest ustanawiane, najpierw.</span><span class="sxs-lookup"><span data-stu-id="c77ff-386">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="c77ff-387">W związku z tym, wpisami tajnymi użytkowników, zmienne środowiskowe i argumenty wiersza polecenia zastępuje konfiguracji ustawione przez *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="c77ff-387">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="c77ff-388">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji dla plików innych niż *appsettings.json* i *appsettings. { Środowisko} .json*:</span><span class="sxs-lookup"><span data-stu-id="c77ff-388">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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
                config.AddJsonFile(
                    "config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="c77ff-389">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-389">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c77ff-390">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-390">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c77ff-391">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c77ff-391">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c77ff-392">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-392">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c77ff-393">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-393">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c77ff-394">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="c77ff-394">**Example**</span></span>

<span data-ttu-id="c77ff-395">Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do hosta, który obejmuje dwa wywołania do tworzenia `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-395">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="c77ff-396">Konfiguracja została załadowana z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="c77ff-396">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="c77ff-397">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="c77ff-397">Run the sample app.</span></span> <span data-ttu-id="c77ff-398">Otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-398">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c77ff-399">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla konfiguracji, jak pokazano w tabeli, w zależności od środowiska.</span><span class="sxs-lookup"><span data-stu-id="c77ff-399">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="c77ff-400">Klucze konfiguracji rejestrowania, użyj dwukropka (`:`) jako separator hierarchicznej.</span><span class="sxs-lookup"><span data-stu-id="c77ff-400">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="c77ff-401">Key</span><span class="sxs-lookup"><span data-stu-id="c77ff-401">Key</span></span>                        | <span data-ttu-id="c77ff-402">Wartość rozwoju</span><span class="sxs-lookup"><span data-stu-id="c77ff-402">Development Value</span></span> | <span data-ttu-id="c77ff-403">Wartość produkcji</span><span class="sxs-lookup"><span data-stu-id="c77ff-403">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="c77ff-404">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="c77ff-404">Logging:LogLevel:System</span></span>    | <span data-ttu-id="c77ff-405">Informacje</span><span class="sxs-lookup"><span data-stu-id="c77ff-405">Information</span></span>       | <span data-ttu-id="c77ff-406">Informacje</span><span class="sxs-lookup"><span data-stu-id="c77ff-406">Information</span></span>      |
| <span data-ttu-id="c77ff-407">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="c77ff-407">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="c77ff-408">Informacje</span><span class="sxs-lookup"><span data-stu-id="c77ff-408">Information</span></span>       | <span data-ttu-id="c77ff-409">Informacje</span><span class="sxs-lookup"><span data-stu-id="c77ff-409">Information</span></span>      |
| <span data-ttu-id="c77ff-410">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="c77ff-410">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="c77ff-411">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="c77ff-411">Debug</span></span>             | <span data-ttu-id="c77ff-412">Błąd</span><span class="sxs-lookup"><span data-stu-id="c77ff-412">Error</span></span>            |
| <span data-ttu-id="c77ff-413">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="c77ff-413">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="c77ff-414">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="c77ff-414">XML Configuration Provider</span></span>

<span data-ttu-id="c77ff-415"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Ładuje konfiguracji z pary klucz wartość plik XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c77ff-415">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="c77ff-416">Aby aktywować Konfiguracja pliku XML, należy wywołać <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-416">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c77ff-417">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c77ff-417">Overloads permit specifying:</span></span>

* <span data-ttu-id="c77ff-418">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c77ff-418">Whether the file is optional.</span></span>
* <span data-ttu-id="c77ff-419">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="c77ff-419">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c77ff-420"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="c77ff-420">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="c77ff-421">Węzeł główny plik konfiguracji jest ignorowany podczas tworzenia pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-421">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="c77ff-422">Określać definicji typu dokumentu (DTD) lub przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="c77ff-422">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="c77ff-423">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-423">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddXmlFile(
                    "config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="c77ff-424">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-424">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c77ff-425">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-425">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c77ff-426">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c77ff-426">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c77ff-427">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-427">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c77ff-428">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-428">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c77ff-429">Pliki konfiguracji XML można użyć nazw unikatowych elementów dla powtarzających się sekcje:</span><span class="sxs-lookup"><span data-stu-id="c77ff-429">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="c77ff-430">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c77ff-430">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c77ff-431">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c77ff-431">section0:key0</span></span>
* <span data-ttu-id="c77ff-432">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c77ff-432">section0:key1</span></span>
* <span data-ttu-id="c77ff-433">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c77ff-433">section1:key0</span></span>
* <span data-ttu-id="c77ff-434">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c77ff-434">section1:key1</span></span>

<span data-ttu-id="c77ff-435">Powtarzające się elementy, które używają takiej samej nazwie elementu pracy, jeśli `name` atrybut jest używany do odróżniania elementów:</span><span class="sxs-lookup"><span data-stu-id="c77ff-435">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="c77ff-436">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c77ff-436">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c77ff-437">sekcja: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="c77ff-437">section:section0:key:key0</span></span>
* <span data-ttu-id="c77ff-438">sekcja: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="c77ff-438">section:section0:key:key1</span></span>
* <span data-ttu-id="c77ff-439">sekcja: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="c77ff-439">section:section1:key:key0</span></span>
* <span data-ttu-id="c77ff-440">sekcja: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="c77ff-440">section:section1:key:key1</span></span>

<span data-ttu-id="c77ff-441">Atrybuty można podać wartości:</span><span class="sxs-lookup"><span data-stu-id="c77ff-441">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="c77ff-442">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c77ff-442">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c77ff-443">klucz: atrybut</span><span class="sxs-lookup"><span data-stu-id="c77ff-443">key:attribute</span></span>
* <span data-ttu-id="c77ff-444">sekcja: klucz: atrybut</span><span class="sxs-lookup"><span data-stu-id="c77ff-444">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="c77ff-445">Dostawca klucza każdego pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c77ff-445">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="c77ff-446"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Korzysta z plików w katalogu jako pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-446">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="c77ff-447">Klucz jest nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="c77ff-447">The key is the file name.</span></span> <span data-ttu-id="c77ff-448">Wartość zawiera zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="c77ff-448">The value contains the file's contents.</span></span> <span data-ttu-id="c77ff-449">Dostawca konfiguracji klucza każdego pliku jest używany w scenariuszach hostingu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="c77ff-449">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="c77ff-450">Aby aktywować klucza dla pliku konfiguracji, należy wywołać <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-450">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="c77ff-451">`directoryPath` Do plików musi być ścieżką bezwzględną.</span><span class="sxs-lookup"><span data-stu-id="c77ff-451">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="c77ff-452">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c77ff-452">Overloads permit specifying:</span></span>

* <span data-ttu-id="c77ff-453">`Action<KeyPerFileConfigurationSource>` Delegat, który konfiguruje źródło.</span><span class="sxs-lookup"><span data-stu-id="c77ff-453">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="c77ff-454">Czy katalog jest opcjonalna i ścieżkę do katalogu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-454">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="c77ff-455">Podwójne podkreślenie (`__`) jest używany jako ogranicznika klucza konfiguracji w nazwach plików.</span><span class="sxs-lookup"><span data-stu-id="c77ff-455">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="c77ff-456">Na przykład, nazwa_pliku `Logging__LogLevel__System` generuje klucz konfiguracji `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-456">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="c77ff-457">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-457">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                var path = Path.Combine(
                    Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="c77ff-458">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-458">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c77ff-459">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-459">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c77ff-460">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c77ff-460">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="c77ff-461">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="c77ff-461">Memory Configuration Provider</span></span>

<span data-ttu-id="c77ff-462"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Korzysta z kolekcji w pamięci jako pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-462">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="c77ff-463">Aby aktywować konfiguracji kolekcji w pamięci, należy wywołać <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-463">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c77ff-464">Dostawca konfiguracji mogą być zainicjowane z `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-464">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="c77ff-465">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-465">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c77ff-466">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c77ff-466">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="c77ff-467">GetValue</span><span class="sxs-lookup"><span data-stu-id="c77ff-467">GetValue</span></span>

<span data-ttu-id="c77ff-468">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnianie wartości z konfiguracji z określonym kluczem i konwertuje je do określonego typu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-468">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="c77ff-469">Przeciążenie umożliwia podanie wartości domyślnej, jeśli nie odnaleziono klucza.</span><span class="sxs-lookup"><span data-stu-id="c77ff-469">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="c77ff-470">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="c77ff-470">The following example:</span></span>

* <span data-ttu-id="c77ff-471">Wyodrębnia wartość ciągu z konfiguracji przy użyciu klucza `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-471">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="c77ff-472">Jeśli `NumberKey` nie zostanie odnaleziona w klucze konfiguracji, wartość domyślna `99` jest używany.</span><span class="sxs-lookup"><span data-stu-id="c77ff-472">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="c77ff-473">Typy wartości jako `int`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-473">Types the value as an `int`.</span></span>
* <span data-ttu-id="c77ff-474">Przechowuje wartość w `NumberConfig` właściwości do użytku przez stronę.</span><span class="sxs-lookup"><span data-stu-id="c77ff-474">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="c77ff-475">GetSection, GetChildren i istnieje</span><span class="sxs-lookup"><span data-stu-id="c77ff-475">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="c77ff-476">Aby uzyskać przykłady, które należy wykonać należy wziąć pod uwagę następujący plik JSON.</span><span class="sxs-lookup"><span data-stu-id="c77ff-476">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="c77ff-477">Cztery klucze znajdują się na dwie sekcje, z których jedna zawiera parę podsekcje:</span><span class="sxs-lookup"><span data-stu-id="c77ff-477">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="c77ff-478">Gdy plik jest do odczytu do konfiguracji, następujące klucze unikatowe hierarchiczne są tworzone do przechowywania wartości konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-478">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="c77ff-479">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c77ff-479">section0:key0</span></span>
* <span data-ttu-id="c77ff-480">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c77ff-480">section0:key1</span></span>
* <span data-ttu-id="c77ff-481">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c77ff-481">section1:key0</span></span>
* <span data-ttu-id="c77ff-482">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c77ff-482">section1:key1</span></span>
* <span data-ttu-id="c77ff-483">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="c77ff-483">section2:subsection0:key0</span></span>
* <span data-ttu-id="c77ff-484">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="c77ff-484">section2:subsection0:key1</span></span>
* <span data-ttu-id="c77ff-485">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="c77ff-485">section2:subsection1:key0</span></span>
* <span data-ttu-id="c77ff-486">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="c77ff-486">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="c77ff-487">GetSection</span><span class="sxs-lookup"><span data-stu-id="c77ff-487">GetSection</span></span>

<span data-ttu-id="c77ff-488">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcji konfiguracji z kluczem określonym podsekcji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-488">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="c77ff-489">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-489">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c77ff-490">Aby zwrócić <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz wartość w `section1`, wywołaj `GetSection` i podaj nazwę sekcji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-490">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="c77ff-491">`configSection` Nie ma wartości, tylko klucz i ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="c77ff-491">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="c77ff-492">Podobnie Aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę do sekcji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-492">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="c77ff-493">`GetSection` nigdy nie zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-493">`GetSection` never returns `null`.</span></span> <span data-ttu-id="c77ff-494">Jeśli nie zostanie znaleziona pasująca sekcja, pusta `IConfigurationSection` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="c77ff-494">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="c77ff-495">Gdy `GetSection` zwraca pasujących sekcji <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione.</span><span class="sxs-lookup"><span data-stu-id="c77ff-495">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="c77ff-496">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> i <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> są zwracane, gdy istnieje sekcji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-496">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="c77ff-497">GetChildren</span><span class="sxs-lookup"><span data-stu-id="c77ff-497">GetChildren</span></span>

<span data-ttu-id="c77ff-498">Wywołanie [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) na `section2` uzyskuje `IEnumerable<IConfigurationSection>` zawierającej:</span><span class="sxs-lookup"><span data-stu-id="c77ff-498">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="c77ff-499">Exists</span><span class="sxs-lookup"><span data-stu-id="c77ff-499">Exists</span></span>

<span data-ttu-id="c77ff-500">Użyj [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) do ustalenia, czy istnieje sekcji konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-500">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="c77ff-501">Podane dane przykładowe `sectionExists` jest `false` , ponieważ nie ma `section2:subsection2` sekcji w danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-501">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="c77ff-502">Powiąż z klasy</span><span class="sxs-lookup"><span data-stu-id="c77ff-502">Bind to a class</span></span>

<span data-ttu-id="c77ff-503">Konfiguracja może być powiązana z klas, które reprezentują grupy powiązane ustawienia za pomocą *wzorzec opcje*.</span><span class="sxs-lookup"><span data-stu-id="c77ff-503">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="c77ff-504">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-504">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="c77ff-505">Wartości konfiguracji są zwracane jako ciągi, ale wywoływania <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) obiektów.</span><span class="sxs-lookup"><span data-stu-id="c77ff-505">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="c77ff-506">`Bind` Trwa [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-506">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c77ff-507">Przykładowa aplikacja zawiera `Starship` modelu (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="c77ff-507">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="c77ff-508">`starship` Części *starship.json* plików umożliwia utworzenie konfiguracji podczas Przykładowa aplikacja używa dostawcy konfiguracji JSON można załadować konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-508">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="c77ff-509">Tworzone są następujące pary klucz wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-509">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="c77ff-510">Key</span><span class="sxs-lookup"><span data-stu-id="c77ff-510">Key</span></span>                   | <span data-ttu-id="c77ff-511">Wartość</span><span class="sxs-lookup"><span data-stu-id="c77ff-511">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="c77ff-512">starship: Nazwa</span><span class="sxs-lookup"><span data-stu-id="c77ff-512">starship:name</span></span>         | <span data-ttu-id="c77ff-513">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="c77ff-513">USS Enterprise</span></span>                                    |
| <span data-ttu-id="c77ff-514">starship: rejestru</span><span class="sxs-lookup"><span data-stu-id="c77ff-514">starship:registry</span></span>     | <span data-ttu-id="c77ff-515">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="c77ff-515">NCC-1701</span></span>                                          |
| <span data-ttu-id="c77ff-516">starship: klasy</span><span class="sxs-lookup"><span data-stu-id="c77ff-516">starship:class</span></span>        | <span data-ttu-id="c77ff-517">Tworzenia</span><span class="sxs-lookup"><span data-stu-id="c77ff-517">Constitution</span></span>                                      |
| <span data-ttu-id="c77ff-518">starship: długość</span><span class="sxs-lookup"><span data-stu-id="c77ff-518">starship:length</span></span>       | <span data-ttu-id="c77ff-519">304.8</span><span class="sxs-lookup"><span data-stu-id="c77ff-519">304.8</span></span>                                             |
| <span data-ttu-id="c77ff-520">starship: upoważnione</span><span class="sxs-lookup"><span data-stu-id="c77ff-520">starship:commissioned</span></span> | <span data-ttu-id="c77ff-521">False</span><span class="sxs-lookup"><span data-stu-id="c77ff-521">False</span></span>                                             |
| <span data-ttu-id="c77ff-522">Znak towarowy</span><span class="sxs-lookup"><span data-stu-id="c77ff-522">trademark</span></span>             | <span data-ttu-id="c77ff-523">Paramount obrazy Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="c77ff-523">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="c77ff-524">Wywołania aplikacji przykładowej `GetSection` z `starship` klucza.</span><span class="sxs-lookup"><span data-stu-id="c77ff-524">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="c77ff-525">`starship` Pary klucz wartość są izolowane.</span><span class="sxs-lookup"><span data-stu-id="c77ff-525">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="c77ff-526">`Bind` Metoda jest wywoływana w podsekcji, przekazując wystąpienie `Starship` klasy.</span><span class="sxs-lookup"><span data-stu-id="c77ff-526">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="c77ff-527">Po powiązaniu wartości wystąpienia, wystąpienie jest przypisany do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="c77ff-527">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="c77ff-528">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-528">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="c77ff-529">Powiąż z wykresu obiektu</span><span class="sxs-lookup"><span data-stu-id="c77ff-529">Bind to an object graph</span></span>

<span data-ttu-id="c77ff-530"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> jest w stanie całego grafu obiektów POCO powiązania.</span><span class="sxs-lookup"><span data-stu-id="c77ff-530"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="c77ff-531">`Bind` Trwa [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-531">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c77ff-532">Przykładowy zawiera `TvShow` model zawiera wykres obiektu, którego `Metadata` i `Actors` klasy (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="c77ff-532">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="c77ff-533">Przykładowa aplikacja ma *tvshow.xml* plik zawierający dane konfiguracyjne:</span><span class="sxs-lookup"><span data-stu-id="c77ff-533">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="c77ff-534">Konfiguracja jest powiązany z całej `TvShow` wykres obiektu z `Bind` metody.</span><span class="sxs-lookup"><span data-stu-id="c77ff-534">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="c77ff-535">Powiązane wystąpienia jest przypisywany do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="c77ff-535">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="c77ff-536">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) wiąże i zwraca wartość określonego typu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-536">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="c77ff-537">`Get<T>` jest bardziej wygodne niż przy użyciu `Bind`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-537">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="c77ff-538">Poniższy kod przedstawia sposób użycia `Get<T>` w poprzednim przykładzie, co umożliwia wystąpienie powiązanych, którego można przypisać bezpośrednio do właściwości, używany do renderowania:</span><span class="sxs-lookup"><span data-stu-id="c77ff-538">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="c77ff-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> Trwa [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="c77ff-540">`Get<T>` jest dostępna w programie ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="c77ff-540">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="c77ff-541">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-541">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="c77ff-542">Powiąż tablicę do klasy</span><span class="sxs-lookup"><span data-stu-id="c77ff-542">Bind an array to a class</span></span>

<span data-ttu-id="c77ff-543">*Przykładowa aplikacja pokazuje pojęcia opisane w tej sekcji.*</span><span class="sxs-lookup"><span data-stu-id="c77ff-543">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="c77ff-544"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Obsługuje tablice powiązania do obiektów przy użyciu tablicy indeksów w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-544">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="c77ff-545">Dowolnym formacie tablicy, który udostępnia liczbowych segment klucza (`:0:`, `:1:`, &hellip; `:{n}:`) jest w stanie tablicy powiązania z macierzą POCO klasy.</span><span class="sxs-lookup"><span data-stu-id="c77ff-545">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="c77ff-546">"Związać" znajduje się w [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-546">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="c77ff-547">Powiązanie jest zapewniana przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="c77ff-547">Binding is provided by convention.</span></span> <span data-ttu-id="c77ff-548">Konfiguracja niestandardowa dostawców nie są wymagane do zaimplementowania tablicy powiązania.</span><span class="sxs-lookup"><span data-stu-id="c77ff-548">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="c77ff-549">**Przetwarzanie w pamięci tablica**</span><span class="sxs-lookup"><span data-stu-id="c77ff-549">**In-memory array processing**</span></span>

<span data-ttu-id="c77ff-550">Należy wziąć pod uwagę kluczy i wartości konfiguracji pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c77ff-550">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="c77ff-551">Key</span><span class="sxs-lookup"><span data-stu-id="c77ff-551">Key</span></span>             | <span data-ttu-id="c77ff-552">Wartość</span><span class="sxs-lookup"><span data-stu-id="c77ff-552">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="c77ff-553">Macierz: wpisów: 0</span><span class="sxs-lookup"><span data-stu-id="c77ff-553">array:entries:0</span></span> | <span data-ttu-id="c77ff-554">value0</span><span class="sxs-lookup"><span data-stu-id="c77ff-554">value0</span></span> |
| <span data-ttu-id="c77ff-555">Macierz: wpisów: 1</span><span class="sxs-lookup"><span data-stu-id="c77ff-555">array:entries:1</span></span> | <span data-ttu-id="c77ff-556">value1</span><span class="sxs-lookup"><span data-stu-id="c77ff-556">value1</span></span> |
| <span data-ttu-id="c77ff-557">Macierz: wpisów: 2</span><span class="sxs-lookup"><span data-stu-id="c77ff-557">array:entries:2</span></span> | <span data-ttu-id="c77ff-558">value2</span><span class="sxs-lookup"><span data-stu-id="c77ff-558">value2</span></span> |
| <span data-ttu-id="c77ff-559">Macierz: wpisów: 4</span><span class="sxs-lookup"><span data-stu-id="c77ff-559">array:entries:4</span></span> | <span data-ttu-id="c77ff-560">value4</span><span class="sxs-lookup"><span data-stu-id="c77ff-560">value4</span></span> |
| <span data-ttu-id="c77ff-561">Macierz: wpisów: 5</span><span class="sxs-lookup"><span data-stu-id="c77ff-561">array:entries:5</span></span> | <span data-ttu-id="c77ff-562">value5</span><span class="sxs-lookup"><span data-stu-id="c77ff-562">value5</span></span> |

<span data-ttu-id="c77ff-563">Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:</span><span class="sxs-lookup"><span data-stu-id="c77ff-563">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

<span data-ttu-id="c77ff-564">Tablica pomija wartość indeksu &num;3.</span><span class="sxs-lookup"><span data-stu-id="c77ff-564">The array skips a value for index &num;3.</span></span> <span data-ttu-id="c77ff-565">Konfiguracja obiektu wiążącego nie jest zdolny do powiązania wartości null lub tworzenie wartości null wpisów w powiązanych obiektów, które stają się w chwili, gdy przedstawiono wynik powiązania tej tablicy do obiektu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-565">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="c77ff-566">W przykładowej aplikacji do przechowywania danych konfiguracji powiązanej dostępnej klasy POCO:</span><span class="sxs-lookup"><span data-stu-id="c77ff-566">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="c77ff-567">Dane konfiguracji jest powiązana z obiektem:</span><span class="sxs-lookup"><span data-stu-id="c77ff-567">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="c77ff-568">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c77ff-568">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c77ff-569">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) składni można również użyć, która skutkuje bardziej kompaktowy kod:</span><span class="sxs-lookup"><span data-stu-id="c77ff-569">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="c77ff-570">Powiązany obiekt, wystąpienie `ArrayExample`, odbiera dane tablicy z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-570">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="c77ff-571">`ArrayExample.Entries` Indeks</span><span class="sxs-lookup"><span data-stu-id="c77ff-571">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="c77ff-572">`ArrayExample.Entries` Wartość</span><span class="sxs-lookup"><span data-stu-id="c77ff-572">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="c77ff-573">0</span><span class="sxs-lookup"><span data-stu-id="c77ff-573">0</span></span>                            | <span data-ttu-id="c77ff-574">value0</span><span class="sxs-lookup"><span data-stu-id="c77ff-574">value0</span></span>                       |
| <span data-ttu-id="c77ff-575">1</span><span class="sxs-lookup"><span data-stu-id="c77ff-575">1</span></span>                            | <span data-ttu-id="c77ff-576">value1</span><span class="sxs-lookup"><span data-stu-id="c77ff-576">value1</span></span>                       |
| <span data-ttu-id="c77ff-577">2</span><span class="sxs-lookup"><span data-stu-id="c77ff-577">2</span></span>                            | <span data-ttu-id="c77ff-578">value2</span><span class="sxs-lookup"><span data-stu-id="c77ff-578">value2</span></span>                       |
| <span data-ttu-id="c77ff-579">3</span><span class="sxs-lookup"><span data-stu-id="c77ff-579">3</span></span>                            | <span data-ttu-id="c77ff-580">value4</span><span class="sxs-lookup"><span data-stu-id="c77ff-580">value4</span></span>                       |
| <span data-ttu-id="c77ff-581">4</span><span class="sxs-lookup"><span data-stu-id="c77ff-581">4</span></span>                            | <span data-ttu-id="c77ff-582">value5</span><span class="sxs-lookup"><span data-stu-id="c77ff-582">value5</span></span>                       |

<span data-ttu-id="c77ff-583">Indeks &num;3 w powiązany obiekt przechowuje dane konfiguracyjne `array:4` klucz konfiguracji i jego wartość `value4`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-583">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="c77ff-584">Gdy dane konfiguracji, które zawiera tablicę jest powiązana, indeksy tablicy w klucze konfiguracji jedynie służą do iteracji dane konfiguracji, podczas tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="c77ff-584">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="c77ff-585">Wartość null nie mogą być przechowywane w danych konfiguracji, a wpis o wartości null nie jest tworzona w powiązany obiekt, w przypadku tablicy w konfiguracji kluczy Pomiń jeden lub więcej indeksów.</span><span class="sxs-lookup"><span data-stu-id="c77ff-585">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="c77ff-586">Brak elementu konfiguracji dla indeksu &num;3 mogą być dostarczane przed powiązanie `ArrayExample` wystąpienie przez dowolnego dostawcę konfiguracji, tworzącego prawidłowe pary klucz wartość w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-586">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="c77ff-587">Jeśli przykład uwzględniony dodatkowego dostawcę konfiguracji JSON z Brak pary klucz wartość `ArrayExample.Entries` pasuje do tablicy kompletna konfiguracja:</span><span class="sxs-lookup"><span data-stu-id="c77ff-587">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="c77ff-588">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="c77ff-588">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="c77ff-589">W <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="c77ff-589">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="c77ff-590">Pary klucz wartość, jak pokazano w tabeli są ładowane do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-590">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="c77ff-591">Key</span><span class="sxs-lookup"><span data-stu-id="c77ff-591">Key</span></span>             | <span data-ttu-id="c77ff-592">Wartość</span><span class="sxs-lookup"><span data-stu-id="c77ff-592">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="c77ff-593">Macierz: wpisów: 3</span><span class="sxs-lookup"><span data-stu-id="c77ff-593">array:entries:3</span></span> | <span data-ttu-id="c77ff-594">value3</span><span class="sxs-lookup"><span data-stu-id="c77ff-594">value3</span></span> |

<span data-ttu-id="c77ff-595">Jeśli `ArrayExample` wystąpienia klasy jest powiązana po dostawcę konfiguracji JSON zawiera wpis dla indeksu &num;3, `ArrayExample.Entries` tablicy zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="c77ff-595">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="c77ff-596">`ArrayExample.Entries` Indeks</span><span class="sxs-lookup"><span data-stu-id="c77ff-596">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="c77ff-597">`ArrayExample.Entries` Wartość</span><span class="sxs-lookup"><span data-stu-id="c77ff-597">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="c77ff-598">0</span><span class="sxs-lookup"><span data-stu-id="c77ff-598">0</span></span>                            | <span data-ttu-id="c77ff-599">value0</span><span class="sxs-lookup"><span data-stu-id="c77ff-599">value0</span></span>                       |
| <span data-ttu-id="c77ff-600">1</span><span class="sxs-lookup"><span data-stu-id="c77ff-600">1</span></span>                            | <span data-ttu-id="c77ff-601">value1</span><span class="sxs-lookup"><span data-stu-id="c77ff-601">value1</span></span>                       |
| <span data-ttu-id="c77ff-602">2</span><span class="sxs-lookup"><span data-stu-id="c77ff-602">2</span></span>                            | <span data-ttu-id="c77ff-603">value2</span><span class="sxs-lookup"><span data-stu-id="c77ff-603">value2</span></span>                       |
| <span data-ttu-id="c77ff-604">3</span><span class="sxs-lookup"><span data-stu-id="c77ff-604">3</span></span>                            | <span data-ttu-id="c77ff-605">value3</span><span class="sxs-lookup"><span data-stu-id="c77ff-605">value3</span></span>                       |
| <span data-ttu-id="c77ff-606">4</span><span class="sxs-lookup"><span data-stu-id="c77ff-606">4</span></span>                            | <span data-ttu-id="c77ff-607">value4</span><span class="sxs-lookup"><span data-stu-id="c77ff-607">value4</span></span>                       |
| <span data-ttu-id="c77ff-608">5</span><span class="sxs-lookup"><span data-stu-id="c77ff-608">5</span></span>                            | <span data-ttu-id="c77ff-609">value5</span><span class="sxs-lookup"><span data-stu-id="c77ff-609">value5</span></span>                       |

<span data-ttu-id="c77ff-610">**Przetwarzanie tablicy JSON**</span><span class="sxs-lookup"><span data-stu-id="c77ff-610">**JSON array processing**</span></span>

<span data-ttu-id="c77ff-611">Jeśli plik JSON zawiera tablicę, klucze konfiguracji zostaną utworzone dla elementów tablicy przy użyciu indeksu zaczynającego się od zera sekcji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-611">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="c77ff-612">W następującym pliku konfiguracji `subsection` jest tablicą:</span><span class="sxs-lookup"><span data-stu-id="c77ff-612">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="c77ff-613">Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz wartość:</span><span class="sxs-lookup"><span data-stu-id="c77ff-613">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="c77ff-614">Key</span><span class="sxs-lookup"><span data-stu-id="c77ff-614">Key</span></span>                     | <span data-ttu-id="c77ff-615">Wartość</span><span class="sxs-lookup"><span data-stu-id="c77ff-615">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="c77ff-616">json_array:key</span><span class="sxs-lookup"><span data-stu-id="c77ff-616">json_array:key</span></span>          | <span data-ttu-id="c77ff-617">valueA</span><span class="sxs-lookup"><span data-stu-id="c77ff-617">valueA</span></span> |
| <span data-ttu-id="c77ff-618">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="c77ff-618">json_array:subsection:0</span></span> | <span data-ttu-id="c77ff-619">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="c77ff-619">valueB</span></span> |
| <span data-ttu-id="c77ff-620">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="c77ff-620">json_array:subsection:1</span></span> | <span data-ttu-id="c77ff-621">valueC</span><span class="sxs-lookup"><span data-stu-id="c77ff-621">valueC</span></span> |
| <span data-ttu-id="c77ff-622">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="c77ff-622">json_array:subsection:2</span></span> | <span data-ttu-id="c77ff-623">valueD</span><span class="sxs-lookup"><span data-stu-id="c77ff-623">valueD</span></span> |

<span data-ttu-id="c77ff-624">W przykładowej aplikacji następującej klasy POCO jest dostępny dla powiązania pary klucz wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c77ff-624">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="c77ff-625">Po powiązaniu `JsonArrayExample.Key` przechowuje wartość `valueA`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-625">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="c77ff-626">Wartości podsekcji są przechowywane we właściwości tablicy POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-626">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="c77ff-627">`JsonArrayExample.Subsection` Indeks</span><span class="sxs-lookup"><span data-stu-id="c77ff-627">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="c77ff-628">`JsonArrayExample.Subsection` Wartość</span><span class="sxs-lookup"><span data-stu-id="c77ff-628">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="c77ff-629">0</span><span class="sxs-lookup"><span data-stu-id="c77ff-629">0</span></span>                                   | <span data-ttu-id="c77ff-630">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="c77ff-630">valueB</span></span>                              |
| <span data-ttu-id="c77ff-631">1</span><span class="sxs-lookup"><span data-stu-id="c77ff-631">1</span></span>                                   | <span data-ttu-id="c77ff-632">valueC</span><span class="sxs-lookup"><span data-stu-id="c77ff-632">valueC</span></span>                              |
| <span data-ttu-id="c77ff-633">2</span><span class="sxs-lookup"><span data-stu-id="c77ff-633">2</span></span>                                   | <span data-ttu-id="c77ff-634">valueD</span><span class="sxs-lookup"><span data-stu-id="c77ff-634">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="c77ff-635">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="c77ff-635">Custom configuration provider</span></span>

<span data-ttu-id="c77ff-636">Przykładowa aplikacja pokazuje, jak utworzyć dostawcę konfiguracji podstawowej, który odczytuje pary klucz wartość konfiguracji z bazy danych za pomocą [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="c77ff-636">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="c77ff-637">Dostawcę ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="c77ff-637">The provider has the following characteristics:</span></span>

* <span data-ttu-id="c77ff-638">Bazy danych w pamięci programu EF jest używana do celów demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="c77ff-638">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="c77ff-639">Aby użyć bazy danych, która wymaga parametrów połączenia, należy zaimplementować pomocniczy `ConfigurationBuilder` podawać parametrów połączenia z innego dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-639">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="c77ff-640">Dostawca odczytuje tabelę bazy danych do konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="c77ff-640">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="c77ff-641">Dostawca nie kwerendy bazy danych na podstawie-key.</span><span class="sxs-lookup"><span data-stu-id="c77ff-641">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="c77ff-642">Załaduj ponownie przy zmianie nie jest zaimplementowany, więc zaktualizowanie bazy danych, po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c77ff-642">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="c77ff-643">Zdefiniuj `EFConfigurationValue` jednostki do przechowywania wartości konfiguracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c77ff-643">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="c77ff-644">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="c77ff-644">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="c77ff-645">Dodaj `EFConfigurationContext` do przechowywania i uzyskiwania dostępu skonfigurowane wartości.</span><span class="sxs-lookup"><span data-stu-id="c77ff-645">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="c77ff-646">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="c77ff-646">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="c77ff-647">Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-647">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="c77ff-648">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="c77ff-648">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="c77ff-649">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-649">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="c77ff-650">Dostawca konfiguracji inicjuje bazy danych, gdy jest on pusty.</span><span class="sxs-lookup"><span data-stu-id="c77ff-650">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="c77ff-651">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="c77ff-651">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="c77ff-652">`AddEFConfiguration` Metody rozszerzenia, który pozwala na dodawanie źródła konfiguracji do `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-652">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="c77ff-653">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="c77ff-653">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="c77ff-654">Poniższy kod przedstawia sposób używania niestandardowego `EFConfigurationProvider` w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c77ff-654">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="c77ff-655">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="c77ff-655">Access configuration during startup</span></span>

<span data-ttu-id="c77ff-656">Wstrzykiwanie `IConfiguration` do `Startup` konstruktora do dostępu do wartości konfiguracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c77ff-656">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c77ff-657">Uzyskać dostępu do konfiguracji w `Startup.Configure`, albo wstrzyknąć `IConfiguration` bezpośrednio do metody lub użyj wystąpienia z konstruktora:</span><span class="sxs-lookup"><span data-stu-id="c77ff-657">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="c77ff-658">Na przykład uzyskiwania dostępu do konfiguracji za pomocą metod jako udogodnienie uruchamiania zobacz [uruchamiania aplikacji: Wygodne metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="c77ff-658">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="c77ff-659">Konfiguracja dostępu na stronie stron Razor lub w widoku MVC</span><span class="sxs-lookup"><span data-stu-id="c77ff-659">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="c77ff-660">Aby uzyskać dostęp do ustawień konfiguracji na stronie stron Razor lub widoku MVC, należy dodać [użycie dyrektywy](xref:mvc/views/razor#using) ([odwołanie w C#: using — dyrektywa](/dotnet/csharp/language-reference/keywords/using-directive)) dla [Microsoft.Extensions.Configuration przestrzeni nazw ](xref:Microsoft.Extensions.Configuration) i wstawiać <xref:Microsoft.Extensions.Configuration.IConfiguration> do strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="c77ff-660">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="c77ff-661">Na stronie stron Razor:</span><span class="sxs-lookup"><span data-stu-id="c77ff-661">In a Razor Pages page:</span></span>

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

<span data-ttu-id="c77ff-662">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="c77ff-662">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="c77ff-663">Dodaj konfigurację z zewnętrznego zestawu</span><span class="sxs-lookup"><span data-stu-id="c77ff-663">Add configuration from an external assembly</span></span>

<span data-ttu-id="c77ff-664"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Implementacji umożliwia dodawanie rozszerzeń do aplikacji przy uruchamianiu z zewnętrznego zestawu poza aplikacji `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="c77ff-664">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="c77ff-665">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c77ff-665">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c77ff-666">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c77ff-666">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="c77ff-667">Głębszej analizy konfiguracji firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="c77ff-667">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
