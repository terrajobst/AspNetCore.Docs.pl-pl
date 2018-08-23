---
title: Bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania w programie ASP.NET Core
author: rick-anderson
description: Informacje o sposobie przechowywania i pobierania informacji poufnych jako wpisy tajne aplikacji podczas tworzenia aplikacji ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/16/2018
uid: security/app-secrets
ms.openlocfilehash: 35c316230c19aa69a0dac26ec25a6e017f102237
ms.sourcegitcommit: 1cf65c25ed16495e27f35ded98b3952a30c68f36
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/17/2018
ms.locfileid: "41754095"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="ce647-103">Bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce647-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="ce647-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), i [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="ce647-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="ce647-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce647-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ce647-106">W tym dokumencie opisano techniki do przechowywania i pobierania danych poufnych podczas tworzenia aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce647-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="ce647-107">Nigdy nie przechowuj haseł i innych poufnych danych w kodzie źródłowym, a nie powinien używania wpisów tajnych produkcji w trakcie opracowywania lub testowania trybu.</span><span class="sxs-lookup"><span data-stu-id="ce647-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="ce647-108">Możesz przechowywać i chronić Azure testowych i produkcyjnych wpisów tajnych za pomocą [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="ce647-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="ce647-109">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="ce647-109">Environment variables</span></span>

<span data-ttu-id="ce647-110">Zmienne środowiskowe są używane w celu uniknięcia przechowywanie kluczy tajnych aplikacji w kodzie lub w plikach konfiguracji lokalnej.</span><span class="sxs-lookup"><span data-stu-id="ce647-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="ce647-111">Zmienne środowiskowe zastępować wartości konfiguracji dla wszystkich źródeł wcześniej określonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="ce647-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="ce647-112">Konfigurowanie odczytywania wartości zmiennych środowiskowych, wywołując [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) w `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="ce647-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="ce647-113">Należy wziąć pod uwagę aplikację internetową platformy ASP.NET Core, w którym **indywidualne konta użytkowników** zabezpieczenia są włączone.</span><span class="sxs-lookup"><span data-stu-id="ce647-113">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="ce647-114">Domyślne parametry połączenia bazy danych znajduje się w projekcie *appsettings.json* pliku przy użyciu klucza `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="ce647-114">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="ce647-115">Domyślny ciąg połączenia jest do LocalDB, który działa w trybie użytkownika i nie wymaga hasła.</span><span class="sxs-lookup"><span data-stu-id="ce647-115">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="ce647-116">Podczas wdrażania aplikacji `DefaultConnection` wartość klucza może zostać zastąpiona przez wartość zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="ce647-116">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="ce647-117">Zmienna środowiskowa mogą być przechowywane pełne parametry połączenia przy użyciu poświadczeń poufnych.</span><span class="sxs-lookup"><span data-stu-id="ce647-117">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="ce647-118">Zmienne środowiskowe są zazwyczaj przechowywane w zwykły tekst niezaszyfrowane.</span><span class="sxs-lookup"><span data-stu-id="ce647-118">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="ce647-119">W przypadku naruszenia zabezpieczeń komputera lub proces, dostęp do zmiennych środowiskowych jest możliwy przez niezaufane.</span><span class="sxs-lookup"><span data-stu-id="ce647-119">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="ce647-120">Dodatkowe środki, aby zapobiec ujawnieniu wpisami tajnymi użytkowników mogą być wymagane.</span><span class="sxs-lookup"><span data-stu-id="ce647-120">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="ce647-121">Menedżer wpisu tajnego</span><span class="sxs-lookup"><span data-stu-id="ce647-121">Secret Manager</span></span>

<span data-ttu-id="ce647-122">Narzędzie Menedżer klucz tajny są przechowywane poufne dane podczas tworzenia projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce647-122">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="ce647-123">W tym kontekście część danych poufnych jest wpis tajny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce647-123">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="ce647-124">Wpisy tajne aplikacji są przechowywane w innej lokalizacji w drzewie projektu.</span><span class="sxs-lookup"><span data-stu-id="ce647-124">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="ce647-125">Wpisy tajne aplikacji są skojarzone z określonym projektem lub udostępnione w kilku projektach.</span><span class="sxs-lookup"><span data-stu-id="ce647-125">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="ce647-126">Wpisy tajne aplikacji nie są sprawdzane w systemie kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="ce647-126">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="ce647-127">Narzędzie Menedżer wpis tajny nie szyfruje przechowywane klucze tajne i nie powinien być traktowany jako zaufany magazyn.</span><span class="sxs-lookup"><span data-stu-id="ce647-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="ce647-128">Jest tylko do celów programowania.</span><span class="sxs-lookup"><span data-stu-id="ce647-128">It's for development purposes only.</span></span> <span data-ttu-id="ce647-129">Klucze i wartości są przechowywane w pliku konfiguracji JSON w katalogu profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ce647-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="ce647-130">Jak działa narzędzie Menedżer wpisu tajnego</span><span class="sxs-lookup"><span data-stu-id="ce647-130">How the Secret Manager tool works</span></span>

<span data-ttu-id="ce647-131">Narzędzie Menedżer klucz tajny ukrywa szczegóły implementacji, takich jak gdzie i w jaki sposób wartości są przechowywane.</span><span class="sxs-lookup"><span data-stu-id="ce647-131">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="ce647-132">Narzędzie nie znając te szczegóły implementacji.</span><span class="sxs-lookup"><span data-stu-id="ce647-132">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="ce647-133">Wartości są przechowywane w pliku konfiguracji JSON w folderze profilu użytkownika chronionych przez system na komputerze lokalnym:</span><span class="sxs-lookup"><span data-stu-id="ce647-133">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ce647-134">Windows</span><span class="sxs-lookup"><span data-stu-id="ce647-134">Windows</span></span>](#tab/windows)

<span data-ttu-id="ce647-135">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="ce647-135">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="ce647-136">macOS</span><span class="sxs-lookup"><span data-stu-id="ce647-136">macOS</span></span>](#tab/macos)

<span data-ttu-id="ce647-137">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="ce647-137">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="ce647-138">Linux</span><span class="sxs-lookup"><span data-stu-id="ce647-138">Linux</span></span>](#tab/linux)

<span data-ttu-id="ce647-139">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="ce647-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="ce647-140">W poprzednim ścieżki plików, Zastąp `<user_secrets_id>` z `UserSecretsId` wartości określonej w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="ce647-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="ce647-141">Nie pisz kod, który zależy od lokalizacji lub formatu danych zapisanych przy użyciu narzędzia menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="ce647-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="ce647-142">Szczegóły tych implementacji mogą ulec zmianie.</span><span class="sxs-lookup"><span data-stu-id="ce647-142">These implementation details may change.</span></span> <span data-ttu-id="ce647-143">Na przykład klucza tajnego wartości nie są szyfrowane, ale może być w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="ce647-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="ce647-144">Zainstaluj narzędzie Menedżer wpisu tajnego</span><span class="sxs-lookup"><span data-stu-id="ce647-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="ce647-145">Narzędzie Menedżer klucz tajny jest powiązane z platformy .NET Core interfejsu wiersza polecenia w zestawie SDK programu .NET Core 2.1.300 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="ce647-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="ce647-146">Wersje programu .NET Core SDK przed 2.1.300 niezbędne jest narzędzie instalacji.</span><span class="sxs-lookup"><span data-stu-id="ce647-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="ce647-147">Uruchom `dotnet --version` z powłoki poleceń, aby wyświetlić numer wersji zainstalowanego zestawu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="ce647-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="ce647-148">Zostanie wyświetlone ostrzeżenie, jeśli używany zestaw .NET Core SDK zawiera narzędzia:</span><span class="sxs-lookup"><span data-stu-id="ce647-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="ce647-149">Zainstaluj [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pakietu NuGet w projekcie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce647-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="ce647-150">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ce647-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="ce647-151">Wykonaj następujące polecenie w powłoce poleceń, aby sprawdzić poprawność instalacji narzędzia:</span><span class="sxs-lookup"><span data-stu-id="ce647-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="ce647-152">Narzędzie Menedżer klucz tajny Wyświetla przykładowe zastosowanie i opcje pomocy dla poleceń:</span><span class="sxs-lookup"><span data-stu-id="ce647-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="ce647-153">Musi być w tym samym katalogu co *.csproj* uruchamianie narzędzi zdefiniowane w pliku *.csproj* pliku `DotNetCliToolReference` elementów.</span><span class="sxs-lookup"><span data-stu-id="ce647-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="ce647-154">Ustaw klucz tajny</span><span class="sxs-lookup"><span data-stu-id="ce647-154">Set a secret</span></span>

<span data-ttu-id="ce647-155">Narzędzie Menedżer klucz tajny działa na ustawień konfiguracji specyficznych dla projektu, przechowywane w profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ce647-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="ce647-156">Aby korzystać z wpisami tajnymi użytkowników, należy zdefiniować `UserSecretsId` elemencie `PropertyGroup` z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="ce647-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="ce647-157">Wartość `UserSecretsId` jest określana, ale jest unikatowy dla projektu.</span><span class="sxs-lookup"><span data-stu-id="ce647-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="ce647-158">Deweloperzy zazwyczaj Wygeneruj identyfikator GUID dla `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="ce647-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="ce647-159">W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **zarządzania wpisami tajnymi użytkowników** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="ce647-159">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="ce647-160">Dodaje ten gest `UserSecretsId` elementu wypełniane przy użyciu identyfikatora GUID do *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="ce647-160">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="ce647-161">Zostanie otwarty program Visual Studio *secrets.json* plik w edytorze tekstów.</span><span class="sxs-lookup"><span data-stu-id="ce647-161">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="ce647-162">Zastąp zawartość *secrets.json* za pomocą pary klucz wartość, która ma być przechowywany.</span><span class="sxs-lookup"><span data-stu-id="ce647-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="ce647-163">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ce647-163">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="ce647-164">Struktura JSON jest spłaszczany po modyfikacji za pośrednictwem `dotnet user-secrets remove` lub `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="ce647-164">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="ce647-165">Aby na przykład uruchomić `dotnet user-secrets remove "Movies:ConnectionString"` zwija `Movies` literału obiektu.</span><span class="sxs-lookup"><span data-stu-id="ce647-165">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="ce647-166">Zmodyfikowany plik jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="ce647-166">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="ce647-167">Zdefiniuj klucz tajny aplikacji składających się z klucza i wartości.</span><span class="sxs-lookup"><span data-stu-id="ce647-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="ce647-168">Klucz tajny jest skojarzony z projektem `UserSecretsId` wartość.</span><span class="sxs-lookup"><span data-stu-id="ce647-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="ce647-169">Na przykład, uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:</span><span class="sxs-lookup"><span data-stu-id="ce647-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="ce647-170">W poprzednim przykładzie, dwukropek, oznacza, że `Movies` jest obiektem literału z `ServiceApiKey` właściwości.</span><span class="sxs-lookup"><span data-stu-id="ce647-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="ce647-171">Narzędzie Menedżer klucz tajny może służyć za z innych katalogów.</span><span class="sxs-lookup"><span data-stu-id="ce647-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="ce647-172">Użyj `--project` opcję, aby podać ścieżka systemu plików, w którym *.csproj* plik istnieje.</span><span class="sxs-lookup"><span data-stu-id="ce647-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="ce647-173">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ce647-173">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="ce647-174">Ustawianie wielu wpisów tajnych</span><span class="sxs-lookup"><span data-stu-id="ce647-174">Set multiple secrets</span></span>

<span data-ttu-id="ce647-175">Seria wpisów tajnych można ustawić przez przekazanie w potoku JSON do `set` polecenia.</span><span class="sxs-lookup"><span data-stu-id="ce647-175">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="ce647-176">W poniższym przykładzie *input.json* zawartość tego pliku są potokiem do `set` polecenia.</span><span class="sxs-lookup"><span data-stu-id="ce647-176">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ce647-177">Windows</span><span class="sxs-lookup"><span data-stu-id="ce647-177">Windows</span></span>](#tab/windows)

<span data-ttu-id="ce647-178">Otwórz powłokę wiersza polecenia, a następnie wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ce647-178">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="ce647-179">macOS</span><span class="sxs-lookup"><span data-stu-id="ce647-179">macOS</span></span>](#tab/macos)

<span data-ttu-id="ce647-180">Otwórz powłokę wiersza polecenia, a następnie wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ce647-180">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="ce647-181">Linux</span><span class="sxs-lookup"><span data-stu-id="ce647-181">Linux</span></span>](#tab/linux)

<span data-ttu-id="ce647-182">Otwórz powłokę wiersza polecenia, a następnie wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ce647-182">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="ce647-183">Dostęp do klucza tajnego</span><span class="sxs-lookup"><span data-stu-id="ce647-183">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ce647-184">[Interfejsu API platformy ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do kluczy tajnych Menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="ce647-184">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="ce647-185">Jeśli projekt jest przeznaczony dla .NET Framework, należy zainstalować [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="ce647-185">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="ce647-186">W programie ASP.NET Core 2.0 lub nowszej, źródło konfiguracji kluczy tajnych użytkownika zostanie automatycznie dodana w trybie projektowania, przy projektu wywołuje [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zainicjować nowe wystąpienie hosta przy użyciu wstępnie skonfigurowanych ustawień domyślnych.</span><span class="sxs-lookup"><span data-stu-id="ce647-186">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="ce647-187">`CreateDefaultBuilder` wywołania [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) podczas [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) jest [rozwoju](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="ce647-187">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="ce647-188">Gdy `CreateDefaultBuilder` nie jest wywoływana podczas konstruowania hosta, Dodaj źródło konfiguracji wpisami tajnymi użytkowników przy użyciu wywołania do [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) w `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="ce647-188">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="ce647-189">[Interfejsu API platformy ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do kluczy tajnych Menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="ce647-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="ce647-190">Zainstaluj [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="ce647-190">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="ce647-191">Dodawanie źródła konfiguracji kluczy tajnych użytkownika z wywołaniem [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) w `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="ce647-191">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="ce647-192">Wpisami tajnymi użytkowników mogą być pobierane za pośrednictwem `Configuration` interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="ce647-192">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="ce647-193">Klucze tajne mapy POCO</span><span class="sxs-lookup"><span data-stu-id="ce647-193">Map secrets to a POCO</span></span>

<span data-ttu-id="ce647-194">Mapowanie całego literału obiektu do POCO (.NET klasą prostą z właściwościami) jest przydatne w przypadku agregowania powiązane właściwości.</span><span class="sxs-lookup"><span data-stu-id="ce647-194">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="ce647-195">Aby mapować POCO poprzednich wpisów tajnych, użyj `Configuration` interfejsów API [obiektu powiązania wykres](xref:fundamentals/configuration/index#bind-to-an-object-graph) funkcji.</span><span class="sxs-lookup"><span data-stu-id="ce647-195">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="ce647-196">Poniższy kod tworzy powiązanie z niestandardowego `MovieSettings` POCO i uzyskuje dostęp do `ServiceApiKey` wartość właściwości:</span><span class="sxs-lookup"><span data-stu-id="ce647-196">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="ce647-197">`Movies:ConnectionString` i `Movies:ServiceApiKey` wpisów tajnych są mapowane do odpowiednich właściwości w `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="ce647-197">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="ce647-198">Zastąpienie ciągu przy użyciu kluczy tajnych</span><span class="sxs-lookup"><span data-stu-id="ce647-198">String replacement with secrets</span></span>

<span data-ttu-id="ce647-199">Przechowywanie haseł w postaci zwykłego tekstu jest niezabezpieczona.</span><span class="sxs-lookup"><span data-stu-id="ce647-199">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="ce647-200">Na przykład parametry połączenia bazy danych są przechowywane w *appsettings.json* może zawierać hasła dla określonego użytkownika:</span><span class="sxs-lookup"><span data-stu-id="ce647-200">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="ce647-201">Bardziej bezpieczną metodą jest w celu zapisania hasła jako klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="ce647-201">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="ce647-202">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ce647-202">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="ce647-203">Usuń `Password` pary klucz wartość z parametrów połączenia w *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ce647-203">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="ce647-204">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ce647-204">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="ce647-205">Klucz tajny, wartość można ustawić na [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) obiektu [hasło](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) właściwości, aby ukończyć parametry połączenia:</span><span class="sxs-lookup"><span data-stu-id="ce647-205">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="ce647-206">Wyświetl klucze tajne</span><span class="sxs-lookup"><span data-stu-id="ce647-206">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="ce647-207">Uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:</span><span class="sxs-lookup"><span data-stu-id="ce647-207">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="ce647-208">Zostanie wyświetlone następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="ce647-208">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="ce647-209">W poprzednim przykładzie, dwukropek w nazwach kluczy oznacza hierarchię obiektów w ramach *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="ce647-209">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="ce647-210">Usunąć pojedynczy wpis tajny</span><span class="sxs-lookup"><span data-stu-id="ce647-210">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="ce647-211">Uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:</span><span class="sxs-lookup"><span data-stu-id="ce647-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="ce647-212">Aplikacja *secrets.json* plik został zmodyfikowany, aby usunąć pary klucz wartość skojarzonych z `MoviesConnectionString` klucza:</span><span class="sxs-lookup"><span data-stu-id="ce647-212">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="ce647-213">Uruchamianie `dotnet user-secrets list` wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="ce647-213">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="ce647-214">Usuwanie wszystkich wpisów tajnych</span><span class="sxs-lookup"><span data-stu-id="ce647-214">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="ce647-215">Uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:</span><span class="sxs-lookup"><span data-stu-id="ce647-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="ce647-216">Wszystkie wpisami tajnymi użytkowników dla aplikacji został usunięty z *secrets.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="ce647-216">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="ce647-217">Uruchamianie `dotnet user-secrets list` wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="ce647-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="ce647-218">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ce647-218">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
