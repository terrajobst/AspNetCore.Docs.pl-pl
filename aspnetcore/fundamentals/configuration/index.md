---
title: Konfiguracja w ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core przy użyciu interfejsu API konfiguracji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 3dcabae3f76d81e641057c346dbb9097c2da42c7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656332"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="e9ea7-103">Konfiguracja w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9ea7-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="e9ea7-104">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e9ea7-105">Konfiguracja aplikacji w ASP.NET Core jest oparta na parach klucz-wartość określonych przez *dostawców konfiguracji*.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="e9ea7-106">Dostawcy konfiguracji odczytują dane konfiguracji do par klucz-wartość z różnych źródeł konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="e9ea7-107">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e9ea7-107">Azure Key Vault</span></span>
* <span data-ttu-id="e9ea7-108">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="e9ea7-108">Azure App Configuration</span></span>
* <span data-ttu-id="e9ea7-109">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-109">Command-line arguments</span></span>
* <span data-ttu-id="e9ea7-110">Dostawcy niestandardowi (instalowani lub utworzony)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="e9ea7-111">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="e9ea7-111">Directory files</span></span>
* <span data-ttu-id="e9ea7-112">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="e9ea7-112">Environment variables</span></span>
* <span data-ttu-id="e9ea7-113">Obiekty w pamięci .NET</span><span class="sxs-lookup"><span data-stu-id="e9ea7-113">In-memory .NET objects</span></span>
* <span data-ttu-id="e9ea7-114">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="e9ea7-114">Settings files</span></span>

<span data-ttu-id="e9ea7-115">Pakiety konfiguracyjne dla typowych scenariuszy dostawcy konfiguracji ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) są dołączone niejawnie przez platformę.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

<span data-ttu-id="e9ea7-116">Przykłady kodu, które obserwują i w przykładowej aplikacji używają przestrzeni nazw <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-116">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="e9ea7-117">*Wzorzec opcji* jest rozszerzeniem pojęć konfiguracyjnych opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-117">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="e9ea7-118">Opcje używają klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-118">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="e9ea7-119">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-119">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e9ea7-120">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e9ea7-120">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="e9ea7-121">Host a konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-121">Host versus app configuration</span></span>

<span data-ttu-id="e9ea7-122">Przed skonfigurowaniem i uruchomieniem aplikacji *host* zostanie skonfigurowany i uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-122">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="e9ea7-123">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-123">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="e9ea7-124">Zarówno aplikacja, jak i Host są konfigurowane przy użyciu dostawców konfiguracji opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-124">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="e9ea7-125">Klucz konfiguracji hosta — pary wartości są również uwzględnione w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-125">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="e9ea7-126">Aby uzyskać więcej informacji na temat tego, jak dostawcy konfiguracji są używani podczas kompilowania hosta i jak źródła konfiguracji wpływają na konfigurację hosta, zobacz <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-126">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="e9ea7-127">Inna konfiguracja</span><span class="sxs-lookup"><span data-stu-id="e9ea7-127">Other configuration</span></span>

<span data-ttu-id="e9ea7-128">Ten temat dotyczy tylko *konfiguracji aplikacji*.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-128">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="e9ea7-129">Inne aspekty uruchamiania i hostowania aplikacji ASP.NET Core są konfigurowane przy użyciu plików konfiguracji nieuwzględnionych w tym temacie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-129">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="e9ea7-130">plik *Launch. json*/*profilu launchsettings. JSON* to pliki konfiguracji narzędzi dla środowiska programistycznego, opisane w temacie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-130">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="e9ea7-131">W <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-131">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="e9ea7-132">W zestawie dokumentacji, w której pliki są używane do konfigurowania ASP.NET Core aplikacji na potrzeby scenariuszy programistycznych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-132">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="e9ea7-133">*Web. config* to plik konfiguracji serwera opisany w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-133">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="e9ea7-134">Aby uzyskać więcej informacji na temat migrowania konfiguracji aplikacji z wcześniejszych wersji programu ASP.NET, zobacz <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-134">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="e9ea7-135">Konfiguracja domyślna</span><span class="sxs-lookup"><span data-stu-id="e9ea7-135">Default configuration</span></span>

<span data-ttu-id="e9ea7-136">Aplikacje sieci Web oparte na ASP.NET Core [nowym szablonów dotnet](/dotnet/core/tools/dotnet-new) <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-136">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="e9ea7-137">`CreateDefaultBuilder` zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-137">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="e9ea7-138">Poniższe zasady dotyczą aplikacji korzystających z [hosta ogólnego](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-138">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="e9ea7-139">Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej podczas korzystania z [hosta sieci Web](xref:fundamentals/host/web-host), zobacz [wersję ASP.NET Core 2,2 tego tematu](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-139">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="e9ea7-140">Konfiguracja hosta jest poświadczona z:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="e9ea7-141">Zmienne środowiskowe poprzedzone prefiksem `DOTNET_` (na przykład `DOTNET_ENVIRONMENT`) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-141">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="e9ea7-142">Prefiks (`DOTNET_`) jest usuwany podczas ładowania par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-142">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="e9ea7-143">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-143">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="e9ea7-144">Konfiguracja domyślna hosta sieci Web (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="e9ea7-144">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="e9ea7-145">Kestrel jest używany jako serwer sieci Web i konfigurowany przy użyciu dostawców konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-145">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="e9ea7-146">Dodaj oprogramowanie pośredniczące do filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-146">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="e9ea7-147">Dodaj przekierowane nagłówki — oprogramowanie pośredniczące, jeśli zmienna środowiskowa `ASPNETCORE_FORWARDEDHEADERS_ENABLED` jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-147">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="e9ea7-148">Włącz integrację usług IIS.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-148">Enable IIS integration.</span></span>
* <span data-ttu-id="e9ea7-149">Podano konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-149">App configuration is provided from:</span></span>
  * <span data-ttu-id="e9ea7-150">*appSettings. JSON* przy użyciu [dostawcy konfiguracji plików](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-150">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e9ea7-151">*appSettings. {Environment}. JSON* przy użyciu [dostawcy konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-151">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e9ea7-152">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development` przy użyciu zestawu wpisów.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-152">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="e9ea7-153">Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-153">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="e9ea7-154">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-154">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="e9ea7-155">Bezpieczeństwo</span><span class="sxs-lookup"><span data-stu-id="e9ea7-155">Security</span></span>

<span data-ttu-id="e9ea7-156">Aby zabezpieczyć poufne dane konfiguracji, należy zastosować następujące rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-156">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="e9ea7-157">Nie należy przechowywać haseł ani innych danych poufnych w kodzie dostawcy konfiguracji ani w plikach konfiguracji zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-157">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="e9ea7-158">Nie używaj tajemnic produkcyjnych w środowiskach deweloperskich i testowych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-158">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="e9ea7-159">Określ wpisy tajne poza projektem, aby nie mogły zostać przypadkowo przekazane do repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="e9ea7-160">Aby uzyskać więcej informacji, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-160">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="e9ea7-161"><xref:security/app-secrets> &ndash; zawiera porady dotyczące używania zmiennych środowiskowych do przechowywania poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-161"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="e9ea7-162">Menedżer wpisów tajnych używa dostawcy konfiguracji plików do przechowywania wpisów tajnych użytkownika w pliku JSON w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-162">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="e9ea7-163">Dostawca konfiguracji plików został opisany w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-163">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="e9ea7-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) bezpieczne przechowywanie wpisów tajnych aplikacji dla ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="e9ea7-165">Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-165">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="e9ea7-166">Hierarchiczne dane konfiguracji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-166">Hierarchical configuration data</span></span>

<span data-ttu-id="e9ea7-167">Interfejs API konfiguracji jest w stanie utrzymywać hierarchiczne dane konfiguracji przez spłaszczonie danych hierarchicznych przy użyciu ogranicznika w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-167">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="e9ea7-168">W poniższym pliku JSON cztery klucze istnieją w hierarchii strukturalnej dwóch sekcji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-168">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="e9ea7-169">Gdy plik jest odczytywany do konfiguracji, są tworzone unikatowe klucze, aby zachować oryginalną hierarchiczną strukturę danych źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-169">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="e9ea7-170">Sekcje i klucze są spłaszczone przy użyciu dwukropka (`:`), aby zachować oryginalną strukturę:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-170">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="e9ea7-171">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-171">section0:key0</span></span>
* <span data-ttu-id="e9ea7-172">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-172">section0:key1</span></span>
* <span data-ttu-id="e9ea7-173">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-173">section1:key0</span></span>
* <span data-ttu-id="e9ea7-174">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-174">section1:key1</span></span>

<span data-ttu-id="e9ea7-175">Metody <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> są dostępne do izolowania sekcji i elementów podrzędnych sekcji w danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-175"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="e9ea7-176">Te metody są opisane w dalszej [części GetSection, GetChildren i EXISTS](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-176">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="e9ea7-177">Konwencja</span><span class="sxs-lookup"><span data-stu-id="e9ea7-177">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="e9ea7-178">Źródła i dostawcy</span><span class="sxs-lookup"><span data-stu-id="e9ea7-178">Sources and providers</span></span>

<span data-ttu-id="e9ea7-179">Podczas uruchamiania aplikacji źródła konfiguracji są odczytywane w kolejności, w jakiej są określone przez ich dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-179">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="e9ea7-180">Dostawcy konfiguracji implementujący funkcję wykrywania zmian mają możliwość ponownego załadowania konfiguracji, gdy ustawienie podstawowe zostanie zmienione.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-180">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="e9ea7-181">Na przykład dostawca konfiguracji plików (opisany w dalszej części tego tematu) i [dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) implementują wykrywanie zmian.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-181">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="e9ea7-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> jest dostępny w kontenerze [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e9ea7-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> można wstrzyknąć do Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> lub MVC <xref:Microsoft.AspNetCore.Mvc.Controller>, aby uzyskać konfigurację dla klasy.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="e9ea7-184">W poniższych przykładach pole `_config` służy do uzyskiwania dostępu do wartości konfiguracyjnych:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-184">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="e9ea7-185">Dostawcy konfiguracji nie mogą używać DI, ponieważ są niedostępne, gdy są skonfigurowane przez hosta.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-185">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="e9ea7-186">Klucze</span><span class="sxs-lookup"><span data-stu-id="e9ea7-186">Keys</span></span>

<span data-ttu-id="e9ea7-187">Klucze konfiguracji przyjmują następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-187">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="e9ea7-188">W kluczach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-188">Keys are case-insensitive.</span></span> <span data-ttu-id="e9ea7-189">Na przykład `ConnectionString` i `connectionstring` są traktowane jako klucze równoważne.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-189">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="e9ea7-190">Jeśli wartość tego samego klucza jest ustawiana przez tych samych lub różnych dostawców konfiguracji, Ostatnia wartość ustawiona w tym kluczu jest używana.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-190">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="e9ea7-191">Klucze hierarchiczne</span><span class="sxs-lookup"><span data-stu-id="e9ea7-191">Hierarchical keys</span></span>
  * <span data-ttu-id="e9ea7-192">W interfejsie API konfiguracji, separator dwukropek (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-192">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="e9ea7-193">W zmiennych środowiskowych separator dwukropek może nie zadziałał na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-193">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="e9ea7-194">Podwójne podkreślenie (`__`) jest obsługiwane przez wszystkie platformy i automatycznie konwertowane na dwukropek.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-194">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="e9ea7-195">W Azure Key Vault klucze hierarchiczne używają `--` (dwóch kresek) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-195">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="e9ea7-196">Napisz kod, aby zastąpić łączniki dwukropkiem, gdy wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-196">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="e9ea7-197"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-197">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e9ea7-198">Powiązanie tablicowe zostało opisane w sekcji [Powiązywanie tablicy z klasą](#bind-an-array-to-a-class) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-198">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="e9ea7-199">Wartości</span><span class="sxs-lookup"><span data-stu-id="e9ea7-199">Values</span></span>

<span data-ttu-id="e9ea7-200">Wartości konfiguracyjne przyjmują następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-200">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="e9ea7-201">Wartości są ciągami.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-201">Values are strings.</span></span>
* <span data-ttu-id="e9ea7-202">Wartości null nie można przechowywać w konfiguracji ani powiązana z obiektami.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-202">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="e9ea7-203">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="e9ea7-203">Providers</span></span>

<span data-ttu-id="e9ea7-204">W poniższej tabeli przedstawiono dostawców konfiguracji dostępnych do ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-204">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="e9ea7-205">Dostawca</span><span class="sxs-lookup"><span data-stu-id="e9ea7-205">Provider</span></span> | <span data-ttu-id="e9ea7-206">Zapewnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="e9ea7-206">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="e9ea7-207">[Dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) (tematy dotyczące*zabezpieczeń* )</span><span class="sxs-lookup"><span data-stu-id="e9ea7-207">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="e9ea7-208">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e9ea7-208">Azure Key Vault</span></span> |
| <span data-ttu-id="e9ea7-209">[Dostawca konfiguracji aplikacji platformy Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (dokumentacja platformy Azure)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-209">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="e9ea7-210">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="e9ea7-210">Azure App Configuration</span></span> |
| [<span data-ttu-id="e9ea7-211">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="e9ea7-212">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-212">Command-line parameters</span></span> |
| [<span data-ttu-id="e9ea7-213">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="e9ea7-214">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="e9ea7-214">Custom source</span></span> |
| [<span data-ttu-id="e9ea7-215">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="e9ea7-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="e9ea7-216">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="e9ea7-216">Environment variables</span></span> |
| [<span data-ttu-id="e9ea7-217">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="e9ea7-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="e9ea7-218">Pliki (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="e9ea7-219">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="e9ea7-219">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="e9ea7-220">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="e9ea7-220">Directory files</span></span> |
| [<span data-ttu-id="e9ea7-221">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="e9ea7-221">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="e9ea7-222">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="e9ea7-222">In-memory collections</span></span> |
| <span data-ttu-id="e9ea7-223">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tematy dotyczące*zabezpieczeń* )</span><span class="sxs-lookup"><span data-stu-id="e9ea7-223">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="e9ea7-224">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="e9ea7-224">File in the user profile directory</span></span> |

<span data-ttu-id="e9ea7-225">Źródła konfiguracji są odczytywane w kolejności, w jakiej dostawcy konfiguracji są określeni podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-225">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="e9ea7-226">Dostawcy konfiguracji opisane w tym temacie są opisane w kolejności alfabetycznej, a nie w kolejności, w jakiej kod ich rozmieszcza.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-226">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="e9ea7-227">Zamów dostawców konfiguracji w kodzie, aby odpowiadały priorytetom źródłowych źródeł konfiguracji wymaganych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-227">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="e9ea7-228">Typową sekwencją dostawców konfiguracji jest:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-228">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="e9ea7-229">Pliki (*appSettings. JSON*, *appSettings. { Environment}. JSON*, gdzie `{Environment}` jest bieżącym środowiskiem hostingu aplikacji)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-229">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="e9ea7-230">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e9ea7-230">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="e9ea7-231">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tylko środowisko programistyczne)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-231">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="e9ea7-232">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="e9ea7-232">Environment variables</span></span>
1. <span data-ttu-id="e9ea7-233">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-233">Command-line arguments</span></span>

<span data-ttu-id="e9ea7-234">Typowym celem jest umieszczenie dostawcy konfiguracji wiersza polecenia jako ostatni w serii dostawców, aby zezwolić na argumenty wiersza polecenia, aby przesłonić konfigurację ustawioną przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-234">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="e9ea7-235">Poprzednia sekwencja dostawców jest używana, gdy nowy Konstruktor hosta zostanie zainicjowany przy użyciu `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-235">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e9ea7-236">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-236">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="e9ea7-237">Konfigurowanie konstruktora hostów za pomocą ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="e9ea7-237">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="e9ea7-238">Aby skonfigurować konstruktora hostów, wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> i podaj konfigurację.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-238">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="e9ea7-239">`ConfigureHostConfiguration` służy do inicjowania <xref:Microsoft.Extensions.Hosting.IHostEnvironment> do późniejszego użycia w procesie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-239">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="e9ea7-240">`ConfigureHostConfiguration` może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-240">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

## <a name="configureappconfiguration"></a><span data-ttu-id="e9ea7-241">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="e9ea7-241">ConfigureAppConfiguration</span></span>

<span data-ttu-id="e9ea7-242">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić dostawców konfiguracji aplikacji oprócz tych dodanych automatycznie przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-242">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="e9ea7-243">Zastąp poprzednią konfigurację argumentami wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-243">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="e9ea7-244">Aby podać konfigurację aplikacji, którą można zastąpić za pomocą argumentów wiersza polecenia, wywołaj `AddCommandLine` Last:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-244">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="e9ea7-245">Usuń dostawców dodanych przez CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="e9ea7-245">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="e9ea7-246">Aby usunąć dostawców dodanych przez `CreateDefaultBuilder`, wywołaj najpierw polecenie [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) w [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="e9ea7-246">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="e9ea7-247">Użyj konfiguracji podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-247">Consume configuration during app startup</span></span>

<span data-ttu-id="e9ea7-248">Konfiguracja dostarczona do aplikacji w `ConfigureAppConfiguration` jest dostępna podczas uruchamiania aplikacji, w tym `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-248">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e9ea7-249">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja dostępu podczas uruchamiania](#access-configuration-during-startup) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-249">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="e9ea7-250">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-250">Command-line Configuration Provider</span></span>

<span data-ttu-id="e9ea7-251"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> ładuje konfigurację z par klucz-wartość argumentu wiersza polecenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-251">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="e9ea7-252">Aby uaktywnić konfigurację wiersza polecenia, Metoda rozszerzenia <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> jest wywoływana w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-252">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e9ea7-253">`AddCommandLine` jest wywoływana automatycznie, gdy zostanie wywołana `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-253">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="e9ea7-254">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-254">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e9ea7-255">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-255">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e9ea7-256">Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-256">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="e9ea7-257">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-257">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e9ea7-258">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-258">Environment variables.</span></span>

<span data-ttu-id="e9ea7-259">`CreateDefaultBuilder` dodaje dostawcę konfiguracji wiersza polecenia Last.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-259">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="e9ea7-260">Argumenty wiersza polecenia przekazane w czasie wykonywania zastępują konfigurację ustawioną przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-260">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="e9ea7-261">`CreateDefaultBuilder` działa, gdy host jest skonstruowany.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-261">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="e9ea7-262">W związku z tym konfiguracja wiersza polecenia aktywowana przez `CreateDefaultBuilder` może mieć wpływ na sposób konfigurowania hosta.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-262">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="e9ea7-263">W przypadku aplikacji opartych na ASP.NET Core szablonach `AddCommandLine` została już wywołana przez `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-263">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e9ea7-264">Aby dodać kolejnych dostawców konfiguracji i zachować możliwość przesłonięcia konfiguracji od tych dostawców za pomocą argumentów wiersza polecenia, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddCommandLine` Last.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-264">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="e9ea7-265">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-265">**Example**</span></span>

<span data-ttu-id="e9ea7-266">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-266">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="e9ea7-267">Otwórz wiersz polecenia w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-267">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="e9ea7-268">Podaj argument wiersza polecenia do polecenia `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-268">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="e9ea7-269">Po uruchomieniu aplikacji otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-269">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e9ea7-270">Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla argumentu wiersza polecenia konfiguracji dostarczonego do `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-270">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="e9ea7-271">Argumenty</span><span class="sxs-lookup"><span data-stu-id="e9ea7-271">Arguments</span></span>

<span data-ttu-id="e9ea7-272">Wartość musi następować po znaku równości (`=`) lub klucz musi mieć prefiks (`--` lub `/`), gdy wartość znajduje się w miejscu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-272">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="e9ea7-273">Wartość nie jest wymagana, jeśli jest używany znak równości (na przykład `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-273">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="e9ea7-274">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="e9ea7-274">Key prefix</span></span>               | <span data-ttu-id="e9ea7-275">Przykład</span><span class="sxs-lookup"><span data-stu-id="e9ea7-275">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="e9ea7-276">Brak prefiksu</span><span class="sxs-lookup"><span data-stu-id="e9ea7-276">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="e9ea7-277">Dwie kreski (`--`)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-277">Two dashes (`--`)</span></span>        | <span data-ttu-id="e9ea7-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="e9ea7-279">Ukośnik (`/`)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-279">Forward slash (`/`)</span></span>      | <span data-ttu-id="e9ea7-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="e9ea7-281">W tym samym poleceniu nie należy mieszać par klucz-wartość argumentu wiersza polecenia, które używają znaku równości z parami klucz-wartość, które używają spacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-281">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="e9ea7-282">Przykładowe polecenia:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-282">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="e9ea7-283">Mapowanie przełączników</span><span class="sxs-lookup"><span data-stu-id="e9ea7-283">Switch mappings</span></span>

<span data-ttu-id="e9ea7-284">Mapowania przełączników Zezwalaj na logikę zamiany nazwy klucza.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-284">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="e9ea7-285">Podczas ręcznego kompilowania konfiguracji przy użyciu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>należy udostępnić słownik przemieszczeń Switch do metody <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-285">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="e9ea7-286">Gdy jest używany słownik mapowania przełączników, słownik jest sprawdzany dla klucza, który pasuje do klucza dostarczonego przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-286">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="e9ea7-287">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) zostanie przeniesiona z powrotem, aby ustawić parę klucz-wartość w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-287">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="e9ea7-288">Mapowanie przełącznika jest wymagane dla każdego klucza wiersza polecenia poprzedzonego jedną kreską (`-`).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-288">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="e9ea7-289">Przełącz reguły klucza słownika mapowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-289">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="e9ea7-290">Przełączniki muszą zaczynać się kreską (`-`) lub podwójną kreską (`--`).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-290">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="e9ea7-291">Słownik mapowania przełącznika nie może zawierać zduplikowanych kluczy.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-291">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="e9ea7-292">Utwórz słownik mapowań mapowania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-292">Create a switch mappings dictionary.</span></span> <span data-ttu-id="e9ea7-293">W poniższym przykładzie są tworzone dwa mapowania przełączników:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-293">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="e9ea7-294">Po skompilowaniu hosta Wywołaj `AddCommandLine` przy użyciu słownika mapowania przełączników:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-294">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="e9ea7-295">W przypadku aplikacji korzystających z mapowań przełączników wywołanie `CreateDefaultBuilder` nie powinno przekazywać argumentów.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-295">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="e9ea7-296">Wywołanie `AddCommandLine` metody `CreateDefaultBuilder` nie obejmuje zamapowanych przełączników i nie ma sposobu przekazywania słownika mapowania przełącznika do `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-296">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e9ea7-297">Rozwiązanie nie przekazuje argumentów do `CreateDefaultBuilder`, ale zamiast zezwalać `AddCommandLine` metody `ConfigurationBuilder` do przetwarzania zarówno argumentów, jak i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-297">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="e9ea7-298">Po utworzeniu słownika mapowań przełączników zawiera dane przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-298">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="e9ea7-299">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-299">Key</span></span>       | <span data-ttu-id="e9ea7-300">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-300">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="e9ea7-301">Jeśli klucze mapowane przez przełącznik są używane podczas uruchamiania aplikacji, konfiguracja otrzymuje wartość konfiguracji klucza dostarczonego przez słownik:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-301">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="e9ea7-302">Po uruchomieniu poprzedniego polecenia Konfiguracja zawiera wartości pokazane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-302">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="e9ea7-303">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-303">Key</span></span>               | <span data-ttu-id="e9ea7-304">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-304">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="e9ea7-305">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="e9ea7-305">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="e9ea7-306"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> ładuje konfigurację ze par klucz-wartość zmiennej środowiskowej w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-306">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="e9ea7-307">Aby uaktywnić konfigurację zmiennych środowiskowych, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-307">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="e9ea7-308">[Azure App Service](https://azure.microsoft.com/services/app-service/) umożliwia ustawianie zmiennych środowiskowych w witrynie Azure Portal, które mogą przesłonić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-308">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="e9ea7-309">Aby uzyskać więcej informacji, zobacz artykuł [Azure Apps: zastępowanie konfiguracji aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-309">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="e9ea7-310">`AddEnvironmentVariables` jest używany do ładowania zmiennych środowiskowych, które są poprzedzone `DOTNET_` na potrzeby [konfiguracji hosta](#host-versus-app-configuration) , gdy nowy Konstruktor hosta zostanie zainicjowany z [hostem ogólnym](xref:fundamentals/host/generic-host) i zostanie wywołane `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-310">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="e9ea7-311">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-311">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e9ea7-312">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-312">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e9ea7-313">Konfiguracja aplikacji z nieoznaczonych zmiennych środowiskowych przez wywołanie `AddEnvironmentVariables` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-313">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="e9ea7-314">Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-314">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="e9ea7-315">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-315">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e9ea7-316">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-316">Command-line arguments.</span></span>

<span data-ttu-id="e9ea7-317">Dostawca konfiguracji zmiennych środowiskowych jest wywoływany po ustanowieniu konfiguracji z poziomu kluczy tajnych użytkownika i plików *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-317">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="e9ea7-318">Wywołanie dostawcy w tym miejscu pozwala odczytywać zmienne środowiskowe w czasie wykonywania w celu przesłania konfiguracji ustawionych przez klucze tajne użytkownika i pliki *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-318">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="e9ea7-319">Aby zapewnić konfigurację aplikacji na podstawie dodatkowych zmiennych środowiskowych, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddEnvironmentVariables` z prefiksem:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-319">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="e9ea7-320">Wywołaj `AddEnvironmentVariables` ostatnią, aby zezwolić na zmienne środowiskowe z danym prefiksem w celu przesłonięcia wartości z innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-320">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="e9ea7-321">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-321">**Example**</span></span>

<span data-ttu-id="e9ea7-322">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-322">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="e9ea7-323">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-323">Run the sample app.</span></span> <span data-ttu-id="e9ea7-324">Otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-324">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e9ea7-325">Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla zmiennej środowiskowej `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-325">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="e9ea7-326">Wartość odzwierciedla środowisko, w którym jest uruchomiona aplikacja, zwykle `Development` podczas uruchamiania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-326">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="e9ea7-327">Aby zachować listę zmiennych środowiskowych renderowanych przez aplikację, aplikacja filtruje zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-327">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="e9ea7-328">Zapoznaj się z plikiem przykładowej *strony aplikacji/index. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-328">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="e9ea7-329">Aby udostępnić wszystkie zmienne środowiskowe dostępne dla aplikacji, należy zmienić `FilteredConfiguration` na *stronie/index. cshtml. cs* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-329">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="e9ea7-330">Prefiksy</span><span class="sxs-lookup"><span data-stu-id="e9ea7-330">Prefixes</span></span>

<span data-ttu-id="e9ea7-331">Zmienne środowiskowe ładowane do konfiguracji aplikacji są filtrowane podczas dostarczania prefiksu do metody `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-331">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="e9ea7-332">Na przykład aby filtrować zmienne środowiskowe na prefiksie `CUSTOM_`, podaj prefiks dla dostawcy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-332">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="e9ea7-333">Prefiks jest usuwany podczas tworzenia par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-333">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="e9ea7-334">Podczas tworzenia konstruktora hostów Konfiguracja hosta jest zapewniana przez zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-334">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="e9ea7-335">Aby uzyskać więcej informacji na temat prefiksu używanego dla tych zmiennych środowiskowych, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-335">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e9ea7-336">**Prefiksy parametrów połączenia**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-336">**Connection string prefixes**</span></span>

<span data-ttu-id="e9ea7-337">Interfejs API konfiguracji ma specjalne reguły przetwarzania dla czterech zmiennych środowiskowych parametrów połączenia związanych z konfigurowaniem parametrów połączenia platformy Azure dla środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-337">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="e9ea7-338">Zmienne środowiskowe z prefiksami podanymi w tabeli są ładowane do aplikacji, jeśli nie podano prefiksu do `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-338">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="e9ea7-339">Prefiks parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-339">Connection string prefix</span></span> | <span data-ttu-id="e9ea7-340">Dostawca</span><span class="sxs-lookup"><span data-stu-id="e9ea7-340">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="e9ea7-341">Dostawca niestandardowy</span><span class="sxs-lookup"><span data-stu-id="e9ea7-341">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="e9ea7-342">MySQL</span><span class="sxs-lookup"><span data-stu-id="e9ea7-342">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="e9ea7-343">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e9ea7-343">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="e9ea7-344">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e9ea7-344">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="e9ea7-345">Gdy zmienna środowiskowa zostanie odnaleziona i załadowana do konfiguracji z dowolnymi z czterech prefiksów pokazanych w tabeli:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-345">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="e9ea7-346">Klucz konfiguracji jest tworzony przez usunięcie prefiksu zmiennej środowiskowej i dodanie sekcji klucza konfiguracji (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-346">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="e9ea7-347">Zostanie utworzona nowa para klucz-wartość konfiguracji, która reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, które nie ma określonego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-347">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="e9ea7-348">Klucz zmiennej środowiskowej</span><span class="sxs-lookup"><span data-stu-id="e9ea7-348">Environment variable key</span></span> | <span data-ttu-id="e9ea7-349">Przekonwertowany klucz konfiguracji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-349">Converted configuration key</span></span> | <span data-ttu-id="e9ea7-350">Wpis konfiguracji dostawcy</span><span class="sxs-lookup"><span data-stu-id="e9ea7-350">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="e9ea7-351">Wpis konfiguracji nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-351">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="e9ea7-352">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-352">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="e9ea7-353">Wartość:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-353">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="e9ea7-354">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-354">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="e9ea7-355">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-355">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="e9ea7-356">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-356">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="e9ea7-357">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-357">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="e9ea7-358">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-358">**Example**</span></span>

<span data-ttu-id="e9ea7-359">Na serwerze zostanie utworzona niestandardowa zmienna środowiskowa parametrów połączenia:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-359">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="e9ea7-360">Nazwa &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-360">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="e9ea7-361">`Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True` &ndash; wartości</span><span class="sxs-lookup"><span data-stu-id="e9ea7-361">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="e9ea7-362">Jeśli `IConfiguration` zostanie dodany i przypisany do pola o nazwie `_config`, przeczytaj wartość:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-362">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="e9ea7-363">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="e9ea7-363">File Configuration Provider</span></span>

<span data-ttu-id="e9ea7-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową do ładowania konfiguracji z systemu plików.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="e9ea7-365">Następujący dostawcy konfiguracji są przydzielone do określonych typów plików:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-365">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="e9ea7-366">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="e9ea7-366">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="e9ea7-367">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="e9ea7-367">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="e9ea7-368">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="e9ea7-368">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="e9ea7-369">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="e9ea7-369">INI Configuration Provider</span></span>

<span data-ttu-id="e9ea7-370"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku INI w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-370">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="e9ea7-371">Aby uaktywnić konfigurację pliku INI, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-371">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e9ea7-372">Dwukropek może służyć jako ogranicznik sekcji w konfiguracji pliku INI.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-372">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="e9ea7-373">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-373">Overloads permit specifying:</span></span>

* <span data-ttu-id="e9ea7-374">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-374">Whether the file is optional.</span></span>
* <span data-ttu-id="e9ea7-375">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-375">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e9ea7-376"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-376">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e9ea7-377">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-377">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e9ea7-378">Ogólny przykład pliku konfiguracji INI:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-378">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="e9ea7-379">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-379">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e9ea7-380">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-380">section0:key0</span></span>
* <span data-ttu-id="e9ea7-381">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-381">section0:key1</span></span>
* <span data-ttu-id="e9ea7-382">Section1: podsekcja: Key</span><span class="sxs-lookup"><span data-stu-id="e9ea7-382">section1:subsection:key</span></span>
* <span data-ttu-id="e9ea7-383">section2: subsection0: klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-383">section2:subsection0:key</span></span>
* <span data-ttu-id="e9ea7-384">section2: subsection1: klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-384">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="e9ea7-385">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="e9ea7-385">JSON Configuration Provider</span></span>

<span data-ttu-id="e9ea7-386"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku JSON podczas środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-386">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="e9ea7-387">Aby uaktywnić konfigurację pliku JSON, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-387">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e9ea7-388">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-388">Overloads permit specifying:</span></span>

* <span data-ttu-id="e9ea7-389">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-389">Whether the file is optional.</span></span>
* <span data-ttu-id="e9ea7-390">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-390">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e9ea7-391"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-391">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e9ea7-392">`AddJsonFile` jest automatycznie wywoływana dwukrotnie, gdy nowy Konstruktor hosta zostanie zainicjowany z `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-392">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e9ea7-393">Metoda jest wywoływana w celu załadowania konfiguracji z:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-393">The method is called to load configuration from:</span></span>

* <span data-ttu-id="e9ea7-394">*appSettings. json* &ndash; ten plik jest najpierw odczytywany.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-394">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="e9ea7-395">Wersja środowiska pliku może przesłonić wartości dostarczone przez plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-395">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="e9ea7-396">*appSettings. {Environment}. JSON* &ndash; wersja środowiska pliku jest ładowana na podstawie [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-396">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="e9ea7-397">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-397">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e9ea7-398">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-398">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e9ea7-399">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-399">Environment variables.</span></span>
* <span data-ttu-id="e9ea7-400">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-400">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e9ea7-401">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-401">Command-line arguments.</span></span>

<span data-ttu-id="e9ea7-402">Dostawca konfiguracji JSON został ustanowiony jako pierwszy.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-402">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="e9ea7-403">W związku z tym klucze tajne użytkownika, zmienne środowiskowe i argumenty wiersza polecenia przesłaniają konfigurację ustawioną przez pliki *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-403">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="e9ea7-404">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji dla plików innych niż *appSettings. JSON* i *appSettings. { Environment}. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-404">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e9ea7-405">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-405">**Example**</span></span>

<span data-ttu-id="e9ea7-406">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje dwa wywołania `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-406">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="e9ea7-407">Pierwsze wywołanie `AddJsonFile` ładuje konfigurację z pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-407">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="e9ea7-408">Drugie wywołanie `AddJsonFile` ładuje konfigurację z *appSettings. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-408">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="e9ea7-409">Dla *appSettings. Plik Development. JSON* w przykładowej aplikacji jest ładowany:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-409">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="e9ea7-410">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-410">Run the sample app.</span></span> <span data-ttu-id="e9ea7-411">Otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-411">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e9ea7-412">Dane wyjściowe zawierają pary klucz-wartość dla konfiguracji w oparciu o środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-412">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="e9ea7-413">Poziom dziennika dla klucza `Logging:LogLevel:Default` jest `Debug` podczas uruchamiania aplikacji w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-413">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="e9ea7-414">Ponownie uruchom przykładową aplikację w środowisku produkcyjnym:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-414">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="e9ea7-415">Otwórz plik *Properties/profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-415">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="e9ea7-416">W profilu `ConfigurationSample` Zmień wartość zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` na `Production`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-416">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="e9ea7-417">Zapisz plik i uruchom aplikację za pomocą `dotnet run` w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-417">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="e9ea7-418">Ustawienia w *appSettings. Plik Development. JSON* nie zastępuje już ustawień w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-418">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="e9ea7-419">Poziom dziennika `Logging:LogLevel:Default` jest `Information`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-419">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="e9ea7-420">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="e9ea7-420">XML Configuration Provider</span></span>

<span data-ttu-id="e9ea7-421"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-421">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="e9ea7-422">Aby uaktywnić konfigurację pliku XML, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-422">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e9ea7-423">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-423">Overloads permit specifying:</span></span>

* <span data-ttu-id="e9ea7-424">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-424">Whether the file is optional.</span></span>
* <span data-ttu-id="e9ea7-425">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-425">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e9ea7-426"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-426">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e9ea7-427">Węzeł główny pliku konfiguracji jest ignorowany, gdy są tworzone pary klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-427">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="e9ea7-428">Nie określaj definicji typu dokumentu (DTD) ani przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-428">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="e9ea7-429">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-429">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e9ea7-430">Pliki konfiguracji XML mogą używać odrębnych nazw elementów dla powtarzających się sekcji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-430">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="e9ea7-431">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-431">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e9ea7-432">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-432">section0:key0</span></span>
* <span data-ttu-id="e9ea7-433">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-433">section0:key1</span></span>
* <span data-ttu-id="e9ea7-434">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-434">section1:key0</span></span>
* <span data-ttu-id="e9ea7-435">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-435">section1:key1</span></span>

<span data-ttu-id="e9ea7-436">Powtarzalne elementy, które używają tej samej nazwy elementu, działają, jeśli atrybut `name` jest używany do odróżnienia elementów:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-436">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="e9ea7-437">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-437">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e9ea7-438">sekcja: section0: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-438">section:section0:key:key0</span></span>
* <span data-ttu-id="e9ea7-439">sekcja: section0: Key: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-439">section:section0:key:key1</span></span>
* <span data-ttu-id="e9ea7-440">sekcja: Section1: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-440">section:section1:key:key0</span></span>
* <span data-ttu-id="e9ea7-441">sekcja: Section1: Key: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-441">section:section1:key:key1</span></span>

<span data-ttu-id="e9ea7-442">Atrybuty mogą służyć do dostarczania wartości:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-442">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="e9ea7-443">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-443">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e9ea7-444">Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="e9ea7-444">key:attribute</span></span>
* <span data-ttu-id="e9ea7-445">sekcja: Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="e9ea7-445">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="e9ea7-446">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="e9ea7-446">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="e9ea7-447"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> używa plików katalogu jako par klucz konfiguracji i wartość.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-447">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="e9ea7-448">Kluczem jest nazwa pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-448">The key is the file name.</span></span> <span data-ttu-id="e9ea7-449">Wartość zawiera zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-449">The value contains the file's contents.</span></span> <span data-ttu-id="e9ea7-450">Dostawca konfiguracji klucza dla plików jest używany w scenariuszach hostingu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-450">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="e9ea7-451">Aby uaktywnić konfigurację klucza dla plików, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-451">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="e9ea7-452">`directoryPath` plików musi być ścieżką bezwzględną.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-452">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="e9ea7-453">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-453">Overloads permit specifying:</span></span>

* <span data-ttu-id="e9ea7-454">Delegat `Action<KeyPerFileConfigurationSource>`, który konfiguruje źródło.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-454">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="e9ea7-455">Określa, czy katalog jest opcjonalny, i ścieżkę do katalogu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-455">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="e9ea7-456">Podwójny znak podkreślenia (`__`) jest używany jako ogranicznik klucza konfiguracji w nazwach plików.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-456">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="e9ea7-457">Na przykład nazwa pliku `Logging__LogLevel__System` generuje klucz konfiguracji `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-457">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="e9ea7-458">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-458">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="e9ea7-459">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="e9ea7-459">Memory Configuration Provider</span></span>

<span data-ttu-id="e9ea7-460"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> używa kolekcji w pamięci jako par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-460">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="e9ea7-461">Aby uaktywnić konfigurację kolekcji w pamięci, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-461">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e9ea7-462">Dostawcę konfiguracji można zainicjować przy użyciu `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-462">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="e9ea7-463">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-463">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="e9ea7-464">W poniższym przykładzie tworzony jest słownik konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-464">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="e9ea7-465">Słownik jest używany z wywołaniem `AddInMemoryCollection`, aby zapewnić konfigurację:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-465">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="e9ea7-466">GetValue</span><span class="sxs-lookup"><span data-stu-id="e9ea7-466">GetValue</span></span>

<span data-ttu-id="e9ea7-467">[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnia jedną wartość z konfiguracji z określonym kluczem i konwertuje ją na określony typ niekolekcje.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-467">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="e9ea7-468">Przeciążenie akceptuje wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-468">An overload accepts a default value.</span></span>

<span data-ttu-id="e9ea7-469">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-469">The following example:</span></span>

* <span data-ttu-id="e9ea7-470">Wyodrębnia wartość ciągu z konfiguracji z kluczem `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-470">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="e9ea7-471">Jeśli nie można odnaleźć `NumberKey` w kluczach konfiguracji, zostanie użyta wartość domyślna `99`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-471">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="e9ea7-472">Typ wartości jako `int`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-472">Types the value as an `int`.</span></span>
* <span data-ttu-id="e9ea7-473">Przechowuje wartość we właściwości `NumberConfig` do użycia na stronie.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-473">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="e9ea7-474">GetSection, GetChildren i EXISTS</span><span class="sxs-lookup"><span data-stu-id="e9ea7-474">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="e9ea7-475">W poniższych przykładach należy wziąć pod uwagę następujący plik JSON.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-475">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="e9ea7-476">Cztery klucze są dostępne w dwóch sekcjach, z których jedna zawiera parę podsekcji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-476">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="e9ea7-477">Gdy plik jest odczytywany do konfiguracji, następujące unikatowe klucze hierarchiczne są tworzone w celu przechowywania wartości konfiguracyjnych:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-477">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="e9ea7-478">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-478">section0:key0</span></span>
* <span data-ttu-id="e9ea7-479">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-479">section0:key1</span></span>
* <span data-ttu-id="e9ea7-480">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-480">section1:key0</span></span>
* <span data-ttu-id="e9ea7-481">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-481">section1:key1</span></span>
* <span data-ttu-id="e9ea7-482">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-482">section2:subsection0:key0</span></span>
* <span data-ttu-id="e9ea7-483">section2: subsection0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-483">section2:subsection0:key1</span></span>
* <span data-ttu-id="e9ea7-484">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-484">section2:subsection1:key0</span></span>
* <span data-ttu-id="e9ea7-485">section2: subsection1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-485">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="e9ea7-486">GetSection</span><span class="sxs-lookup"><span data-stu-id="e9ea7-486">GetSection</span></span>

<span data-ttu-id="e9ea7-487">[IConfiguration. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcję konfiguracji z określonym kluczem podsekcji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="e9ea7-488">Aby zwrócić <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz-wartość w `section1`, wywołaj `GetSection` i podaj nazwę sekcji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-488">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="e9ea7-489">`configSection` nie ma wartości, tylko klucza i ścieżki.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-489">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="e9ea7-490">Podobnie, aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę sekcji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-490">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="e9ea7-491">`GetSection` nigdy nie zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-491">`GetSection` never returns `null`.</span></span> <span data-ttu-id="e9ea7-492">Jeśli nie znaleziono pasującej sekcji, zwracany jest pusty `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-492">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="e9ea7-493">Gdy `GetSection` zwraca pasującą sekcję, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-493">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="e9ea7-494"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> i <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> są zwracane, gdy istnieje sekcja.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-494">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="e9ea7-495">GetChildren</span><span class="sxs-lookup"><span data-stu-id="e9ea7-495">GetChildren</span></span>

<span data-ttu-id="e9ea7-496">Wywołanie [iConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) w `section2` uzyskuje `IEnumerable<IConfigurationSection>` obejmujący:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-496">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="e9ea7-497">Exists</span><span class="sxs-lookup"><span data-stu-id="e9ea7-497">Exists</span></span>

<span data-ttu-id="e9ea7-498">Użyj [ConfigurationExtensions. istnieje](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) , aby określić, czy istnieje sekcja konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-498">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="e9ea7-499">Dane przykładowe `sectionExists` jest `false`, ponieważ w danych konfiguracyjnych nie ma `section2:subsection2` sekcji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-499">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="e9ea7-500">Powiąż z klasą</span><span class="sxs-lookup"><span data-stu-id="e9ea7-500">Bind to a class</span></span>

<span data-ttu-id="e9ea7-501">Konfigurację można powiązać z klasami, które reprezentują grupy powiązanych ustawień przy użyciu *wzorca opcji*.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-501">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="e9ea7-502">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-502">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e9ea7-503">Wartości konfiguracji są zwracane jako ciągi, ale wywołanie <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie obiektów [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-503">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="e9ea7-504">Obiekt wiążący wiąże wartości ze wszystkimi publicznymi właściwościami odczytu/zapisu dostarczonego typu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-504">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="e9ea7-505">Pola **nie** są powiązane.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-505">Fields are **not** bound.</span></span>

<span data-ttu-id="e9ea7-506">Przykładowa aplikacja zawiera model `Starship` (*modele/Starship. cs*):</span><span class="sxs-lookup"><span data-stu-id="e9ea7-506">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="e9ea7-507">Sekcja `starship` pliku *Starship. JSON* tworzy konfigurację, gdy aplikacja Przykładowa używa dostawcy konfiguracji JSON do załadowania konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-507">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

<span data-ttu-id="e9ea7-508">Tworzone są następujące pary klucz-wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-508">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="e9ea7-509">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-509">Key</span></span>                   | <span data-ttu-id="e9ea7-510">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-510">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="e9ea7-511">Starship: Nazwa</span><span class="sxs-lookup"><span data-stu-id="e9ea7-511">starship:name</span></span>         | <span data-ttu-id="e9ea7-512">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="e9ea7-512">USS Enterprise</span></span>                                    |
| <span data-ttu-id="e9ea7-513">Starship: Rejestr</span><span class="sxs-lookup"><span data-stu-id="e9ea7-513">starship:registry</span></span>     | <span data-ttu-id="e9ea7-514">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="e9ea7-514">NCC-1701</span></span>                                          |
| <span data-ttu-id="e9ea7-515">Starship: Klasa</span><span class="sxs-lookup"><span data-stu-id="e9ea7-515">starship:class</span></span>        | <span data-ttu-id="e9ea7-516">Skład</span><span class="sxs-lookup"><span data-stu-id="e9ea7-516">Constitution</span></span>                                      |
| <span data-ttu-id="e9ea7-517">Starship: Długość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-517">starship:length</span></span>       | <span data-ttu-id="e9ea7-518">304,8</span><span class="sxs-lookup"><span data-stu-id="e9ea7-518">304.8</span></span>                                             |
| <span data-ttu-id="e9ea7-519">Starship: prowizja</span><span class="sxs-lookup"><span data-stu-id="e9ea7-519">starship:commissioned</span></span> | <span data-ttu-id="e9ea7-520">False</span><span class="sxs-lookup"><span data-stu-id="e9ea7-520">False</span></span>                                             |
| <span data-ttu-id="e9ea7-521">handlowych</span><span class="sxs-lookup"><span data-stu-id="e9ea7-521">trademark</span></span>             | <span data-ttu-id="e9ea7-522">Najważniejsze obrazy Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="e9ea7-522">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="e9ea7-523">Przykładowa aplikacja wywołuje `GetSection` z kluczem `starship`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-523">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="e9ea7-524">Pary klucz-wartość `starship` są odizolowane.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-524">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="e9ea7-525">Metoda `Bind` jest wywoływana w podsekcji przekazującej w wystąpieniu klasy `Starship`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-525">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="e9ea7-526">Po powiązaniu wartości wystąpień wystąpienie jest przypisywane do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-526">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="e9ea7-527">Powiąż z grafem obiektów</span><span class="sxs-lookup"><span data-stu-id="e9ea7-527">Bind to an object graph</span></span>

<span data-ttu-id="e9ea7-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> jest w stanie powiązać cały Graf obiektów POCO.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="e9ea7-529">Podobnie jak w przypadku powiązania prostego obiektu, powiązane są tylko publiczne właściwości odczytu i zapisu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-529">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="e9ea7-530">Przykład zawiera model `TvShow`, którego wykres obiektu zawiera klasy `Metadata` i `Actors` (*modele/TvShow. cs*):</span><span class="sxs-lookup"><span data-stu-id="e9ea7-530">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="e9ea7-531">Przykładowa aplikacja zawiera plik *tvshow. XML* zawierający dane konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-531">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="e9ea7-532">Konfiguracja jest powiązana z całym grafem obiektu `TvShow` za pomocą metody `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-532">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="e9ea7-533">Powiązane wystąpienie jest przypisane do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-533">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="e9ea7-534">[ConfigurationBinder. Get\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) tworzy powiązania i zwraca określony typ.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="e9ea7-535">`Get<T>` jest wygodniejszy niż korzystanie z `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-535">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="e9ea7-536">Poniższy kod pokazuje, jak używać `Get<T>` w poprzednim przykładzie, co umożliwia bezpośrednie przypisanie wystąpienia powiązanego do właściwości używanej do renderowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-536">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="e9ea7-537">Powiąż tablicę z klasą</span><span class="sxs-lookup"><span data-stu-id="e9ea7-537">Bind an array to a class</span></span>

<span data-ttu-id="e9ea7-538">*Przykładowa aplikacja pokazuje Koncepcje opisane w tej sekcji.*</span><span class="sxs-lookup"><span data-stu-id="e9ea7-538">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="e9ea7-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-539">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e9ea7-540">Każdy format tablicy, który ujawnia segment klucza numerycznego (`:0:`, `:1:`, &hellip; `:{n}:`), jest w stanie powiązać powiązanie tablicową z tablicą klas POCO.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-540">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="e9ea7-541">Powiązanie jest dostarczane według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-541">Binding is provided by convention.</span></span> <span data-ttu-id="e9ea7-542">Niestandardowi dostawcy konfiguracji nie muszą implementować powiązania tablicy.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-542">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="e9ea7-543">**Przetwarzanie tablicy w pamięci**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-543">**In-memory array processing**</span></span>

<span data-ttu-id="e9ea7-544">Należy wziąć pod uwagę klucze konfiguracji i wartości podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-544">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="e9ea7-545">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-545">Key</span></span>             | <span data-ttu-id="e9ea7-546">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-546">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="e9ea7-547">Tablica: wpisy: 0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-547">array:entries:0</span></span> | <span data-ttu-id="e9ea7-548">value0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-548">value0</span></span> |
| <span data-ttu-id="e9ea7-549">Tablica: wpisy: 1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-549">array:entries:1</span></span> | <span data-ttu-id="e9ea7-550">Sekwencj</span><span class="sxs-lookup"><span data-stu-id="e9ea7-550">value1</span></span> |
| <span data-ttu-id="e9ea7-551">Tablica: wpisy: 2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-551">array:entries:2</span></span> | <span data-ttu-id="e9ea7-552">Wartość2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-552">value2</span></span> |
| <span data-ttu-id="e9ea7-553">Tablica: wpisy: 4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-553">array:entries:4</span></span> | <span data-ttu-id="e9ea7-554">value4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-554">value4</span></span> |
| <span data-ttu-id="e9ea7-555">Tablica: wpisy: 5</span><span class="sxs-lookup"><span data-stu-id="e9ea7-555">array:entries:5</span></span> | <span data-ttu-id="e9ea7-556">value5</span><span class="sxs-lookup"><span data-stu-id="e9ea7-556">value5</span></span> |

<span data-ttu-id="e9ea7-557">Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-557">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="e9ea7-558">Tablica pomija wartość indeksu &num;3.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-558">The array skips a value for index &num;3.</span></span> <span data-ttu-id="e9ea7-559">Segregator konfiguracji nie może powiązać wartości null ani tworzyć wpisów o wartości null w obiektach powiązanych, co oznacza, że w chwili pojawi się wynik powiązania tej tablicy z obiektem.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-559">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="e9ea7-560">W przykładowej aplikacji jest dostępna Klasa POCO, która przechowuje powiązane dane konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-560">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="e9ea7-561">Dane konfiguracji są powiązane z obiektem:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-561">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="e9ea7-562">[ConfigurationBinder. Get\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) można również użyć składni, co spowoduje zwiększenie kodu kompaktowego:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-562">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="e9ea7-563">Obiekt powiązany, wystąpienie `ArrayExample`, otrzymuje dane tablicy z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-563">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="e9ea7-564">Indeks `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-564">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="e9ea7-565">Wartość `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-565">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="e9ea7-566">0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-566">0</span></span>                            | <span data-ttu-id="e9ea7-567">value0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-567">value0</span></span>                       |
| <span data-ttu-id="e9ea7-568">1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-568">1</span></span>                            | <span data-ttu-id="e9ea7-569">Sekwencj</span><span class="sxs-lookup"><span data-stu-id="e9ea7-569">value1</span></span>                       |
| <span data-ttu-id="e9ea7-570">2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-570">2</span></span>                            | <span data-ttu-id="e9ea7-571">Wartość2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-571">value2</span></span>                       |
| <span data-ttu-id="e9ea7-572">3</span><span class="sxs-lookup"><span data-stu-id="e9ea7-572">3</span></span>                            | <span data-ttu-id="e9ea7-573">value4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-573">value4</span></span>                       |
| <span data-ttu-id="e9ea7-574">4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-574">4</span></span>                            | <span data-ttu-id="e9ea7-575">value5</span><span class="sxs-lookup"><span data-stu-id="e9ea7-575">value5</span></span>                       |

<span data-ttu-id="e9ea7-576">Indeks &num;3 w obiekcie powiązanym zawiera dane konfiguracyjne `array:4` klucza konfiguracji i jego wartość `value4`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-576">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="e9ea7-577">Gdy dane konfiguracji zawierające tablicę są powiązane, indeksy tablic w kluczach konfiguracji są używane tylko do iteracji danych konfiguracji podczas tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-577">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="e9ea7-578">Wartości null nie można zachować w danych konfiguracyjnych, a wpis o wartości null nie jest tworzony w obiekcie powiązanym, gdy tablica w kluczach konfiguracji pomija jeden lub więcej indeksów.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-578">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="e9ea7-579">Brakujący element konfiguracji dla indeksu &num;3 można dostarczyć przed powiązaniem do wystąpienia `ArrayExample` przez dowolnego dostawcę konfiguracji, który wygeneruje poprawną parę klucz-wartość w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-579">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="e9ea7-580">Jeśli przykład zawiera dodatkowego dostawcę konfiguracji JSON z brakującą parą klucz-wartość, `ArrayExample.Entries` pasuje do kompletnej tablicy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-580">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="e9ea7-581">plik *missing_value. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-581">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="e9ea7-582">W pliku `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-582">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="e9ea7-583">Para klucz-wartość pokazana w tabeli jest ładowana do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-583">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="e9ea7-584">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-584">Key</span></span>             | <span data-ttu-id="e9ea7-585">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-585">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="e9ea7-586">Tablica: wpisy: 3</span><span class="sxs-lookup"><span data-stu-id="e9ea7-586">array:entries:3</span></span> | <span data-ttu-id="e9ea7-587">Wartość3</span><span class="sxs-lookup"><span data-stu-id="e9ea7-587">value3</span></span> |

<span data-ttu-id="e9ea7-588">Jeśli wystąpienie klasy `ArrayExample` jest powiązane, gdy dostawca konfiguracji JSON zawiera wpis dla indeksu &num;3, tablica `ArrayExample.Entries` zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-588">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="e9ea7-589">Indeks `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-589">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="e9ea7-590">Wartość `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-590">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="e9ea7-591">0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-591">0</span></span>                            | <span data-ttu-id="e9ea7-592">value0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-592">value0</span></span>                       |
| <span data-ttu-id="e9ea7-593">1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-593">1</span></span>                            | <span data-ttu-id="e9ea7-594">Sekwencj</span><span class="sxs-lookup"><span data-stu-id="e9ea7-594">value1</span></span>                       |
| <span data-ttu-id="e9ea7-595">2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-595">2</span></span>                            | <span data-ttu-id="e9ea7-596">Wartość2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-596">value2</span></span>                       |
| <span data-ttu-id="e9ea7-597">3</span><span class="sxs-lookup"><span data-stu-id="e9ea7-597">3</span></span>                            | <span data-ttu-id="e9ea7-598">Wartość3</span><span class="sxs-lookup"><span data-stu-id="e9ea7-598">value3</span></span>                       |
| <span data-ttu-id="e9ea7-599">4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-599">4</span></span>                            | <span data-ttu-id="e9ea7-600">value4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-600">value4</span></span>                       |
| <span data-ttu-id="e9ea7-601">5</span><span class="sxs-lookup"><span data-stu-id="e9ea7-601">5</span></span>                            | <span data-ttu-id="e9ea7-602">value5</span><span class="sxs-lookup"><span data-stu-id="e9ea7-602">value5</span></span>                       |

<span data-ttu-id="e9ea7-603">**Przetwarzanie tablicy JSON**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-603">**JSON array processing**</span></span>

<span data-ttu-id="e9ea7-604">Jeśli plik JSON zawiera tablicę, klucze konfiguracji są tworzone dla elementów tablicy z indeksem sekcji o wartości zero.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-604">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="e9ea7-605">W poniższym pliku konfiguracyjnym `subsection` jest tablicą:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-605">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="e9ea7-606">Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz-wartość:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-606">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="e9ea7-607">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-607">Key</span></span>                     | <span data-ttu-id="e9ea7-608">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-608">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="e9ea7-609">json_array: klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-609">json_array:key</span></span>          | <span data-ttu-id="e9ea7-610">wartośća</span><span class="sxs-lookup"><span data-stu-id="e9ea7-610">valueA</span></span> |
| <span data-ttu-id="e9ea7-611">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-611">json_array:subsection:0</span></span> | <span data-ttu-id="e9ea7-612">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="e9ea7-612">valueB</span></span> |
| <span data-ttu-id="e9ea7-613">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-613">json_array:subsection:1</span></span> | <span data-ttu-id="e9ea7-614">valueC</span><span class="sxs-lookup"><span data-stu-id="e9ea7-614">valueC</span></span> |
| <span data-ttu-id="e9ea7-615">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-615">json_array:subsection:2</span></span> | <span data-ttu-id="e9ea7-616">Znajdując</span><span class="sxs-lookup"><span data-stu-id="e9ea7-616">valueD</span></span> |

<span data-ttu-id="e9ea7-617">W przykładowej aplikacji jest dostępna następująca Klasa POCO z powiązaniem par klucz-wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-617">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="e9ea7-618">Po powiązaniu `JsonArrayExample.Key` utrzymuje `valueA`wartości.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-618">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="e9ea7-619">Wartości podsekcji są przechowywane we właściwości tablicy POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-619">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="e9ea7-620">Indeks `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-620">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="e9ea7-621">Wartość `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-621">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="e9ea7-622">0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-622">0</span></span>                                   | <span data-ttu-id="e9ea7-623">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="e9ea7-623">valueB</span></span>                              |
| <span data-ttu-id="e9ea7-624">1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-624">1</span></span>                                   | <span data-ttu-id="e9ea7-625">valueC</span><span class="sxs-lookup"><span data-stu-id="e9ea7-625">valueC</span></span>                              |
| <span data-ttu-id="e9ea7-626">2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-626">2</span></span>                                   | <span data-ttu-id="e9ea7-627">Znajdując</span><span class="sxs-lookup"><span data-stu-id="e9ea7-627">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="e9ea7-628">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-628">Custom configuration provider</span></span>

<span data-ttu-id="e9ea7-629">Przykładowa aplikacja pokazuje, jak utworzyć podstawowego dostawcę konfiguracji, który odczytuje pary klucz-wartość konfiguracji z bazy danych przy użyciu [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-629">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="e9ea7-630">Dostawca ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-630">The provider has the following characteristics:</span></span>

* <span data-ttu-id="e9ea7-631">Baza danych EF w pamięci jest używana w celach demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-631">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="e9ea7-632">Aby użyć bazy danych, która wymaga parametrów połączenia, zaimplementuj pomocniczą `ConfigurationBuilder`, aby podać parametry połączenia od innego dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-632">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="e9ea7-633">Dostawca odczytuje tabelę bazy danych w konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-633">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="e9ea7-634">Dostawca nie wykonuje zapytania do bazy danych w oparciu o klucz.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-634">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="e9ea7-635">Ponowne załadowanie nie zostało zaimplementowane, więc aktualizacja bazy danych po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-635">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="e9ea7-636">Zdefiniuj jednostkę `EFConfigurationValue` do przechowywania wartości konfiguracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-636">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="e9ea7-637">*Modele/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-637">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="e9ea7-638">Dodaj `EFConfigurationContext` do przechowywania skonfigurowanych wartości i uzyskiwania do nich dostępu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-638">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="e9ea7-639">*EFConfigurationProvider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-639">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="e9ea7-640">Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-640">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="e9ea7-641">*EFConfigurationProvider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-641">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="e9ea7-642">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-642">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="e9ea7-643">Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-643">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="e9ea7-644">Ponieważ w [kluczach konfiguracji jest rozróżniana wielkość liter](#keys), słownik używany do inicjowania bazy danych jest tworzony przy użyciu funkcji porównującej bez uwzględniania wielkości liter ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-644">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="e9ea7-645">*EFConfigurationProvider/EFConfigurationProvider. cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-645">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="e9ea7-646">Metoda rozszerzenia `AddEFConfiguration` zezwala na Dodawanie źródła konfiguracji do `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-646">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="e9ea7-647">*Rozszerzenia/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-647">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="e9ea7-648">Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-648">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="e9ea7-649">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="e9ea7-649">Access configuration during startup</span></span>

<span data-ttu-id="e9ea7-650">Wsuń `IConfiguration` do konstruktora `Startup`, aby uzyskać dostęp do wartości konfiguracyjnych w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-650">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e9ea7-651">Aby uzyskać dostęp do konfiguracji w `Startup.Configure`, należy wstrzyknąć `IConfiguration` bezpośrednio do metody lub użyć wystąpienia z konstruktora:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-651">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="e9ea7-652">Aby zapoznać się z przykładem uzyskiwania dostępu do konfiguracji przy użyciu metod uruchamiania, zobacz [Uruchamianie aplikacji: wygodne metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-652">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="e9ea7-653">Konfiguracja dostępu na stronie Razor Pages lub widoku MVC</span><span class="sxs-lookup"><span data-stu-id="e9ea7-653">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="e9ea7-654">Aby uzyskać dostęp do ustawień konfiguracji na stronie Razor Pages lub widoku MVC, Dodaj [dyrektywę using](xref:mvc/views/razor#using) ([ C# odwołanie: Using](/dotnet/csharp/language-reference/keywords/using-directive)) dla [przestrzeni nazw Microsoft. Extensions. Configuration](xref:Microsoft.Extensions.Configuration) i wsuń <xref:Microsoft.Extensions.Configuration.IConfiguration> do strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-654">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="e9ea7-655">Na stronie Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-655">In a Razor Pages page:</span></span>

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

<span data-ttu-id="e9ea7-656">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-656">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="e9ea7-657">Dodawanie konfiguracji z zestawu zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="e9ea7-657">Add configuration from an external assembly</span></span>

<span data-ttu-id="e9ea7-658">Implementacja <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zestawu zewnętrznego poza klasą `Startup` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-658">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="e9ea7-659">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-659">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e9ea7-660">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e9ea7-660">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e9ea7-661">Konfiguracja aplikacji w ASP.NET Core jest oparta na parach klucz-wartość określonych przez *dostawców konfiguracji*.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-661">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="e9ea7-662">Dostawcy konfiguracji odczytują dane konfiguracji do par klucz-wartość z różnych źródeł konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-662">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="e9ea7-663">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e9ea7-663">Azure Key Vault</span></span>
* <span data-ttu-id="e9ea7-664">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="e9ea7-664">Azure App Configuration</span></span>
* <span data-ttu-id="e9ea7-665">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-665">Command-line arguments</span></span>
* <span data-ttu-id="e9ea7-666">Dostawcy niestandardowi (instalowani lub utworzony)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-666">Custom providers (installed or created)</span></span>
* <span data-ttu-id="e9ea7-667">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="e9ea7-667">Directory files</span></span>
* <span data-ttu-id="e9ea7-668">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="e9ea7-668">Environment variables</span></span>
* <span data-ttu-id="e9ea7-669">Obiekty w pamięci .NET</span><span class="sxs-lookup"><span data-stu-id="e9ea7-669">In-memory .NET objects</span></span>
* <span data-ttu-id="e9ea7-670">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="e9ea7-670">Settings files</span></span>

<span data-ttu-id="e9ea7-671">Pakiety konfiguracyjne dla typowych scenariuszy dostawcy konfiguracji ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-671">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="e9ea7-672">Przykłady kodu, które obserwują i w przykładowej aplikacji używają przestrzeni nazw <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-672">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="e9ea7-673">*Wzorzec opcji* jest rozszerzeniem pojęć konfiguracyjnych opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-673">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="e9ea7-674">Opcje używają klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-674">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="e9ea7-675">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-675">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e9ea7-676">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e9ea7-676">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="e9ea7-677">Host a konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-677">Host versus app configuration</span></span>

<span data-ttu-id="e9ea7-678">Przed skonfigurowaniem i uruchomieniem aplikacji *host* zostanie skonfigurowany i uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-678">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="e9ea7-679">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-679">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="e9ea7-680">Zarówno aplikacja, jak i Host są konfigurowane przy użyciu dostawców konfiguracji opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-680">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="e9ea7-681">Klucz konfiguracji hosta — pary wartości są również uwzględnione w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-681">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="e9ea7-682">Aby uzyskać więcej informacji na temat tego, jak dostawcy konfiguracji są używani podczas kompilowania hosta i jak źródła konfiguracji wpływają na konfigurację hosta, zobacz <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-682">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="e9ea7-683">Inna konfiguracja</span><span class="sxs-lookup"><span data-stu-id="e9ea7-683">Other configuration</span></span>

<span data-ttu-id="e9ea7-684">Ten temat dotyczy tylko *konfiguracji aplikacji*.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-684">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="e9ea7-685">Inne aspekty uruchamiania i hostowania aplikacji ASP.NET Core są konfigurowane przy użyciu plików konfiguracji nieuwzględnionych w tym temacie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-685">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="e9ea7-686">plik *Launch. json*/*profilu launchsettings. JSON* to pliki konfiguracji narzędzi dla środowiska programistycznego, opisane w temacie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-686">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="e9ea7-687">W <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-687">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="e9ea7-688">W zestawie dokumentacji, w której pliki są używane do konfigurowania ASP.NET Core aplikacji na potrzeby scenariuszy programistycznych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-688">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="e9ea7-689">*Web. config* to plik konfiguracji serwera opisany w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-689">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="e9ea7-690">Aby uzyskać więcej informacji na temat migrowania konfiguracji aplikacji z wcześniejszych wersji programu ASP.NET, zobacz <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-690">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="e9ea7-691">Konfiguracja domyślna</span><span class="sxs-lookup"><span data-stu-id="e9ea7-691">Default configuration</span></span>

<span data-ttu-id="e9ea7-692">Aplikacje sieci Web oparte na ASP.NET Core [nowym szablonów dotnet](/dotnet/core/tools/dotnet-new) <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-692">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="e9ea7-693">`CreateDefaultBuilder` zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-693">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="e9ea7-694">Poniższe zasady dotyczą aplikacji korzystających z [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-694">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="e9ea7-695">Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej w przypadku korzystania z [hosta ogólnego](xref:fundamentals/host/generic-host), zobacz [najnowszą wersję tego tematu](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-695">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="e9ea7-696">Konfiguracja hosta jest poświadczona z:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-696">Host configuration is provided from:</span></span>
  * <span data-ttu-id="e9ea7-697">Zmienne środowiskowe poprzedzone prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-697">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="e9ea7-698">Prefiks (`ASPNETCORE_`) jest usuwany podczas ładowania par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-698">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="e9ea7-699">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-699">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="e9ea7-700">Podano konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-700">App configuration is provided from:</span></span>
  * <span data-ttu-id="e9ea7-701">*appSettings. JSON* przy użyciu [dostawcy konfiguracji plików](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-701">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e9ea7-702">*appSettings. {Environment}. JSON* przy użyciu [dostawcy konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-702">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e9ea7-703">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development` przy użyciu zestawu wpisów.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-703">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="e9ea7-704">Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-704">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="e9ea7-705">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-705">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="e9ea7-706">Bezpieczeństwo</span><span class="sxs-lookup"><span data-stu-id="e9ea7-706">Security</span></span>

<span data-ttu-id="e9ea7-707">Aby zabezpieczyć poufne dane konfiguracji, należy zastosować następujące rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-707">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="e9ea7-708">Nie należy przechowywać haseł ani innych danych poufnych w kodzie dostawcy konfiguracji ani w plikach konfiguracji zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-708">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="e9ea7-709">Nie używaj tajemnic produkcyjnych w środowiskach deweloperskich i testowych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-709">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="e9ea7-710">Określ wpisy tajne poza projektem, aby nie mogły zostać przypadkowo przekazane do repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-710">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="e9ea7-711">Aby uzyskać więcej informacji, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-711">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="e9ea7-712"><xref:security/app-secrets> &ndash; zawiera porady dotyczące używania zmiennych środowiskowych do przechowywania poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-712"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="e9ea7-713">Menedżer wpisów tajnych używa dostawcy konfiguracji plików do przechowywania wpisów tajnych użytkownika w pliku JSON w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-713">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="e9ea7-714">Dostawca konfiguracji plików został opisany w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-714">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="e9ea7-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) bezpieczne przechowywanie wpisów tajnych aplikacji dla ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="e9ea7-716">Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-716">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="e9ea7-717">Hierarchiczne dane konfiguracji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-717">Hierarchical configuration data</span></span>

<span data-ttu-id="e9ea7-718">Interfejs API konfiguracji jest w stanie utrzymywać hierarchiczne dane konfiguracji przez spłaszczonie danych hierarchicznych przy użyciu ogranicznika w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-718">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="e9ea7-719">W poniższym pliku JSON cztery klucze istnieją w hierarchii strukturalnej dwóch sekcji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-719">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="e9ea7-720">Gdy plik jest odczytywany do konfiguracji, są tworzone unikatowe klucze, aby zachować oryginalną hierarchiczną strukturę danych źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-720">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="e9ea7-721">Sekcje i klucze są spłaszczone przy użyciu dwukropka (`:`), aby zachować oryginalną strukturę:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-721">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="e9ea7-722">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-722">section0:key0</span></span>
* <span data-ttu-id="e9ea7-723">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-723">section0:key1</span></span>
* <span data-ttu-id="e9ea7-724">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-724">section1:key0</span></span>
* <span data-ttu-id="e9ea7-725">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-725">section1:key1</span></span>

<span data-ttu-id="e9ea7-726">Metody <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> są dostępne do izolowania sekcji i elementów podrzędnych sekcji w danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-726"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="e9ea7-727">Te metody są opisane w dalszej [części GetSection, GetChildren i EXISTS](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-727">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="e9ea7-728">Konwencja</span><span class="sxs-lookup"><span data-stu-id="e9ea7-728">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="e9ea7-729">Źródła i dostawcy</span><span class="sxs-lookup"><span data-stu-id="e9ea7-729">Sources and providers</span></span>

<span data-ttu-id="e9ea7-730">Podczas uruchamiania aplikacji źródła konfiguracji są odczytywane w kolejności, w jakiej są określone przez ich dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-730">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="e9ea7-731">Dostawcy konfiguracji implementujący funkcję wykrywania zmian mają możliwość ponownego załadowania konfiguracji, gdy ustawienie podstawowe zostanie zmienione.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-731">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="e9ea7-732">Na przykład dostawca konfiguracji plików (opisany w dalszej części tego tematu) i [dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) implementują wykrywanie zmian.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-732">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="e9ea7-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> jest dostępny w kontenerze [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e9ea7-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> można wstrzyknąć do Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> lub MVC <xref:Microsoft.AspNetCore.Mvc.Controller>, aby uzyskać konfigurację dla klasy.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="e9ea7-735">W poniższych przykładach pole `_config` służy do uzyskiwania dostępu do wartości konfiguracyjnych:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-735">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="e9ea7-736">Dostawcy konfiguracji nie mogą używać DI, ponieważ są niedostępne, gdy są skonfigurowane przez hosta.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-736">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="e9ea7-737">Klucze</span><span class="sxs-lookup"><span data-stu-id="e9ea7-737">Keys</span></span>

<span data-ttu-id="e9ea7-738">Klucze konfiguracji przyjmują następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-738">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="e9ea7-739">W kluczach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-739">Keys are case-insensitive.</span></span> <span data-ttu-id="e9ea7-740">Na przykład `ConnectionString` i `connectionstring` są traktowane jako klucze równoważne.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-740">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="e9ea7-741">Jeśli wartość tego samego klucza jest ustawiana przez tych samych lub różnych dostawców konfiguracji, Ostatnia wartość ustawiona w tym kluczu jest używana.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-741">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="e9ea7-742">Klucze hierarchiczne</span><span class="sxs-lookup"><span data-stu-id="e9ea7-742">Hierarchical keys</span></span>
  * <span data-ttu-id="e9ea7-743">W interfejsie API konfiguracji, separator dwukropek (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-743">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="e9ea7-744">W zmiennych środowiskowych separator dwukropek może nie zadziałał na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-744">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="e9ea7-745">Podwójne podkreślenie (`__`) jest obsługiwane przez wszystkie platformy i automatycznie konwertowane na dwukropek.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-745">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="e9ea7-746">W Azure Key Vault klucze hierarchiczne używają `--` (dwóch kresek) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-746">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="e9ea7-747">Napisz kod, aby zastąpić łączniki dwukropkiem, gdy wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-747">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="e9ea7-748"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-748">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e9ea7-749">Powiązanie tablicowe zostało opisane w sekcji [Powiązywanie tablicy z klasą](#bind-an-array-to-a-class) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-749">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="e9ea7-750">Wartości</span><span class="sxs-lookup"><span data-stu-id="e9ea7-750">Values</span></span>

<span data-ttu-id="e9ea7-751">Wartości konfiguracyjne przyjmują następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-751">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="e9ea7-752">Wartości są ciągami.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-752">Values are strings.</span></span>
* <span data-ttu-id="e9ea7-753">Wartości null nie można przechowywać w konfiguracji ani powiązana z obiektami.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-753">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="e9ea7-754">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="e9ea7-754">Providers</span></span>

<span data-ttu-id="e9ea7-755">W poniższej tabeli przedstawiono dostawców konfiguracji dostępnych do ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-755">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="e9ea7-756">Dostawca</span><span class="sxs-lookup"><span data-stu-id="e9ea7-756">Provider</span></span> | <span data-ttu-id="e9ea7-757">Zapewnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="e9ea7-757">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="e9ea7-758">[Dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) (tematy dotyczące*zabezpieczeń* )</span><span class="sxs-lookup"><span data-stu-id="e9ea7-758">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="e9ea7-759">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e9ea7-759">Azure Key Vault</span></span> |
| <span data-ttu-id="e9ea7-760">[Dostawca konfiguracji aplikacji platformy Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (dokumentacja platformy Azure)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-760">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="e9ea7-761">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="e9ea7-761">Azure App Configuration</span></span> |
| [<span data-ttu-id="e9ea7-762">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-762">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="e9ea7-763">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-763">Command-line parameters</span></span> |
| [<span data-ttu-id="e9ea7-764">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-764">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="e9ea7-765">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="e9ea7-765">Custom source</span></span> |
| [<span data-ttu-id="e9ea7-766">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="e9ea7-766">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="e9ea7-767">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="e9ea7-767">Environment variables</span></span> |
| [<span data-ttu-id="e9ea7-768">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="e9ea7-768">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="e9ea7-769">Pliki (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-769">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="e9ea7-770">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="e9ea7-770">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="e9ea7-771">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="e9ea7-771">Directory files</span></span> |
| [<span data-ttu-id="e9ea7-772">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="e9ea7-772">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="e9ea7-773">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="e9ea7-773">In-memory collections</span></span> |
| <span data-ttu-id="e9ea7-774">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tematy dotyczące*zabezpieczeń* )</span><span class="sxs-lookup"><span data-stu-id="e9ea7-774">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="e9ea7-775">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="e9ea7-775">File in the user profile directory</span></span> |

<span data-ttu-id="e9ea7-776">Źródła konfiguracji są odczytywane w kolejności, w jakiej dostawcy konfiguracji są określeni podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-776">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="e9ea7-777">Dostawcy konfiguracji opisane w tym temacie są opisane w kolejności alfabetycznej, a nie w kolejności, w jakiej kod ich rozmieszcza.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-777">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="e9ea7-778">Zamów dostawców konfiguracji w kodzie, aby odpowiadały priorytetom źródłowych źródeł konfiguracji wymaganych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-778">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="e9ea7-779">Typową sekwencją dostawców konfiguracji jest:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-779">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="e9ea7-780">Pliki (*appSettings. JSON*, *appSettings. { Environment}. JSON*, gdzie `{Environment}` jest bieżącym środowiskiem hostingu aplikacji)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-780">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="e9ea7-781">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e9ea7-781">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="e9ea7-782">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tylko środowisko programistyczne)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-782">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="e9ea7-783">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="e9ea7-783">Environment variables</span></span>
1. <span data-ttu-id="e9ea7-784">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-784">Command-line arguments</span></span>

<span data-ttu-id="e9ea7-785">Typowym celem jest umieszczenie dostawcy konfiguracji wiersza polecenia jako ostatni w serii dostawców, aby zezwolić na argumenty wiersza polecenia, aby przesłonić konfigurację ustawioną przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-785">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="e9ea7-786">Poprzednia sekwencja dostawców jest używana, gdy nowy Konstruktor hosta zostanie zainicjowany przy użyciu `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-786">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e9ea7-787">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-787">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="e9ea7-788">Konfigurowanie konstruktora hostów za pomocą UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="e9ea7-788">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="e9ea7-789">Aby skonfigurować konstruktora hostów, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> w konstruktorze hosta z konfiguracją.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-789">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="e9ea7-790">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="e9ea7-790">ConfigureAppConfiguration</span></span>

<span data-ttu-id="e9ea7-791">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić dostawców konfiguracji aplikacji oprócz tych dodanych automatycznie przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-791">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="e9ea7-792">Zastąp poprzednią konfigurację argumentami wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-792">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="e9ea7-793">Aby podać konfigurację aplikacji, którą można zastąpić za pomocą argumentów wiersza polecenia, wywołaj `AddCommandLine` Last:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-793">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="e9ea7-794">Usuń dostawców dodanych przez CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="e9ea7-794">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="e9ea7-795">Aby usunąć dostawców dodanych przez `CreateDefaultBuilder`, wywołaj najpierw polecenie [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) w [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="e9ea7-795">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="e9ea7-796">Użyj konfiguracji podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-796">Consume configuration during app startup</span></span>

<span data-ttu-id="e9ea7-797">Konfiguracja dostarczona do aplikacji w `ConfigureAppConfiguration` jest dostępna podczas uruchamiania aplikacji, w tym `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-797">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e9ea7-798">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja dostępu podczas uruchamiania](#access-configuration-during-startup) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-798">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="e9ea7-799">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-799">Command-line Configuration Provider</span></span>

<span data-ttu-id="e9ea7-800"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> ładuje konfigurację z par klucz-wartość argumentu wiersza polecenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-800">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="e9ea7-801">Aby uaktywnić konfigurację wiersza polecenia, Metoda rozszerzenia <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> jest wywoływana w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-801">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e9ea7-802">`AddCommandLine` jest wywoływana automatycznie, gdy zostanie wywołana `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-802">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="e9ea7-803">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-803">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e9ea7-804">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-804">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e9ea7-805">Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-805">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="e9ea7-806">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-806">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e9ea7-807">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-807">Environment variables.</span></span>

<span data-ttu-id="e9ea7-808">`CreateDefaultBuilder` dodaje dostawcę konfiguracji wiersza polecenia Last.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-808">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="e9ea7-809">Argumenty wiersza polecenia przekazane w czasie wykonywania zastępują konfigurację ustawioną przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-809">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="e9ea7-810">`CreateDefaultBuilder` działa, gdy host jest skonstruowany.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-810">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="e9ea7-811">W związku z tym konfiguracja wiersza polecenia aktywowana przez `CreateDefaultBuilder` może mieć wpływ na sposób konfigurowania hosta.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-811">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="e9ea7-812">W przypadku aplikacji opartych na ASP.NET Core szablonach `AddCommandLine` została już wywołana przez `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-812">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e9ea7-813">Aby dodać kolejnych dostawców konfiguracji i zachować możliwość przesłonięcia konfiguracji od tych dostawców za pomocą argumentów wiersza polecenia, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddCommandLine` Last.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-813">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="e9ea7-814">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-814">**Example**</span></span>

<span data-ttu-id="e9ea7-815">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-815">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="e9ea7-816">Otwórz wiersz polecenia w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-816">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="e9ea7-817">Podaj argument wiersza polecenia do polecenia `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-817">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="e9ea7-818">Po uruchomieniu aplikacji otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-818">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e9ea7-819">Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla argumentu wiersza polecenia konfiguracji dostarczonego do `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-819">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="e9ea7-820">Argumenty</span><span class="sxs-lookup"><span data-stu-id="e9ea7-820">Arguments</span></span>

<span data-ttu-id="e9ea7-821">Wartość musi następować po znaku równości (`=`) lub klucz musi mieć prefiks (`--` lub `/`), gdy wartość znajduje się w miejscu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-821">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="e9ea7-822">Wartość nie jest wymagana, jeśli jest używany znak równości (na przykład `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-822">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="e9ea7-823">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="e9ea7-823">Key prefix</span></span>               | <span data-ttu-id="e9ea7-824">Przykład</span><span class="sxs-lookup"><span data-stu-id="e9ea7-824">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="e9ea7-825">Brak prefiksu</span><span class="sxs-lookup"><span data-stu-id="e9ea7-825">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="e9ea7-826">Dwie kreski (`--`)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-826">Two dashes (`--`)</span></span>        | <span data-ttu-id="e9ea7-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="e9ea7-828">Ukośnik (`/`)</span><span class="sxs-lookup"><span data-stu-id="e9ea7-828">Forward slash (`/`)</span></span>      | <span data-ttu-id="e9ea7-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="e9ea7-830">W tym samym poleceniu nie należy mieszać par klucz-wartość argumentu wiersza polecenia, które używają znaku równości z parami klucz-wartość, które używają spacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-830">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="e9ea7-831">Przykładowe polecenia:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-831">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="e9ea7-832">Mapowanie przełączników</span><span class="sxs-lookup"><span data-stu-id="e9ea7-832">Switch mappings</span></span>

<span data-ttu-id="e9ea7-833">Mapowania przełączników Zezwalaj na logikę zamiany nazwy klucza.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-833">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="e9ea7-834">Podczas ręcznego kompilowania konfiguracji przy użyciu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>należy udostępnić słownik przemieszczeń Switch do metody <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-834">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="e9ea7-835">Gdy jest używany słownik mapowania przełączników, słownik jest sprawdzany dla klucza, który pasuje do klucza dostarczonego przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-835">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="e9ea7-836">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) zostanie przeniesiona z powrotem, aby ustawić parę klucz-wartość w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-836">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="e9ea7-837">Mapowanie przełącznika jest wymagane dla każdego klucza wiersza polecenia poprzedzonego jedną kreską (`-`).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-837">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="e9ea7-838">Przełącz reguły klucza słownika mapowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-838">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="e9ea7-839">Przełączniki muszą zaczynać się kreską (`-`) lub podwójną kreską (`--`).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-839">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="e9ea7-840">Słownik mapowania przełącznika nie może zawierać zduplikowanych kluczy.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-840">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="e9ea7-841">Utwórz słownik mapowań mapowania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-841">Create a switch mappings dictionary.</span></span> <span data-ttu-id="e9ea7-842">W poniższym przykładzie są tworzone dwa mapowania przełączników:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-842">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="e9ea7-843">Po skompilowaniu hosta Wywołaj `AddCommandLine` przy użyciu słownika mapowania przełączników:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-843">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="e9ea7-844">W przypadku aplikacji korzystających z mapowań przełączników wywołanie `CreateDefaultBuilder` nie powinno przekazywać argumentów.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-844">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="e9ea7-845">Wywołanie `AddCommandLine` metody `CreateDefaultBuilder` nie obejmuje zamapowanych przełączników i nie ma sposobu przekazywania słownika mapowania przełącznika do `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-845">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e9ea7-846">Rozwiązanie nie przekazuje argumentów do `CreateDefaultBuilder`, ale zamiast zezwalać `AddCommandLine` metody `ConfigurationBuilder` do przetwarzania zarówno argumentów, jak i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-846">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="e9ea7-847">Po utworzeniu słownika mapowań przełączników zawiera dane przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-847">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="e9ea7-848">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-848">Key</span></span>       | <span data-ttu-id="e9ea7-849">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-849">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="e9ea7-850">Jeśli klucze mapowane przez przełącznik są używane podczas uruchamiania aplikacji, konfiguracja otrzymuje wartość konfiguracji klucza dostarczonego przez słownik:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-850">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="e9ea7-851">Po uruchomieniu poprzedniego polecenia Konfiguracja zawiera wartości pokazane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-851">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="e9ea7-852">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-852">Key</span></span>               | <span data-ttu-id="e9ea7-853">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-853">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="e9ea7-854">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="e9ea7-854">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="e9ea7-855"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> ładuje konfigurację ze par klucz-wartość zmiennej środowiskowej w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-855">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="e9ea7-856">Aby uaktywnić konfigurację zmiennych środowiskowych, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-856">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="e9ea7-857">[Azure App Service](https://azure.microsoft.com/services/app-service/) umożliwia ustawianie zmiennych środowiskowych w witrynie Azure Portal, które mogą przesłonić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-857">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="e9ea7-858">Aby uzyskać więcej informacji, zobacz artykuł [Azure Apps: zastępowanie konfiguracji aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-858">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="e9ea7-859">`AddEnvironmentVariables` jest używany do ładowania zmiennych środowiskowych, które są poprzedzone `ASPNETCORE_` na potrzeby [konfiguracji hosta](#host-versus-app-configuration) , gdy nowy Konstruktor hosta zostanie zainicjowany przy użyciu [hosta sieci Web](xref:fundamentals/host/web-host) i zostanie wywołane `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-859">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="e9ea7-860">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-860">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e9ea7-861">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-861">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e9ea7-862">Konfiguracja aplikacji z nieoznaczonych zmiennych środowiskowych przez wywołanie `AddEnvironmentVariables` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-862">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="e9ea7-863">Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-863">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="e9ea7-864">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-864">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e9ea7-865">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-865">Command-line arguments.</span></span>

<span data-ttu-id="e9ea7-866">Dostawca konfiguracji zmiennych środowiskowych jest wywoływany po ustanowieniu konfiguracji z poziomu kluczy tajnych użytkownika i plików *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-866">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="e9ea7-867">Wywołanie dostawcy w tym miejscu pozwala odczytywać zmienne środowiskowe w czasie wykonywania w celu przesłania konfiguracji ustawionych przez klucze tajne użytkownika i pliki *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-867">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="e9ea7-868">Aby zapewnić konfigurację aplikacji na podstawie dodatkowych zmiennych środowiskowych, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddEnvironmentVariables` z prefiksem:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-868">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="e9ea7-869">Wywołaj `AddEnvironmentVariables` ostatnią, aby zezwolić na zmienne środowiskowe z danym prefiksem w celu przesłonięcia wartości z innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-869">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="e9ea7-870">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-870">**Example**</span></span>

<span data-ttu-id="e9ea7-871">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-871">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="e9ea7-872">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-872">Run the sample app.</span></span> <span data-ttu-id="e9ea7-873">Otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-873">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e9ea7-874">Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla zmiennej środowiskowej `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-874">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="e9ea7-875">Wartość odzwierciedla środowisko, w którym jest uruchomiona aplikacja, zwykle `Development` podczas uruchamiania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-875">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="e9ea7-876">Aby zachować listę zmiennych środowiskowych renderowanych przez aplikację, aplikacja filtruje zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-876">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="e9ea7-877">Zapoznaj się z plikiem przykładowej *strony aplikacji/index. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-877">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="e9ea7-878">Aby udostępnić wszystkie zmienne środowiskowe dostępne dla aplikacji, należy zmienić `FilteredConfiguration` na *stronie/index. cshtml. cs* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-878">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="e9ea7-879">Prefiksy</span><span class="sxs-lookup"><span data-stu-id="e9ea7-879">Prefixes</span></span>

<span data-ttu-id="e9ea7-880">Zmienne środowiskowe ładowane do konfiguracji aplikacji są filtrowane podczas dostarczania prefiksu do metody `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-880">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="e9ea7-881">Na przykład aby filtrować zmienne środowiskowe na prefiksie `CUSTOM_`, podaj prefiks dla dostawcy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-881">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="e9ea7-882">Prefiks jest usuwany podczas tworzenia par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-882">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="e9ea7-883">Podczas tworzenia konstruktora hostów Konfiguracja hosta jest zapewniana przez zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-883">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="e9ea7-884">Aby uzyskać więcej informacji na temat prefiksu używanego dla tych zmiennych środowiskowych, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-884">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e9ea7-885">**Prefiksy parametrów połączenia**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-885">**Connection string prefixes**</span></span>

<span data-ttu-id="e9ea7-886">Interfejs API konfiguracji ma specjalne reguły przetwarzania dla czterech zmiennych środowiskowych parametrów połączenia związanych z konfigurowaniem parametrów połączenia platformy Azure dla środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-886">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="e9ea7-887">Zmienne środowiskowe z prefiksami podanymi w tabeli są ładowane do aplikacji, jeśli nie podano prefiksu do `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-887">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="e9ea7-888">Prefiks parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="e9ea7-888">Connection string prefix</span></span> | <span data-ttu-id="e9ea7-889">Dostawca</span><span class="sxs-lookup"><span data-stu-id="e9ea7-889">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="e9ea7-890">Dostawca niestandardowy</span><span class="sxs-lookup"><span data-stu-id="e9ea7-890">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="e9ea7-891">MySQL</span><span class="sxs-lookup"><span data-stu-id="e9ea7-891">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="e9ea7-892">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e9ea7-892">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="e9ea7-893">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e9ea7-893">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="e9ea7-894">Gdy zmienna środowiskowa zostanie odnaleziona i załadowana do konfiguracji z dowolnymi z czterech prefiksów pokazanych w tabeli:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-894">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="e9ea7-895">Klucz konfiguracji jest tworzony przez usunięcie prefiksu zmiennej środowiskowej i dodanie sekcji klucza konfiguracji (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-895">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="e9ea7-896">Zostanie utworzona nowa para klucz-wartość konfiguracji, która reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, które nie ma określonego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-896">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="e9ea7-897">Klucz zmiennej środowiskowej</span><span class="sxs-lookup"><span data-stu-id="e9ea7-897">Environment variable key</span></span> | <span data-ttu-id="e9ea7-898">Przekonwertowany klucz konfiguracji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-898">Converted configuration key</span></span> | <span data-ttu-id="e9ea7-899">Wpis konfiguracji dostawcy</span><span class="sxs-lookup"><span data-stu-id="e9ea7-899">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="e9ea7-900">Wpis konfiguracji nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-900">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="e9ea7-901">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-901">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="e9ea7-902">Wartość:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-902">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="e9ea7-903">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-903">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="e9ea7-904">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-904">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="e9ea7-905">Klucz: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-905">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="e9ea7-906">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-906">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="e9ea7-907">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-907">**Example**</span></span>

<span data-ttu-id="e9ea7-908">Na serwerze zostanie utworzona niestandardowa zmienna środowiskowa parametrów połączenia:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-908">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="e9ea7-909">Nazwa &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-909">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="e9ea7-910">`Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True` &ndash; wartości</span><span class="sxs-lookup"><span data-stu-id="e9ea7-910">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="e9ea7-911">Jeśli `IConfiguration` zostanie dodany i przypisany do pola o nazwie `_config`, przeczytaj wartość:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-911">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="e9ea7-912">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="e9ea7-912">File Configuration Provider</span></span>

<span data-ttu-id="e9ea7-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową do ładowania konfiguracji z systemu plików.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="e9ea7-914">Następujący dostawcy konfiguracji są przydzielone do określonych typów plików:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-914">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="e9ea7-915">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="e9ea7-915">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="e9ea7-916">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="e9ea7-916">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="e9ea7-917">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="e9ea7-917">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="e9ea7-918">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="e9ea7-918">INI Configuration Provider</span></span>

<span data-ttu-id="e9ea7-919"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku INI w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-919">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="e9ea7-920">Aby uaktywnić konfigurację pliku INI, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-920">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e9ea7-921">Dwukropek może służyć jako ogranicznik sekcji w konfiguracji pliku INI.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-921">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="e9ea7-922">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-922">Overloads permit specifying:</span></span>

* <span data-ttu-id="e9ea7-923">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-923">Whether the file is optional.</span></span>
* <span data-ttu-id="e9ea7-924">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-924">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e9ea7-925"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-925">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e9ea7-926">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-926">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e9ea7-927">Ogólny przykład pliku konfiguracji INI:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-927">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="e9ea7-928">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-928">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e9ea7-929">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-929">section0:key0</span></span>
* <span data-ttu-id="e9ea7-930">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-930">section0:key1</span></span>
* <span data-ttu-id="e9ea7-931">Section1: podsekcja: Key</span><span class="sxs-lookup"><span data-stu-id="e9ea7-931">section1:subsection:key</span></span>
* <span data-ttu-id="e9ea7-932">section2: subsection0: klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-932">section2:subsection0:key</span></span>
* <span data-ttu-id="e9ea7-933">section2: subsection1: klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-933">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="e9ea7-934">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="e9ea7-934">JSON Configuration Provider</span></span>

<span data-ttu-id="e9ea7-935"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku JSON podczas środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-935">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="e9ea7-936">Aby uaktywnić konfigurację pliku JSON, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-936">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e9ea7-937">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-937">Overloads permit specifying:</span></span>

* <span data-ttu-id="e9ea7-938">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-938">Whether the file is optional.</span></span>
* <span data-ttu-id="e9ea7-939">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-939">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e9ea7-940"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-940">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e9ea7-941">`AddJsonFile` jest automatycznie wywoływana dwukrotnie, gdy nowy Konstruktor hosta zostanie zainicjowany z `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-941">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e9ea7-942">Metoda jest wywoływana w celu załadowania konfiguracji z:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-942">The method is called to load configuration from:</span></span>

* <span data-ttu-id="e9ea7-943">*appSettings. json* &ndash; ten plik jest najpierw odczytywany.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-943">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="e9ea7-944">Wersja środowiska pliku może przesłonić wartości dostarczone przez plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-944">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="e9ea7-945">*appSettings. {Environment}. JSON* &ndash; wersja środowiska pliku jest ładowana na podstawie [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-945">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="e9ea7-946">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-946">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e9ea7-947">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-947">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e9ea7-948">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-948">Environment variables.</span></span>
* <span data-ttu-id="e9ea7-949">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-949">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e9ea7-950">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-950">Command-line arguments.</span></span>

<span data-ttu-id="e9ea7-951">Dostawca konfiguracji JSON został ustanowiony jako pierwszy.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-951">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="e9ea7-952">W związku z tym klucze tajne użytkownika, zmienne środowiskowe i argumenty wiersza polecenia przesłaniają konfigurację ustawioną przez pliki *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-952">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="e9ea7-953">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji dla plików innych niż *appSettings. JSON* i *appSettings. { Environment}. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-953">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e9ea7-954">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-954">**Example**</span></span>

<span data-ttu-id="e9ea7-955">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje dwa wywołania `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-955">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="e9ea7-956">Pierwsze wywołanie `AddJsonFile` ładuje konfigurację z pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-956">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="e9ea7-957">Drugie wywołanie `AddJsonFile` ładuje konfigurację z *appSettings. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-957">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="e9ea7-958">Dla *appSettings. Plik Development. JSON* w przykładowej aplikacji jest ładowany:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-958">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="e9ea7-959">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-959">Run the sample app.</span></span> <span data-ttu-id="e9ea7-960">Otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-960">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e9ea7-961">Dane wyjściowe zawierają pary klucz-wartość dla konfiguracji w oparciu o środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-961">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="e9ea7-962">Poziom dziennika dla klucza `Logging:LogLevel:Default` jest `Debug` podczas uruchamiania aplikacji w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-962">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="e9ea7-963">Ponownie uruchom przykładową aplikację w środowisku produkcyjnym:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-963">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="e9ea7-964">Otwórz plik *Properties/profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-964">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="e9ea7-965">W profilu `ConfigurationSample` Zmień wartość zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` na `Production`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-965">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="e9ea7-966">Zapisz plik i uruchom aplikację za pomocą `dotnet run` w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-966">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="e9ea7-967">Ustawienia w *appSettings. Plik Development. JSON* nie zastępuje już ustawień w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-967">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="e9ea7-968">Poziom dziennika `Logging:LogLevel:Default` jest `Warning`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-968">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="e9ea7-969">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="e9ea7-969">XML Configuration Provider</span></span>

<span data-ttu-id="e9ea7-970"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-970">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="e9ea7-971">Aby uaktywnić konfigurację pliku XML, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-971">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e9ea7-972">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-972">Overloads permit specifying:</span></span>

* <span data-ttu-id="e9ea7-973">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-973">Whether the file is optional.</span></span>
* <span data-ttu-id="e9ea7-974">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-974">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e9ea7-975"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-975">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e9ea7-976">Węzeł główny pliku konfiguracji jest ignorowany, gdy są tworzone pary klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-976">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="e9ea7-977">Nie określaj definicji typu dokumentu (DTD) ani przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-977">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="e9ea7-978">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-978">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e9ea7-979">Pliki konfiguracji XML mogą używać odrębnych nazw elementów dla powtarzających się sekcji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-979">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="e9ea7-980">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-980">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e9ea7-981">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-981">section0:key0</span></span>
* <span data-ttu-id="e9ea7-982">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-982">section0:key1</span></span>
* <span data-ttu-id="e9ea7-983">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-983">section1:key0</span></span>
* <span data-ttu-id="e9ea7-984">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-984">section1:key1</span></span>

<span data-ttu-id="e9ea7-985">Powtarzalne elementy, które używają tej samej nazwy elementu, działają, jeśli atrybut `name` jest używany do odróżnienia elementów:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-985">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="e9ea7-986">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-986">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e9ea7-987">sekcja: section0: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-987">section:section0:key:key0</span></span>
* <span data-ttu-id="e9ea7-988">sekcja: section0: Key: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-988">section:section0:key:key1</span></span>
* <span data-ttu-id="e9ea7-989">sekcja: Section1: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-989">section:section1:key:key0</span></span>
* <span data-ttu-id="e9ea7-990">sekcja: Section1: Key: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-990">section:section1:key:key1</span></span>

<span data-ttu-id="e9ea7-991">Atrybuty mogą służyć do dostarczania wartości:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-991">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="e9ea7-992">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-992">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e9ea7-993">Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="e9ea7-993">key:attribute</span></span>
* <span data-ttu-id="e9ea7-994">sekcja: Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="e9ea7-994">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="e9ea7-995">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="e9ea7-995">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="e9ea7-996"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> używa plików katalogu jako par klucz konfiguracji i wartość.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-996">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="e9ea7-997">Kluczem jest nazwa pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-997">The key is the file name.</span></span> <span data-ttu-id="e9ea7-998">Wartość zawiera zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-998">The value contains the file's contents.</span></span> <span data-ttu-id="e9ea7-999">Dostawca konfiguracji klucza dla plików jest używany w scenariuszach hostingu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-999">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="e9ea7-1000">Aby uaktywnić konfigurację klucza dla plików, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1000">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="e9ea7-1001">`directoryPath` plików musi być ścieżką bezwzględną.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1001">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="e9ea7-1002">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1002">Overloads permit specifying:</span></span>

* <span data-ttu-id="e9ea7-1003">Delegat `Action<KeyPerFileConfigurationSource>`, który konfiguruje źródło.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1003">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="e9ea7-1004">Określa, czy katalog jest opcjonalny, i ścieżkę do katalogu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1004">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="e9ea7-1005">Podwójny znak podkreślenia (`__`) jest używany jako ogranicznik klucza konfiguracji w nazwach plików.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1005">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="e9ea7-1006">Na przykład nazwa pliku `Logging__LogLevel__System` generuje klucz konfiguracji `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1006">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="e9ea7-1007">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1007">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="e9ea7-1008">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1008">Memory Configuration Provider</span></span>

<span data-ttu-id="e9ea7-1009"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> używa kolekcji w pamięci jako par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1009">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="e9ea7-1010">Aby uaktywnić konfigurację kolekcji w pamięci, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1010">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e9ea7-1011">Dostawcę konfiguracji można zainicjować przy użyciu `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1011">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="e9ea7-1012">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1012">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="e9ea7-1013">W poniższym przykładzie tworzony jest słownik konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1013">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="e9ea7-1014">Słownik jest używany z wywołaniem `AddInMemoryCollection`, aby zapewnić konfigurację:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1014">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="e9ea7-1015">GetValue</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1015">GetValue</span></span>

<span data-ttu-id="e9ea7-1016">[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnia jedną wartość z konfiguracji z określonym kluczem i konwertuje ją na określony typ niekolekcje.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1016">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="e9ea7-1017">Przeciążenie akceptuje wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1017">An overload accepts a default value.</span></span>

<span data-ttu-id="e9ea7-1018">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1018">The following example:</span></span>

* <span data-ttu-id="e9ea7-1019">Wyodrębnia wartość ciągu z konfiguracji z kluczem `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1019">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="e9ea7-1020">Jeśli nie można odnaleźć `NumberKey` w kluczach konfiguracji, zostanie użyta wartość domyślna `99`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1020">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="e9ea7-1021">Typ wartości jako `int`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1021">Types the value as an `int`.</span></span>
* <span data-ttu-id="e9ea7-1022">Przechowuje wartość we właściwości `NumberConfig` do użycia na stronie.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1022">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="e9ea7-1023">GetSection, GetChildren i EXISTS</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1023">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="e9ea7-1024">W poniższych przykładach należy wziąć pod uwagę następujący plik JSON.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1024">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="e9ea7-1025">Cztery klucze są dostępne w dwóch sekcjach, z których jedna zawiera parę podsekcji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1025">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="e9ea7-1026">Gdy plik jest odczytywany do konfiguracji, następujące unikatowe klucze hierarchiczne są tworzone w celu przechowywania wartości konfiguracyjnych:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1026">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="e9ea7-1027">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1027">section0:key0</span></span>
* <span data-ttu-id="e9ea7-1028">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1028">section0:key1</span></span>
* <span data-ttu-id="e9ea7-1029">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1029">section1:key0</span></span>
* <span data-ttu-id="e9ea7-1030">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1030">section1:key1</span></span>
* <span data-ttu-id="e9ea7-1031">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1031">section2:subsection0:key0</span></span>
* <span data-ttu-id="e9ea7-1032">section2: subsection0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1032">section2:subsection0:key1</span></span>
* <span data-ttu-id="e9ea7-1033">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1033">section2:subsection1:key0</span></span>
* <span data-ttu-id="e9ea7-1034">section2: subsection1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1034">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="e9ea7-1035">GetSection</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1035">GetSection</span></span>

<span data-ttu-id="e9ea7-1036">[IConfiguration. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcję konfiguracji z określonym kluczem podsekcji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="e9ea7-1037">Aby zwrócić <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz-wartość w `section1`, wywołaj `GetSection` i podaj nazwę sekcji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1037">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="e9ea7-1038">`configSection` nie ma wartości, tylko klucza i ścieżki.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1038">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="e9ea7-1039">Podobnie, aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę sekcji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1039">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="e9ea7-1040">`GetSection` nigdy nie zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1040">`GetSection` never returns `null`.</span></span> <span data-ttu-id="e9ea7-1041">Jeśli nie znaleziono pasującej sekcji, zwracany jest pusty `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1041">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="e9ea7-1042">Gdy `GetSection` zwraca pasującą sekcję, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1042">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="e9ea7-1043"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> i <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> są zwracane, gdy istnieje sekcja.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1043">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="e9ea7-1044">GetChildren</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1044">GetChildren</span></span>

<span data-ttu-id="e9ea7-1045">Wywołanie [iConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) w `section2` uzyskuje `IEnumerable<IConfigurationSection>` obejmujący:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1045">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="e9ea7-1046">Exists</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1046">Exists</span></span>

<span data-ttu-id="e9ea7-1047">Użyj [ConfigurationExtensions. istnieje](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) , aby określić, czy istnieje sekcja konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1047">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="e9ea7-1048">Dane przykładowe `sectionExists` jest `false`, ponieważ w danych konfiguracyjnych nie ma `section2:subsection2` sekcji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1048">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="e9ea7-1049">Powiąż z klasą</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1049">Bind to a class</span></span>

<span data-ttu-id="e9ea7-1050">Konfigurację można powiązać z klasami, które reprezentują grupy powiązanych ustawień przy użyciu *wzorca opcji*.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1050">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="e9ea7-1051">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1051">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e9ea7-1052">Wartości konfiguracji są zwracane jako ciągi, ale wywołanie <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie obiektów [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) .</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1052">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="e9ea7-1053">Obiekt wiążący wiąże wartości ze wszystkimi publicznymi właściwościami odczytu/zapisu dostarczonego typu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1053">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="e9ea7-1054">Pola **nie** są powiązane.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1054">Fields are **not** bound.</span></span>

<span data-ttu-id="e9ea7-1055">Przykładowa aplikacja zawiera model `Starship` (*modele/Starship. cs*):</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1055">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="e9ea7-1056">Sekcja `starship` pliku *Starship. JSON* tworzy konfigurację, gdy aplikacja Przykładowa używa dostawcy konfiguracji JSON do załadowania konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1056">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="e9ea7-1057">Tworzone są następujące pary klucz-wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1057">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="e9ea7-1058">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1058">Key</span></span>                   | <span data-ttu-id="e9ea7-1059">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1059">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="e9ea7-1060">Starship: Nazwa</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1060">starship:name</span></span>         | <span data-ttu-id="e9ea7-1061">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1061">USS Enterprise</span></span>                                    |
| <span data-ttu-id="e9ea7-1062">Starship: Rejestr</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1062">starship:registry</span></span>     | <span data-ttu-id="e9ea7-1063">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1063">NCC-1701</span></span>                                          |
| <span data-ttu-id="e9ea7-1064">Starship: Klasa</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1064">starship:class</span></span>        | <span data-ttu-id="e9ea7-1065">Skład</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1065">Constitution</span></span>                                      |
| <span data-ttu-id="e9ea7-1066">Starship: Długość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1066">starship:length</span></span>       | <span data-ttu-id="e9ea7-1067">304,8</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1067">304.8</span></span>                                             |
| <span data-ttu-id="e9ea7-1068">Starship: prowizja</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1068">starship:commissioned</span></span> | <span data-ttu-id="e9ea7-1069">False</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1069">False</span></span>                                             |
| <span data-ttu-id="e9ea7-1070">handlowych</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1070">trademark</span></span>             | <span data-ttu-id="e9ea7-1071">Najważniejsze obrazy Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1071">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="e9ea7-1072">Przykładowa aplikacja wywołuje `GetSection` z kluczem `starship`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1072">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="e9ea7-1073">Pary klucz-wartość `starship` są odizolowane.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1073">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="e9ea7-1074">Metoda `Bind` jest wywoływana w podsekcji przekazującej w wystąpieniu klasy `Starship`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1074">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="e9ea7-1075">Po powiązaniu wartości wystąpień wystąpienie jest przypisywane do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1075">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="e9ea7-1076">Powiąż z grafem obiektów</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1076">Bind to an object graph</span></span>

<span data-ttu-id="e9ea7-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> jest w stanie powiązać cały Graf obiektów POCO.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="e9ea7-1078">Podobnie jak w przypadku powiązania prostego obiektu, powiązane są tylko publiczne właściwości odczytu i zapisu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1078">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="e9ea7-1079">Przykład zawiera model `TvShow`, którego wykres obiektu zawiera klasy `Metadata` i `Actors` (*modele/TvShow. cs*):</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1079">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="e9ea7-1080">Przykładowa aplikacja zawiera plik *tvshow. XML* zawierający dane konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1080">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="e9ea7-1081">Konfiguracja jest powiązana z całym grafem obiektu `TvShow` za pomocą metody `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1081">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="e9ea7-1082">Powiązane wystąpienie jest przypisane do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1082">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="e9ea7-1083">[ConfigurationBinder. Get\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) tworzy powiązania i zwraca określony typ.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="e9ea7-1084">`Get<T>` jest wygodniejszy niż korzystanie z `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1084">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="e9ea7-1085">Poniższy kod pokazuje, jak używać `Get<T>` w poprzednim przykładzie, co umożliwia bezpośrednie przypisanie wystąpienia powiązanego do właściwości używanej do renderowania:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1085">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="e9ea7-1086">Powiąż tablicę z klasą</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1086">Bind an array to a class</span></span>

<span data-ttu-id="e9ea7-1087">*Przykładowa aplikacja pokazuje Koncepcje opisane w tej sekcji.*</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1087">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="e9ea7-1088"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1088">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e9ea7-1089">Każdy format tablicy, który ujawnia segment klucza numerycznego (`:0:`, `:1:`, &hellip; `:{n}:`), jest w stanie powiązać powiązanie tablicową z tablicą klas POCO.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1089">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="e9ea7-1090">Powiązanie jest dostarczane według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1090">Binding is provided by convention.</span></span> <span data-ttu-id="e9ea7-1091">Niestandardowi dostawcy konfiguracji nie muszą implementować powiązania tablicy.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1091">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="e9ea7-1092">**Przetwarzanie tablicy w pamięci**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1092">**In-memory array processing**</span></span>

<span data-ttu-id="e9ea7-1093">Należy wziąć pod uwagę klucze konfiguracji i wartości podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1093">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="e9ea7-1094">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1094">Key</span></span>             | <span data-ttu-id="e9ea7-1095">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1095">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="e9ea7-1096">Tablica: wpisy: 0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1096">array:entries:0</span></span> | <span data-ttu-id="e9ea7-1097">value0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1097">value0</span></span> |
| <span data-ttu-id="e9ea7-1098">Tablica: wpisy: 1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1098">array:entries:1</span></span> | <span data-ttu-id="e9ea7-1099">Sekwencj</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1099">value1</span></span> |
| <span data-ttu-id="e9ea7-1100">Tablica: wpisy: 2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1100">array:entries:2</span></span> | <span data-ttu-id="e9ea7-1101">Wartość2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1101">value2</span></span> |
| <span data-ttu-id="e9ea7-1102">Tablica: wpisy: 4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1102">array:entries:4</span></span> | <span data-ttu-id="e9ea7-1103">value4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1103">value4</span></span> |
| <span data-ttu-id="e9ea7-1104">Tablica: wpisy: 5</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1104">array:entries:5</span></span> | <span data-ttu-id="e9ea7-1105">value5</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1105">value5</span></span> |

<span data-ttu-id="e9ea7-1106">Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1106">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="e9ea7-1107">Tablica pomija wartość indeksu &num;3.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1107">The array skips a value for index &num;3.</span></span> <span data-ttu-id="e9ea7-1108">Segregator konfiguracji nie może powiązać wartości null ani tworzyć wpisów o wartości null w obiektach powiązanych, co oznacza, że w chwili pojawi się wynik powiązania tej tablicy z obiektem.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1108">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="e9ea7-1109">W przykładowej aplikacji jest dostępna Klasa POCO, która przechowuje powiązane dane konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1109">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="e9ea7-1110">Dane konfiguracji są powiązane z obiektem:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1110">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="e9ea7-1111">[ConfigurationBinder. Get\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) można również użyć składni, co spowoduje zwiększenie kodu kompaktowego:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1111">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="e9ea7-1112">Obiekt powiązany, wystąpienie `ArrayExample`, otrzymuje dane tablicy z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1112">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="e9ea7-1113">Indeks `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1113">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="e9ea7-1114">Wartość `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1114">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="e9ea7-1115">0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1115">0</span></span>                            | <span data-ttu-id="e9ea7-1116">value0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1116">value0</span></span>                       |
| <span data-ttu-id="e9ea7-1117">1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1117">1</span></span>                            | <span data-ttu-id="e9ea7-1118">Sekwencj</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1118">value1</span></span>                       |
| <span data-ttu-id="e9ea7-1119">2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1119">2</span></span>                            | <span data-ttu-id="e9ea7-1120">Wartość2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1120">value2</span></span>                       |
| <span data-ttu-id="e9ea7-1121">3</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1121">3</span></span>                            | <span data-ttu-id="e9ea7-1122">value4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1122">value4</span></span>                       |
| <span data-ttu-id="e9ea7-1123">4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1123">4</span></span>                            | <span data-ttu-id="e9ea7-1124">value5</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1124">value5</span></span>                       |

<span data-ttu-id="e9ea7-1125">Indeks &num;3 w obiekcie powiązanym zawiera dane konfiguracyjne `array:4` klucza konfiguracji i jego wartość `value4`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1125">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="e9ea7-1126">Gdy dane konfiguracji zawierające tablicę są powiązane, indeksy tablic w kluczach konfiguracji są używane tylko do iteracji danych konfiguracji podczas tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1126">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="e9ea7-1127">Wartości null nie można zachować w danych konfiguracyjnych, a wpis o wartości null nie jest tworzony w obiekcie powiązanym, gdy tablica w kluczach konfiguracji pomija jeden lub więcej indeksów.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1127">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="e9ea7-1128">Brakujący element konfiguracji dla indeksu &num;3 można dostarczyć przed powiązaniem do wystąpienia `ArrayExample` przez dowolnego dostawcę konfiguracji, który wygeneruje poprawną parę klucz-wartość w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1128">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="e9ea7-1129">Jeśli przykład zawiera dodatkowego dostawcę konfiguracji JSON z brakującą parą klucz-wartość, `ArrayExample.Entries` pasuje do kompletnej tablicy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1129">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="e9ea7-1130">plik *missing_value. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1130">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="e9ea7-1131">W pliku `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1131">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="e9ea7-1132">Para klucz-wartość pokazana w tabeli jest ładowana do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1132">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="e9ea7-1133">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1133">Key</span></span>             | <span data-ttu-id="e9ea7-1134">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1134">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="e9ea7-1135">Tablica: wpisy: 3</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1135">array:entries:3</span></span> | <span data-ttu-id="e9ea7-1136">Wartość3</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1136">value3</span></span> |

<span data-ttu-id="e9ea7-1137">Jeśli wystąpienie klasy `ArrayExample` jest powiązane, gdy dostawca konfiguracji JSON zawiera wpis dla indeksu &num;3, tablica `ArrayExample.Entries` zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1137">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="e9ea7-1138">Indeks `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1138">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="e9ea7-1139">Wartość `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1139">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="e9ea7-1140">0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1140">0</span></span>                            | <span data-ttu-id="e9ea7-1141">value0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1141">value0</span></span>                       |
| <span data-ttu-id="e9ea7-1142">1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1142">1</span></span>                            | <span data-ttu-id="e9ea7-1143">Sekwencj</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1143">value1</span></span>                       |
| <span data-ttu-id="e9ea7-1144">2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1144">2</span></span>                            | <span data-ttu-id="e9ea7-1145">Wartość2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1145">value2</span></span>                       |
| <span data-ttu-id="e9ea7-1146">3</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1146">3</span></span>                            | <span data-ttu-id="e9ea7-1147">Wartość3</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1147">value3</span></span>                       |
| <span data-ttu-id="e9ea7-1148">4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1148">4</span></span>                            | <span data-ttu-id="e9ea7-1149">value4</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1149">value4</span></span>                       |
| <span data-ttu-id="e9ea7-1150">5</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1150">5</span></span>                            | <span data-ttu-id="e9ea7-1151">value5</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1151">value5</span></span>                       |

<span data-ttu-id="e9ea7-1152">**Przetwarzanie tablicy JSON**</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1152">**JSON array processing**</span></span>

<span data-ttu-id="e9ea7-1153">Jeśli plik JSON zawiera tablicę, klucze konfiguracji są tworzone dla elementów tablicy z indeksem sekcji o wartości zero.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1153">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="e9ea7-1154">W poniższym pliku konfiguracyjnym `subsection` jest tablicą:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1154">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="e9ea7-1155">Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz-wartość:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1155">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="e9ea7-1156">Klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1156">Key</span></span>                     | <span data-ttu-id="e9ea7-1157">Wartość</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1157">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="e9ea7-1158">json_array: klucz</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1158">json_array:key</span></span>          | <span data-ttu-id="e9ea7-1159">wartośća</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1159">valueA</span></span> |
| <span data-ttu-id="e9ea7-1160">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1160">json_array:subsection:0</span></span> | <span data-ttu-id="e9ea7-1161">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1161">valueB</span></span> |
| <span data-ttu-id="e9ea7-1162">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1162">json_array:subsection:1</span></span> | <span data-ttu-id="e9ea7-1163">valueC</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1163">valueC</span></span> |
| <span data-ttu-id="e9ea7-1164">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1164">json_array:subsection:2</span></span> | <span data-ttu-id="e9ea7-1165">Znajdując</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1165">valueD</span></span> |

<span data-ttu-id="e9ea7-1166">W przykładowej aplikacji jest dostępna następująca Klasa POCO z powiązaniem par klucz-wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1166">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="e9ea7-1167">Po powiązaniu `JsonArrayExample.Key` utrzymuje `valueA`wartości.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1167">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="e9ea7-1168">Wartości podsekcji są przechowywane we właściwości tablicy POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1168">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="e9ea7-1169">Indeks `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1169">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="e9ea7-1170">Wartość `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1170">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="e9ea7-1171">0</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1171">0</span></span>                                   | <span data-ttu-id="e9ea7-1172">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1172">valueB</span></span>                              |
| <span data-ttu-id="e9ea7-1173">1</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1173">1</span></span>                                   | <span data-ttu-id="e9ea7-1174">valueC</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1174">valueC</span></span>                              |
| <span data-ttu-id="e9ea7-1175">2</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1175">2</span></span>                                   | <span data-ttu-id="e9ea7-1176">Znajdując</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1176">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="e9ea7-1177">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1177">Custom configuration provider</span></span>

<span data-ttu-id="e9ea7-1178">Przykładowa aplikacja pokazuje, jak utworzyć podstawowego dostawcę konfiguracji, który odczytuje pary klucz-wartość konfiguracji z bazy danych przy użyciu [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1178">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="e9ea7-1179">Dostawca ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1179">The provider has the following characteristics:</span></span>

* <span data-ttu-id="e9ea7-1180">Baza danych EF w pamięci jest używana w celach demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1180">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="e9ea7-1181">Aby użyć bazy danych, która wymaga parametrów połączenia, zaimplementuj pomocniczą `ConfigurationBuilder`, aby podać parametry połączenia od innego dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1181">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="e9ea7-1182">Dostawca odczytuje tabelę bazy danych w konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1182">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="e9ea7-1183">Dostawca nie wykonuje zapytania do bazy danych w oparciu o klucz.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1183">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="e9ea7-1184">Ponowne załadowanie nie zostało zaimplementowane, więc aktualizacja bazy danych po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1184">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="e9ea7-1185">Zdefiniuj jednostkę `EFConfigurationValue` do przechowywania wartości konfiguracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1185">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="e9ea7-1186">*Modele/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1186">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="e9ea7-1187">Dodaj `EFConfigurationContext` do przechowywania skonfigurowanych wartości i uzyskiwania do nich dostępu.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1187">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="e9ea7-1188">*EFConfigurationProvider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1188">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="e9ea7-1189">Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1189">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="e9ea7-1190">*EFConfigurationProvider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1190">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="e9ea7-1191">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1191">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="e9ea7-1192">Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1192">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="e9ea7-1193">*EFConfigurationProvider/EFConfigurationProvider. cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="e9ea7-1194">Metoda rozszerzenia `AddEFConfiguration` zezwala na Dodawanie źródła konfiguracji do `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1194">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="e9ea7-1195">*Rozszerzenia/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1195">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="e9ea7-1196">Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1196">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="e9ea7-1197">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1197">Access configuration during startup</span></span>

<span data-ttu-id="e9ea7-1198">Wsuń `IConfiguration` do konstruktora `Startup`, aby uzyskać dostęp do wartości konfiguracyjnych w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1198">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e9ea7-1199">Aby uzyskać dostęp do konfiguracji w `Startup.Configure`, należy wstrzyknąć `IConfiguration` bezpośrednio do metody lub użyć wystąpienia z konstruktora:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1199">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="e9ea7-1200">Aby zapoznać się z przykładem uzyskiwania dostępu do konfiguracji przy użyciu metod uruchamiania, zobacz [Uruchamianie aplikacji: wygodne metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1200">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="e9ea7-1201">Konfiguracja dostępu na stronie Razor Pages lub widoku MVC</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1201">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="e9ea7-1202">Aby uzyskać dostęp do ustawień konfiguracji na stronie Razor Pages lub widoku MVC, Dodaj [dyrektywę using](xref:mvc/views/razor#using) ([ C# odwołanie: Using](/dotnet/csharp/language-reference/keywords/using-directive)) dla [przestrzeni nazw Microsoft. Extensions. Configuration](xref:Microsoft.Extensions.Configuration) i wsuń <xref:Microsoft.Extensions.Configuration.IConfiguration> do strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1202">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="e9ea7-1203">Na stronie Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1203">In a Razor Pages page:</span></span>

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

<span data-ttu-id="e9ea7-1204">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1204">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="e9ea7-1205">Dodawanie konfiguracji z zestawu zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1205">Add configuration from an external assembly</span></span>

<span data-ttu-id="e9ea7-1206">Implementacja <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zestawu zewnętrznego poza klasą `Startup` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1206">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="e9ea7-1207">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1207">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e9ea7-1208">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e9ea7-1208">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
