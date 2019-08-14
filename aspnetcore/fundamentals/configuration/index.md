---
title: Konfiguracja w ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core przy użyciu interfejsu API konfiguracji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 5723295c70f8d893f758ca5dc87180c6b707f493
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994169"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="0c531-103">Konfiguracja w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c531-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="0c531-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0c531-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0c531-105">Konfiguracja aplikacji w ASP.NET Core jest oparta na parach klucz-wartość określonych przez *dostawców konfiguracji*.</span><span class="sxs-lookup"><span data-stu-id="0c531-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="0c531-106">Dostawcy konfiguracji odczytują dane konfiguracji do par klucz-wartość z różnych źródeł konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="0c531-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="0c531-107">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0c531-107">Azure Key Vault</span></span>
* <span data-ttu-id="0c531-108">Konfiguracja aplikacji platformy Azure</span><span class="sxs-lookup"><span data-stu-id="0c531-108">Azure App Configuration</span></span>
* <span data-ttu-id="0c531-109">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="0c531-109">Command-line arguments</span></span>
* <span data-ttu-id="0c531-110">Dostawcy niestandardowi (instalowani lub utworzony)</span><span class="sxs-lookup"><span data-stu-id="0c531-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="0c531-111">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="0c531-111">Directory files</span></span>
* <span data-ttu-id="0c531-112">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="0c531-112">Environment variables</span></span>
* <span data-ttu-id="0c531-113">Obiekty w pamięci .NET</span><span class="sxs-lookup"><span data-stu-id="0c531-113">In-memory .NET objects</span></span>
* <span data-ttu-id="0c531-114">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="0c531-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0c531-115">Pakiety konfiguracyjne dla typowych scenariuszy dostawcy konfiguracji ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) są dołączone niejawnie przez platformę.</span><span class="sxs-lookup"><span data-stu-id="0c531-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0c531-116">Pakiety konfiguracyjne dla typowych scenariuszy dostawcy konfiguracji ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0c531-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="0c531-117">Przykłady kodu, które obserwują i w przykładowej aplikacji <xref:Microsoft.Extensions.Configuration> używają przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="0c531-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="0c531-118">*Wzorzec opcji* jest rozszerzeniem pojęć konfiguracyjnych opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="0c531-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="0c531-119">Opcje używają klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="0c531-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="0c531-120">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="0c531-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="0c531-121">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0c531-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="0c531-122">Host a konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="0c531-122">Host versus app configuration</span></span>

<span data-ttu-id="0c531-123">Przed skonfigurowaniem i uruchomieniem aplikacji *host* zostanie skonfigurowany i uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="0c531-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="0c531-124">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="0c531-125">Zarówno aplikacja, jak i Host są konfigurowane przy użyciu dostawców konfiguracji opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="0c531-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="0c531-126">Klucz konfiguracji hosta — pary wartości są również uwzględnione w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="0c531-127">Aby uzyskać więcej informacji na temat tego, jak dostawcy konfiguracji są używani podczas kompilowania hosta i jak źródła konfiguracji wpływają na <xref:fundamentals/index#host>konfigurację hosta, zobacz.</span><span class="sxs-lookup"><span data-stu-id="0c531-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="0c531-128">Konfiguracja domyślna</span><span class="sxs-lookup"><span data-stu-id="0c531-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0c531-129">Aplikacje sieci Web oparte na ASP.NET Coreniu [nowych](/dotnet/core/tools/dotnet-new) szablonów dotnet <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> są wywoływane podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="0c531-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="0c531-130">`CreateDefaultBuilder`zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="0c531-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="0c531-131">Poniższe zasady dotyczą aplikacji korzystających z [hosta ogólnego](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="0c531-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="0c531-132">Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej podczas korzystania z [hosta sieci Web](xref:fundamentals/host/web-host), zobacz [wersję ASP.NET Core 2,2 tego tematu](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="0c531-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="0c531-133">Konfiguracja hosta jest poświadczona z:</span><span class="sxs-lookup"><span data-stu-id="0c531-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="0c531-134">Zmienne środowiskowe poprzedzone `DOTNET_` znakiem (na `DOTNET_ENVIRONMENT`przykład) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="0c531-135">Prefiks (`DOTNET_`) jest usuwany, gdy są ładowane pary klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="0c531-136">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="0c531-137">Konfiguracja domyślna hosta sieci Web (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="0c531-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="0c531-138">Kestrel jest używany jako serwer sieci Web i konfigurowany przy użyciu dostawców konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="0c531-139">Dodaj oprogramowanie pośredniczące do filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="0c531-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="0c531-140">Dodaj przekierowane nagłówki — oprogramowanie pośredniczące, `ASPNETCORE_FORWARDEDHEADERS_ENABLED` Jeśli zmienna środowiskowa jest ustawiona na. `true`</span><span class="sxs-lookup"><span data-stu-id="0c531-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="0c531-141">Włącz integrację usług IIS.</span><span class="sxs-lookup"><span data-stu-id="0c531-141">Enable IIS integration.</span></span>
* <span data-ttu-id="0c531-142">Podano konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="0c531-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="0c531-143">*appSettings. JSON* przy użyciu [dostawcy konfiguracji plików](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="0c531-144">*appSettings. {Environment}. JSON* przy użyciu [dostawcy konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="0c531-145">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana `Development` w środowisku przy użyciu zestawu wpisów.</span><span class="sxs-lookup"><span data-stu-id="0c531-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="0c531-146">Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="0c531-147">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0c531-148">Aplikacje sieci Web oparte na ASP.NET Coreniu [nowych](/dotnet/core/tools/dotnet-new) szablonów dotnet <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> są wywoływane podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="0c531-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="0c531-149">`CreateDefaultBuilder`zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="0c531-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="0c531-150">Poniższe zasady dotyczą aplikacji korzystających z [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="0c531-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="0c531-151">Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej w przypadku korzystania z [hosta ogólnego](xref:fundamentals/host/generic-host), zobacz [najnowszą wersję tego tematu](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="0c531-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="0c531-152">Konfiguracja hosta jest poświadczona z:</span><span class="sxs-lookup"><span data-stu-id="0c531-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="0c531-153">Zmienne środowiskowe poprzedzone `ASPNETCORE_` znakiem (na `ASPNETCORE_ENVIRONMENT`przykład) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="0c531-154">Prefiks (`ASPNETCORE_`) jest usuwany, gdy są ładowane pary klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="0c531-155">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="0c531-156">Podano konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="0c531-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="0c531-157">*appSettings. JSON* przy użyciu [dostawcy konfiguracji plików](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="0c531-158">*appSettings. {Environment}. JSON* przy użyciu [dostawcy konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="0c531-159">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana `Development` w środowisku przy użyciu zestawu wpisów.</span><span class="sxs-lookup"><span data-stu-id="0c531-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="0c531-160">Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="0c531-161">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c531-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="0c531-162">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="0c531-162">Security</span></span>

<span data-ttu-id="0c531-163">Aby zabezpieczyć poufne dane konfiguracji, należy zastosować następujące rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="0c531-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="0c531-164">Nie należy przechowywać haseł ani innych danych poufnych w kodzie dostawcy konfiguracji ani w plikach konfiguracji zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="0c531-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="0c531-165">Nie używaj tajemnic produkcyjnych w środowiskach deweloperskich i testowych.</span><span class="sxs-lookup"><span data-stu-id="0c531-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="0c531-166">Określ wpisy tajne poza projektem, aby nie mogły zostać przypadkowo przekazane do repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="0c531-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="0c531-167">Więcej informacji znajduje się w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="0c531-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="0c531-168"><xref:security/app-secrets>&ndash; Zawiera porady dotyczące używania zmiennych środowiskowych do przechowywania poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="0c531-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="0c531-169">Menedżer wpisów tajnych używa dostawcy konfiguracji plików do przechowywania wpisów tajnych użytkownika w pliku JSON w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="0c531-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="0c531-170">Dostawca konfiguracji plików został opisany w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="0c531-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="0c531-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) bezpieczne przechowywanie wpisów tajnych aplikacji dla ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="0c531-172">Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0c531-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="0c531-173">Hierarchiczne dane konfiguracji</span><span class="sxs-lookup"><span data-stu-id="0c531-173">Hierarchical configuration data</span></span>

<span data-ttu-id="0c531-174">Interfejs API konfiguracji jest w stanie utrzymywać hierarchiczne dane konfiguracji przez spłaszczonie danych hierarchicznych przy użyciu ogranicznika w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="0c531-175">W poniższym pliku JSON cztery klucze istnieją w hierarchii strukturalnej dwóch sekcji:</span><span class="sxs-lookup"><span data-stu-id="0c531-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="0c531-176">Gdy plik jest odczytywany do konfiguracji, są tworzone unikatowe klucze, aby zachować oryginalną hierarchiczną strukturę danych źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="0c531-177">Sekcje i klucze są spłaszczone przy użyciu dwukropka (`:`), aby zachować oryginalną strukturę:</span><span class="sxs-lookup"><span data-stu-id="0c531-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="0c531-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0c531-178">section0:key0</span></span>
* <span data-ttu-id="0c531-179">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="0c531-179">section0:key1</span></span>
* <span data-ttu-id="0c531-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="0c531-180">section1:key0</span></span>
* <span data-ttu-id="0c531-181">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="0c531-181">section1:key1</span></span>

<span data-ttu-id="0c531-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>metody <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> i są dostępne do izolowania sekcji i elementów podrzędnych sekcji w danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="0c531-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="0c531-183">Te metody są opisane w dalszej [części GetSection, GetChildren i EXISTS](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="0c531-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="0c531-184">Konwencje</span><span class="sxs-lookup"><span data-stu-id="0c531-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="0c531-185">Źródła i dostawcy</span><span class="sxs-lookup"><span data-stu-id="0c531-185">Sources and providers</span></span>

<span data-ttu-id="0c531-186">Podczas uruchamiania aplikacji źródła konfiguracji są odczytywane w kolejności, w jakiej są określone przez ich dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="0c531-187">Dostawcy konfiguracji implementujący funkcję wykrywania zmian mają możliwość ponownego załadowania konfiguracji, gdy ustawienie podstawowe zostanie zmienione.</span><span class="sxs-lookup"><span data-stu-id="0c531-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="0c531-188">Na przykład dostawca konfiguracji plików (opisany w dalszej części tego tematu) i [dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) implementują wykrywanie zmian.</span><span class="sxs-lookup"><span data-stu-id="0c531-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="0c531-189"><xref:Microsoft.Extensions.Configuration.IConfiguration>jest dostępny w kontenerze [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="0c531-190"><xref:Microsoft.Extensions.Configuration.IConfiguration>można wstrzyknąć do Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> , aby uzyskać konfigurację dla klasy:</span><span class="sxs-lookup"><span data-stu-id="0c531-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

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

<span data-ttu-id="0c531-191">Dostawcy konfiguracji nie mogą używać DI, ponieważ są niedostępne, gdy są skonfigurowane przez hosta.</span><span class="sxs-lookup"><span data-stu-id="0c531-191">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="0c531-192">Ponownie</span><span class="sxs-lookup"><span data-stu-id="0c531-192">Keys</span></span>

<span data-ttu-id="0c531-193">Klucze konfiguracji przyjmują następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="0c531-193">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="0c531-194">W kluczach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="0c531-194">Keys are case-insensitive.</span></span> <span data-ttu-id="0c531-195">Na przykład `ConnectionString` i `connectionstring` są traktowane jako równoważne klucze.</span><span class="sxs-lookup"><span data-stu-id="0c531-195">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="0c531-196">Jeśli wartość tego samego klucza jest ustawiana przez tych samych lub różnych dostawców konfiguracji, Ostatnia wartość ustawiona w tym kluczu jest używana.</span><span class="sxs-lookup"><span data-stu-id="0c531-196">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="0c531-197">Klucze hierarchiczne</span><span class="sxs-lookup"><span data-stu-id="0c531-197">Hierarchical keys</span></span>
  * <span data-ttu-id="0c531-198">W interfejsie API konfiguracji, separator dwukropek`:`() działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="0c531-198">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="0c531-199">W zmiennych środowiskowych separator dwukropek może nie zadziałał na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="0c531-199">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="0c531-200">Podwójne podkreślenie (`__`) jest obsługiwane przez wszystkie platformy i automatycznie konwertowane na dwukropek.</span><span class="sxs-lookup"><span data-stu-id="0c531-200">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="0c531-201">W Azure Key Vault klucze hierarchiczne używają `--` (dwóch kresek) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="0c531-201">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="0c531-202">Należy podać kod, aby zastąpić łączniki dwukropkiem, gdy wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-202">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="0c531-203"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Obsługuje tablice powiązań z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-203">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="0c531-204">Powiązanie tablicowe zostało opisane w sekcji [Powiązywanie tablicy z klasą](#bind-an-array-to-a-class) .</span><span class="sxs-lookup"><span data-stu-id="0c531-204">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="0c531-205">Wartości</span><span class="sxs-lookup"><span data-stu-id="0c531-205">Values</span></span>

<span data-ttu-id="0c531-206">Wartości konfiguracyjne przyjmują następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="0c531-206">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="0c531-207">Wartości są ciągami.</span><span class="sxs-lookup"><span data-stu-id="0c531-207">Values are strings.</span></span>
* <span data-ttu-id="0c531-208">Wartości null nie można przechowywać w konfiguracji ani powiązana z obiektami.</span><span class="sxs-lookup"><span data-stu-id="0c531-208">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="0c531-209">Udostępnia</span><span class="sxs-lookup"><span data-stu-id="0c531-209">Providers</span></span>

<span data-ttu-id="0c531-210">W poniższej tabeli przedstawiono dostawców konfiguracji dostępnych do ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-210">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="0c531-211">Dostawca</span><span class="sxs-lookup"><span data-stu-id="0c531-211">Provider</span></span> | <span data-ttu-id="0c531-212">Zapewnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="0c531-212">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="0c531-213">[Dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) (Tematy dotyczące*zabezpieczeń* )</span><span class="sxs-lookup"><span data-stu-id="0c531-213">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="0c531-214">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0c531-214">Azure Key Vault</span></span> |
| <span data-ttu-id="0c531-215">[Dostawca konfiguracji aplikacji platformy Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Dokumentacja platformy Azure)</span><span class="sxs-lookup"><span data-stu-id="0c531-215">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="0c531-216">Konfiguracja aplikacji platformy Azure</span><span class="sxs-lookup"><span data-stu-id="0c531-216">Azure App Configuration</span></span> |
| [<span data-ttu-id="0c531-217">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="0c531-217">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="0c531-218">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="0c531-218">Command-line parameters</span></span> |
| [<span data-ttu-id="0c531-219">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="0c531-219">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="0c531-220">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="0c531-220">Custom source</span></span> |
| [<span data-ttu-id="0c531-221">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="0c531-221">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="0c531-222">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="0c531-222">Environment variables</span></span> |
| [<span data-ttu-id="0c531-223">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="0c531-223">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="0c531-224">Pliki (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="0c531-224">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="0c531-225">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="0c531-225">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="0c531-226">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="0c531-226">Directory files</span></span> |
| [<span data-ttu-id="0c531-227">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="0c531-227">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="0c531-228">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="0c531-228">In-memory collections</span></span> |
| <span data-ttu-id="0c531-229">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (Tematy dotyczące*zabezpieczeń* )</span><span class="sxs-lookup"><span data-stu-id="0c531-229">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="0c531-230">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="0c531-230">File in the user profile directory</span></span> |

<span data-ttu-id="0c531-231">Źródła konfiguracji są odczytywane w kolejności, w jakiej dostawcy konfiguracji są określeni podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="0c531-231">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="0c531-232">Dostawcy konfiguracji opisane w tym temacie są opisywane w kolejności alfabetycznej, a nie w kolejności, w jakiej kod może je rozmieścić.</span><span class="sxs-lookup"><span data-stu-id="0c531-232">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="0c531-233">Zamów dostawcy konfiguracji w kodzie, aby odpowiadały Twoim priorytetom dla źródłowych źródeł konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-233">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="0c531-234">Typową sekwencją dostawców konfiguracji jest:</span><span class="sxs-lookup"><span data-stu-id="0c531-234">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="0c531-235">Pliki (*appSettings. JSON*, *appSettings. { Environment}. JSON*, gdzie `{Environment}` to bieżące środowisko hostingu aplikacji)</span><span class="sxs-lookup"><span data-stu-id="0c531-235">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="0c531-236">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0c531-236">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="0c531-237">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (Tylko środowisko programistyczne)</span><span class="sxs-lookup"><span data-stu-id="0c531-237">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="0c531-238">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="0c531-238">Environment variables</span></span>
1. <span data-ttu-id="0c531-239">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="0c531-239">Command-line arguments</span></span>

<span data-ttu-id="0c531-240">Typowym celem jest umieszczenie dostawcy konfiguracji wiersza polecenia jako ostatni w serii dostawców, aby zezwolić na argumenty wiersza polecenia, aby przesłonić konfigurację ustawioną przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="0c531-240">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="0c531-241">Poprzednia sekwencja dostawców jest używana po zainicjowaniu nowego konstruktora hostów za `CreateDefaultBuilder`pomocą programu.</span><span class="sxs-lookup"><span data-stu-id="0c531-241">The preceding sequence of providers is used when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0c531-242">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="0c531-242">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="0c531-243">Konfigurowanie konstruktora hostów za pomocą ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="0c531-243">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="0c531-244">Aby skonfigurować konstruktora hostów, wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> i podaj konfigurację.</span><span class="sxs-lookup"><span data-stu-id="0c531-244">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="0c531-245">`ConfigureHostConfiguration`służy do inicjowania <xref:Microsoft.Extensions.Hosting.IHostEnvironment> usługi do użycia w dalszej części procesu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-245">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="0c531-246">`ConfigureHostConfiguration`może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="0c531-246">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration((hostingContext, config) =>
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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="0c531-247">Konfigurowanie konstruktora hostów za pomocą UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="0c531-247">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="0c531-248">Aby skonfigurować konstruktora hosta, należy wywołać <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> konstruktora hosta z konfiguracją.</span><span class="sxs-lookup"><span data-stu-id="0c531-248">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

::: moniker-end

## <a name="configureappconfiguration"></a><span data-ttu-id="0c531-249">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="0c531-249">ConfigureAppConfiguration</span></span>

<span data-ttu-id="0c531-250">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić dostawców konfiguracji aplikacji oprócz tych dodanych automatycznie przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0c531-250">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="0c531-251">Zastąp poprzednią konfigurację argumentami wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="0c531-251">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="0c531-252">Aby podać konfigurację aplikacji, którą można zastąpić za pomocą argumentów wiersza polecenia, wywołaj `AddCommandLine` ostatni:</span><span class="sxs-lookup"><span data-stu-id="0c531-252">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="0c531-253">Użyj konfiguracji podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="0c531-253">Consume configuration during app startup</span></span>

<span data-ttu-id="0c531-254">Konfiguracja dostarczona do aplikacji w `ConfigureAppConfiguration` programie jest dostępna podczas uruchamiania aplikacji, w tym `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0c531-254">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0c531-255">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja dostępu podczas uruchamiania](#access-configuration-during-startup) .</span><span class="sxs-lookup"><span data-stu-id="0c531-255">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="0c531-256">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="0c531-256">Command-line Configuration Provider</span></span>

<span data-ttu-id="0c531-257"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Ładowanie konfiguracji z par klucz-wartość argumentu wiersza polecenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="0c531-257">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="0c531-258">Aby uaktywnić konfigurację wiersza polecenia, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> Metoda rozszerzenia jest wywoływana w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="0c531-258">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0c531-259">`AddCommandLine`jest wywoływana automatycznie, `CreateDefaultBuilder(string [])` gdy jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="0c531-259">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="0c531-260">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="0c531-260">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="0c531-261">`CreateDefaultBuilder`ładuje również:</span><span class="sxs-lookup"><span data-stu-id="0c531-261">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="0c531-262">Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.</span><span class="sxs-lookup"><span data-stu-id="0c531-262">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="0c531-263">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="0c531-263">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="0c531-264">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="0c531-264">Environment variables.</span></span>

<span data-ttu-id="0c531-265">`CreateDefaultBuilder`dodaje dostawcę konfiguracji wiersza polecenia Last.</span><span class="sxs-lookup"><span data-stu-id="0c531-265">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="0c531-266">Argumenty wiersza polecenia przekazane w czasie wykonywania zastępują konfigurację ustawioną przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="0c531-266">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="0c531-267">`CreateDefaultBuilder`działa, gdy host jest skonstruowany.</span><span class="sxs-lookup"><span data-stu-id="0c531-267">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="0c531-268">W związku z tym konfiguracja wiersza polecenia aktywowana przez `CreateDefaultBuilder` program może mieć wpływ na sposób konfigurowania hosta.</span><span class="sxs-lookup"><span data-stu-id="0c531-268">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="0c531-269">W przypadku aplikacji opartych na ASP.NET Core szablonach program `AddCommandLine` został już wywołany przez. `CreateDefaultBuilder`</span><span class="sxs-lookup"><span data-stu-id="0c531-269">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0c531-270">Aby dodać kolejnych dostawców konfiguracji i zachować możliwość przesłonięcia konfiguracji od tych dostawców za pomocą argumentów wiersza polecenia, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddCommandLine` jako ostatni.</span><span class="sxs-lookup"><span data-stu-id="0c531-270">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="0c531-271">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="0c531-271">**Example**</span></span>

<span data-ttu-id="0c531-272">Przykładowa aplikacja korzysta z statycznej wygodnej metody `CreateDefaultBuilder` tworzenia hosta, który obejmuje <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>wywołanie.</span><span class="sxs-lookup"><span data-stu-id="0c531-272">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="0c531-273">Otwórz wiersz polecenia w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="0c531-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="0c531-274">Podaj do `dotnet run` polecenia argument wiersza polecenia, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="0c531-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="0c531-275">Po uruchomieniu aplikacji otwórz w przeglądarce aplikację w lokalizacji `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0c531-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="0c531-276">Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla argumentu wiersza polecenia konfiguracji dostarczonego do `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="0c531-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="0c531-277">Argumenty</span><span class="sxs-lookup"><span data-stu-id="0c531-277">Arguments</span></span>

<span data-ttu-id="0c531-278">Wartość musi następować po znaku równości (`=`) lub klucz musi mieć prefiks (`--` lub `/`), gdy wartość znajduje się w miejscu.</span><span class="sxs-lookup"><span data-stu-id="0c531-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="0c531-279">Wartość nie jest wymagana, jeśli jest używany znak równości (na przykład `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="0c531-279">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="0c531-280">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="0c531-280">Key prefix</span></span>               | <span data-ttu-id="0c531-281">Przykład</span><span class="sxs-lookup"><span data-stu-id="0c531-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="0c531-282">Brak prefiksu</span><span class="sxs-lookup"><span data-stu-id="0c531-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="0c531-283">Dwie kreski (`--`)</span><span class="sxs-lookup"><span data-stu-id="0c531-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="0c531-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="0c531-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="0c531-285">Ukośnik (`/`)</span><span class="sxs-lookup"><span data-stu-id="0c531-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="0c531-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="0c531-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="0c531-287">W tym samym poleceniu nie należy mieszać par klucz-wartość argumentu wiersza polecenia, które używają znaku równości z parami klucz-wartość, które używają spacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="0c531-288">Przykładowe polecenia:</span><span class="sxs-lookup"><span data-stu-id="0c531-288">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="0c531-289">Mapowanie przełączników</span><span class="sxs-lookup"><span data-stu-id="0c531-289">Switch mappings</span></span>

<span data-ttu-id="0c531-290">Mapowania przełączników Zezwalaj na logikę zamiany nazwy klucza.</span><span class="sxs-lookup"><span data-stu-id="0c531-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="0c531-291">W przypadku ręcznego kompilowania konfiguracji za <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>pomocą programu można udostępnić <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> do metody słownik przemieszczeń przełączeń.</span><span class="sxs-lookup"><span data-stu-id="0c531-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="0c531-292">Gdy jest używany słownik mapowania przełączników, słownik jest sprawdzany dla klucza, który pasuje do klucza dostarczonego przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="0c531-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="0c531-293">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) zostanie przeniesiona z powrotem, aby ustawić parę klucz-wartość w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="0c531-294">Mapowanie przełącznika jest wymagane dla każdego klucza wiersza polecenia poprzedzonego pojedynczą kreską (`-`).</span><span class="sxs-lookup"><span data-stu-id="0c531-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="0c531-295">Przełącz reguły klucza słownika mapowania:</span><span class="sxs-lookup"><span data-stu-id="0c531-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="0c531-296">Przełączniki muszą zaczynać się kreską`-`() lub podwójną kreską (`--`).</span><span class="sxs-lookup"><span data-stu-id="0c531-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="0c531-297">Słownik mapowania przełącznika nie może zawierać zduplikowanych kluczy.</span><span class="sxs-lookup"><span data-stu-id="0c531-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="0c531-298">Utwórz słownik mapowań mapowania.</span><span class="sxs-lookup"><span data-stu-id="0c531-298">Create a switch mappings dictionary.</span></span> <span data-ttu-id="0c531-299">W poniższym przykładzie są tworzone dwa mapowania przełączników:</span><span class="sxs-lookup"><span data-stu-id="0c531-299">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="0c531-300">Po skompilowaniu hosta Wywołaj `AddCommandLine` przy użyciu słownika mapowania przełączników:</span><span class="sxs-lookup"><span data-stu-id="0c531-300">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="0c531-301">W przypadku aplikacji korzystających z mapowań przełączników wywołanie `CreateDefaultBuilder` nie powinno przekazywać argumentów.</span><span class="sxs-lookup"><span data-stu-id="0c531-301">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="0c531-302">Wywołanie metody nie obejmuje zamapowanych przełączników i nie ma sposobu przekazywania słownika mapowania przełącznika do `CreateDefaultBuilder`. `CreateDefaultBuilder` `AddCommandLine`</span><span class="sxs-lookup"><span data-stu-id="0c531-302">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0c531-303">Rozwiązanie nie przekazuje argumentów do `CreateDefaultBuilder` , ale zamiast tego `ConfigurationBuilder` zezwala metodzie metody `AddCommandLine` na przetwarzanie zarówno argumentów, jak i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="0c531-303">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="0c531-304">Po utworzeniu słownika mapowań przełączników zawiera dane przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="0c531-304">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="0c531-305">Key</span><span class="sxs-lookup"><span data-stu-id="0c531-305">Key</span></span>       | <span data-ttu-id="0c531-306">Wartość</span><span class="sxs-lookup"><span data-stu-id="0c531-306">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="0c531-307">Jeśli klucze mapowane przez przełącznik są używane podczas uruchamiania aplikacji, konfiguracja otrzymuje wartość konfiguracji klucza dostarczonego przez słownik:</span><span class="sxs-lookup"><span data-stu-id="0c531-307">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="0c531-308">Po uruchomieniu poprzedniego polecenia Konfiguracja zawiera wartości pokazane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="0c531-308">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="0c531-309">Key</span><span class="sxs-lookup"><span data-stu-id="0c531-309">Key</span></span>               | <span data-ttu-id="0c531-310">Wartość</span><span class="sxs-lookup"><span data-stu-id="0c531-310">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="0c531-311">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="0c531-311">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="0c531-312"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Ładowanie konfiguracji ze zmiennej środowiskowej par klucz-wartość w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="0c531-312">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="0c531-313">Aby uaktywnić konfigurację zmiennych środowiskowych, <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> Wywołaj metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="0c531-313">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="0c531-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) pozwala ustawić zmienne środowiskowe w witrynie Azure Portal, które mogą przesłonić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="0c531-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="0c531-315">Aby uzyskać więcej informacji, [Zobacz Azure Apps: Przesłoń konfigurację aplikacji przy użyciu witryny](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="0c531-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0c531-316">`AddEnvironmentVariables`służy do ładowania zmiennych środowiskowych, które są `DOTNET_` poprzedzone [konfiguracją hosta](#host-versus-app-configuration) , gdy nowy Konstruktor hosta zostanie zainicjowany z hostem `CreateDefaultBuilder` [ogólnym](xref:fundamentals/host/generic-host) i jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="0c531-316">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="0c531-317">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="0c531-317">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0c531-318">`AddEnvironmentVariables`służy do ładowania zmiennych środowiskowych, które są `ASPNETCORE_` poprzedzone [konfiguracją hosta](#host-versus-app-configuration) , gdy nowy Konstruktor hosta zostanie zainicjowany przy użyciu [hosta sieci Web](xref:fundamentals/host/web-host) i `CreateDefaultBuilder` jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="0c531-318">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="0c531-319">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="0c531-319">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="0c531-320">`CreateDefaultBuilder`ładuje również:</span><span class="sxs-lookup"><span data-stu-id="0c531-320">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="0c531-321">Konfiguracja aplikacji z nieoznaczonych zmiennych środowiskowych przez `AddEnvironmentVariables` wywołanie bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="0c531-321">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="0c531-322">Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.</span><span class="sxs-lookup"><span data-stu-id="0c531-322">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="0c531-323">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="0c531-323">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="0c531-324">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="0c531-324">Command-line arguments.</span></span>

<span data-ttu-id="0c531-325">Dostawca konfiguracji zmiennych środowiskowych jest wywoływany po ustanowieniu konfiguracji z poziomu kluczy tajnych użytkownika i plików *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="0c531-325">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="0c531-326">Wywołanie dostawcy w tym miejscu pozwala odczytywać zmienne środowiskowe w czasie wykonywania w celu przesłania konfiguracji ustawionych przez klucze tajne użytkownika i pliki *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="0c531-326">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="0c531-327">Jeśli musisz podać konfigurację aplikacji na podstawie dodatkowych zmiennych środowiskowych, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddEnvironmentVariables` z prefiksem.</span><span class="sxs-lookup"><span data-stu-id="0c531-327">If you need to provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call additional providers here as needed.
    // Call AddEnvironmentVariables last if you need to allow
    // environment variables to override values from other 
    // providers.
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
}
```

<span data-ttu-id="0c531-328">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="0c531-328">**Example**</span></span>

<span data-ttu-id="0c531-329">Przykładowa aplikacja korzysta z statycznej wygodnej metody `CreateDefaultBuilder` tworzenia hosta, który obejmuje `AddEnvironmentVariables`wywołanie.</span><span class="sxs-lookup"><span data-stu-id="0c531-329">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="0c531-330">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="0c531-330">Run the sample app.</span></span> <span data-ttu-id="0c531-331">Otwórz w przeglądarce aplikację pod adresem `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0c531-331">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="0c531-332">Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla zmiennej `ENVIRONMENT`środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="0c531-332">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="0c531-333">Wartość odzwierciedla środowisko, w którym jest uruchomiona aplikacja, zazwyczaj `Development` w przypadku uruchamiania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="0c531-333">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="0c531-334">Aby zachować listę zmiennych środowiskowych renderowanych przez aplikację, aplikacja filtruje zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="0c531-334">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="0c531-335">Zapoznaj się z plikiem przykładowej *strony aplikacji/index. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="0c531-335">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="0c531-336">Jeśli chcesz uwidocznić wszystkie zmienne środowiskowe dostępne dla aplikacji, Zmień wartość `FilteredConfiguration` na *stronie/index. cshtml. cs* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0c531-336">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="0c531-337">Prefiksy</span><span class="sxs-lookup"><span data-stu-id="0c531-337">Prefixes</span></span>

<span data-ttu-id="0c531-338">Zmienne środowiskowe ładowane do konfiguracji aplikacji są filtrowane w przypadku podania prefiksu do `AddEnvironmentVariables` metody.</span><span class="sxs-lookup"><span data-stu-id="0c531-338">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="0c531-339">Na przykład aby filtrować zmienne środowiskowe na prefiksie `CUSTOM_`, podaj prefiks dla dostawcy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="0c531-339">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="0c531-340">Prefiks jest usuwany podczas tworzenia par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-340">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="0c531-341">Podczas tworzenia konstruktora hostów Konfiguracja hosta jest zapewniana przez zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="0c531-341">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="0c531-342">Aby uzyskać więcej informacji na temat prefiksu używanego dla tych zmiennych środowiskowych, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="0c531-342">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="0c531-343">**Prefiksy parametrów połączenia**</span><span class="sxs-lookup"><span data-stu-id="0c531-343">**Connection string prefixes**</span></span>

<span data-ttu-id="0c531-344">Interfejs API konfiguracji ma specjalne reguły przetwarzania dla czterech zmiennych środowiskowych parametrów połączenia związanych z konfigurowaniem parametrów połączenia platformy Azure dla środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-344">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="0c531-345">Zmienne środowiskowe z prefiksami podanymi w tabeli są ładowane do aplikacji, jeśli nie podano `AddEnvironmentVariables`prefiksu.</span><span class="sxs-lookup"><span data-stu-id="0c531-345">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="0c531-346">Prefiks parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="0c531-346">Connection string prefix</span></span> | <span data-ttu-id="0c531-347">Dostawca</span><span class="sxs-lookup"><span data-stu-id="0c531-347">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="0c531-348">Dostawca niestandardowy</span><span class="sxs-lookup"><span data-stu-id="0c531-348">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="0c531-349">MySQL</span><span class="sxs-lookup"><span data-stu-id="0c531-349">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="0c531-350">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="0c531-350">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="0c531-351">SQL Server</span><span class="sxs-lookup"><span data-stu-id="0c531-351">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="0c531-352">Gdy zmienna środowiskowa zostanie odnaleziona i załadowana do konfiguracji z dowolnymi z czterech prefiksów pokazanych w tabeli:</span><span class="sxs-lookup"><span data-stu-id="0c531-352">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="0c531-353">Klucz konfiguracji jest tworzony przez usunięcie prefiksu zmiennej środowiskowej i dodanie sekcji klucza konfiguracji (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="0c531-353">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="0c531-354">Zostanie utworzona nowa para klucz-wartość konfiguracji, która reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`tego, który nie ma określonego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="0c531-354">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="0c531-355">Klucz zmiennej środowiskowej</span><span class="sxs-lookup"><span data-stu-id="0c531-355">Environment variable key</span></span> | <span data-ttu-id="0c531-356">Przekonwertowany klucz konfiguracji</span><span class="sxs-lookup"><span data-stu-id="0c531-356">Converted configuration key</span></span> | <span data-ttu-id="0c531-357">Wpis konfiguracji dostawcy</span><span class="sxs-lookup"><span data-stu-id="0c531-357">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="0c531-358">Wpis konfiguracji nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="0c531-358">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="0c531-359">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="0c531-359">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="0c531-360">Wartość:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="0c531-360">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="0c531-361">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="0c531-361">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="0c531-362">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="0c531-362">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="0c531-363">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="0c531-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="0c531-364">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="0c531-364">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="0c531-365">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="0c531-365">File Configuration Provider</span></span>

<span data-ttu-id="0c531-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>jest klasą bazową do ładowania konfiguracji z systemu plików.</span><span class="sxs-lookup"><span data-stu-id="0c531-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="0c531-367">Następujący dostawcy konfiguracji są przydzielone do określonych typów plików:</span><span class="sxs-lookup"><span data-stu-id="0c531-367">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="0c531-368">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="0c531-368">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="0c531-369">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="0c531-369">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="0c531-370">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="0c531-370">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="0c531-371">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="0c531-371">INI Configuration Provider</span></span>

<span data-ttu-id="0c531-372"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Ładowanie konfiguracji z par klucz-wartość pliku ini w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="0c531-372">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="0c531-373">Aby uaktywnić konfigurację pliku ini, wywołaj <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="0c531-373">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0c531-374">Dwukropek może służyć jako ogranicznik sekcji w konfiguracji pliku INI.</span><span class="sxs-lookup"><span data-stu-id="0c531-374">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="0c531-375">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="0c531-375">Overloads permit specifying:</span></span>

* <span data-ttu-id="0c531-376">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="0c531-376">Whether the file is optional.</span></span>
* <span data-ttu-id="0c531-377">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="0c531-377">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="0c531-378"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="0c531-378">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="0c531-379">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0c531-379">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="0c531-380">Ścieżka bazowa jest ustawiana za <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>pomocą.</span><span class="sxs-lookup"><span data-stu-id="0c531-380">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

<span data-ttu-id="0c531-381">Ogólny przykład pliku konfiguracji INI:</span><span class="sxs-lookup"><span data-stu-id="0c531-381">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="0c531-382">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="0c531-382">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0c531-383">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0c531-383">section0:key0</span></span>
* <span data-ttu-id="0c531-384">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="0c531-384">section0:key1</span></span>
* <span data-ttu-id="0c531-385">Section1: podsekcja: Key</span><span class="sxs-lookup"><span data-stu-id="0c531-385">section1:subsection:key</span></span>
* <span data-ttu-id="0c531-386">section2: subsection0: klucz</span><span class="sxs-lookup"><span data-stu-id="0c531-386">section2:subsection0:key</span></span>
* <span data-ttu-id="0c531-387">section2: subsection1: klucz</span><span class="sxs-lookup"><span data-stu-id="0c531-387">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="0c531-388">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="0c531-388">JSON Configuration Provider</span></span>

<span data-ttu-id="0c531-389"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Ładowanie konfiguracji z par klucz-wartość pliku JSON podczas środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="0c531-389">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="0c531-390">Aby uaktywnić konfigurację pliku JSON, wywołaj <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="0c531-390">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0c531-391">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="0c531-391">Overloads permit specifying:</span></span>

* <span data-ttu-id="0c531-392">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="0c531-392">Whether the file is optional.</span></span>
* <span data-ttu-id="0c531-393">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="0c531-393">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="0c531-394"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="0c531-394">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="0c531-395">`AddJsonFile`jest automatycznie wywoływana dwukrotnie po zainicjowaniu nowego konstruktora hostów za `CreateDefaultBuilder`pomocą programu.</span><span class="sxs-lookup"><span data-stu-id="0c531-395">`AddJsonFile` is automatically called twice when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0c531-396">Metoda jest wywoływana w celu załadowania konfiguracji z:</span><span class="sxs-lookup"><span data-stu-id="0c531-396">The method is called to load configuration from:</span></span>

* <span data-ttu-id="0c531-397">*appSettings. JSON* &ndash; ten plik jest odczytywany jako pierwszy.</span><span class="sxs-lookup"><span data-stu-id="0c531-397">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="0c531-398">Wersja środowiska pliku może przesłonić wartości dostarczone przez plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="0c531-398">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="0c531-399">*appSettings. {Environment}. JSON* &ndash; wersja środowiska pliku jest ładowana na podstawie [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="0c531-399">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="0c531-400">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="0c531-400">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="0c531-401">`CreateDefaultBuilder`ładuje również:</span><span class="sxs-lookup"><span data-stu-id="0c531-401">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="0c531-402">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="0c531-402">Environment variables.</span></span>
* <span data-ttu-id="0c531-403">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="0c531-403">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="0c531-404">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="0c531-404">Command-line arguments.</span></span>

<span data-ttu-id="0c531-405">Dostawca konfiguracji JSON został ustanowiony jako pierwszy.</span><span class="sxs-lookup"><span data-stu-id="0c531-405">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="0c531-406">W związku z tym klucze tajne użytkownika, zmienne środowiskowe i argumenty wiersza polecenia przesłaniają konfigurację ustawioną przez pliki *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="0c531-406">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="0c531-407">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji dla plików innych niż *appSettings. JSON* i *appSettings. { Environment}. JSON*:</span><span class="sxs-lookup"><span data-stu-id="0c531-407">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="0c531-408">Ścieżka bazowa jest ustawiana za <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>pomocą.</span><span class="sxs-lookup"><span data-stu-id="0c531-408">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

<span data-ttu-id="0c531-409">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="0c531-409">**Example**</span></span>

<span data-ttu-id="0c531-410">Przykładowa aplikacja korzysta z statycznej wygodnej metody `CreateDefaultBuilder` tworzenia hosta, który obejmuje dwa wywołania programu. `AddJsonFile`</span><span class="sxs-lookup"><span data-stu-id="0c531-410">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="0c531-411">Konfiguracja jest ładowana z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="0c531-411">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="0c531-412">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="0c531-412">Run the sample app.</span></span> <span data-ttu-id="0c531-413">Otwórz w przeglądarce aplikację pod adresem `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0c531-413">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="0c531-414">Zwróć uwagę, że dane wyjściowe zawierają pary klucz-wartość dla konfiguracji pokazanej w tabeli w zależności od środowiska.</span><span class="sxs-lookup"><span data-stu-id="0c531-414">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="0c531-415">Klucze konfiguracji rejestrowania używają dwukropka`:`() jako separatora hierarchicznego.</span><span class="sxs-lookup"><span data-stu-id="0c531-415">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="0c531-416">Key</span><span class="sxs-lookup"><span data-stu-id="0c531-416">Key</span></span>                        | <span data-ttu-id="0c531-417">Wartość programistyczna</span><span class="sxs-lookup"><span data-stu-id="0c531-417">Development Value</span></span> | <span data-ttu-id="0c531-418">Wartość produkcyjna</span><span class="sxs-lookup"><span data-stu-id="0c531-418">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="0c531-419">Rejestrowanie: LogLevel: system</span><span class="sxs-lookup"><span data-stu-id="0c531-419">Logging:LogLevel:System</span></span>    | <span data-ttu-id="0c531-420">Informacje</span><span class="sxs-lookup"><span data-stu-id="0c531-420">Information</span></span>       | <span data-ttu-id="0c531-421">Informacje</span><span class="sxs-lookup"><span data-stu-id="0c531-421">Information</span></span>      |
| <span data-ttu-id="0c531-422">Rejestrowanie: LogLevel: Microsoft</span><span class="sxs-lookup"><span data-stu-id="0c531-422">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="0c531-423">Informacje</span><span class="sxs-lookup"><span data-stu-id="0c531-423">Information</span></span>       | <span data-ttu-id="0c531-424">Informacje</span><span class="sxs-lookup"><span data-stu-id="0c531-424">Information</span></span>      |
| <span data-ttu-id="0c531-425">Rejestrowanie: wartość domyślna: LogLevel</span><span class="sxs-lookup"><span data-stu-id="0c531-425">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="0c531-426">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="0c531-426">Debug</span></span>             | <span data-ttu-id="0c531-427">Błąd</span><span class="sxs-lookup"><span data-stu-id="0c531-427">Error</span></span>            |
| <span data-ttu-id="0c531-428">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="0c531-428">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="0c531-429">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="0c531-429">XML Configuration Provider</span></span>

<span data-ttu-id="0c531-430"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Ładowanie konfiguracji z par klucz-wartość pliku XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="0c531-430">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="0c531-431">Aby uaktywnić konfigurację pliku XML, wywołaj <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="0c531-431">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0c531-432">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="0c531-432">Overloads permit specifying:</span></span>

* <span data-ttu-id="0c531-433">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="0c531-433">Whether the file is optional.</span></span>
* <span data-ttu-id="0c531-434">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="0c531-434">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="0c531-435"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="0c531-435">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="0c531-436">Węzeł główny pliku konfiguracji jest ignorowany, gdy są tworzone pary klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-436">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="0c531-437">Nie określaj definicji typu dokumentu (DTD) ani przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="0c531-437">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="0c531-438">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0c531-438">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="0c531-439">Ścieżka bazowa jest ustawiana za <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>pomocą.</span><span class="sxs-lookup"><span data-stu-id="0c531-439">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

<span data-ttu-id="0c531-440">Pliki konfiguracji XML mogą używać odrębnych nazw elementów dla powtarzających się sekcji:</span><span class="sxs-lookup"><span data-stu-id="0c531-440">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="0c531-441">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="0c531-441">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0c531-442">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0c531-442">section0:key0</span></span>
* <span data-ttu-id="0c531-443">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="0c531-443">section0:key1</span></span>
* <span data-ttu-id="0c531-444">section1:key0</span><span class="sxs-lookup"><span data-stu-id="0c531-444">section1:key0</span></span>
* <span data-ttu-id="0c531-445">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="0c531-445">section1:key1</span></span>

<span data-ttu-id="0c531-446">Powtarzające się elementy, które używają tej samej nazwy elementu `name` , działają, jeśli atrybut jest używany do odróżnienia elementów:</span><span class="sxs-lookup"><span data-stu-id="0c531-446">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="0c531-447">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="0c531-447">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0c531-448">sekcja: section0: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="0c531-448">section:section0:key:key0</span></span>
* <span data-ttu-id="0c531-449">sekcja: section0: Key: Klucz1</span><span class="sxs-lookup"><span data-stu-id="0c531-449">section:section0:key:key1</span></span>
* <span data-ttu-id="0c531-450">sekcja: Section1: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="0c531-450">section:section1:key:key0</span></span>
* <span data-ttu-id="0c531-451">sekcja: Section1: Key: Klucz1</span><span class="sxs-lookup"><span data-stu-id="0c531-451">section:section1:key:key1</span></span>

<span data-ttu-id="0c531-452">Atrybuty mogą służyć do dostarczania wartości:</span><span class="sxs-lookup"><span data-stu-id="0c531-452">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="0c531-453">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="0c531-453">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0c531-454">Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="0c531-454">key:attribute</span></span>
* <span data-ttu-id="0c531-455">sekcja: Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="0c531-455">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="0c531-456">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="0c531-456">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="0c531-457"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Używa plików katalogu jako par klucz konfiguracji i wartość.</span><span class="sxs-lookup"><span data-stu-id="0c531-457">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="0c531-458">Kluczem jest nazwa pliku.</span><span class="sxs-lookup"><span data-stu-id="0c531-458">The key is the file name.</span></span> <span data-ttu-id="0c531-459">Wartość zawiera zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="0c531-459">The value contains the file's contents.</span></span> <span data-ttu-id="0c531-460">Dostawca konfiguracji klucza dla plików jest używany w scenariuszach hostingu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="0c531-460">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="0c531-461">Aby uaktywnić konfigurację klucza dla plików, wywołaj <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="0c531-461">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="0c531-462">`directoryPath` Do plików musi być ścieżką bezwzględną.</span><span class="sxs-lookup"><span data-stu-id="0c531-462">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="0c531-463">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="0c531-463">Overloads permit specifying:</span></span>

* <span data-ttu-id="0c531-464">`Action<KeyPerFileConfigurationSource>` Delegat, który konfiguruje źródło.</span><span class="sxs-lookup"><span data-stu-id="0c531-464">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="0c531-465">Określa, czy katalog jest opcjonalny, i ścieżkę do katalogu.</span><span class="sxs-lookup"><span data-stu-id="0c531-465">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="0c531-466">Podwójny znak podkreślenia (`__`) jest używany jako ogranicznik klucza konfiguracji w nazwach plików.</span><span class="sxs-lookup"><span data-stu-id="0c531-466">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="0c531-467">Na przykład nazwa `Logging__LogLevel__System` pliku generuje klucz `Logging:LogLevel:System`konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-467">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="0c531-468">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0c531-468">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

<span data-ttu-id="0c531-469">Ścieżka bazowa jest ustawiana za <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>pomocą.</span><span class="sxs-lookup"><span data-stu-id="0c531-469">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

## <a name="memory-configuration-provider"></a><span data-ttu-id="0c531-470">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="0c531-470">Memory Configuration Provider</span></span>

<span data-ttu-id="0c531-471"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Używa kolekcji w pamięci jako par klucz konfiguracji-wartość.</span><span class="sxs-lookup"><span data-stu-id="0c531-471">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="0c531-472">Aby uaktywnić konfigurację kolekcji w pamięci, wywołaj <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="0c531-472">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0c531-473">Dostawcę konfiguracji można zainicjować przy użyciu `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="0c531-473">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="0c531-474">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-474">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="0c531-475">W poniższym przykładzie tworzony jest słownik konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="0c531-475">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="0c531-476">Słownik jest używany z wywołaniem `AddInMemoryCollection` do zapewnienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="0c531-476">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="0c531-477">GetValue</span><span class="sxs-lookup"><span data-stu-id="0c531-477">GetValue</span></span>

<span data-ttu-id="0c531-478">[ConfigurationBinder. GetValue\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnia wartość z konfiguracji z określonym kluczem i konwertuje ją na określony typ.</span><span class="sxs-lookup"><span data-stu-id="0c531-478">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="0c531-479">Przeciążenie zezwala na podanie wartości domyślnej, jeśli nie znaleziono klucza.</span><span class="sxs-lookup"><span data-stu-id="0c531-479">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="0c531-480">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="0c531-480">The following example:</span></span>

* <span data-ttu-id="0c531-481">Wyodrębnia wartość ciągu z konfiguracji przy użyciu klucza `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="0c531-481">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="0c531-482">Jeśli `NumberKey` nie znaleziono w kluczach konfiguracji, `99` zostanie użyta wartość domyślna.</span><span class="sxs-lookup"><span data-stu-id="0c531-482">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="0c531-483">Typ wartości `int`.</span><span class="sxs-lookup"><span data-stu-id="0c531-483">Types the value as an `int`.</span></span>
* <span data-ttu-id="0c531-484">Przechowuje wartość we `NumberConfig` właściwości do użycia na stronie.</span><span class="sxs-lookup"><span data-stu-id="0c531-484">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="0c531-485">GetSection, GetChildren i EXISTS</span><span class="sxs-lookup"><span data-stu-id="0c531-485">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="0c531-486">W poniższych przykładach należy wziąć pod uwagę następujący plik JSON.</span><span class="sxs-lookup"><span data-stu-id="0c531-486">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="0c531-487">Cztery klucze są dostępne w dwóch sekcjach, z których jedna zawiera parę podsekcji:</span><span class="sxs-lookup"><span data-stu-id="0c531-487">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="0c531-488">Gdy plik jest odczytywany do konfiguracji, następujące unikatowe klucze hierarchiczne są tworzone w celu przechowywania wartości konfiguracyjnych:</span><span class="sxs-lookup"><span data-stu-id="0c531-488">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="0c531-489">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0c531-489">section0:key0</span></span>
* <span data-ttu-id="0c531-490">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="0c531-490">section0:key1</span></span>
* <span data-ttu-id="0c531-491">section1:key0</span><span class="sxs-lookup"><span data-stu-id="0c531-491">section1:key0</span></span>
* <span data-ttu-id="0c531-492">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="0c531-492">section1:key1</span></span>
* <span data-ttu-id="0c531-493">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="0c531-493">section2:subsection0:key0</span></span>
* <span data-ttu-id="0c531-494">section2: subsection0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="0c531-494">section2:subsection0:key1</span></span>
* <span data-ttu-id="0c531-495">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="0c531-495">section2:subsection1:key0</span></span>
* <span data-ttu-id="0c531-496">section2: subsection1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="0c531-496">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="0c531-497">GetSection</span><span class="sxs-lookup"><span data-stu-id="0c531-497">GetSection</span></span>

<span data-ttu-id="0c531-498">[IConfiguration. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcję konfiguracji z określonym kluczem podsekcji.</span><span class="sxs-lookup"><span data-stu-id="0c531-498">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="0c531-499">Aby zwrócić element <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz-wartość w `section1`, wywołaniu `GetSection` i podać nazwę sekcji:</span><span class="sxs-lookup"><span data-stu-id="0c531-499">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="0c531-500">`configSection` Nie ma wartości, tylko klucza i ścieżki.</span><span class="sxs-lookup"><span data-stu-id="0c531-500">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="0c531-501">Podobnie Aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę do sekcji:</span><span class="sxs-lookup"><span data-stu-id="0c531-501">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="0c531-502">`GetSection`nigdy nie `null`zwraca.</span><span class="sxs-lookup"><span data-stu-id="0c531-502">`GetSection` never returns `null`.</span></span> <span data-ttu-id="0c531-503">Jeśli nie znaleziono pasującej sekcji, zwracany jest `IConfigurationSection` pusty.</span><span class="sxs-lookup"><span data-stu-id="0c531-503">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="0c531-504">Gdy `GetSection` zwraca pasującą sekcję, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione.</span><span class="sxs-lookup"><span data-stu-id="0c531-504">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="0c531-505">A i są zwracane, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> gdy istnieje sekcja. <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key></span><span class="sxs-lookup"><span data-stu-id="0c531-505">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="0c531-506">GetChildren</span><span class="sxs-lookup"><span data-stu-id="0c531-506">GetChildren</span></span>

<span data-ttu-id="0c531-507">Wywołanie [iConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) w `section2` programie uzyskuje element `IEnumerable<IConfigurationSection>` obejmujący:</span><span class="sxs-lookup"><span data-stu-id="0c531-507">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="0c531-508">Exists</span><span class="sxs-lookup"><span data-stu-id="0c531-508">Exists</span></span>

<span data-ttu-id="0c531-509">Użyj [ConfigurationExtensions. istnieje](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) , aby określić, czy istnieje sekcja konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="0c531-509">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="0c531-510">Dane przykładowe są `false` spowodowane tym `sectionExists` , że w danych konfiguracji `section2:subsection2` nie ma sekcji.</span><span class="sxs-lookup"><span data-stu-id="0c531-510">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="0c531-511">Powiąż z klasą</span><span class="sxs-lookup"><span data-stu-id="0c531-511">Bind to a class</span></span>

<span data-ttu-id="0c531-512">Konfigurację można powiązać z klasami, które reprezentują grupy powiązanych ustawień przy użyciu *wzorca opcji*.</span><span class="sxs-lookup"><span data-stu-id="0c531-512">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="0c531-513">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="0c531-513">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="0c531-514">Wartości konfiguracji są zwracane jako ciągi, ale wywołanie <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie obiektów [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) .</span><span class="sxs-lookup"><span data-stu-id="0c531-514">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="0c531-515">Przykładowa aplikacja zawiera `Starship` model (*modele/Starship. cs*):</span><span class="sxs-lookup"><span data-stu-id="0c531-515">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0c531-516">Sekcja pliku *Starship. JSON* tworzy konfigurację, gdy aplikacja Przykładowa używa dostawcy konfiguracji JSON do załadowania konfiguracji: `starship`</span><span class="sxs-lookup"><span data-stu-id="0c531-516">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="0c531-517">Tworzone są następujące pary klucz-wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="0c531-517">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="0c531-518">Key</span><span class="sxs-lookup"><span data-stu-id="0c531-518">Key</span></span>                   | <span data-ttu-id="0c531-519">Wartość</span><span class="sxs-lookup"><span data-stu-id="0c531-519">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="0c531-520">Starship: Nazwa</span><span class="sxs-lookup"><span data-stu-id="0c531-520">starship:name</span></span>         | <span data-ttu-id="0c531-521">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="0c531-521">USS Enterprise</span></span>                                    |
| <span data-ttu-id="0c531-522">Starship: Rejestr</span><span class="sxs-lookup"><span data-stu-id="0c531-522">starship:registry</span></span>     | <span data-ttu-id="0c531-523">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="0c531-523">NCC-1701</span></span>                                          |
| <span data-ttu-id="0c531-524">Starship: Klasa</span><span class="sxs-lookup"><span data-stu-id="0c531-524">starship:class</span></span>        | <span data-ttu-id="0c531-525">Skład</span><span class="sxs-lookup"><span data-stu-id="0c531-525">Constitution</span></span>                                      |
| <span data-ttu-id="0c531-526">Starship: Długość</span><span class="sxs-lookup"><span data-stu-id="0c531-526">starship:length</span></span>       | <span data-ttu-id="0c531-527">304,8</span><span class="sxs-lookup"><span data-stu-id="0c531-527">304.8</span></span>                                             |
| <span data-ttu-id="0c531-528">Starship: prowizja</span><span class="sxs-lookup"><span data-stu-id="0c531-528">starship:commissioned</span></span> | <span data-ttu-id="0c531-529">False</span><span class="sxs-lookup"><span data-stu-id="0c531-529">False</span></span>                                             |
| <span data-ttu-id="0c531-530">handlowych</span><span class="sxs-lookup"><span data-stu-id="0c531-530">trademark</span></span>             | <span data-ttu-id="0c531-531">Najważniejsze obrazy Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="0c531-531">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="0c531-532">Przykładowa aplikacja wywołuje `GetSection` `starship` klucz.</span><span class="sxs-lookup"><span data-stu-id="0c531-532">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="0c531-533">Pary `starship` klucz-wartość są odizolowane.</span><span class="sxs-lookup"><span data-stu-id="0c531-533">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="0c531-534">Metoda jest wywoływana w podsekcji przekazującej w wystąpieniu `Starship` klasy. `Bind`</span><span class="sxs-lookup"><span data-stu-id="0c531-534">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="0c531-535">Po powiązaniu wartości wystąpień wystąpienie jest przypisywane do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="0c531-535">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="0c531-536">Powiąż z grafem obiektów</span><span class="sxs-lookup"><span data-stu-id="0c531-536">Bind to an object graph</span></span>

<span data-ttu-id="0c531-537"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>jest w stanie powiązać cały Graf obiektów POCO.</span><span class="sxs-lookup"><span data-stu-id="0c531-537"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="0c531-538">Przykład zawiera `TvShow` model, którego obiekt zawiera `Metadata` obiekty i `Actors` klasy (*modele/TvShow. cs*):</span><span class="sxs-lookup"><span data-stu-id="0c531-538">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0c531-539">Przykładowa aplikacja zawiera plik *tvshow. XML* zawierający dane konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="0c531-539">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="0c531-540">Konfiguracja jest powiązana z wykresem całego `TvShow` obiektu `Bind` za pomocą metody.</span><span class="sxs-lookup"><span data-stu-id="0c531-540">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="0c531-541">Powiązane wystąpienie jest przypisane do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="0c531-541">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="0c531-542">[ConfigurationBinder. get\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) tworzy powiązania i zwraca określony typ.</span><span class="sxs-lookup"><span data-stu-id="0c531-542">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="0c531-543">`Get<T>`jest wygodniejszy niż używanie `Bind`.</span><span class="sxs-lookup"><span data-stu-id="0c531-543">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="0c531-544">Poniższy kod pokazuje, jak używać `Get<T>` w poprzednim przykładzie, co umożliwia bezpośrednie przypisanie wystąpienia powiązanego do właściwości używanej do renderowania:</span><span class="sxs-lookup"><span data-stu-id="0c531-544">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="0c531-545">Powiąż tablicę z klasą</span><span class="sxs-lookup"><span data-stu-id="0c531-545">Bind an array to a class</span></span>

<span data-ttu-id="0c531-546">*Przykładowa aplikacja pokazuje Koncepcje opisane w tej sekcji.*</span><span class="sxs-lookup"><span data-stu-id="0c531-546">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="0c531-547"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Obsługuje tablice powiązań z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-547">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="0c531-548">Każdy format tablicy, który ujawnia segment klucza numerycznego`:0:`( `:1:`, &hellip; , `:{n}:`) jest zdolny do powiązania tablicy z tablicą klas poco.</span><span class="sxs-lookup"><span data-stu-id="0c531-548">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="0c531-549">Powiązanie jest dostarczane według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="0c531-549">Binding is provided by convention.</span></span> <span data-ttu-id="0c531-550">Niestandardowi dostawcy konfiguracji nie muszą implementować powiązania tablicy.</span><span class="sxs-lookup"><span data-stu-id="0c531-550">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="0c531-551">**Przetwarzanie tablicy w pamięci**</span><span class="sxs-lookup"><span data-stu-id="0c531-551">**In-memory array processing**</span></span>

<span data-ttu-id="0c531-552">Należy wziąć pod uwagę klucze konfiguracji i wartości podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="0c531-552">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="0c531-553">Key</span><span class="sxs-lookup"><span data-stu-id="0c531-553">Key</span></span>             | <span data-ttu-id="0c531-554">Wartość</span><span class="sxs-lookup"><span data-stu-id="0c531-554">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="0c531-555">Tablica: wpisy: 0</span><span class="sxs-lookup"><span data-stu-id="0c531-555">array:entries:0</span></span> | <span data-ttu-id="0c531-556">value0</span><span class="sxs-lookup"><span data-stu-id="0c531-556">value0</span></span> |
| <span data-ttu-id="0c531-557">Tablica: wpisy: 1</span><span class="sxs-lookup"><span data-stu-id="0c531-557">array:entries:1</span></span> | <span data-ttu-id="0c531-558">sekwencj</span><span class="sxs-lookup"><span data-stu-id="0c531-558">value1</span></span> |
| <span data-ttu-id="0c531-559">Tablica: wpisy: 2</span><span class="sxs-lookup"><span data-stu-id="0c531-559">array:entries:2</span></span> | <span data-ttu-id="0c531-560">wartość2</span><span class="sxs-lookup"><span data-stu-id="0c531-560">value2</span></span> |
| <span data-ttu-id="0c531-561">Tablica: wpisy: 4</span><span class="sxs-lookup"><span data-stu-id="0c531-561">array:entries:4</span></span> | <span data-ttu-id="0c531-562">value4</span><span class="sxs-lookup"><span data-stu-id="0c531-562">value4</span></span> |
| <span data-ttu-id="0c531-563">Tablica: wpisy: 5</span><span class="sxs-lookup"><span data-stu-id="0c531-563">array:entries:5</span></span> | <span data-ttu-id="0c531-564">value5</span><span class="sxs-lookup"><span data-stu-id="0c531-564">value5</span></span> |

<span data-ttu-id="0c531-565">Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:</span><span class="sxs-lookup"><span data-stu-id="0c531-565">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

::: moniker-end

<span data-ttu-id="0c531-566">Tablica pomija wartość dla indeksu &num;3.</span><span class="sxs-lookup"><span data-stu-id="0c531-566">The array skips a value for index &num;3.</span></span> <span data-ttu-id="0c531-567">Segregator konfiguracji nie może powiązać wartości null ani tworzyć wpisów o wartości null w obiektach powiązanych, co oznacza, że w chwili pojawi się wynik powiązania tej tablicy z obiektem.</span><span class="sxs-lookup"><span data-stu-id="0c531-567">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="0c531-568">W przykładowej aplikacji jest dostępna Klasa POCO, która przechowuje powiązane dane konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="0c531-568">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0c531-569">Dane konfiguracji są powiązane z obiektem:</span><span class="sxs-lookup"><span data-stu-id="0c531-569">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="0c531-570">[ConfigurationBinder. get\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) można również użyć składni, która daje w wyniku bardziej zwarty kod:</span><span class="sxs-lookup"><span data-stu-id="0c531-570">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="0c531-571">Obiekt powiązany, wystąpienie elementu `ArrayExample`, otrzymuje dane tablicy z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-571">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="0c531-572">`ArrayExample.Entries`Indeks</span><span class="sxs-lookup"><span data-stu-id="0c531-572">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="0c531-573">`ArrayExample.Entries`Wartościami</span><span class="sxs-lookup"><span data-stu-id="0c531-573">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="0c531-574">0</span><span class="sxs-lookup"><span data-stu-id="0c531-574">0</span></span>                            | <span data-ttu-id="0c531-575">value0</span><span class="sxs-lookup"><span data-stu-id="0c531-575">value0</span></span>                       |
| <span data-ttu-id="0c531-576">1</span><span class="sxs-lookup"><span data-stu-id="0c531-576">1</span></span>                            | <span data-ttu-id="0c531-577">sekwencj</span><span class="sxs-lookup"><span data-stu-id="0c531-577">value1</span></span>                       |
| <span data-ttu-id="0c531-578">2</span><span class="sxs-lookup"><span data-stu-id="0c531-578">2</span></span>                            | <span data-ttu-id="0c531-579">wartość2</span><span class="sxs-lookup"><span data-stu-id="0c531-579">value2</span></span>                       |
| <span data-ttu-id="0c531-580">3</span><span class="sxs-lookup"><span data-stu-id="0c531-580">3</span></span>                            | <span data-ttu-id="0c531-581">value4</span><span class="sxs-lookup"><span data-stu-id="0c531-581">value4</span></span>                       |
| <span data-ttu-id="0c531-582">4</span><span class="sxs-lookup"><span data-stu-id="0c531-582">4</span></span>                            | <span data-ttu-id="0c531-583">value5</span><span class="sxs-lookup"><span data-stu-id="0c531-583">value5</span></span>                       |

<span data-ttu-id="0c531-584">Indeks &num;3 w obiekcie powiązanym przechowuje dane konfiguracji `array:4` dla `value4`klucza konfiguracji i jego wartość.</span><span class="sxs-lookup"><span data-stu-id="0c531-584">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="0c531-585">Gdy dane konfiguracji zawierające tablicę są powiązane, indeksy tablic w kluczach konfiguracji są używane tylko do iteracji danych konfiguracji podczas tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="0c531-585">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="0c531-586">Wartości null nie można zachować w danych konfiguracyjnych, a wpis o wartości null nie jest tworzony w obiekcie powiązanym, gdy tablica w kluczach konfiguracji pomija jeden lub więcej indeksów.</span><span class="sxs-lookup"><span data-stu-id="0c531-586">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="0c531-587">Brakujący element konfiguracji dla indeksu &num;3 można podać przed powiązaniem `ArrayExample` z wystąpieniem przez dowolnego dostawcę konfiguracji, który generuje poprawną parę klucz-wartość w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-587">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="0c531-588">Jeśli przykład zawiera dodatkowego dostawcę konfiguracji JSON z brakującą parą klucz-wartość, `ArrayExample.Entries` dopasowuje pełną tablicę konfiguracyjną:</span><span class="sxs-lookup"><span data-stu-id="0c531-588">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="0c531-589">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="0c531-589">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="0c531-590">W `ConfigureAppConfiguration`programie:</span><span class="sxs-lookup"><span data-stu-id="0c531-590">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="0c531-591">Para klucz-wartość pokazana w tabeli jest ładowana do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-591">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="0c531-592">Key</span><span class="sxs-lookup"><span data-stu-id="0c531-592">Key</span></span>             | <span data-ttu-id="0c531-593">Wartość</span><span class="sxs-lookup"><span data-stu-id="0c531-593">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="0c531-594">Tablica: wpisy: 3</span><span class="sxs-lookup"><span data-stu-id="0c531-594">array:entries:3</span></span> | <span data-ttu-id="0c531-595">Wartość3</span><span class="sxs-lookup"><span data-stu-id="0c531-595">value3</span></span> |

<span data-ttu-id="0c531-596">Jeśli wystąpienie &num; `ArrayExample.Entries` klasy jest powiązane, gdy dostawca konfiguracji JSON zawiera wpis dla indeksu 3, tablica zawiera wartość. `ArrayExample`</span><span class="sxs-lookup"><span data-stu-id="0c531-596">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="0c531-597">`ArrayExample.Entries`Indeks</span><span class="sxs-lookup"><span data-stu-id="0c531-597">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="0c531-598">`ArrayExample.Entries`Wartościami</span><span class="sxs-lookup"><span data-stu-id="0c531-598">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="0c531-599">0</span><span class="sxs-lookup"><span data-stu-id="0c531-599">0</span></span>                            | <span data-ttu-id="0c531-600">value0</span><span class="sxs-lookup"><span data-stu-id="0c531-600">value0</span></span>                       |
| <span data-ttu-id="0c531-601">1</span><span class="sxs-lookup"><span data-stu-id="0c531-601">1</span></span>                            | <span data-ttu-id="0c531-602">sekwencj</span><span class="sxs-lookup"><span data-stu-id="0c531-602">value1</span></span>                       |
| <span data-ttu-id="0c531-603">2</span><span class="sxs-lookup"><span data-stu-id="0c531-603">2</span></span>                            | <span data-ttu-id="0c531-604">wartość2</span><span class="sxs-lookup"><span data-stu-id="0c531-604">value2</span></span>                       |
| <span data-ttu-id="0c531-605">3</span><span class="sxs-lookup"><span data-stu-id="0c531-605">3</span></span>                            | <span data-ttu-id="0c531-606">Wartość3</span><span class="sxs-lookup"><span data-stu-id="0c531-606">value3</span></span>                       |
| <span data-ttu-id="0c531-607">4</span><span class="sxs-lookup"><span data-stu-id="0c531-607">4</span></span>                            | <span data-ttu-id="0c531-608">value4</span><span class="sxs-lookup"><span data-stu-id="0c531-608">value4</span></span>                       |
| <span data-ttu-id="0c531-609">5</span><span class="sxs-lookup"><span data-stu-id="0c531-609">5</span></span>                            | <span data-ttu-id="0c531-610">value5</span><span class="sxs-lookup"><span data-stu-id="0c531-610">value5</span></span>                       |

<span data-ttu-id="0c531-611">**Przetwarzanie tablicy JSON**</span><span class="sxs-lookup"><span data-stu-id="0c531-611">**JSON array processing**</span></span>

<span data-ttu-id="0c531-612">Jeśli plik JSON zawiera tablicę, klucze konfiguracji są tworzone dla elementów tablicy z indeksem sekcji o wartości zero.</span><span class="sxs-lookup"><span data-stu-id="0c531-612">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="0c531-613">W poniższym pliku `subsection` konfiguracji jest tablicą:</span><span class="sxs-lookup"><span data-stu-id="0c531-613">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="0c531-614">Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz-wartość:</span><span class="sxs-lookup"><span data-stu-id="0c531-614">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="0c531-615">Key</span><span class="sxs-lookup"><span data-stu-id="0c531-615">Key</span></span>                     | <span data-ttu-id="0c531-616">Wartość</span><span class="sxs-lookup"><span data-stu-id="0c531-616">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="0c531-617">json_array: klucz</span><span class="sxs-lookup"><span data-stu-id="0c531-617">json_array:key</span></span>          | <span data-ttu-id="0c531-618">wartośća</span><span class="sxs-lookup"><span data-stu-id="0c531-618">valueA</span></span> |
| <span data-ttu-id="0c531-619">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="0c531-619">json_array:subsection:0</span></span> | <span data-ttu-id="0c531-620">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="0c531-620">valueB</span></span> |
| <span data-ttu-id="0c531-621">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="0c531-621">json_array:subsection:1</span></span> | <span data-ttu-id="0c531-622">valueC</span><span class="sxs-lookup"><span data-stu-id="0c531-622">valueC</span></span> |
| <span data-ttu-id="0c531-623">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="0c531-623">json_array:subsection:2</span></span> | <span data-ttu-id="0c531-624">Znajdując</span><span class="sxs-lookup"><span data-stu-id="0c531-624">valueD</span></span> |

<span data-ttu-id="0c531-625">W przykładowej aplikacji jest dostępna następująca Klasa POCO z powiązaniem par klucz-wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="0c531-625">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0c531-626">Po powiązaniu `JsonArrayExample.Key` utrzymuje wartość `valueA`.</span><span class="sxs-lookup"><span data-stu-id="0c531-626">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="0c531-627">Wartości podsekcji są przechowywane we właściwości `Subsection`tablicy poco.</span><span class="sxs-lookup"><span data-stu-id="0c531-627">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="0c531-628">`JsonArrayExample.Subsection`Indeks</span><span class="sxs-lookup"><span data-stu-id="0c531-628">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="0c531-629">`JsonArrayExample.Subsection`Wartościami</span><span class="sxs-lookup"><span data-stu-id="0c531-629">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="0c531-630">0</span><span class="sxs-lookup"><span data-stu-id="0c531-630">0</span></span>                                   | <span data-ttu-id="0c531-631">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="0c531-631">valueB</span></span>                              |
| <span data-ttu-id="0c531-632">1</span><span class="sxs-lookup"><span data-stu-id="0c531-632">1</span></span>                                   | <span data-ttu-id="0c531-633">valueC</span><span class="sxs-lookup"><span data-stu-id="0c531-633">valueC</span></span>                              |
| <span data-ttu-id="0c531-634">2</span><span class="sxs-lookup"><span data-stu-id="0c531-634">2</span></span>                                   | <span data-ttu-id="0c531-635">Znajdując</span><span class="sxs-lookup"><span data-stu-id="0c531-635">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="0c531-636">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="0c531-636">Custom configuration provider</span></span>

<span data-ttu-id="0c531-637">Przykładowa aplikacja pokazuje, jak utworzyć podstawowego dostawcę konfiguracji, który odczytuje pary klucz-wartość konfiguracji z bazy danych przy użyciu [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="0c531-637">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="0c531-638">Dostawca ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="0c531-638">The provider has the following characteristics:</span></span>

* <span data-ttu-id="0c531-639">Baza danych EF w pamięci jest używana w celach demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="0c531-639">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="0c531-640">Aby użyć bazy danych, która wymaga parametrów połączenia, zaimplementuj dodatkową `ConfigurationBuilder` wartość w celu dostarczenia parametrów połączenia od innego dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0c531-640">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="0c531-641">Dostawca odczytuje tabelę bazy danych w konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="0c531-641">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="0c531-642">Dostawca nie wykonuje zapytania do bazy danych w oparciu o klucz.</span><span class="sxs-lookup"><span data-stu-id="0c531-642">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="0c531-643">Ponowne załadowanie nie zostało zaimplementowane, więc aktualizacja bazy danych po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c531-643">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="0c531-644">`EFConfigurationValue` Zdefiniuj jednostkę do przechowywania wartości konfiguracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0c531-644">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0c531-645">*Modele/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-645">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="0c531-646">`EFConfigurationContext` Dodaj do magazynu i uzyskaj dostęp do skonfigurowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="0c531-646">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="0c531-647">*EFConfigurationProvider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-647">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="0c531-648">Utwórz klasę implementującą <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="0c531-648">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="0c531-649">*EFConfigurationProvider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-649">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="0c531-650">Utwórz niestandardowego dostawcę konfiguracji, dziedziczących od <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="0c531-650">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="0c531-651">Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta.</span><span class="sxs-lookup"><span data-stu-id="0c531-651">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="0c531-652">*EFConfigurationProvider/EFConfigurationProvider. cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-652">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="0c531-653">Metoda rozszerzająca zezwala na Dodawanie źródła konfiguracji `ConfigurationBuilder`do. `AddEFConfiguration`</span><span class="sxs-lookup"><span data-stu-id="0c531-653">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="0c531-654">*Rozszerzenia/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-654">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="0c531-655">Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-655">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0c531-656">*Modele/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-656">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="0c531-657">`EFConfigurationContext` Dodaj do magazynu i uzyskaj dostęp do skonfigurowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="0c531-657">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="0c531-658">*EFConfigurationProvider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-658">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="0c531-659">Utwórz klasę implementującą <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="0c531-659">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="0c531-660">*EFConfigurationProvider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-660">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="0c531-661">Utwórz niestandardowego dostawcę konfiguracji, dziedziczących od <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="0c531-661">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="0c531-662">Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta.</span><span class="sxs-lookup"><span data-stu-id="0c531-662">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="0c531-663">*EFConfigurationProvider/EFConfigurationProvider. cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-663">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="0c531-664">Metoda rozszerzająca zezwala na Dodawanie źródła konfiguracji `ConfigurationBuilder`do. `AddEFConfiguration`</span><span class="sxs-lookup"><span data-stu-id="0c531-664">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="0c531-665">*Rozszerzenia/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-665">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="0c531-666">Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="0c531-666">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="0c531-667">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="0c531-667">Access configuration during startup</span></span>

<span data-ttu-id="0c531-668">Wsuń `IConfiguration` do konstruktora `Startup` , aby uzyskać dostęp do wartości `Startup.ConfigureServices`konfiguracyjnych w.</span><span class="sxs-lookup"><span data-stu-id="0c531-668">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0c531-669">Aby uzyskać dostęp do `Startup.Configure`konfiguracji w programie `IConfiguration` , należy wstrzyknąć bezpośrednio do metody lub użyć wystąpienia z konstruktora:</span><span class="sxs-lookup"><span data-stu-id="0c531-669">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="0c531-670">Aby zapoznać się z przykładem uzyskiwania dostępu do konfiguracji przy użyciu [metod uruchamiania, zobacz Uruchamianie aplikacji: Wygodne metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="0c531-670">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="0c531-671">Konfiguracja dostępu na stronie Razor Pages lub widoku MVC</span><span class="sxs-lookup"><span data-stu-id="0c531-671">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="0c531-672">Aby uzyskać dostęp do ustawień konfiguracji na stronie Razor Pages lub widoku MVC, Dodaj [dyrektywę using](xref:mvc/views/razor#using) ([ C# odwołanie: Using](/dotnet/csharp/language-reference/keywords/using-directive)) dla [przestrzeni nazw Microsoft. Extensions. Configuration](xref:Microsoft.Extensions.Configuration) i wsuń <xref:Microsoft.Extensions.Configuration.IConfiguration> ją na stronę lub widokiem.</span><span class="sxs-lookup"><span data-stu-id="0c531-672">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="0c531-673">Na stronie Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="0c531-673">In a Razor Pages page:</span></span>

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

<span data-ttu-id="0c531-674">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="0c531-674">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="0c531-675">Dodawanie konfiguracji z zestawu zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="0c531-675">Add configuration from an external assembly</span></span>

<span data-ttu-id="0c531-676">Implementacja umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zewnętrznego zestawu poza `Startup` klasą aplikacji. <xref:Microsoft.AspNetCore.Hosting.IHostingStartup></span><span class="sxs-lookup"><span data-stu-id="0c531-676">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="0c531-677">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0c531-677">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0c531-678">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0c531-678">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
