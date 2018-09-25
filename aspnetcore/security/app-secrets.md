---
title: Bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania w programie ASP.NET Core
author: rick-anderson
description: Informacje o sposobie przechowywania i pobierania informacji poufnych jako wpisy tajne aplikacji podczas tworzenia aplikacji ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/24/2018
uid: security/app-secrets
ms.openlocfilehash: 1ecd9b87b9e4aa2c511349fe407c0cea9dc9f921
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028274"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="eb1a4-103">Bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eb1a4-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="eb1a4-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), i [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="eb1a4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="eb1a4-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eb1a4-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="eb1a4-106">W tym dokumencie opisano techniki do przechowywania i pobierania danych poufnych podczas tworzenia aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="eb1a4-107">Nigdy nie przechowuj hasła lub innych danych poufnych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="eb1a4-108">Wpisy tajne w środowisku produkcyjnym nie powinny być używane do projektowania lub testowania.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="eb1a4-109">Możesz przechowywać i chronić Azure testowych i produkcyjnych wpisów tajnych za pomocą [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="eb1a4-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="eb1a4-110">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="eb1a4-110">Environment variables</span></span>

<span data-ttu-id="eb1a4-111">Zmienne środowiskowe są używane w celu uniknięcia przechowywanie kluczy tajnych aplikacji w kodzie lub w plikach konfiguracji lokalnej.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="eb1a4-112">Zmienne środowiskowe zastępować wartości konfiguracji dla wszystkich źródeł wcześniej określonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="eb1a4-113">Konfigurowanie odczytywania wartości zmiennych środowiskowych, wywołując [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) w `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="eb1a4-114">Należy wziąć pod uwagę aplikację internetową platformy ASP.NET Core, w którym **indywidualne konta użytkowników** zabezpieczenia są włączone.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="eb1a4-115">Domyślne parametry połączenia bazy danych znajduje się w projekcie *appsettings.json* pliku przy użyciu klucza `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="eb1a4-116">Domyślny ciąg połączenia jest do LocalDB, który działa w trybie użytkownika i nie wymaga hasła.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="eb1a4-117">Podczas wdrażania aplikacji `DefaultConnection` wartość klucza może zostać zastąpiona przez wartość zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="eb1a4-118">Zmienna środowiskowa mogą być przechowywane pełne parametry połączenia przy użyciu poświadczeń poufnych.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="eb1a4-119">Zmienne środowiskowe są zazwyczaj przechowywane w zwykły tekst niezaszyfrowane.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="eb1a4-120">W przypadku naruszenia zabezpieczeń komputera lub proces, dostęp do zmiennych środowiskowych jest możliwy przez niezaufane.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="eb1a4-121">Dodatkowe środki, aby zapobiec ujawnieniu wpisami tajnymi użytkowników mogą być wymagane.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="eb1a4-122">Menedżer wpisu tajnego</span><span class="sxs-lookup"><span data-stu-id="eb1a4-122">Secret Manager</span></span>

<span data-ttu-id="eb1a4-123">Narzędzie Menedżer klucz tajny są przechowywane poufne dane podczas tworzenia projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="eb1a4-124">W tym kontekście część danych poufnych jest wpis tajny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="eb1a4-125">Wpisy tajne aplikacji są przechowywane w innej lokalizacji w drzewie projektu.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="eb1a4-126">Wpisy tajne aplikacji są skojarzone z określonym projektem lub udostępnione w kilku projektach.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="eb1a4-127">Wpisy tajne aplikacji nie są sprawdzane w systemie kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="eb1a4-128">Narzędzie Menedżer wpis tajny nie szyfruje przechowywane klucze tajne i nie powinien być traktowany jako zaufany magazyn.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="eb1a4-129">Jest tylko do celów programowania.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-129">It's for development purposes only.</span></span> <span data-ttu-id="eb1a4-130">Klucze i wartości są przechowywane w pliku konfiguracji JSON w katalogu profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="eb1a4-131">Jak działa narzędzie Menedżer wpisu tajnego</span><span class="sxs-lookup"><span data-stu-id="eb1a4-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="eb1a4-132">Narzędzie Menedżer klucz tajny ukrywa szczegóły implementacji, takich jak gdzie i w jaki sposób wartości są przechowywane.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="eb1a4-133">Narzędzie nie znając te szczegóły implementacji.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="eb1a4-134">Wartości są przechowywane w pliku konfiguracji JSON w folderze profilu użytkownika chronionych przez system na komputerze lokalnym:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="eb1a4-135">Windows</span><span class="sxs-lookup"><span data-stu-id="eb1a4-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="eb1a4-136">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="eb1a4-137">macOS</span><span class="sxs-lookup"><span data-stu-id="eb1a4-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="eb1a4-138">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="eb1a4-139">Linux</span><span class="sxs-lookup"><span data-stu-id="eb1a4-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="eb1a4-140">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="eb1a4-141">W poprzednim ścieżki plików, Zastąp `<user_secrets_id>` z `UserSecretsId` wartości określonej w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="eb1a4-142">Nie pisz kod, który zależy od lokalizacji lub formatu danych zapisanych przy użyciu narzędzia menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="eb1a4-143">Szczegóły tych implementacji mogą ulec zmianie.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-143">These implementation details may change.</span></span> <span data-ttu-id="eb1a4-144">Na przykład klucza tajnego wartości nie są szyfrowane, ale może być w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="eb1a4-145">Zainstaluj narzędzie Menedżer wpisu tajnego</span><span class="sxs-lookup"><span data-stu-id="eb1a4-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="eb1a4-146">Narzędzie Menedżer klucz tajny jest powiązane z platformy .NET Core interfejsu wiersza polecenia w zestawie SDK programu .NET Core 2.1.300 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-146">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="eb1a4-147">Wersje programu .NET Core SDK przed 2.1.300 niezbędne jest narzędzie instalacji.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="eb1a4-148">Uruchom `dotnet --version` z powłoki poleceń, aby wyświetlić numer wersji zainstalowanego zestawu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="eb1a4-149">Zostanie wyświetlone ostrzeżenie, jeśli używany zestaw .NET Core SDK zawiera narzędzia:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="eb1a4-150">Zainstaluj [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pakietu NuGet w projekcie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="eb1a4-151">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-151">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="eb1a4-152">Wykonaj następujące polecenie w powłoce poleceń, aby sprawdzić poprawność instalacji narzędzia:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-152">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="eb1a4-153">Narzędzie Menedżer klucz tajny Wyświetla przykładowe zastosowanie i opcje pomocy dla poleceń:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-153">The Secret Manager tool displays sample usage, options, and command help:</span></span>

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> <span data-ttu-id="eb1a4-154">Musi być w tym samym katalogu co *.csproj* uruchamianie narzędzi zdefiniowane w pliku *.csproj* pliku `DotNetCliToolReference` elementów.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-154">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="eb1a4-155">Ustaw klucz tajny</span><span class="sxs-lookup"><span data-stu-id="eb1a4-155">Set a secret</span></span>

<span data-ttu-id="eb1a4-156">Narzędzie Menedżer klucz tajny działa na ustawień konfiguracji specyficznych dla projektu, przechowywane w profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-156">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="eb1a4-157">Aby korzystać z wpisami tajnymi użytkowników, należy zdefiniować `UserSecretsId` elemencie `PropertyGroup` z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-157">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="eb1a4-158">Wartość `UserSecretsId` jest określana, ale jest unikatowy dla projektu.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-158">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="eb1a4-159">Deweloperzy zazwyczaj Wygeneruj identyfikator GUID dla `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-159">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="eb1a4-160">W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **zarządzania wpisami tajnymi użytkowników** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-160">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="eb1a4-161">Dodaje ten gest `UserSecretsId` elementu wypełniane przy użyciu identyfikatora GUID do *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-161">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="eb1a4-162">Zostanie otwarty program Visual Studio *secrets.json* plik w edytorze tekstów.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-162">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="eb1a4-163">Zastąp zawartość *secrets.json* za pomocą pary klucz wartość, która ma być przechowywany.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-163">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="eb1a4-164">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-164">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="eb1a4-165">Struktura JSON jest spłaszczany po modyfikacji za pośrednictwem `dotnet user-secrets remove` lub `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-165">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="eb1a4-166">Aby na przykład uruchomić `dotnet user-secrets remove "Movies:ConnectionString"` zwija `Movies` literału obiektu.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-166">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="eb1a4-167">Zmodyfikowany plik jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-167">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="eb1a4-168">Zdefiniuj klucz tajny aplikacji składających się z klucza i wartości.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="eb1a4-169">Klucz tajny jest skojarzony z projektem `UserSecretsId` wartość.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="eb1a4-170">Na przykład, uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="eb1a4-171">W poprzednim przykładzie, dwukropek, oznacza, że `Movies` jest obiektem literału z `ServiceApiKey` właściwości.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="eb1a4-172">Narzędzie Menedżer klucz tajny może służyć za z innych katalogów.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="eb1a4-173">Użyj `--project` opcję, aby podać ścieżka systemu plików, w którym *.csproj* plik istnieje.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="eb1a4-174">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="eb1a4-175">Ustawianie wielu wpisów tajnych</span><span class="sxs-lookup"><span data-stu-id="eb1a4-175">Set multiple secrets</span></span>

<span data-ttu-id="eb1a4-176">Seria wpisów tajnych można ustawić przez przekazanie w potoku JSON do `set` polecenia.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="eb1a4-177">W poniższym przykładzie *input.json* zawartość tego pliku są potokiem do `set` polecenia.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="eb1a4-178">Windows</span><span class="sxs-lookup"><span data-stu-id="eb1a4-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="eb1a4-179">Otwórz powłokę wiersza polecenia, a następnie wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="eb1a4-180">macOS</span><span class="sxs-lookup"><span data-stu-id="eb1a4-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="eb1a4-181">Otwórz powłokę wiersza polecenia, a następnie wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="eb1a4-182">Linux</span><span class="sxs-lookup"><span data-stu-id="eb1a4-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="eb1a4-183">Otwórz powłokę wiersza polecenia, a następnie wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="eb1a4-184">Dostęp do klucza tajnego</span><span class="sxs-lookup"><span data-stu-id="eb1a4-184">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="eb1a4-185">[Interfejsu API platformy ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do kluczy tajnych Menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="eb1a4-186">Jeśli projekt jest przeznaczony dla .NET Framework, należy zainstalować [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-186">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="eb1a4-187">W programie ASP.NET Core 2.0 lub nowszej, źródło konfiguracji kluczy tajnych użytkownika zostanie automatycznie dodana w trybie projektowania, przy projektu wywołuje [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zainicjować nowe wystąpienie hosta przy użyciu wstępnie skonfigurowanych ustawień domyślnych.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-187">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="eb1a4-188">`CreateDefaultBuilder` wywołania [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) podczas [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) jest [rozwoju](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="eb1a4-188">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="eb1a4-189">Gdy `CreateDefaultBuilder` nie jest wywoływana podczas konstruowania hosta, Dodaj źródło konfiguracji wpisami tajnymi użytkowników przy użyciu wywołania do [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) w `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-189">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="eb1a4-190">[Interfejsu API platformy ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do kluczy tajnych Menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-190">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="eb1a4-191">Zainstaluj [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-191">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="eb1a4-192">Dodawanie źródła konfiguracji kluczy tajnych użytkownika z wywołaniem [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) w `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-192">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="eb1a4-193">Wpisami tajnymi użytkowników mogą być pobierane za pośrednictwem `Configuration` interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-193">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="eb1a4-194">Klucze tajne mapy POCO</span><span class="sxs-lookup"><span data-stu-id="eb1a4-194">Map secrets to a POCO</span></span>

<span data-ttu-id="eb1a4-195">Mapowanie całego literału obiektu do POCO (.NET klasą prostą z właściwościami) jest przydatne w przypadku agregowania powiązane właściwości.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-195">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eb1a4-196">Aby mapować POCO poprzednich wpisów tajnych, użyj `Configuration` interfejsów API [obiektu powiązania wykres](xref:fundamentals/configuration/index#bind-to-an-object-graph) funkcji.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-196">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="eb1a4-197">Poniższy kod tworzy powiązanie z niestandardowego `MovieSettings` POCO i uzyskuje dostęp do `ServiceApiKey` wartość właściwości:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-197">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="eb1a4-198">`Movies:ConnectionString` i `Movies:ServiceApiKey` wpisów tajnych są mapowane do odpowiednich właściwości w `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-198">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="eb1a4-199">Zastąpienie ciągu przy użyciu kluczy tajnych</span><span class="sxs-lookup"><span data-stu-id="eb1a4-199">String replacement with secrets</span></span>

<span data-ttu-id="eb1a4-200">Przechowywanie haseł w postaci zwykłego tekstu jest niezabezpieczona.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-200">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="eb1a4-201">Na przykład parametry połączenia bazy danych są przechowywane w *appsettings.json* może zawierać hasła dla określonego użytkownika:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="eb1a4-202">Bardziej bezpieczną metodą jest w celu zapisania hasła jako klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="eb1a4-203">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="eb1a4-204">Usuń `Password` pary klucz wartość z parametrów połączenia w *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-204">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="eb1a4-205">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-205">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="eb1a4-206">Klucz tajny, wartość można ustawić na [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) obiektu [hasło](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) właściwości, aby ukończyć parametry połączenia:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-206">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="eb1a4-207">Wyświetl klucze tajne</span><span class="sxs-lookup"><span data-stu-id="eb1a4-207">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eb1a4-208">Uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-208">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="eb1a4-209">Zostanie wyświetlone następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-209">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="eb1a4-210">W poprzednim przykładzie, dwukropek w nazwach kluczy oznacza hierarchię obiektów w ramach *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="eb1a4-210">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="eb1a4-211">Usunąć pojedynczy wpis tajny</span><span class="sxs-lookup"><span data-stu-id="eb1a4-211">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eb1a4-212">Uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-212">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="eb1a4-213">Aplikacja *secrets.json* plik został zmodyfikowany, aby usunąć pary klucz wartość skojarzonych z `MoviesConnectionString` klucza:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-213">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="eb1a4-214">Uruchamianie `dotnet user-secrets list` wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-214">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="eb1a4-215">Usuwanie wszystkich wpisów tajnych</span><span class="sxs-lookup"><span data-stu-id="eb1a4-215">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eb1a4-216">Uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-216">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="eb1a4-217">Wszystkie wpisami tajnymi użytkowników dla aplikacji został usunięty z *secrets.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-217">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="eb1a4-218">Uruchamianie `dotnet user-secrets list` wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="eb1a4-218">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="eb1a4-219">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="eb1a4-219">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
