---
title: Konfiguracja w programie ASP.NET Core
author: guardrex
description: 'Dowiedz się, jak skonfigurować aplikację ASP.NET Core za pomocą interfejsu API konfiguracji.'
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2019
uid: fundamentals/configuration/index
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="c93dc-103">Konfiguracja w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c93dc-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="c93dc-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c93dc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c93dc-105">Konfiguracja aplikacji w programie ASP.NET Core opiera się na parach klucz wartość ustanowione przez *dostawcy konfiguracji*.</span><span class="sxs-lookup"><span data-stu-id="c93dc-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="c93dc-106">Dostawcy konfiguracji odczytania danych konfiguracyjnych do pary klucz wartość z wielu źródeł w konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="c93dc-107">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c93dc-107">Azure Key Vault</span></span>
* <span data-ttu-id="c93dc-108">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c93dc-108">Command-line arguments</span></span>
* <span data-ttu-id="c93dc-109">Niestandardowi dostawcy (zainstalowane lub utworzone)</span><span class="sxs-lookup"><span data-stu-id="c93dc-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="c93dc-110">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="c93dc-110">Directory files</span></span>
* <span data-ttu-id="c93dc-111">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c93dc-111">Environment variables</span></span>
* <span data-ttu-id="c93dc-112">Obiekty platformy .NET w pamięci</span><span class="sxs-lookup"><span data-stu-id="c93dc-112">In-memory .NET objects</span></span>
* <span data-ttu-id="c93dc-113">Pliki ustawień</span><span class="sxs-lookup"><span data-stu-id="c93dc-113">Settings files</span></span>

<span data-ttu-id="c93dc-114">*Wzorzec opcje* jest rozszerzeniem pojęć związanych z konfiguracją opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="c93dc-114">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="c93dc-115">Opcje używa klas do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="c93dc-115">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="c93dc-116">Aby uzyskać więcej informacji na temat korzystania z wzorca opcji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-116">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="c93dc-117">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c93dc-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c93dc-118">Te trzy pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-118">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="host-vs-app-configuration"></a><span data-ttu-id="c93dc-119">Hostowanie i konfiguracji aplikacji</span><span class="sxs-lookup"><span data-stu-id="c93dc-119">Host vs. app configuration</span></span>

<span data-ttu-id="c93dc-120">Zanim aplikacja jest skonfigurowana i uruchomiona, *hosta* skonfigurowany i uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="c93dc-120">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="c93dc-121">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-121">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="c93dc-122">Zarówno aplikacja, jak i hosta są skonfigurowane przy użyciu dostawcy konfiguracji opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="c93dc-122">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="c93dc-123">Pary klucz wartość konfiguracji hosta stają się częścią globalnej konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-123">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="c93dc-124">Aby uzyskać więcej informacji na temat sposobu konfiguracji dostawcy są używane, gdy host jest wbudowana i wpływ źródła konfiguracji hosta konfiguracji, zobacz [hosta](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="c93dc-124">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="c93dc-125">Konfiguracja domyślna</span><span class="sxs-lookup"><span data-stu-id="c93dc-125">Default configuration</span></span>

<span data-ttu-id="c93dc-126">Aplikacje oparte na platformy ASP.NET Core sieci Web [dotnet nowe](/dotnet/core/tools/dotnet-new) wywołania szablonów <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> podczas tworzenia na hoście.</span><span class="sxs-lookup"><span data-stu-id="c93dc-126">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="c93dc-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> udostępnia domyślną konfigurację aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="c93dc-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="c93dc-128">Konfiguracja hosta jest dostarczana z:</span><span class="sxs-lookup"><span data-stu-id="c93dc-128">Host configuration is provided from:</span></span>
  * <span data-ttu-id="c93dc-129">Zmienne środowiskowe prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`) przy użyciu [dostawcę konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c93dc-129">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="c93dc-130">Prefiks (`ASPNETCORE_`) jest usuwany po załadowaniu pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-130">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="c93dc-131">Argumenty wiersza polecenia przy użyciu [dostawcę konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c93dc-131">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="c93dc-132">Konfiguracja aplikacji jest dostarczana z:</span><span class="sxs-lookup"><span data-stu-id="c93dc-132">App configuration is provided from:</span></span>
  * <span data-ttu-id="c93dc-133">*appSettings.JSON* przy użyciu [dostawcę konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c93dc-133">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="c93dc-134">*appSettings. {Środowiska} .json* przy użyciu [dostawcę konfiguracji pliku](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c93dc-134">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="c93dc-135">[Klucz tajny Menedżera](xref:security/app-secrets) uruchamiania aplikacji `Development` środowisko przy użyciu zestawu wpisu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="c93dc-136">Zmienne środowiskowe za pomocą [dostawcę konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c93dc-136">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="c93dc-137">Jeśli jest używany prefiks niestandardowy (na przykład `PREFIX_` z `.AddEnvironmentVariables(prefix: "PREFIX_")`), prefiks jest usuwany po załadowaniu pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-137">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="c93dc-138">Argumenty wiersza polecenia przy użyciu [dostawcę konfiguracji wiersza polecenia](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="c93dc-138">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="c93dc-139">Dostawcy konfiguracji zostały omówione w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-139">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="c93dc-140">Aby uzyskać więcej informacji na hoście i <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, zobacz <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-140">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="c93dc-141">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="c93dc-141">Security</span></span>

<span data-ttu-id="c93dc-142">Przyjmuje następujące najlepsze rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="c93dc-142">Adopt the following best practices:</span></span>

* <span data-ttu-id="c93dc-143">Nigdy nie przechowują hasła lub innych danych poufnych w kodzie dostawcy konfiguracji lub w plikach konfiguracyjnych w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-143">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="c93dc-144">Nie używać kluczy tajnych w środowisku produkcyjnym w trakcie opracowywania lub środowisk testowych.</span><span class="sxs-lookup"><span data-stu-id="c93dc-144">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="c93dc-145">Określ klucze tajne poza projektem, nie może być przypadkowo wszelkich starań, by repozytorium kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="c93dc-145">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="c93dc-146">Dowiedz się więcej o [jak używać wielu środowisk](xref:fundamentals/environments) i zarządzanie nimi [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie programowania przy użyciu klucza tajnego Menedżera](xref:security/app-secrets) (zawiera wskazówki dotyczące używania zmiennych środowiskowych do przechowywania dane poufne).</span><span class="sxs-lookup"><span data-stu-id="c93dc-146">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="c93dc-147">Menedżer klucz tajny używa dostawcy konfiguracji plików do przechowywania kluczy tajnych użytkownika w pliku JSON w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="c93dc-147">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="c93dc-148">Dostawca konfiguracji pliku jest opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-148">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="c93dc-149">[Usługa Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stanowi jedną z opcji dla bezpieczne przechowywanie kluczy tajnych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="c93dc-150">Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-150">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="c93dc-151">Dane hierarchiczne konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c93dc-151">Hierarchical configuration data</span></span>

<span data-ttu-id="c93dc-152">Interfejs API konfiguracji jest zdolny do utrzymywania dane hierarchiczne konfiguracji spłaszczając dane hierarchiczne przy użyciu ogranicznika w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-152">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="c93dc-153">W następującym pliku JSON cztery klucze, istnieją w ze strukturą hierarchii dwie sekcje:</span><span class="sxs-lookup"><span data-stu-id="c93dc-153">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="c93dc-154">Gdy plik jest do odczytu do konfiguracji, unikatowe klucze są tworzone do obsługi struktury danych hierarchicznych oryginalnego źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-154">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="c93dc-155">Sekcji i kluczy są spłaszczone przy użyciu dwukropek (`:`) do pierwotnej struktury obsługi:</span><span class="sxs-lookup"><span data-stu-id="c93dc-155">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="c93dc-156">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c93dc-156">section0:key0</span></span>
* <span data-ttu-id="c93dc-157">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c93dc-157">section0:key1</span></span>
* <span data-ttu-id="c93dc-158">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c93dc-158">section1:key0</span></span>
* <span data-ttu-id="c93dc-159">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c93dc-159">section1:key1</span></span>

<span data-ttu-id="c93dc-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> metody są dostępne do izolowania sekcje i elementy podrzędne do sekcji w danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="c93dc-161">Te metody są opisane w dalszej części w [GetSection GetChildren i Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="c93dc-161">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="c93dc-162">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-162">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="c93dc-163">Konwencje</span><span class="sxs-lookup"><span data-stu-id="c93dc-163">Conventions</span></span>

<span data-ttu-id="c93dc-164">Przy uruchamianiu aplikacji źródła konfiguracji są do odczytu w kolejności, że podano ich dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-164">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="c93dc-165">Dostawcy konfiguracji pliku mają możliwość Załaduj ponownie konfigurację, gdy podstawowy plik ustawień zostanie zmieniona po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-165">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="c93dc-166">Dostawca konfiguracji pliku jest opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-166">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="c93dc-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> jest dostępna w aplikacji [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="c93dc-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="c93dc-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> być dodane do stron Razor <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> można uzyskać konfiguracji dla klasy:</span><span class="sxs-lookup"><span data-stu-id="c93dc-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

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

<span data-ttu-id="c93dc-169">Dostawcy konfiguracji nie może wykorzystywać DI, ponieważ nie jest dostępna podczas konfigurowania one przez hosta.</span><span class="sxs-lookup"><span data-stu-id="c93dc-169">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="c93dc-170">Klucze konfiguracji przyjęcie następujących konwencji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-170">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="c93dc-171">Kluczy jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="c93dc-171">Keys are case-insensitive.</span></span> <span data-ttu-id="c93dc-172">Na przykład `ConnectionString` i `connectionstring` są traktowane jako równoważne kluczy.</span><span class="sxs-lookup"><span data-stu-id="c93dc-172">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="c93dc-173">Jeśli ten sam klucz jest ustawiany przez dostawców o tej samej lub innej konfiguracji, ostatnia wartość klucza jest wartość.</span><span class="sxs-lookup"><span data-stu-id="c93dc-173">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="c93dc-174">Klucze hierarchicznych</span><span class="sxs-lookup"><span data-stu-id="c93dc-174">Hierarchical keys</span></span>
  * <span data-ttu-id="c93dc-175">W ramach konfiguracji interfejsu API, separator dwukropka (`:`) działa na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="c93dc-175">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="c93dc-176">W zmiennych środowiskowych separator dwukropek, może nie działać na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="c93dc-176">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="c93dc-177">Podwójnym podkreśleniem (`__`) jest obsługiwana przez wszystkie platformy i jest konwertowany na dwukropka.</span><span class="sxs-lookup"><span data-stu-id="c93dc-177">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="c93dc-178">W usłudze Azure Key Vault, klucze hierarchiczne, użyj `--` (dwa łączniki) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="c93dc-178">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="c93dc-179">Należy podać kod, aby zastąpić kresek dwukropka po wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-179">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="c93dc-180"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Obsługuje tablice powiązania do obiektów przy użyciu tablicy indeksów w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-180">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="c93dc-181">Powiązanie tablicy jest opisana w [tablicy należy powiązać klasę](#bind-an-array-to-a-class) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-181">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="c93dc-182">Wartości konfiguracji przyjęcie następujących konwencji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-182">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="c93dc-183">Wartości są ciągami.</span><span class="sxs-lookup"><span data-stu-id="c93dc-183">Values are strings.</span></span>
* <span data-ttu-id="c93dc-184">Wartości null nie przechowywanych w konfiguracji lub powiązane z obiektami.</span><span class="sxs-lookup"><span data-stu-id="c93dc-184">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="c93dc-185">dostawcy</span><span class="sxs-lookup"><span data-stu-id="c93dc-185">Providers</span></span>

<span data-ttu-id="c93dc-186">W poniższej tabeli przedstawiono dostępne dla aplikacji platformy ASP.NET Core dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-186">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="c93dc-187">Dostawca</span><span class="sxs-lookup"><span data-stu-id="c93dc-187">Provider</span></span> | <span data-ttu-id="c93dc-188">Udostępnia konfigurację z&hellip;</span><span class="sxs-lookup"><span data-stu-id="c93dc-188">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="c93dc-189">[Dostawca konfiguracji magazynu w usłudze Azure klucza](xref:security/key-vault-configuration) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="c93dc-189">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="c93dc-190">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c93dc-190">Azure Key Vault</span></span> |
| [<span data-ttu-id="c93dc-191">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c93dc-191">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="c93dc-192">Parametry wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c93dc-192">Command-line parameters</span></span> |
| [<span data-ttu-id="c93dc-193">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="c93dc-193">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="c93dc-194">Źródło niestandardowe</span><span class="sxs-lookup"><span data-stu-id="c93dc-194">Custom source</span></span> |
| [<span data-ttu-id="c93dc-195">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="c93dc-195">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="c93dc-196">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c93dc-196">Environment variables</span></span> |
| [<span data-ttu-id="c93dc-197">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="c93dc-197">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="c93dc-198">Pliki INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="c93dc-198">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="c93dc-199">Dostawca klucza każdego pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c93dc-199">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="c93dc-200">Pliki katalogu</span><span class="sxs-lookup"><span data-stu-id="c93dc-200">Directory files</span></span> |
| [<span data-ttu-id="c93dc-201">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="c93dc-201">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="c93dc-202">Kolekcje w pamięci</span><span class="sxs-lookup"><span data-stu-id="c93dc-202">In-memory collections</span></span> |
| <span data-ttu-id="c93dc-203">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (*zabezpieczeń* tematy)</span><span class="sxs-lookup"><span data-stu-id="c93dc-203">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="c93dc-204">Plik w katalogu profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="c93dc-204">File in the user profile directory</span></span> |

<span data-ttu-id="c93dc-205">Źródła konfiguracji są do odczytu w kolejności określono ich dostawców konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="c93dc-205">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="c93dc-206">Dostawcy konfiguracji opisanych w tym temacie są opisane w kolejności alfabetycznej, a nie w kolejności, Twój kod może zorganizować ich.</span><span class="sxs-lookup"><span data-stu-id="c93dc-206">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="c93dc-207">Kolejność dostawców konfiguracji w kodzie do własnych priorytetów do bazowego źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-207">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="c93dc-208">Jest zwykle kolejność dostawców konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-208">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="c93dc-209">Pliki (*appsettings.json*, *appsettings. { Środowisko} .json*, gdzie `{Environment}` jest bieżące środowisko hostingu aplikacji)</span><span class="sxs-lookup"><span data-stu-id="c93dc-209">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="c93dc-210">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c93dc-210">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="c93dc-211">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym tylko)</span><span class="sxs-lookup"><span data-stu-id="c93dc-211">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="c93dc-212">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="c93dc-212">Environment variables</span></span>
1. <span data-ttu-id="c93dc-213">Argumenty wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c93dc-213">Command-line arguments</span></span>

<span data-ttu-id="c93dc-214">Jest to powszechną praktyką do ostatniej pozycji dostawcę konfiguracji wiersza polecenia w serii dostawcy, aby zezwolić na argumenty wiersza polecenia zastąpić konfiguracji ustawione przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="c93dc-214">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="c93dc-215">Ta sekwencja dostawcy są umieszczane w miejscu, podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-215">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="c93dc-216">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c93dc-216">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="c93dc-217">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="c93dc-217">ConfigureAppConfiguration</span></span>

<span data-ttu-id="c93dc-218">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta do określenia aplikacji dostawcy konfiguracji oprócz tych dodawane automatycznie przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="c93dc-218">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="c93dc-219">Dostarczony do aplikacji w konfiguracji <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> jest dostępna podczas uruchamiania aplikacji, w tym `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-219">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c93dc-220">Aby uzyskać więcej informacji, zobacz [konfiguracji dostępu podczas uruchamiania](#access-configuration-during-startup) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-220">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="c93dc-221">Dostawca konfiguracji wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c93dc-221">Command-line Configuration Provider</span></span>

<span data-ttu-id="c93dc-222"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Ładuje konfiguracji z pary klucz wartość argumentu wiersza polecenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c93dc-222">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="c93dc-223">Aby aktywować wiersza polecenia konfiguracji, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metoda rozszerzająca zostanie wywołana w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-223">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c93dc-224">`AddCommandLine` jest wywoływana automatycznie podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-224">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="c93dc-225">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c93dc-225">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="c93dc-226">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c93dc-226">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c93dc-227">Konfiguracja opcjonalna z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="c93dc-227">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="c93dc-228">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="c93dc-228">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="c93dc-229">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="c93dc-229">Environment variables.</span></span>

<span data-ttu-id="c93dc-230">`CreateDefaultBuilder` dodaje ostatniego wiersza polecenia dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-230">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="c93dc-231">Argumenty wiersza polecenia przekazywane w czasie wykonywania zastąpić konfiguracji ustawione przez innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="c93dc-231">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="c93dc-232">`CreateDefaultBuilder` działa, gdy host jest konstruowany.</span><span class="sxs-lookup"><span data-stu-id="c93dc-232">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="c93dc-233">W związku z tym, wiersza polecenia konfiguracji aktywowany przez `CreateDefaultBuilder` mogą mieć wpływ na sposób host jest skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="c93dc-233">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="c93dc-234">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-234">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="c93dc-235">`AddCommandLine` zostały już wywołane `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-235">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c93dc-236">Jeśli zachodzi potrzeba zapewniają konfigurację aplikacji i nadal będzie można przesłonić tę konfigurację za pomocą argumentów wiersza polecenia, należy wywołać dodatkowych dostawców aplikacji <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> i wywołać `AddCommandLine` ostatni.</span><span class="sxs-lookup"><span data-stu-id="c93dc-236">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="c93dc-237">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c93dc-237">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c93dc-238">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="c93dc-238">**Example**</span></span>

<span data-ttu-id="c93dc-239">Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta, który zawiera wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-239">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="c93dc-240">Otwórz wiersz polecenia w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-240">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="c93dc-241">Argument wiersza polecenia, aby podać `dotnet run` polecenia `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-241">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="c93dc-242">Po uruchomieniu aplikacji otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-242">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c93dc-243">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla argumentu wiersza polecenia konfiguracji udostępnionego `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-243">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="c93dc-244">Argumenty</span><span class="sxs-lookup"><span data-stu-id="c93dc-244">Arguments</span></span>

<span data-ttu-id="c93dc-245">Wartość musi stosować się znak równości (`=`), lub klucz musi mieć prefiks (`--` lub `/`) Jeśli wartość jest zgodna spację.</span><span class="sxs-lookup"><span data-stu-id="c93dc-245">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="c93dc-246">Wartość może być null, jeśli jest używany znak równości (na przykład `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="c93dc-246">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="c93dc-247">Prefiks klucza</span><span class="sxs-lookup"><span data-stu-id="c93dc-247">Key prefix</span></span>               | <span data-ttu-id="c93dc-248">Przykład</span><span class="sxs-lookup"><span data-stu-id="c93dc-248">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="c93dc-249">Nie prefiksu</span><span class="sxs-lookup"><span data-stu-id="c93dc-249">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="c93dc-250">Dwa łączniki (`--`)</span><span class="sxs-lookup"><span data-stu-id="c93dc-250">Two dashes (`--`)</span></span>        | <span data-ttu-id="c93dc-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="c93dc-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="c93dc-252">Kreski ułamkowej (`/`)</span><span class="sxs-lookup"><span data-stu-id="c93dc-252">Forward slash (`/`)</span></span>      | <span data-ttu-id="c93dc-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="c93dc-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="c93dc-254">W tym samym poleceniu nie Mieszaj argument wiersza polecenia pary klucz wartość, które znakiem równości za pomocą pary klucz wartość, korzystających z przestrzeni.</span><span class="sxs-lookup"><span data-stu-id="c93dc-254">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="c93dc-255">Przykładowe polecenia:</span><span class="sxs-lookup"><span data-stu-id="c93dc-255">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="c93dc-256">Przełącz mapowania</span><span class="sxs-lookup"><span data-stu-id="c93dc-256">Switch mappings</span></span>

<span data-ttu-id="c93dc-257">Przełącznik jest transformowanie na nazwę klucza wymiany logiki.</span><span class="sxs-lookup"><span data-stu-id="c93dc-257">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="c93dc-258">Podczas ręcznego tworzenia konfiguracji za pomocą <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, może zapewnić słownika zastępstw przełącznika <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metody.</span><span class="sxs-lookup"><span data-stu-id="c93dc-258">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="c93dc-259">W przypadku przełącznika słownik mapowań słownika jest sprawdzane dla klucza, który pasuje do klucza, dostarczone przez argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c93dc-259">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="c93dc-260">Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) jest przekazywana do zestawu pary klucz wartość do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-260">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="c93dc-261">Mapowanie przełącznika jest wymagane dla dowolnego klucz wiersza polecenia poprzedzone kreską pojedynczego (`-`).</span><span class="sxs-lookup"><span data-stu-id="c93dc-261">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="c93dc-262">Przełącz mapowania słownika kluczowych reguł:</span><span class="sxs-lookup"><span data-stu-id="c93dc-262">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="c93dc-263">Przełączniki musi rozpoczynać się kreską (`-`) lub podwójne kreska (`--`).</span><span class="sxs-lookup"><span data-stu-id="c93dc-263">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="c93dc-264">Słownik mapowań przełącznik nie może zawierać zduplikowane klucze.</span><span class="sxs-lookup"><span data-stu-id="c93dc-264">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="c93dc-265">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-265">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c93dc-266">Jak pokazano w powyższym przykładzie wywołanie `CreateDefaultBuilder` nie należy przekazywać argumenty, gdy przełącznik mapowania są używane.</span><span class="sxs-lookup"><span data-stu-id="c93dc-266">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="c93dc-267">`CreateDefaultBuilder` metody `AddCommandLine` wywołania nie zawiera zamapowanego przełączników, a nie istnieje sposób do przekazania słownik mapowania przełącznika `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-267">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c93dc-268">Jeśli argumenty zamapowanego przełącznik dołączania i są przekazywane do `CreateDefaultBuilder`, jego `AddCommandLine` dostawcy nie uda się zainicjowanie z <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-268">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="c93dc-269">Rozwiązanie nie jest do przekazywania argumentów do `CreateDefaultBuilder` , ale zamiast tego umożliwiające `ConfigurationBuilder` metody `AddCommandLine` metody do przetwarzania argumentów i słownika mapowania przełącznika.</span><span class="sxs-lookup"><span data-stu-id="c93dc-269">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="c93dc-270">Po utworzeniu przełącznika słownik mapowań zawiera dane wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c93dc-270">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="c93dc-271">Key</span><span class="sxs-lookup"><span data-stu-id="c93dc-271">Key</span></span>       | <span data-ttu-id="c93dc-272">Wartość</span><span class="sxs-lookup"><span data-stu-id="c93dc-272">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="c93dc-273">Jeśli mapowane przełącznika klucze są używane podczas uruchamiania aplikacji, konfiguracji odbiera wartości konfiguracji na kluczu pochodzącego ze słownika:</span><span class="sxs-lookup"><span data-stu-id="c93dc-273">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="c93dc-274">Po uruchomieniu polecenia poprzedniej konfiguracji zawiera wartości podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c93dc-274">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="c93dc-275">Key</span><span class="sxs-lookup"><span data-stu-id="c93dc-275">Key</span></span>               | <span data-ttu-id="c93dc-276">Wartość</span><span class="sxs-lookup"><span data-stu-id="c93dc-276">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="c93dc-277">Dostawca konfiguracji zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="c93dc-277">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="c93dc-278"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Ładuje konfiguracji ze środowiska zmiennej pary klucz wartość w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c93dc-278">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="c93dc-279">Aby aktywować środowisko zmiennych konfiguracji, należy wywołać <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-279">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="c93dc-280">[Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) pozwala na Ustawianie zmiennych środowiskowych w portalu Azure, które mogą zastąpić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="c93dc-280">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="c93dc-281">Aby uzyskać więcej informacji, zobacz [aplikacje platformy Azure: Zastąpienie konfiguracji aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="c93dc-281">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="c93dc-282">`AddEnvironmentVariables` jest wywoływana automatycznie dla zmiennych środowiskowych prefiksem `ASPNETCORE_` podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-282">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="c93dc-283">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c93dc-283">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="c93dc-284">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c93dc-284">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c93dc-285">Konfiguracja aplikacji ze zmiennych środowiskowych unprefixed przez wywołanie metody `AddEnvironmentVariables` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-285">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="c93dc-286">Konfiguracja opcjonalna z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="c93dc-286">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="c93dc-287">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="c93dc-287">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="c93dc-288">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c93dc-288">Command-line arguments.</span></span>

<span data-ttu-id="c93dc-289">Dostawca konfiguracji zmiennych środowiskowych jest wywoływana po konfiguracji zostanie nawiązane z wpisami tajnymi użytkowników i *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="c93dc-289">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="c93dc-290">W tym miejscu podczas wywoływania dostawcy umożliwia zmienne środowiskowe są odczytywane w czasie wykonywania do zastępowania konfiguracji ustawione przez użytkownika hasła i *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="c93dc-290">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="c93dc-291">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-291">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="c93dc-292">Jeśli konieczne jest zapewnienie konfiguracji aplikacji z dodatkowe zmienne środowiskowe, wywołaj dodatkowych dostawców aplikacji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> i wywołać `AddEnvironmentVariables` z prefiksem.</span><span class="sxs-lookup"><span data-stu-id="c93dc-292">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="c93dc-293">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c93dc-293">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="c93dc-294">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="c93dc-294">**Example**</span></span>

<span data-ttu-id="c93dc-295">Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta, który zawiera wywołanie `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-295">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="c93dc-296">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="c93dc-296">Run the sample app.</span></span> <span data-ttu-id="c93dc-297">Otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-297">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c93dc-298">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla zmiennej środowiskowej `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-298">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="c93dc-299">Wartość odzwierciedla środowisko, w którym aplikacja jest uruchomiona, zwykle `Development` podczas uruchamiania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="c93dc-299">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="c93dc-300">Przechowywać listę zmiennych środowiskowych renderowany przez aplikację krótki, aplikacji filtry zmienne środowiskowe do tych rozpoczynających się od następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c93dc-300">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="c93dc-301">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="c93dc-301">ASPNETCORE_</span></span>
* <span data-ttu-id="c93dc-302">adresy URL</span><span class="sxs-lookup"><span data-stu-id="c93dc-302">urls</span></span>
* <span data-ttu-id="c93dc-303">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="c93dc-303">Logging</span></span>
* <span data-ttu-id="c93dc-304">ŚRODOWISKO</span><span class="sxs-lookup"><span data-stu-id="c93dc-304">ENVIRONMENT</span></span>
* <span data-ttu-id="c93dc-305">contentRoot</span><span class="sxs-lookup"><span data-stu-id="c93dc-305">contentRoot</span></span>
* <span data-ttu-id="c93dc-306">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="c93dc-306">AllowedHosts</span></span>
* <span data-ttu-id="c93dc-307">applicationName</span><span class="sxs-lookup"><span data-stu-id="c93dc-307">applicationName</span></span>
* <span data-ttu-id="c93dc-308">Wiersz polecenia</span><span class="sxs-lookup"><span data-stu-id="c93dc-308">CommandLine</span></span>

<span data-ttu-id="c93dc-309">Jeśli chcesz udostępnić wszystkie zmienne środowiskowe, dostępne dla aplikacji, zmień `FilteredConfiguration` w *Pages/Index.cshtml.cs* do następującego:</span><span class="sxs-lookup"><span data-stu-id="c93dc-309">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="c93dc-310">Prefiksy</span><span class="sxs-lookup"><span data-stu-id="c93dc-310">Prefixes</span></span>

<span data-ttu-id="c93dc-311">Zmienne środowiskowe ładowane z konfiguracji aplikacji są filtrowane podczas podawania prefiks `AddEnvironmentVariables` metody.</span><span class="sxs-lookup"><span data-stu-id="c93dc-311">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="c93dc-312">Na przykład, aby filtr zmiennych środowiskowych na prefiksie `CUSTOM_`, podaj prefiks, który dostawca konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-312">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="c93dc-313">Prefiks jest odłączany podczas tworzenia pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-313">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="c93dc-314">Statyczne wygodna metoda `CreateDefaultBuilder` tworzy <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> Aby ustanowić hosta aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-314">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="c93dc-315">Gdy `WebHostBuilder` jest tworzony, znajdzie jego konfigurację hosta w zmiennych środowiskowych z prefiksem `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-315">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="c93dc-316">**Prefiksy ciąg połączenia**</span><span class="sxs-lookup"><span data-stu-id="c93dc-316">**Connection string prefixes**</span></span>

<span data-ttu-id="c93dc-317">Interfejs API konfiguracji zawiera reguły jest przetwarzana w specjalny cztery połączenia ciągu zmiennych środowiskowych zajmujących się Konfigurowanie parametrów połączenia platformy Azure dla środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-317">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="c93dc-318">Zmienne środowiskowe z prefiksami pokazano w tabeli są ładowane do aplikacji, jeśli nie dostarczono żadnego prefiksu do `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-318">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="c93dc-319">Prefiks ciągu połączenia</span><span class="sxs-lookup"><span data-stu-id="c93dc-319">Connection string prefix</span></span> | <span data-ttu-id="c93dc-320">Dostawca</span><span class="sxs-lookup"><span data-stu-id="c93dc-320">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="c93dc-321">Dostawcy niestandardowego</span><span class="sxs-lookup"><span data-stu-id="c93dc-321">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="c93dc-322">MySQL</span><span class="sxs-lookup"><span data-stu-id="c93dc-322">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="c93dc-323">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c93dc-323">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="c93dc-324">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c93dc-324">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="c93dc-325">Gdy zmienna środowiskowa są odnajdywane i ładowane do konfiguracji za pomocą dowolnego z czterech prefiksy z tabelą:</span><span class="sxs-lookup"><span data-stu-id="c93dc-325">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="c93dc-326">Klucz konfiguracji jest tworzony, usuwając prefiks zmiennej środowiska i dodając kluczowych sekcję konfiguracji (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="c93dc-326">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="c93dc-327">Tworzony jest nową parę klucz wartość konfiguracji, który reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, który nie ma określonego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="c93dc-327">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="c93dc-328">Klucz zmiennych środowiska</span><span class="sxs-lookup"><span data-stu-id="c93dc-328">Environment variable key</span></span> | <span data-ttu-id="c93dc-329">Klucz konfiguracji przekonwertowany</span><span class="sxs-lookup"><span data-stu-id="c93dc-329">Converted configuration key</span></span> | <span data-ttu-id="c93dc-330">Pozycja konfiguracji dostawcy</span><span class="sxs-lookup"><span data-stu-id="c93dc-330">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c93dc-331">Wpis konfiguracyjny nie utworzony.</span><span class="sxs-lookup"><span data-stu-id="c93dc-331">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c93dc-332">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="c93dc-332">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="c93dc-333">Wartość:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="c93dc-333">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c93dc-334">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="c93dc-334">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="c93dc-335">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="c93dc-335">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="c93dc-336">Klucz: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="c93dc-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="c93dc-337">Wartość:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="c93dc-337">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="c93dc-338">Dostawca konfiguracji pliku</span><span class="sxs-lookup"><span data-stu-id="c93dc-338">File Configuration Provider</span></span>

<span data-ttu-id="c93dc-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową dla ładowania konfiguracji z systemu plików.</span><span class="sxs-lookup"><span data-stu-id="c93dc-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="c93dc-340">Następujących dostawców konfiguracji są przeznaczone dla określonych typów plików:</span><span class="sxs-lookup"><span data-stu-id="c93dc-340">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="c93dc-341">Dostawca konfiguracji INI</span><span class="sxs-lookup"><span data-stu-id="c93dc-341">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="c93dc-342">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="c93dc-342">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="c93dc-343">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="c93dc-343">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="c93dc-344">Dostawca konfiguracji INI</span><span class="sxs-lookup"><span data-stu-id="c93dc-344">INI Configuration Provider</span></span>

<span data-ttu-id="c93dc-345"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Ładuje konfiguracji z pary klucz wartość pliku INI w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c93dc-345">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="c93dc-346">Aby aktywować konfiguracji pliku INI, należy wywołać <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-346">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c93dc-347">Dwukropek może służyć do jako ogranicznik sekcji w konfiguracji pliku INI.</span><span class="sxs-lookup"><span data-stu-id="c93dc-347">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="c93dc-348">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c93dc-348">Overloads permit specifying:</span></span>

* <span data-ttu-id="c93dc-349">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c93dc-349">Whether the file is optional.</span></span>
* <span data-ttu-id="c93dc-350">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="c93dc-350">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c93dc-351"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="c93dc-351">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="c93dc-352">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-352">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c93dc-353">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-353">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c93dc-354">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-354">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c93dc-355">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c93dc-355">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c93dc-356">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-356">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c93dc-357">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-357">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c93dc-358">Przykładowy plik konfiguracji INI:</span><span class="sxs-lookup"><span data-stu-id="c93dc-358">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="c93dc-359">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c93dc-359">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c93dc-360">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c93dc-360">section0:key0</span></span>
* <span data-ttu-id="c93dc-361">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c93dc-361">section0:key1</span></span>
* <span data-ttu-id="c93dc-362">section1:Subsection:key</span><span class="sxs-lookup"><span data-stu-id="c93dc-362">section1:subsection:key</span></span>
* <span data-ttu-id="c93dc-363">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="c93dc-363">section2:subsection0:key</span></span>
* <span data-ttu-id="c93dc-364">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="c93dc-364">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="c93dc-365">Dostawca konfiguracji JSON</span><span class="sxs-lookup"><span data-stu-id="c93dc-365">JSON Configuration Provider</span></span>

<span data-ttu-id="c93dc-366"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Ładuje konfiguracji z pary klucz wartość plik JSON podczas wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c93dc-366">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="c93dc-367">Aby aktywować konfiguracji pliku JSON, należy wywołać <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-367">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c93dc-368">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c93dc-368">Overloads permit specifying:</span></span>

* <span data-ttu-id="c93dc-369">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c93dc-369">Whether the file is optional.</span></span>
* <span data-ttu-id="c93dc-370">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="c93dc-370">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c93dc-371"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="c93dc-371">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="c93dc-372">`AddJsonFile` jest wywoływana automatycznie, dwa razy, podczas inicjowania nowego <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> z <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-372">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="c93dc-373">Metoda jest wywoływana, aby załadować konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-373">The method is called to load configuration from:</span></span>

* <span data-ttu-id="c93dc-374">*appSettings.JSON* &ndash; ten plik jest najpierw do odczytu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-374">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="c93dc-375">Wersja środowiska pliku można zastąpić wartości dostarczone przez *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="c93dc-375">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="c93dc-376">*appSettings. {Środowiska} .json* &ndash; środowiska wersję pliku zostanie załadowany na podstawie [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="c93dc-376">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="c93dc-377">Aby uzyskać więcej informacji, zobacz [hosta sieci Web: Konfigurowanie hosta](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="c93dc-377">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="c93dc-378">`CreateDefaultBuilder` także obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c93dc-378">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c93dc-379">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="c93dc-379">Environment variables.</span></span>
* <span data-ttu-id="c93dc-380">[Wpisami tajnymi użytkowników (klucz tajny Menedżer)](xref:security/app-secrets) (w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="c93dc-380">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="c93dc-381">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c93dc-381">Command-line arguments.</span></span>

<span data-ttu-id="c93dc-382">Dostawca konfiguracji JSON jest ustanawiane, najpierw.</span><span class="sxs-lookup"><span data-stu-id="c93dc-382">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="c93dc-383">W związku z tym, wpisami tajnymi użytkowników, zmienne środowiskowe i argumenty wiersza polecenia zastępuje konfiguracji ustawione przez *appsettings* plików.</span><span class="sxs-lookup"><span data-stu-id="c93dc-383">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="c93dc-384">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji dla plików innych niż *appsettings.json* i *appsettings. { Środowisko} .json*:</span><span class="sxs-lookup"><span data-stu-id="c93dc-384">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="c93dc-385">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-385">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c93dc-386">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-386">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c93dc-387">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c93dc-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c93dc-388">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-388">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c93dc-389">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-389">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c93dc-390">**Przykład**</span><span class="sxs-lookup"><span data-stu-id="c93dc-390">**Example**</span></span>

<span data-ttu-id="c93dc-391">Przykładowa aplikacja korzysta z metody statyczne wygody `CreateDefaultBuilder` do hosta, który obejmuje dwa wywołania do tworzenia `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-391">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="c93dc-392">Konfiguracja została załadowana z *appsettings.json* i *appsettings. { Środowisko} .json*.</span><span class="sxs-lookup"><span data-stu-id="c93dc-392">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="c93dc-393">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="c93dc-393">Run the sample app.</span></span> <span data-ttu-id="c93dc-394">Otwórz przeglądarkę dla aplikacji w momencie `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-394">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c93dc-395">Sprawdź, czy dane wyjściowe zawierają pary klucz wartość dla konfiguracji, jak pokazano w tabeli, w zależności od środowiska.</span><span class="sxs-lookup"><span data-stu-id="c93dc-395">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="c93dc-396">Klucze konfiguracji rejestrowania, użyj dwukropka (`:`) jako separator hierarchicznej.</span><span class="sxs-lookup"><span data-stu-id="c93dc-396">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="c93dc-397">Key</span><span class="sxs-lookup"><span data-stu-id="c93dc-397">Key</span></span>                        | <span data-ttu-id="c93dc-398">Wartość rozwoju</span><span class="sxs-lookup"><span data-stu-id="c93dc-398">Development Value</span></span> | <span data-ttu-id="c93dc-399">Wartość produkcji</span><span class="sxs-lookup"><span data-stu-id="c93dc-399">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="c93dc-400">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="c93dc-400">Logging:LogLevel:System</span></span>    | <span data-ttu-id="c93dc-401">Informacje</span><span class="sxs-lookup"><span data-stu-id="c93dc-401">Information</span></span>       | <span data-ttu-id="c93dc-402">Informacje</span><span class="sxs-lookup"><span data-stu-id="c93dc-402">Information</span></span>      |
| <span data-ttu-id="c93dc-403">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="c93dc-403">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="c93dc-404">Informacje</span><span class="sxs-lookup"><span data-stu-id="c93dc-404">Information</span></span>       | <span data-ttu-id="c93dc-405">Informacje</span><span class="sxs-lookup"><span data-stu-id="c93dc-405">Information</span></span>      |
| <span data-ttu-id="c93dc-406">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="c93dc-406">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="c93dc-407">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="c93dc-407">Debug</span></span>             | <span data-ttu-id="c93dc-408">Błąd</span><span class="sxs-lookup"><span data-stu-id="c93dc-408">Error</span></span>            |
| <span data-ttu-id="c93dc-409">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="c93dc-409">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="c93dc-410">Dostawca konfiguracji XML</span><span class="sxs-lookup"><span data-stu-id="c93dc-410">XML Configuration Provider</span></span>

<span data-ttu-id="c93dc-411"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Ładuje konfiguracji z pary klucz wartość plik XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c93dc-411">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="c93dc-412">Aby aktywować Konfiguracja pliku XML, należy wywołać <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-412">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c93dc-413">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c93dc-413">Overloads permit specifying:</span></span>

* <span data-ttu-id="c93dc-414">Czy plik jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c93dc-414">Whether the file is optional.</span></span>
* <span data-ttu-id="c93dc-415">Czy konfiguracji jest załadowany ponownie, jeśli zmieni się plik.</span><span class="sxs-lookup"><span data-stu-id="c93dc-415">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c93dc-416"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskania dostępu do pliku.</span><span class="sxs-lookup"><span data-stu-id="c93dc-416">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="c93dc-417">Węzeł główny plik konfiguracji jest ignorowany podczas tworzenia pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-417">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="c93dc-418">Określać definicji typu dokumentu (DTD) lub przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="c93dc-418">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="c93dc-419">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-419">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c93dc-420">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-420">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c93dc-421">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-421">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c93dc-422">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c93dc-422">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="c93dc-423">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-423">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c93dc-424">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-424">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c93dc-425">Pliki konfiguracji XML można użyć nazw unikatowych elementów dla powtarzających się sekcje:</span><span class="sxs-lookup"><span data-stu-id="c93dc-425">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="c93dc-426">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c93dc-426">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c93dc-427">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c93dc-427">section0:key0</span></span>
* <span data-ttu-id="c93dc-428">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c93dc-428">section0:key1</span></span>
* <span data-ttu-id="c93dc-429">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c93dc-429">section1:key0</span></span>
* <span data-ttu-id="c93dc-430">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c93dc-430">section1:key1</span></span>

<span data-ttu-id="c93dc-431">Powtarzające się elementy, które używają takiej samej nazwie elementu pracy, jeśli `name` atrybut jest używany do odróżniania elementów:</span><span class="sxs-lookup"><span data-stu-id="c93dc-431">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="c93dc-432">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c93dc-432">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c93dc-433">sekcja: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="c93dc-433">section:section0:key:key0</span></span>
* <span data-ttu-id="c93dc-434">sekcja: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="c93dc-434">section:section0:key:key1</span></span>
* <span data-ttu-id="c93dc-435">sekcja: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="c93dc-435">section:section1:key:key0</span></span>
* <span data-ttu-id="c93dc-436">sekcja: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="c93dc-436">section:section1:key:key1</span></span>

<span data-ttu-id="c93dc-437">Atrybuty można podać wartości:</span><span class="sxs-lookup"><span data-stu-id="c93dc-437">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="c93dc-438">Plik konfiguracyjny poprzedniego ładuje następujące klucze za pomocą `value`:</span><span class="sxs-lookup"><span data-stu-id="c93dc-438">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c93dc-439">klucz: atrybut</span><span class="sxs-lookup"><span data-stu-id="c93dc-439">key:attribute</span></span>
* <span data-ttu-id="c93dc-440">sekcja: klucz: atrybut</span><span class="sxs-lookup"><span data-stu-id="c93dc-440">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="c93dc-441">Dostawca klucza każdego pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c93dc-441">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="c93dc-442"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Korzysta z plików w katalogu jako pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-442">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="c93dc-443">Klucz jest nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="c93dc-443">The key is the file name.</span></span> <span data-ttu-id="c93dc-444">Wartość zawiera zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="c93dc-444">The value contains the file's contents.</span></span> <span data-ttu-id="c93dc-445">Dostawca konfiguracji klucza każdego pliku jest używany w scenariuszach hostingu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="c93dc-445">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="c93dc-446">Aby aktywować klucza dla pliku konfiguracji, należy wywołać <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-446">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="c93dc-447">`directoryPath` Do plików musi być ścieżką bezwzględną.</span><span class="sxs-lookup"><span data-stu-id="c93dc-447">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="c93dc-448">Przeciążenia zezwala na określenie:</span><span class="sxs-lookup"><span data-stu-id="c93dc-448">Overloads permit specifying:</span></span>

* <span data-ttu-id="c93dc-449">`Action<KeyPerFileConfigurationSource>` Delegat, który konfiguruje źródło.</span><span class="sxs-lookup"><span data-stu-id="c93dc-449">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="c93dc-450">Czy katalog jest opcjonalna i ścieżkę do katalogu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-450">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="c93dc-451">Podwójne podkreślenie (`__`) jest używany jako ogranicznika klucza konfiguracji w nazwach plików.</span><span class="sxs-lookup"><span data-stu-id="c93dc-451">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="c93dc-452">Na przykład, nazwa_pliku `Logging__LogLevel__System` generuje klucz konfiguracji `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-452">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="c93dc-453">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c93dc-454">Ścieżka podstawowa została ustawiona za pomocą <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="c93dc-455">`SetBasePath` Trwa [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c93dc-456">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c93dc-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="c93dc-457">Dostawca konfiguracji pamięci</span><span class="sxs-lookup"><span data-stu-id="c93dc-457">Memory Configuration Provider</span></span>

<span data-ttu-id="c93dc-458"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Korzysta z kolekcji w pamięci jako pary klucz wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-458">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="c93dc-459">Aby aktywować konfiguracji kolekcji w pamięci, należy wywołać <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> metody rozszerzenia w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-459">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c93dc-460">Dostawca konfiguracji mogą być zainicjowane z `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-460">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="c93dc-461">Wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> podczas tworzenia hosta, aby określić konfigurację aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-461">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="c93dc-462">Podczas tworzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> , dzwoniąc <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="c93dc-462">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="c93dc-463">GetValue</span><span class="sxs-lookup"><span data-stu-id="c93dc-463">GetValue</span></span>

<span data-ttu-id="c93dc-464">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnianie wartości z konfiguracji z określonym kluczem i konwertuje je do określonego typu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-464">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="c93dc-465">Przeciążenie umożliwia podanie wartości domyślnej, jeśli nie odnaleziono klucza.</span><span class="sxs-lookup"><span data-stu-id="c93dc-465">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="c93dc-466">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="c93dc-466">The following example:</span></span>

* <span data-ttu-id="c93dc-467">Wyodrębnia wartość ciągu z konfiguracji przy użyciu klucza `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-467">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="c93dc-468">Jeśli `NumberKey` nie zostanie odnaleziona w klucze konfiguracji, wartość domyślna `99` jest używany.</span><span class="sxs-lookup"><span data-stu-id="c93dc-468">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="c93dc-469">Typy wartości jako `int`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-469">Types the value as an `int`.</span></span>
* <span data-ttu-id="c93dc-470">Przechowuje wartość w `NumberConfig` właściwości do użytku przez stronę.</span><span class="sxs-lookup"><span data-stu-id="c93dc-470">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="c93dc-471">GetSection, GetChildren i istnieje</span><span class="sxs-lookup"><span data-stu-id="c93dc-471">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="c93dc-472">Aby uzyskać przykłady, które należy wykonać należy wziąć pod uwagę następujący plik JSON.</span><span class="sxs-lookup"><span data-stu-id="c93dc-472">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="c93dc-473">Cztery klucze znajdują się na dwie sekcje, z których jedna zawiera parę podsekcje:</span><span class="sxs-lookup"><span data-stu-id="c93dc-473">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="c93dc-474">Gdy plik jest do odczytu do konfiguracji, następujące klucze unikatowe hierarchiczne są tworzone do przechowywania wartości konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-474">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="c93dc-475">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c93dc-475">section0:key0</span></span>
* <span data-ttu-id="c93dc-476">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c93dc-476">section0:key1</span></span>
* <span data-ttu-id="c93dc-477">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c93dc-477">section1:key0</span></span>
* <span data-ttu-id="c93dc-478">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c93dc-478">section1:key1</span></span>
* <span data-ttu-id="c93dc-479">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="c93dc-479">section2:subsection0:key0</span></span>
* <span data-ttu-id="c93dc-480">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="c93dc-480">section2:subsection0:key1</span></span>
* <span data-ttu-id="c93dc-481">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="c93dc-481">section2:subsection1:key0</span></span>
* <span data-ttu-id="c93dc-482">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="c93dc-482">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="c93dc-483">GetSection</span><span class="sxs-lookup"><span data-stu-id="c93dc-483">GetSection</span></span>

<span data-ttu-id="c93dc-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcji konfiguracji z kluczem określonym podsekcji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="c93dc-485">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-485">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c93dc-486">Aby zwrócić <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz wartość w `section1`, wywołaj `GetSection` i podaj nazwę sekcji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-486">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="c93dc-487">`configSection` Nie ma wartości, tylko klucz i ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="c93dc-487">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="c93dc-488">Podobnie Aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę do sekcji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-488">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="c93dc-489">`GetSection` nigdy nie zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-489">`GetSection` never returns `null`.</span></span> <span data-ttu-id="c93dc-490">Jeśli nie zostanie znaleziona pasująca sekcja, pusta `IConfigurationSection` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="c93dc-490">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="c93dc-491">Gdy `GetSection` zwraca pasujących sekcji <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione.</span><span class="sxs-lookup"><span data-stu-id="c93dc-491">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="c93dc-492">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> i <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> są zwracane, gdy istnieje sekcji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-492">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="c93dc-493">GetChildren</span><span class="sxs-lookup"><span data-stu-id="c93dc-493">GetChildren</span></span>

<span data-ttu-id="c93dc-494">Wywołanie [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) na `section2` uzyskuje `IEnumerable<IConfigurationSection>` zawierającej:</span><span class="sxs-lookup"><span data-stu-id="c93dc-494">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="c93dc-495">Exists</span><span class="sxs-lookup"><span data-stu-id="c93dc-495">Exists</span></span>

<span data-ttu-id="c93dc-496">Użyj [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) do ustalenia, czy istnieje sekcji konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-496">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="c93dc-497">Podane dane przykładowe `sectionExists` jest `false` , ponieważ nie ma `section2:subsection2` sekcji w danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-497">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="c93dc-498">Powiąż z klasy</span><span class="sxs-lookup"><span data-stu-id="c93dc-498">Bind to a class</span></span>

<span data-ttu-id="c93dc-499">Konfiguracja może być powiązana z klas, które reprezentują grupy powiązane ustawienia za pomocą *wzorzec opcje*.</span><span class="sxs-lookup"><span data-stu-id="c93dc-499">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="c93dc-500">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-500">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="c93dc-501">Wartości konfiguracji są zwracane jako ciągi, ale wywoływania <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) obiektów.</span><span class="sxs-lookup"><span data-stu-id="c93dc-501">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="c93dc-502">`Bind` Trwa [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-502">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c93dc-503">Przykładowa aplikacja zawiera `Starship` modelu (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="c93dc-503">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="c93dc-504">`starship` Części *starship.json* plików umożliwia utworzenie konfiguracji podczas Przykładowa aplikacja używa dostawcy konfiguracji JSON można załadować konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-504">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="c93dc-505">Tworzone są następujące pary klucz wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-505">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="c93dc-506">Key</span><span class="sxs-lookup"><span data-stu-id="c93dc-506">Key</span></span>                   | <span data-ttu-id="c93dc-507">Wartość</span><span class="sxs-lookup"><span data-stu-id="c93dc-507">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="c93dc-508">starship: Nazwa</span><span class="sxs-lookup"><span data-stu-id="c93dc-508">starship:name</span></span>         | <span data-ttu-id="c93dc-509">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="c93dc-509">USS Enterprise</span></span>                                    |
| <span data-ttu-id="c93dc-510">starship: rejestru</span><span class="sxs-lookup"><span data-stu-id="c93dc-510">starship:registry</span></span>     | <span data-ttu-id="c93dc-511">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="c93dc-511">NCC-1701</span></span>                                          |
| <span data-ttu-id="c93dc-512">starship: klasy</span><span class="sxs-lookup"><span data-stu-id="c93dc-512">starship:class</span></span>        | <span data-ttu-id="c93dc-513">Tworzenia</span><span class="sxs-lookup"><span data-stu-id="c93dc-513">Constitution</span></span>                                      |
| <span data-ttu-id="c93dc-514">starship: długość</span><span class="sxs-lookup"><span data-stu-id="c93dc-514">starship:length</span></span>       | <span data-ttu-id="c93dc-515">304.8</span><span class="sxs-lookup"><span data-stu-id="c93dc-515">304.8</span></span>                                             |
| <span data-ttu-id="c93dc-516">starship: upoważnione</span><span class="sxs-lookup"><span data-stu-id="c93dc-516">starship:commissioned</span></span> | <span data-ttu-id="c93dc-517">False</span><span class="sxs-lookup"><span data-stu-id="c93dc-517">False</span></span>                                             |
| <span data-ttu-id="c93dc-518">Znak towarowy</span><span class="sxs-lookup"><span data-stu-id="c93dc-518">trademark</span></span>             | <span data-ttu-id="c93dc-519">Paramount obrazy Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="c93dc-519">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="c93dc-520">Wywołania aplikacji przykładowej `GetSection` z `starship` klucza.</span><span class="sxs-lookup"><span data-stu-id="c93dc-520">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="c93dc-521">`starship` Pary klucz wartość są izolowane.</span><span class="sxs-lookup"><span data-stu-id="c93dc-521">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="c93dc-522">`Bind` Metoda jest wywoływana w podsekcji, przekazując wystąpienie `Starship` klasy.</span><span class="sxs-lookup"><span data-stu-id="c93dc-522">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="c93dc-523">Po powiązaniu wartości wystąpienia, wystąpienie jest przypisany do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="c93dc-523">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="c93dc-524">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-524">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="c93dc-525">Powiąż z wykresu obiektu</span><span class="sxs-lookup"><span data-stu-id="c93dc-525">Bind to an object graph</span></span>

<span data-ttu-id="c93dc-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> jest w stanie całego grafu obiektów POCO powiązania.</span><span class="sxs-lookup"><span data-stu-id="c93dc-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="c93dc-527">`Bind` Trwa [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-527">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c93dc-528">Przykładowy zawiera `TvShow` model zawiera wykres obiektu, którego `Metadata` i `Actors` klasy (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="c93dc-528">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="c93dc-529">Przykładowa aplikacja ma *tvshow.xml* plik zawierający dane konfiguracyjne:</span><span class="sxs-lookup"><span data-stu-id="c93dc-529">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="c93dc-530">Konfiguracja jest powiązany z całej `TvShow` wykres obiektu z `Bind` metody.</span><span class="sxs-lookup"><span data-stu-id="c93dc-530">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="c93dc-531">Powiązane wystąpienia jest przypisywany do właściwości w celu renderowania:</span><span class="sxs-lookup"><span data-stu-id="c93dc-531">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="c93dc-532">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) wiąże i zwraca wartość określonego typu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-532">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="c93dc-533">`Get<T>` jest bardziej wygodne niż przy użyciu `Bind`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-533">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="c93dc-534">Poniższy kod przedstawia sposób użycia `Get<T>` w poprzednim przykładzie, co umożliwia wystąpienie powiązanych, którego można przypisać bezpośrednio do właściwości, używany do renderowania:</span><span class="sxs-lookup"><span data-stu-id="c93dc-534">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="c93dc-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> Trwa [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="c93dc-536">`Get<T>` jest dostępna w programie ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="c93dc-536">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="c93dc-537">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-537">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="c93dc-538">Powiąż tablicę do klasy</span><span class="sxs-lookup"><span data-stu-id="c93dc-538">Bind an array to a class</span></span>

<span data-ttu-id="c93dc-539">*Przykładowa aplikacja pokazuje pojęcia opisane w tej sekcji.*</span><span class="sxs-lookup"><span data-stu-id="c93dc-539">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="c93dc-540"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Obsługuje tablice powiązania do obiektów przy użyciu tablicy indeksów w klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-540">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="c93dc-541">Dowolnym formacie tablicy, który udostępnia liczbowych segment klucza (`:0:`, `:1:`, &hellip; `:{n}:`) jest w stanie tablicy powiązania z macierzą POCO klasy.</span><span class="sxs-lookup"><span data-stu-id="c93dc-541">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="c93dc-542">"Związać" znajduje się w [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-542">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="c93dc-543">Powiązanie jest zapewniana przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="c93dc-543">Binding is provided by convention.</span></span> <span data-ttu-id="c93dc-544">Konfiguracja niestandardowa dostawców nie są wymagane do zaimplementowania tablicy powiązania.</span><span class="sxs-lookup"><span data-stu-id="c93dc-544">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="c93dc-545">**Przetwarzanie w pamięci tablica**</span><span class="sxs-lookup"><span data-stu-id="c93dc-545">**In-memory array processing**</span></span>

<span data-ttu-id="c93dc-546">Należy wziąć pod uwagę kluczy i wartości konfiguracji pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c93dc-546">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="c93dc-547">Key</span><span class="sxs-lookup"><span data-stu-id="c93dc-547">Key</span></span>             | <span data-ttu-id="c93dc-548">Wartość</span><span class="sxs-lookup"><span data-stu-id="c93dc-548">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="c93dc-549">Macierz: wpisów: 0</span><span class="sxs-lookup"><span data-stu-id="c93dc-549">array:entries:0</span></span> | <span data-ttu-id="c93dc-550">value0</span><span class="sxs-lookup"><span data-stu-id="c93dc-550">value0</span></span> |
| <span data-ttu-id="c93dc-551">Macierz: wpisów: 1</span><span class="sxs-lookup"><span data-stu-id="c93dc-551">array:entries:1</span></span> | <span data-ttu-id="c93dc-552">value1</span><span class="sxs-lookup"><span data-stu-id="c93dc-552">value1</span></span> |
| <span data-ttu-id="c93dc-553">Macierz: wpisów: 2</span><span class="sxs-lookup"><span data-stu-id="c93dc-553">array:entries:2</span></span> | <span data-ttu-id="c93dc-554">value2</span><span class="sxs-lookup"><span data-stu-id="c93dc-554">value2</span></span> |
| <span data-ttu-id="c93dc-555">Macierz: wpisów: 4</span><span class="sxs-lookup"><span data-stu-id="c93dc-555">array:entries:4</span></span> | <span data-ttu-id="c93dc-556">value4</span><span class="sxs-lookup"><span data-stu-id="c93dc-556">value4</span></span> |
| <span data-ttu-id="c93dc-557">Macierz: wpisów: 5</span><span class="sxs-lookup"><span data-stu-id="c93dc-557">array:entries:5</span></span> | <span data-ttu-id="c93dc-558">value5</span><span class="sxs-lookup"><span data-stu-id="c93dc-558">value5</span></span> |

<span data-ttu-id="c93dc-559">Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:</span><span class="sxs-lookup"><span data-stu-id="c93dc-559">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

<span data-ttu-id="c93dc-560">Tablica pomija wartość indeksu &num;3.</span><span class="sxs-lookup"><span data-stu-id="c93dc-560">The array skips a value for index &num;3.</span></span> <span data-ttu-id="c93dc-561">Konfiguracja obiektu wiążącego nie jest zdolny do powiązania wartości null lub tworzenie wartości null wpisów w powiązanych obiektów, które stają się w chwili, gdy przedstawiono wynik powiązania tej tablicy do obiektu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-561">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="c93dc-562">W przykładowej aplikacji do przechowywania danych konfiguracji powiązanej dostępnej klasy POCO:</span><span class="sxs-lookup"><span data-stu-id="c93dc-562">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="c93dc-563">Dane konfiguracji jest powiązana z obiektem:</span><span class="sxs-lookup"><span data-stu-id="c93dc-563">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="c93dc-564">`GetSection` Trwa [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c93dc-564">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c93dc-565">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) składni można również użyć, która skutkuje bardziej kompaktowy kod:</span><span class="sxs-lookup"><span data-stu-id="c93dc-565">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="c93dc-566">Powiązany obiekt, wystąpienie `ArrayExample`, odbiera dane tablicy z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-566">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="c93dc-567">`ArrayExample.Entries` Indeks</span><span class="sxs-lookup"><span data-stu-id="c93dc-567">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="c93dc-568">`ArrayExample.Entries` Wartość</span><span class="sxs-lookup"><span data-stu-id="c93dc-568">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="c93dc-569">0</span><span class="sxs-lookup"><span data-stu-id="c93dc-569">0</span></span>                            | <span data-ttu-id="c93dc-570">value0</span><span class="sxs-lookup"><span data-stu-id="c93dc-570">value0</span></span>                       |
| <span data-ttu-id="c93dc-571">1</span><span class="sxs-lookup"><span data-stu-id="c93dc-571">1</span></span>                            | <span data-ttu-id="c93dc-572">value1</span><span class="sxs-lookup"><span data-stu-id="c93dc-572">value1</span></span>                       |
| <span data-ttu-id="c93dc-573">2</span><span class="sxs-lookup"><span data-stu-id="c93dc-573">2</span></span>                            | <span data-ttu-id="c93dc-574">value2</span><span class="sxs-lookup"><span data-stu-id="c93dc-574">value2</span></span>                       |
| <span data-ttu-id="c93dc-575">3</span><span class="sxs-lookup"><span data-stu-id="c93dc-575">3</span></span>                            | <span data-ttu-id="c93dc-576">value4</span><span class="sxs-lookup"><span data-stu-id="c93dc-576">value4</span></span>                       |
| <span data-ttu-id="c93dc-577">4</span><span class="sxs-lookup"><span data-stu-id="c93dc-577">4</span></span>                            | <span data-ttu-id="c93dc-578">value5</span><span class="sxs-lookup"><span data-stu-id="c93dc-578">value5</span></span>                       |

<span data-ttu-id="c93dc-579">Indeks &num;3 w powiązany obiekt przechowuje dane konfiguracyjne `array:4` klucz konfiguracji i jego wartość `value4`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-579">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="c93dc-580">Gdy dane konfiguracji, które zawiera tablicę jest powiązana, indeksy tablicy w klucze konfiguracji jedynie służą do iteracji dane konfiguracji, podczas tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="c93dc-580">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="c93dc-581">Wartość null nie mogą być przechowywane w danych konfiguracji, a wpis o wartości null nie jest tworzona w powiązany obiekt, w przypadku tablicy w konfiguracji kluczy Pomiń jeden lub więcej indeksów.</span><span class="sxs-lookup"><span data-stu-id="c93dc-581">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="c93dc-582">Brak elementu konfiguracji dla indeksu &num;3 mogą być dostarczane przed powiązanie `ArrayExample` wystąpienie przez dowolnego dostawcę konfiguracji, tworzącego prawidłowe pary klucz wartość w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-582">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="c93dc-583">Jeśli przykład uwzględniony dodatkowego dostawcę konfiguracji JSON z Brak pary klucz wartość `ArrayExample.Entries` pasuje do tablicy kompletna konfiguracja:</span><span class="sxs-lookup"><span data-stu-id="c93dc-583">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="c93dc-584">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="c93dc-584">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="c93dc-585">W <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="c93dc-585">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="c93dc-586">Pary klucz wartość, jak pokazano w tabeli są ładowane do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-586">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="c93dc-587">Key</span><span class="sxs-lookup"><span data-stu-id="c93dc-587">Key</span></span>             | <span data-ttu-id="c93dc-588">Wartość</span><span class="sxs-lookup"><span data-stu-id="c93dc-588">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="c93dc-589">Macierz: wpisów: 3</span><span class="sxs-lookup"><span data-stu-id="c93dc-589">array:entries:3</span></span> | <span data-ttu-id="c93dc-590">value3</span><span class="sxs-lookup"><span data-stu-id="c93dc-590">value3</span></span> |

<span data-ttu-id="c93dc-591">Jeśli `ArrayExample` wystąpienia klasy jest powiązana po dostawcę konfiguracji JSON zawiera wpis dla indeksu &num;3, `ArrayExample.Entries` tablicy zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="c93dc-591">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="c93dc-592">`ArrayExample.Entries` Indeks</span><span class="sxs-lookup"><span data-stu-id="c93dc-592">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="c93dc-593">`ArrayExample.Entries` Wartość</span><span class="sxs-lookup"><span data-stu-id="c93dc-593">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="c93dc-594">0</span><span class="sxs-lookup"><span data-stu-id="c93dc-594">0</span></span>                            | <span data-ttu-id="c93dc-595">value0</span><span class="sxs-lookup"><span data-stu-id="c93dc-595">value0</span></span>                       |
| <span data-ttu-id="c93dc-596">1</span><span class="sxs-lookup"><span data-stu-id="c93dc-596">1</span></span>                            | <span data-ttu-id="c93dc-597">value1</span><span class="sxs-lookup"><span data-stu-id="c93dc-597">value1</span></span>                       |
| <span data-ttu-id="c93dc-598">2</span><span class="sxs-lookup"><span data-stu-id="c93dc-598">2</span></span>                            | <span data-ttu-id="c93dc-599">value2</span><span class="sxs-lookup"><span data-stu-id="c93dc-599">value2</span></span>                       |
| <span data-ttu-id="c93dc-600">3</span><span class="sxs-lookup"><span data-stu-id="c93dc-600">3</span></span>                            | <span data-ttu-id="c93dc-601">value3</span><span class="sxs-lookup"><span data-stu-id="c93dc-601">value3</span></span>                       |
| <span data-ttu-id="c93dc-602">4</span><span class="sxs-lookup"><span data-stu-id="c93dc-602">4</span></span>                            | <span data-ttu-id="c93dc-603">value4</span><span class="sxs-lookup"><span data-stu-id="c93dc-603">value4</span></span>                       |
| <span data-ttu-id="c93dc-604">5</span><span class="sxs-lookup"><span data-stu-id="c93dc-604">5</span></span>                            | <span data-ttu-id="c93dc-605">value5</span><span class="sxs-lookup"><span data-stu-id="c93dc-605">value5</span></span>                       |

<span data-ttu-id="c93dc-606">**Przetwarzanie tablicy JSON**</span><span class="sxs-lookup"><span data-stu-id="c93dc-606">**JSON array processing**</span></span>

<span data-ttu-id="c93dc-607">Jeśli plik JSON zawiera tablicę, klucze konfiguracji zostaną utworzone dla elementów tablicy przy użyciu indeksu zaczynającego się od zera sekcji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-607">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="c93dc-608">W następującym pliku konfiguracji `subsection` jest tablicą:</span><span class="sxs-lookup"><span data-stu-id="c93dc-608">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="c93dc-609">Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz wartość:</span><span class="sxs-lookup"><span data-stu-id="c93dc-609">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="c93dc-610">Key</span><span class="sxs-lookup"><span data-stu-id="c93dc-610">Key</span></span>                     | <span data-ttu-id="c93dc-611">Wartość</span><span class="sxs-lookup"><span data-stu-id="c93dc-611">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="c93dc-612">json_array:key</span><span class="sxs-lookup"><span data-stu-id="c93dc-612">json_array:key</span></span>          | <span data-ttu-id="c93dc-613">valueA</span><span class="sxs-lookup"><span data-stu-id="c93dc-613">valueA</span></span> |
| <span data-ttu-id="c93dc-614">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="c93dc-614">json_array:subsection:0</span></span> | <span data-ttu-id="c93dc-615">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="c93dc-615">valueB</span></span> |
| <span data-ttu-id="c93dc-616">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="c93dc-616">json_array:subsection:1</span></span> | <span data-ttu-id="c93dc-617">valueC</span><span class="sxs-lookup"><span data-stu-id="c93dc-617">valueC</span></span> |
| <span data-ttu-id="c93dc-618">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="c93dc-618">json_array:subsection:2</span></span> | <span data-ttu-id="c93dc-619">valueD</span><span class="sxs-lookup"><span data-stu-id="c93dc-619">valueD</span></span> |

<span data-ttu-id="c93dc-620">W przykładowej aplikacji następującej klasy POCO jest dostępny dla powiązania pary klucz wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c93dc-620">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="c93dc-621">Po powiązaniu `JsonArrayExample.Key` przechowuje wartość `valueA`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-621">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="c93dc-622">Wartości podsekcji są przechowywane we właściwości tablicy POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-622">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="c93dc-623">`JsonArrayExample.Subsection` Indeks</span><span class="sxs-lookup"><span data-stu-id="c93dc-623">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="c93dc-624">`JsonArrayExample.Subsection` Wartość</span><span class="sxs-lookup"><span data-stu-id="c93dc-624">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="c93dc-625">0</span><span class="sxs-lookup"><span data-stu-id="c93dc-625">0</span></span>                                   | <span data-ttu-id="c93dc-626">Wartośćb</span><span class="sxs-lookup"><span data-stu-id="c93dc-626">valueB</span></span>                              |
| <span data-ttu-id="c93dc-627">1</span><span class="sxs-lookup"><span data-stu-id="c93dc-627">1</span></span>                                   | <span data-ttu-id="c93dc-628">valueC</span><span class="sxs-lookup"><span data-stu-id="c93dc-628">valueC</span></span>                              |
| <span data-ttu-id="c93dc-629">2</span><span class="sxs-lookup"><span data-stu-id="c93dc-629">2</span></span>                                   | <span data-ttu-id="c93dc-630">valueD</span><span class="sxs-lookup"><span data-stu-id="c93dc-630">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="c93dc-631">Dostawca konfiguracji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="c93dc-631">Custom configuration provider</span></span>

<span data-ttu-id="c93dc-632">Przykładowa aplikacja pokazuje, jak utworzyć dostawcę konfiguracji podstawowej, który odczytuje pary klucz wartość konfiguracji z bazy danych za pomocą [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="c93dc-632">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="c93dc-633">Dostawcę ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="c93dc-633">The provider has the following characteristics:</span></span>

* <span data-ttu-id="c93dc-634">Bazy danych w pamięci programu EF jest używana do celów demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="c93dc-634">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="c93dc-635">Aby użyć bazy danych, która wymaga parametrów połączenia, należy zaimplementować pomocniczy `ConfigurationBuilder` podawać parametrów połączenia z innego dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-635">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="c93dc-636">Dostawca odczytuje tabelę bazy danych do konfiguracji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="c93dc-636">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="c93dc-637">Dostawca nie kwerendy bazy danych na podstawie-key.</span><span class="sxs-lookup"><span data-stu-id="c93dc-637">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="c93dc-638">Załaduj ponownie przy zmianie nie jest zaimplementowany, więc zaktualizowanie bazy danych, po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c93dc-638">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="c93dc-639">Zdefiniuj `EFConfigurationValue` jednostki do przechowywania wartości konfiguracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c93dc-639">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="c93dc-640">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="c93dc-640">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="c93dc-641">Dodaj `EFConfigurationContext` do przechowywania i uzyskiwania dostępu skonfigurowane wartości.</span><span class="sxs-lookup"><span data-stu-id="c93dc-641">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="c93dc-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="c93dc-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="c93dc-643">Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-643">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="c93dc-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="c93dc-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="c93dc-645">Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-645">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="c93dc-646">Dostawca konfiguracji inicjuje bazy danych, gdy jest on pusty.</span><span class="sxs-lookup"><span data-stu-id="c93dc-646">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="c93dc-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="c93dc-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="c93dc-648">`AddEFConfiguration` Metody rozszerzenia, który pozwala na dodawanie źródła konfiguracji do `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-648">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="c93dc-649">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="c93dc-649">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="c93dc-650">Poniższy kod przedstawia sposób używania niestandardowego `EFConfigurationProvider` w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c93dc-650">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="c93dc-651">Konfiguracja dostępu podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="c93dc-651">Access configuration during startup</span></span>

<span data-ttu-id="c93dc-652">Wstrzykiwanie `IConfiguration` do `Startup` konstruktora do dostępu do wartości konfiguracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c93dc-652">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c93dc-653">Uzyskać dostępu do konfiguracji w `Startup.Configure`, albo wstrzyknąć `IConfiguration` bezpośrednio do metody lub użyj wystąpienia z konstruktora:</span><span class="sxs-lookup"><span data-stu-id="c93dc-653">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="c93dc-654">Na przykład uzyskiwania dostępu do konfiguracji za pomocą metod jako udogodnienie uruchamiania zobacz [uruchamiania aplikacji: Wygodne metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="c93dc-654">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="c93dc-655">Konfiguracja dostępu na stronie stron Razor lub w widoku MVC</span><span class="sxs-lookup"><span data-stu-id="c93dc-655">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="c93dc-656">Aby uzyskać dostęp do ustawień konfiguracji na stronie stron Razor lub widoku MVC, należy dodać [użycie dyrektywy](xref:mvc/views/razor#using) ([odwołanie w C#: using — dyrektywa](/dotnet/csharp/language-reference/keywords/using-directive)) dla [Microsoft.Extensions.Configuration przestrzeni nazw ](xref:Microsoft.Extensions.Configuration) i wstawiać <xref:Microsoft.Extensions.Configuration.IConfiguration> do strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="c93dc-656">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="c93dc-657">Na stronie stron Razor:</span><span class="sxs-lookup"><span data-stu-id="c93dc-657">In a Razor Pages page:</span></span>

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

<span data-ttu-id="c93dc-658">W widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="c93dc-658">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="c93dc-659">Dodaj konfigurację z zewnętrznego zestawu</span><span class="sxs-lookup"><span data-stu-id="c93dc-659">Add configuration from an external assembly</span></span>

<span data-ttu-id="c93dc-660"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Implementacji umożliwia dodawanie rozszerzeń do aplikacji przy uruchamianiu z zewnętrznego zestawu poza aplikacji `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="c93dc-660">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="c93dc-661">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c93dc-661">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c93dc-662">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c93dc-662">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="c93dc-663">Głębszej analizy konfiguracji firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="c93dc-663">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
