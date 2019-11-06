---
title: Konfiguracja w ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core przy użyciu interfejsu API konfiguracji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 9f0ad2791e504a0ff46daad07054b6bf909a546a
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634075"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="1c66e-103">Konfiguracja w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c66e-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="1c66e-104">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1c66e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1c66e-105">Konfiguracja aplikacji w ASP.NET Core jest oparta na parach klucz-wartość określonych przez *dostawców konfiguracji*.</span><span class="sxs-lookup"><span data-stu-id="1c66e-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="1c66e-106">Dostawcy konfiguracji odczytują dane konfiguracji do par klucz-wartość z różnych źródeł konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="1c66e-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1c66e-107">Azure Key Vault</span></span>
* <span data-ttu-id="1c66e-108">Konfiguracja aplikacji platformy Azure</span><span class="sxs-lookup"><span data-stu-id="1c66e-108">Azure App Configuration</span></span>
* <span data-ttu-id="1c66e-109">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1c66e-109">Command-line arguments</span></span>
* <span data-ttu-id="1c66e-110">Dostawcy niestandardowi (instalowani lub utworzony)</span><span class="sxs-lookup"><span data-stu-id="1c66e-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="1c66e-111">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="1c66e-111">Directory files</span></span>
* <span data-ttu-id="1c66e-112">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="1c66e-112">Environment variables</span></span>
* <span data-ttu-id="1c66e-113">Obiekty w pamięci .NET</span><span class="sxs-lookup"><span data-stu-id="1c66e-113">In-memory .NET objects</span></span>
* <span data-ttu-id="1c66e-114">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="1c66e-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c66e-115">Pakiety konfiguracyjne dla typowych scenariuszy dostawcy konfiguracji ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) są dołączone niejawnie przez platformę.</span><span class="sxs-lookup"><span data-stu-id="1c66e-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c66e-116">Pakiety konfiguracyjne dla typowych scenariuszy dostawcy konfiguracji ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1c66e-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="1c66e-117">Przykłady kodu, które obserwują i w przykładowej aplikacji używają przestrzeni nazw <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="1c66e-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="1c66e-118">*Wzorzec opcji* jest rozszerzeniem pojęć konfiguracyjnych opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="1c66e-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="1c66e-119">Opcje używają klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="1c66e-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="1c66e-120">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="1c66e-121">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1c66e-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="1c66e-122">Host a konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="1c66e-122">Host versus app configuration</span></span>

<span data-ttu-id="1c66e-123">Przed skonfigurowaniem i uruchomieniem aplikacji *host* zostanie skonfigurowany i uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="1c66e-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="1c66e-124">Host jest odpowiedzialny za uruchamianie aplikacji i zarządzanie okresem istnienia.</span><span class="sxs-lookup"><span data-stu-id="1c66e-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1c66e-125">Zarówno aplikacja, jak i Host są konfigurowane przy użyciu dostawców konfiguracji opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="1c66e-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="1c66e-126">Klucz konfiguracji hosta — pary wartości są również uwzględnione w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="1c66e-127">Aby uzyskać więcej informacji na temat tego, jak dostawcy konfiguracji są używani podczas kompilowania hosta i jak źródła konfiguracji wpływają na konfigurację hosta, zobacz <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="1c66e-128">Konfiguracja domyślna</span><span class="sxs-lookup"><span data-stu-id="1c66e-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c66e-129">Aplikacje sieci Web oparte na ASP.NET Core [nowym szablonów dotnet](/dotnet/core/tools/dotnet-new) <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="1c66e-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="1c66e-130">`CreateDefaultBuilder` zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="1c66e-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="1c66e-131">Poniższe zasady dotyczą aplikacji korzystających z [hosta ogólnego](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="1c66e-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="1c66e-132">Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej podczas korzystania z [hosta sieci Web](xref:fundamentals/host/web-host), zobacz [wersję ASP.NET Core 2,2 tego tematu](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="1c66e-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="1c66e-133">Konfiguracja hosta jest poświadczona z:</span><span class="sxs-lookup"><span data-stu-id="1c66e-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="1c66e-134">Zmienne środowiskowe poprzedzone prefiksem `DOTNET_` (na przykład `DOTNET_ENVIRONMENT`) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="1c66e-135">Prefiks (`DOTNET_`) jest usuwany podczas ładowania par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="1c66e-136">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="1c66e-137">Konfiguracja domyślna hosta sieci Web (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="1c66e-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="1c66e-138">Kestrel jest używany jako serwer sieci Web i konfigurowany przy użyciu dostawców konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="1c66e-139">Dodaj oprogramowanie pośredniczące do filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="1c66e-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="1c66e-140">Dodaj przekierowane nagłówki — oprogramowanie pośredniczące, jeśli zmienna środowiskowa `ASPNETCORE_FORWARDEDHEADERS_ENABLED` jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="1c66e-141">Włącz integrację usług IIS.</span><span class="sxs-lookup"><span data-stu-id="1c66e-141">Enable IIS integration.</span></span>
* <span data-ttu-id="1c66e-142">Podano konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="1c66e-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="1c66e-143">*appSettings. JSON* przy użyciu [dostawcy konfiguracji plików](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1c66e-144">*appSettings. {Environment}. JSON* przy użyciu [dostawcy konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1c66e-145">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development` przy użyciu zestawu wpisów.</span><span class="sxs-lookup"><span data-stu-id="1c66e-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="1c66e-146">Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="1c66e-147">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c66e-148">Aplikacje sieci Web oparte na ASP.NET Core [nowym szablonów dotnet](/dotnet/core/tools/dotnet-new) <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="1c66e-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="1c66e-149">`CreateDefaultBuilder` zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="1c66e-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="1c66e-150">Poniższe zasady dotyczą aplikacji korzystających z [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="1c66e-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="1c66e-151">Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej w przypadku korzystania z [hosta ogólnego](xref:fundamentals/host/generic-host), zobacz [najnowszą wersję tego tematu](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="1c66e-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="1c66e-152">Konfiguracja hosta jest poświadczona z:</span><span class="sxs-lookup"><span data-stu-id="1c66e-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="1c66e-153">Zmienne środowiskowe poprzedzone prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="1c66e-154">Prefiks (`ASPNETCORE_`) jest usuwany podczas ładowania par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="1c66e-155">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="1c66e-156">Podano konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="1c66e-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="1c66e-157">*appSettings. JSON* przy użyciu [dostawcy konfiguracji plików](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1c66e-158">*appSettings. {Environment}. JSON* przy użyciu [dostawcy konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1c66e-159">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development` przy użyciu zestawu wpisów.</span><span class="sxs-lookup"><span data-stu-id="1c66e-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="1c66e-160">Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="1c66e-161">Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c66e-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="1c66e-162">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="1c66e-162">Security</span></span>

<span data-ttu-id="1c66e-163">Aby zabezpieczyć poufne dane konfiguracji, należy zastosować następujące rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="1c66e-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="1c66e-164">Nie należy przechowywać haseł ani innych danych poufnych w kodzie dostawcy konfiguracji ani w plikach konfiguracji zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="1c66e-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="1c66e-165">Nie używaj tajemnic produkcyjnych w środowiskach deweloperskich i testowych.</span><span class="sxs-lookup"><span data-stu-id="1c66e-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="1c66e-166">Określ wpisy tajne poza projektem, aby nie mogły zostać przypadkowo przekazane do repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="1c66e-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="1c66e-167">Więcej informacji znajduje się w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="1c66e-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="1c66e-168"><xref:security/app-secrets> &ndash; zawiera porady dotyczące używania zmiennych środowiskowych do przechowywania poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="1c66e-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="1c66e-169">Menedżer wpisów tajnych używa dostawcy konfiguracji plików do przechowywania wpisów tajnych użytkownika w pliku JSON w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="1c66e-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="1c66e-170">Dostawca konfiguracji plików został opisany w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="1c66e-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="1c66e-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) bezpieczne przechowywanie wpisów tajnych aplikacji dla ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="1c66e-172">Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="1c66e-173">Hierarchiczne dane konfiguracji</span><span class="sxs-lookup"><span data-stu-id="1c66e-173">Hierarchical configuration data</span></span>

<span data-ttu-id="1c66e-174">Interfejs API konfiguracji jest w stanie utrzymywać hierarchiczne dane konfiguracji przez spłaszczonie danych hierarchicznych przy użyciu ogranicznika w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="1c66e-175">W poniższym pliku JSON cztery klucze istnieją w hierarchii strukturalnej dwóch sekcji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="1c66e-176">Gdy plik jest odczytywany do konfiguracji, są tworzone unikatowe klucze, aby zachować oryginalną hierarchiczną strukturę danych źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="1c66e-177">Sekcje i klucze są spłaszczone przy użyciu dwukropka (`:`), aby zachować oryginalną strukturę:</span><span class="sxs-lookup"><span data-stu-id="1c66e-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="1c66e-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c66e-178">section0:key0</span></span>
* <span data-ttu-id="1c66e-179">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="1c66e-179">section0:key1</span></span>
* <span data-ttu-id="1c66e-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1c66e-180">section1:key0</span></span>
* <span data-ttu-id="1c66e-181">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="1c66e-181">section1:key1</span></span>

<span data-ttu-id="1c66e-182">Metody <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> są dostępne do izolowania sekcji i elementów podrzędnych sekcji w danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="1c66e-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="1c66e-183">Te metody są opisane w dalszej [części GetSection, GetChildren i EXISTS](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="1c66e-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="1c66e-184">Konwencje</span><span class="sxs-lookup"><span data-stu-id="1c66e-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="1c66e-185">Źródła i dostawcy</span><span class="sxs-lookup"><span data-stu-id="1c66e-185">Sources and providers</span></span>

<span data-ttu-id="1c66e-186">Podczas uruchamiania aplikacji źródła konfiguracji są odczytywane w kolejności, w jakiej są określone przez ich dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="1c66e-187">Dostawcy konfiguracji implementujący funkcję wykrywania zmian mają możliwość ponownego załadowania konfiguracji, gdy ustawienie podstawowe zostanie zmienione.</span><span class="sxs-lookup"><span data-stu-id="1c66e-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="1c66e-188">Na przykład dostawca konfiguracji plików (opisany w dalszej części tego tematu) i [dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) implementują wykrywanie zmian.</span><span class="sxs-lookup"><span data-stu-id="1c66e-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="1c66e-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> jest dostępny w kontenerze [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1c66e-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> można wstrzyknąć do Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> w celu uzyskania konfiguracji dla klasy:</span><span class="sxs-lookup"><span data-stu-id="1c66e-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

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

<span data-ttu-id="1c66e-191">Dostawcy konfiguracji nie mogą używać DI, ponieważ są niedostępne, gdy są skonfigurowane przez hosta.</span><span class="sxs-lookup"><span data-stu-id="1c66e-191">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="1c66e-192">Ponownie</span><span class="sxs-lookup"><span data-stu-id="1c66e-192">Keys</span></span>

<span data-ttu-id="1c66e-193">Klucze konfiguracji przyjmują następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="1c66e-193">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="1c66e-194">W kluczach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="1c66e-194">Keys are case-insensitive.</span></span> <span data-ttu-id="1c66e-195">Na przykład `ConnectionString` i `connectionstring` są traktowane jako klucze równoważne.</span><span class="sxs-lookup"><span data-stu-id="1c66e-195">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="1c66e-196">Jeśli wartość tego samego klucza jest ustawiana przez tych samych lub różnych dostawców konfiguracji, Ostatnia wartość ustawiona w tym kluczu jest używana.</span><span class="sxs-lookup"><span data-stu-id="1c66e-196">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="1c66e-197">Klucze hierarchiczne</span><span class="sxs-lookup"><span data-stu-id="1c66e-197">Hierarchical keys</span></span>
  * <span data-ttu-id="1c66e-198">W interfejsie API konfiguracji, separator dwukropek (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="1c66e-198">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="1c66e-199">W zmiennych środowiskowych separator dwukropek może nie zadziałał na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="1c66e-199">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="1c66e-200">Podwójne podkreślenie (`__`) jest obsługiwane przez wszystkie platformy i automatycznie konwertowane na dwukropek.</span><span class="sxs-lookup"><span data-stu-id="1c66e-200">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="1c66e-201">W Azure Key Vault klucze hierarchiczne używają `--` (dwóch kresek) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="1c66e-201">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="1c66e-202">Należy podać kod, aby zastąpić łączniki dwukropkiem, gdy wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-202">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="1c66e-203"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-203">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1c66e-204">Powiązanie tablicowe zostało opisane w sekcji [Powiązywanie tablicy z klasą](#bind-an-array-to-a-class) .</span><span class="sxs-lookup"><span data-stu-id="1c66e-204">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="1c66e-205">Wartości</span><span class="sxs-lookup"><span data-stu-id="1c66e-205">Values</span></span>

<span data-ttu-id="1c66e-206">Wartości konfiguracyjne przyjmują następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="1c66e-206">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="1c66e-207">Wartości są ciągami.</span><span class="sxs-lookup"><span data-stu-id="1c66e-207">Values are strings.</span></span>
* <span data-ttu-id="1c66e-208">Wartości null nie można przechowywać w konfiguracji ani powiązana z obiektami.</span><span class="sxs-lookup"><span data-stu-id="1c66e-208">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="1c66e-209">udostępnia</span><span class="sxs-lookup"><span data-stu-id="1c66e-209">Providers</span></span>

<span data-ttu-id="1c66e-210">W poniższej tabeli przedstawiono dostawców konfiguracji dostępnych do ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-210">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="1c66e-211">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="1c66e-211">Provider</span></span> | <span data-ttu-id="1c66e-212">Zapewnia konfigurację z &hellip;</span><span class="sxs-lookup"><span data-stu-id="1c66e-212">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="1c66e-213">[Dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) (tematy dotyczące*zabezpieczeń* )</span><span class="sxs-lookup"><span data-stu-id="1c66e-213">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="1c66e-214">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1c66e-214">Azure Key Vault</span></span> |
| <span data-ttu-id="1c66e-215">[Dostawca konfiguracji aplikacji platformy Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (dokumentacja platformy Azure)</span><span class="sxs-lookup"><span data-stu-id="1c66e-215">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="1c66e-216">Konfiguracja aplikacji platformy Azure</span><span class="sxs-lookup"><span data-stu-id="1c66e-216">Azure App Configuration</span></span> |
| [<span data-ttu-id="1c66e-217">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1c66e-217">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="1c66e-218">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1c66e-218">Command-line parameters</span></span> |
| [<span data-ttu-id="1c66e-219">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="1c66e-219">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="1c66e-220">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="1c66e-220">Custom source</span></span> |
| [<span data-ttu-id="1c66e-221">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="1c66e-221">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="1c66e-222">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="1c66e-222">Environment variables</span></span> |
| [<span data-ttu-id="1c66e-223">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="1c66e-223">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="1c66e-224">Pliki (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="1c66e-224">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="1c66e-225">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="1c66e-225">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="1c66e-226">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="1c66e-226">Directory files</span></span> |
| [<span data-ttu-id="1c66e-227">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="1c66e-227">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="1c66e-228">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="1c66e-228">In-memory collections</span></span> |
| <span data-ttu-id="1c66e-229">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tematy dotyczące*zabezpieczeń* )</span><span class="sxs-lookup"><span data-stu-id="1c66e-229">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="1c66e-230">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="1c66e-230">File in the user profile directory</span></span> |

<span data-ttu-id="1c66e-231">Źródła konfiguracji są odczytywane w kolejności, w jakiej dostawcy konfiguracji są określeni podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1c66e-231">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="1c66e-232">Dostawcy konfiguracji opisane w tym temacie są opisywane w kolejności alfabetycznej, a nie w kolejności, w jakiej kod może je rozmieścić.</span><span class="sxs-lookup"><span data-stu-id="1c66e-232">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="1c66e-233">Zamów dostawcy konfiguracji w kodzie, aby odpowiadały Twoim priorytetom dla źródłowych źródeł konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-233">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="1c66e-234">Typową sekwencją dostawców konfiguracji jest:</span><span class="sxs-lookup"><span data-stu-id="1c66e-234">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="1c66e-235">Pliki (*appSettings. JSON*, *appSettings. { Environment}. JSON*, gdzie `{Environment}` jest bieżącym środowiskiem hostingu aplikacji)</span><span class="sxs-lookup"><span data-stu-id="1c66e-235">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="1c66e-236">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1c66e-236">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="1c66e-237">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tylko środowisko programistyczne)</span><span class="sxs-lookup"><span data-stu-id="1c66e-237">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="1c66e-238">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="1c66e-238">Environment variables</span></span>
1. <span data-ttu-id="1c66e-239">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1c66e-239">Command-line arguments</span></span>

<span data-ttu-id="1c66e-240">Typowym celem jest umieszczenie dostawcy konfiguracji wiersza polecenia jako ostatni w serii dostawców, aby zezwolić na argumenty wiersza polecenia, aby przesłonić konfigurację ustawioną przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="1c66e-240">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="1c66e-241">Poprzednia sekwencja dostawców jest używana po zainicjowaniu nowego konstruktora hostów przy użyciu `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-241">The preceding sequence of providers is used when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c66e-242">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="1c66e-242">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="1c66e-243">Konfigurowanie konstruktora hostów za pomocą ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="1c66e-243">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="1c66e-244">Aby skonfigurować konstruktora hostów, wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> i podaj konfigurację.</span><span class="sxs-lookup"><span data-stu-id="1c66e-244">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="1c66e-245">`ConfigureHostConfiguration` służy do inicjowania <xref:Microsoft.Extensions.Hosting.IHostEnvironment> do późniejszego użycia w procesie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-245">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="1c66e-246">`ConfigureHostConfiguration` może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="1c66e-246">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="1c66e-247">Konfigurowanie konstruktora hostów za pomocą UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="1c66e-247">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="1c66e-248">Aby skonfigurować konstruktora hostów, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> w konstruktorze hosta z konfiguracją.</span><span class="sxs-lookup"><span data-stu-id="1c66e-248">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="1c66e-249">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="1c66e-249">ConfigureAppConfiguration</span></span>

<span data-ttu-id="1c66e-250">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić dostawców konfiguracji aplikacji oprócz tych dodanych automatycznie przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1c66e-250">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="1c66e-251">Zastąp poprzednią konfigurację argumentami wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1c66e-251">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="1c66e-252">Aby podać konfigurację aplikacji, którą można zastąpić za pomocą argumentów wiersza polecenia, wywołaj `AddCommandLine` Last:</span><span class="sxs-lookup"><span data-stu-id="1c66e-252">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="1c66e-253">Usuń dostawców dodanych przez CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="1c66e-253">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="1c66e-254">Aby usunąć dostawców dodanych przez `CreateDefaultBuilder`, wywołaj najpierw polecenie [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) w [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="1c66e-254">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="1c66e-255">Użyj konfiguracji podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="1c66e-255">Consume configuration during app startup</span></span>

<span data-ttu-id="1c66e-256">Konfiguracja dostarczona do aplikacji w `ConfigureAppConfiguration` jest dostępna podczas uruchamiania aplikacji, w tym `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-256">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1c66e-257">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja dostępu podczas uruchamiania](#access-configuration-during-startup) .</span><span class="sxs-lookup"><span data-stu-id="1c66e-257">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="1c66e-258">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1c66e-258">Command-line Configuration Provider</span></span>

<span data-ttu-id="1c66e-259"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> ładuje konfigurację z par klucz-wartość argumentu wiersza polecenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1c66e-259">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="1c66e-260">Aby uaktywnić konfigurację wiersza polecenia, Metoda rozszerzenia <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> jest wywoływana w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-260">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c66e-261">`AddCommandLine` jest wywoływana automatycznie, gdy zostanie wywołana `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-261">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="1c66e-262">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="1c66e-262">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="1c66e-263">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="1c66e-263">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1c66e-264">Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.</span><span class="sxs-lookup"><span data-stu-id="1c66e-264">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="1c66e-265">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="1c66e-265">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="1c66e-266">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="1c66e-266">Environment variables.</span></span>

<span data-ttu-id="1c66e-267">`CreateDefaultBuilder` dodaje dostawcę konfiguracji wiersza polecenia Last.</span><span class="sxs-lookup"><span data-stu-id="1c66e-267">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="1c66e-268">Argumenty wiersza polecenia przekazane w czasie wykonywania zastępują konfigurację ustawioną przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="1c66e-268">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="1c66e-269">`CreateDefaultBuilder` działa, gdy host jest skonstruowany.</span><span class="sxs-lookup"><span data-stu-id="1c66e-269">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="1c66e-270">W związku z tym konfiguracja wiersza polecenia aktywowana przez `CreateDefaultBuilder` może mieć wpływ na sposób konfigurowania hosta.</span><span class="sxs-lookup"><span data-stu-id="1c66e-270">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="1c66e-271">W przypadku aplikacji opartych na ASP.NET Core szablonach `AddCommandLine` została już wywołana przez `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-271">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c66e-272">Aby dodać kolejnych dostawców konfiguracji i zachować możliwość przesłonięcia konfiguracji od tych dostawców za pomocą argumentów wiersza polecenia, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddCommandLine` Last.</span><span class="sxs-lookup"><span data-stu-id="1c66e-272">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="1c66e-273">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="1c66e-273">**Example**</span></span>

<span data-ttu-id="1c66e-274">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-274">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="1c66e-275">Otwórz wiersz polecenia w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="1c66e-275">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="1c66e-276">Podaj argument wiersza polecenia do polecenia `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-276">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="1c66e-277">Po uruchomieniu aplikacji otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-277">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1c66e-278">Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla argumentu wiersza polecenia konfiguracji dostarczonego do `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-278">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="1c66e-279">Argumenty</span><span class="sxs-lookup"><span data-stu-id="1c66e-279">Arguments</span></span>

<span data-ttu-id="1c66e-280">Wartość musi następować po znaku równości (`=`) lub klucz musi mieć prefiks (`--` lub `/`), gdy wartość znajduje się w miejscu.</span><span class="sxs-lookup"><span data-stu-id="1c66e-280">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="1c66e-281">Wartość nie jest wymagana, jeśli jest używany znak równości (na przykład `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="1c66e-281">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="1c66e-282">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="1c66e-282">Key prefix</span></span>               | <span data-ttu-id="1c66e-283">Przykład</span><span class="sxs-lookup"><span data-stu-id="1c66e-283">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="1c66e-284">Brak prefiksu</span><span class="sxs-lookup"><span data-stu-id="1c66e-284">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="1c66e-285">Dwie kreski (`--`)</span><span class="sxs-lookup"><span data-stu-id="1c66e-285">Two dashes (`--`)</span></span>        | <span data-ttu-id="1c66e-286">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="1c66e-286">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="1c66e-287">Ukośnik (`/`)</span><span class="sxs-lookup"><span data-stu-id="1c66e-287">Forward slash (`/`)</span></span>      | <span data-ttu-id="1c66e-288">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="1c66e-288">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="1c66e-289">W tym samym poleceniu nie należy mieszać par klucz-wartość argumentu wiersza polecenia, które używają znaku równości z parami klucz-wartość, które używają spacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-289">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="1c66e-290">Przykładowe polecenia:</span><span class="sxs-lookup"><span data-stu-id="1c66e-290">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="1c66e-291">Mapowanie przełączników</span><span class="sxs-lookup"><span data-stu-id="1c66e-291">Switch mappings</span></span>

<span data-ttu-id="1c66e-292">Mapowania przełączników Zezwalaj na logikę zamiany nazwy klucza.</span><span class="sxs-lookup"><span data-stu-id="1c66e-292">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="1c66e-293">Podczas ręcznego kompilowania konfiguracji za pomocą <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> można dostarczyć słownik przemieszczeń przełączeń do metody <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-293">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="1c66e-294">Gdy jest używany słownik mapowania przełączników, słownik jest sprawdzany dla klucza, który pasuje do klucza dostarczonego przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="1c66e-294">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="1c66e-295">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) zostanie przeniesiona z powrotem, aby ustawić parę klucz-wartość w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-295">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="1c66e-296">Mapowanie przełącznika jest wymagane dla każdego klucza wiersza polecenia poprzedzonego jedną kreską (`-`).</span><span class="sxs-lookup"><span data-stu-id="1c66e-296">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="1c66e-297">Przełącz reguły klucza słownika mapowania:</span><span class="sxs-lookup"><span data-stu-id="1c66e-297">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="1c66e-298">Przełączniki muszą zaczynać się kreską (`-`) lub podwójną kreską (`--`).</span><span class="sxs-lookup"><span data-stu-id="1c66e-298">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="1c66e-299">Słownik mapowania przełącznika nie może zawierać zduplikowanych kluczy.</span><span class="sxs-lookup"><span data-stu-id="1c66e-299">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="1c66e-300">Utwórz słownik mapowań mapowania.</span><span class="sxs-lookup"><span data-stu-id="1c66e-300">Create a switch mappings dictionary.</span></span> <span data-ttu-id="1c66e-301">W poniższym przykładzie są tworzone dwa mapowania przełączników:</span><span class="sxs-lookup"><span data-stu-id="1c66e-301">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="1c66e-302">Po skompilowaniu hosta Wywołaj `AddCommandLine` przy użyciu słownika mapowania przełączników:</span><span class="sxs-lookup"><span data-stu-id="1c66e-302">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="1c66e-303">W przypadku aplikacji korzystających z mapowań przełączników wywołanie `CreateDefaultBuilder` nie powinno przekazywać argumentów.</span><span class="sxs-lookup"><span data-stu-id="1c66e-303">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="1c66e-304">Wywołanie `AddCommandLine` metody `CreateDefaultBuilder` nie obejmuje zamapowanych przełączników i nie ma sposobu przekazywania słownika mapowania przełącznika do `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-304">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c66e-305">Rozwiązanie nie przekazuje argumentów do `CreateDefaultBuilder`, ale zamiast zezwalać `AddCommandLine` metody `ConfigurationBuilder` do przetwarzania zarówno argumentów, jak i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="1c66e-305">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="1c66e-306">Po utworzeniu słownika mapowań przełączników zawiera dane przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="1c66e-306">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="1c66e-307">Key</span><span class="sxs-lookup"><span data-stu-id="1c66e-307">Key</span></span>       | <span data-ttu-id="1c66e-308">Wartość</span><span class="sxs-lookup"><span data-stu-id="1c66e-308">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="1c66e-309">Jeśli klucze mapowane przez przełącznik są używane podczas uruchamiania aplikacji, konfiguracja otrzymuje wartość konfiguracji klucza dostarczonego przez słownik:</span><span class="sxs-lookup"><span data-stu-id="1c66e-309">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="1c66e-310">Po uruchomieniu poprzedniego polecenia Konfiguracja zawiera wartości pokazane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="1c66e-310">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="1c66e-311">Key</span><span class="sxs-lookup"><span data-stu-id="1c66e-311">Key</span></span>               | <span data-ttu-id="1c66e-312">Wartość</span><span class="sxs-lookup"><span data-stu-id="1c66e-312">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="1c66e-313">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="1c66e-313">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="1c66e-314"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> ładuje konfigurację ze par klucz-wartość zmiennej środowiskowej w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1c66e-314">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="1c66e-315">Aby uaktywnić konfigurację zmiennych środowiskowych, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-315">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="1c66e-316">[Azure App Service](https://azure.microsoft.com/services/app-service/) pozwala ustawić zmienne środowiskowe w witrynie Azure Portal, które mogą przesłonić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="1c66e-316">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="1c66e-317">Aby uzyskać więcej informacji, zobacz artykuł [Azure Apps: zastępowanie konfiguracji aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="1c66e-317">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c66e-318">`AddEnvironmentVariables` jest używany do ładowania zmiennych środowiskowych, które są poprzedzone `DOTNET_` na potrzeby [konfiguracji hosta](#host-versus-app-configuration) , gdy nowy Konstruktor hosta zostanie zainicjowany z [hostem ogólnym](xref:fundamentals/host/generic-host) i zostanie wywołane `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-318">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="1c66e-319">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="1c66e-319">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c66e-320">`AddEnvironmentVariables` jest używany do ładowania zmiennych środowiskowych, które są poprzedzone `ASPNETCORE_` na potrzeby [konfiguracji hosta](#host-versus-app-configuration) , gdy nowy Konstruktor hosta zostanie zainicjowany przy użyciu [hosta sieci Web](xref:fundamentals/host/web-host) i zostanie wywołane `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-320">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="1c66e-321">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="1c66e-321">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="1c66e-322">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="1c66e-322">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1c66e-323">Konfiguracja aplikacji z nieoznaczonych zmiennych środowiskowych przez wywołanie `AddEnvironmentVariables` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="1c66e-323">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="1c66e-324">Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.</span><span class="sxs-lookup"><span data-stu-id="1c66e-324">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="1c66e-325">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="1c66e-325">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="1c66e-326">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="1c66e-326">Command-line arguments.</span></span>

<span data-ttu-id="1c66e-327">Dostawca konfiguracji zmiennych środowiskowych jest wywoływany po ustanowieniu konfiguracji z poziomu kluczy tajnych użytkownika i plików *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="1c66e-327">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="1c66e-328">Wywołanie dostawcy w tym miejscu pozwala odczytywać zmienne środowiskowe w czasie wykonywania w celu przesłania konfiguracji ustawionych przez klucze tajne użytkownika i pliki *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="1c66e-328">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="1c66e-329">Jeśli musisz podać konfigurację aplikacji na podstawie dodatkowych zmiennych środowiskowych, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddEnvironmentVariables` z prefiksem.</span><span class="sxs-lookup"><span data-stu-id="1c66e-329">If you need to provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="1c66e-330">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="1c66e-330">**Example**</span></span>

<span data-ttu-id="1c66e-331">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-331">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="1c66e-332">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="1c66e-332">Run the sample app.</span></span> <span data-ttu-id="1c66e-333">Otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-333">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1c66e-334">Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla zmiennej środowiskowej `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-334">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="1c66e-335">Wartość odzwierciedla środowisko, w którym jest uruchomiona aplikacja, zwykle `Development` podczas uruchamiania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1c66e-335">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="1c66e-336">Aby zachować listę zmiennych środowiskowych renderowanych przez aplikację, aplikacja filtruje zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="1c66e-336">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="1c66e-337">Zapoznaj się z plikiem przykładowej *strony aplikacji/index. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="1c66e-337">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="1c66e-338">Jeśli chcesz uwidocznić wszystkie zmienne środowiskowe dostępne dla aplikacji, Zmień `FilteredConfiguration` na *stronie/index. cshtml. cs* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1c66e-338">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="1c66e-339">Prefiksy</span><span class="sxs-lookup"><span data-stu-id="1c66e-339">Prefixes</span></span>

<span data-ttu-id="1c66e-340">Zmienne środowiskowe ładowane do konfiguracji aplikacji są filtrowane w przypadku podania prefiksu do metody `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-340">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="1c66e-341">Na przykład aby filtrować zmienne środowiskowe na prefiksie `CUSTOM_`, podaj prefiks dla dostawcy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-341">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="1c66e-342">Prefiks jest usuwany podczas tworzenia par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-342">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="1c66e-343">Podczas tworzenia konstruktora hostów Konfiguracja hosta jest zapewniana przez zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="1c66e-343">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="1c66e-344">Aby uzyskać więcej informacji na temat prefiksu używanego dla tych zmiennych środowiskowych, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="1c66e-344">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="1c66e-345">**Prefiksy parametrów połączenia**</span><span class="sxs-lookup"><span data-stu-id="1c66e-345">**Connection string prefixes**</span></span>

<span data-ttu-id="1c66e-346">Interfejs API konfiguracji ma specjalne reguły przetwarzania dla czterech zmiennych środowiskowych parametrów połączenia związanych z konfigurowaniem parametrów połączenia platformy Azure dla środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-346">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="1c66e-347">Zmienne środowiskowe z prefiksami podanymi w tabeli są ładowane do aplikacji, jeśli nie podano prefiksu do `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-347">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="1c66e-348">Prefiks parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="1c66e-348">Connection string prefix</span></span> | <span data-ttu-id="1c66e-349">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="1c66e-349">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="1c66e-350">Dostawca niestandardowy</span><span class="sxs-lookup"><span data-stu-id="1c66e-350">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="1c66e-351">MySQL</span><span class="sxs-lookup"><span data-stu-id="1c66e-351">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="1c66e-352">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="1c66e-352">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="1c66e-353">SQL Server</span><span class="sxs-lookup"><span data-stu-id="1c66e-353">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="1c66e-354">Gdy zmienna środowiskowa zostanie odnaleziona i załadowana do konfiguracji z dowolnymi z czterech prefiksów pokazanych w tabeli:</span><span class="sxs-lookup"><span data-stu-id="1c66e-354">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="1c66e-355">Klucz konfiguracji jest tworzony przez usunięcie prefiksu zmiennej środowiskowej i dodanie sekcji klucza konfiguracji (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="1c66e-355">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="1c66e-356">Zostanie utworzona nowa para klucz-wartość konfiguracji, która reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, które nie ma określonego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="1c66e-356">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="1c66e-357">Klucz zmiennej środowiskowej</span><span class="sxs-lookup"><span data-stu-id="1c66e-357">Environment variable key</span></span> | <span data-ttu-id="1c66e-358">Przekonwertowany klucz konfiguracji</span><span class="sxs-lookup"><span data-stu-id="1c66e-358">Converted configuration key</span></span> | <span data-ttu-id="1c66e-359">Wpis konfiguracji dostawcy</span><span class="sxs-lookup"><span data-stu-id="1c66e-359">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1c66e-360">Wpis konfiguracji nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="1c66e-360">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1c66e-361">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1c66e-361">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1c66e-362">Wartość: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c66e-362">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1c66e-363">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1c66e-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1c66e-364">Wartość: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c66e-364">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1c66e-365">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1c66e-365">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1c66e-366">Wartość: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c66e-366">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="1c66e-367">Dostawca konfiguracji plików</span><span class="sxs-lookup"><span data-stu-id="1c66e-367">File Configuration Provider</span></span>

<span data-ttu-id="1c66e-368"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową do ładowania konfiguracji z systemu plików.</span><span class="sxs-lookup"><span data-stu-id="1c66e-368"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="1c66e-369">Następujący dostawcy konfiguracji są przydzielone do określonych typów plików:</span><span class="sxs-lookup"><span data-stu-id="1c66e-369">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="1c66e-370">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="1c66e-370">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="1c66e-371">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="1c66e-371">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="1c66e-372">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="1c66e-372">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="1c66e-373">Dostawca konfiguracji pliku INI</span><span class="sxs-lookup"><span data-stu-id="1c66e-373">INI Configuration Provider</span></span>

<span data-ttu-id="1c66e-374"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku INI w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1c66e-374">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="1c66e-375">Aby uaktywnić konfigurację pliku INI, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-375">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c66e-376">Dwukropek może służyć jako ogranicznik sekcji w konfiguracji pliku INI.</span><span class="sxs-lookup"><span data-stu-id="1c66e-376">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="1c66e-377">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="1c66e-377">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c66e-378">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="1c66e-378">Whether the file is optional.</span></span>
* <span data-ttu-id="1c66e-379">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="1c66e-379">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1c66e-380"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="1c66e-380">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1c66e-381">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-381">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="1c66e-382">Ogólny przykład pliku konfiguracji INI:</span><span class="sxs-lookup"><span data-stu-id="1c66e-382">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="1c66e-383">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="1c66e-383">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c66e-384">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c66e-384">section0:key0</span></span>
* <span data-ttu-id="1c66e-385">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="1c66e-385">section0:key1</span></span>
* <span data-ttu-id="1c66e-386">Section1: podsekcja: Key</span><span class="sxs-lookup"><span data-stu-id="1c66e-386">section1:subsection:key</span></span>
* <span data-ttu-id="1c66e-387">section2: subsection0: klucz</span><span class="sxs-lookup"><span data-stu-id="1c66e-387">section2:subsection0:key</span></span>
* <span data-ttu-id="1c66e-388">section2: subsection1: klucz</span><span class="sxs-lookup"><span data-stu-id="1c66e-388">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="1c66e-389">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="1c66e-389">JSON Configuration Provider</span></span>

<span data-ttu-id="1c66e-390"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku JSON podczas środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1c66e-390">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="1c66e-391">Aby uaktywnić konfigurację pliku JSON, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-391">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c66e-392">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="1c66e-392">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c66e-393">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="1c66e-393">Whether the file is optional.</span></span>
* <span data-ttu-id="1c66e-394">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="1c66e-394">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1c66e-395"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="1c66e-395">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1c66e-396">`AddJsonFile` jest automatycznie wywoływana dwukrotnie po zainicjowaniu nowego konstruktora hostów z `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-396">`AddJsonFile` is automatically called twice when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c66e-397">Metoda jest wywoływana w celu załadowania konfiguracji z:</span><span class="sxs-lookup"><span data-stu-id="1c66e-397">The method is called to load configuration from:</span></span>

* <span data-ttu-id="1c66e-398">*appSettings. json* &ndash; ten plik jest najpierw odczytywany.</span><span class="sxs-lookup"><span data-stu-id="1c66e-398">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="1c66e-399">Wersja środowiska pliku może przesłonić wartości dostarczone przez plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="1c66e-399">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="1c66e-400">*appSettings. {Environment}. JSON* &ndash; wersja środowiska pliku jest ładowana na podstawie [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="1c66e-400">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="1c66e-401">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .</span><span class="sxs-lookup"><span data-stu-id="1c66e-401">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="1c66e-402">`CreateDefaultBuilder` również ładowania:</span><span class="sxs-lookup"><span data-stu-id="1c66e-402">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1c66e-403">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="1c66e-403">Environment variables.</span></span>
* <span data-ttu-id="1c66e-404">Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="1c66e-404">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="1c66e-405">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="1c66e-405">Command-line arguments.</span></span>

<span data-ttu-id="1c66e-406">Dostawca konfiguracji JSON został ustanowiony jako pierwszy.</span><span class="sxs-lookup"><span data-stu-id="1c66e-406">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="1c66e-407">W związku z tym klucze tajne użytkownika, zmienne środowiskowe i argumenty wiersza polecenia przesłaniają konfigurację ustawioną przez pliki *AppSettings* .</span><span class="sxs-lookup"><span data-stu-id="1c66e-407">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="1c66e-408">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji dla plików innych niż *appSettings. JSON* i *appSettings. { Environment}. JSON*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-408">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="1c66e-409">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="1c66e-409">**Example**</span></span>

<span data-ttu-id="1c66e-410">Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje dwa wywołania `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-410">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="1c66e-411">Konfiguracja jest ładowana z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="1c66e-411">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="1c66e-412">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="1c66e-412">Run the sample app.</span></span> <span data-ttu-id="1c66e-413">Otwórz w przeglądarce aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-413">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1c66e-414">Zwróć uwagę, że dane wyjściowe zawierają pary klucz-wartość dla konfiguracji pokazanej w tabeli w zależności od środowiska.</span><span class="sxs-lookup"><span data-stu-id="1c66e-414">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="1c66e-415">Klucze konfiguracji rejestrowania używają dwukropka (`:`) jako separatora hierarchicznego.</span><span class="sxs-lookup"><span data-stu-id="1c66e-415">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="1c66e-416">Key</span><span class="sxs-lookup"><span data-stu-id="1c66e-416">Key</span></span>                        | <span data-ttu-id="1c66e-417">Wartość programistyczna</span><span class="sxs-lookup"><span data-stu-id="1c66e-417">Development Value</span></span> | <span data-ttu-id="1c66e-418">Wartość produkcyjna</span><span class="sxs-lookup"><span data-stu-id="1c66e-418">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="1c66e-419">Rejestrowanie: LogLevel: system</span><span class="sxs-lookup"><span data-stu-id="1c66e-419">Logging:LogLevel:System</span></span>    | <span data-ttu-id="1c66e-420">Informacje</span><span class="sxs-lookup"><span data-stu-id="1c66e-420">Information</span></span>       | <span data-ttu-id="1c66e-421">Informacje</span><span class="sxs-lookup"><span data-stu-id="1c66e-421">Information</span></span>      |
| <span data-ttu-id="1c66e-422">Rejestrowanie: LogLevel: Microsoft</span><span class="sxs-lookup"><span data-stu-id="1c66e-422">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="1c66e-423">Informacje</span><span class="sxs-lookup"><span data-stu-id="1c66e-423">Information</span></span>       | <span data-ttu-id="1c66e-424">Informacje</span><span class="sxs-lookup"><span data-stu-id="1c66e-424">Information</span></span>      |
| <span data-ttu-id="1c66e-425">Rejestrowanie: wartość domyślna: LogLevel</span><span class="sxs-lookup"><span data-stu-id="1c66e-425">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="1c66e-426">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="1c66e-426">Debug</span></span>             | <span data-ttu-id="1c66e-427">Błąd</span><span class="sxs-lookup"><span data-stu-id="1c66e-427">Error</span></span>            |
| <span data-ttu-id="1c66e-428">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="1c66e-428">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="1c66e-429">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="1c66e-429">XML Configuration Provider</span></span>

<span data-ttu-id="1c66e-430"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1c66e-430">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="1c66e-431">Aby uaktywnić konfigurację pliku XML, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-431">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c66e-432">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="1c66e-432">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c66e-433">Czy plik jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="1c66e-433">Whether the file is optional.</span></span>
* <span data-ttu-id="1c66e-434">Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="1c66e-434">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1c66e-435"><xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="1c66e-435">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1c66e-436">Węzeł główny pliku konfiguracji jest ignorowany, gdy są tworzone pary klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-436">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="1c66e-437">Nie określaj definicji typu dokumentu (DTD) ani przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="1c66e-437">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="1c66e-438">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-438">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="1c66e-439">Pliki konfiguracji XML mogą używać odrębnych nazw elementów dla powtarzających się sekcji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-439">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="1c66e-440">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="1c66e-440">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c66e-441">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c66e-441">section0:key0</span></span>
* <span data-ttu-id="1c66e-442">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="1c66e-442">section0:key1</span></span>
* <span data-ttu-id="1c66e-443">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1c66e-443">section1:key0</span></span>
* <span data-ttu-id="1c66e-444">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="1c66e-444">section1:key1</span></span>

<span data-ttu-id="1c66e-445">Powtarzalne elementy, które używają tej samej nazwy elementu, działają, jeśli atrybut `name` jest używany do odróżnienia elementów:</span><span class="sxs-lookup"><span data-stu-id="1c66e-445">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="1c66e-446">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="1c66e-446">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c66e-447">sekcja: section0: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="1c66e-447">section:section0:key:key0</span></span>
* <span data-ttu-id="1c66e-448">sekcja: section0: Key: Klucz1</span><span class="sxs-lookup"><span data-stu-id="1c66e-448">section:section0:key:key1</span></span>
* <span data-ttu-id="1c66e-449">sekcja: Section1: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="1c66e-449">section:section1:key:key0</span></span>
* <span data-ttu-id="1c66e-450">sekcja: Section1: Key: Klucz1</span><span class="sxs-lookup"><span data-stu-id="1c66e-450">section:section1:key:key1</span></span>

<span data-ttu-id="1c66e-451">Atrybuty mogą służyć do dostarczania wartości:</span><span class="sxs-lookup"><span data-stu-id="1c66e-451">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="1c66e-452">Poprzedni plik konfiguracji ładuje następujące klucze z `value`:</span><span class="sxs-lookup"><span data-stu-id="1c66e-452">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c66e-453">Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="1c66e-453">key:attribute</span></span>
* <span data-ttu-id="1c66e-454">sekcja: Key: Attribute</span><span class="sxs-lookup"><span data-stu-id="1c66e-454">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="1c66e-455">Dostawca konfiguracji klucza dla plików</span><span class="sxs-lookup"><span data-stu-id="1c66e-455">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="1c66e-456"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> używa plików katalogu jako par klucz konfiguracji i wartość.</span><span class="sxs-lookup"><span data-stu-id="1c66e-456">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="1c66e-457">Kluczem jest nazwa pliku.</span><span class="sxs-lookup"><span data-stu-id="1c66e-457">The key is the file name.</span></span> <span data-ttu-id="1c66e-458">Wartość zawiera zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="1c66e-458">The value contains the file's contents.</span></span> <span data-ttu-id="1c66e-459">Dostawca konfiguracji klucza dla plików jest używany w scenariuszach hostingu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="1c66e-459">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="1c66e-460">Aby uaktywnić konfigurację klucza dla plików, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-460">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="1c66e-461">`directoryPath` plików musi być ścieżką bezwzględną.</span><span class="sxs-lookup"><span data-stu-id="1c66e-461">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="1c66e-462">Przeciążania Zezwalaj na określanie:</span><span class="sxs-lookup"><span data-stu-id="1c66e-462">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c66e-463">Delegat `Action<KeyPerFileConfigurationSource>`, który konfiguruje źródło.</span><span class="sxs-lookup"><span data-stu-id="1c66e-463">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="1c66e-464">Określa, czy katalog jest opcjonalny, i ścieżkę do katalogu.</span><span class="sxs-lookup"><span data-stu-id="1c66e-464">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="1c66e-465">Podwójny znak podkreślenia (`__`) jest używany jako ogranicznik klucza konfiguracji w nazwach plików.</span><span class="sxs-lookup"><span data-stu-id="1c66e-465">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="1c66e-466">Na przykład nazwa pliku `Logging__LogLevel__System` generuje klucz konfiguracji `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-466">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="1c66e-467">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-467">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="1c66e-468">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="1c66e-468">Memory Configuration Provider</span></span>

<span data-ttu-id="1c66e-469"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> używa kolekcji w pamięci jako par klucz-wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-469">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="1c66e-470">Aby uaktywnić konfigurację kolekcji w pamięci, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-470">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c66e-471">Dostawcę konfiguracji można zainicjować przy użyciu `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-471">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="1c66e-472">Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-472">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="1c66e-473">W poniższym przykładzie tworzony jest słownik konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-473">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="1c66e-474">Słownik jest używany z wywołaniem `AddInMemoryCollection`, aby zapewnić konfigurację:</span><span class="sxs-lookup"><span data-stu-id="1c66e-474">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="1c66e-475">GetValue</span><span class="sxs-lookup"><span data-stu-id="1c66e-475">GetValue</span></span>

<span data-ttu-id="1c66e-476">[ConfigurationBinder. GetValue \<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnia jedną wartość z konfiguracji z określonym kluczem i konwertuje ją na określony typ niekolekcje.</span><span class="sxs-lookup"><span data-stu-id="1c66e-476">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="1c66e-477">Przeciążenie akceptuje wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="1c66e-477">An overload accepts a default value.</span></span>

<span data-ttu-id="1c66e-478">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="1c66e-478">The following example:</span></span>

* <span data-ttu-id="1c66e-479">Wyodrębnia wartość ciągu z konfiguracji z kluczem `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-479">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="1c66e-480">Jeśli nie można odnaleźć `NumberKey` w kluczach konfiguracji, zostanie użyta wartość domyślna `99`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-480">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="1c66e-481">Typ wartości jako `int`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-481">Types the value as an `int`.</span></span>
* <span data-ttu-id="1c66e-482">Przechowuje wartość we właściwości `NumberConfig` do użycia na stronie.</span><span class="sxs-lookup"><span data-stu-id="1c66e-482">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="1c66e-483">GetSection, GetChildren i EXISTS</span><span class="sxs-lookup"><span data-stu-id="1c66e-483">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="1c66e-484">W poniższych przykładach należy wziąć pod uwagę następujący plik JSON.</span><span class="sxs-lookup"><span data-stu-id="1c66e-484">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="1c66e-485">Cztery klucze są dostępne w dwóch sekcjach, z których jedna zawiera parę podsekcji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-485">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="1c66e-486">Gdy plik jest odczytywany do konfiguracji, następujące unikatowe klucze hierarchiczne są tworzone w celu przechowywania wartości konfiguracyjnych:</span><span class="sxs-lookup"><span data-stu-id="1c66e-486">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="1c66e-487">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c66e-487">section0:key0</span></span>
* <span data-ttu-id="1c66e-488">section0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="1c66e-488">section0:key1</span></span>
* <span data-ttu-id="1c66e-489">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1c66e-489">section1:key0</span></span>
* <span data-ttu-id="1c66e-490">Section1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="1c66e-490">section1:key1</span></span>
* <span data-ttu-id="1c66e-491">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="1c66e-491">section2:subsection0:key0</span></span>
* <span data-ttu-id="1c66e-492">section2: subsection0: Klucz1</span><span class="sxs-lookup"><span data-stu-id="1c66e-492">section2:subsection0:key1</span></span>
* <span data-ttu-id="1c66e-493">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="1c66e-493">section2:subsection1:key0</span></span>
* <span data-ttu-id="1c66e-494">section2: subsection1: Klucz1</span><span class="sxs-lookup"><span data-stu-id="1c66e-494">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="1c66e-495">GetSection</span><span class="sxs-lookup"><span data-stu-id="1c66e-495">GetSection</span></span>

<span data-ttu-id="1c66e-496">[IConfiguration. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcję konfiguracji z określonym kluczem podsekcji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-496">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="1c66e-497">Aby zwrócić <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz-wartość w `section1`, wywołaj `GetSection` i podaj nazwę sekcji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-497">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="1c66e-498">`configSection` nie ma wartości, tylko klucza i ścieżki.</span><span class="sxs-lookup"><span data-stu-id="1c66e-498">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="1c66e-499">Podobnie, aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę sekcji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-499">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="1c66e-500">`GetSection` nigdy nie zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-500">`GetSection` never returns `null`.</span></span> <span data-ttu-id="1c66e-501">Jeśli nie znaleziono pasującej sekcji, zwracany jest pusty `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-501">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="1c66e-502">Gdy `GetSection` zwraca pasującą sekcję, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione.</span><span class="sxs-lookup"><span data-stu-id="1c66e-502">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="1c66e-503"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> i <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> są zwracane, gdy istnieje sekcja.</span><span class="sxs-lookup"><span data-stu-id="1c66e-503">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="1c66e-504">GetChildren —</span><span class="sxs-lookup"><span data-stu-id="1c66e-504">GetChildren</span></span>

<span data-ttu-id="1c66e-505">Wywołanie [iConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) w `section2` uzyskuje `IEnumerable<IConfigurationSection>` obejmujący:</span><span class="sxs-lookup"><span data-stu-id="1c66e-505">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="1c66e-506">Istniejący</span><span class="sxs-lookup"><span data-stu-id="1c66e-506">Exists</span></span>

<span data-ttu-id="1c66e-507">Użyj [ConfigurationExtensions. istnieje](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) , aby określić, czy istnieje sekcja konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-507">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="1c66e-508">Dane przykładowe `sectionExists` jest `false`, ponieważ w danych konfiguracyjnych nie ma `section2:subsection2` sekcji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-508">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="1c66e-509">Powiąż z klasą</span><span class="sxs-lookup"><span data-stu-id="1c66e-509">Bind to a class</span></span>

<span data-ttu-id="1c66e-510">Konfigurację można powiązać z klasami, które reprezentują grupy powiązanych ustawień przy użyciu *wzorca opcji*.</span><span class="sxs-lookup"><span data-stu-id="1c66e-510">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="1c66e-511">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-511">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="1c66e-512">Wartości konfiguracji są zwracane jako ciągi, ale wywołanie <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie obiektów [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) .</span><span class="sxs-lookup"><span data-stu-id="1c66e-512">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="1c66e-513">Przykładowa aplikacja zawiera model `Starship` (*modele/Starship. cs*):</span><span class="sxs-lookup"><span data-stu-id="1c66e-513">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1c66e-514">Sekcja `starship` pliku *Starship. JSON* tworzy konfigurację, gdy aplikacja Przykładowa używa dostawcy konfiguracji JSON do załadowania konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-514">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="1c66e-515">Tworzone są następujące pary klucz-wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-515">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="1c66e-516">Key</span><span class="sxs-lookup"><span data-stu-id="1c66e-516">Key</span></span>                   | <span data-ttu-id="1c66e-517">Wartość</span><span class="sxs-lookup"><span data-stu-id="1c66e-517">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="1c66e-518">Starship: Nazwa</span><span class="sxs-lookup"><span data-stu-id="1c66e-518">starship:name</span></span>         | <span data-ttu-id="1c66e-519">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="1c66e-519">USS Enterprise</span></span>                                    |
| <span data-ttu-id="1c66e-520">Starship: Rejestr</span><span class="sxs-lookup"><span data-stu-id="1c66e-520">starship:registry</span></span>     | <span data-ttu-id="1c66e-521">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="1c66e-521">NCC-1701</span></span>                                          |
| <span data-ttu-id="1c66e-522">Starship: Klasa</span><span class="sxs-lookup"><span data-stu-id="1c66e-522">starship:class</span></span>        | <span data-ttu-id="1c66e-523">Skład</span><span class="sxs-lookup"><span data-stu-id="1c66e-523">Constitution</span></span>                                      |
| <span data-ttu-id="1c66e-524">Starship: Długość</span><span class="sxs-lookup"><span data-stu-id="1c66e-524">starship:length</span></span>       | <span data-ttu-id="1c66e-525">304,8</span><span class="sxs-lookup"><span data-stu-id="1c66e-525">304.8</span></span>                                             |
| <span data-ttu-id="1c66e-526">Starship: prowizja</span><span class="sxs-lookup"><span data-stu-id="1c66e-526">starship:commissioned</span></span> | <span data-ttu-id="1c66e-527">False</span><span class="sxs-lookup"><span data-stu-id="1c66e-527">False</span></span>                                             |
| <span data-ttu-id="1c66e-528">handlowych</span><span class="sxs-lookup"><span data-stu-id="1c66e-528">trademark</span></span>             | <span data-ttu-id="1c66e-529">Najważniejsze obrazy Corp.  https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="1c66e-529">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="1c66e-530">Przykładowa aplikacja wywołuje `GetSection` z kluczem `starship`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-530">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="1c66e-531">Pary klucz-wartość `starship` są odizolowane.</span><span class="sxs-lookup"><span data-stu-id="1c66e-531">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="1c66e-532">Metoda `Bind` jest wywoływana w podsekcji przekazującej w wystąpieniu klasy `Starship`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-532">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="1c66e-533">Po powiązaniu wartości wystąpień wystąpienie jest przypisywane do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="1c66e-533">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="1c66e-534">Powiąż z grafem obiektów</span><span class="sxs-lookup"><span data-stu-id="1c66e-534">Bind to an object graph</span></span>

<span data-ttu-id="1c66e-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> jest w stanie powiązać cały Graf obiektów POCO.</span><span class="sxs-lookup"><span data-stu-id="1c66e-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="1c66e-536">Przykład zawiera model `TvShow`, którego wykres obiektu zawiera klasy `Metadata` i `Actors` (*modele/TvShow. cs*):</span><span class="sxs-lookup"><span data-stu-id="1c66e-536">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1c66e-537">Przykładowa aplikacja zawiera plik *tvshow. XML* zawierający dane konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-537">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="1c66e-538">Konfiguracja jest powiązana z całym grafem obiektu `TvShow` za pomocą metody `Bind`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-538">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="1c66e-539">Powiązane wystąpienie jest przypisane do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="1c66e-539">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="1c66e-540">[ConfigurationBinder. Get \<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) tworzy powiązania i zwraca określony typ.</span><span class="sxs-lookup"><span data-stu-id="1c66e-540">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="1c66e-541">`Get<T>` jest wygodniejszy niż korzystanie z `Bind`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-541">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="1c66e-542">Poniższy kod pokazuje, jak używać `Get<T>` w poprzednim przykładzie, co umożliwia bezpośrednie przypisanie wystąpienia powiązanego do właściwości używanej do renderowania:</span><span class="sxs-lookup"><span data-stu-id="1c66e-542">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="1c66e-543">Powiąż tablicę z klasą</span><span class="sxs-lookup"><span data-stu-id="1c66e-543">Bind an array to a class</span></span>

<span data-ttu-id="1c66e-544">*Przykładowa aplikacja pokazuje Koncepcje opisane w tej sekcji.*</span><span class="sxs-lookup"><span data-stu-id="1c66e-544">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="1c66e-545"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-545">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1c66e-546">Każdy format tablicy, który ujawnia segment klucza numerycznego (`:0:`, `:1:`, &hellip; `:{n}:`), jest w stanie powiązać powiązanie tablicową z tablicą klas POCO.</span><span class="sxs-lookup"><span data-stu-id="1c66e-546">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="1c66e-547">Powiązanie jest dostarczane według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-547">Binding is provided by convention.</span></span> <span data-ttu-id="1c66e-548">Niestandardowi dostawcy konfiguracji nie muszą implementować powiązania tablicy.</span><span class="sxs-lookup"><span data-stu-id="1c66e-548">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="1c66e-549">**Przetwarzanie tablicy w pamięci**</span><span class="sxs-lookup"><span data-stu-id="1c66e-549">**In-memory array processing**</span></span>

<span data-ttu-id="1c66e-550">Należy wziąć pod uwagę klucze konfiguracji i wartości podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="1c66e-550">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="1c66e-551">Key</span><span class="sxs-lookup"><span data-stu-id="1c66e-551">Key</span></span>             | <span data-ttu-id="1c66e-552">Wartość</span><span class="sxs-lookup"><span data-stu-id="1c66e-552">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="1c66e-553">Tablica: wpisy: 0</span><span class="sxs-lookup"><span data-stu-id="1c66e-553">array:entries:0</span></span> | <span data-ttu-id="1c66e-554">value0</span><span class="sxs-lookup"><span data-stu-id="1c66e-554">value0</span></span> |
| <span data-ttu-id="1c66e-555">Tablica: wpisy: 1</span><span class="sxs-lookup"><span data-stu-id="1c66e-555">array:entries:1</span></span> | <span data-ttu-id="1c66e-556">sekwencj</span><span class="sxs-lookup"><span data-stu-id="1c66e-556">value1</span></span> |
| <span data-ttu-id="1c66e-557">Tablica: wpisy: 2</span><span class="sxs-lookup"><span data-stu-id="1c66e-557">array:entries:2</span></span> | <span data-ttu-id="1c66e-558">wartość2</span><span class="sxs-lookup"><span data-stu-id="1c66e-558">value2</span></span> |
| <span data-ttu-id="1c66e-559">Tablica: wpisy: 4</span><span class="sxs-lookup"><span data-stu-id="1c66e-559">array:entries:4</span></span> | <span data-ttu-id="1c66e-560">value4</span><span class="sxs-lookup"><span data-stu-id="1c66e-560">value4</span></span> |
| <span data-ttu-id="1c66e-561">Tablica: wpisy: 5</span><span class="sxs-lookup"><span data-stu-id="1c66e-561">array:entries:5</span></span> | <span data-ttu-id="1c66e-562">value5</span><span class="sxs-lookup"><span data-stu-id="1c66e-562">value5</span></span> |

<span data-ttu-id="1c66e-563">Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:</span><span class="sxs-lookup"><span data-stu-id="1c66e-563">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="1c66e-564">Tablica pomija wartość indeksu &num;3.</span><span class="sxs-lookup"><span data-stu-id="1c66e-564">The array skips a value for index &num;3.</span></span> <span data-ttu-id="1c66e-565">Segregator konfiguracji nie może powiązać wartości null ani tworzyć wpisów o wartości null w obiektach powiązanych, co oznacza, że w chwili pojawi się wynik powiązania tej tablicy z obiektem.</span><span class="sxs-lookup"><span data-stu-id="1c66e-565">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="1c66e-566">W przykładowej aplikacji jest dostępna Klasa POCO, która przechowuje powiązane dane konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-566">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1c66e-567">Dane konfiguracji są powiązane z obiektem:</span><span class="sxs-lookup"><span data-stu-id="1c66e-567">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="1c66e-568">[ConfigurationBinder. Get \<T](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) można również użyć składni >, co powoduje zwiększenie kodu kompaktowego:</span><span class="sxs-lookup"><span data-stu-id="1c66e-568">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="1c66e-569">Obiekt powiązany, wystąpienie `ArrayExample`, otrzymuje dane tablicy z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-569">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="1c66e-570">Indeks `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1c66e-570">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="1c66e-571">Wartość `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1c66e-571">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="1c66e-572">0</span><span class="sxs-lookup"><span data-stu-id="1c66e-572">0</span></span>                            | <span data-ttu-id="1c66e-573">value0</span><span class="sxs-lookup"><span data-stu-id="1c66e-573">value0</span></span>                       |
| <span data-ttu-id="1c66e-574">1</span><span class="sxs-lookup"><span data-stu-id="1c66e-574">1</span></span>                            | <span data-ttu-id="1c66e-575">sekwencj</span><span class="sxs-lookup"><span data-stu-id="1c66e-575">value1</span></span>                       |
| <span data-ttu-id="1c66e-576">2</span><span class="sxs-lookup"><span data-stu-id="1c66e-576">2</span></span>                            | <span data-ttu-id="1c66e-577">wartość2</span><span class="sxs-lookup"><span data-stu-id="1c66e-577">value2</span></span>                       |
| <span data-ttu-id="1c66e-578">3</span><span class="sxs-lookup"><span data-stu-id="1c66e-578">3</span></span>                            | <span data-ttu-id="1c66e-579">value4</span><span class="sxs-lookup"><span data-stu-id="1c66e-579">value4</span></span>                       |
| <span data-ttu-id="1c66e-580">4</span><span class="sxs-lookup"><span data-stu-id="1c66e-580">4</span></span>                            | <span data-ttu-id="1c66e-581">value5</span><span class="sxs-lookup"><span data-stu-id="1c66e-581">value5</span></span>                       |

<span data-ttu-id="1c66e-582">Indeks &num;3 w obiekcie powiązanym zawiera dane konfiguracyjne `array:4` klucza konfiguracji i jego wartość `value4`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-582">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="1c66e-583">Gdy dane konfiguracji zawierające tablicę są powiązane, indeksy tablic w kluczach konfiguracji są używane tylko do iteracji danych konfiguracji podczas tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="1c66e-583">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="1c66e-584">Wartości null nie można zachować w danych konfiguracyjnych, a wpis o wartości null nie jest tworzony w obiekcie powiązanym, gdy tablica w kluczach konfiguracji pomija jeden lub więcej indeksów.</span><span class="sxs-lookup"><span data-stu-id="1c66e-584">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="1c66e-585">Brakujący element konfiguracji dla indeksu &num;3 można dostarczyć przed powiązaniem do wystąpienia `ArrayExample` przez dowolnego dostawcę konfiguracji, który generuje poprawną parę klucz-wartość w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-585">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="1c66e-586">Jeśli przykład zawiera dodatkowego dostawcę konfiguracji JSON z brakującą parą klucz-wartość, `ArrayExample.Entries` pasuje do kompletnej tablicy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-586">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="1c66e-587">*missing_value. JSON*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-587">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="1c66e-588">W `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="1c66e-588">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="1c66e-589">Para klucz-wartość pokazana w tabeli jest ładowana do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-589">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="1c66e-590">Key</span><span class="sxs-lookup"><span data-stu-id="1c66e-590">Key</span></span>             | <span data-ttu-id="1c66e-591">Wartość</span><span class="sxs-lookup"><span data-stu-id="1c66e-591">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="1c66e-592">Tablica: wpisy: 3</span><span class="sxs-lookup"><span data-stu-id="1c66e-592">array:entries:3</span></span> | <span data-ttu-id="1c66e-593">Wartość3</span><span class="sxs-lookup"><span data-stu-id="1c66e-593">value3</span></span> |

<span data-ttu-id="1c66e-594">Jeśli wystąpienie klasy `ArrayExample` jest powiązane, gdy dostawca konfiguracji JSON zawiera wpis dla &num;3 indeksu, tablica `ArrayExample.Entries` zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="1c66e-594">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="1c66e-595">Indeks `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1c66e-595">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="1c66e-596">Wartość `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1c66e-596">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="1c66e-597">0</span><span class="sxs-lookup"><span data-stu-id="1c66e-597">0</span></span>                            | <span data-ttu-id="1c66e-598">value0</span><span class="sxs-lookup"><span data-stu-id="1c66e-598">value0</span></span>                       |
| <span data-ttu-id="1c66e-599">1</span><span class="sxs-lookup"><span data-stu-id="1c66e-599">1</span></span>                            | <span data-ttu-id="1c66e-600">sekwencj</span><span class="sxs-lookup"><span data-stu-id="1c66e-600">value1</span></span>                       |
| <span data-ttu-id="1c66e-601">2</span><span class="sxs-lookup"><span data-stu-id="1c66e-601">2</span></span>                            | <span data-ttu-id="1c66e-602">wartość2</span><span class="sxs-lookup"><span data-stu-id="1c66e-602">value2</span></span>                       |
| <span data-ttu-id="1c66e-603">3</span><span class="sxs-lookup"><span data-stu-id="1c66e-603">3</span></span>                            | <span data-ttu-id="1c66e-604">Wartość3</span><span class="sxs-lookup"><span data-stu-id="1c66e-604">value3</span></span>                       |
| <span data-ttu-id="1c66e-605">4</span><span class="sxs-lookup"><span data-stu-id="1c66e-605">4</span></span>                            | <span data-ttu-id="1c66e-606">value4</span><span class="sxs-lookup"><span data-stu-id="1c66e-606">value4</span></span>                       |
| <span data-ttu-id="1c66e-607">5</span><span class="sxs-lookup"><span data-stu-id="1c66e-607">5</span></span>                            | <span data-ttu-id="1c66e-608">value5</span><span class="sxs-lookup"><span data-stu-id="1c66e-608">value5</span></span>                       |

<span data-ttu-id="1c66e-609">**Przetwarzanie tablicy JSON**</span><span class="sxs-lookup"><span data-stu-id="1c66e-609">**JSON array processing**</span></span>

<span data-ttu-id="1c66e-610">Jeśli plik JSON zawiera tablicę, klucze konfiguracji są tworzone dla elementów tablicy z indeksem sekcji o wartości zero.</span><span class="sxs-lookup"><span data-stu-id="1c66e-610">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="1c66e-611">W poniższym pliku konfiguracyjnym `subsection` jest tablicą:</span><span class="sxs-lookup"><span data-stu-id="1c66e-611">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="1c66e-612">Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz-wartość:</span><span class="sxs-lookup"><span data-stu-id="1c66e-612">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="1c66e-613">Key</span><span class="sxs-lookup"><span data-stu-id="1c66e-613">Key</span></span>                     | <span data-ttu-id="1c66e-614">Wartość</span><span class="sxs-lookup"><span data-stu-id="1c66e-614">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="1c66e-615">json_array: klucz</span><span class="sxs-lookup"><span data-stu-id="1c66e-615">json_array:key</span></span>          | <span data-ttu-id="1c66e-616">wartośća</span><span class="sxs-lookup"><span data-stu-id="1c66e-616">valueA</span></span> |
| <span data-ttu-id="1c66e-617">json_array: podsekcja: 0</span><span class="sxs-lookup"><span data-stu-id="1c66e-617">json_array:subsection:0</span></span> | <span data-ttu-id="1c66e-618">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="1c66e-618">valueB</span></span> |
| <span data-ttu-id="1c66e-619">json_array: podsekcja: 1</span><span class="sxs-lookup"><span data-stu-id="1c66e-619">json_array:subsection:1</span></span> | <span data-ttu-id="1c66e-620">valueC</span><span class="sxs-lookup"><span data-stu-id="1c66e-620">valueC</span></span> |
| <span data-ttu-id="1c66e-621">json_array: podsekcja: 2</span><span class="sxs-lookup"><span data-stu-id="1c66e-621">json_array:subsection:2</span></span> | <span data-ttu-id="1c66e-622">Znajdując</span><span class="sxs-lookup"><span data-stu-id="1c66e-622">valueD</span></span> |

<span data-ttu-id="1c66e-623">W przykładowej aplikacji jest dostępna następująca Klasa POCO z powiązaniem par klucz-wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1c66e-623">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1c66e-624">Po powiązaniu `JsonArrayExample.Key` utrzymuje `valueA` wartości.</span><span class="sxs-lookup"><span data-stu-id="1c66e-624">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="1c66e-625">Wartości podsekcji są przechowywane we właściwości tablicy POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-625">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="1c66e-626">Indeks `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="1c66e-626">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="1c66e-627">Wartość `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="1c66e-627">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="1c66e-628">0</span><span class="sxs-lookup"><span data-stu-id="1c66e-628">0</span></span>                                   | <span data-ttu-id="1c66e-629">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="1c66e-629">valueB</span></span>                              |
| <span data-ttu-id="1c66e-630">1</span><span class="sxs-lookup"><span data-stu-id="1c66e-630">1</span></span>                                   | <span data-ttu-id="1c66e-631">valueC</span><span class="sxs-lookup"><span data-stu-id="1c66e-631">valueC</span></span>                              |
| <span data-ttu-id="1c66e-632">2</span><span class="sxs-lookup"><span data-stu-id="1c66e-632">2</span></span>                                   | <span data-ttu-id="1c66e-633">Znajdując</span><span class="sxs-lookup"><span data-stu-id="1c66e-633">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="1c66e-634">Niestandardowy dostawca konfiguracji</span><span class="sxs-lookup"><span data-stu-id="1c66e-634">Custom configuration provider</span></span>

<span data-ttu-id="1c66e-635">Przykładowa aplikacja pokazuje, jak utworzyć podstawowego dostawcę konfiguracji, który odczytuje pary klucz-wartość konfiguracji z bazy danych przy użyciu [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="1c66e-635">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="1c66e-636">Dostawca ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="1c66e-636">The provider has the following characteristics:</span></span>

* <span data-ttu-id="1c66e-637">Baza danych EF w pamięci jest używana w celach demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="1c66e-637">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="1c66e-638">Aby użyć bazy danych, która wymaga parametrów połączenia, zaimplementuj pomocniczą `ConfigurationBuilder`, aby podać parametry połączenia od innego dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-638">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="1c66e-639">Dostawca odczytuje tabelę bazy danych w konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1c66e-639">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="1c66e-640">Dostawca nie wykonuje zapytania do bazy danych w oparciu o klucz.</span><span class="sxs-lookup"><span data-stu-id="1c66e-640">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="1c66e-641">Ponowne załadowanie nie zostało zaimplementowane, więc aktualizacja bazy danych po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-641">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="1c66e-642">Zdefiniuj jednostkę `EFConfigurationValue` do przechowywania wartości konfiguracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1c66e-642">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c66e-643">*Modele/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-643">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="1c66e-644">Dodaj `EFConfigurationContext` do przechowywania skonfigurowanych wartości i uzyskiwania do nich dostępu.</span><span class="sxs-lookup"><span data-stu-id="1c66e-644">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="1c66e-645">*EFConfigurationProvider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-645">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="1c66e-646">Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-646">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="1c66e-647">*EFConfigurationProvider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-647">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="1c66e-648">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-648">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="1c66e-649">Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta.</span><span class="sxs-lookup"><span data-stu-id="1c66e-649">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="1c66e-650">Ponieważ w [kluczach konfiguracji jest rozróżniana wielkość liter](#keys), słownik używany do inicjowania bazy danych jest tworzony przy użyciu funkcji porównującej bez uwzględniania wielkości liter ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="1c66e-650">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="1c66e-651">*EFConfigurationProvider/EFConfigurationProvider. cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-651">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="1c66e-652">Metoda rozszerzenia `AddEFConfiguration` zezwala na Dodawanie źródła konfiguracji do `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-652">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="1c66e-653">*Rozszerzenia/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-653">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="1c66e-654">Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-654">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c66e-655">*Modele/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-655">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="1c66e-656">Dodaj `EFConfigurationContext` do przechowywania skonfigurowanych wartości i uzyskiwania do nich dostępu.</span><span class="sxs-lookup"><span data-stu-id="1c66e-656">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="1c66e-657">*EFConfigurationProvider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-657">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="1c66e-658">Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-658">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="1c66e-659">*EFConfigurationProvider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-659">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="1c66e-660">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-660">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="1c66e-661">Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta.</span><span class="sxs-lookup"><span data-stu-id="1c66e-661">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="1c66e-662">*EFConfigurationProvider/EFConfigurationProvider. cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-662">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="1c66e-663">Metoda rozszerzenia `AddEFConfiguration` zezwala na Dodawanie źródła konfiguracji do `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-663">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="1c66e-664">*Rozszerzenia/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-664">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="1c66e-665">Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c66e-665">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="1c66e-666">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="1c66e-666">Access configuration during startup</span></span>

<span data-ttu-id="1c66e-667">Wsuń `IConfiguration` do konstruktora `Startup`, aby uzyskać dostęp do wartości konfiguracyjnych w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1c66e-667">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1c66e-668">Aby uzyskać dostęp do konfiguracji w `Startup.Configure`, należy wstrzyknąć `IConfiguration` bezpośrednio do metody lub użyć wystąpienia z konstruktora:</span><span class="sxs-lookup"><span data-stu-id="1c66e-668">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="1c66e-669">Aby zapoznać się z przykładem uzyskiwania dostępu do konfiguracji przy użyciu metod uruchamiania, zobacz [Uruchamianie aplikacji: wygodne metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="1c66e-669">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="1c66e-670">Konfiguracja dostępu na stronie Razor Pages lub widoku MVC</span><span class="sxs-lookup"><span data-stu-id="1c66e-670">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="1c66e-671">Aby uzyskać dostęp do ustawień konfiguracji na stronie Razor Pages lub widoku MVC, Dodaj [dyrektywę using](xref:mvc/views/razor#using) ([ C# odwołanie: Using](/dotnet/csharp/language-reference/keywords/using-directive)) dla [przestrzeni nazw Microsoft. Extensions. Configuration](xref:Microsoft.Extensions.Configuration) i wsuń <xref:Microsoft.Extensions.Configuration.IConfiguration> do strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="1c66e-671">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="1c66e-672">Na stronie Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="1c66e-672">In a Razor Pages page:</span></span>

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

<span data-ttu-id="1c66e-673">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="1c66e-673">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="1c66e-674">Dodawanie konfiguracji z zestawu zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="1c66e-674">Add configuration from an external assembly</span></span>

<span data-ttu-id="1c66e-675">Implementacja <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zestawu zewnętrznego poza klasą `Startup` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c66e-675">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="1c66e-676">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1c66e-676">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1c66e-677">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1c66e-677">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
